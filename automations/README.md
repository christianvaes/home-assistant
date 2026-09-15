# Home Assistant automations

Automations that let appliances follow the electricity price and use the solar
left over. Each one is a separate YAML file.

## Files

| file | what it does |
|---|---|
| [`accu_dagplan_en_bijplan.yaml`](accu_dagplan_en_bijplan.yaml) | Fills the Zendure battery with solar and buys the rest in the cheapest quarter-hours, planned on the solar forecast. Sets the battery mode |
| [`vaatwasser_starten_op_de_goedkoopste_prijs.yaml`](vaatwasser_starten_op_de_goedkoopste_prijs.yaml) | Starts the selected program on the Siemens dishwasher, delayed until the cheapest window |
| [`boiler_verhogen_zonoverschot.yaml`](boiler_verhogen_zonoverschot.yaml) | Raises the hot water setpoint on the Ecoforest heat pump when there is really solar left over |
| [`warmtepomp_koeling_uit_bij_hoge_stroomprijs.yaml`](warmtepomp_koeling_uit_bij_hoge_stroomprijs.yaml) | Turns the Ecoforest heat pump off above EUR 0.50 per kWh when warm weather is forecast |

## Hardware

What I run. Other models from the same brands work, with different entity IDs.

- **Zendure SolarFlow** home battery (mine is a 2400 AC+)
- **Ecoforest** heat pump with its domestic hot water tank, controlled through
  an Eplucon Th-touch
- **Siemens** dishwasher on Home Connect
- **HomeWizard P1** meter, for grid import and export

## Battery charging/discharging logic

Every quarter-hour [`accu_dagplan_en_bijplan.yaml`](accu_dagplan_en_bijplan.yaml) picks a mode, based on the
solar left over, the price, and whether the forecast fills the battery without
the grid.

| solar left over | price | battery | what happens | mode |
|---|---|---|---|---|
| yes | any | fills from solar alone | solar fills the battery and covers the house | `Nul op de meter` |
| yes | cheapest quarter | will not fill from solar | solar fills the battery, the house buys the shortfall | `Alleen slim opladen` |
| no | cheapest quarter | will not fill from solar | the house buys, and the battery is saved for a more expensive hour | `Alleen slim opladen` |
| no | cheapest quarter, grid charging active | any | the battery charges from the grid, the house buys its own power | `Dynamisch NOM` |
| no | expensive | any | the battery supplies the house | `Nul op de meter` or `Dynamisch NOM` |

The mode names are Dutch, they come from the Gielz zenSDK package. A quarter
counts as cheap when the price is within the round trip loss of the lowest price
of the day, and below the day average. Moving a kWh through the battery costs
about 13 percent, so a smaller spread than that costs money instead of saving
it. On a flat day nothing qualifies.

## Dependencies

- **Zendure zenSDK package by Gielz1986** for the battery entities and the
  dynamic price sensors
- **Solcast** for the solar forecast
- **Home Connect** for the dishwasher
- **Eplucon** and the **tech** integration for the Ecoforest heat pump
- A **Nordpool** sensor via HACS for the quarter-hour prices

## Measured assumptions

The numbers in [`accu_dagplan_en_bijplan.yaml`](accu_dagplan_en_bijplan.yaml) come from measurements on my own
installation.

| variable | value | source |
|---|---|---|
| `accu_inhoud` | 11.04 kWh | Zendure SolarFlow 2400 AC+ |
| `laadvermogen_per_kwartier` | 0.53 kWh | measured at full charging power |
| `zon_naar_accu_deel` | 0.55 | calibrated on 355 days of generation and feed-in |
| `zon_basislast` | 1.5 kWh | same |
| `rendement` | 0.87 | round trip, just below the measured 87.6 percent |

[`boiler_verhogen_zonoverschot.yaml`](boiler_verhogen_zonoverschot.yaml) uses a threshold of 2400 W, based on the
measured 2177 W draw during water heating.
