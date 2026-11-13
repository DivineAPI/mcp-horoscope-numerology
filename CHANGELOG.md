# Changelog

## [1.8.0] - 2026-11-12

### Added

**New tool: `divine_chinese_horoscope_period` (DV-681).** It wraps the Chinese horoscope endpoints the backend added on 2026-11-10 (astroapi-3.divineapi.com commit db63ad2, served from astroapi-5):

- `POST /api/v3/chinese-horoscope-weekly` (selector field `week`)
- `POST /api/v3/chinese-horoscope-monthly` (`month`)
- `POST /api/v3/chinese-horoscope-yearly` (`year`)

One tool covers all three. `period` (`weekly`, `monthly` or `yearly`) picks the endpoint, and `selector` (`current`, `prev` or `next`, default `current`) is sent under that endpoint's own field name. It also takes a Chinese animal `sign`, `tzone` (which decides what "current" is) and `lan`. Sign, period and selector are lowercased and checked before the call. The API compares the selector case-sensitively, so `"Current"` would otherwise be rejected.

The response has `sign`, the period covered (`week`: "08 Nov 2026 to 14 Nov 2026", `month`: "November 2026", `year`: "2026"), and a `prediction`. In it, `growth`, `health`, `wealth`, `career`, `love` and `family` are text, and `fortune` is a list of "Label : text" lines: lucky numbers, lucky alphabets, a cosmic tip, and tips for singles and for couples. The endpoints use the same plan as the daily Chinese horoscope (api_id 26). Verified live on 2026-11-13 for current, prev and next of all three periods.

**English only on this host.** astroapi-5 rejects any other `lan` with "Translation is not supported on this domain. Please use https://astroapi-5-translator.divineapi.com/…", and the translator host also needs a translator plan on the account. The same applies to the other horoscope tools. Routing translated requests there is a separate DV-681 item; the `lan` field description says so.

`divine_chinese_horoscope` (daily) now points to the new tool for longer periods. The server instructions say the Chinese tools take an animal sign and that the period tool uses current/prev/next.

Tool count: 66 -> 67.

## [1.7.0] - 2026-11-10

### Added

**`hide_frontpage_details` and `front_image` on the PDF report tools (DV-528, parent DV-523).** Some clients use a custom cover image, and the default front-page details (report name, client details, "Produced by") were printed over it. The new flag hides them so the cover shows only the image.

- **Vedic V1 reports (9 tools):** Kundali Sampoorna, Ananta and Prakash, Match Making, Government Job, Foreign Travel, and Vedic Yearly 5/10/15. They take two new optional inputs, `front_image` and `hide_frontpage_details` (a boolean, default false), through the new `VedicPDFReportInput` / `VedicPDFMatchmakingInput` models. The V1 API takes the flag as `0`/`1`, so `true` is sent as `"1"` and `false` is not sent at all. Backend: Dinesh-Gahlot/pdf.divineapi.com#3, rendered by reports.divinestore.net. The Western and Numerology V1 PDF tools are unchanged, because their covers don't use the flag.
- **`divine_pdf_reports_v2`:** gains `front_image` and `hide_frontpage_details`, sent as a JSON boolean (backend: white-label-v2). Its `report_type` examples are corrected: they listed `western-natal-report` and `numerology-report`, which this endpoint does not accept for those families.
- **New tool `divine_pdf_western_reports_v2`:** wraps `POST /api/v1/western/reports/generate`, the separate Western Reports V2 endpoint. It takes a slug or a family code plus `tier`, the birth details, the seven branding fields, and optional `house_system`, `theme`, `lan`, `front_image` and `hide_frontpage_details`. `tier` is checked before the call. `_call_reports_v2` now takes the path as a parameter.

`hide_frontpage_details=true` without `front_image` is rejected before the call. Reports V2 answers that combination with a 400, and on the V1 reports the flag has no effect without a cover image.

**Credentials:** Reports V2 accepts only active DivineAPI PDF accounts (`user_type = 2`); a standard API key gets 401 "Invalid Bearer token". Both V2 tool descriptions now say so. The `divine_pdf_reports_v2` description also stops claiming Western and Numerology support: Western has its own tool, and Numerology V2 needs `fname`/`lname`/`mobile_number`, which this tool doesn't take.

**Reports V2 errors:** Reports V2 errors (`{"error": "..."}`) now show the API's own text, e.g. "Reports V2 returned 400: front_image is required when hide_frontpage_details is true." A 401 "Invalid Bearer token" adds a hint that a PDF account is needed. Before, every 401 said "check your DIVINE_API_KEY", which pointed the wrong way here.

Tool count: 65 -> 66.

## [1.6.0] - 2026-11-09

### Added

**New tool: `divine_search_location` (DV-648).** Wraps the Location API, `POST https://geolocation.divineapi.com/location` (parent ticket DV-614; docs: developers.divineapi.com/geo-location-api/location-api). It takes `place` (at least 3 characters after trimming, checked before any call) and returns up to 10 matches, largest population first, with `latitude`, `longitude`, `timezone` (IANA name), `timezone_value` and more, plus the GeoNames attribution. The server instructions now point to it for getting `lat`, `lon` and a timezone from a place name.

`timezone_value` is the zone's offset **today**, so for a birth date in a zone with summer time it can be wrong by an hour (New York: -5 now, -4 on 1990-07-15, verified live). The tool description says so and tells the model to work out the birth-date offset from the IANA timezone.

The Location API reports errors with real HTTP status codes and its own message and details. `_call_divine_api` gains an optional `on_http_error` hook, and the tool uses a Location-specific handler; every other tool (including the PDF tools' own HTTP call) keeps the generic `_handle_http_error`. Same implementation as the Indian and Western MCPs. Tool count: 64 -> 65.

## [1.5.1] - 2026-09-17

### Fixed

**Pinned `mcp[cli]` to `<2`.** The dependency was `>=1.2.0` with no upper bound. mcp 2.x renamed `FastMCP` to `MCPServer` and removed `mcp.server.fastmcp`, which `server.py` imports at module level, so any image rebuild would have installed mcp 2.x and crashed the container on startup. Production runs 1.28.1. No behaviour change — this only prevents a future rebuild from breaking.

## [1.5.0] - 2026-07-24

### Added

- `divine_name_correction` tool for the new Chaldean Numerology Name Correction endpoint (`/numerology/v1/name-correction`, astroapi-7). Takes `full_name` + birth date and returns the current name number, life path and birthday numbers, an alignment percentage, the target name number, whether the name is already aligned, and suggested corrected spellings. Total tools 63 -> 64. This endpoint takes `full_name`, not `fname`/`lname` like its numerology siblings (verified live: fname/lname returns HTTP 422); deprecated fname/lname inputs are accepted but not forwarded.

## [1.4.2] - 2026-07-08

### Added

Add server-level instructions (how-to-use note read at connect: sign casing, h_day / current-prev-next selectors, PDF branding).

## [1.4.1] - 2026-07-08

### Fixed

**OAuth: clients registered without a scope are no longer rejected when they
request one.** The metadata advertises scope "astrology", so spec-following
connectors (e.g. ChatGPT) may request it even when their dynamic client
registration omitted it; registration now defaults the client scope to
"astrology" (default_scopes). Verified against the full simulated connector
flow: discovery, registration, PKCE authorize, login, token exchange.

## [1.4.0] - 2026-07-08

### Added

**Single-token authentication: `Authorization: Bearer <api_key>:<auth_token>`.**
For platforms that can send only one credential field and no custom headers
(e.g. the Claude Messages API MCP connector). The middleware splits the value
on the first colon and converts it to the internal JWT, exactly like the
X-Divine header pair. Real OAuth JWTs never contain a colon and pass through
untouched. Existing auth methods are unchanged.

## [1.3.0] - 2026-07-08

Param-parity fix batch for the Horoscope & Numerology server. Every change
below was verified against the live backend with curl before coding, and every
changed tool passed a live functional test after coding (23/23 astro,
14/14 PDF, including one real generated report per PDF tool).

### Base import

**v1.2.0 imported from PyPI/production.** The repository was stale at v1.0.0:
the code published to PyPI and running in the production container (v1.2.0)
was never committed to git. This commit's baseline is the extracted PyPI 1.2.0
`server.py`, which adds the `divine_pdf_reports_v2` tool (63 tools total) and
the `ApiKeyToJwtMiddleware` ASGI middleware. All 1.2.0 non-tool changes are
preserved byte-for-byte. Note: none of the parameter bugs below were fixed in
1.2.0; all fixes in this batch are new.

### Fixed (tools that FAILED ON EVERY CALL, now working)

**`divine_daily_horoscope` rewired to `h_day`.** The endpoint requires
`h_day` ('today', 'tomorrow', 'yesterday') and ignores day/month/year
entirely (byte-identical responses with and without them). The old tool sent
only day/month/year, so every call failed with "Please enter valid h_day."
New schema: required `h_day` (validated); day/month/year remain accepted as
deprecated optional inputs for backward compatibility but are not sent
upstream.

**`divine_yearly_horoscope` selector fixed.** The endpoint accepts ONLY
'current', 'prev', or 'next', but the old schema forced a 4-character year, so
'current' was rejected client-side and '2026' rejected server-side: no input
could ever succeed. The length constraint is gone; the value is validated
against the three selectors.

**`divine_love_compatibility` now takes the two signs.** The endpoint requires
`sign_1` and `sign_2`; the old tool sent neither (and `extra="forbid"` blocked
any workaround), failing every call. New required, validated zodiac-sign
params, lowercased before sending.

**`divine_which_animal_are_you` now takes name and birth date.** The endpoint
requires `full_name`, `day`, `month`, `year` (1901 to 2100); the old tool sent
only `lan` and failed every call.

**`divine_core_numbers` schema corrected.** The endpoint requires `full_name`
(it rejects fname/lname) and `method` ('general', 'chaldean', or
'pythagorean'); the old tool sent fname/lname and no method, failing every
call. `gender` is optional (the API accepts it but it provably does not change
the result). fname/lname/lan remain accepted as deprecated optional inputs,
not sent upstream.

**`divine_flames_calculator` field renamed to `your_name` (BREAKING, but the
old tool never worked).** The endpoint requires `your_name`; the old tool sent
`full_name` and got 422 "Please enter your name" on every call.

**Lifestyle tools now send `h_day` and `tzone`.** `divine_zodiac_gift_guru`,
`divine_beauty_by_stars`, and `divine_astro_chic_picks` all require h_day and
tzone (422 without); the old tools sent only sign+lan and failed every call.
h_day is validated client-side ('today', 'tomorrow', 'yesterday').

**`divine_pdf_natal_report` now takes `report_code` and `theme`.** Both are
required by the endpoint (e.g., 'CAREER-REPORT' and '001'); the old tool sent
neither and failed every call.

**`divine_pdf_couple_report` now takes `report_code`.** Required by the
endpoint (e.g., 'ALIGNED-ENERGIES-REPORT'); the old tool failed every call.

**`divine_pdf_numerology_prediction` and `divine_pdf_numerology_report`
schemas corrected.** The endpoints require `full_name` (not fname/lname),
`gender`, and a `report_code` (e.g., 'YEARLY-PREDICTION-3-YEAR' and
'SCHOLARLY-SPIRITS'); the old tools sent fname/lname and none of the rest,
failing every call. Birth time/place are optional for these reports and sent
only when provided. fname/lname remain accepted as deprecated optional inputs,
not sent upstream.

**`divine_pdf_reports_v2` transport fixed (new tool in 1.2.0, broken as
shipped).** The Reports V2 endpoint takes a JSON body with an `x-api-key`
header; the 1.2.0 tool posted form fields with an `api_key` field and got 401
"x-api-key header required" on every call. It now posts JSON with the correct
headers. The endpoint also requires ALL seven branding fields (including
company_mobile) plus footer_text and logo_url, which were missing from the
schema entirely; all are now required params.

### Changed (BREAKING schema tightening that matches API reality)

**PDF branding fields are now required on all 13 classic PDF tools.** The PDF
backend rejects every request that lacks company_name, company_url,
company_email, company_bio, logo_url, or footer_text (400 USER_INPUT_ERROR,
verified on every endpoint individually). They were previously optional and
silently dropped when empty, so any call without full branding failed at the
API. The schema now requires the six fields; only company_mobile remains
optional. This is a breaking schema change for callers who previously passed
branding, and a fix for everyone else, since those calls never succeeded.

**Weekly and monthly horoscope selectors validated.** The endpoints accept
ONLY 'current', 'prev', or 'next'. The old descriptions asked for a
'YYYY-MM-DD' week start and a month number like '03', both of which the API
rejects. Descriptions corrected and values validated client-side.

**API failures now raise ToolError.** `_call_divine_api` previously returned
plain "Error: ..." strings for HTTP errors and returned upstream error
envelopes (success=2/3, status='error') as if they were successes. It now
raises ToolError for non-2xx responses, network errors, and HTTP-200 error
envelopes, so MCP clients see isError: true. Plain "Error: ..." string returns
remain only for client-side enum validation of top-level params.

### Unchanged on purpose

The 12 tarot tools that call /api/v3/ paths where the master collection
documents /api/v2/ are left as-is: both versions are live (v3 returns a
richer 'cards' shape) and collection alignment is a separate decision.
