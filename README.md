# Nationwide Spatiotemporal Modeling of Air Pollutants in Japan Using Generalized Random Forest Models

This repository contains the Generalized Random Forest (GRF) models developed for spatiotemporal modeling of air pollutants in Japan. The models estimate concentrations for the following pollutants at a 0.74 km² resolution across Japan from 2010 to 2022:

* Particulate matter ≤2.5μm in diameter (PM₂.₅)
* Nitrogen dioxide (NO₂)
* Sulfur dioxide (SO₂)
* Carbon monoxide (CO)
* Non-methane hydrocarbons (NMHCs)
* Total hydrocarbons (THCs)
* Photochemical oxidants (Ox)

For more details, refer to our manuscript (currently under review).

Our study demonstrates that simple machine learning models, based solely on inverse distance weighted (IDW) interpolation, can achieve predictive performance comparable to more complex approaches. The reduced models uploaded here utilize 21 input variables, which include interpolated concentrations of each pollutant (same-day values, three-day moving averages, and three-day moving averages of values from nearby resolution-3 hexagonal cells within a five-ring buffer).

We used the H3 hexagonal hierarchical spatial index at a resolution of 8 as spatiotemporal grids.

The prediction process is as follows:

1. Prepare the set of input variables

The GRF models require a set of 21 input variables for prediction. 

Below is the complete list of variables:

* nearby_pm25
* nearby_pm25_today
* nearby_pm25_avg
* nearby_no2
* nearby_no2_today
* nearby_no2_avg
* nearby_so2
* nearby_so2_today
* nearby_so2_avg
* nearby_co
* nearby_co_today
* nearby_co_avg
* nearby_nmhc
* nearby_nmhc_today
* nearby_nmhc_avg
* nearby_thc
* nearby_thc_today
* nearby_thc_avg
* nearby_ox
* nearby_ox_today
* nearby_ox_avg

<Note>
"nearby_{air pollutant}": three-day moving averages of the interpolated values
"nearby_{air pollutant}_today": same-day interpolated values
"nearby_{air pollutant}_avg": three-day moving averages of interpolated values from nearby resolution-3 hexagonal cells within a five-ring buffer

Make sure the input variables are prepared in matrix format.
  
2. Run Predictions Using the GRF Model

To generate predictions, load the appropriate GRF model (from our Box folder [here](https://vanderbilt.box.com/s/g6nn64e39tphqsfsqbi0m8kl49kozhxz)), then use the predict() function. Below is an example workflow in R:

```
library(tidyverse)
library(lubridate)
library(grf)

grf <- read_rds("output_reduced/grf_pm25_2020.rds")

predicted <- predict(grf, X, estimate.variance = FALSE) # X is the set of input variables (matrix format)
predicted$predictions
```

Replace grf_pm25_2020.rds with the specific model file you need, and ensure X contains the input variables in the correct format.
