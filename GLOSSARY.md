# Glossary

**Music tag**: a tag whose URI the blueprint sends to Music Assistant, Sonos or a plain HA media player (`spotify://...`, `plex://...`, `https://...`, etc.). Note that `plex://` means Plex *music* through Music Assistant.

**Video tag**: a tag holding a Plezy watch link (`plezy://watch?plex=show/<hash>`, `plezy://watch?plex=movie/<hash>`, or the server form `plezy://watch?id=<ratingKey>&server=<machineIdentifier>`) or, for older tags, an Infuse deep link (`infuse://...`). The blueprint opens it on the Apple TV.

**Watch link**: Plezy's `plezy://watch` URL for a Plex show or movie. Plezy shows it as a QR code under "Show watch link". The `plex=` value is the item's Plex GUID without `plex://`; `%2F` and `/` are the same.

**Show tag**: a video tag naming a whole show. A Plezy show tag plays the On Deck episode (`mode=next`, the default) or shuffles the show (`&mode=shuffle`).

**Current tag**: the URI in the reader's "Playlist URI" text entity, which the firmware sets on every scan and keeps after the tag is lifted. The blueprint routes controls by it: while the current tag is a video tag, click, knob and volume drive the Apple TV.

**Write webhook**: the HA webhook the iPhone shortcut posts a scanned watch link to. It runs the write script, which writes the tag on the reader.

**Open in Infuse prompt**: the tvOS confirmation shown when a deep link opens Infuse. The blueprint accepts it by pressing Select on the Apple TV remote.
