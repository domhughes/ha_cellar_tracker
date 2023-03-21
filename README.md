<h2 align="center">
  <a href="https://www.cellartracker.com/"><img src="./img/ct_logo.png" alt="Cellar Tracker logo" width="200"></a>
  <br>
  <i>Home Assistant Cellar Tracker custom integration</i>
  <br>
</h2>

<p align="center">
  <a href="https://github.com/custom-components/hacs"><img src="https://img.shields.io/badge/HACS-Custom-orange.svg"></a>
</p>

The `cellar tracker` implementation allows you to integrate your [Cellar Tracker](https://www.cellartracker.com/) data in Home Assistant. The is a fork of the original Home Assistant integration by @ahoernecke which in turn is built upon the cellartracker package by @mathroule.

# Disclaimer
This is an unofficial integration of Cellar Tracker for Home Assistant, the developer and the contributors are not, in any way, affiliated to CellarTracker! LLC.

"CellarTracker!" is a trademark of CellarTracker! LLC

# Requirements
- Cellar Tracker account - https://cellartracker.com
- HACS: Home Assistant Community Store - https://hacs.xyz/
- Flex Table Card - Available in HACS or https://github.com/custom-cards/flex-table-card/
- **(Optional)** secrets.yaml - https://www.home-assistant.io/docs/configuration/secrets/

# Installation
The integration should be available in HACS under Custom Integration, if that is not the case, just add manually the repository:

![HACS Adding a repository](./img/hacs_1.png)

- **Repository:** https://github.com/domhughes/ha_cellar_tracker
- **Category:** Integration

# Configuration:

Add your CellarTracker! username and password in `secrets.yaml`:

```
cellar_tracker_username: YOUR_USERNAME
cellar_tracker_password: YOUR_PASSWORD
```

Then go to `configuration.yaml` and add:

```
cellar_tracker:
  username:  !secret cellar_tracker_username
  password:  !secret cellar_tracker_password
```

# Dashboard visualization
![CellarTracker! Dashboard](./img/dashboard.png)

To create a new dashboard for CellarTracker! go to Configuration -> Dashboards -> Add a new dashboard (the following is just a suggestion)

![CellarTracker! Dashboard Creation](./img/new_dashboard.png)

## To populate your Dashboard:
Note: missing / unavailable entities will throw a type error in the When to drink flex-table-card. This may happen because of recategorisation in the cellar tracker database or possibly other causes leading to stranded entities. If this happens just remove the entity from HA and the card will generate.

**Glance Card Visualization**

```
type: glance
entities:
  - entity: sensor.cellar_tracker_total_bottles
  - entity: sensor.cellar_tracker_total_value
```

**Bottles by Country**
```
type: custom:flex-table-card
title: Bottles by Country
entities:
  include: sensor.cellar_tracker_country*
columns:
  - name: Country
    data: sub_type
  - name: Count
    data: count
  - name: Percentage
    data: '%'
    modify: parseFloat(x).toFixed(0)
    suffix: '%'
  - name: Average Value
    data: value_avg
    prefix: €  #change here according to your currency
    modify: parseFloat(x).toFixed(0)
sort_by: count-
```

**Bottles by Producer**
```
type: custom:flex-table-card
title: Bottles by Producer
entities:
  include: sensor.cellar_tracker_producer*
columns:
  - name: Producer
    data: sub_type
  - name: Count
    data: count
    modify: parseFloat(x)
  - name: Percentage
    data: '%'
    modify: parseFloat(x).toFixed(0)
    suffix: '%'
  - name: Average Value
    data: value_avg
    prefix: €  #change here according to your currency
    modify: parseFloat(x).toFixed(0)
sort_by: count-
```

**Bottles by Vintage**
```
type: custom:flex-table-card
title: Bottles by Vintage
entities:
  include: sensor.cellar_tracker_vintage*
columns:
  - name: Vintage
    data: sub_type
  - name: Count
    data: count
  - name: Percentage
    data: '%'
    modify: parseFloat(x).toFixed(0)
    suffix: '%'
  - name: Average Value
    data: value_avg
    prefix: €  #change here according to your currency
    modify: parseFloat(x).toFixed(0)
sort_by: count-
```

**Bottles by Varietal**
```
type: custom:flex-table-card
title: Bottles by Varietal
entities:
  include: sensor.cellar_tracker_varietal*
columns:
  - name: Varietal
    data: sub_type
  - name: Count
    data: count
  - name: Percentage
    data: '%'
    modify: parseFloat(x).toFixed(0)
    suffix: '%'
  - name: Average Value
    data: value_avg
    prefix: €  #change here according to your currency
    modify: parseFloat(x).toFixed(0)
sort_by: count-
```

**Wines with vintage and drinking dates**
```
type: custom:flex-table-card
title: When to drink
entities:
  include: sensor.cellar_tracker_wine_vintage_*
columns:
  - name: Wine
    data: sub_type
  - name: '#'
    data: count
    align: right
  - name: Score
    data: CT
    align: right
    modify: parseFloat(x).toFixed(1)
  - name: Start Drink Date
    data: BeginConsume
    align: right
  - name: End drinking
    data: EndConsume
    align: right
  - name: Bought
    data: PurchaseDate
    align: right
  - name: Value (ea)
    data: value_avg
    align: right
    modify: parseFloat(x).toFixed(2)
  - name: Location
    data: Location
    align: right
  - name: Bin
    data: Bin
    align: right
sort_by: BeginConsume+
```

# Contribute
Feel free to contribute by opening a PR, issue on this project
