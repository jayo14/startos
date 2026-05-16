# Presently — CTO Week 1 Technical Assessment
> Author: CTO Agent
> Date: May 16, 2026
> Status: CRITICAL - Hardening required for May 30th Demo

## 1. Codebase Health Summary
The Presently codebase is in a "transition state" that is dangerously fragile for a 500-student live demo. While the core Django REST Framework (DRF) backend is architecturally sound and follows standard patterns, the frontend is a fragmented mess of "Modern" UI components and legacy structures. We are currently maintaining two parallel layout systems (`ResponsiveContainer.tsx` vs raw Tailwind), leading to the visual chaos reported. The GPS logic is "lab-perfect" but "field-broken"—it assumes high-precision hardware and stable networks that do not exist in the Nigerian university environment. The codebase is currently held together by mock data fallbacks in the frontend services (`api/system.ts`) which mask backend integration gaps.

## 2. UI — Root Cause Analysis

*   **Map not rendering in admin dashboard**
    *   **File:** `products/presently/frontend/components/Admin/VenueMap.tsx`
    *   **Cause:** The component relies on `basemaps.cartocdn.com` for road tiles and a missing `VITE_MAPBOX_TOKEN` for satellite views. Furthermore, the `map.resize()` call is not consistently triggered after the `VenueModal.tsx` animation finishes, resulting in a 0x0 canvas error.
*   **Layout inconsistencies on mobile and desktop**
    *   **File:** `products/presently/frontend/components/layouts/ResponsiveContainer.tsx` & `DashboardSidebar.tsx`
    *   **Cause:** The `ResponsiveContainer` is not wrapped around the main `Outlet` in `App.tsx`. Desktop views are forced into mobile-first containers, causing 390px-optimized cards to stretch awkwardly on 1440px screens.
*   **Theme and contrast issues**
    *   **File:** `products/presently/frontend/src/theme.ts`
    *   **Cause:** `onSurfaceVariant` is hardcoded to `#64748B`. On a `#F9F7F3` (warm ivory) background, this yields a contrast ratio of ~3.5:1, failing WCAG AA (4.5:1) for small text.
*   **General layout mess in student and lecturer flows**
    *   **File:** `products/presently/frontend/components/Dashboard/LecturerDashboard.tsx` & `StudentDashboard.tsx`
    *   **Cause:** Hardcoded padding in the "Modern" shell (`space-y-10`) conflicts with the global layout padding in `App.tsx`, resulting in double-padding on mobile and zero-padding on desktop.

## 3. GPS & Geofence — Root Cause Analysis

*   **Current Geofence Radius:** Defaulted to **100m** in the `AttendanceSession` model, but the scoring logic in `attendance/views.py` uses a hard fallback of **50m** if the lecturer's specific coordinates are missing. This inconsistency will lead to rejected check-ins.
*   **Tolerance Buffer:** **Non-existent.** `backend/institutions/geolocation_utils.py` uses strict `distance <= radius` logic. There is no `±20m` buffer for GPS drift, meaning a student 51m away from a 50m zone will be rejected.
*   **Idempotency:** **Enforced.** `attendance/views.py` correctly uses `select_for_update()` and checks for existing `AttendanceRecord` entries to prevent double check-ins.
*   **Manual Fallback:** **Partially implemented.** The UI exists in `SessionDetailDialog.tsx`, but it calls `dashboardApi.manualMark` which is currently unmapped in `api/attendance.ts`.
*   **Confidence Level:** **45%**. Without a drift buffer and with the current 50m/100m conflict, we will see a 30%+ "False Rejected" rate at scale.

## 4. Backend Stability — Root Cause Analysis

*   **Render Wake-up:** **Incorrectly implemented.** `App.tsx` uses a primitive `fetch()` call in a `useEffect`. It does not use the robust `useWarmUp.ts` hook which handles visibility changes and exponential backoff.
*   **Timeout Risks:** High. `api/client.ts` has a 45s timeout but **zero retry logic**. Under 3G congestion, a single packet drop will kill the check-in without an automatic retry.
*   **Supabase RLS:** Policies in `schema.sql` are standard but the Django backend bypasses them by using a service-level connection. However, frontend direct calls (if any remain) will fail because the `profiles` table policy requires `auth.uid()` which doesn't match the Django JWT.

## 5. Sprint Task Assignments

### Frontend Agent Tasks
*   **Fix `products/presently/frontend/src/theme.ts`:** Update `onSurfaceVariant` to `#475569` to meet WCAG AA.
*   **Fix `products/presently/frontend/components/Admin/VenueMap.tsx`:** Wrap `map.resize()` in a 300ms timeout after modal mount to fix rendering.
*   **Refactor `products/presently/frontend/App.tsx`:** Replace the custom `wakeServer` function with the `useWarmUp` hook.
*   **Update `products/presently/frontend/services/api/client.ts`:** Implement `p-retry` on all POST requests to `/check-in/`.

### Backend Agent Tasks
*   **Fix `products/presently/backend/attendance/views.py`:** Align the default radius in `_server_recompute_score` with the model's 100m default.
*   **Update `products/presently/backend/institutions/geolocation_utils.py`:** Add a `GEOFENCE_DRIFT_BUFFER = 20` constant to the `verify_location` function.
*   **Map Endpoint:** Ensure `manualMark` is correctly routed in `attendance/urls.py`.

### DevOps Agent Tasks
*   **Render Config:** Increase `WEB_CONCURRENCY` to 4 to handle the 500-student burst.
*   **Environment:** Inject `VITE_MAPBOX_TOKEN` into the Vercel production environment.

## 6. Architecture Decisions Required This Week

1.  **Strict vs. Fuzzy Geofencing:** Do we stick to the 100m circle or allow the 20m buffer?
    *   *Recommendation:* **Allow 20m buffer.** Nigerian theatre construction (thick concrete) kills signal accuracy.
    *   *Risk of Delay:* 200+ students complaining of "I'm in the hall but it says I'm out."
2.  **Render Tier Upgrade:**
    *   *Recommendation:* **Upgrade to Starter Tier ($7/mo)** before May 27th.
    *   *Risk of Delay:* Cold starts on the free tier can take 50s+, exceeding our 45s frontend timeout.

## 7. CTO Confidence Rating
**60%**.
The core logic is there, but the "last mile" connectivity and UI polish are missing. If we don't fix the GPS tolerance and add frontend retries, the demo will be a chaotic failure of "network error" popups.

## 8. Immediate Escalations to CEO
*   **Blocker:** We need a Mapbox API Key immediately to fix the Admin Dashboard map.
*   **Decision:** Confirm the 70m total radius (50m base + 20m buffer) for the LASUSTECH theatre.
*   **Infrastructure:** Approve $7/mo for Render Starter tier to kill the cold-start risk.
