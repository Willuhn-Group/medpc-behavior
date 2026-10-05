# CLAUDE.md — medpc-behavior

@../.github/LAB_CONVENTIONS.md

> If the import above did not load (the `.github` repo is not cloned next to this
> one), read https://github.com/Willuhn-Group/.github/blob/main/LAB_CONVENTIONS.md
> before changing anything. Those rules apply here.

## What this repo is
- **Type:** general-purpose (used by project repos, e.g. `ephys-induced-polydipsia`)
- **Purpose:** read MedPC (Med Associates) output files and turn them into
  trial-by-trial behavioral data: trials, events, latencies, bouts, histograms.
- **Maintainer(s):** Sergio Conde-Ocazionez
- **Current version:** untagged (first tag planned: v0.1.0)

## Layout
```
matlab/
  readMedpc.m        % raw MedPC file -> struct
  getTrials.m        % trial/ITI structure from event codes (cfg.events)
  addEvent.m         % add event times, latencies, first events, bouts per trial
  extractBouts.m     % group timestamps into bouts (minInterval, minDuration)
  eventHistogram.m   % event histograms across trials
  medEvents.m        % event extraction helper
example_use/
  multipelletExample.m   % full walkthrough on example_data/example_rat_multipellet
example_data/
```

## Typical flow
`readMedpc` → `getTrials(cfg)` → `addEvent(trialStruct, evConfig)` →
`extractBouts` / `eventHistogram`

The MedPC event codes are task-specific and defined by the user in `cfg.events`
(see `multipelletExample.m` for the multipellet task codes).

## Dependencies
| Dependency | Version | Why |
|---|---|---|
| MATLAB | R2020a+ | — |

## Rules specific to this repo
- `ephys-induced-polydipsia` calls `getTrials`, `addEvent` and `extractBouts`:
  keep their inputs and output fields backward compatible.
- `multipelletExample.m` must run from a fresh clone with no edits.

## Known issues / in progress
- `getTrials.m` calls `read_medpc` (old name); the function is now `readMedpc`.
- Several headers still use old snake_case names (`read_medpc`, `med_file`, `get_trials`).
- `multipelletExample.m` hard-codes `M:\GitHub\...`; switch to a path relative to the script.
- README mentions a `python/` folder that does not exist yet.
