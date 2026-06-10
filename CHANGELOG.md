# Changelog

All notable changes to this project are documented here.

This is a community-maintained fork of
[cheapestinference/claude-auto-retry](https://github.com/cheapestinference/claude-auto-retry),
created to merge community contributions that were left open upstream. All
fixes below were authored by the contributors credited; git author information
is preserved on each commit.

## [0.3.0]

Integration of open community pull requests from the upstream repository.

### Fixed
- **Rate-limit reset waits in UTC+ timezones** no longer over-wait by ~24h.
  The timezone correction now converges on the same-day reset time.
  (#9, [@adbrowne](https://github.com/adbrowne))
- **Retries no longer pile up unsubmitted in Claude's input box.** `sendKeys`
  now sends the text and the submitting Enter as two separate tmux calls (with
  the text sent via the `-l` literal flag), so Claude's TUI paste-heuristic no
  longer swallows the Enter. (#8, [@ibootz](https://github.com/ibootz))
- **Stale rate-limit text in scrollback no longer causes ~24h sleeps.** The
  monitor scans the pane bottom-up for the most recent reset line and guards
  against absolute reset times that have already passed.
  (#8, [@ibootz](https://github.com/ibootz))
- **Redundant resends after Claude resumes** are avoided via a pane-bottom
  signature check, instead of re-matching the lingering limit message.
  (#8, [@ibootz](https://github.com/ibootz))
- **Mouse scroll and clean exit for tmux sessions.** Mouse mode (scroll,
  copy-mode, pane selection) and vi copy-mode keys are enabled, and the session
  now exits cleanly when the launcher exits. (#5, [@benzntech](https://github.com/benzntech))

### Added
- **Detection of "session limit" and "weekly limit" messages**, in addition to
  the existing 5-hour / usage-limit patterns.
  (#13, [@jamesjlopez](https://github.com/jamesjlopez))
- **Auto-navigation of Claude's spend-limit menu** ("Adjust monthly spend
  limit" / "Wait for limit to reset"), selecting "Wait for limit to reset".
  Includes a guard against false positives on unrelated Claude menus.
  (#14, [@kkleidal](https://github.com/kkleidal))
- **Auto-confirmation of the "Stop and wait for limit to reset" prompt** by
  sending Enter when that dialog is the default selection.
  (#12, [@SpaceMoehre](https://github.com/SpaceMoehre))
- **Compound relative-time parsing** such as `resets in 1h 25m`.
  (#5, [@benzntech](https://github.com/benzntech))

### Notes
- Where multiple upstream PRs solved the same bug, the most robust
  implementation was kept and the duplicates dropped: the send-keys split came
  from #8 (superseding the equivalent changes in #11, #12, #13, #14), and the
  timezone fix came from #9 (superseding #4 and the equivalent change in #5).
- PR #4 ([@kpmrozowski](https://github.com/kpmrozowski)) and PR #11
  ([@phillipcheng](https://github.com/phillipcheng)) addressed bugs that were
  fixed here via #9 and #8 respectively; thanks to both for the reports and fixes.
