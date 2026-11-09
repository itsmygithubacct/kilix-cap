# Verifying Kilix Cap

The test targets, headless self-tests and render checks. Moved from the [README](../README.md); the text is unchanged.

## Verify

~~~sh
make test

./bin/kilix-cap --interaction-test
./bin/kilix-cap --launcher-test
./bin/kilix-cap --game-catalog-test
./bin/kilix-cap --visual-test
./bin/kilix-cap --audio-test
python3 tools/mansion_tui.py selftest
python3 tools/validate_visual.py
python3 tests/pty_smoke.py --binary ./bin/kilix-cap

render_dir=$(mktemp -d)
./bin/kilix-cap --render-test "$render_dir"
python3 tools/check_render.py "$render_dir"

make sanitize
~~~

The release render set contains all eight base rooms plus direct-object hover
and Storeroom movement states. The visual validator checks the exact
33-file art inventory, dimensions, hashes, generation prompts, eight opaque
RGB plates, game-media alpha, and all thirteen semantic Study hit IDs.
