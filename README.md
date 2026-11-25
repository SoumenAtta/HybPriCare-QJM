# HybPriCare-QJM v1.0: An Integrated Geospatial Dataset for Hybrid Primary-Care Network Design and Digital Vulnerability Analysis

## Study Area: NHS Lincolnshire Integrated Care Board (QJM), England

**Author:** Soumen Atta  
**Affiliation:** Faculty of Information Technology, University of Jyväskylä, Finland  
**Version:** 1.0  
**First public release date:** 2026-11-25  
**Year:** 2026  
**GitHub data repository:** https://github.com/SoumenAtta/HybPriCare-QJM  
**Zenodo dataset DOI:** 10.5281/zenodo.22232373  
**Zenodo archival URL:** https://doi.org/10.5281/zenodo.22232373

This versioned, data-only package is the common release package for GitHub
and Zenodo. Use the Zenodo DOI as the archival identifier and the GitHub
repository for versioned access to the same files. The first public release
date above refers to the dataset release, not the historical source
snapshots or dataset-construction date.

---

## 1. Overview

**HybPriCare-QJM** is an author-developed, integrated, model-ready geospatial research dataset constructed from publicly available administrative and geospatial data. It was developed to support research on primary-care accessibility, healthcare facility location, hybrid physical–digital healthcare networks, assisted telehealth, digital vulnerability, service allocation, and equity-aware service planning.

The dataset represents the geographical area of the **NHS Lincolnshire Integrated Care Board (ICB), England**. The corresponding identifiers are:

- NHS Organisation Data Service ICB code: **QJM**
- Office for National Statistics ICB geography code: **E54000013**

HybPriCare-QJM integrates information from several publicly available administrative and geospatial data sources maintained by NHS England, NHS Business Services Authority, the Office for National Statistics, the English Indices of Deprivation, Ofcom, and Ordnance Survey.

### Independence and institutional disclaimer

HybPriCare-QJM is an independent research dataset developed by the author from publicly available administrative and geospatial data. It was not produced, commissioned, sponsored, endorsed, or validated by NHS Lincolnshire Integrated Care Board, NHS England, NHS Business Services Authority, the Office for National Statistics, Ofcom, Ordnance Survey, or any other healthcare or data-provider organization. References to NHS Lincolnshire Integrated Care Board and the code `QJM` identify the geographical study area and its official administrative designation only. The views, assumptions, derived indicators, candidate-site definitions, and interpretations contained in HybPriCare-QJM are those of the author and should not be attributed to the organizations that published the underlying source data.

The final dataset contains:

| Component | Value |
|---|---:|
| Demand zones (2021 LSOAs) | 435 |
| Aggregate registered-patient demand | 822,413 |
| Existing GP practices | 81 |
| Eligible NHSBSA pharmacy-type contractor records | 112 |
| Unique candidate physical access locations | 109 |
| Demand–GP pairs | 35,235 |
| Demand–candidate-site pairs | 47,415 |
| National LSOAs used for DVI calibration | 33,755 |
| OS Open Roads links extracted | 404,625 |
| Usable road links | 402,387 |
| Missing required road distances | 0 |

The dataset is intended to provide a common empirical basis for operations-research, health-services, healthcare-accessibility, and geospatial studies. It does **not** prescribe a particular optimization model.

For example, the same empirical dataset can be used to study:

- healthcare facility location;
- candidate community access-site placement for assisted-telehealth studies;
- hybrid physical–digital healthcare networks;
- healthcare accessibility;
- service allocation;
- capacitated location-allocation problems;
- equity-aware healthcare planning;
- digital exclusion and healthcare access;
- coverage models;
- multiobjective healthcare network design; and
- sensitivity of healthcare-network decisions to digital vulnerability.

---

## 2. Important Interpretation of the Dataset

HybPriCare-QJM should be interpreted as an **empirical base dataset**, not as a collection of pre-generated optimization instances.

The dataset contains geographical demand, existing healthcare facilities, potential community access locations, area-level digital-vulnerability information, and accessibility matrices.

Researchers may generate multiple planning or optimization instances from this same empirical base by introducing model-specific assumptions such as:

- the number of candidate sites that may be activated;
- service capacities;
- telehealth eligibility;
- virtual-clinician capacity;
- accessibility thresholds;
- digital-access assumptions;
- budget limits;
- equity requirements; or
- objective-function parameters.

Such planning parameters are deliberately not embedded as observed facts in the dataset because they depend on the particular decision problem being investigated.

---

## 3. Repository Contents

This repository contains data, metadata, and documentation only. Dataset
construction software and application-specific optimization programs are not
included in this package. The 15 release files have the following structure,
with the documentation and citation files at the repository root:

```text
HybPriCare-QJM/
|-- README.md
|-- DATA_DICTIONARY.md
|-- LICENSES.md
|-- CITATION.cff
|-- data/
|   |-- demand_nodes.csv
|   |-- gp_facilities.csv
|   |-- telehealth_candidates.csv
|   |-- distance_demand_gp.csv
|   |-- distance_demand_hub.csv
|   |-- road_snapping.csv
|   `-- dvi_national_reference.csv
|-- metadata/
|   |-- instance_metadata.json
|   |-- qa_report.json
|   `-- source_manifest.csv
`-- checksums/
    `-- SHA256SUMS.txt
```

- `README.md`, `DATA_DICTIONARY.md`, `LICENSES.md`, and `CITATION.cff` provide repository documentation, variable definitions, source-attribution notes, licensing information, and citation metadata.
- `data/` contains the seven derived, model-ready CSV files described below.
- `metadata/` contains machine-readable instance metadata, quality-assurance output, and source-manifest information.
- `checksums/SHA256SUMS.txt` provides SHA-256 checksums for the deposited files.

### Getting started

Download the GitHub repository using **Code > Download ZIP**, or download the
dataset files from the Zenodo record. For reproducible
reuse, select the versioned `v1.0` GitHub release/tag or the corresponding
Zenodo version rather than relying on a changing default branch.

After extraction, open the directory containing `README.md` and `data/`. Read
`DATA_DICTIONARY.md` for variable definitions and `LICENSES.md` for the dataset
licence and required upstream attributions. No solver or dataset-generation
software is needed to read the deposited tables.

On systems with GNU Coreutils, verify the files from that directory with:

```bash
sha256sum --check checksums/SHA256SUMS.txt
```

The manifest covers all 14 other release files and does not hash itself.

### Coordinates, units, and table linkage

Coordinate and distance conventions are as follows: British National Grid
eastings and northings are projected coordinates in metres using EPSG:27700;
latitude and longitude are WGS84 geographic coordinates using EPSG:4326;
pairwise road-network and Euclidean distances are reported in kilometres; and
point-to-network snapping distances are reported in metres.

The CSV files are UTF-8 encoded, comma-delimited tables with a header row.
Treat geographical and facility identifiers as strings. Join demand records
using `lsoa21cd`, GP records using `gp_id`, and candidate-site records using
`hub_id`. Each distance table contains every demand-facility pair for its
corresponding facility set. In `road_snapping.csv`, interpret `entity_id`
together with `entity_type`.

The main model-ready files in `data/` are described below.

### `demand_nodes.csv`

Contains one record for each of the 435 demand zones.

Demand zones correspond to **2021 Lower-layer Super Output Areas (LSOAs)**.

The file contains information including:

- LSOA identifier;
- aggregate registered-patient demand;
- geographical coordinates;
- raw connectivity-related information;
- income-vulnerability information;
- adult-skills vulnerability;
- older-population information;
- nationally normalized vulnerability components; and
- the composite Digital Vulnerability Index.

The precise column definitions and units are provided in the accompanying data dictionary.

---

### `gp_facilities.csv`

Contains the **81 existing GP practices** associated with NHS Lincolnshire ICB.

The file includes information such as:

- NHS practice identifier;
- postcode;
- geographical coordinates;
- associated statistical geography; and
- reconstructed registered list size.

Practice names are omitted from the public table because the stable NHS
practice identifier is sufficient for joining the facility and distance
tables and names are not required for the intended spatial analyses.

The GP practices represent existing physical primary-care facilities. They are not candidate locations.

Registered list size should not automatically be interpreted as operational treatment capacity. Actual GP capacity also depends on staffing, appointment types, working patterns, and other operational factors that are outside the scope of this dataset.

---

### `telehealth_candidates.csv`

Contains **109 geographically distinct physical locations derived from NHSBSA
records classified as pharmacy contractors**.

The original NHS Business Services Authority extraction contained 112 eligible contractor records. Some administrative records corresponded to the same geographical location. Records with identical geocoded British National Grid coordinates were therefore consolidated into a single geographical candidate site.

Source contractor names and contractor codes are deliberately omitted from
the public candidate-site table because they are not required for spatial
analysis and could incorrectly imply participation in or endorsement of
assisted-telehealth research. The consolidation count is retained to document
how many source records were represented by each geographical location.

Seven eligible source records were flagged as out-of-hours dispensing
contractors in the NHSBSA source and correspond to six unique locations after
geographical consolidation. They are retained because the released empirical
instance was defined by physical location and the stated pharmacy-type,
private-contractor, and distance-selling filters. Their inclusion does not
indicate service suitability, availability, or willingness to participate.

The term **candidate site** does not imply that any represented contractor or
location currently provides assisted telehealth services.

Rather, these locations provide a real-world set of geographically distributed community access points that may be used in prospective planning studies involving, for example:

- assisted telehealth;
- digitally supported healthcare;
- community diagnostics;
- healthcare outreach;
- community-based service access; or
- other location-allocation applications.

---

### `distance_demand_gp.csv`

Contains the complete accessibility matrix between the 435 demand zones and 81 GP practices.

Number of demand–GP pairs:

```math
435 \times 81 = 35,235.
```

Both road-network and Euclidean accessibility measures are retained.

---

### `distance_demand_hub.csv`

Contains the complete accessibility matrix between the 435 demand zones and 109 candidate community-access sites.

Number of demand–candidate-site pairs:

```math
435 \times 109 = 47,415.
```

Both road-network and Euclidean accessibility measures are retained.

---

### `road_snapping.csv`

Contains information about the association of each geographical demand or facility point with the retained OS Open Roads routing network.

The file records point-to-network snapping information for:

- demand centroids;
- GP practices; and
- candidate community-access sites.

---

### `dvi_national_reference.csv`

Contains the England-wide reference information used to calibrate the Digital Vulnerability Index.

The normalization reference consists of all:

```math
33,755
```

2021 LSOAs in England.

This national normalization makes the DVI values comparable with values that could be generated for other English ICBs using the same methodology.

---

### `metadata/source_manifest.csv`

Contains provenance information for the public source datasets used to construct HybPriCare-QJM.

Depending on the source, this includes information such as:

- source identifier;
- source version, release date, or snapshot description;
- source access URL or service endpoint;
- local source file or cache name used during construction; and
- SHA-256 checksum or provenance note;
- upstream licence identifier;
- required source attribution; and
- source access date.

---

### `metadata/instance_metadata.json`

Provides machine-readable metadata describing the final empirical instance, including:

- dataset name, title, version, and historical build timestamp;
- geographical identifiers and coordinate-reference systems;
- distance units;
- DVI definition, weights, and percentile conventions;
- candidate-site eligibility, consolidation, and public-identifier policies;
- road-network characteristics;
- overall quality-assurance status.

Entity counts, aggregate demand, DVI summaries, source-field choices, and
distance-matrix completeness are reported in `metadata/qa_report.json`.
Source provenance and attribution are recorded in
`metadata/source_manifest.csv`.

---

### `metadata/qa_report.json`

Contains automated quality-assurance results for the final dataset.

Checks include:

- uniqueness of demand identifiers;
- uniqueness of GP identifiers;
- uniqueness of candidate-site identifiers;
- uniqueness of candidate-site coordinates;
- positivity of demand;
- completeness of geographical coordinates;
- validity of normalized DVI components;
- completeness of distance matrices;
- nonnegativity of distances;
- road-routing completeness; and
- road-network extraction integrity.

---

### Documentation files

The repository also includes:

- `README.md` — this document;
- `DATA_DICTIONARY.md` — definitions and units for the deposited variables;
- `LICENSES.md` — source attribution and licensing information;
- `CITATION.cff` — machine-readable citation information;
- `metadata/instance_metadata.json` — machine-readable metadata for the released empirical instance;
- `metadata/qa_report.json` — automated quality-assurance output;
- `metadata/source_manifest.csv` — source provenance and licensing metadata; and
- `checksums/SHA256SUMS.txt` — SHA-256 checksums for the deposited files.

---

## 4. Primary Data Sources

HybPriCare-QJM was constructed from publicly accessible official data sources.

### 4.1 NHS England — Registered Patients

**Source:** Patients Registered at a GP Practice  
**Release used:** July 2026  
**Publisher:** NHS England

Used for:

- GP practice identifiers;
- GP practice mapping;
- practice-by-LSOA registered-patient counts; and
- reconstruction of practice registered list sizes.

Source:

https://digital.nhs.uk/data-and-information/publications/statistical/patients-registered-at-a-gp-practice/july-2026

---

### 4.2 NHS Business Services Authority — Contractor Details

**Source:** Contractor Details  
**Release used:** July 2026  
**Publisher:** NHS Business Services Authority

Used to identify physical NHSBSA pharmacy-type contractor locations within NHS
Lincolnshire ICB.

Source:

https://opendata.nhsbsa.net/dataset/contractor-details

---

### 4.3 English Indices of Deprivation 2025

**Source:** English Indices of Deprivation 2025  
**Component used:** File 7, including ranks, scores, deciles, and population denominators  
**Publisher:** Ministry of Housing, Communities and Local Government

Used for:

- income vulnerability;
- Adult Skills vulnerability;
- total population; and
- population aged 60 years or older.

Source:

https://www.gov.uk/government/statistics/english-indices-of-deprivation-2025

---

### 4.4 Ofcom Connected Nations

**Source:** Connected Nations Update: Spring 2026  
**Publisher:** Ofcom

Used to construct broadband-connectivity vulnerability.

The selected indicator is the percentage of premises unable to receive decent broadband from fixed or fixed-wireless-access services.

Output-Area values were aggregated to LSOA level using premises-weighted averaging.

Source:

https://www.ofcom.org.uk/phones-and-broadband/coverage-and-speeds/connected-nations-update-spring-2026

---

### 4.5 National Statistics Postcode Lookup

**Source:** National Statistics Postcode Lookup  
**Release used:** May 2026  
**Publisher:** Office for National Statistics

Used for:

- postcode geocoding;
- Output Area identifiers;
- LSOA identifiers;
- ICB geography identifiers;
- British National Grid coordinates; and
- latitude/longitude information.

---

### 4.6 ONS OA-to-LSOA Lookup

The ONS 2021 Output Area to LSOA exact-fit lookup was used to aggregate Output-Area broadband information to 2021 LSOAs.

---

### 4.7 ONS LSOA Population-Weighted Centroids

Official 2021 LSOA population-weighted centroids were used as representative spatial locations for aggregate demand zones.

These centroids should be interpreted as statistical representative points rather than individual residential addresses.

---

### 4.8 OS Open Roads

**Publisher:** Ordnance Survey  
**Dataset:** OS Open Roads  
**Reference release:** 2026

Used to derive shortest-path road-network accessibility between demand zones and healthcare/community access locations.

Source:

https://www.ordnancesurvey.co.uk/products/os-open-roads

---

## 5. Construction of Demand

Let $n_{pi}$ denote the number of patients registered with GP practice $p$ who reside in LSOA $i$.

Demand associated with LSOA $i$ is defined as:

```math
d_i = \sum_p n_{pi}.
```

The summation is performed across the GP practices represented in the national NHS registered-patient source rather than being restricted to practices administratively assigned to Lincolnshire ICB.

This is important because a resident of a Lincolnshire LSOA may be registered with a GP practice outside the ICB boundary.

The resulting geographical demand representation contains:

```math
|I| = 435
```

LSOAs and total registered-patient demand:

```math
\sum_{i\in I} d_i = 822,413.
```

Demand locations are represented by ONS population-weighted LSOA centroids.

---

## 6. Construction of Existing GP Facilities

Existing GP practices were identified using the NHS England practice mapping and NHS ICB code:

`QJM`

The final facility set contains:

```math
|J| = 81.
```

For GP practice $j$, registered list size was reconstructed as:

```math
Q_j^{\mathrm{reg}}
=
\sum_i n_{ji}.
```

This variable is retained as an empirical indicator of practice scale.

It should not be interpreted automatically as a fixed operational capacity.

---

## 7. Construction of Candidate Community-Access Sites

Pharmacy-type contractor records were obtained from the NHSBSA Contractor
Details dataset.

Candidate records were restricted to records associated with NHS Lincolnshire
ICB for which `PHARMACY_TYPE` was `PHARMACY`. Private-contractor and
distance-selling records were excluded. Out-of-hours dispensing records were
not excluded; seven such source records are represented by six consolidated
candidate locations.

These filters define a reproducible set of physical candidate locations; they
do not establish operational suitability for any particular healthcare
service.

The source extraction produced:

```math
112
```

eligible administrative pharmacy-type contractor records.

Because multiple contractor records can represent the same physical location, geographically colocated records were identified using geocoded British National Grid coordinates.

After consolidation:

```math
|K| = 109
```

unique geographical candidate sites remained.

The consolidation prevents artificial duplicate location decisions in downstream facility-location or network-design models.

---

## 8. Digital Vulnerability Index

Each demand zone is associated with an area-level Digital Vulnerability Index:

```math
R_i \in [0,1].
```

The DVI is constructed from four dimensions:

1. broadband-connectivity vulnerability;
2. income vulnerability;
3. adult-skills vulnerability; and
4. age-related vulnerability.

The DVI is a **relative area-level indicator**.

It is **not**:

- the percentage of people digitally excluded;
- the probability that an individual cannot use telehealth;
- an individual-level patient characteristic; or
- a clinically validated individual risk score.

Empirical percentiles were computed using average ranks for tied observations,
equivalent to `pandas` `rank(method="average")`. For the income and
older-share components, the ascending average rank $r_i$ was scaled as
$(r_i-1)/(N-1)$ over all $N=33,755$ English 2021 LSOAs. For the
zero-preserving connectivity component, values with $C_i=0$ were fixed at
0; strictly positive values were average-ranked among the $n_+=2,535$
positive-risk English LSOAs and scaled as $r_i/n_+$.

For numerical reconstruction from the deposited CSV files, preserve the
floating-point precision of the raw component values before ranking. For
example, use `pandas.read_csv(path, engine="c", float_precision="round_trip")`
with pandas. Default parsing or spreadsheet rounding can merge distinct,
nearly equal values into artificial ties and slightly change recomputed
percentiles. Use the supplied normalized components when recalibration is
not required.

---

### 8.1 Connectivity Vulnerability

Let $c_o$ denote the proportion of premises in Output Area $o$ unable to receive decent broadband from fixed or fixed-wireless-access services, and let $q_o$ denote the number of premises.

The raw LSOA connectivity measure is:

```math
C_i
=
\frac{
\sum_{o\in\mathcal{O}_i}q_oc_o
}{
\sum_{o\in\mathcal{O}_i}q_o
}.
```

Because a large proportion of English LSOAs have zero values for the selected connectivity-risk indicator, zero values are preserved explicitly.

The normalized connectivity component is:

```math
C_i^*
=
\begin{cases}
0, & C_i=0,\\
P_+(C_i), & C_i>0,
\end{cases}
```

where $P_+$ denotes the empirical percentile among English LSOAs with positive connectivity risk.

The national reference contains:

```math
2,535
```

LSOAs with strictly positive values for this indicator.

---

### 8.2 Income Vulnerability

Let $Y_i$ denote the Income Score from the English Indices of Deprivation 2025.

The nationally normalized income component is:

```math
I_i^* = P(Y_i),
```

where $P(\cdot)$ is the empirical percentile across all English 2021 LSOAs.

---

### 8.3 Adult-Skills Vulnerability

Let $r_i^S$ denote the Adult Skills Sub-domain Rank, where rank 1 corresponds to the most deprived LSOA.

The normalized skills component is:

```math
S_i^*
=
1-\frac{r_i^S-1}{N-1},
```

where:

```math
N = 33,755
```

is the number of English 2021 LSOAs.

---

### 8.4 Age-Related Vulnerability

Let:

```math
A_i
=
\frac{
\text{population aged 60+ in LSOA }i
}{
\text{total population in LSOA }i
}.
```

The nationally normalized age component is:

```math
O_i^*=P(A_i).
```

---

### 8.5 Composite DVI

The baseline composite index is:

```math
R_i
=
\frac{1}{4}
\left(
C_i^*
+
I_i^*
+
S_i^*
+
O_i^*
\right).
```

For the 435 Lincolnshire demand zones:

```math
\min_i R_i = 0.0167,
```

```math
\overline{R}=0.4676,
```

and

```math
\max_i R_i = 0.8384.
```

The individual raw and normalized components are retained to allow researchers to construct alternative vulnerability measures without being required to use the equal-weight composite.

---

## 9. Road-Network Accessibility

Two accessibility measures are retained:

- Euclidean distance; and
- shortest-path road-network distance.

Road-network distance is the primary measure intended for healthcare-accessibility and location-allocation applications.

---

## 10. OS Open Roads Extraction

A British National Grid bounding box extending 40 km beyond the study locations was used to define the road-network extraction.

An independent server-side spatial count reported:

```math
404,625
```

Road Link records intersecting the selected bounding box.

The final road cache also contained exactly:

```math
404,625
```

Road Links.

Thus:

```text
Expected spatial Road Links: 404625
Retrieved Road Links:        404625
Object-ID completeness:      True
```

Of these links:

```math
402,387
```

contained usable geometry and length information for routing.

---

## 11. Road-Network Topology

Road topology was reconstructed from the endpoints of the published OS Open Roads polylines in British National Grid coordinates.

Endpoint coordinates were matched using a tolerance of:

```math
0.1\text{ m}.
```

Only road-polyline endpoints create graph junctions. Geometric crossings occurring solely within the interior of two road segments were not automatically converted into network connections. This avoids artificial connections at grade-separated crossings.

The resulting network contained:

| Network property | Value |
|---|---:|
| Road Links extracted | 404,625 |
| Usable Road Links | 402,387 |
| Road nodes across all components | 346,460 |
| Connected components | 231 |
| Nodes in largest connected component | 345,332 |
| Largest-component share | 99.67% |
| Routing edges | 396,068 |

The largest connected component was retained for routing.

---

## 12. Point-to-Network Snapping

Each:

- LSOA demand centroid;
- GP practice; and
- candidate community-access site

was associated with the nearest node of the retained road network.

The resulting snapping statistics are:

| Entity | Mean (m) | Median (m) | 90th pct. (m) | 95th pct. (m) | 99th pct. (m) | Maximum (m) |
|---|---:|---:|---:|---:|---:|---:|
| Demand centroid | 69.90 | 40.68 | 124.01 | 264.52 | 499.24 | 814.52 |
| GP practice | 41.77 | 37.44 | 70.61 | 84.14 | 103.45 | 113.23 |
| Candidate site | 42.55 | 34.53 | 75.40 | 91.36 | 109.35 | 133.32 |

The larger upper-tail snapping distances for some demand nodes arise because LSOA population-weighted centroids are statistical representative points rather than physical addresses.

---

## 13. Definition of Road Distance

Let $v(a)$ denote the road-network node associated with geographical point $a$, and let $s_a$ denote the point-to-network snapping distance.

For two geographical points $a$ and $b$, road-network accessibility is defined as:

```math
d^R(a,b)
=
s_a
+
\mathrm{dist}_G(v(a),v(b))
+
s_b,
```

where:

```math
\mathrm{dist}_G(v(a),v(b))
```

is the shortest-path distance between the corresponding road-network nodes.

The Euclidean distance:

```math
d^E(a,b)
```

is retained separately.

---

## 14. Accessibility Validation

Road distances were compared independently with their corresponding Euclidean distances.

### Demand–GP pairs

Number of pairs:

```math
35,235.
```

Road-to-Euclidean distance ratio:

| Statistic | Value |
|---|---:|
| Mean | 1.209 |
| Median | 1.187 |
| 90th percentile | 1.302 |
| 95th percentile | 1.367 |
| 99th percentile | 1.675 |

---

### Demand–candidate-site pairs

Number of pairs:

```math
47,415.
```

Road-to-Euclidean distance ratio:

| Statistic | Value |
|---|---:|
| Mean | 1.205 |
| Median | 1.179 |
| 90th percentile | 1.299 |
| 95th percentile | 1.374 |
| 99th percentile | 1.722 |

For every pair in both matrices:

```math
d^R(a,b)\ge d^E(a,b).
```

The final validation therefore returned:

```text
Demand–GP Road < Euclidean:   0
Demand–hub Road < Euclidean:  0
```

There are no missing road-network distances in either matrix.

---

## 15. Quality Assurance

The frozen HybPriCare-QJM v1.0 dataset passed all automated validation checks.

The final checks include:

```text
unique_demand_ids:                 True
unique_gp_ids:                     True
unique_hub_ids:                    True
unique_hub_coordinates:            True
positive_demand:                   True
dvi_components_in_0_1:             True
national_reference_is_england:     True
national_reference_large_enough:   True
nonnegative_euclidean_gp:          True
nonnegative_euclidean_hub:         True
complete_demand_gp_matrix:         True
complete_demand_hub_matrix:        True
metadata_qa_passed:                True
nonnegative_road_gp:               True
nonnegative_road_hub:              True
no_missing_road_gp:                True
no_missing_road_hub:               True
```

Road-network extraction completeness was additionally verified independently:

```text
Expected spatial links: 404625
Cached links:           404625
Object-ID completeness: True
```

---

## 16. Recommended Use

HybPriCare-QJM may be used directly as an empirical input to optimization and spatial-analysis models.

A generic model may define:

```math
I=\text{demand zones},
```

```math
J=\text{existing GP practices},
```

and

```math
K=\text{candidate community-access sites}.
```

The dataset provides the empirical quantities associated with these sets, including:

- demand;
- geographical coordinates;
- digital vulnerability;
- existing facility locations;
- candidate locations; and
- complete accessibility matrices.

Researchers should introduce application-specific planning parameters separately.

For example, a healthcare-network model may introduce parameters describing:

- the number of sites to open;
- service capacity;
- telehealth suitability;
- virtual-clinician availability;
- accessibility standards;
- equity requirements; or
- budget restrictions.

These quantities are **not observed properties of HybPriCare-QJM** and should be clearly identified as scenario or modelling assumptions in downstream studies.

---

## 17. Limitations

Several limitations should be considered when using the dataset.

### Aggregate demand

Registered-patient counts represent aggregate primary-care registration rather than observed appointments, healthcare visits, or disease-specific demand.

### Spatial representation of demand

Demand is represented using LSOA population-weighted centroids rather than individual patient addresses.

### Candidate-site interpretation

The 109 physical NHSBSA pharmacy-type contractor locations are candidate
geographical access sites. They include six locations represented by source
records flagged as out-of-hours dispensing contractors.

The dataset does not assert that the represented contractors or locations:

- currently provide telehealth;
- have telehealth equipment;
- have suitable consultation rooms;
- have available staff capacity; or
- would participate in a telehealth programme.

### GP capacity

Registered GP list size is not equivalent to observed operational service capacity.

### Digital Vulnerability Index

The DVI is an area-level relative indicator and must not be interpreted as an individual-level measure or probability of digital exclusion.

### Accessibility

Road-network distance represents geographical distance rather than actual travel time.

The dataset does not directly account for:

- congestion;
- public transport;
- walking accessibility;
- road speed;
- weather;
- temporal accessibility; or
- individual mobility limitations.

### Geographical scope

HybPriCare-QJM represents one NHS Integrated Care Board.

Results obtained using this dataset should not automatically be generalized to all English healthcare regions.

---

## 18. Ethics and Privacy

HybPriCare-QJM was constructed from publicly available aggregate
administrative and geospatial data. It contains no patient-level personal
data.

No:

- individual-level patient records;
- clinical records;
- patient names;
- individual residential addresses;
- contact details; or
- other directly identifiable patient information

are included in the dataset.

No human participants were recruited, contacted, surveyed, or subjected to an
intervention during dataset construction.

Registered-patient demand is represented through publicly available aggregate information.

No ethical review was sought because the author determined that none of the
applicable criteria requiring prior ethical review were triggered. Informed
consent was not applicable because no participants were recruited and no
individual-level patient data were collected or processed.

---

## 19. Source Licensing and Attribution

HybPriCare-QJM is a derived dataset constructed from multiple public administrative and geospatial sources.

Users should distinguish between:

1. the derived HybPriCare-QJM dataset; and
2. the underlying primary datasets maintained by their respective publishers.

The primary datasets remain subject to their original licences and attribution requirements.

Detailed source attribution and applicable licensing information are provided in:

`LICENSES.md`

and:

`metadata/source_manifest.csv`

The repository does not redistribute all upstream national source files.

**Dataset release licence:** Creative Commons Attribution 4.0 International
(CC BY 4.0; https://creativecommons.org/licenses/by/4.0/).

This licence applies to the compilation, selection, arrangement,
documentation, and author-created derived content of HybPriCare-QJM only to
the extent that the author holds copyright or database rights in those
materials. It does not replace, restrict, or supersede the licences,
attribution requirements, or other rights associated with the underlying
source data. Names and marks appearing in the documentation identify public
data providers and source datasets only. They do not indicate participation
in, endorsement of, or validation of HybPriCare-QJM by the named
organizations. Third-party names, identifiers, and trademarks remain the
property of their respective owners, and no trademark rights are granted.

Users redistributing or extending the dataset should preserve the required attribution to the relevant primary data providers.

### As-is and operational-use notice

HybPriCare-QJM is provided **as is**, without warranties or representations
of accuracy, completeness, currency, fitness for a particular purpose, or
continued availability of any facility or candidate location. Facility
identifiers, locations, classifications, and service status may change after
the source snapshots used for this release. Before any operational, clinical,
commissioning, investment, or policy use, users must independently verify the
current existence, status, eligibility, capacity, and willingness to
participate of every relevant facility or location. The dataset is a research
resource and does not constitute operational or clinical advice.

---

## 20. Versioning

### HybPriCare-QJM v1.0

This is the first frozen public release of the dataset.

The v1.0 release is based on the validated reference instance with:

```text
ICB:                              QJM
ONS ICB:                          E54000013
Demand zones:                     435
Registered-patient demand:        822413
GP practices:                     81
Candidate sites:                  109
Demand–GP pairs:                  35235
Demand–candidate-site pairs:      47415
Expected OS Open Roads links:     404625
Retrieved OS Open Roads links:    404625
Missing road-distance pairs:      0
```

Future corrections or extensions should be released under a new version number rather than silently replacing the v1.0 data.

---

## 21. Citation

If you use HybPriCare-QJM in academic research, cite the dataset version used
with its Zenodo DOI. In studies using the GitHub copy, also identify the
release tag or commit identifier.

### Zenodo archival citation

> Atta, S. (2026). *HybPriCare-QJM v1.0: An Integrated Geospatial Dataset for Hybrid Primary-Care Network Design and Digital Vulnerability Analysis*. Zenodo. https://doi.org/10.5281/zenodo.22232373

### BibTeX

```bibtex
@dataset{atta2026hybpricare,
  author    = {Atta, Soumen},
  title     = {HybPriCare-QJM v1.0: An Integrated Geospatial Dataset for Hybrid Primary-Care Network Design and Digital Vulnerability Analysis},
  year      = {2026},
  publisher = {Zenodo},
  version   = {1.0},
  doi       = {10.5281/zenodo.22232373},
  url       = {https://doi.org/10.5281/zenodo.22232373}
}
```

If a peer-reviewed Data in Brief article describing HybPriCare-QJM becomes available, users are encouraged to cite both the dataset DOI and the corresponding data article where appropriate.

---

## 22. Related Publications

### Data article

A dedicated data article describing the construction, contents, validation,
and reuse potential of HybPriCare-QJM is being prepared for *Data in Brief*.
No published article citation is available for this release. The dataset is
a standalone research resource and can be reused independently of that
article.

### Operations-research application

The dataset is also intended to support research on hybrid primary-care network design and community access-site planning for assisted telehealth.

Any optimization models, planning scenarios, or computational experiments developed using the dataset are separate research outputs and are not part of the empirical HybPriCare-QJM v1.0 dataset itself.

---

## 23. Contact

For questions concerning the dataset:

**Soumen Atta**  
Faculty of Information Technology  
University of Jyväskylä  
Finland  

Email: soumen.s.atta@jyu.fi

---

## 24. Disclaimer

HybPriCare-QJM is a research dataset intended for scientific and methodological use.

The candidate-site designation is a modelling construct and should not be
interpreted as an endorsement, commitment, or indication that any represented
contractor, location, or healthcare provider currently offers or intends to
offer assisted telehealth services.

The dataset is not intended for individual clinical decision-making or for identifying the healthcare or digital-access status of individual persons.
