# Decisions Log

| Date | Decision | Rationale | Made by |
| :--- | :--- | :--- | :--- |
| May 2026 | Disabled all email sending until funded | Resend free tier exhausted during Test Run 2 (~100 emails in minutes). DNS/SSL multi-provider setup too error-prone without dedicated DevOps time. Conscious tradeoff: no email = no OTP, no welcome emails, no session notifications. Acceptable for informal test. | CEO |
| May 2026 | Signup flow proceeds without email verification | Direct consequence of email disable decision. Students can sign up and log in immediately. Reduces friction for demo onboarding. Security tradeoff accepted for pre-pilot phase. | CEO |
| May 2026 | Render cold start partial mitigation deployed | Server wake-up call triggered on web app load before any API requests. Preloader screen added. Full fix requires paid Render tier — deferred until funding. | CTO |
| May 2026 | 48-hour pre-demo deployment freeze policy | Last-minute push caused Render crash night before Test Run 1. Policy: no production pushes within 48 hours of a scheduled demo. Enforced by CTO agent. | CTO |
| May 2026 | May 27th feature freeze for May 30th demo | All new features must be complete by May 27th EOD. May 28th–30th reserved for testing, hardening, and bug fixes only. | PM |
| Foundational | **Supabase for Backend/Auth** | Integrated PostgreSQL, Auth, and Realtime features reduce maintenance overhead and speed up development. | CEO |
| Foundational | **Django REST Framework (DRF)** | Robust, mature ecosystem for API development with strong security defaults. Preferred over FastAPI for stability and battery-included approach. | CEO |
| Foundational | **Vite + React 19** | Modern frontend stack with high performance and support for latest React features (Server Components, improved hooks). | CEO |
| Foundational | **Render for Backend Deployment** | Simplified deployment for DRF with easy scaling and auto-deployments. | CEO |
| Foundational | **Vercel for Frontend Deployment** | Best-in-class performance for React/Vite applications and seamless DX. | CEO |
| Foundational | **Resend as Primary Email Provider** | Highest deliverability for .com.ng domains. Multi-provider failover implemented for critical infrastructure. | CEO |
| Foundational | **Paystack for Payments** | Default payment gateway for the Nigerian market; handles local cards and bank transfers natively. | CEO |
| Foundational | **GPS Verification as Core Mechanism** | Addresses the primary pain point of "proxy attendance" in Nigerian universities. | CEO |
| Foundational | **B2B SaaS Model** | Institutional-level subscriptions (per-semester) provide more stable revenue than per-student billing. | CEO |
| Foundational | **NUC Compliance Strategy** | Mandating 75% attendance for exam eligibility creates a regulatory "must-have" for university administrations. | CEO |
| Foundational | **NDPR Awareness** | Ensuring data privacy compliance from day one to mitigate regulatory risk in Nigeria. | CEO |
