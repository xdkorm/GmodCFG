# GmodCFG

Personal Garry's Mod config, restructured for readability.

## Install

Copy everything into `garrysmod/cfg/` — `autoexec.cfg`, `valve.rc`, the
`skill*.cfg` files, `mount.cfg`, `mapcycle.txt`, etc. must stay at the
root of `cfg/` because the engine/server expects them there by name.
Everything under `cfg/` and `Addons/` is safe to move around freely
since it's only ever reached through explicit `exec` calls.

## Structure

```
GmodCFG/
├── autoexec.cfg         # entry point — load order + aliases + binds ONLY
├── valve.rc, mount.cfg, mountdepots.txt, mapcycle.txt,     ┐
├── skill.cfg, skill1-3.cfg, skill_manifest.cfg,            │ engine/server files,
├── game.cfg, server.cfg, listenserver.cfg,                 │ expected at cfg root,
├── settings_default.scr, addonnomount.txt,                 │ don't edit paths here
├── server_blacklist.txt, config.cfg, config_default.cfg,   │
├── client.vdf, server.vdf                                  ┘
├── Addons/               # your existing structure, untouched
│   ├── AddonsConfigApplier.cfg
│   └── Config/            # fltl.cfg, mtrcp.cfg, tnts.cfg
└── cfg/                   # everything below is safe to reorganize
    ├── core/               # AutoSettings
    ├── graphics/           # BadGraphics / ExcellentGraphics
    ├── viewmodel/          # VMOn / VMOff  (NEW, see below)
    ├── hud/                # HUDOn / HUDOff
    ├── movement/           # mousewheel bhop, mousewheel reload
    ├── controller/         # 360 controller on/off (win + linux)
    ├── display/            # 1080 / 1440 resolution presets
    └── misc/               # guide.cfg (the in-game cheatsheet)
```

## What changed from the old layout

- **Viewmodel is now its own module**, `cfg/viewmodel/VMOn.cfg` and
  `VMOff.cfg`, exec'd from `autoexec.cfg` — same pattern as TF2CFG. It
  didn't exist as a separate thing before; there was nothing controlling
  viewmodel visibility independently of graphics at all.
- **`BadGraphics.cfg` and `ExcellentGraphics.cfg` now carry a header
  comment** saying viewmodel settings don't belong there. Neither file
  actually had viewmodel cvars in it, so nothing was removed — this
  formalizes the rule so a future edit doesn't quietly couple the two
  again.
- **`network.cfg` was deleted.** It only set `cl_cmdrate 66` and
  `cl_updaterate 66`, both of which `AutoSettings.cfg` already sets to
  the same values — and its own top comment was literally
  `// TODO: Why does this file even exist?`. It wasn't `exec`'d from
  anywhere, so removing it changes nothing at runtime.
- **`autoexec.cfg` is now grouped into load order / aliases / binds**
  instead of one flat list, and every `exec` call for a moved file was
  updated to its new `cfg/<module>/` path.
- **Added quick-access aliases** (`Res1080`, `Res1440`, `Controller360On`,
  `Controller360Off`, `ReloadOnWheel`) so moving those files into
  subfolders doesn't cost you the ability to type a short command in
  console — `guide.cfg` was updated to reflect them.
- `Addons/` is untouched — that's the structure you already started, and
  it was clean, so `cfg/` follows the same spirit rather than replacing it.

## Notes

- `ExcellentGraphics` is the active preset in `autoexec.cfg`;
  `BadGraphics` is commented out — swap the comment to flip.
- `VMToggle` exists but isn't bound to a key by default (`v` is already
  `noclip` in this config) — bind it to whatever you like.
