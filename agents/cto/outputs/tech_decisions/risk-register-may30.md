# Presently — CTO Risk Register (May 30th Demo)
> Author: CTO Agent
> Last updated: May 2026
> Purpose: The five threats most likely to kill the May 30th demo, with mitigations.

---

## Risk 1 — Render Cold Start Failure [CRITICAL]
**Likelihood:** High (has occurred in both prior tests)
**Impact:** Demo cannot begin — no logins, no check-ins
**Mitigation:**
- Wake-up call on app load is in place — verify it fires reliably
- Extend preloader screen to 6 seconds minimum
- On demo day: manually open the app 10 minutes before demo begins to warm the server
- Escalate to CEO if wake-up call fails during May 27th end-to-end test — tier upgrade decision required

---

## Risk 2 — Poor In-Venue Network [CRITICAL]
**Likelihood:** High (confirmed in both prior tests)
**Impact:** Students cannot reach the server, GPS cannot resolve, check-in fails
**Mitigation:**
- All API calls must have retry logic before May 27th
- Check-in flow should be as lightweight as possible — minimize payload size
- CEO to bring a 4G hotspot as a controlled network baseline for the lecturer's device
- Consider: can check-in be cached locally and synced when connection restores? (post-May 30th feature — log for future sprint)

---

## Risk 3 — GPS Inaccuracy on Low-End Android Devices [HIGH]
**Likelihood:** Medium-High
**Impact:** Legitimate students fail geofence check, chaos at scale
**Mitigation:**
- Tune geofence radius to 50m from theatre centre point
- Add ±20m tolerance buffer for GPS drift
- Manual fallback must be demonstrated working before demo day
- Do not advertise GPS precision as a feature during this demo — frame it as "verified presence"

---

## Risk 4 — Broken UI Destroys First Impression [HIGH]
**Likelihood:** High (UI is currently significantly broken)
**Impact:** DSA and students lose confidence immediately — credibility gone
**Mitigation:**
- UI fixes are Priority 1 — must be complete by May 24th
- Full mobile audit on 360px–390px width range
- CTO personally reviews UI on a real low-end Android device before May 27th sign-off
- If UI is not clean by May 26th — CEO must be alerted immediately (not Saturday — immediately)

---

## Risk 5 — Last-Minute Production Push Crashes Deploy [CRITICAL]
**Likelihood:** High (caused Test Run 1 failure)
**Impact:** Complete demo failure before it begins
**Mitigation:**
- 48-hour deployment freeze policy is now a hard rule
- Final production deploy: May 28th EOD
- After May 28th: no code changes, no config changes, no dependency updates on production
- All last-minute fixes go to a staging branch and are held until after demo
