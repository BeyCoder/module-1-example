# Module 1 Student Instructions: Weather-Reactive Business Dashboard

## Purpose

Build a small personal dashboard for a business student. The dashboard should make a student's day easier to scan: current weather, today's tasks, and a friendly draggable Sparky-inspired companion.

This exercise is about practicing the full specification-to-change workflow. It is intentionally small. Focus on one reliable interaction rather than building a large product.

## Start here

1. Read `MODULE_1_PERSONAL_DASHBOARD_SPEC.md` before writing code.
2. Review the four transparent Sparky reference images in this repository.
3. Create your own application repository and choose your frontend stack.
4. Write your feature specification before implementation.

The reference images are original generated assets inspired by a Sun Devil character. They are not official ASU logo or mascot files. Do not claim that they are official university assets.

## Required baseline

Your dashboard must include:

- A greeting and editable first name.
- A weather card for Tempe.
- A daily task list with add, complete, reopen, and delete actions.
- A draggable Sparky-inspired companion with a short message and Hide/Show control.
- Responsive layout for desktop and mobile widths.
- Browser-local persistence for the name, tasks, weather state, pet position, and visibility.
- A readable weather background that does not block clicks or cover the cards.

Use one weather configuration map so the weather card, background, and Sparky image cannot disagree.

## Weather data

You may use Open-Meteo directly from the browser. It does not require an API key or backend. Keep a local fallback so the dashboard still works when the request fails. Clearly label live data and fallback/sample data.

Do not add secrets, paid APIs, a backend proxy, or credentials to the project.

## Choose one bounded change

Before coding, select one small feature such as:

- Add a keyboard shortcut to move Sparky to a preset corner.
- Add a task priority flag and filter.
- Add a fifth weather state with its own image and background treatment.
- Add a local weather-aware task suggestion without an AI API.
- Add reduced-motion behavior for the weather scene.

Your feature must change user-visible behavior. Cosmetic-only changes do not satisfy the exercise.

## Specification-first checklist

Create a short feature note containing:

- User story.
- Non-goals.
- Acceptance criteria.
- Affected components or files.
- Risks and accessibility considerations.
- Test plan.

Ask your coding agent to critique the feature note and propose an implementation plan before it edits code. Review the plan yourself.

## Implementation checklist

- Work on a feature branch.
- Implement the smallest complete change.
- Add or update at least one meaningful automated test.
- Test both the normal path and one failure or edge case.
- Check keyboard access and narrow-screen layout.
- Inspect the final diff and remove unused code or assets.
- Document any AI-generated code you rejected or corrected.

## Submission

Submit a pull request containing:

1. The feature specification.
2. The implementation.
3. Test evidence and the command used to run tests.
4. A short architecture note describing what changed and what stayed stable.
5. One screenshot or short recording showing the feature working.

## Definition of done

The feature is complete when the acceptance criteria pass in the browser, the automated test passes, the app remains usable without external secrets, and another student can understand the change from the pull request alone.
