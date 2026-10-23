# PUFSD Pulse

A Cloudflare-native real-time feedback app for Pleasantville Schools.

## Included
- Poll builder: choice, ranking, Likert, open response
- Participant join by short code/QR
- Durable Object WebSocket refresh notifications
- D1 persistence and duplicate response protection
- Cloudflare Access presenter identity support
- Moderation-by-default storage for open responses
- Security headers, server validation, no trackers

## Deploy
1. `npm install`
2. `npx wrangler login`
3. `npx wrangler d1 create pufsd-pulse`
4. Replace `REPLACE_WITH_D1_DATABASE_ID` in `wrangler.jsonc`.
5. `npm run db:remote`
6. `npm run deploy`
7. Add `pulse.pufsd.org` as the Worker custom domain.
8. In Cloudflare Zero Trust, protect `/studio*` and presenter mutation routes with Access.

## Important production hardening
- Add a Cloudflare Access service-token check or JWT validation inside the Worker, not only at the route edge.
- Add Turnstile verification to `/api/sessions/:id/respond`.
- Apply Cloudflare rate-limiting rules to join, response, and WebSocket endpoints.
- Add a moderation interface before enabling projected open-ended responses.
- Test the custom domain and WebSocket path on a PUFSD student device through Lightspeed.
- Review retention settings with district privacy counsel before collecting student responses.

## Local development
Run `npm run dev`. Presenter routes expect `Cf-Access-Authenticated-User-Email`; for local-only testing, the Worker also accepts `x-dev-user`. Never expose that development bypass in production; remove it before launch.
