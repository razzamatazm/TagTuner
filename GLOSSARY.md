# Glossary

**Music tag**: a tag whose URI the blueprint sends to Music Assistant, Sonos or a plain HA media player (`spotify://...`, `plex://...`, `https://...`, etc.). Note that `plex://` means Plex *music* through Music Assistant.

**Video tag**: a tag whose URI starts with `plexvideo://` followed by a JSON Plex search, e.g. `plexvideo://{"library_name":"TV Shows","show_name":"Bluey"}`. Plays on the Apple TV's Plex app. Avoid calling these "Plex tags", since `plex://` music tags exist too.

**Show tag**: a video tag naming a show without a season or episode. Plays **in order** by default (next unwatched episode, then continues), or **random** when the JSON has `"shuffle":true`.

**Current tag**: the URI in the reader's "Playlist URI" text entity, which the firmware sets on every scan and keeps after the tag is lifted. The blueprint routes controls by it: while the current tag is a video tag, click, knob and volume drive the Apple TV.

**Cold start**: starting a video tag when the Apple TV is asleep or Plex is closed. The blueprint wakes the Apple TV, opens Plex, rescans Plex clients and waits for the Plex player entity before playing.
