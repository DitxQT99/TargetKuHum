# Research Notes — 24 September 2026

Web research informed this architecture.

1. The Push API delivers pushes to an active Service Worker, where a `push` event can be handled.
2. Persistent mobile notifications should be created from the Service Worker with `ServiceWorkerRegistration.showNotification()`.
3. VAPID is used by the application server to authenticate Web Push requests; the public key is used for subscription and the private key stays server-side.
4. Vercel Cron triggers Vercel Functions with HTTP GET. Cron timezone is UTC.
5. Vercel Hobby cron is limited to once per day; Pro/Enterprise have per-minute precision.
6. Neon supplies a serverless PostgreSQL driver for server-side JavaScript and exposes the connection through `DATABASE_URL`.
7. `web-push` provides VAPID configuration and `sendNotification()`.

Implementation:
Vercel Cron -> Serverless Function -> Neon due-reminder rows -> web-push/VAPID -> browser push service -> Service Worker -> Android notification.


8. TARGETKU AI integration (October 2026)
- Upstream contract retained from the supplied endpoint: `GET https://api-faa.my.id/faa/ai-promt?prompt=...&query=...`.
- The frontend continues to use same-origin `POST /api/ai-chat` as the primary path and a direct upstream fallback.
- The AI context is compact and app-specific rather than the raw localStorage blob. It contains user basics, active targets, today's completion state, recent 14-day progress, notes, streak, XP/level, achievements, equipped frame, weekly/monthly statistics, and insight data.
- Chat history is trimmed to recent messages so the model can understand phrases such as "yang tadi" without sending the entire conversation.
- Local action handling is deterministic for the supported target operations: create, edit, delete, complete, and list. This keeps state mutation inside the application instead of granting the model arbitrary write access.
- New-target creation uses the exact New Target fields and exact reminder/repetition options from the UI. Missing values are requested one by one; completed input is previewed before save.
- The AI personality is intentionally conversational and can answer light out-of-topic chat instead of using a fixed refusal sentence for every unrelated message.
- No API key or private credential is added to the frontend.
