# MeltingPoint — Product Requirements

## Problem Statement

People regularly meet a temperature in the Unit they don't think in: a recipe giving an oven setting in Fahrenheit, a weather report or a fever reading in Celsius. Getting from one to the other means remembering a formula or using a general-purpose search or calculator. Those tools are slower than they need to be, often cluttered with adverts and tracking, and are vague about edge cases. They round silently, accept nonsense input, or show `NaN` or `-0.00` without explanation.

The User wants to type a Temperature in whichever Unit they have and see the same Temperature in the other Unit straight away, with confidence that the answer is correct and that nothing they type is sent anywhere.

## Solution

MeltingPoint is a single web page with exactly two linked Fields: the Celsius Field and the Fahrenheit Field. The User types in either one. That Field becomes the Source Field, and the other becomes the Converted Field, which shows the Converted Value immediately through Live Update. There is no Convert button.

The Conversion is exact. Only the Displayed Value is rounded, to 2 decimal places. When the Source Value is not a valid Temperature, a short, plain Validation Message appears beside the Source Field and the Converted Field is cleared, so a stale or wrong result is never shown. Emptying the Source Field returns the page to the Blank State, which shows no message at all.

Everything runs in the browser. After the page loads, it makes no network requests and stores nothing about the User. It works with a keyboard and a screen reader on current desktop and mobile browsers.

## Requirements

### Converting

1. As a User, I want to type a Temperature into the Celsius Field, so that I can see it in Fahrenheit.
2. As a User, I want to type a Temperature into the Fahrenheit Field, so that I can see it in Celsius.
3. As a User, I want the Converted Value to appear as I type, without pressing a Convert button, so that converting takes no extra step.
4. As a User, I want the Converted Value to update on every change to the Source Value, so that what I see always matches what I have typed.
5. As a User, I want to switch to typing in the other Field at any time, so that I can convert in either direction without resetting the page.
6. As a User, I want the Field I last typed in to be the one that drives the other, so that the page never overwrites what I am typing.
7. As a User, I want the Conversion to use the exact formulas (°F = °C × 9/5 + 32 and °C = (°F − 32) × 5/9), so that the answer is correct.
8. As a User, I want known reference points to convert exactly (0 °C is 32.00 °F, 100 °C is 212.00 °F, −40 °C is −40.00 °F, 37 °C is 98.60 °F), so that I can trust the tool.
9. As a User, I want the Converted Value shown to 2 decimal places, so that the result is precise without being noisy.
10. As a User, I want rounding applied only to the Displayed Value and never to the Conversion, so that rounding errors don't build up.
11. As a User, I want what I typed left exactly as I typed it, so that the page never rewrites my input.
12. As a User, I want a result that rounds to zero shown as `0.00` without a minus sign, so that I never see a confusing `-0.00`.
13. As a User, I want the Displayed Value written with a space before the unit symbol (`37.00 °C`, `98.60 °F`), so that it reads naturally.

### What counts as a valid Temperature

14. As a User, I want to enter whole numbers and decimals using a full stop as the decimal point (`37`, `37.5`, `.5`, `5.`), so that ordinary numbers just work.
15. As a User, I want to enter negative and explicitly positive Temperatures (`-40`, `+5`), so that I can convert cold Temperatures.
16. As a User, I want leading and trailing spaces ignored, so that a stray space doesn't make my input invalid.
17. As a User, I want Absolute Zero itself (−273.15 °C or −459.67 °F) accepted, so that the physical limit is still convertible.
18. As a User, I want a Temperature below Absolute Zero rejected, so that I am never shown a physically impossible result.
19. As a User, I want input with more than 7 digits before the decimal point rejected, so that results always stay readable and finite.
20. As a User, I want input with more than 2 digits after the decimal point rejected, so that my input is never silently rounded.
21. As a User, I want a decimal comma (`37,5`) treated as an Invalid Input, so that there is no doubt whether a comma means a decimal or a thousands separator.
22. As a User, I want thousands separators (`1,000`) treated as an Invalid Input, so that the meaning of a number is never guessed.
23. As a User, I want scientific notation (`1e3`) treated as an Invalid Input, so that only plain numbers are accepted.
24. As a User, I want a sign on its own (`-`, `+`), part-way through typing, treated as an Invalid Input, so that I never see a result for a number I haven't finished.
25. As a User, I want text that is not a number (`abc`) treated as an Invalid Input, so that I am never shown `NaN`.

### Feedback on Invalid Input

26. As a User, I want a Validation Message beside the Source Field when my Source Value is an Invalid Input, so that I know what is wrong and where.
27. As a User, I want the Validation Message in plain, terse UK English (for example "Enter a number."), so that I understand it at a glance.
28. As a User, I want a distinct Validation Message for each reason (not a number, too many digits, below Absolute Zero), so that I know how to fix my input.
29. As a User, I want the Absolute Zero message to state the limit in the Source Field's Unit (for example "Below absolute zero (−459.67 °F)."), so that the limit makes sense in the Unit I am typing.
30. As a User, I want the Converted Field cleared while my Source Value is an Invalid Input, so that I am never shown a stale or wrong result.
31. As a User, I want the Validation Message to disappear as soon as my Source Value becomes valid, so that the page reflects my current input.
32. As a User, I want only the Source Field to carry a Validation Message, so that I am never told the Field I am not typing in is wrong.
33. As a User, I want no pop-ups, toasts, error codes or exclamation marks, so that the page stays calm and uncluttered.

### Blank State

34. As a User, I want the page to open in the Blank State with both Fields empty and no message, so that I am not greeted by an error.
35. As a User, I want emptying the Source Field to return the page to the Blank State with no Validation Message, so that starting again doesn't look like a mistake.
36. As a User, I want the Converted Field emptied when I empty the Source Field, so that the page is consistent.

### Accessibility

37. As a keyboard User, I want to reach and use both Fields with the keyboard alone, so that I don't need a mouse.
38. As a screen-reader User, I want each Field to have a label naming its Unit, so that I know which Field I am in.
39. As a screen-reader User, I want the Converted Value announced politely as it changes, so that I hear the result without leaving the Source Field.
40. As a screen-reader User, I want the Validation Message announced and tied to the Source Field, so that I hear what is wrong.
41. As a User with low vision, I want text and controls to meet WCAG 2.2 AA contrast, so that I can read the page.
42. As a mobile User, I want the Fields to bring up a keyboard suited to number entry and to fit my screen, so that converting on a phone is easy.

### Privacy, reach and reliability

43. As a User, I want the page to make no network requests after it loads, so that nothing I type leaves my browser.
44. As a User, I want no account, login, cookies, analytics or tracking, so that I can convert anonymously.
45. As a User, I want the page to work in current Chrome, Edge, Firefox and Safari on desktop and mobile, so that I can use it wherever I am.
46. As a User, I want the page to load quickly from a static host, so that converting is faster than searching.
47. As a User, I want an unexpected fault never to show me raw technical output, so that the page stays understandable.

### Maintainers

48. As a maintainer, I want unexpected errors written to the browser console through a single error handler, so that faults can be diagnosed in devtools without any remote logging.
49. As a maintainer, I want the production bundle size reported on every build, so that the page stays small.
50. As a maintainer, I want every validity rule, Conversion and display rule covered by fast automated tests, so that changes can't silently break correctness.

## Implementation Decisions

- **Client-side only, static build.** The product is an Angular single-page app built to static files, with no server-side rendering, so the page makes no runtime server calls (Technical-Context Principle 1) and can be served from any static host.
- **Conversion is exact and pure.** The two Conversions are side-effect-free functions, kept separate from the UI and computed at full precision, so they can be verified on their own and rounding never feeds back into a result (Principle 2).
- **Validity is decided in one place.** Classifying a Source Value as the Blank State, a valid Temperature or an Invalid Input (with its reason) is a single pure step that comes before Conversion. This keeps the validity rules agreed for the glossary (number format, digit limit, Absolute Zero) consistent across both Fields.
- **Number format.** A number is an optional sign, digits, and an optional decimal point written as a full stop, with surrounding spaces ignored. A decimal comma, thousands separators, scientific notation and a lone sign are rejected. This avoids guessing between comma conventions, and the copy is UK English.
- **Digit limit.** At most 7 digits before the decimal point and 2 after. This keeps every Converted Value finite and readable, and means input is never silently rounded.
- **Absolute Zero is inclusive.** −273.15 °C and −459.67 °F are valid; anything below is an Invalid Input. The check uses the Source Field's own Unit.
- **Blank State is not an error.** An empty Source Value shows no Validation Message (Principle 3, as amended). The page opens in the Blank State.
- **No stale results.** An Invalid Input or an empty Source Field clears the Converted Field. A previous Converted Value is never kept.
- **Display rules.** Displayed Values are rounded to 2 decimal places for display only, written with a space before the unit symbol, and shown without a sign when they round to zero.
- **Role by last edit.** The Field the User last typed in is the Source Field. Typing in the other Field swaps the roles, and the new Converted Field's content is replaced.
- **State and binding.** State is held in Angular signals bound directly to the two Fields, with no forms library and no RxJS beyond what Angular itself uses, as `Technical-Context.MD` requires.
- **Feedback in the page only.** Validation Messages are inline, and Converted Values are announced through a polite live region. There are no toasts, dialogs or notifications.
- **No new runtime dependencies.** No UI kits, CSS frameworks, unit libraries, analytics or error-tracking SDKs. Adding any runtime dependency needs an ADR.

## Testing Decisions

- **What a good test is.** A test asserts behaviour that can be observed from outside: given a Source Value, the resulting Displayed Value, Validation Message or Blank State. It never asserts internal state or how something is implemented, so a refactor that keeps behaviour unchanged keeps every test green.
- **Tier 1 unit tests (Vitest), every commit:**
  - **Source Value parser.** Every class at its boundary: valid forms (`37`, `-40`, `+5`, `.5`, `5.`, padded with spaces); each rejection (`37,5`, `1,000`, `1e3`, a lone sign, `abc`); the digit limit at and over 7 digits before the decimal point and 2 after; Absolute Zero at and just below the limit, in both Units; and empty input as the Blank State.
  - **Conversion.** The reference points (0, 100, −40 and 37 °C, and Absolute Zero), each in both directions, and round trips at full precision.
  - **Displayed Value formatter.** Rounding to 2 decimal places, the unsigned zero (including results just below zero), and the unit-symbol spacing.
- **Tier 1 component tests (Vitest):**
  - **Converter component.** Live Update in both directions, role swapping between the Fields, a Validation Message beside the Source Field only, the Converted Field cleared on Invalid Input and on emptying, the Blank State on load, and the live-region announcement.
  - **Error handler.** It routes an unexpected error to `console.error`.
- **Tier 2 end-to-end (Playwright with axe), every PR.** Against the production build on Chromium, Firefox and WebKit: typing in each Field, Invalid Input handling, the Blank State, keyboard-only operation, an axe scan with no WCAG 2.2 AA violations, and an assertion that the page makes no network requests after it loads.
- **Seams.** The boundary between the pure modules and the Converter component is covered by component tests that use the real modules, not mocks. The boundary between the component and a real browser is covered by Tier 2.
- **Tier 3.** A Playwright smoke test against the live production URL, once a static host is chosen.
- **The ratchet.** Any defect found in production is fixed together with a Tier 1 or Tier 2 test for its class of defect.
- **Prior art.** None yet; the app has not been scaffolded. The first Feature sets the test patterns that later ones follow.

## Out of Scope

- Kelvin, Rankine or any Unit other than Celsius and Fahrenheit.
- Converting quantities other than temperature (length, weight, and so on).
- A unit selector, a Convert button, conversion history, favourites or sharing.
- Localisation: decimal-comma input, languages other than UK English, and locale-specific number formats.
- Accounts, settings, persistence, cookies, analytics and remote error tracking.
- Offline install (PWA) or native apps.
- Choosing, configuring and operating the static host (DNS, TLS, CDN), and the deployment pipeline. These are tracked separately.
- Working around bugs in old or non-current browsers.

## Further Notes

- `Context.MD` defines every capitalised domain term used here (Source Field, Converted Value, Blank State, and so on). Where this PRD and the glossary differ, the glossary wins.
- The CI pipeline (the required `ci` check) and the static host have not been set up yet. Tier 2 on every PR, and Tier 3, depend on them.
- This is a small, single-page product. Breaking it down with `/factory-roadmap` may produce only one or two Features (for example, the converter, then deployment).
