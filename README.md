# Home Assistant

<p align="center">
  <img src="images/overview.svg" width="100%" alt="The lowest energy bill, fully automated. A modern house with a white upper floor, dark brick below, a garage on the left and solar panels on the flat roof. The battery and the heat pump are in the garage, the dishwasher in the kitchen and the rainwater tank under the front lawn. Home Assistant sends my own solar first to the house, the battery, the dishwasher and the boiler, buys from the grid in the cheapest quarter-hours, and curtails the panels only for what nobody can use">
</p>

Things I built for my own Home Assistant and use every day.

Most of it is about energy: use as much of my own solar power as possible, and
buy the rest in the cheapest quarter-hours of my dynamic contract. The goal is
to make the energy bill as low as possible.

## Contents

| folder | what is in it |
|---|---|
| [`automations/`](automations/) | Automations for the home battery, dishwasher, hot water and heat pump, a reminder for empty sensor batteries, and a watch on topping up the rainwater tank |
| [`cards/`](cards/) | Dashboard cards, such as a pop-up with every parcel on its way |

The solar panels have their own repo: [homeassistant-apsystems_modbus](https://github.com/christianvaes/homeassistant-apsystems_modbus) reads the APsystems inverters. Curtailing them when exporting costs money comes next.
