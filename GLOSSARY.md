# Glossary

**Music tag**: a tag whose URI the blueprint sends to Music Assistant, Sonos or a plain HA media player (`spotify://...`, `plex://...`, `https://...`, etc.). Note that `plex://` means Plex *music* through Music Assistant.

**Video tag**: a tag holding an Infuse deep link: `infuse://movie/<tmdb>`, `infuse://series/<tmdb>` or `infuse://series/<tmdb>-<season>-<episode>`. The blueprint opens it on the Apple TV with `?play` appended. Items are identified by their TMDB id and must be in the Infuse library.

**Show tag**: a video tag naming a whole series (`infuse://series/<tmdb>`). Infuse plays the next unwatched episode, the same as its Play button.

**Current tag**: the URI in the reader's "Playlist URI" text entity, which the firmware sets on every scan and keeps after the tag is lifted. The blueprint routes controls by it: while the current tag is a video tag, click, knob and volume drive the Apple TV.

**Open in Infuse prompt**: the tvOS confirmation shown when a deep link opens Infuse. The blueprint accepts it by pressing Select on the Apple TV remote.
