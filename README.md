# Churchill REST API Endpoints

## Overview

The available API endpoints were built as part of the Churchill Project. Currently, all of the data available originates from either  [Nomis](https://www.nomisweb.co.uk/) or [StatXplore](https://stat-xplore.dwp.gov.uk/).

All of the endpoints are REST endpoints and use HTTP `GET` method only - so there is no need to construct request bodies when calling them.

To test the APIs, you can use tools such as [Postman](https://www.getpostman.com/), or even just a browser (as the endpoints all only accept HTTP `GET` methods) to get sample responses.

## External APIs

* Stat-Xplore front end https://stat-xplore.dwp.gov.uk/webapi
* Stat-Xplore API documentation https://stat-xplore.dwp.gov.uk/webapi/online-help/Open-Data-API.html
* Nomis front end https://www.nomisweb.co.uk/

## Variables

The endpoint paths contain some variable values. These are the different variables and possible values:

#### Dataset and Metric

Dataset and metric are used in combination to retrieve the data from the API.

The following table shows the available dataset and metric combinations and the description of the data values that will be returned:

|Dataset|Code|Metric|Description|
|:--:|:--:|:--:|:--:|
|Attendance Allowance|`aa`|`totals`|Number of people entitled to Attendance Allowance|
|Attendance Allowance|`aacip`|`totals`|Number of people in receipt of Attendance Allowance|
|Attendance Allowance|`aapa`|`totals`|Average weekly amount for those in receipt of Attendance Allowance|
|Carers Allowance|`cae`|`totals`|Number of people entitled to Carers Allowance|
|Carers Allowance|`capc`|`totals`|Number of people in receipt of Carers Allowance|
|Claimant Count|`cc`|`totals`|Number of people on Job Seekers Allowance and the number of people on Universal Credit who would have been eligible for Job Seekers Allowance combined|
|Disability Living Allowance|`dlaec`|`totals`|Number of people entitled to Disability Living Allowance|
|Disability Living Allowance|`dlapc`|`totals`|Number of people in receipt of Disability Living Allowance|
|Disability Living Allowance|`dlapwaa`|`totals`|Average weekly amount of Disability Living Allowance|
|Earnings (Resident Based)|`ebr`|`totals`|Average weekly amount of Resident Based Earnings|
|Earnings (Workplace Based)|`ebw`|`totals`|Average weekly amount of Workplace Based Earnings|
|Employment|`emp`|`totals`|Number of people in employment|
|Employment|`emp`|`percentages`|Percentage of people in employment|
|Employment Support Allowance|`esa`|`totals`|Number of people claiming Employment Support Allowance|
|Employment Support Allowance|`esa`|`percentages`|Percentage of population claiming Employment Support Allowance|
|Hours Worked (Residence Based)|`hwbr`|`totals`|Number of Residence Based Hours Worked|
|Hours Worked (Workplace Based)|`hwbw`|`totals`|Number of Workplace Based Hours Worked|
|Job Seekers Allowance|`jsa`|`totals`|Number of people claiming Job Seekers Allowance|
|Job Seekers Allowance|`jsa`|`percentages`|Percentage of population claiming Job Seekers Allowance|
|Pension Credit|`pca`|`totals`|Average weekly amount of Pension Credit|
|Pension Credit|`pcb`|`totals`|Number of people benefitting from Pension Credit|
|Pension Credit|`pcc`|`totals`|Number of people claiming Pension Credit|
|Pension Credit|`pcc`|`percentages`|Percentage of population claiming Pension Credit|
|Pension Credit|`pcc`|`percentages`|Percentage of population claiming Pension Credit|
|Personal Independence Payment|`pip`|`totals`|Number of people claiming Personal Independence Payment|
|Personal Independence Payment|`pip`|`percentages`|Percentage of population claiming Personal Independence Payment|
|Population|`pop`|`totals`|Number of people|
|State Pension|`sp`|`totals`|Number of people claiming State Pension|
|Universal Credit|`uc`|`totals`|Number of people claiming Universal Credit|
|Universal Credit|`uc`|`percentages`|Percentage of population claiming Universal Credit|
|Unemployed|`unemp`|`totals`|Number of unemployed people|
|Unemployed|`unemp`|`percentages`|Percentage of unemployed people|

#### Geography Type
Many of the datasets exist on different levels of **Geography Type**.

The available values for **Geography Type** are:

|Geography Type|Code|Example area|
|:---:|:---:|:---:|
|Local Authority|`la`|Manchester|
|Westminster Parliamentary Constituency|`wpc`|Manchester Central|

#### Month and Year
The format for month and year are as follows:

|Variable|Format|Examples|
|:---:|:---:|:---:|
|`month`|`M` or `MM`|`1` for January, `12` for December|
|`year`|`YYYY`|`2017`|

Years range from `1999` to `2017` - though for each dataset and metric combination you can use the **Available Dates for Dataset** endpoint to find which year and month combinations are available.

## Endpoints

#### Available Dates for Dataset
```
http://dwp.joedrumgoole.com:3000/datasets/{datasetCode}/metrics/{metric}/geog-types/{geographyType}/available-dates
```
This endpoint will return the available dates; combinations of **Month and Year** pairs for this dataset.

Example Call:
```
http://dwp.joedrumgoole.com:3000/datasets/cae/metrics/totals/geog-types/wpc/available-dates
```
The example will return the available **Month and Year** combinations for the `Westminster Parliamentary Constituency` **Geography Type** for the `Carers Allowance` **Dataset** with the `totals` **Metric** (Number of people entitled to Carers Allowance).

Response Format/Example:
```json
[
  {
    "month": 5,
    "year": 2017
  },
  ...
]
```
The response is an array of objects containing a `month` and `year` attributes.

#### Totals Data for All Areas by Month and Year
```
http://dwp.joedrumgoole.com:3000/datasets/{datasetCode}/metrics/{metric}/geog-types/la?month={month}&year={year}
```

This endpoint will return the data for the given **Dataset and Metric** combination, for all areas of a given **Geography Type** for a given **Month and Year**.

Example Call:
```
http://dwp.joedrumgoole.com:3000/datasets/cae/metrics/totals/geog-types/wpc?month=2&year=2017
```

Response Format/Example:
```json
[
  {
    "name": "Knowsley",
    "code": "E14000775",
    "total": 4585,
    "gender": {
      "male": 1649,
      "female": 2935
    },
    "ageBands": {
      "under18": 11,
      "18to24": 220,
      "25to29": 246,
      "30to34": 318,
      "35to39": 299,
      "40to44": 334,
      "45to49": 436,
      "50to54": 464,
      "55to59": 504,
      "60to64": 420,
      "over64": 1326,
      "unknown": 0
    }
  },
  ...
]
```

The response is an array of objects, each of which represents an area (of the type provided in the **Geography Type**) and the values for the provided **Dataset and Metric** at the provided **Month and Year**. It will have `name` and `code` to identify the area, the `total` value as well as demographic breakdowns for `gender` and `ageBands`.

#### All Demographics Data for a Single Area
```
http://dwp.joedrumgoole.com:3000/datasets/{datasetCode}/metrics/{metric}/geog-types/{geographyType}?geogCode={geographyCode}
```

This endpoint returns the demographics (gender and age) data for the provided **Dataset and Metric** for an area of the provided **Geography Type** with the provided _Geography Code_ (Geographic Area).

_Geography Codes_ for Geographic Areas can be obtained using the **Geographies** endpoint for Local Authorities (`la`) and Westminster Parliamentary Constituencies (`wpc`).

Example Call:
```
http://dwp.joedrumgoole.com:3000/datasets/cae/metrics/totals/geog-types/wpc?geogCode=E14000775
```

Response Format/Example:
```json
{
  "meta": {
    "dataset": "cae",
    "source": {
      "name": "Stat-Xplore",
      "url": "https://stat-xplore.dwp.gov.uk/",
      "info": "https://stat-xplore.dwp.gov.uk/webapi/metadata/CA_Entitled/CA_Entitled.html"
    }
  },
  "data": [
      {
        "name": "Knowsley",
        "total": 2542,
        "gender": {
          "male": 873,
          "female": 1664
        },
        "ageBands": {
          "under18": 8,
          "18to24": 135,
          "25to29": 224,
          "30to34": 311,
          "35to39": 385,
          "40to44": 373,
          "45to49": 268,
          "50to54": 263,
          "55to59": 225,
          "60to64": 180,
          "over64": 168,
          "unknown": 0
        },
        "year": 1999,
        "month": 8
      },
      ...
    ]
}
```

The response is an object with two parts; `meta` and `data`.

The `meta` attribute contains the same as the **Metadata** endpoint response as if it had been called with the provided **Dataset** and **Geography Type**.

The `data` attribute is an array consisting of entries for individual **Month and Year** combinations, with the `total` for this _Geography Code_ (Geographic Area) and a breakdown by `gender` and age (`ageBands`).

:warning: Note that some of the demographics will _not_ add up to the `total` - this is how they appear in the source data.

#### All Totals Data for Great Britain
```
http://dwp.joedrumgoole.com:3000/datasets/{datasetCode}/metrics/{metric}/geog-types/{geographyType}
```

This endpoint returns data for all time for Great Britain for the given **Dataset and Metric** (and **Geography Type** - just in case the different types use difference data sources etc).

Example Call:
```
http://dwp.joedrumgoole.com:3000/datasets/cae/metrics/totals/geog-types/wpc
```

Response Format/Example:
```json
[
  {
    "year": 1999,
    "month": 8,
    "total": 445275
  },
  ...
]
```

The response is an array consisting of `total` values for **Month and Year** combinations.

#### All Totals Data for All Areas (Time Series)
```
http://dwp.joedrumgoole.com:3000/datasets/{datasetCode}/metrics/{metric}/geog-types/{geographyType}/time-series
```

This endpoint returns data for the provided **Dataset and Metric** combination for all time for all Geographic Areas of the provided **Geography Type**.

Example Call:
```
http://dwp.joedrumgoole.com:3000/datasets/cae/metrics/totals/geog-types/wpc/time-series
```

Response Format/Example:
```json
{
  "xAxis": [
    "Aug 1999",
    "Nov 1999",
    "Feb 2000",
    ...
  ],
  "yAxis": [
    {
      "code": "E14000775",
      "name": "Knowsley",
      "data": [
        2542,
        2511,
        2472,
        ...
      ]
    },
    ...
  ]
}
```

The response is an object with two parts; `xAxis` and `yAxis`.

`xAxis` contains an _ordered_ array of the **Month and Year** combinations, whose indices match with the `data` attributes of the `yAxis` array items.

`yAxis` contains an array of objects representing _Geographic Areas_. Each _Geographic Area_ has a `code` and `name` to identify it as well as an _ordered_ array of the `data` for this _Geographic Area_. The indices of the data in the `data` array will match with the `xAxis` array items.

#### Metadata
```
http://dwp.joedrumgoole.com:3000/datasets/{datasetCode}/geog-types/{geographyType}/meta
```

This endpoint will return the metadata for the provided **Dataset** for the provided **Geography Type**. This will include information on the source of the data .

Example Call:
```
http://dwp.joedrumgoole.com:3000/datasets/cae/geog-types/wpc/meta
```

Response Format/Example:
```json
{
  "dataset": "cae",
  "source": {
    "name": "Stat-Xplore",
    "url":"https://stat-xplore.dwp.gov.uk/",
    "info":"https://stat-xplore.dwp.gov.uk/webapi/metadata/CA_Entitled/CA_Entitled.html"
  }
}
```

The response is an object with information on the `source` of the dataset including the `name`, `url` for the site and an `info` page about that dataset and/or when it may be updated.

#### Geographies
```
http://dwp.joedrumgoole.com:3000/geog-types/{geographyType}
```

This endpoint returns data in the [GeoJSON](http://geojson.org/) format for the provided **Geography Type**.

Example Call:
```
http://dwp.joedrumgoole.com:3000/geog-types/la
 ```

Response Format/Example:
```json
[
  {
    "type": "Feature",
    "geometry": {
      "type": "Polygon",
      "coordinates": [
        [
          [
            -2.8878755399382374,
            53.503639700290606
          ],
          ...
        ],
      ]
    },
    "properties": {
      "PCON13CDO": "C48",
      "code": "E14000775",
      "name": "Knowsley"
    }
  },
  ...
]
```

The response is an array of GeoJSON `Feature` objects. Each `Feature` object is a GeoJSON `geometry` with additional `properties`.

The `properties` for these objects contain the `name` and `code` (used for _Geographic Area_ in other endpoints) for the given area as well as additional properties that vary depending on the **Geography Type** (in this example, the `PCON13CDO` for `wpc`).
