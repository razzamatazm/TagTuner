# 3. Plezy watch links for video tags

Date: 2026-10-14

## Status

Accepted. Supersedes [ADR 2](0002-infuse-deep-links-for-video-tags.md) for new tags.

## Context

The user's Plezy fork (bundle `com.razzamatazm.plezy`, upstream PR edde746/plezy#2681) opens `plezy://watch` links on the Apple TV. A link names a Plex show or movie by its Plex GUID, plays the On Deck episode or shuffles the whole show, and works on every server that has the title. Plezy shows the link as a QR code under "Show watch link", which is the easiest way to get a GUID. Infuse tags needed a TMDB id and couldn't shuffle without HA picking episodes from Plex.

Tested 2026-10-14: HA's Apple TV integration reports `app_name: Plezy` while it plays, and the Plex server answers `/library/all?guid=plex://show/<hash>` with the title.

## Decision

- New video tags hold the watch link exactly as Plezy reads it, with `%2F` written as `/`. The blueprint opens it with `media_player.play_media`, `media_content_type: url`. No `?play`, no Select press (the "Open in Plezy?" prompt only appears on first use), no HA-side shuffle.
- The write script takes a watch link or a title. With a link it validates the format, rejects episodes and seasons, appends `&mode=shuffle` when asked and looks the title up by GUID for the tag's label. With a title it searches Plex and builds the portable link from the GUID.
- Phone scanning goes through an iPhone shortcut (Scan QR code, choose mode, POST JSON) to a local-only HA webhook automation that runs the write script. The phone never writes the tag; the TagTuner does. iOS Camera can't open `plezy://` on a phone without Plezy, and HA's companion app has no QR scanner event.
- `infuse://` tags keep playing as before.

## Consequences

- Tags depend on the watch-link query format, which can still change before the upstream PR merges.
- The 45 s playing timeout covers Plezy's cold start wait for Plex servers (up to about 20 s).
- A label and a shuffle link nearly fill an NTAG213 (144 bytes), so the label is cut to 20 characters and the playlist name record is left empty.
