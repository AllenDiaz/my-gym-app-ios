# CLAUDE.md

Instructions for the coding assistant working in this repo. The spec is [docs/idea.md](docs/idea.md); read it before non-trivial work.

The Xcode project does not exist yet. When it is created, the module name is `GymMe` and the layout below applies from day one.

## Stack

- **Swift 6** — latest stable; ships with current Xcode; no reason to pin lower.
- **SwiftUI, iOS 17 deployment target** — every API a checklist view needs exists in 17; picking higher only shrinks the sideload target pool.
- **Xcode 16+** — required by Swift 6.
- **Swift Testing** (not XCTest) — Swift 6's default, less ceremony.
- **No third-party dependencies.** Do not add SwiftPM/CocoaPods packages without asking.

## Commands

```
# Build (Xcode: Cmd+B). CLI:
xcodebuild -scheme GymMe -configuration Debug -destination 'generic/platform=iOS' build

# Run on the owner's iPhone: Xcode → Cmd+R with the device selected.
# There is no CLI path for sideload to a personal device.

# Test
xcodebuild test -scheme GymMe -destination 'generic/platform=iOS Simulator'

# Lint (ships with the Swift toolchain; `xcrun swift-format` if not on PATH)
swift-format lint --recursive --strict Sources GymMeTests

# Format in place
swift-format format --recursive --in-place Sources GymMeTests
```

No `install` step. There are no dependencies to fetch.

## Layout

```
GymMe.xcodeproj/         do not hand-edit
GymMe/
  App/                   @main App struct only
  Views/                 SwiftUI views, one type per file, filename == type
  Models/                value types (Day, Exercise, MuscleGroup, ...)
  Data/                  routine loader + bundled routine data
  Resources/             Assets.xcassets, routine JSON
GymMeTests/              Swift Testing suites
docs/
  idea.md                spec
  routine.md             human source of truth for the routine
  learnings.md           AC #8 notes file
```

- New view → `Views/`, one file per view.
- New value type → `Models/`.
- Anything touching routine data → `Data/`.
- Anything importing `UIKit` directly → ask first. This is SwiftUI-only.

## Conventions

- **State lives in `@State` / `@Observable`.** No `@AppStorage`, `UserDefaults`, SwiftData, or Core Data. v1 is in-memory only; adding persistence is a spec change and requires asking.
- **No network code, ever.** No `URLSession`, no `Network.framework`.
- **Error handling.** No runtime failure surface worth showing the user. Programmer errors (malformed bundled routine, missing asset) → `fatalError` with a message. Do not wrap code in `do/try/catch` for errors that cannot occur.
- **Comments** only where code cannot say it itself, and one line. No doc-comment blocks on internal types.
- **Learning log.** When you introduce a Swift or SwiftUI concept not yet used in this repo, append one line to `docs/learnings.md` naming the concept and the file it first appears in. This is how AC #8 gets satisfied.

## Definition of done for any task

1. `xcodebuild test ...` (above) passes.
2. `swift-format lint --recursive --strict Sources GymMeTests` prints nothing.
3. `xcodebuild ... build` produces zero warnings.
4. Any acceptance criterion in [docs/idea.md](docs/idea.md) the task touches still holds.
5. One commit, imperative subject line, scope limited to the task.

If any of the above fails, the task is not done — do not report success.

## Never edit without asking

- `docs/idea.md` — the spec. Propose changes; do not apply them.
- `docs/routine.md` — the human source of truth for the routine.
- `GymMe.xcodeproj/**` — Xcode owns this. Use Xcode to add files to the project.
- Signing, team, capabilities, or bundle identifier in project settings.
- `CLAUDE.md` — this file. Propose changes.
