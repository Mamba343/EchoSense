# Datasets

Recorded CSI sessions will live here.

Datasets should use versioned session folders instead of flat CSV files.

Example:

```text
datasets/
  2026-07-03_14-35-21/
    metadata.json
    raw.csv
    processed.csv
```

Session metadata should include timestamp, firmware version, backend version, board type, router channel, sampling rate, and calibration information.
