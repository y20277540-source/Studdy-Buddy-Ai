# StudyBuddy AI code update notes

## Included in this update
- Purple Glass Premium styling refresh in `artifacts/studybuddy-ai/public/premium-overrides.css`.
- Account dropdown question counter and rolling-reset countdown.
- Server-side quota endpoint and atomic Supabase RPC quota consumption in `artifacts/studybuddy-ai/server.js`.
- `SUPABASE_SETUP.sql` creates the quota table and secure RPC functions. The 24-hour window starts on the first successful quota reservation; failed AI calls release the reservation.
- New Chat already clears the in-memory AI context (`history.splice(0)`) while existing conversations remain in local chat history. This behavior is retained.

## Required setup before deploy
1. Open Supabase Dashboard -> SQL Editor and run `artifacts/studybuddy-ai/SUPABASE_SETUP.sql`.
2. In Replit Secrets, ensure `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`, and `GEMINI_API_KEY` are set. Never expose the service-role key in browser code.
3. Deploy the `artifacts/studybuddy-ai` Express app (its package start command is `node server.js`).
4. Run `npm run build` in that app directory and test `/healthz`, `/api/quota`, chat, image, and voice features.

## Important scope note
This update implements the quota and UI styling in the source ZIP; it does not automatically change the already-published Replit deployment. The SQL setup must be run and the app redeployed. The secure admin live-chat dashboard and Supabase-backed cross-device chat history are not yet implemented by this patch; existing chat history remains browser-local. Do not advertise those as active until added and tested.

Anonymous quotas use a server-issued HttpOnly cookie. Clearing cookies or switching browsers/devices can create a new anonymous identity; preventing that fully requires sign-in or another verification mechanism.
