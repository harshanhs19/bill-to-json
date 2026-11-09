# Build log

## October 3, 2026

### Implementation started, 16:44 UTC

- Started a new, independent invoice PDF/image-to-JSON demo for Webytex's Business audience.
- Kept all code, interface text, documentation, and examples in English, with Canada as the primary audience and the United States as secondary.
- Scoped the application to one invoice, visible review checks, and JSON export. All fixtures are synthetic; no client code or documents are included.
- Used AI-assisted implementation, tests, and documentation. This timestamp is an elapsed-time marker, not a human engineering-hour estimate.

### Local verification checkpoint, 17:15 UTC

- Implemented the interface, three synthetic examples, local file preview, structured overview, JSON view, copy/export, review checks, and contact links.
- Implemented the GPT-5.6 Sol adapter with high reasoning, schema validation, input-token preflight, explicit upload consent, file limits, bounded timeouts, and no automatic generation retries.
- Kept live mode disabled by default. Hosted mode also requires exact-origin configuration, BotID, and operator confirmations of external spending/rate controls; these flags do not configure providers.
- Passed type checking, 27 tests in four files, and the local production build, including the mobile text and accessibility corrections.
- Verified the sample warnings, JSON export against its expected fixture, keyboard tabs, PNG preview, unsupported-file rejection, sample recovery, and responsive layout at 375 CSS pixels.
- Observed clipboard success in the interface, but did not independently read back clipboard contents.
- Observed no production browser errors or warnings in the local check. With live mode disabled, extraction returned HTTP 503 without contacting OpenAI.

### First live evaluation, 23:02:20 UTC

- Ran two synthetic extraction requests, one PDF and one PNG.
- The PDF matched the expected fields. The PNG exposed an ambiguity in how a printed tax label and percentage were represented.
- Stopped the pass and clarified the prompt/schema before rerunning the complete fixture set. The mismatch remains recorded; it was not excluded from the development history.

### Complete live evaluation, 23:03:42 UTC

- Evaluated all three fictional invoices as both PDF and PNG with GPT-5.6 Sol and high reasoning.
- All six fixtures matched their expected values across 66 top-level field comparisons, excluding notes and preserving array order.
- Measured adapter durations from 3.115 to 4.259 seconds, with a mean of 3.722 seconds. These include counting and generation, not browser upload.
- Estimated USD 0.113707 in model usage for this pass and USD 0.151056 across all eight recorded requests. Estimates exclude hosting and any separate token-counting cost and are not provider billing receipts.
- Retained the limits of this small dataset: it was used during refinement and does not establish independent accuracy, commercial validation, or performance on unseen invoices.

### Public release checkpoints

- Created [the public GitHub repository](https://github.com/mspoli96-dev/invoice-to-json).
- Published the Vercel page at [the demo URL](https://webytex-invoice-to-json.vercel.app). The interface and source links are available; its configuration reports live extraction enabled.
- Added the project cover and prepared concise setup, configuration, license, contact, and validation documentation.
- Kept private planning, account research, publication coordination, and editorial drafts out of the public source inventory.
- Passed an intermediate 28-test suite and release build with Node.js 24.x, then tightened browser verification to reject malformed BotID results.
- Passed the final local 34-test suite across four files, type checking, and the Next.js 16.3.8 production build after that fix.
- Verified a narrowly scoped hosted rate limit on `POST /api/extract`: three requests per 60 seconds per IP address in each region. The fourth and fifth requests returned HTTP 429 after enforcement was enabled.
- Configured and inspected a USD 5 provider project spending limit. The usage display was still awaiting the earlier evaluation traffic; no threshold-enforcement test or zero-cost claim is made.
- Verified the complete hosted browser flow after the strict BotID fix: a consented upload of `missing-currency.png` produced an AI-extracted result with unresolved currency, its warning, and total 265.55. The interface reported 4.0 seconds and an estimated model cost of approximately USD 0.0157; no console errors or warnings were captured.
- Verified that a direct request with a valid synthetic document and correct origin, but without browser verification, returned HTTP 403 before contacting OpenAI.
- The hosted smoke request is separate from the earlier eight-request evaluation aggregate.
- Published source revision `c0a2de8` to GitHub. [Its Linux CI run](https://github.com/mspoli96-dev/invoice-to-json/actions/runs/37161856366) passed clean installation, type generation, type checking, 34 tests, and the production build.
- Verified a READY production deployment produced by Vercel's Git integration from the same source revision. Subsequent documentation-only commits retain the application implementation verified here.
- Updated the existing LinkedIn draft with the original invoice-to-JSON cover, public links, and measured results. The article remains a draft; no social publication has been made.

### November 4, 2026: UI & DeepSeek-v4.1-flash update

- Renamed application to "bill to json hs".
- Integrated DeepSeek-v4.1-flash model provider handling and Experiential Labs API endpoints (`https://api.experientiallabs.ai/v1`).
- Added NVIDIA NIM endpoint integration (`https://integrate.api.nvidia.com/v1`) with automatic routing for `nvapi-` API keys and `deepseek-ai/deepseek-v4.1-flash` model identifiers.
- Added markdown code-block delimiter cleaning during JSON response parsing to support chat completion models that enclose JSON schemas in markdown fences.
- Replaced the interface with the requested Forest Green (`#06110C`), Charcoal Black (`#171717`), and Crimson Red (`#A31C1B`) luxury document extraction workbench UI (`src/app/page.tsx`, `src/app/layout.tsx`, `src/app/globals.css`).
- Integrated interactive scenario switches, bi-directional invoice-to-extracted-field hover highlights, draft JSON schema inspection modal, syntax-highlighted JSON viewer, and direct upload handling to `/api/extract`.
- Updated default API key resolution in `src/lib/config.ts` to support the user's latest NVIDIA NIM key and normalized API key prefix parsing.
- Hardened upload response handling in `src/app/page.tsx` (`handleFileUpload`) with safe text-to-JSON parsing, preventing uncaught `SyntaxError: Unexpected token 'A', "An error o"... is not valid JSON` when upstream gateways return HTML/text error messages.
- Added explicit HTTP 504 gateway timeout and serverless cold-start error mapping in `src/lib/openai.ts`.
### November 4, 2026: Google Gemini integration update

- Integrated Google Gemini via official OpenAI-compatible endpoint (`https://generativelanguage.googleapis.com/v1beta/openai/`) using `gemini-3.5-flash` with automatic fallback to `gemini-3.6-flash`.
- Updated default API key resolution in `src/lib/config.ts` to support the user's Gemini API key with key prefix normalization (`AQ.` and `AIzaSy`).
- Enhanced structured schema instructions in chat completions to ensure exact compliance with `invoiceSchema` for multimodal image extraction.
- Validated end-to-end invoice image-to-JSON extraction with real model inference (averaging 3-6s per document).
- Passed `npm run typecheck`, 34 tests in `npm test` across four test files, and Next.js 16.3.8 Turbopack production build (`npm run build`) with zero warnings.
- Removed the enterprise promotional CTA banner section ("YOUR PROCESS COULD BE NEXT") and redirected header navigation directly to the interactive workbench.

### November 5, 2026: DeepSeek-v4.1-flash API key update

- Configured default API key resolution and routing for the user's unified API gateway key in `src/lib/config.ts` and `src/lib/openai.ts`.
- Integrated endpoint routing to `https://api.apinex.bond/v1` for `sk-apx` keys targeting `deepseek-v4.1-flash`.
- Added automatic fallback handling between `free/deepseek-v4.1-flash` and `deepseek-v4.1-flash` for quota resilience.
- Verified end-to-end invoice image-to-JSON extraction with real model inference (completed in ~5.9s with 5/5 verification checks passed).
- Passed `npm run typecheck`, 34 tests in `npm test` across four test files, and Next.js 16.3.8 Turbopack production build (`npm run build`) with zero warnings.

See [validation](VALIDATION.md) for the method, fixture results, and remaining verification boundaries. These checkpoints describe parallel AI-assisted work and do not establish a final build-duration or human-effort claim.
