# Telegram Accessible — patch set

Drop all `.patch` files from this folder into your fork's `patches/` directory
(replacing any older/superseded ones — see notes below), commit, and run the
`Build APK` GitHub Action.

## Included patches

- **01-dialogcell-name-then-type.patch** — Chat list rows: TalkBack now
  announces the chat name before its type ("shamloo. Channel." instead of
  "Channel. shamloo.").

- **03-progress-announce-and-share.patch** — Two changes to
  `ChatMessageCell.java`:
  - The small "Share" icon drawn directly on message bubbles is hidden
    (the Share option in the long-press message menu is untouched).
  - Upload/download progress is announced to TalkBack every 5%.

- **04-comment-and-reactions.patch** — Two changes to `ChatActivity.java`:
  - The on-bubble "Leave a Comment" button is hidden; a "Leave a Comment"
    entry is added to the long-press message menu instead.
  - A new "Reactions" entry is added at the top of the long-press message
    menu; tapping it reveals the reactions row (hidden by default instead
    of always being a focusable row).
  - **Known risk:** this patch uses three resource names
    (`R.string.Reactions`, `R.string.LeaveAComment`, `R.drawable.msg_reactions2`)
    that could not be confirmed against the actual strings/drawables files.
    If the build fails with "cannot find symbol" for any of these, send the
    exact error back and it's a quick fix.

- **06-voice-quality-native.patch** / **06-voice-quality-java.patch** —
  Adds an adjustable voice-message recording quality (low/medium/high
  bitrate) to `TMessagesProj/jni/audio.c` and
  `TMessagesProj/src/main/java/org/telegram/messenger/MediaController.java`.
  **Apply both together** — they're two halves of one feature.
  **Not yet done:** there's no settings-screen toggle yet for the user to
  actually pick low/medium/high — the setting is read from
  `voiceRecordQuality` (0/1/2) in the app's global preferences, defaulting
  to 2 (high, i.e. today's original behavior) if never set. A follow-up
  patch is needed to add a real UI control for this once the relevant
  settings-screen file is available.

## Superseded — do NOT use these older patches if you still have them

- `02-hide-share-button.patch` — removed Share from the long-press menu,
  which is not what was wanted; replaced by the on-bubble-only fix in
  03-progress-announce-and-share.patch.
- `05-hide-onbubble-share.patch` — its content is now included inside
  03-progress-announce-and-share.patch.
