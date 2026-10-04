
# PRABASI Dataset Card

## 1. Dataset Summary

### Dataset Name

PRABASI: A Multi-Decadal Dataset of Socio-Economic Disparities and Interstate Migration in India

### Version

Anonymous peer-review version

### Dataset Type

Structured tabular source-destination dataset

### Geographic Coverage

India (States and Union Territories)

### Temporal Coverage

1991, 2001, and 2011 Census periods

### Primary Application

Computational analysis and machine-learning-based modelling of interstate migration in India.

---

## 2. Dataset Summary

PRABASI is a multi-decadal dataset integrating interstate migration observations with demographic and socio-economic characteristics of Indian states and Union Territories.

The dataset is designed primarily for computational analysis and machine-learning-based modelling of interstate migration in India.

The fundamental observational unit is a **source-destination state pair**. Each observation represents a migration relationship between a source state/Union Territory and a destination state/Union Territory and combines:

1. state-pair-specific features;
2. demographic features of the source and destination states; and
3. socio-economic features of the source and destination states.

The dataset covers the 1991, 2001, and 2011 Census periods.

---

## 3. Motivation

Interstate migration is influenced by characteristics of both the origin and destination as well as by the relationship between them. 

PRABASI was constructed to provide a structured representation in which migration relationships can be analysed jointly with the demographic and socio-economic characteristics of the corresponding source and destination states. The dataset is intended to reduce the preprocessing burden associated with combining migration observations and various state-level indicators from different administrative sources.

---

## 4. Intended Use

PRABASI may be used for:

- interstate migration modelling;
- migration prediction;
- exploratory migration analysis;
- socio-economic disparity analysis;
- source-destination modelling;
- machine-learning research;
- temporal comparison of migration patterns;
- feature engineering experiments;
- model interpretability studies.

---

## 5. Out-of-Scope Uses

The dataset should not be interpreted as:

- a causal dataset;
- a record of individual migration trajectories;
- a substitute for individual-level migration microdata;
- a representation of actual travel routes;
- evidence that a particular socio-economic variable causes migration.

The data are aggregated at the state/Union Territory levels representing the source-destination pairs for interstate migrataion.

---



## 6. Dataset Structure

The features in PRABASI are divided into four categories:

- **Category A:** state-pair-specific features;
- **Category B:** state-specific demographic features; 
- **Category C:** state-specific socio-economic features; and
- **Category D:** state-pair-specific distance between centroids.

The distinction between these categories is important because Category A describes a relationship between two states, whereas Categories B and C describe individual states.

For a source state \(x\) and destination state \(y\), the overall feature representation can be expressed as:

\[
X_{xy} =
[A_{xy}, B_x, C_x, B_y, C_y]
\]

where:

- \(A_{xy}\) represents features specific to the source-destination pair;
- \(B_x\) represents demographic characteristics of the source state;
- \(C_x\) represents socio-economic characteristics of the source state;
- \(B_y\) represents demographic characteristics of the destination state; and
- \(C_y\) represents socio-economic characteristics of the destination state.

---

## 6. Data Sources

### 6.1 Census Migration Data (Category A)

Migration observations were obtained from official Census of India migration data.

The relevant migration data are from D-series migration tables, particularly the D-02 series.

The migration categories include sex-specific observations and three census year categories.

PRABASI uses:

**Duration of stay: Less than one year**

---

### 6.2 Socio-Economic and Demographic Data (Category B and C)

Demographic and socio-economic indicators were obtained from official statistical publications. 

One principal source is:

**Selected Socio-Economic Statistics, India, 2011**

Government of India  
Ministry of Statistics and Programme Implementation  
Central Statistics Office  
Social Statistics Division  
October 2011

The publication contains indicators from multiple domains, including population, migration, labour, health, education, and other socio-economic areas.

The publication also contains indicators whose reference years differ from 2011. Therefore, source-year information should be retained when interpreting individual variables.

### 6.3 Distance between source-destination pairs (Category D)

PRABASI documents distance (in kilometres) between the centroids of the source and destination state polygons, calculated using the Haversine formula.
The code used for calculating the distance between pairs is provided in the **Code** folder. 

---

## 8. Data Collection

The data-collection process can be summarized as:

```
Official source documents
          |
          v
Extraction of available indicators
          |
          v
Migration-relevant indicator selection
          |
          v
Cleaning and nomenclature harmonization
          |
          v
State/UT code harmonization
          |
          v
Missing-value treatment
          |
          v
Source-destination construction
          |
          v
Distance calculation
          |
          v
PRABASI
```





