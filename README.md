# Telegram Landing Page

Meena-inspired responsive landing page with animated glass UI, unique Telegram invite links, Redis attribution and join-request-based Meta Conversions API events.

## Project-specific configuration

- Telegram channel ID default: `-3778248565`
- Meta Pixel ID: supplied separately through `META_PIXEL_ID` (no Meena pixel ID is copied or hardcoded).
- Channel title: optional `CHANNEL_NAME`.

## Vercel environment variables

- `TELEGRAM_BOT_TOKEN` — bot token; bot must be channel administrator and allowed to create invite links.
- `TELEGRAM_CHAT_ID` — optional; defaults to `-3778248565`.
- `KV_REST_API_URL` and `KV_REST_API_TOKEN` — Redis REST credentials.
- `META_PIXEL_ID` — NEW project's browser Pixel and CAPI dataset ID.
- `META_CAPI_ACCESS_TOKEN` — Meta Conversions API token for that Pixel.
- `META_GRAPH_API_VERSION` — optional; defaults to `v24.0`.
- `META_TEST_EVENT_CODE` — optional, testing only.
- `CHANNEL_NAME` — optional landing-page heading.
- `LANDING_URL` — optional deployed landing-page URL fallback.

## Setup

1. Import this repo into Vercel and configure the environment variables.
2. Add the bot as administrator in channel `-3778248565` with invite-link permission.
3. Set Telegram webhook to `https://YOUR-DOMAIN/api/telegram`, with `chat_join_request` enabled.
4. Deploy, test with an ad URL containing UTM parameters and submit a join request through the generated invite link.
5. Verify Vercel logs and Meta Events Manager. Remove `META_TEST_EVENT_CODE` after testing.

## Tracking flow

Browser PageView -> click -> unique join-request invite -> Redis stores UTM, _fbp, _fbc and click ID -> Telegram join-request webhook validates the configured channel -> Redis stores subscriber -> Meta CAPI Subscribe fires only when the invite maps to stored click attribution.

A join request is not an approved membership. Organic requests or requests without a mapped ad click do not trigger Subscribe. Do not commit bot/API credentials or access tokens.
