# Home Assistant automations

Automations from my Home Assistant that make appliances follow the dynamic
electricity price and the solar surplus. Each one lives as a separate YAML
file in `automations/`.

## Files

| file | what it does |
|---|---|
| `accu_dagplan_en_bijplan.yaml` | Fills the Zendure battery with solar wherever possible and tops up the remainder from the grid, only in the cheapest quarter-hours of the day. Plans ahead on the solar forecast and picks the battery mode to match |
| `vaatwasser_starten_op_de_goedkoopste_prijs.yaml` | Starts the selected program on the Siemens dishwasher, delayed until the cheapest window |
| `boiler_verhogen_zonoverschot.yaml` | Raises the hot water setpoint on the Ecoforest heat pump when there is real solar surplus |
| `warmtepomp_koeling_uit_bij_hoge_stroomprijs.yaml` | Turns the Ecoforest heat pump off above EUR 0.50 per kWh when warm weather is forecast |

## Hardware

Not tied to one specific model. Comparable models from these brands will work,
with different entity IDs.

- **Zendure SolarFlow** home battery (mine is a 2400 AC+)
- **Ecoforest** heat pump with its domestic hot water tank, controlled through
  an Eplucon Th-touch
- **Siemens** dishwasher on Home Connect
- **HomeWizard P1** meter, for grid import and export

## Dependencies

- **Zendure zenSDK package by Gielz1986** for the battery entities and the
  dynamic price sensors
- **Solcast** for the solar forecast
- **Home Connect** for the dishwasher
- **Eplucon** and the **tech** integration for the Ecoforest heat pump
- A **Nordpool** sensor via HACS for the quarter-hour prices

## Measured assumptions

The numbers in `accu_dagplan_en_bijplan.yaml` rest on measurements taken on this
installation and do not transfer as-is:

| variable | value | source |
|---|---|---|
| `accu_inhoud` | 11.04 kWh | Zendure SolarFlow 2400 AC+ |
| `laadvermogen_per_kwartier` | 0.53 kWh | measured at full charging power |
| `zon_naar_accu_deel` | 0.55 | calibrated on 355 days of generation and feed-in |
| `zon_basislast` | 1.5 kWh | same |
| `rendement` | 0.87 | round trip, slightly below the measured 87.9 percent |

`boiler_verhogen_zonoverschot.yaml` uses a threshold of 2400 W, based on the
measured 2177 W draw during water heating.
