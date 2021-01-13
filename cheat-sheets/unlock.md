# Screen Unlock

## KDE

Sometimes, the KDE screen lock tool will not display or correctly unlock. To resolve
that perform the following steps:

1. Switch to alternate console: `ctrl+alt+f2`
2. Run: `loginctl unlock-session 3`
3. Switch back to main console: `ctrl+alt+f1`

A login may be required when switching to the alternate console. The session to unlock
(3 in the example above) may need to be changed, however I have always used 3 without
issue thus far.
