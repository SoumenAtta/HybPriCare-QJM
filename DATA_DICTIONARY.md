# Data dictionary

This file defines the variables in the tabular files deposited with
HybPriCare-QJM v1.0.

General conventions:

- CSV files use UTF-8 encoding, commas as delimiters, and a header row.
  Geographical and facility identifiers should be read as strings.
- British National Grid eastings and northings are projected coordinates in
  metres using EPSG:27700.
- Latitude and longitude are WGS84 geographic coordinates using EPSG:4326.
- Pairwise road-network and Euclidean distance columns (`road_distance_km`,
  `euclidean_distance_km`, and `distance_km`) are in kilometres.
- Point-to-network snapping distance columns (`demand_snap_m`, `gp_snap_m`,
  `hub_snap_m`, and `snap_distance_m`) are in metres.
- Normalized vulnerability components and the composite Digital Vulnerability
  Index are scaled to the interval [0, 1], where larger values indicate higher
  relative vulnerability.
- Preserve raw floating-point precision when reconstructing percentile ranks.
  For example, use `pandas.read_csv(path, engine="c", float_precision="round_trip")`
  with pandas to avoid creating artificial ties between nearly equal values.
  Supplied normalized components can be used directly without recalibration.

## data/demand_nodes.csv

One row per 2021 Lower-layer Super Output Area (LSOA) demand zone in the
study area.

- `lsoa21cd`: 2021 LSOA code used as the demand-node identifier.
- `registered_patients`: aggregate registered-patient demand assigned to the
  LSOA.
- `demand_weight`: share of total study-region registered-patient demand
  represented by the LSOA.
- `digital_vulnerability_index`: equal-weight composite of the normalized
  connectivity, income, adult-skills, and older-population vulnerability
  components.
- `connectivity_risk_raw`: raw fixed-broadband connectivity-risk measure
  aggregated from Output Area information to the LSOA using premises weights;
  a proportion in [0, 1], not a percentage in [0, 100].
- `income_risk_raw`: IMD 2025 Income Score used as the raw income-vulnerability
  measure.
- `adult_skills_rank`: IMD 2025 adult-skills rank; lower ranks indicate higher
  deprivation in the source ranking.
- `older_share_raw`: share of the population aged 60 years or over.
- `connectivity_vulnerability`: nationally scaled connectivity-vulnerability
  component.
- `income_vulnerability`: nationally scaled income-vulnerability component.
- `adult_skills_vulnerability`: nationally scaled adult-skills vulnerability
  component, reverse-scaled so larger values indicate higher vulnerability.
- `older_share_vulnerability`: nationally scaled older-population component.
- `oa_count`: number of Output Areas contributing to the LSOA-level
  connectivity aggregation.
- `imd_population_mid2022`: IMD 2025 population denominator for the LSOA.
- `older_population_60plus_mid2022`: population aged 60 years or over used to
  compute `older_share_raw`.
- `easting`: British National Grid easting of the LSOA population-weighted
  centroid.
- `northing`: British National Grid northing of the LSOA population-weighted
  centroid.
- `latitude`: WGS84 latitude of the demand-node centroid.
- `longitude`: WGS84 longitude of the demand-node centroid.

## data/gp_facilities.csv

One row per existing GP practice associated with NHS Lincolnshire ICB in the
source mapping.

- `gp_id`: NHS GP practice code.
- `postcode`: practice postcode.
- `lsoa21cd`: 2021 LSOA code associated with the practice postcode.
- `registered_list_size`: reconstructed registered list-size measure for the
  practice.
- `easting`: British National Grid easting of the practice location.
- `northing`: British National Grid northing of the practice location.
- `latitude`: WGS84 latitude of the practice location.
- `longitude`: WGS84 longitude of the practice location.

Practice names are omitted from the public table because they are not needed
for spatial analysis or linkage to the deposited distance matrix. The stable
`gp_id` remains the facility identifier.

## data/telehealth_candidates.csv

One row per geographically distinct physical location derived from eligible
NHSBSA pharmacy-type contractor records and represented as a candidate access
site for planning studies. Seven source records flagged as out-of-hours
dispensing contractors are represented by six of the 109 consolidated
locations.

- `hub_id`: candidate-site identifier used in the deposited distance matrix.
- `source_record_count`: number of source NHSBSA records consolidated into the
  candidate site.
- `postcode`: candidate-site postcode.
- `lsoa21cd`: 2021 LSOA code associated with the candidate-site postcode.
- `easting`: British National Grid easting of the candidate site.
- `northing`: British National Grid northing of the candidate site.
- `latitude`: WGS84 latitude of the candidate site.
- `longitude`: WGS84 longitude of the candidate site.

Source contractor names and contractor codes are omitted from the public
table because they are not required for spatial reuse. The author-created
`hub_id` is the candidate-site identifier, while `source_record_count`
preserves the information needed to interpret geographical consolidation.

## data/distance_demand_gp.csv

One row per demand-zone and GP-practice pair.

- `lsoa21cd`: 2021 LSOA demand-node identifier.
- `gp_id`: NHS GP practice identifier.
- `road_distance_km`: shortest-path road-network distance between the demand
  node and GP practice.
- `demand_snap_m`: distance from the demand-node centroid to its snapped road
  node.
- `gp_snap_m`: distance from the GP practice to its snapped road node.
- `euclidean_distance_km`: straight-line distance between the demand node and
  GP practice.
- `distance_km`: canonical distance field equal to `road_distance_km`.

## data/distance_demand_hub.csv

One row per demand-zone and candidate-site pair.

- `lsoa21cd`: 2021 LSOA demand-node identifier.
- `hub_id`: candidate-site identifier.
- `road_distance_km`: shortest-path road-network distance between the demand
  node and candidate site.
- `demand_snap_m`: distance from the demand-node centroid to its snapped road
  node.
- `hub_snap_m`: distance from the candidate site to its snapped road node.
- `euclidean_distance_km`: straight-line distance between the demand node and
  candidate site.
- `distance_km`: canonical distance field equal to `road_distance_km`.

## data/road_snapping.csv

One row per demand node, GP practice, or candidate site.

- `entity_type`: entity class; `demand`, `gp`, or `hub` for a demand zone, GP
  practice, or candidate site, respectively.
- `entity_id`: identifier of the demand node, GP practice, or candidate site.
- `road_node_id`: identifier of the retained OS Open Roads network node to
  which the entity was snapped. This is an endpoint-derived identifier used
  by the constructed routing graph, not an official OS road-link identifier;
  the underlying road graph is not included in this data-only release.
- `snap_distance_m`: point-to-network snapping distance in metres.

## data/dvi_national_reference.csv

England-wide 2021 LSOA reference table used to scale the Digital
Vulnerability Index components.

- `lsoa21cd`: 2021 LSOA code.
- `income_risk_raw`: IMD 2025 Income Score used as the raw
  income-vulnerability measure.
- `adult_skills_rank`: IMD 2025 adult-skills rank; lower ranks indicate higher
  deprivation in the source ranking.
- `imd_population_mid2022`: IMD 2025 population denominator for the LSOA.
- `older_population_60plus_mid2022`: population aged 60 years or over used to
  compute `older_share_raw`.
- `older_share_raw`: share of the population aged 60 years or over.
- `connectivity_risk_raw`: raw fixed-broadband connectivity-risk measure
  aggregated from Output Area information to the LSOA using premises weights;
  a proportion in [0, 1], not a percentage in [0, 100].
- `oa_count`: number of Output Areas contributing to the LSOA-level
  connectivity aggregation.
- `connectivity_vulnerability`: nationally scaled connectivity-vulnerability
  component.
- `income_vulnerability`: nationally scaled income-vulnerability component.
- `adult_skills_vulnerability`: nationally scaled adult-skills vulnerability
  component, reverse-scaled so larger values indicate higher vulnerability.
- `older_share_vulnerability`: nationally scaled older-population component.
- `digital_vulnerability_index`: equal-weight composite of the normalized
  vulnerability components.

## metadata/source_manifest.csv

One row per public source dataset or source service used during construction.

- `source_id`: short internal identifier for the source.
- `snapshot`: source version, release date, or snapshot description used.
- `url`: source access URL or service endpoint.
- `raw_file`: local source file or cache name used during dataset construction.
- `sha256`: SHA-256 checksum for the local source file, or a provenance note
  when the source was obtained from a cached service query.
- `licence_identifier`: machine-readable identifier for the upstream source
  licence.
- `attribution`: source-specific attribution statement to retain when reusing
  the derived data.
- `access_date`: date on which the source file or service was accessed, in
  ISO 8601 date format.

## metadata/instance_metadata.json

Machine-readable metadata for the released empirical instance, including the
dataset name and version, study-area identifiers, DVI definition and weights,
pharmacy consolidation rule, road-network extraction characteristics, and
quality-assurance status.

## metadata/qa_report.json

Machine-readable quality-assurance report summarizing entity counts, demand
totals, DVI ranges, source-field choices, road-network diagnostics,
point-to-network snapping maxima, and completeness of the road-distance
matrices.
