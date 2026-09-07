# Activity Group — SAP B1 Screen Painter design

Form type: `PRAX_AGP`  
File to open in Screen Painter / B1 Studio: `sap-b1/forms/PRAX_AGP.srf`  
Visual mockup (browser): `sap-b1/preview/activity-group-form.html`

## Screen layout (General)

```
Activity Group
┌ General ── List ────────────────────────────────────────┐
│ Classification                                          │
│   Type *            [ Activity Group            ▼ ]     │
│   Level 2 *       ▶ [ SALES                       ] CFL │
│   Activity Group *▶ [ SALES - BROKERAGE & COMM…   ] CFL │
│   Level 1           [ Sales & Marketing           ] RO  │
│                                                         │
│ Location (manual)                                       │
│   Location        ▶ [ SB-19D                      ] CFL │
│   Sub Location    ▶ [ DEPT-SALES                  ] CFL │
│                                                         │
│ [OK] [Cancel]                                           │
└─────────────────────────────────────────────────────────┘
```

`▶` = Linked Button + Choose from List (yellow field).  
`RO` = disabled, filled by addon / UI API.  
`*` = mandatory.

**List** tab is a matrix of the 18 master Activity Groups (same columns as the source sheet).

## Field behaviour (from the sheet)

| Field | Control | UID | DB field | Rule |
| --- | --- | --- | --- | --- |
| Type | Combo | `cmbType` | `U_Type` | `AG` = Activity Group, `AC` = Activity |
| Level 2 | Edit + CFL | `txtL2` | `U_Level2` | Header driver. On change, reload Activity Group CFL. Also auto-filled when an Activity Group is chosen. |
| Activity Group | Edit + CFL | `txtAG` | `U_ActGrp` | CFL filtered: `U_Level2 = header Level 2` |
| Level 1 | Edit (disabled) | `txtL1` | `U_Level1` | Auto from Activity Group master. Never typed. |
| Location | Edit + CFL | `txtLoc` | `U_Loc` | Manual. Enabled only if Type = Activity Group. |
| Sub Location | Edit + CFL | `txtSLoc` | `U_SubLoc` | Manual. Enabled only if Type = Activity Group. |

When Type = **Activity**, Location and Sub Location stay empty (controls disabled).

## UI API events to wire after Screen Painter

1. `cmbType` combo select after → enable/disable `txtLoc`, `txtSLoc`, `lnkLoc`, `lnkSLoc`.
2. `txtL2` ChooseFromList after → apply `Conditions` on `CFL_AG` (`U_Level2` equal selected value); clear Activity Group / Level 1 if Level 2 changed.
3. `txtAG` ChooseFromList after → write Level 1 and Level 2 from the chosen master row. Do not overwrite Location unless the user has not set it yet (default suggestion only).

## UDT / UDO (create before binding the form)

| Table | Type | Purpose |
| --- | --- | --- |
| `@PRAX_OAGP` | Document / Master | Form header (or use as UDF set on a marketing document) |
| `@PRAX_AGM` | Master Data | Activity Group catalogue (the 18 rows) |
| `@PRAX_L2` | Master Data | Level 2 values: SALES, MARKETING - PROJECT, MARKETING - BRAND, Finance |
| `@PRAX_LOC` | Master Data | Locations (SB-19D, …) |
| `@PRAX_SLC` | Master Data | Sub locations (DEPT-SALES, DEPT-MARKETING) |

Suggested UDO codes: `PRAX_AGP` (this form), `PRAX_AGM`, `PRAX_L2`, `PRAX_LOC`, `PRAX_SLC`.

Seed data is in `sap-b1/data/activity-groups.json`.

## How to open in Screen Painter

1. In SAP Business One: *Tools → Customization Tools → Screen Painter* (or B1 Studio).
2. Open `PRAX_AGP.srf`.
3. Adjust pixel positions if the company UI font differs (Tahoma 8 vs 9).
4. Bind CFLs to the UDOs after they exist in the company database.
5. Save to database / assign as UDO default form (`FormSRF`).
