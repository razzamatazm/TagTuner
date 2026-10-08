# 2. Infuse deep links for video tags

Date: 2026-10-08

## Status

Accepted. Supersedes the playback path in [ADR 1](0001-plex-video-tags-in-main-blueprint.md).

## Context

Plex's redesigned Apple TV app (rolling out from 2026-09-17) dropped remote control and "Advertise as Player" (Plex staff on forums.plex.tv). Home Assistant's Plex integration can no longer start playback on the Apple TV, so `plexvideo://` tags stopped working.

Infuse Pro on the Apple TV is connected to the same Plex server. Infuse documents deep links that work on tvOS (support.firecore.com article 215090997): `infuse://movie/{tmdb}`, `infuse://series/{tmdb}`, `infuse://series/{tmdb}-{season}-{episode}`, with `?play` to start playback. Items played from the Infuse library sync resume and watched status back to Plex. HA's Apple TV integration opens such links with `media_player.play_media` and `media_content_type: url`.

Tested on the user's Apple TV on 2026-10-08: `infuse://series/82728?play` (Bluey) started playing straight away after a tvOS "Open in Infuse" prompt, and only worked with the Apple TV awake.

## Decision

- Video tags hold the Infuse link itself (`infuse://...`). The `plexvideo://` JSON format is dropped.
- Starting a video tag: pause music, wake the Apple TV if it's off (CEC turns on the TV), open the link with `?play`, press Select after a short delay to accept the prompt (configurable), then wait for the Apple TV to report playing and post an HA notification if it doesn't.
- Whole-show tags rely on Infuse's own next-unwatched behaviour.
- Everything else from ADR 1 stands: one blueprint, current tag routes controls through the Apple TV entity and remote, lifting a video tag does nothing, one source at a time.

## Consequences

- No Plex client entity or Plex integration is needed for playback.
- Random episodes are not possible through Infuse links. Doing it would need HA to read the episode list from the Plex server (e.g. a `rest_command`) and open a specific episode link.
- Tags need a TMDB id, which has to be looked up on themoviedb.org.
- Whether Infuse honours `skip_forward`/`skip_backward` from the remote is untested.
