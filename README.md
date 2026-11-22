<h1 align="center">
  <a href="https://github.com/Ahmet-Dedeler/awesome-gta6-reverse-engineering">
    <img src="assets/banner.png" alt="Awesome GTA VI Reverse Engineering" width="100%">
  </a>
</h1>

<p align="center">
  A curated index of open-source <b>Grand Theft Auto VI</b> reverse engineering tools:<br>
  RAGE 9 / RPF9 archive explorers, TTPF texture and ADAT/AWC audio decoders,<br>
  byte-level format specs, and Xbox Series (Scarlett) / PS5 translation layers.
</p>

<p align="center">
  <a href="https://awesome.re"><img src="https://awesome.re/badge-flat2.svg" alt="Awesome"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-CC0_1.0-1a1b2e?style=flat-square" alt="License: CC0-1.0"></a>
  <a href="CONTRIBUTING.md"><img src="https://img.shields.io/badge/PRs-welcome-ff5c8a?style=flat-square" alt="PRs welcome"></a>
  <a href="https://github.com/Ahmet-Dedeler/awesome-gta6-reverse-engineering/commits/main"><img src="https://img.shields.io/github/last-commit/Ahmet-Dedeler/awesome-gta6-reverse-engineering?style=flat-square&color=ff8a3d&label=updated" alt="Last commit"></a>
</p>

<p align="center">
  <a href="https://discord.gg/reADpA6Cd"><img src="https://img.shields.io/badge/Join%20the%20Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Join the Discord" height="36" /></a>
  <br />
  <sub>Questions, previews, bug reports and format research. Come say hi.</sub>
</p>

<p align="center">
  <a href="#ecosystem-overview">Map</a> ·
  <a href="#rpf9-archive--asset-explorers">Archive Explorers</a> ·
  <a href="#texture-ttpf--audio-adat--awc-tools">Textures &amp; Audio</a> ·
  <a href="#console-translation-layers--static-recompilation">Recomp &amp; Shims</a> ·
  <a href="#format-documentation--research">Format Docs</a> ·
  <a href="#format--architecture-glossary">Glossary</a>
</p>

<br>

> [!IMPORTANT]
> **Clean-room research and open-source code only.** This list indexes reverse engineering tools, clean-room reimplementations, and format documentation. It does **not** host, link to, or endorse game executables, copyrighted assets, encryption keys, DRM bypass tools, or leaked build downloads. Every tool listed expects you to supply files from your own copy on disk.

> [!CAUTION]
> **There is no playable GTA VI for PC.** The circulating Xbox build has no game executable (`game_scarlett_master.exe` is missing), and no open-source project boots the game. Any "GTA 6 PC download", "Switch/PS5 emulator", installer, or prebuilt launcher `.exe` is fake at best and malware at worst. One public "launcher" template even ships a self-signed `CN=Rockstar Games, Inc.` certificate with its private key, so anything signed with it shows Rockstar's name in the file's signature details (Windows still marks it untrusted). Expect a wave of fake "leaked executable" downloads in the coming days; a scam kit built around a fake installer is reportedly being sold. Build tools from source you have read.

---

## Contents

- [Ecosystem Overview](#ecosystem-overview)
- [RPF9 Archive & Asset Explorers](#rpf9-archive--asset-explorers)
- [Texture (`.ttpf`) & Audio (`ADAT` / AWC) Tools](#texture-ttpf--audio-adat--awc-tools)
- [Console Translation Layers & Static Recompilation](#console-translation-layers--static-recompilation)
- [Format Documentation & Research](#format-documentation--research)
- [Visualizers & Related Experiments](#visualizers--related-experiments)
- [Format & Architecture Glossary](#format--architecture-glossary)
- [Inclusion Criteria & Exclusions](#inclusion-criteria--exclusions)
- [Contributing](#contributing)

---

## Ecosystem Overview

Everything starts with an `RPF9` archive. Explorers open it, then each payload type has its own decoders. Most active work targets the Xbox Series X|S (Scarlett) RAGE 9 stack, since Xbox Series runs on x86-64 Zen 2 with RDNA2 (GFX10.3) shaders and a Windows-derived Game OS (`d3d12_xs`, `kernelx`). Early PS5 library-translation efforts exist as well.

<p align="center">
  <img src="assets/ecosystem-map.png" alt="Ecosystem map: RPF9 archive explorers feed six payload lanes (textures, audio, video, models and maps, shaders, executable), each with the projects that handle it and what they output" width="100%">
</p>

---

## RPF9 Archive & Asset Explorers

Tools and libraries for inspecting, browsing, salvaging, and exporting `RPF9` containers, 3D drawables (`glTF` / `OBJ`), map placements, and embedded media.

| Project | Stack | Platform | Focus |
| :--- | :--- | :--- | :--- |
| [rpatty-dev/PercyStar](https://github.com/rpatty-dev/PercyStar) | Rust | Windows | High-speed RPF9 archive explorer with D3D11 Bink2 video playback and ADAT Opus audio decoding. Now adds a World Map tab (heightmap, NavMesh, textured city blocks), a streaming 3D GPU world view, an animation / PSO browser, an AWC player, and vehicle configurations with swappable modular parts. |
| [Ahmet-Dedeler/Percy6Stars](https://github.com/Ahmet-Dedeler/Percy6Stars) | Rust | macOS, Windows | Cross-platform fork of PercyStar adding macOS support, in-app BK2 video, AWC audio previews with real macOS playback, 3D model browsing, and a thumbnail gallery for `.ttpf` texture packs. Tracks upstream PercyStar features. |
| [holdmysocks/SixNG](https://github.com/holdmysocks/SixNG) | Rust | Cross-platform (CLI + GUI) | Asset extractor for GTA VI Xbox Series `RPF9` archives. Exports models to `glTF`, textures, and map data, streams the whole map in a 3D view with BC textures, resolves LOD links and shader/parameter names, and decodes PSO files and vehicle tables (`GPL-3.0`). |
| [meetox80/6stars](https://github.com/meetox80/6stars) | Swift, Metal | macOS | Native macOS engine that streams the world straight from your own RPF9 archives: the map with its full LOD hierarchy, interiors, vegetation, textures, timecycle lighting and weather, and a physically based sky and ocean. Ships an `rpftool` CLI too. Written from scratch; no license file yet. |
| [breadbbbb/rpf9-explorer](https://github.com/breadbbbb/rpf9-explorer) | C# | Windows | RPF9 archive browser with model, texture, audio, and video previews, asset export, and saved replacement projects. v1.2 recovers more models and adds a clean-room BCPack decoder for BC1, BC3, BC4, and BC5 (BC6H/BC7 not yet). Used as a submodule by `Leonida`. |
| [offs/rpf9-extract](https://github.com/offs/rpf9-extract) | Rust | Cross-platform (CLI) | Clean-room CLI extractor and inspector (`-n` list, `-v` header fields and PSIN tables). |
| [nullbrik/RageForge](https://github.com/nullbrik/RageForge) | C# | Windows (GUI + CLI) | RPF9 archive inspector (entry counts, types, verification hashes) with a live `.ttpf` texture decoder (Gfx10 detiling + BC1-BC7), texture dictionary detection, a 3D model viewer, and (since 0.4) a "Load world" view that streams the whole map with terrain, water, and time of day. |
| [zeozeozeo/rpf9](https://github.com/zeozeozeo/rpf9) | Rust | Library crate | Minimal clean-room Rust library implementing the `RPF9` archive format with runnable `examples/`. |

---

## Texture (`.ttpf`) & Audio (`ADAT` / AWC) Tools

Focused decoders for RAGE 9 GPU-tiled texture packs (`.ttpf`, `BCPack`, `Gfx10` swizzling) and audio containers (`ADAT` / `AWC` Opus and `XMA2` banks).

| Project | Stack | Platform | Focus |
| :--- | :--- | :--- | :--- |
| [woreksami/ttpf-studio](https://github.com/woreksami/ttpf-studio) | Python | Cross-platform (GUI) | View, export, and replace `.ttpf` textures (BC1-BC7, RGBA8/BGRA8/RGBA16F, GPU unswizzling, headerless scan recovery), recover RPF9 files, export models to OBJ, and play Bink video. |
| [paryzius/ttpf-viewer](https://github.com/paryzius/ttpf-viewer) | Rust (`MIT`) | Windows (`.exe`), macOS / Linux from source | Fast `.ttpf` gallery and extractor. Decodes BC1-BC7, BC6H HDR, and uncompressed formats with exact Gfx10 tile/mip layouts, shows cube maps in 3D and texture arrays per slice, exports PNG, lossless DDS (mips kept), or 360° panoramas, and searches or batch-extracts whole folders. Includes a CLI and a documented `RDS5`/`ETPF` header spec. BCPack-only mips are not decodable yet. |
| [Ahmet-Dedeler/bcpack-rs](https://github.com/Ahmet-Dedeler/bcpack-rs) | Rust, Python (`MIT OR Apache-2.0`) | Cross-platform (CLI) | Standalone clean-room decoder for `BCPack`, the Xbox Series texture compression (`Leonida` and `rpf9-explorer` now ship their own decoders too). **BC1 decodes** with a Rust CLI (`bcpack ttpf-export <store.ttpf> <texture> out.png\|dds`). It is checked on held-out data: tables fitted on half the texture pools decode the other half at 34.7 / 34.0 dB median vs a 37.9 dB plain-BC1 ceiling, and 600/600 random textures decode. The entropy layer also converges exactly for BC7, BC6H, BC5, BC4 and BC3. BC7's mode layer is solved too; its block decode and the other formats' block mappings are in progress. |
| [jaylikesbunda/rpf9-audio](https://github.com/jaylikesbunda/rpf9-audio) | Python | Cross-platform (CLI) | Dependency-free, read-only RPF9 and `ADAT`/AWC audio research toolkit. Extracts Opus packets, decodes to WAV, and remuxes playable Ogg Opus. |

---

## Console Translation Layers & Static Recompilation

Projects translating Xbox Series X|S (`Scarlett`) or PS5 system libraries, memory models, and RDNA2 shaders so console binaries can run natively on PC. Note that GTA VI itself does **not** boot in any open-source project yet.

| Project | Stack | Target | Focus & Current Status |
| :--- | :--- | :--- | :--- |
| [ItsNotRexed/Leonida](https://github.com/ItsNotRexed/Leonida) | Python, C# | Xbox Series → Windows (Vulkan) | The scene's largest static recompilation effort, for build `1.0.2.1`. Translates all 26,503 RDNA2 shaders to SPIR-V passing `spirv-val`, with hand-written GPU tests run on 30+ GPU and driver combinations (small test shaders, not the game's own). Exports OBJ models, DDS textures (plus a clean-room BCPack decoder in `tools/bcpack.py`), and AWC audio. The PE loader runs a stand-in executable end to end, and the Xbox D3D12 → Vulkan device is in progress. Does not boot. |
| [R0Y00/Xbox2Windows](https://github.com/R0Y00/Xbox2Windows) | C++, Python (`MIT`) | Xbox Series → Windows | GTA VI-focused static recompilation research. Translates selected x64 code to C++ and gfx1013 shaders (now including linked mesh + pixel stages) to desktop DXIL, runs recompiled FidelityFX code, samples BC7 textures, and collects wave64 BVH results from community GPUs. Source-built programs run from entry to exit; the game's own CPU code is unverified because the executable is missing. Tests build their own fixtures, so no game files are needed. |
| [ivall/xcl](https://github.com/ivall/xcl) | Rust (`GPL-2.0`) | Xbox Series X\|S → Windows (D3D12) | Wine-style compatibility layer: game x64 code runs natively while XCL reimplements `kernelx`, XGameRuntime, `d3d12_xs`, DirectStorage, XAudio2 (XMA decoded in pure Rust), and GameInput. Recompiles RDNA2 shaders → HLSL → DXIL, and all 24,064 programs in a RAGE 9 `shaders.rpf` translate. The game's own console DLLs (Bink 2, BVH builder, FSR 4) run on PC, and GFX10 detiling matches 909/912 retail textures byte for byte. XDefiant, Fortnite, and Minecraft reach their title screens or menus; GTA VI is not run because the dump has no executable. 150 tests; no game data in the repo. |
| [omonkeyz/xbox-emulator](https://github.com/omonkeyz/xbox-emulator) | Rust | Xbox Series X\|S → Windows | Active continuation of `TomBox` (`TomBox + rage9`), relicensed to `GPL-3.0-or-later`. Reimplements `d3d12_xs`, RDNA2 shader translation, unified memory aliasing, and console OS DLLs, with automated per-function progress maps. Fortnite reaches its lobby; GTA VI is not run because the dump has no executable. |
| [drycheetah84/TomBox](https://github.com/drycheetah84/TomBox) | Rust | Xbox Series X\|S → Windows | Upstream Xbox Series X\|S compatibility runtime that `omonkeyz/xbox-emulator` builds upon. One title reaches its main menu; not playable. |
| [realdody/americas-recomp (`xbox-series`)](https://github.com/realdody/americas-recomp/tree/xbox-series) | C++ | Xbox Series → PC | Tests whether AnyPS5's shader recompiler can run Xbox Series shaders. Valid SPIR-V so far: pixel 99.97% (strict), compute 99.6%, mesh 99.5%, geometry 100%, vertex 64.7% (the rest need the real vertex fetch shader), with GPU-vs-CPU bit-exact checks and `d3d12_xs` / `xmem` / `xgameruntime` stand-ins. Check the `xbox-series` branch (`main` only holds `LICENSE`). |
| [californiaAlmonds/OpenScarlett](https://github.com/californiaAlmonds/OpenScarlett) | C++20 (`MIT`) | Xbox Series X\|S → Windows | Clean-room Scarlett translation layer with PE32+ APC hook loader, `d3d12_xs` COM proxy, `xg_xs` swizzling, `kernelx` memory aliasing, RDNA2 → SPIR-V/DXIL lifter, GDeflate/BCPack/XMA2/Bink2 codecs, and GDK shims. `v0.5.0-alpha` passes 51/51 automated tests and runs official Microsoft Xbox GDK samples unmodified; no commercial title yet. |
| [Freminet/ScarlettX](https://github.com/Freminet/ScarlettX) | C++ | Xbox Series X\|S → Windows | Clean-room high-level emulator for GDK titles: PE64 loader, DirectStorage/XVD (unencrypted only), `d3d12_xs` → D3D12, GameInput, XGameSave, XAudio2. *Concept Destruction* and *Unicorn Overlord* are playable on a hardware GPU. No license file yet. |
| [sammwyy/xvalkyrie](https://github.com/sammwyy/xvalkyrie) | C++20 (`GPL-3.0`) | Xbox One + Series X\|S → Windows | Wine-style translation layer for both console generations: maps the title's executable, runs its code natively, and answers `d3d12_xs` / `d3d11_x`, `kernelx`, and `xgameruntime` imports itself, with a generated per-function status page. Xbox One ERA titles reach menus and scenes. On Series, 37 of 40 public GDK samples draw, and retail titles start and present frames, but the picture is not right yet (1,786 of the 2,055 shaders one title creates translate). Releases are built by CI from the tagged source. |
| [brcly/TuxBox](https://github.com/brcly/TuxBox) | Rust, Python | Xbox Series X\|S → Linux (Proton); Windows in progress | Native-first layer for unpacked, decrypted Series games, run on Linux under stock Proton Experimental. *The Matrix Awakens* runs from its menu through the highway chase into the open city (some surfaces still missing), with its Nanite shaders translated. Publishes per-library export and shader coverage. No license file yet. |
| [jadeowaowa/scarlett-layer](https://github.com/jadeowaowa/scarlett-layer) | C++ (`Apache-2.0`) | Xbox Series X\|S → Windows | Loader + hook DLL that rewrites imports of unpacked, unencrypted GDK executables and routes them to `d3d12_xs`, DirectStorage, `xmem` / `xg_xs`, `xgameruntime`, and audio shims. 74 automated checks; no commercial title runs end to end yet. Rejects encrypted packages by design. |
| [JamesCicada/GTAVI-Recomp](https://github.com/JamesCicada/GTAVI-Recomp) | C++ | PS5 → PC | GTA VI layer on top of AnyPS5: research, patches, and library work sent upstream. Refuses to work from game files, keys, or leaked material, so game-specific research is blocked until files can be studied lawfully; nothing has been tested with GTA VI. |
| [boykopovar/AnyPS5](https://github.com/boykopovar/AnyPS5) | C++ (`GPL-2.0`) | PS5 → Windows / Linux | General relinker and PS5 system-library reimplementation (no emulation) with an RDNA2 → SPIR-V shader recompiler. Not GTA-specific, but it is the upstream that `americas-recomp` and `GTAVI-Recomp` build on. |

---

## Format Documentation & Research

Byte-level specifications, field confidence tables, and reproducible inspection scripts reverse engineered from local file headers and decoded outputs.

| Project | Focus | Notes |
| :--- | :--- | :--- |
| [TrinitronClient/GTA6-Asset-Info](https://github.com/TrinitronClient/GTA6-Asset-Info) | 17-part asset format spec, `structure/` byte references + SixNG guide | Maintained by the SixNG team. Covers `RPF9`, resources/pages, texture dictionaries, Xbox tiling, `ttpf` (incl. array/cubemap layouts), `BCPack` codec types, models, map meta, placements, LOD parents, interiors, vehicle paint and tables, materials, shader names, collision, navmesh, lights, road graphs, PSO schema, RBF/BVH4, and `AWC`/`ADAT` audio. |
| [oguzhanozfe/gta-vi-port-research](https://github.com/oguzhanozfe/gta-vi-port-research) | Xbox-to-PC research handbook | Markdown format documentation and reproducible Python file-inspection scripts in `tools/`. |
| [frenzy42069/RPF9-tools](https://github.com/frenzy42069/RPF9-tools) | Byte-level `format_desc.md` + C/Python scripts | Detailed `RPF9` container geometry, PSIN checks, Bink2 range carving, and `ADAT` Opus extraction (`rpf9_extract_audio.c`). Ignore the satirical disclaimer in the README. |
| [Ahmet-Dedeler/rage9-re](https://github.com/Ahmet-Dedeler/rage9-re) | `script.rpf` PSO script data and `anim.rpf` crAnimation channels | Rust + Python decoder with glTF 2.0 export, byte-level notes in `docs/`, tests on synthetic data only. |

---

## Visualizers & Related Experiments

Standalone graphics and media tools built alongside GTA VI / RPF9 format exploration.

- [Ahmet-Dedeler/weather-study](https://github.com/Ahmet-Dedeler/weather-study) (`TypeScript / WebGL2`) - Open-source WebGL2 weather engine and timecycle visualizer with volumetric clouds, ocean reflections, branching lightning, and 4K capture, inspired by RPF9 research.
- [Ahmet-Dedeler/bink-player-mac](https://github.com/Ahmet-Dedeler/bink-player-mac) (`Objective-C / macOS`) - Native macOS Bink2 (`.bk2`) video player built alongside PercyStar and RPF9 exploration.


**See also:** [PortWatch VI](https://github.com/4lwayz/portwatch-vi), a daily tracker of the PC port efforts that also flags clone networks and re-uploads of tracked projects.

---

## Format & Architecture Glossary

Quick reference for terms that show up across these repositories:

| Term | Meaning |
| :--- | :--- |
| `RPF9` | Rockstar Package File v9 (`9FPR` / `0x52504639`). Next-generation archive container used by GTA VI / RAGE 9, supporting nested RPFs, PSIN tables, and zlib/BCPack payloads. |
| `TTPF` | RAGE 9 texture store format holding mipmapped GPU-swizzled surfaces (`BC1`-`BC7`, `RGBA8`, `RGBA16F`, texture arrays, cubemaps). |
| `BCPack` | Block-compression stream format used on Xbox Series X\|S alongside GDeflate to compress GPU textures and resource pages. |
| `ADAT` / `AWC` | Rockstar audio bank containers storing multi-stream `Opus` and `XMA2` voice, SFX, and radio assets. |
| `BK2` (Bink 2) | RAD Game Tools video codec used in `movies.rpf` reels and UI cutscenes. |
| `RDNA2` (`GFX10.3`) | AMD GPU ISA used by Xbox Series X\|S and PS5. Tools like `Leonida` and `OpenScarlett` lift precompiled RDNA2 shader binaries to `SPIR-V` (Vulkan) or `DXIL` (D3D12). |
| `Scarlett` / `d3d12_xs` | Microsoft codename for Xbox Series X\|S and its console-extended Direct3D 12 + `kernelx` runtime shimmed on Windows by `OpenScarlett`, `xbox-emulator`, and `xcl`. |

---

## Inclusion Criteria & Exclusions

To keep this directory useful and safe, projects are audited before being added:

- **Included:** Real working code, libraries, format specs, or active runtime/translation research backed by actual commits.
- **Excluded:** Empty placeholder repos (0 commits / empty tree), raw extracted copyrighted game strings or assets (including text dumps of the build), decryption keys or projects hunting for archive cipher keys, cheat-code / wallpaper hubs, `make.fun` bounty/meme repos, and clickbait "100% playable GTA 6 PC port" or malware-bait repositories.
- **Also excluded:** the dozens of identical `GTA-6-Switch-Version--PC-Emulator-2026` / "PC installer hub" repos, fake or Rockstar-branded launchers (including ones signed with a fake `Rockstar Games, Inc.` certificate), README-only "recomp" repos with a release attached, repos whose only download is an unverified prebuilt `.exe`, and renamed forks of projects already listed (the original is linked instead).

---

## Contributing

Know an open-source RAGE 9 / RPF9 / GTA VI reverse engineering tool, library, or format writeup that belongs here?

1. Check [CONTRIBUTING.md](CONTRIBUTING.md) to verify clean-room compliance and format guidelines.
2. Open a pull request or [submit a project via the issue form](.github/ISSUE_TEMPLATE/submit-project.yml).

Questions, or want to talk formats? [Join the Discord](https://discord.gg/reADpA6Cd).

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](LICENSE)

To the extent possible under law, contributors have waived all copyright and related or neighboring rights to this curated list under [CC0 1.0 Universal](LICENSE). Linked projects remain under their respective licenses.
