# Praxis_01

SAP Business One Screen Painter design for **Activity Group**.

## Screen (preview)

Open `sap-b1/preview/activity-group-form.html` in a browser for an interactive mockup of the B1 form:

- **Level 2** (header) loads the **Activity Group** list
- Choosing an Activity Group fills **Level 1** (read-only)
- **Location** / **Sub Location** are manual Choose-from-List fields when Type = Activity Group
- **List** tab shows all 18 master rows from the source sheet

## Import into Screen Painter

1. SAP B1 → Tools → Customization Tools → Screen Painter
2. Open `sap-b1/forms/PRAX_AGP.srf`
3. Follow field rules in `sap-b1/docs/screen-spec.md`
