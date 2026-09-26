# Gym-Me

## Problem
An existing 5-day hypertrophy routine lives in a markdown file with `- [ ]` checkboxes. Reading it on a phone between sets is clumsy: scrolling through raw markdown, tapping bracketed syntax, losing your place. The file works as a *plan*; it does not work as a *screen you look at in the gym*.

Secondary, and equally important: this project is the owner's SwiftUI learning vehicle. v1 exists as much to acquire the stack as to solve the routine-viewer problem.

## Users and primary use case
Single user: the app owner. Primary use case is standing at the gym between sets, glancing at the phone to see the selected training day — which exercises remain, target sets and rep range — and tapping exercises off as they are completed.

## v1 scope — the smallest thing that is genuinely usable end to end
A native iOS app, installed via Xcode to the owner's personal iPhone, that:
- Opens on a picker of the five training days, or on whichever day was last viewed.
- For the selected day, displays every exercise grouped by muscle group, with the target sets × rep range shown correctly and unambiguously (e.g. `4×6–8`).
- Lets the user tap an exercise to toggle a visible "done" state.
- Holds checked state in memory only for the current app session. Closing the app clears everything. No storage layer.

That is v1 in full. Static routine data, in-memory checkboxes, one day-picker, one day view. Nothing else.

## Non-goals for v1 (some are candidates for later versions)
- Logging actual weight lifted or reps performed.
- Any history, charts, PRs, week-over-week progress, or completion streaks.
- Rest timer, warm-up guidance, form videos, exercise descriptions.
- Persisting checkbox state across app launches.
- Auto-selecting today's training day from the calendar / weekday.
- Editing the routine inside the app.
- Accounts, sign-in, cloud sync, sharing, or multi-device use.
- Apple Watch app, HealthKit integration, push notifications, widgets.
- App Store submission or TestFlight distribution.
- iPad, Mac, landscape, or non-owner-device support.

A paid Apple Developer account ($99/yr) is explicitly deferred, not permanently ruled out — see Constraints.

## Constraints (stack, hosting, budget, deadline)
- **Stack:** Swift + SwiftUI, native iOS. No cross-platform frameworks, no web views.
- **Distribution:** Sideloaded from Xcode to the owner's personal iPhone using a free Apple ID. Accepted consequence: the provisioning profile expires every 7 days and the app must be rebuilt and reinstalled to keep running. Upgrading to a paid Apple Developer account is a deferred option for post-v1 if the app proves worth carrying every day.
- **Backend:** None. All data stays on-device.
- **Budget:** $0 for v1.
- **Deadline:** None stated. Open-ended timeline is a known risk; because the co-purpose is learning SwiftUI, progress is measured by the learning trigger in Open Questions rather than a date.

## Acceptance criteria — numbered, each one objectively checkable
1. The app builds in Xcode and installs to a physical iPhone with no errors.
2. On launch, the app shows either a picker of the five training days or the day that was viewed most recently in the same session; it never guesses a day from the calendar.
3. From any screen, the user can switch to any of the five training days in no more than two taps.
4. Every exercise from `docs/routine.md` appears under the correct day and muscle-group heading, with its target sets × rep range correct and unambiguous (e.g. `4×6–8`, `3×12–15`, `1×MAX`).
5. Tapping an exercise row toggles a visibly distinct "done" state; tapping again clears it.
6. The app makes no network requests in any flow and functions fully with the device in airplane mode.
7. No login, account, sign-up, or personal-data prompt is shown at any point.
8. As a learning-outcome check, the repository contains a short notes file that lists at least three distinct SwiftUI or Swift concepts the owner used for the first time while building v1 (e.g. `List`, `@State`, `NavigationStack`).

## Open questions
- **Routine source of truth.** Default assumption: routine ships as a bundled data file derived from `docs/routine.md`, so edits happen in one human-readable place. Alternative is hardcoding in Swift — cheaper for v1, worse if the routine changes even once.
- **Optional / finisher items** (Day 4 "Optional Finisher", Day 5 forearms). Treated as ordinary checkable rows in v1, or visually marked optional? Default assumption: ordinary rows.
- **v1 "done" trigger.** Because the co-purpose is learning, the suggested trigger is *both*: (a) the app has been used to run one full workout at an actual gym on the owner's iPhone, and (b) the notes file from acceptance criterion 8 exists and is honest.
- **When to buy a paid Apple Developer account.** Suggested trigger: after the app has been rebuilt-and-sideloaded three separate times because the free provisioning expired. At that point, the annoyance has exceeded the $99/yr price.
