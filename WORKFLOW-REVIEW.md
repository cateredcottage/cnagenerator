# Catered Cottage workflow review

The current package is a tested local nursing assignment application. The interface and assignment checks below passed in Microsoft Edge. It still stores each browser's data separately; it is not yet a shared staff system.

## Interface decisions

- Keep the light hospital-style interface and the official centered logo. Remove dark mode and the repeated facility name above the page title.
- Keep OCC, PSA, EA, HOSP, and VAC unchanged.
- Add **View all residents**, a searchable resident and bed directory with status filters, CNA/PSA assignments, and **Open room** actions. The directory lists bed identifiers; the existing app does not contain resident names.
- Add explicit **Find** and **Show all rooms** buttons.
- Keep notices, workload adjustments, backups, and compact printing. CNA coverage remains removed.

## Problems corrected

- Census summary buttons now update the room list and editor together.
- Searching for a room clears a filter that would otherwise hide the room. Invalid room or bed identifiers produce feedback.
- Empty filters disable room actions and show an empty editor instead of presenting an unrelated room.
- Browser printing refreshes date and shift metadata. The generated timestamp now reflects when assignments were generated.
- Individual CNA sheets now repeat the logo and shift header on each sheet.
- Copy for Word now supplies formatted HTML with page-break formatting where supported. Plain-text fallback retains clearly marked page-break text.
- Failed browser storage remains visible. Backup export stops when the current census could not be saved, rather than exporting stale stored data.

## Verification

| Workflow | Result |
| --- | --- |
| Workload target calculation | 20,400 cases matched Catered Cottage Management v4.23.363, including reduced workloads and 0–84 eligible beds. |
| Bed assignment | 2,550 cases using consecutive room and bed assignments and 1–30 CNAs; every eligible bed assigned once, with target counts respected. |
| Census | Individual and room updates, summary filters, search, reset, undo/redo, empty results, and status totals passed. |
| Resident directory | All 84 beds, resident-only results, each status filter, search, current staff, room navigation, close controls, and responsive widths passed. |
| Staffing | Names, persistence, workload reductions, recalculation, range limits, and assignment invalidation passed. |
| Copying | All assignments, individual CNA copying, PSA/hospital lists, formatted HTML, and manual-copy fallback checked. |
| Notices | Post, edit, remove, reload, templates, placeholder validation, plain-text safety, backup restore, and print selection passed. |
| Backups | Export/restore, legacy status and reduction migration, malformed/invalid rejection, canceled restore, and storage failure passed. |
| Printing | Compact, separate CNA sheets, bed details, refreshed metadata, logos, optional notices, and white backgrounds checked through PDF rendering. |
| Layout | Header centering and page overflow checked at 320, 390, 768, 1024, 1440, and 1920 pixels. Directory checked at phone, tablet, and desktop widths. |

The tested seven-CNA compact layouts fit one Letter page, including a short printed notice. Larger notices, additional PSA groups, or other staffing arrangements may require more pages.

Actual pasting into the staff's Microsoft Word version and physical printer output still need a short acceptance check on their equipment. Automated clipboard checks verify the exported text and HTML; they do not verify Word's handling of that HTML.

## Recommended next steps for a shared product

1. Shared storage so staff see the same census, assignments, and notices, with protection against simultaneous edits.
2. Staff sign-in and roles so notice posting, facility-wide updates, and backup restoration can be limited to authorized users.
3. Saved shift snapshots and a change history showing who changed census or assignments and when.
4. Automatic backups and a tested recovery process.
5. A short staff acceptance trial using their usual browser, Word version, and printer before a broader rollout.

These are recommendations, not features included in this local package. Hosting the current static folder by itself does not provide them.


## Latest workload revision

Removed **Distribute beds across CNAs**. Old saved spread preferences are ignored, and new backups no longer contain that preference. All bed assignments use consecutive room and bed blocks.

Randomized standard workloads now avoid immediately repeating the same smaller-assignment group when another group is possible. The previous group persists across reloads. The interface distinguishes randomized standard workloads from intentionally fixed reduced workloads.

Regression tests passed for 101 generations with 83 residents and 7 CNAs: all seven CNAs received the smaller assignment, no consecutive group repeated, and all eligible beds were assigned once. Another 50 generations preserved a fixed CNA 1 reduction while varying standard assignments. Equal workloads, history persistence, and legacy preference migration passed.

## Automatic mode and removed controls

Randomization is always automatic. The ON/OFF switch is removed, and older OFF preferences are ignored. Generate assignments is now the single generation action. Shuffle workloads and the workload-balancing display were removed; fair randomization operates in the background. Explicit CNA reductions remain fixed, as chosen in the reduced-assignment settings.

The running browser issue was reproduced with 83 eligible beds, six CNAs, and a fixed one-resident reduction for CNA 1. That intentionally produced 13 for CNA 1 and 14 for everyone else. Clearing the adjustment enabled automatic variation. The live page was then verified with Generate and Shuffle changing which CNA receives 13 while the others receive 14.

Facility-wide Set all controls were removed. Empty hospital sections are hidden in print; populated lists still appear. Individual bed and room controls passed regression checks, and the compact print output was rendered and visually checked.


## Simplified assignment action

Removed Shuffle workloads from desktop and mobile and removed the workload-balancing panel. Generate assignments always uses the current census and applies fair randomization in the background. Random history and selected CNA reductions are preserved. Repeated generation tests, mobile Generate/Print wiring, full workflow checks, and the running browser verification passed.

