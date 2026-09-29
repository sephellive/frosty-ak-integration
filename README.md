# Frosty AK Integration

A profile-driven S.T.A.L.K.E.R. Anomaly DLTX compatibility addon for weapons from Frosty's Escape From Tarkov Rifle Pack. The initial profile covers the AK-101.

The addon separates weapon integration from scope definitions. Each supported HUD has one declarative profile containing its native iron-sight ADS and MAS base/mount transform. A generator calculates the compensated iron-sight offsets and emits the late DLTX override. MAS/PUSSY remains the sole owner of every scope-specific offset, so adding a scope requires no change here and adding a weapon requires one profile rather than a weapon-by-scope matrix.

It does not replace weapon models, animations, sounds, scopes, balance values, or files from its dependencies.

## Requirements

- S.T.A.L.K.E.R. Anomaly 1.5.3
- [Anomaly Modded Exes](https://github.com/themrdemonized/xray-monolith) with DLTX support
- Escape From Tarkov Rifle Pack 4.7 by frostychun
- Modular Attachment System: Vanilla Weapons by party_50

The patch is intended for a setup that also uses 3DSS EFT Reposition/Pizza scope integration. Its late DLTX filename ensures that the AK-101 values are applied after the dependency overrides.

## Installation

Download the ZIP from the latest GitHub Release and extract it directly into the S.T.A.L.K.E.R. Anomaly directory. The archive starts with `gamedata/`; it has no extra wrapper directory.

For a local checkout, run:

```powershell
./tools/install.ps1 -GamePath "D:\Stalker\Anomaly-Test"
```

The installer copies and updates this addon's files. It does not delete the game's existing `gamedata` or files belonging to other addons.

To download and install the latest GitHub Release instead of the local files:

```powershell
./tools/install.ps1 -GamePath "D:\Stalker\Anomaly-Test" -Latest
```

`-Latest` derives the repository from the `origin` remote. Public releases need no token. For a private repository, set `GH_TOKEN` or `GITHUB_TOKEN` to a token that can read the repository. Local installation never uses the GitHub API.

## Profile model

Profiles live in `profiles/weapon-profiles.json`. Each profile declares:

- one or more weapon sections that use the same transform;
- one or more HUD sections that share the same model origin;
- the MAS `scope_group_*`;
- Frosty's native iron-sight position and rotation;
- the MAS attachment base, mount, rotation, and scale.

Run the generator after editing profiles:

```powershell
./tools/generate-profiles.ps1
```

Validate that the committed DLTX file matches the profiles:

```powershell
./tools/generate-profiles.ps1 -Check
```

The generator computes `aim = native ADS - attach_base` independently for position, rotation, and optional `*16x9` values. It verifies that `attach_base + aim` resolves exactly to the declared native ADS. It also rejects duplicate weapon/HUD ownership, malformed vectors, invalid scope-group names, and non-positive scales.

The generated AK-101 integration patches only these sections:

```ini
![wpn_ak101]
![wpn_ak101_hud]
```

The weapon section selects the shared `scope_group_picatinny_short`. The HUD section owns only weapon-side `aim_hud_offset_*`, `attach_base_*`, `attach_mount_*`, and `attach_scale`. There are deliberately no scope sections or scope-specific offsets in this repository.

## Extending the addon

To support another weapon, add one object to the `profiles` array and regenerate the DLTX file. Multiple variants can list the same transform in `weapon_sections` or `hud_sections`. If 16:9 values differ, add `position_16x9`, `rotation_16x9`, `base_position_16x9`, `base_rotation_16x9`, `mount_position_16x9`, or `mount_rotation_16x9`; otherwise the generator reuses the normal value.

New scopes automatically work through the selected MAS scope group. Do not add scope names or optic-specific HUD sections to a weapon profile. A scope that is misaligned on every weapon must be fixed once in its owning MAS/PUSSY addon, not duplicated across weapon integrations.

## Branching

- `master` is stable and releasable.
- `feature/*` is for development and testing.

## Releases

Every push or merge to `master` runs GitHub Actions. The workflow packages `gamedata/`, creates version `v0.0.<run number>`, creates the matching Git tag and GitHub Release with generated notes, and uploads `<repository>-<version>.zip`.

Rerunning the same workflow keeps the same version and replaces the release asset instead of creating a conflicting tag.

## Git LFS

The template tracks common binary game assets (`.dds`, `.ogf`, `.object`, `.ogg`, `.wav`, `.tga`, and `.png`) with Git LFS. Install Git LFS before adding those files and ensure CI has access to the LFS objects. Text files such as LTX, Lua scripts, Markdown, YAML, and PowerShell remain in normal Git history.

## Credits and license

The integration patch is released under the MIT License. Frosty's Rifle Pack, MAS, 3DSS EFT Reposition, S.T.A.L.K.E.R. Anomaly, and their assets remain subject to their respective licenses and are not redistributed here.
