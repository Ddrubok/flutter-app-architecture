# Flutter App Architecture

[한국어](README.ko.md)

Trace a user action through widgets, screen state, and the part of a Flutter app that reads or writes data.

```text
$flutter-app-architecture In my habit tracker, a user saves a habit and immediately leaves the screen. Decide who owns the pending save and how the list shows the result when they return.
```

## Habit save after leaving the screen

For this example, saving must finish after the user leaves the editor. Keep the operation in the app's existing state management owner outside the editor route. The repository owns the write and the durable list; the editor owns only its visible pending/error state. Give each save a stable operation ID so a retry cannot create a second habit. After a retry succeeds, ignore a late failure from an earlier attempt for that operation. A disposed editor must not navigate or show a message when a response arrives. The list observes repository data and shows the saved habit once.

| Controlled event order | Repository and list | Closed editor |
|---|---|---|
| Tap Save → leave editor → delayed success | One saved habit; list shows one row | No navigation or message |
| Tap Save → leave editor → failure → retry succeeds | One saved habit; list shows one row | No navigation or message |
| First request held → timeout → retry succeeds → first request reports failure | One saved habit; list stays on success, with no error or unsaved state | No navigation or message |

Use a controllable fake repository in a unit test to release responses in those exact orders and check operation identity and final state. A widget test checks the list and absence of editor effects after the route closes. An integration check saves, leaves, and reopens the list against the real persistence path. Design restart recovery separately if the product requires it.

## Install

[Download the skill ZIP](https://github.com/Ddrubok/flutter-app-architecture/releases/latest/download/flutter-app-architecture.zip) and extract it into your Codex skills directory, `$CODEX_HOME/skills` or the default `~/.codex/skills`. The ZIP contains the `flutter-app-architecture` folder. After extraction, open `flutter-app-architecture/SKILL.md` to read the instructions.

You can use this skill on its own. If you already use the general skill, it can fetch this specialist during a design task. [Software Architecture](https://github.com/Ddrubok/software-architecture)
