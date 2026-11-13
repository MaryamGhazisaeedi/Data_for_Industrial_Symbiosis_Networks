# Industrial Symbiosis Through Online Platforms: Research Data

This repository contains the location datasets and physical and financial parameters used in a study of online platform-based industrial symbiosis networks. The study examines how government subsidies, taxes, and their combination affect network performance under different levels of market demand volatility.

## Industrial symbiosis and industrial symbiosis networks

**Industrial symbiosis (IS)** is a collaborative approach in which the waste, by-products, or surplus resources of one firm become useful inputs for another. Such exchanges can involve materials, water, or energy and can reduce demand for primary resources, waste disposal, and associated costs and environmental impacts.

An **industrial symbiosis network (ISN)** consists of participating firms and the resource-exchange relationships connecting them. Firms act as nodes, while exchanges form the links between them. The feasibility of a partnership depends on factors such as material compatibility, supply and demand, treatment requirements, transportation distances, and economic incentives.

Online platforms can support these networks by coordinating information and identifying and recommending potential partners. In this study, fruit-juice manufacturers represent waste producers, while dairy manufacturers represent waste users that can use fruit residuals as an input to their feed supply, subject to the model's treatment and technical assumptions.

## Research overview

The associated study develops an agent-based model informed by case studies of fruit juice and dairy manufacturers in Iran. The online platform is represented as a dynamic agent that uses clustering-based machine learning to support partner identification and recommendations as market conditions change.

The study evaluates economic and environmental benefits, waste exchanges, and the realization of symbiotic transactions. Its findings indicate that policy interventions improve network performance across the market conditions examined. Subsidies outperform taxes as standalone instruments, while their combination delivers the strongest overall performance. Market dynamism does not significantly diminish network performance in the modeled setting, primarily because machine learning-based partner identification enables adaptive matching.

**Keywords:** industrial symbiosis; online platforms; government policy; agent-based model; machine learning; clustering.

## Data sources and simulation setting

The data used in this study were derived from case studies of **two companies in Iran: one fruit-juice manufacturer and one dairy manufacturer** (Ghazisaeedi et al., 2026). These case studies provide the empirical basis for the physical and financial parameters used in the numerical example.

Additional location points were selected from industrial estates and industrial zones in Iran to provide the spatial setting for the simulated network. The 200 location records should therefore be interpreted as locations used to represent simulation participants, rather than evidence of separate empirical case studies of 200 companies.

In each simulation run, **100 fruit-juice manufacturers and 100 dairy manufacturers** are randomly distributed across a square region in Iran measuring **200 km per side**. Euclidean distances between companies are taken into account. The locations assigned to all companies are plotted using a Geographic Information System (GIS).

The case-study parameters, industrial-zone location data, and simulation design serve distinct purposes: the parameters characterize production and costs, the location files supply spatial information, and the simulation represents interactions among 200 agents.

## Dataset contents

| File | Description | Data records | Worksheet |
| --- | --- | ---: | --- |
| [Waste producers_locations.xlsx](Waste%20producers_locations.xlsx) | Locations used to represent fruit-juice manufacturers (waste producers) | 100 | `Sheet1` |
| [Waste users_locations.xlsx](Waste%20users_locations.xlsx) | Locations used to represent dairy manufacturers (waste users) | 100 | `Sheet1` |
| [Physical and financial data applied in the numerical example.xlsx](Physical%20and%20financial%20data%20applied%20in%20the%20numerical%20example.xlsx) | Physical and financial parameters derived from the two case-study companies | 9 parameters | `Parameters` |

Here, **waste users** means participants that use waste as a resource, rather than users of the online platform in general.

Each location workbook contains one header row and 100 data rows, with five columns. Both include locations labeled Alborz, Markazi, Qazvin, Qom, Semnan, and Tehran in the `Province / City` field.

## Data dictionary

### Location datasets

Both location workbooks use the same column structure. Column names below match the files exactly.

| Column | Type | Description |
| --- | --- | --- |
| `Row` | Integer | Sequential record number, from 1 to 100 within each workbook. |
| `Province / City` | Text | Province or city label associated with the location. |
| `Industrial Zone` | Text | Industrial zone or locality label, as recorded in the source file. |
| `Latitude` | Numeric | Latitude expressed as a decimal-degree value. |
| `Longitude` | Numeric | Longitude expressed as a decimal-degree value. |

Record numbers are local to each workbook. Matching `Row` values across the two files do not establish a producer–user partnership or an observed waste exchange.

### Physical and financial parameters

The `Parameters` worksheet contains five columns: `Parameter`, `Symbol`, `Unit`, `Fruit Juice Producer`, and `Dairy Producer`. The values below reproduce the supplied workbook.

| Parameter | Symbol | Unit | Fruit Juice Producer | Dairy Producer |
| --- | --- | --- | ---: | ---: |
| Mean production volume | `x` | ton/day | 300 | 50 |
| Waste production technical coefficient | `W` | t fruit residuals / t fruit | 0.2 | — |
| Waste disposal cost | `udc` | IRR / t fruit residuals | 20,000,000 | — |
| Technical input-use coefficient | `R` | t feed / t dairy | — | 1.2 |
| Input procurement cost | `upc` | IRR / t feed | — | 80,000,000 |
| Waste transportation cost | `utra` | IRR / (t fruit residuals·km) | 800,000 | — |
| Minimum waste treatment cost | `q_min` | IRR / t fruit residuals | 10,000,000 | — |
| Maximum waste treatment cost | `q_max` | IRR / t fruit residuals | 60,000,000 | — |
| Daily storage cost | `ust` | IRR / t fruit residuals | 1,200,000 | — |

**Unit notes:** IRR denotes Iranian rials; t denotes tonnes. A dash represents an empty cell in the supplied workbook, not a zero value. Units are reproduced as provided. In particular, the daily storage cost is labeled as daily, although its unit field does not explicitly include a per-day denominator.

## Accessing the data

Download the workbooks and open them in Microsoft Excel, LibreOffice Calc, or another application that supports `.xlsx` files.

For Python, install `pandas` and `openpyxl`, then run the following from the directory containing the workbooks:

```bash
pip install pandas openpyxl
```

```python
import pandas as pd

producers = pd.read_excel(
    "Waste producers_locations.xlsx", sheet_name="Sheet1"
)
users = pd.read_excel(
    "Waste users_locations.xlsx", sheet_name="Sheet1"
)

parameters = pd.read_excel(
    "Physical and financial data applied in the numerical example.xlsx",
    sheet_name="Parameters",
)

print(producers.head())
print(users.head())
print(parameters)

# Preserve participant roles when combining location records.
locations = pd.concat(
    [
        producers.assign(role="waste_producer"),
        users.assign(role="waste_user"),
    ],
    ignore_index=True,
)
```

## Scope and interpretation

- The repository provides location inputs and physical and financial parameters for the numerical example. These files do not include the agent-based model implementation, policy scenario settings, transaction histories, or simulation outputs.
- Reproducing the article's results also requires the model implementation, complete parameter settings, experimental design, and randomization procedure.
- The location workbooks contain geographic latitude and longitude values. Euclidean distances in kilometers require an appropriate projected coordinate system or a documented conversion into the model's spatial coordinates; distances should not be calculated directly from degree values and interpreted as kilometers.
- The workbooks do not document the coordinate reference system, geocoding method, positional accuracy, or the mapping from the listed coordinates to the 200 km × 200 km simulation region. These implementation details should be consulted in the associated study or model documentation.
- No missing cells were found in the 100 data records and five documented columns of either location workbook. Blank parameter cells are retained as supplied and should not automatically be replaced with zeros.

## Associated article and citation

If you use these datasets in research, please cite the associated article and identify the version or commit of the dataset used.

**Article reference:** [Authors]. [Year]. *[Article title]*. [Journal or preprint venue]. [DOI or URL].

<!-- Replace the bracketed citation fields with the article's confirmed bibliographic details before publishing. -->

**Case-study data source:** Ghazisaeedi et al. (2026). [Full reference and DOI or URL to be added].

<!-- Complete the case-study reference separately from the associated article citation, unless they refer to the same publication. -->
