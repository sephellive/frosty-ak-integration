# Changelog

## v0.0.4

- Restored the AK-101 attachment base expected by 3DSS EFT Reposition and PUSSY dynamic scopes.
- Recalculated the unscoped compensation so Frosty's iron-sight ADS remains unchanged.
- Kept all scope-specific sections and scope definitions owned by PUSSY/MAS.

## v0.0.3

- Added mathematically compensated AK-101 `aim_hud_offset_*` values for the Modded Exes `attach_base + aim` transform.
- Preserved MAS scopes while making unscoped ADS resolve to Frosty's native AK-101 position and rotation.

## v0.0.2

- Removed incompatible Frosty `aim_hud_offset_*` overrides that moved the MAS-enabled AK-101 into the upper-left corner.
- Kept only the AK-101 MAS `attach_base_hud_offset_*` correction.

## v0.0.1

- Added an AK-101-only DLTX compatibility patch.
- Restored Frosty iron-sight ADS offsets.
- Restored MAS base attachment offsets after 3DSS EFT Reposition overrides.
