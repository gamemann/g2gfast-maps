# g2gfast-maps

The imported maps game-g2gfast plays, as their own repository. Read `../../CLAUDE.md` and `../game-g2gfast/CLAUDE.md` first.

**Each map is its own signed pack, `gamemann/<map id>`, never one pack for all of them.** A player joining a server downloads the map that is running; the game fetches the rest one at a time on a map change (`G2GConfig.map_content_owner`, `sv_map_content_owner` in the game's own `game.yml`). One pack for 570 MB of maps would put every map on every join.

**`maps/` is where the game loads them from.** `tools/g2gmaps link` makes `game-g2gfast/maps/imported` a relative link here, which is the path the `.import` markers were written against, so the game imports, tests and publishes these files without a copy. `link` refuses to replace a real directory that differs from `maps/`, because that directory would be the only copy of the difference.

**Publishing goes through the game's `tools/publish_maps.sh`**, not a second publisher: the pack has to carry `.godot/imported/` and the rewritten markers, and only a Godot project that has imported the maps can produce those. `dist/` is the signed output (781 MB for 571 MB of maps, the difference being the imported textures) and is gitignored.

**`fetch` mirrors packs verbatim; it does not rebuild `maps/`.** A published pack's markers point at its mount prefix, not at `res://maps/imported/`, so unpacking one into `maps/` would break the game's import. A mirror is a content origin (`<out>/gamemann/<id>/manifest.json` + `objects/`) a server can list in `content_urls`. Every object is hash-checked; `--pub` checks the manifest's signature over the DECODED payload bytes, which is what dot-cloud signs.

**Credits are mandatory.** `manifest` copies each map's author and source page from the game's `maps/zones/<id>.json` and refuses to finish if any map has none. The game shows the author on screen at a map change (`[credit-1]`).

Plain git, no LFS: the largest file is 62 MB (`surf_summit.bin`), under GitHub's 100 MB limit, and LFS bandwidth quotas would make every clone of a public repository a bill.
