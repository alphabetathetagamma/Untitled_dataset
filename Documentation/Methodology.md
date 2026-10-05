# PRABASI Methodology

## 1. Overview

PRABASI is a multi-decadal dataset constructed by integrating Census interstate migration observations with state-level demographic and socio-economic indicators for India.

The dataset was constructed to support computational analysis of interstate migration using machine-learning and related analytical approaches. The construction workflow consisted of:

1. extraction of interstate migration observations from Census migration tables;
2. selection of the relevant migration-duration category;
3. collection of candidate state-level demographic and socio-economic indicators;
4. selection of indicators relevant to interstate migration modelling;
5. harmonization of state and Union Territory nomenclature;
6. harmonization of state/Union Territory identifiers;
7. treatment of missing observations;
8. construction of directed source-destination records;
9. computation of geographic distance between state/Union Territory centroids; and
10. preparation of the resulting records for downstream machine-learning experiments.

Dataset construction and downstream machine-learning feature engineering are treated as separate stages.

---

## 2. Migration Data

### 2.1 Census source

The migration component of PRABASI is derived from Census of India migration data, particularly the D-series tables and the D-02 tables.

The D-02 tables provide migration observations classified by source/place of last residence, destination/place of enumeration, sex, and duration of stay.

The selected duration category for PRABASI is:

**Less than one year**

The migration count therefore represents the number of persons corresponding to a specified source, destination, sex, and less-than-one-year duration category. It should not be interpreted as the total number of people who have ever migrated between the two states.

The D-02 tables provide sex-specific observations, and PRABASI retains male and female observations separately.

### 2.2 Temporal coverage

PRABASI contains migration observations for the following Census years:

- 1991
- 2001
- 2011

The released dataset contains 6,682 records:

- 1,922 records for 1991;
- 2,380 records for 2001; and
- 2,380 records for 2011.

Male and female observations are represented separately, with 3,341 records for each sex across the three years.

### 2.3 Source and destination representation

Each migration observation is represented as a directed source-destination record.

Here:

- **Source (x)** denotes the state/Union Territory corresponding to the place of last residence; and
- **Destination (y)** denotes the state/Union Territory corresponding to the place of enumeration.

The migration count is stored in the `Migrants` field.

Source and destination attributes are attached separately to the corresponding record so that the characteristics of the origin and destination can be modelled jointly.

Self-pairs, in which source and destination are the same state/Union Territory, are not included in the released records.

---

## 3. Socio-Economic Indicator Selection

Candidate demographic and socio-economic indicators were collected from official statistical publications.

The selection process was:

```text
Available indicators
        ↓
Migration-relevant indicators
        ↓
Final PRABASI indicators
```

Indicators were screened based on their relevance to interstate migration modelling and their suitability for integration across the Census years.

The selection was therefore based on substantive relevance rather than solely on statistical redundancy or pairwise correlation.

Examples of indicators considered but not retained include:

- Number of AYUSH Doctors per 100,000 Population;
- Workforce Participation Rate;
- State-wise Worker Population Ratio;
- Main Workers in various age groups; and
- Average Wage Earning received by specified categories of casual labourers.

These indicators were excluded because they were considered less directly relevant to the intended migration-modelling framework.

The final PRABASI dataset contains two groups of state-level attributes:

1. demographic indicators; and
2. socio-economic indicators.

The same indicators are associated separately with the source and destination states.

---

## 4. Feature Organization

The released PRABASI dataset contains 46 columns. These columns are organized into four conceptual categories.

### 4.1 Category A: Migration and record attributes

Category A contains five attributes:

- **A1:** Year
- **A2:** Sex
- **A3:** Source (x)
- **A4:** Destination (y)
- **A5:** Migrants

These attributes identify the migration observation and its corresponding source-destination record.

`Migrants` represents the migration count and serves as the migration outcome in the machine-learning experiments. It should therefore not be interpreted as a state-level explanatory feature.

### 4.2 Category B: Demographic attributes

Category B contains six state-level demographic indicators:

- **B1:** Total Male
- **B2:** Total Female
- **B3:** Rural Male
- **B4:** Rural Female
- **B5:** Rural Density
- **B6:** Urban Density

For each source-destination record, these indicators are represented separately for the source and destination states.

The corresponding CSV fields use the suffix `_x` for source-state values and `_y` for destination-state values.

### 4.3 Category C: Socio-economic attributes

Category C contains fourteen state-level socio-economic indicators:

- **C1:** Male Job-Seekers Registered with Employment Exchanges
- **C2:** Female Job-Seekers Registered with Employment Exchanges
- **C3:** Primary SC
- **C4:** Primary ST
- **C5:** Middle SC
- **C6:** Middle ST
- **C7:** Population Density
- **C8:** Urban Population
- **C9:** Population below Poverty Line — URP
- **C10:** Population below Poverty Line — MRP
- **C11:** CPR
- **C12:** Total Employed (lakhs)
- **C13:** Percentage of Women Employment to Total Employment
- **C14:** Girls Marriage Below 18 (%)

Each socio-economic indicator is represented separately for the source and destination states.

The `_x` suffix identifies source-state values and the `_y` suffix identifies destination-state values.

### 4.4 Category D: Geographic distance

Category D contains one geographic attribute:

- **D1:** Distance

D1 represents the geographic distance between the source and destination state/Union Territory centroids, measured in kilometres.

No travel-time variable is derived from D1 or retained in the released dataset.

---

## 5. Source-Destination Record Construction

For every eligible migration observation, source and destination state-level attributes are attached to the corresponding source-destination record.

Conceptually, a record has the following structure:

```text
A1 A2 A3 A4 A5
|
B1 B2 B3 B4 B5 B6       [Source]
|
C1 C2 C3 C4 C5 C6
C7 C8 C9 C10 C11 C12 C13 C14   [Source]
|
B1 B2 B3 B4 B5 B6       [Destination]
|
C1 C2 C3 C4 C5 C6
C7 C8 C9 C10 C11 C12 C13 C14   [Destination]
|
D1
```

For example, a source-destination record involving Delhi as the source and Haryana as the destination contains:

```text
A1 A2 A3 A4 A5
|
B1-B6 for Delhi
|
C1-C14 for Delhi
|
B1-B6 for Haryana
|
C1-C14 for Haryana
|
D1
```

This representation preserves the distinction between origin and destination characteristics and allows directional migration relationships to be modelled.

---

## 6. State and Union Territory Harmonization

State and Union Territory identifiers and nomenclature differ across Census years and source publications.

To construct a consistent multi-decadal dataset, state and Union Territory identifiers were harmonized using the 2011 state-code convention where corresponding mappings were available.

Historical nomenclature differences were reconciled where appropriate. Examples include:

- Orissa / Odisha;
- Pondicherry / Puducherry; and
- Uttaranchal / Uttarakhand.

The states of Jharkhand, Chhattisgarh, and Uttarakhand were treated according to their historical administrative availability rather than being artificially introduced into years in which they did not exist as separate states.

Historical administrative and Census-coverage differences were not forcibly reconciled where a valid correspondence could not be established.

---

## 7. Treatment of Jammu and Kashmir in 1991

The 1991 Census presents a specific data-availability constraint for Jammu and Kashmir.

Jammu and Kashmir corresponds to state code 1 in the harmonized identifier system. Actual 1991 migration observations required for the source-destination matrix were unavailable. Consequently, Jammu and Kashmir does not occur as a destination in the 1991 PRABASI migration records.

No synthetic migration observations were generated to compensate for this absence.

This results in fewer source-destination records for 1991 than for 2001 and 2011.

The absence should therefore be interpreted as a limitation of historical data availability rather than as evidence of zero migration involving Jammu and Kashmir.

---

## 8. Temporal Alignment of Socio-Economic Indicators

The socio-economic indicators do not necessarily correspond to exactly the same reference year as the Census migration observations.

Where an indicator was unavailable for the exact Census year, the nearest available reference year was used. Consequently, individual indicators within a given Census-year record may represent different reference periods.

This temporal alignment was adopted to maximize the availability of state-level explanatory information while retaining the Census year as the temporal identifier of the migration observation.

The reference period of an individual indicator should therefore be considered when interpreting temporal relationships in the dataset.

---

## 9. Missing-Value Treatment

Missing observations occurred only for the indicator:

**Girls Marriage Below 18 (%)**

The affected Union Territories were:

- Andaman and Nicobar Islands
- Chandigarh
- Dadra and Nagar Haveli
- Daman and Diu
- Lakshadweep
- Puducherry

No statistical or mathematical imputation model was applied.

For these six Union Territories, the available values of the corresponding sex-ratio indicator were small numerical values. Based on this contextual assessment, the minimum numerical value observed among the available observations of `Girls Marriage Below 18 (%)` was assigned to the missing entries.

The replacement was therefore a deterministic value assignment rather than an estimate obtained from a fitted statistical model.

The resulting released CSV contains no missing values in the retained variables.

The assigned values should be interpreted with caution because they are replacements for unavailable observations and are not independently observed measurements.

---

## 10. Geographic Distance Calculation

### 10.1 Geographic source

Geographic state/Union Territory boundaries were obtained from the India states GeoJSON used for dataset construction.

Before calculating centroids, state names were normalized to ensure consistent matching with the state identifiers used in the migration data.

In particular, geographic labels such as:

- `Ladakh`; and
- `Jammu & Kashmir`

were normalized to the corresponding harmonized state names.

### 10.2 Polygon aggregation

Some states or Union Territories are represented by multiple polygon features, for example because of islands or multipart administrative geometries.

The polygons corresponding to the same state/Union Territory were therefore dissolved before centroid calculation.

This produces a single geometry for each state/Union Territory used in the distance calculation.

### 10.3 Centroid calculation

The dissolved geometries were projected to the following equal-area coordinate reference system before calculating centroids:

```text
+proj=laea
+lat_0=22.9734
+lon_0=78.6569
+units=m
+ellps=WGS84
```

Centroids were calculated in the projected coordinate system and subsequently transformed back to geographic coordinates (EPSG:4326).

### 10.4 Haversine distance

The distance between every source and destination centroid was calculated using the Haversine formula.

For two points with geographic coordinates $(\phi_1,\lambda_1)$ and $(\phi_2,\lambda_2)$, the angular distance is:

$$
a =
\sin^2\left(\frac{\phi_2-\phi_1}{2}\right)
+
\cos(\phi_1)\cos(\phi_2)
\sin^2\left(\frac{\lambda_2-\lambda_1}{2}\right)
$$

$$
c = 2\arctan2(\sqrt{a},\sqrt{1-a})
$$

and the geographic distance is:

$$
D = Rc
$$

where the mean Earth radius was taken as:

$$
R = 6371.0088\ \text{km}.
$$

A complete source-destination distance matrix was calculated from the state and Union Territory centroids.

The resulting distance is stored as **D1** in the PRABASI dataset.

### 10.5 Zero distance for Daman and Diu and Dadra and Nagar Haveli

Daman and Diu (state code 25) and Dadra and Nagar Haveli (state code 26) have coincident centroids in the geographic representation used for PRABASI.

Consequently, the Haversine distance between their centroids is zero.

Therefore:

$$
D1(25,26)=0\ \text{km}
$$

and

$$
D1(26,25)=0\ \text{km}.
$$

This zero value is a consequence of the centroid-based geographic representation and does not indicate missing distance information.

---

## 11. Derived Features for Machine-Learning Experiments

The released PRABASI CSV contains the original state-level demographic and socio-economic attributes described above.

Additional relational features can be constructed during downstream machine-learning experiments from source-destination attributes. In particular, source-destination difference and ratio features can be derived to represent relative disparities between origin and destination states.

These derived variables are not part of the released 46-column PRABASI CSV and should therefore be considered downstream feature-engineering variables rather than original dataset attributes.

The state-level variables used as denominators in ratio construction do not contain zero values in the relevant feature set. Migration counts, which can be zero for individual source-destination observations, are not used as denominators for these state-level ratio features.

---

## 12. Dataset Structure

The final released dataset contains:

- **6,682 records**
- **46 columns**
- **3 Census years:** 1991, 2001, and 2011
- **2 sexes:** Male and Female
- **5 Category A attributes**
- **6 Category B demographic attributes for the source**
- **14 Category C socio-economic attributes for the source**
- **6 Category B demographic attributes for the destination**
- **14 Category C socio-economic attributes for the destination**
- **1 Category D geographic attribute**

The 46 columns comprise record identifiers/context, migration outcome, source-state attributes, destination-state attributes, and geographic distance.

---

## 13. Data-Construction Summary

The overall PRABASI construction pipeline can be summarized as:

```text
Census D-series migration data
            |
            v
Select "Less than one year"
            |
            v
Retain sex-specific
source-destination observations
            |
            v
Collect candidate state-level
demographic and socio-economic indicators
            |
            v
Select migration-relevant indicators
            |
            v
Harmonize state/UT nomenclature
and identifiers
            |
            v
Align indicator reference periods
            |
            v
Handle limited missing observations
            |
            v
Attach source-state attributes
            |
            v
Attach destination-state attributes
            |
            v
Calculate centroid-based Haversine distance
            |
            v
Construct PRABASI records
            |
            v
Downstream feature engineering
for machine-learning experiments
```

---

## 14. Scope and Interpretation

PRABASI is intended to provide a harmonized computational representation of interstate migration observations and associated state-level demographic and socio-economic characteristics.

The dataset should not be interpreted as a complete representation of all forms of migration. In particular, the migration outcome is restricted to the selected Census category of **less than one year** and is represented separately by sex.

State-level indicators may originate from different reference periods when an exact Census-year value was unavailable. Historical administrative and data-availability constraints, particularly for Jammu and Kashmir in 1991, also affect the coverage of the resulting source-destination network.

PRABASI is therefore intended as a harmonized analytical resource for computational studies of interstate migration rather than as a direct replacement for the original Census tables.

---

## 15. Relationship to Downstream Machine-Learning Analysis

PRABASI provides the structured input data used in subsequent machine-learning experiments.

The dataset construction stage establishes:

1. the migration outcome;
2. source and destination identifiers;
3. source-state demographic characteristics;
4. source-state socio-economic characteristics;
5. destination-state demographic characteristics;
6. destination-state socio-economic characteristics; and
7. geographic distance.

Subsequent modelling stages may transform these variables into relational features, including source-destination differences and ratios, and may apply additional preprocessing or target transformations appropriate to the specific machine-learning experiment.

Such modelling operations are analytically distinct from the construction of the released PRABASI dataset.
