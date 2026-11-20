---
name: flutter-app-architecture
description: Design or revise a Flutter app's architecture around a real user flow, including view/state ownership and data boundaries. Use for new Flutter apps or substantial structural changes (Flutter architecture, 플러터 아키텍처, 상태 관리, 앱 구조); skip routine widget fixes.
---

# Flutter app architecture

Start with the first user action that must work end to end. Name its entry point, input, loading and error states, data read or write, navigation outcome, and what must survive a screen rebuild, route change, or app restart. In an existing app, trace that flow through the current widgets, state management, data sources, and tests before changing structure. Preserve its chosen state management approach unless a concrete failure calls for migration.

## Assign authority and lifetime

- Keep presentation and short-lived interaction state near the view. A selected tab, animation, or unsent form value can stay in widget state when no other feature needs it. A screen's loading, error, and displayed data belong to its existing presentation state owner (for example a controller, notifier, bloc, or view model). Define when that owner is created and disposed.
- Give shared or durable application data one source of truth. Put reads, writes, caching, and reconciliation behind a repository when multiple views, data sources, or lifetimes make that boundary useful. A service wraps an external API, database, file, or platform plugin where isolating that I/O helps. Do not add layers solely to match a diagram.
- Make dependency direction legible: user event → presentation command → data owner → updated presentation state → view. Keep storage and network details out of widgets. Add a domain/use-case component only for complex or reused logic that does not fit cleanly in a presentation owner or repository.

## Check the flow across boundaries

- Choose who owns navigation decisions and how route arguments/results cross the boundary. Follow the app's existing router; keep `BuildContext` use in the view or routing adapter. Account for back navigation and restoration only where the flow requires them.
- For each async action, specify pending, success, failure, retry, and disposal behavior. Prevent stale completions or duplicate submissions when they would change the outcome. Add offline reads, queued writes, and conflict policy only if offline use is a product requirement.
- Show a compact boundary map naming each component, its input/output, source of truth, and lifetime. For a design request, show the first implementation slice. When implementation is requested, connect that flow through the chosen boundaries.
- For a design, specify the expected observations. When implementing or testing, verify with focused unit tests for data/presentation rules, a widget test for visible states and interaction, and an integration or manual flow check where routing, persistence, or platform behavior matters. Report the observed path and any unverified edge.

Flutter's [architecture guide](https://docs.flutter.dev/app-architecture/guide), [state lifetime guidance](https://docs.flutter.dev/data-and-backend/state-mgmt/ephemeral-vs-app), and [testing overview](https://docs.flutter.dev/testing/overview) provide the baseline; adapt their examples to the app's size and existing conventions.
