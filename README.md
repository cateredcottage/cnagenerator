# Catered Cottage — Hospital-Style Nursing Interface

Extract the entire ZIP, then open `index.html` in Microsoft Edge or Chrome. Keep the `assets` folder beside it. The webpage also works on a static web host; upload the full folder together.

The new hospital-style interface has a centered facility masthead, navy and blue design, census summary, room-status workspace, shift staffing panel, and a full-width assignment sheet section. It retains the Classic app's room map (84 beds across 29 rooms), census editing, observation groups, search and filters, staff assignments, reduced workloads, randomized smaller standard workloads, copying for Word, and printing. CNA coverage changes have been removed.

## Reduced workload

The calculation matches Catered Cottage Management v4.23.363. “1 fewer” or “2 fewer” uses the normal rounded-up workload for the shift. For example, 75 residents across 6 CNAs has a normal ceiling of 13; a reduction of 1 targets 12, and a reduction of 2 targets 11. Standard assignments share the remaining beds fairly. Each eligible bed is assigned once.

Occupied beds and expected admissions count toward CNA workload. Occupied and observation beds count toward census. Observation residents receive separate PSA assignments. Hospitalized residents and vacant beds do not enter either total.

Changing the census, staffing count, reduced workloads, or randomization preferences clears old assignments. Generate again before printing or copying.

If every staff member has a reduction, the app explains that remaining beds still need assignments. Those preferences may not all be achievable. Reductions never cause residents to disappear from the assignment.

## Saved census and backups

Autosave is local to the browser and location where you open the app. Moving to a new folder, browser, device, or web address can use different browser storage. The original Classic file has been left unchanged.

Use **Export backup** to download the census, observer groups, names, reduced workloads, and preferences. Use **Import backup** to import that file. Generated assignments are recreated from the restored census.

If the new webpage does not find your old Classic census, transfer it before entering new data:

1. Open the original Classic app in the browser where staff used it.
2. Press F12, select Console, and run this command to download the original saved census:

```javascript
(() => {
  const saved = localStorage.getItem('cc_cna_roomlist_editor_v2');
  if (!saved) { alert('No saved Classic census was found in this browser.'); return; }
  const url = URL.createObjectURL(new Blob([saved], {type:'application/json'}));
  const link = document.createElement('a');
  link.href = url; link.download = 'classic-census-backup.json'; link.click();
  setTimeout(() => URL.revokeObjectURL(url), 1000);
})();
```

3. In the redesigned app, choose **Import backup** and select that JSON file.

Older “Out to Hospital” statuses display as “Hospitalized” while retaining their compatible saved status values. Legacy reduced-workload adjustments are supported. Restore asks before replacing the saved census.

## Project files

- `index.html`: page structure.
- `assets/css/hospital.css`: hospital-style interface and responsive layouts.
- `assets/css/print.css`: nursing assignment sheet print layout.
- `assets/js/app.js`: census, assignment engine, persistence, and backups.
- `assets/js/interface.js`: mobile actions and control accessibility.
- `assets/js/workspace.js`: notices, templates, and notice backup preferences.
- `assets/images/logo-cch.png`: official Catered Cottage Healthcare logo supplied by the user.

This package uses browser storage for local use. Staff on different computers have separate saved data. Hosting it alone does not add shared data synchronization.

## Verification

The target calculator matched management build v4.23.363 in 20,400 scenarios, covering 0–84 workload beds, 1–30 staff, both randomization settings, and multiple reduction patterns. Edge browser checks passed for generation, current reduction settings, redistribution, clearing reductions, staff names, saved settings, undo/redo, changed-census invalidation, observation and hospital totals, room distribution, zero census, backup restore, legacy migration, and mobile actions. Desktop and phone layouts and print output were checked.

## Branding and compact printing

The official Catered Cottage Healthcare logo is centered above the page title and on the assignment printout. Source: https://cateredcottagehc.com/wp-content/uploads/2025/12/Logo-CCH.png

The status key uses OCC (Occupied), PSA (Observation), EA (Expected Admission), HOSP (Hospitalized), and VAC (Vacant). Existing saved status values remain compatible. PSA assignments remain separate from the CNA workload.

Printing defaults to two assignment cards per row with compact room and bed columns and 9-point room text. The tested seven-CNA shift, including separate PSA and hospital entries, fits on one Letter page at 100% scale. Other staffing counts and PSA groups can require more pages. One sheet per CNA remains optional.


## Appearance and announcements

The interface uses a light hospital-style appearance. Dark mode was removed after review. The official logo is centered above the title, without repeating the facility name below it. Assignment printing uses a white background.

Use **Write a notice** to post a title, notice type, and message. You can edit or remove notices. Suggested messages cover holiday scheduling, holiday celebrations, shift handoff reminders, and team appreciation. Replace bracketed details before posting. Notices are displayed as plain text.

Select **Include this notice on assignment printouts** only for notices that should appear on paper. Notices are otherwise excluded from printing to preserve the compact layout.

Announcements are included in exported backups. Importing an older census-only backup preserves existing announcements. Notices are local to the current browser and address; they do not automatically synchronize across staff computers.

Additional browser checks passed for the light interface, announcement posting/editing/removal, suggestion placeholders, plain-text safety, backup transfer, mobile layouts, and print selection. A tested seven-CNA shift with a short notice still fits one Letter page.

## Resident directory and navigation

Select **View all residents** in the Resident census panel to see a searchable directory with bed identifiers, status, and current CNA/PSA assignments. **All residents** includes occupied, PSA, and hospitalized residents. **All beds and statuses** also includes expected admissions and vacant beds. Select **Open room** to edit that bed in the census workspace.

**Find** opens a room or bed even when the current room filter would hide it. **Show all rooms** resets the room filter. Empty filters disable room update actions.

**Copy for Word** provides formatted HTML with page-break formatting where the browser permits it. Clipboard restrictions fall back to plain text and manual copy, with textual page-break markers. Check pasting once in your team's Word version.

Browser print and the Print button refresh shift/date metadata. Individual CNA sheets repeat the facility logo and shift header. Failed local saving stays visible, and export stops rather than using stale saved data.

## Consecutive assignments and randomization

The **Distribute beds across CNAs** feature was removed. All assignments now use consecutive room and bed blocks. Old backups with that option enabled are accepted, but the obsolete option is ignored and omitted from new exports.

Automatic randomization determines which standard CNAs receive the smaller workload when totals cannot divide evenly. For 83 eligible residents and 7 CNAs, six receive 12 and one receives 11. Generating again varies the smaller standard group and avoids immediately repeating that group when another group is possible. The previous random selection is remembered across reloads. Randomization is always automatic; the ON/OFF switch has been removed. Old backups with randomization OFF are upgraded to automatic mode.

Selected reduced workloads remain assigned to the chosen CNA. A reduction configured for CNA 1 intentionally keeps CNA 1's target lower; it is not moved by standard workload randomization. If all standard workloads are equal, the page explains that no smaller assignment needs randomization.

Regression checks covered 101 randomized generations with one smaller workload across seven CNAs, 50 generations with a fixed CNA 1 reduction, automatic mode, equal totals, unique complete bed assignments, and obsolete preference migration.

## Simplified controls and print sections

Facility-wide **Set all** status controls have been removed. Individual bed and selected-room updates remain available. Empty hospitalized-resident sections are omitted from printouts; a populated hospital list still prints.

**Generate assignments** is the only generation action on desktop and mobile. Shuffle workloads and the workload-balancing display have been removed. Fair automatic randomization continues in the background whenever another standard workload split is possible. Explicit selected reductions remain fixed; clear adjustments to include those CNAs in standard randomization.

