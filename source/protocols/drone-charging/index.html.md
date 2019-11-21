---
title: Drone Charging

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - typescript

toc_footers:
  - Documentation powered by <a href="https://github.com/lord/slate" target="blank">Slate</a>.

search: true
---

<p class="header-image"><img src="/images/drone_charging/header.png" alt="Drone Charging"></p>

# Drone Charging Protocol

The communication protocol for drone charging describes the format of a request for a charging service (`need`), and the response sent by a charging provider (`bid`).

For example, a drone might search for charging stations within 2 km of its location that supports 2mm bullet connectors.

In response, a charging station might send back a bid with a price per kWh, a time at which the service can be provided, and additional amenities provided (e.g., the ability for the drone to remain parked on the charger after charging completes).

# Need

A statement of need for charging services. Typically this will be sent by a drone that is looking for a charging station around certain coordinates.

This request is sent to the discovery engine which broadcasts the need to DAV identities that can provide this service. <a href="#bid">Bids</a> are later received in response.

## Arguments

```javascript
const { SDKFactory } = require("dav-js");
const { NeedParams, enums } = require("dav-js/dist/drone-charging");
const DAV = SDKFactory({
  apiSeedUrls,
  kafkaSeedUrls
});
const drone = await DAV.getIdentity(droneDavId);

const needParams = new NeedParams({
  location: {
    lat: 32.050382,
    long: 34.766149
  },
  radius: 20,
  startAt: 1538995253092,
  dimensions: {
    length: 50,
    width: 15,
    height: 20
  },
  weight: 50000,
  batteryCapacity: 4,
  currentBatteryCharge: 45,
  energySource: enums.EnergySources.Solar,
  amenities: [enums.Amenities.Docking]
});
const need = await drone.publishNeed(needParams);
```

```typescript
import { SDKFactory } from "dav-js";
import { NeedParams, enums } from "dav-js/dist/drone-charging";
const DAV = SDKFactory({
  apiSeedUrls,
  kafkaSeedUrls
});
const drone = await DAV.getIdentity(droneDavId);

const needParams = new NeedParams({
  location: {
    lat: 32.050382,
    long: 34.766149
  },
  radius: 20,
  startAt: 1538995253092,
  dimensions: {
    length: 50,
    width: 15,
    height: 20
  },
  weight: 50000,
  batteryCapacity: 4,
  currentBatteryCharge: 45,
  energySource: enums.EnergySources.Solar,
  amenities: [enums.Amenities.Docking]
});
const need = await drone.publishNeed(needParams);
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">location</code>
      <div class="type required">required</div>
    </td>
    <td>The coordinates around which to search</td>
  </tr>
  <tr>
    <td>
      <code class="field">radius</code>
      <div class="type required">required</div>
    </td>
    <td>Radius in meters around the coordinates in which to search for charging services. Specified as an integer</td>
  </tr>
  <tr>
    <td>
      <code class="field">drone_type</code>
      <div class="type">optional</div>
    </td>
    <td>The manufacturer and/or the model number of the drone. An unformatted string</td>
  </tr>
  <tr>
      <td>
        <code class="field">plug_types</code>
        <div class="type required">required</div>
      </td>
      <td>The drone's plug type ID. See <a href="#plug-types">Plug Types</a> for possible values</td>
    </tr>
  <tr>
    <td>
      <code class="field">charge_pad_type</code>
      <div class="type">optional</div>
    </td>
    <td>The type of charging pad. Accepted values can be either <code>open</code> (for an unprotected pad) or <code>enclosed</code> (for an enclosed charging pad)</td>
  </tr>
  <tr>
    <td>
      <code class="field">droneport_protection_level</code>
      <div class="type">optional</div>
    </td>
    <td>Charging stations may also provide protection services for drones. This parameter specifies the level of protection given to a drone. See possible codes under <a href="#drone-protection-level">Drone Protection Level</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">startAt</code>
      <div class="type">optional</div>
    </td>
    <td>The time at which the requester would like to arrive at charger (if undefined, the arrival time will be ASAP). Specified as time in seconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">dimensions</code>
      <div class="type">optional</div>
    </td>
    <td>The minimum length, width, and height clearance that this drone requires from the charger. Specified as an object containing integers representing centimeters</td>
  </tr>
  <tr>
    <td>
      <code class="field">weight</code>
      <div class="type">optional</div>
    </td>
    <td>The weight of this drone. Specified as an integer representing grams</td>
  </tr>
  <tr>
    <td>
      <code class="field">batteryCapacity</code>
      <div class="type">optional</div>
    </td>
    <td>The drone's total battery capacity, specified in kWh</td>
  </tr>
  <tr>
    <td>
      <code class="field">currentBatteryCharge</code>
      <div class="type">optional</div>
    </td>
    <td>The drone's current battery charge level, as it was at the time the request was sent. Specified as an integer denoting percentage of full capacity</td>
  </tr>
  <tr>
    <td>
      <code class="field">energySource</code>
      <div class="type">optional</div>
    </td>
    <td>Limit the request to only receive bids from chargers using a specific source of energy. Specified as an energy source id. See <a href="#energy-sources">Energy Sources</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">amenities</code>
      <div class="type">optional</div>
    </td>
    <td>A list of amenities that need to be present at charging station. Specified as an array of amenity ids. See <a href="#amenities">Amenities</a></td>
  </tr>
</table>

# Need filter

Begin listening for incoming needs that match certain requirements. Typically this will be a charging station subscribing to incoming needs from drones.

## Arguments

```javascript
const { SDKFactory } = require("dav-js");
const { NeedFilterParams } = require("dav-js/dist/drone-charging");
const DAV = SDKFactory({
  apiSeedUrls,
  kafkaSeedUrls
});
const charger = await DAV.getIdentity(chargerDavId);

const needFilterParams = new NeedFilterParams({
  location: {
    lat: 32.050382,
    long: 34.766149
  },
  radius: 1000,
  maxDimensions: {
    length: 120,
    width: 80,
    height: 100
  }
});
const needs = await charger.needsForType(needFilterParams);
```

```typescript
import { SDKFactory } from "dav-js";
import { NeedFilterParams } from "dav-js/dist/drone-charging";
const DAV = SDKFactory({
  apiSeedUrls,
  kafkaSeedUrls
});
const charger = await DAV.getIdentity(chargerDavId);

const needFilterParams = new NeedFilterParams({
  location: {
    lat: 32.050382,
    long: 34.766149
  },
  radius: 1000,
  maxDimensions: {
    length: 120,
    width: 80,
    height: 100
  }
});
const needs = await charger.needsForType(needFilterParams);
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">location</code>
      <div class="type required">required</div>
    </td>
    <td>The coordinates around which to listen for bids</td>
  </tr>
  <tr>
    <td>
      <code class="field">radius</code>
      <div class="type required">required</div>
    </td>
    <td>Radius in meters around the coordinates in which to listen for bids. Specified as an integer</td>
  </tr>
  <tr>
    <td>
      <code class="field">maxDimensions</code>
      <div class="type">optional</div>
    </td>
    <td>The maximum length, width, and height clearance that this charger can accomodate. Specified as an object containing integers representing centimeters</td>
</table>

# Bid

A bid to provide a charging service. Typically sent from a charger to a drone.

## Arguments

```javascript
const { BidParams } = require('dav-js/dist/drone-charging');

needs.subscribe(need => {
  const bidParams = new BidParams({
    ttl: Date.now() + 3600000,
    price: [
      '15000000000000000000',
      { amount: '1000000000000000000', type: 'flat', description: 'Tax' },
    ],
    entranceLocation: {
      lat: 32.050382,
      long: 34.766149,
    },
    exitLocation: {
      lat: 32.050382,
      long: 34.766149,
    },
    locationName: 'IKEA parking lot B'
    locationNameLang: 'eng'
    locationStreet: 'King',
    locationHouseNumber: '372',
    locationCity: 'Charleston',
    locationPostalCode: '29401',
    locationCounty: 'Charleston',
    locationState: 'SC',
    locationCountry: 'USA',
    availableFrom: Date.now(),
    availableUntil: Date.now() + 3600000,
    energySource: EnergySources.Hydro,
    amenities: [Amenities.Park],
    provider: 'MegaCharge',
    manufacturer: 'Mega Tech LLC',
    model: 'MegaBolt',
  });
  const bid = await need.createBid(bidParams);
});
```

```typescript
import { BidParams } from 'dav-js/dist/drone-charging';

needs.subscribe((need: Need<NeedParams>) => {
  const bidParams = new BidParams({
    ttl: Date.now() + 3600000,
    price: [
      '15000000000000000000',
      { amount: '1000000000000000000', type: 'flat', description: 'Tax' },
    ],
    entranceLocation: {
      lat: 32.050382,
      long: 34.766149,
    },
    exitLocation: {
      lat: 32.050382,
      long: 34.766149,
    },
    locationName: 'IKEA parking lot B'
    locationNameLang: 'eng'
    locationStreet: 'King',
    locationHouseNumber: '372',
    locationCity: 'Charleston',
    locationPostalCode: '29401',
    locationCounty: 'Charleston',
    locationState: 'SC',
    locationCountry: 'USA',
    availableFrom: Date.now(),
    availableUntil: Date.now() + 3600000,
    energySource: EnergySources.Hydro,
    amenities: [Amenities.Park],
    provider: 'MegaCharge',
    manufacturer: 'Mega Tech LLC',
    model: 'MegaBolt',
  });
  const bid = await need.createBid(bidParams);
});
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">ttl</code>
      <div class="type">optional</div>
    </td>
    <td>This bid will expire at this time. Specified as time in seconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">price</code>
      <div class="type required">required</div>
    </td>
    <td>A single price or an array containing prices. If an array is given, all the prices will be charged (e.g., a price per kWh plus a flat rate tax). Each price is specified as either an string representing Vinci, or a price object containing amount, <a href="#price-types">price type</a>, and an optional description
    <br>1 DAV == 1e18 Vinci == 1000000000000000000 Vinci</td>
  </tr>
  <tr>
    <td>
      <code class="field">entranceLocation</code>
      <div class="type">optional</div>
    </td>
    <td>The coordinates of the charger entrance</td>
  </tr>
  <tr>
    <td>
      <code class="field">exitLocation</code>
      <div class="type">optional</div>
    </td>
    <td>The coordinates of the exit from to the charger</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationName</code>
      <div class="type">optional</div>
    </td>
    <td>A human readable name/description of the charger location (e.g., Cal Maritime Dock C)</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationNameLang</code>
      <div class="type">optional</div>
    </td>
    <td>The language used in <code>location_name</code>. Specified using the 3 letter <a href="https://en.wikipedia.org/wiki/ISO_639-3" target="blank">ISO 639-3</a> language code</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationHouseNumber</code>
      <div class="type">optional</div>
    </td>
    <td>The house number where the station is located</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationStreet</code>
      <div class="type">optional</div>
    </td>
    <td>The street name where the station is located</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationCity</code>
      <div class="type">optional</div>
    </td>
    <td>The city where the station is located</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationPostalCode</code>
      <div class="type">optional</div>
    </td>
    <td>The postal code where the station is located</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationCounty</code>
      <div class="type">optional</div>
    </td>
    <td>The county where the charger is located</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationState</code>
      <div class="type">optional</div>
    </td>
    <td>The state where the charger is located</td>
  </tr>
  <tr>
    <td>
      <code class="field">locationCountry</code>
      <div class="type">optional</div>
    </td>
    <td>The country where the charger is located</td>
  </tr>
  <tr>
    <td>
      <code class="field">availableFrom</code>
      <div class="type required">required</div>
    </td>
    <td>The time from which the charger can be made available for the drone requesting a charge. Specified as time in seconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">availableUntil</code>
      <div class="type">optional</div>
    </td>
    <td>The time until which the charger can be made available for the drone requesting a charge. Specified as time in seconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">energySource</code>
      <div class="type">optional</div>
    </td>
    <td>The source of the energy used by this charger. Specified as an energy source id. See <a href="#energy-sources">Energy Sources</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">amenities</code>
      <div class="type">optional</div>
    </td>
    <td>A list of amenities that are present at this charger. Specified as an array of amenity ids. See <a href="#amenities">Amenities</a></td>
  </tr>
  <tr>
      <td>
        <code class="field">plug_types</code>
        <div class="type required">required</div>
      </td>
      <td>The drone's plug type ID. See <a href="#plug-types">Plug Types</a> for possible values</td>
    </tr>
  <tr>
    <td>
      <code class="field">charge_pad_type</code>
      <div class="type">optional</div>
    </td>
    <td>The type of charging pad. Accepted values can be either <code>open</code> (for an outdoor pad) or <code>enclosed</code> (for an enclosed charging pad)</td>
  </tr>
  <tr>
    <td>
      <code class="field">droneport_protection_level</code>
      <div class="type">optional</div>
    </td>
    <td>Charging stations may also provide protection services for drones. This parameter specifies the level of protection given to a drone. See possible codes under <a href="#drone-protection-level">Drone Protection Level</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">provider</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the service provider or charging network operating this charger</td>
  </tr>
  <tr>
    <td>
      <code class="field">manufacturer</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the manufacturer of this charger</td>
  </tr>
  <tr>
    <td>
      <code class="field">model</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the model of this charger</td>
  </tr>
</table>

# Starting

A message sent by the service provider (the charger) to the service requester, notifying it that the mission has started

## Arguments

```javascript
const { StartingMessageParams } = require("dav-js/dist/drone-charging");

const startingMessage = new StartingMessageParams();
mission.sendMessage(startingMessage);
```

```typescript
import { StartingMessageParams } from "dav-js/dist/drone-charging";

const startingMessageParams = new StartingMessageParams();
mission.sendMessage(startingMessage);
```

<table class="arguments">
  <tr>
    <td>None</td>
  </tr>
</table>

# Request Status

A request message sent by either party, asking the other party for a status update

## Arguments

```javascript
const { StatusRequestMessageParams } = require("dav-js/dist/drone-charging");

const statusRequestMessage = new StatusRequestMessageParams({});
mission.sendMessage(statusRequestMessage);
```

```typescript
import { StatusRequestMessageParams } from "dav-js/dist/drone-charging";

const statusRequestMessage = new StatusRequestMessageParams({});
mission.sendMessage(statusRequestMessage);
```

<table class="arguments">
  <tr>
    <td>None</td>
  </tr>
</table>

# Provider Status

A status update sent by the service provider (usually a charging station) to the drone

## Arguments

```javascript
const { ProviderStatusMessageParams } = require("dav-js/dist/drone-charging");

const providerStatusMessage = new ProviderStatusMessageParams({
  chargeCompletionEstimatedTime: Date.now() + 5000
});
mission.sendMessage(providerStatusMessage);
```

```typescript
import { ProviderStatusMessageParams } from "dav-js/dist/drone-charging";

const providerStatusMessage = new ProviderStatusMessageParams({
  chargeCompletionEstimatedTime: Date.now() + 5000
});
mission.sendMessage(providerStatusMessage);
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">chargeCompletionEstimatedTime</code>
      <div class="type">optional</div>
    </td>
    <td>The estimated time at which charging will be complete. Specified as time in seconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
</table>

# Drone Status

A status update sent by the service requester (the drone) to the service provider (charger)

## Arguments

```javascript
const { DroneStatusMessageParams } = require("dav-js/dist/drone-charging");

const droneStatusMessage = new DroneStatusMessageParams({
  location: {
    lat: 32.050382,
    long: 34.766149
  }
});
mission.sendMessage(droneStatusMessage);
```

```typescript
import { DroneStatusMessageParams } from "dav-js/dist/drone-charging";

const droneStatusMessage = new DroneStatusMessageParams({
  location: {
    lat: 32.050382,
    long: 34.766149
  }
});
mission.sendMessage(droneStatusMessage);
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">location</code>
      <div class="type">optional</div>
    </td>
    <td>The current coordinates of the vehicle's location</td>
  </tr>
</table>

# Charging Arrival

A message sent by the service requester (the drone) to the service provider (charger), notifying it that it has arrived at the charger's location

## Arguments

```javascript
const { ChargingArrivalMessageParams } = require("dav-js/dist/drone-charging");

const chargingArrivalMessage = new ChargingArrivalMessageParams();
mission.sendMessage(chargingArrivalMessage);
```

```typescript
import { ChargingArrivalMessageParams } from "dav-js/dist/drone-charging";

const chargingArrivalMessage = new ChargingArrivalMessageParams();
mission.sendMessage(chargingArrivalMessage);
```

<table class="arguments">
  <tr>
    <td>None</td>
  </tr>
</table>

# Charging Started

A message sent by the service provider to the service requester, notifying it that charging has begun

## Arguments

```typescript
import { ChargingStartedMessageParams } from "dav-js/dist/drone-charging";

const chargingStartedMessage = new ChargingStartedMessageParams();
mission.sendMessage(chargingStartedMessage);
```

```javascript
const { ChargingStartedMessageParams } = require("dav-js/dist/drone-charging");

const chargingStartedMessage = new ChargingStartedMessageParams();
mission.sendMessage(chargingStartedMessage);
```

<table class="arguments">
  <tr>
    <td>None</td>
  </tr>
</table>

# Charging Complete

A message sent by the service provider to the service requester, notifying it that charging has completed

## Arguments

```javascript
const { ChargingCompleteMessageParams } = require("dav-js/dist/drone-charging");

const chargingCompleteMessage = new ChargingCompleteMessageParams();
mission.sendMessage(chargingCompleteMessage);
```

```typescript
import { ChargingCompleteMessageParams } from "dav-js/dist/drone-charging";

const chargingCompleteMessage = new ChargingCompleteMessageParams();
mission.sendMessage(chargingCompleteMessage);
```

<table class="arguments">
  <tr>
    <td>None</td>
  </tr>
</table>

# Plug Types

Below is a list of all the supported drone plug types.

<table class="reference connectors">
  <tr>
    <th>Plug Type ID</th>
    <th>Name</th>
    <th>Amp Tolerance to Plug Type</th>
  </tr>
  <tr>
    <td><code>jst_739</code></td>
    <td>JST Connector 739</td>
    <td>Up to 5 Amps</td>
  </tr>
  <tr>
    <td><code>bullet_2mm</code></td>
    <td>2mm bullet connectors</td>
    <td>Up to 20 Amps</td>
  </tr>
  <tr>
    <td><code>bullet_3_5mm</code></td>
    <td>3.5mm bullet connectors</td>
    <td>Up to 40 Amps</td>
  </tr>
  <tr>
    <td><code>xt_60_420</code></td>
    <td>XT60 connectors 420</td>
    <td>Up to 60 Amps</td>
  </tr>
  <tr>
    <td><code>t_plug_204</code></td>
    <td>T-Plug 204</td>
    <td>Up to 60 Amps</td>
  </tr>
  <tr>
    <td><code>ec3_306</code></td>
    <td>EC3 connector 306</td>
    <td>Up to 60 Amps</td>
  </tr>
  <tr>
    <td><code>bullet_4mm</code></td>
    <td>4mm bullet connectors</td>
    <td>Up to 70 Amps</td>
  </tr>
  <tr>
    <td><code>ec5_678</code></td>
    <td>EC5 connectors 678</td>
    <td>Up to 120 Amps</td>
  </tr>
  <tr>
    <td><code>bullet_6mm</code></td>
    <td>6mm bullet connectors</td>
    <td>Up to 120 Amps</td>
  </tr>
  <tr>
    <td><code>bullet_8mm</code></td>
    <td>8mm bullet connectors</td>
    <td>Up to 200 Amps</td>
  </tr>
</table>

# Energy Sources

The energy source used by the charging station.

<table class="energy_sources">
  <tr>
    <th>Source</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>grid</code></td>
    <td>Connected to the electrical grid and using an unspecified energy source, or an unspecified mix of energy source</td>
  </tr>
  <tr>
    <td><code>renewable</code></td>
    <td>Uses 100% renewable energy of an unspecified source, or a mix of different renewable energy sources</td>
  </tr>
  <tr>
    <td><code>solar</code></td>
    <td>Uses 100% solar energy</td>
  </tr>
  <tr>
    <td><code>wind</code></td>
    <td>Uses 100% wind energy</td>
  </tr>
  <tr>
    <td><code>hydro</code></td>
    <td>Uses 100% hydropower energy</td>
  </tr>
  <tr>
    <td><code>geothermal</code></td>
    <td>Uses 100% geothermal energy</td>
  </tr>
</table>

# Drone Protection Level

A charging station may also provide a certain level of protection from solids and/or liquids (mainly water and dust). The following table describes the standard levels of protection according to the International Protection Marking, IEC standard 60529.

The first digit indicates the level of protection that the enclosure provides against access to hazardous parts (e.g., electrical conductors, moving parts) and the ingress of solid foreign objects.

The second digit indicates the level of protection that the enclosure provides against harmful ingress of water.

For a full listing of all available codes, read more about <a href="https://en.wikipedia.org/wiki/IP_Code" target="_blank">International Protection Marking, IEC standard 60529</a>.

<table class="iplevel">
  <tr>
    <th>Rating Code</th>
    <th>IP Rating</th>
    <th>Solid Particle Protection</th>
    <th>Liquid Ingress Protection</th>
  </tr>
  <tr>
    <td><code>54</code></td>
    <td>IP54</td>
    <td>Protected from limited dust ingress</td>
    <td>Protected from water spray from any direction, limited ingress protection</td>
  </tr>
  <tr>
    <td><code>55</code></td>
    <td>IP55</td>
    <td>Protected from limited dust ingress</td>
    <td>Protected from low pressure water jets from any direction, limited ingress protection</td>
  </tr>
  <tr>
    <td><code>56</code></td>
    <td>IP56</td>
    <td>Protected from limited dust ingress</td>
    <td>Protected from high pressure water jets from any direction, limited ingress protection</td>
  </tr>
  <tr>
    <td><code>57</code></td>
    <td>IP57</td>
    <td>Protected from limited dust ingress</td>
    <td>Protected from immersion between 15 centimeters and 1 meter in depth, limited ingress protection</td>
  </tr>
  <tr>
    <td><code>58</code></td>
    <td>IP58</td>
    <td>Protected from limited dust ingress</td>
    <td>Protected from long term immersion up to a specified pressure, limited ingress protection</td>
  </tr>
  <tr>
    <td><code>60</code></td>
    <td>IP60</td>
    <td>Protected from total dust ingress</td>
    <td>Not protected from liquids, limited ingress protection</td>
  </tr>
  <tr>
    <td><code>61</code></td>
    <td>IP61</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from condensation, limited ingress protection</td>
  </tr>
  <tr>
    <td><code>62</code></td>
    <td>IP62</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from water spray less than 15 degrees from vertical, limited ingress protection</td>
  </tr>
  <tr>
    <td><code>63</code></td>
    <td>IP63</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from water spray less than 60 degrees from vertical, limited ingress protection</td>
  </tr>
    <tr>
    <td><code>64</code></td>
    <td>IP64</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from water spray from any direction, limited ingress protection</td>
  </tr>
    <tr>
    <td><code>65</code></td>
    <td>IP65</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from low pressure water jets from any direction, limited ingress protection</td>
  </tr>
    <tr>
    <td><code>66</code></td>
    <td>IP66</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from high pressure water jets from any direction, limited ingress protection</td>
  </tr>
    <tr>
    <td><code>67</code></td>
    <td>IP67</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from immersion between 15 centimeters and 1 meter in depth, limited ingress protection</td>
  </tr>
    <tr>
    <td><code>68</code></td>
    <td>IP68</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from long term immersion up to a specified pressure, limited ingress protection</td>
  </tr>
    <tr>
    <td><code>69k</code></td>
    <td>IP69K</td>
    <td>Protected from total dust ingress</td>
    <td>Protected from steam-jet cleaning, limited ingress protection</td>
  </tr>
</table>

# Amenities

A list of amenities can be included in both requests and responses.

<table class="reference">
  <tr>
    <th>ID</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>1</code></td>
    <td>Lodging</td>
  </tr>
  <tr>
    <td><code>2</code></td>
    <td>Dining</td>
  </tr>
  <tr>
    <td><code>3</code></td>
    <td>Restrooms</td>
  </tr>
  <tr>
    <td><code>4</code></td>
    <td>Docking</td>
  </tr>
  <tr>
    <td><code>5</code></td>
    <td>Park</td>
  </tr>
  <tr>
    <td><code>6</code></td>
    <td>WiFi</td>
  </tr>
  <tr>
    <td><code>7</code></td>
    <td>Shopping</td>
  </tr>
  <tr>
    <td><code>8</code></td>
    <td>Grocery</td>
  </tr>
</table>

# Price Types

Price types and their unique identifier.

<table class="price_types">
  <tr>
    <th>Price Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>kwh</code></td>
    <td>Cost per kWh</td>
  </tr>
  <tr>
    <td><code>second</code></td>
    <td>Cost per second</td>
  </tr>
  <tr>
    <td><code>minute</code></td>
    <td>Cost per minute</td>
  </tr>
  <tr>
    <td><code>hour</code></td>
    <td>Cost per hour</td>
  </tr>
  <tr>
    <td><code>day</code></td>
    <td>Cost per day</td>
  </tr>
  <tr>
    <td><code>week</code></td>
    <td>Cost per week</td>
  </tr>
  <tr>
    <td><code>flat</code></td>
    <td>The listed <code>price</code> is a flat price</td>
  </tr>
</table>
