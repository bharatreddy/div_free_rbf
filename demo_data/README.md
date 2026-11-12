# Local demo inputs

Place fitacf / fitacf3 files (raw or bz2 compressed) in `fitacf/` and the
matching MAP2 file in `map2/`. Observation files are ignored by Git and are not
included in this distribution. Each fitacf file must contain one radar.

In `demo.ipynb` at the repository root, edit the glob patterns and UTC fit time to match your
event. The MAP2 glob must select exactly one file. You may select multiple fitacf
files and radars. Configure processing in the root `default_config.json`.
