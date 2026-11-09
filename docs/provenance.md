# Kilix Cap runtime boundary and provenance

What Kilix Cap does and does not do at runtime, and where its generated art came from. Moved from the [README](../README.md); the text is unchanged.

## Runtime boundary and provenance

Kilix Cap itself makes no runtime network request, downloads nothing, and
never regenerates committed art. A separately launched desktop application or
selected Kilix 95 game may access the network or persist data according to its
own behavior.

The mansion plates and seven room-specific door editions were created with
the OpenAI built-in image generation tool, then center-fitted offline to
480×256 P6 RGB with `tools/prepare_visual.py`. Every door edition edits its
own room source, so its perspective, trim, materials, illumination, and
shadows belong to that scene. Their complete prompts, dimensions, roles,
generator-output hashes, lossless public-snapshot records, and earlier
room/sprite lineage are in
[docs/visual-provenance.json](visual-provenance.json). The clean-room
asset boundary and explicit asset-license scope are recorded in
[COPYING-ASSETS.md](../COPYING-ASSETS.md).

Detailed behavior is in [docs/INTERACTIONS.md](INTERACTIONS.md), app and
TUI launch vectors in [docs/APPS.md](APPS.md), and engine invariants in
[docs/ENGINE.md](ENGINE.md).
