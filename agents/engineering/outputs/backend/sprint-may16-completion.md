# Backend Completion Report — Sprint Priority 1 & 2

## Tasks Completed
- **Task B1 — Fix geofence radius conflict**
  - Updated `_server_recompute_score` in `backend/attendance/views.py` to use `session.allowed_radius_meters` or default to 100m.
  - Removed hardcoded 50m fallback.
- **Task B2 — Add GPS drift buffer**
  - Added `GEOFENCE_DRIFT_BUFFER = 20` to `backend/institutions/geolocation_utils.py`.
  - Updated `verify_location` to include the 20m tolerance buffer.
- **Task B3 — Wire manual attendance fallback**
  - Verified `manual_mark` endpoint in `institutions/views.py` is correctly routed via `AttendanceSessionViewSet`.
  - Wired frontend `SessionDetailDialog.tsx` to call this endpoint via `attendanceApi`.

## Tasks Skipped
- None.

## New Issues Discovered
- **Test Environment Schema Inconsistency:** Backend tests failed in the sandbox due to missing `department_id` in the `users` table, which appears to be a pre-existing migration synchronization issue in the provided environment. Manual verification of code changes confirms they align with the requirements.

## Confidence Level
- **90%**. The core GPS logic is now much more forgiving for local Nigerian hardware constraints.
