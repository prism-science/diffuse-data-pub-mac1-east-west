# Raw X-ray diffraction data for apo Mac1 (`mac1-east-west`)

This repository contains the raw diffraction images used to produce the diffuse scattering maps of apo Mac1 (SARS-CoV-2 nsp3 macrodomain) reported in:

Zielinski K, Meisburger S, Lu K, Bassim V, Holton J, Fraser JS, Ando N. (2026). Iterating diffuse scattering protocols in parallel leads to reproducible, high-quality data from Mac1. The Stacks. [DOI: 10.82153/hrpy-t681](https://doi.org/10.82153/hrpy-t681)

It is a [DataLad](https://www.datalad.org) dataset. The images are tracked with git-annex and hosted on public object storage, so they can be downloaded anonymously and verified by checksum. Code that processes these data with DIALS and mdx2 is provided in [diffuse-processing-mac1-east-west](https://github.com/prism-science/diffuse-processing-mac1-east-west), which includes this repository as the subdataset `inputs/raw/mac1-east-west`. Lower-quality datasets analyzed in the article are in the sibling repository [diffuse-data-pub-mac1-suboptimal](https://github.com/prism-science/diffuse-data-pub-mac1-suboptimal).

## Contents

Data are organized by beamtime and crystal. Crystal names are unique only within a beamtime (for example, `mac1_2` appears in three beamtimes).

```
<beamtime>/
    README.md           crystal-to-logbook table for the beamtime
    <crystal>/
        crystal/        diffraction images of the crystal (Bragg and diffuse scattering)
        background/     background images (no crystal) for diffuse background subtraction
metadata.csv            one row per crystal
```

| Beamtime | Beamline | Date | Crystals | Images per crystal (data / background) | Format | Size |
|---|---|---|---|---|---|---|
| `als_20251015` | ALS 8.3.1 | 2025-10-15 | `mac1_10`, `mac1_12`, `mac1_13` | 2800 / 280 | CBF | 58 GB |
| `als_20251125` | ALS 8.3.1 | 2025-11-25 | `mac1_2`, `mac1_4` | 3600 / 360 | CBF | 49 GB |
| `chess_20250924` | CHESS 7B2 | 2025-09-24 | `mac1_2`, `mac1_3`, `mac1_5` | 3600 / 360 | Eiger HDF5 | 47 GB |
| `chess_20251105` | CHESS 7B2 | 2025-11-05 | `mac1_4`, `mac1_5` | 7200 / 720 | Eiger HDF5 | 66 GB |

The total size is approximately 220 GB. ALS = Advanced Light Source (Berkeley, CA); CHESS = Cornell High Energy Synchrotron Source (Ithaca, NY).

## Metadata

- [`metadata.csv`](metadata.csv) has one row per crystal with the columns `beamtime`, `crystal`, `logbook_entry` (link to the electronic logbook), `sample`, `data_type`, `date_collected`, `subject_composition`, `beamline`, `beamline_ror` ([ROR](https://ror.org) identifier of the facility), `has_background_images`, `has_braggs`, and `has_diffuse`.
- Each beamtime directory has a `README.md` with a table linking every crystal to its `logbook_entry`: [`als_20251015`](als_20251015/README.md), [`als_20251125`](als_20251125/README.md), [`chess_20250924`](chess_20250924/README.md), and [`chess_20251105`](chess_20251105/README.md). The logbook entries describe sample preparation and the beamtime experiments.

## Usage

Install [DataLad](https://handbook.datalad.org/en/latest/intro/installation.html), which requires git-annex (for example, `mamba install -c conda-forge datalad`). Cloning downloads only file names and metadata; the images are fetched on request.

```bash
datalad clone https://github.com/prism-science/diffuse-data-pub-mac1-east-west.git
cd diffuse-data-pub-mac1-east-west

# fetch one crystal
datalad get chess_20250924/mac1_2

# fetch a whole beamtime using four parallel jobs
datalad get -J4 als_20251125

# free up disk space when finished
datalad drop als_20251125
```

Downloads are checked against the recorded checksums. Plain git-annex works as well (`git annex get <path>`). To use these data with the processing scripts, clone [diffuse-processing-mac1-east-west](https://github.com/prism-science/diffuse-processing-mac1-east-west) instead; each script fetches the data it needs.
