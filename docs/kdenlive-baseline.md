# Kdenlive baseline

The unchanged Kdenlive build that roadmap step 2 qualifies ([roadmap](roadmap.md#2-qualify-the-selected-editor-foundation)).
Nothing here is modified from upstream. Restyling and the harness bridge come after it is qualified.

## Windows build

Built on 2026-11-13 with KDE Craft. Kdenlive is compiled from source; its dependencies come from KDE's binary cache.

### Pins

| Item | Pin |
| --- | --- |
| Kdenlive | 26.08.3, `kdenlive-26.08.3.tar.xz` from download.kde.org, SHA-256 `4bfa3dc856ecabd1f1b3283741591350e33fd0141f878bcf6331450ee4adcaee` (matches KDE's published hash). Upstream tag `v26.08.3` is commit `73f290d`. |
| Craft | `1ef5a8fd0c8a53c3afd062a4d229a92e161b93f9` |
| Craft blueprints | `06e9517a3739b92159f7c4f3b9b76c0daf2de54a` |
| Binary cache | `https://files.kde.org/craft/Qt6/26.10/`, `windows-cl-msvc2022-x86_64`, `RelWithDebInfo` |
| Compiler | Visual Studio 2022 Build Tools 17.14, MSVC 19.44.35228 (toolset 14.44.35207), Windows SDK 10.0.26100.0 |
| Python | 3.13.7 |

Main dependencies as installed:

| Package | Version |
| --- | --- |
| Qt | 6.12.0 |
| KDE Frameworks | 6.31.0 |
| MLT | `HEAD-7a3319fc`, a snapshot, not a release |
| FFmpeg | 8.1.1 |
| OpenTimelineIO | 0.18.1 |
| KDDockWidgets | 2.4.1 |
| frei0r plugins | `HEAD-618dde7` |

Gap: Craft's blueprint pin is recorded, not enforced. A later `craft` run can update the blueprints and the cache.
MLT and frei0r are snapshots chosen by the blueprints, not tagged releases.

### Setup commands

Run from PowerShell, not Git Bash.
From Git Bash, Craft found Git under `Git\mingw64` and failed to locate `patch.exe`.
`CraftBootstrap.py` is fetched from the pinned Craft commit's `setup/` folder.

```powershell
python -I CraftBootstrap.py --prefix F:\CraftRoot --branch 1ef5a8fd0c8a53c3afd062a4d229a92e161b93f9
```

Answers: MSVC 2022, short path root `F:\_`, no Start menu entry, coloured logs.

```powershell
. F:\CraftRoot\craft\craftenv.ps1
craft --set version=26.08.3 kdenlive
craft --install-deps kdenlive
craft --no-cache kdenlive
```

### Measured on this machine

AMD Ryzen 7 9800X3D (8 cores), 31 GB RAM, Windows 11 Pro 26200. One run each, so these are single samples.

| Step | Result |
| --- | --- |
| Install dependencies from the cache | Passed, 8.5 minutes, network bound |
| Compile Kdenlive (816 build steps) | Passed, 4.2 minutes, 225 compiler warnings |
| Launch to the Quick Setup window | 1.3 seconds, 115 MB working set |
| Quick Setup to the main editor window | 2.6 seconds, 307 MB working set |
| Upstream tests (`ctest` in the build folder) | 36 of 36 passed in 17.3 seconds: 116 test cases, 16,465 assertions |

The main window showed the project bin, clip and project monitors, timeline with two video and two audio tracks, and the audio mixer.
Startup and every test logged one warning: `mlt_repository_init: failed to register F:\CraftRoot\lib\mlt/mltopenfx.dll`. OpenFX effects may be missing; not yet investigated.
The launch was checked through Windows UI Automation and window captures, not a full native UI test.
The Quick Setup kept its defaults (dark theme, 1920x1080 at 25 fps, 2 video and 2 audio tracks) and saved them to the user's Kdenlive settings.

Not yet done: editing, preview, save and reopen, and export against the [first workflow](first-workflow.md).

## macOS build

Blocked: no Mac is available to this project yet.
