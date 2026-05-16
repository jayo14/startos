# Decisions Log

| Date | Decision | Rationale | Made by |
| :--- | :--- | :--- | :--- |
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
