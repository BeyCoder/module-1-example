# Module 1: Personal Dashboard with a Weather-Reactive Sparky Companion

Version: 2.2 • October 6, 2026  
Status: Implemented classroom prototype specification

## 1. Purpose

Build a simple personal dashboard for business students. The page shows a greeting, current Tempe weather, daily tasks, and a draggable Sparky-inspired Sun Devil companion.

The dashboard is intentionally small. It gives students a concrete app for practicing specification, implementation, interaction design, local state, responsive layout, testing, and review. It is not a business simulation and does not require an external AI service.

This is a proposed alternative Module 1 exercise and does not claim to replace the syllabus's Maze lab without instructor approval.

## 2. Core experience

“I open my dashboard, see the current weather and today’s tasks, drag Sparky to a comfortable spot, and check off ‘Prepare presentation slides.’ Sparky celebrates.”

The pet is a floating, draggable companion inspired by the ChatGPT-style pet interaction. The project uses a custom original Sun Devil-style illustration with maroon and gold colors. It must not be presented as an official ASU logo or official mascot asset.

## 3. Scope

### Required

- One responsive dashboard page.
- Editable first name and greeting with the displayed date.
- Weather card for Tempe conditions from Open-Meteo, with local sample fallback.
- Automatic current Tempe weather from Open-Meteo with local fallback states.
- Animated full-page weather background behind the dashboard cards.
- Daily tasks with add, complete, reopen, and delete actions.
- Draggable Sparky companion with a speech bubble, celebration state, and Hide/Show control.
- Browser-local persistence for profile, tasks, pet position/visibility, and weather state.
- Weather-aware Sparky reactions that stay synchronized with the selected weather state.
- Lottie-ready Sparky animation runtime with a safe transparent-image fallback so a malformed animation asset can never cover the dashboard with an opaque block.

### Explicitly out of scope

Accounts, cloud databases, multi-user collaboration, Canvas/calendar connections, business simulations, paid weather services, API keys, backend weather proxies, AI-generated advice, chat history, voice, pet-care mechanics, points, and external secrets.

## 4. Page and layer layout

- **Welcome area:** date, greeting, and editable name.
- **Weather card:** location, live or fallback temperature, condition, icon, and source label.
- **Task card:** completion count, progress ring, task rows, and add-task input.
- **Weather scene layer:** fixed, full-viewport, pointer-events-disabled background behind the content layer.
- **Pet layer:** fixed above the content and weather layers. Sparky can be moved without blocking dashboard controls.

Cards remain opaque and readable while the weather scene supplies the visual motion around them.

## 5. Weather model and requirements

Use one configuration map for fallback weather content so the card text, icon, state, and scene class stay synchronized.

```text
weather: {
  state: "sunny" | "cloudy" | "rainy" | "windy"
}
```

The dashboard calls Open-Meteo directly from the browser for current Tempe temperature and weather code. It requires no account, API key, backend, or secret. If the request fails, the four local sample configurations remain usable.

The implemented states are:

| State | Card content | Background behavior |
|---|---|---|
| Sunny | Current API temperature or 76° fallback, Clear skies · comfortable, ☼ | Warm cream/sky gradient, large pulsing sun glow, and separate sunglasses-and-pool Sparky asset |
| Cloudy | Current API temperature or 68° fallback, Soft clouds · mild, ☁ | Cool gray-blue gradient with drifting cloud forms and separate cloud-accessory Sparky asset |
| Rainy | Current API temperature or 62° fallback, Light rain · take an umbrella, ☂ | Blue-gray gradient, continuous diagonal rain, glowing lightning, and separate umbrella Sparky asset |
| Windy | Current API temperature or 71° fallback, Breezy · bright intervals, ≋ | Pale sky gradient with visible directional streaks and separate scarf Sparky asset |

Fallback weather values are fictional and labeled “Sample weather.” Successful current conditions are labeled “Live weather.”

### Weather acceptance criteria

- Open-Meteo current conditions update temperature, condition, source label, and background state when available.
- If Open-Meteo is unavailable, the last local weather state remains usable.
- Rain includes visible animated lightning in addition to rain motion.
- Wind includes clearly visible moving streaks/particles, not only a subtle color change.
- The scene never intercepts clicks or pointer events intended for cards, buttons, inputs, or Sparky.
- `prefers-reduced-motion: reduce` disables continuous motion and leaves static weather styling visible.

## 6. Functional requirements

| ID | Requirement | Acceptance criteria |
|---|---|---|
| F1 | Personal greeting | Student edits their first name; greeting updates immediately and survives refresh. Empty name produces a neutral greeting. |
| F2 | Weather state | Four manual states update the card and scene from one weather configuration map. |
| F3 | Tasks | Student adds a nonempty task, completes it, reopens it, and deletes it. Count and progress update correctly. |
| F5 | Drag Sparky | Mouse and touch dragging move the full mascot within the viewport. The image's entire visible hit area is draggable, and the dropped position persists. |
| F6 | Click Sparky | A click/tap without meaningful movement shows one short predefined greeting. A drag does not show a greeting. |
| F7 | Celebrate | Completing an incomplete task triggers a brief Sparky bounce and congratulatory message exactly once for that transition. |
| F8 | Hide/Show | Hide removes Sparky and its bubble. Show Sparky restores it. No Reset control is required in the current UI. |
| F9 | Layering | Weather animation is behind cards; Sparky remains above the background and interactive. |
| F10 | Persistence | Refresh restores name, tasks, weather, pet visibility, and pet position. Invalid storage falls back to usable defaults. |
| F11 | Weather reaction | Sparky shows a matching animated weather prop and short predefined message. Task celebration text takes priority temporarily, then the weather reaction returns. |

## 7. Sparky companion behavior

Use the project asset `public/sparky-companion.png`, an original custom illustration with a transparent background, maroon hoodie/body, gold face and accents, horns, tail, and waving pose. It is a visual inspiration for a Sun Devil companion, not an official trademark asset.

| State | Trigger | Behavior |
|---|---|---|
| Idle | Default | Stable mascot artwork with a short bubble asking for the top priority. Weather effects can animate around the mascot without shaking its body. |
| Greeting | Click or tap without drag | Shows a short predefined message. |
| Dragging | Pointer/touch movement beyond the small movement threshold | Moves one fixed layer using a GPU-backed transform; does not open the greeting. |
| Celebration | Task changes from incomplete to complete | Brief bounce and “Sparky says: nice work!” message, then returns to idle. |
| Hidden | Hide button | Removes mascot and bubble; Show Sparky restores it. |

The mascot body remains stable during normal operation; weather motion is communicated through the animated background and weather props. A brief bounce is reserved for completing a task. Reduced-motion preferences disable continuous weather motion.

The frontend includes separate generated transparent still assets: `public/sparky-sunny.png`, `public/sparky-cloudy.png`, `public/sparky-rainy.png`, and `public/sparky-windy.png`. Each selected state remounts and swaps its image immediately so a previous weather asset cannot remain stuck. The companion remains visually stable; weather motion is communicated by the background layer. The local `lottie-web` hook remains isolated with a safe fallback, and an invalid animation asset must not render an opaque rectangle or hide the mascot. The visible companion remains draggable as one element, with native image drag disabled.

Sparky’s reaction mapping is deterministic and local: Sunny uses a sunglasses-and-pool illustration and an upbeat focus message; Cloudy uses a cloud illustration and calm planning message; Rainy uses an umbrella illustration and umbrella/small-win message; Windy uses a scarf illustration and priorities message. The reaction changes when the live or fallback weather state changes. These are generated transparent image assets, not emoji icons.

Keep the full mascot inside the viewport by clamping coordinates. Do not duplicate the mascot during movement. The position update must move one DOM element rather than render a trail or second copy.

## 8. Accessibility and practical behavior

- Task actions and Hide/Show control are keyboard accessible with visible focus indicators.
- Give the mascot an accessible label such as “Move Sparky, your dashboard companion.”
- The weather layer has `aria-hidden="true"` and `pointer-events: none`.
- Do not communicate task completion or weather state through color alone; pair color with text/icon labels.
- Respect reduced-motion preferences for weather particles, lightning, cloud movement, and pet celebration.
- Render entered name and task text as text, not HTML.
- If browser storage is unavailable, the app remains usable for the current session.

## 9. Implementation details

Use the existing React/Vite frontend. No backend is needed for the baseline. Keep weather definitions in the main dashboard state and keep weather-scene CSS in a separate stylesheet so content and animation rules remain easy to review.

Use CSS keyframes for the weather scenes, generated state-specific images for Sparky, and the local Lottie runtime as the companion animation integration point. Fetch only current Tempe weather from Open-Meteo in the browser; do not add a backend, API key, or secret.

The browser-local record is:

```text
version: 1
profile: { firstName }
tasks: [{ id, title, completed }]
pet: { x, y, hidden }
weather: { state: "sunny" | "cloudy" | "rainy" | "windy" }
```

Use pointer events for dragging. Save position changes through the existing local persistence mechanism. Position the pet with a single `translate3d(x, y, 0)` transform and `will-change: transform` so movement does not leave a stale painted copy. The weather scene must remain fixed behind the content and must not receive pointer input.

## 10. Suggested Module 1 exercise

Provide the working dashboard as a starter. Students choose one bounded feature change, such as:

- Add a fifth weather state with a new card configuration and animation.
- Add a weather-aware task suggestion without using an AI API.
- Add a task priority flag and filter.
- Add a keyboard shortcut to move Sparky to a preset corner.

Before coding, students write a user story, non-goals, acceptance criteria, affected components, and test plan. They implement on a feature branch, add at least one meaningful automated test, inspect the diff, and submit the specification, implementation, test evidence, and architecture note in a pull request.

## 11. Verification and completion

Verify in the browser at desktop and mobile widths:

- Each weather state updates all card fields and its background.
- Rain visibly flashes lightning; Windy visibly moves particles/streaks.
- Reduced-motion mode produces static weather styling.
- Weather layers do not block cards or controls.
- Sparky can be dragged with mouse and touch, remains one image, stays on-screen, and persists after refresh.
- Clicking Sparky without dragging shows a greeting; dragging does not.
- Sparky’s visible artwork remains intact while its local Lottie hook initializes; no maroon/opaque placeholder rectangle is allowed.
- Completing a task celebrates once; reopening or refreshing does not.
- Name, tasks, weather state, pet position, and visibility persist.
- The layout remains usable on desktop and narrow mobile viewports.

The prototype is complete when these behaviors work, `npm run build` passes, and the one-minute demonstration can show a weather switch, a task completion, and a Sparky move without relying on an external service.
