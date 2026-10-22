[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on006107-blue)](https://doi.org/10.82901/nemar.on006107)

<!-- nemar:provenance -->
## Provenance

This dataset is mirrored on NEMAR from OpenNeuro.

- Source: [ds006107](https://openneuro.org/datasets/ds006107)
- DOI: [10.18112/openneuro.ds006107.v1.0.0](https://doi.org/10.18112/openneuro.ds006107.v1.0.0)

## Channel names follow the data headers (18 Oct 2026)

In 167 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count), so `mne_bids.read_raw_bids` stopped with 'Channel mismatch' (or 'Duplicate channel names'); EEGDash renamed the data channels by position to the channels.tsv names, which gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 18189 rows took the header name (the header labels carry '-AV' suffix).

2 header channels had no channels.tsv row (1 files) and got a new row: type from the name (ECG/EMG/EOG/trigger) or from the other contacts of the same shaft, else MISC; units from the header; status n/a. Examples: `sub-32_ses-01_task-sws_run-01_ieeg.edf`: B35-AV, B36-AV.

47 channels.tsv rows had no channel in the data file (6 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-06_ses-01_task-sws_run-01_ieeg.edf`: B35, B36; `sub-148_ses-01_task-sws_run-01_ieeg.edf`: C1, C2, C3, C4, C5; `sub-150_ses-01_task-sws_run-01_ieeg.edf`: C1, C2, C3, C4, C5.

1 files listed the channels in another order than the header; rows now follow the header order.

EEGDash's positional renaming had given wrong labels to 76 channels in 2 files (rows missing, extra or out of order before that position).

electrodes.tsv is unchanged: it names the contacts (e.g. `A1`), while channel names follow the recorder (e.g. `A1-AV`); to attach positions in MNE, map channel names to contact names first, e.g. `raw.rename_channels(lambda n: re.sub(r'-[A-Za-z0-9]+$', '', n))`.

## Electrode coordinate system (19 Oct 2026; described 22 Oct 2026)

A coordsystem.json (`fsaverage`, `mm`) was added next to 166 of the 167 electrodes.tsv. This deposit does not state a coordinate system, so the frame is inferred, not documented: in each of these 166 files, contacts have x/y/z identical (compared to 0.001 mm) to an electrodes.tsv of another deposit from the same group whose metadata declares the frame. Each sidecar's iEEGCoordinateSystemDescription names its source file, deposit and commit, and how many of its contacts are identical. The other 1 electrodes.tsv share no such identity and have no coordsystem.json.

Reproducible check (22 Oct 2026): for each of the 166 electrodes.tsv, every contact's x/y/z was compared (to 0.001 mm) with every electrodes.tsv of other deposits from the same group at pinned nemarDatasets commits (on004551@c4c035f6, on004859@db21ebb2, on005007@b59363aa, on005545@948f2825, on005931@cbd529e5, on006233@396692f0, on006910@9eb96ebd, on006914@1956b035, on007118@8f2c41fa, on007119@c099745b, on007120@66296ebd). 126 files have all contacts identical to one file of another deposit: 113 to a deposit whose coordsystem.json declares fsaverage/mm (on007118, on007119, on007120) and 64 to a deposit whose README states MNI305/MNI-305 (on004551, on005545, on006233, on006910, on006914); a file can match both. The other 40 match partly (37 with at least 89 % of contacts identical, 3 with 50-89 %, 0 below 50 %, 0 none).

Limitation: these sidecars declare the frame and units only. electrodes.tsv is unchanged and no coordinate value changed. Positions do not attach to channels in mne_bids or EEGDash, because the channel names (data-header labels, e.g. prefixed labels or bipolar derivations) differ from the contact names in electrodes.tsv; a channel-to-contact mapping is not part of this change. This dataset mirrors OpenNeuro; the same change belongs upstream.
