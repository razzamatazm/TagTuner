# 1. Plex video tags live in the main blueprint

Date: 2026-10-07

## Status

Superseded in part by [ADR 2](0002-infuse-deep-links-for-video-tags.md): Plex playback replaced by Infuse deep links.

## Context

We want tags that start Plex movies and shows on an Apple TV, next to the existing music tags. The firmware only emits events and the XIAO flash is nearly full, so the logic has to live in a Home Assistant blueprint. The music automation reacts to every tag removal, click and knob turn regardless of tag content, so a second automation on the same reader would fight it.

Research (HA `dev` source and docs, 2026-10-07): HA's Plex integration can play a title on a Plex client entity from a JSON search (`library_name`, `title`, `show_name`, ...), but the Apple TV's Plex client only exists while the Plex app is running. The Apple TV integration can wake the device and launch Plex, but can't play Plex library items itself. Netflix stopped honouring tvOS deep links in September 2025.

## Decision

- Add an optional "Plex videos on Apple TV" section to `TagTuner4HAss.yaml` instead of a separate blueprint. Empty inputs mean no change for music-only users.
- Video tags use `plexvideo://{json}`. The prefix can't collide with Music Assistant providers (`plex://` is already Plex music). JSON only, no rating keys: readable on the tag, survives items being re-added, and lets the blueprint tell shows from movies.
- Show tags default to the next unwatched episode with `continuous`, falling back to shuffle when nothing is unwatched. `"shuffle":true` on the tag means random. Movies and episodes resume by default.
- Starting a video wakes the Apple TV, launches Plex, presses Plex's scan clients button and waits for the Plex entity, then plays. A failure posts an HA notification.
- The current tag (reader's Playlist URI text entity) decides where controls go. Video controls go through the Apple TV entity and remote (play/pause, `skip_forward`/`skip_backward`, volume steps) so they don't depend on the Plex entity's limited volume support.
- One source at a time: a video tag pauses music, a music tag pauses Plex on the Apple TV. Lifting a video tag does nothing.
- Netflix is out for now.

## Consequences

- The fork diverges further from upstream's blueprint.
- The in-order show search (`unwatched`, `sort: originallyAvailableAt:asc`, `maxresults: 1`) is built from documented plexapi filters but untested; air-date order can differ from episode order for specials.
- Skip step size is whatever the Plex app does for `skip_forward`/`skip_backward`. The volume limiter doesn't apply to the Apple TV.
- A cold start can take up to the startup timeout (60s default), and other reader events queue behind it.
