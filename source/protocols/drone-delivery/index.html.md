---
title: Drone Delivery

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - python

toc_footers:
  - Documentation powered by <a href="https://github.com/lord/slate" target="blank">Slate</a>.

search: true
---

<p class="header-image"><img src="/images/drone_delivery/header.png" alt="Drone Delivery"></p>

# Drone Delivery Protocol

The following document describes the communication protocol for a cargo delivery service provided by an autonomous drone. It includes the format for both the request for a delivery service (also referred to as `need`) and the response sent by drones that `bid` on providing the delivery service.

For example, a user is looking for a drone to pick up a small tube containing corrosive materials from their doorstep and deliver it to a friend's backyard.

> Need

```shell
curl "discovery_endpoint_here" \
  --data "{ \
    \"pickup_at\": \"1513005534000\", \
    \"pickup_latitude\": \"32.787793\", \
    \"pickup_longitude\": \"-79.500593\", \
    \"dropoff_latitude\": \"32.937778\", \
    \"dropoff_longitude\": \"-79.500593\", \
    \"cargo_type\": \"11\", \
    \"hazardous_goods\": \"8\" \
  }"
```

```javascript
const discoveryEndPoint = "discovery_endpoint_here";

fetch(discoveryEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "pickup_at": "1513005534000",
    "pickup_latitude": "32.787793",
    "pickup_longitude": "-79.500593",
    "dropoff_latitude": "32.937778",
    "dropoff_longitude": "-79.500593",
    "cargo_type": "11",
    "hazardous_goods": "8",
  })
});
```

```python
import requests
payload = {
    "pickup_at": "1513005534000",
    "pickup_latitude": "32.787793",
    "pickup_longitude": "-79.500593",
    "dropoff_latitude": "32.937778",
    "dropoff_longitude": "-79.500593",
    "cargo_type": "11",
    "hazardous_goods": "8",
  }
requests.post("discovery_endpoint_here", data=payload)
```

In response, a drone might send back a bid with a price, the estimated time of arrival at the pickup location, and the estimated time of arrival at the dropoff location.

> Bid

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"need_id\": \"ae7bd8f67f3089c\", \
    \"expires_at\": \"1513005539000\", \
    \"price\": \"2000000000000000,20000000000000000\", \
    \"price_type\": \"second,flat\", \
    \"price_description\": \"Price per second,Tax\", \
    \"eta_pickup\": \"1513005719000\", \
    \"eta_dropoff\": \"1513006460000\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "need_id": "ae7bd8f67f3089c",
    "expires_at": "1513005539000",
    "price": "2000000000000000,20000000000000000",
    "price_type": "second,flat",
    "price_description": "Price per second,Tax",
    "eta_pickup": "1513005719000",
    "eta_dropoff": "1513006460000",
  })
});
```

```python
import requests
payload = {
    "need_id": "ae7bd8f67f3089c",
    "expires_at": "1513005539000",
    "price": "2000000000000000,20000000000000000",
    "price_type": "second,flat",
    "price_description": "Price per second,Tax",
    "eta_pickup": "1513005719000",
    "eta_dropoff": "1513006460000",
  }
requests.post("bidding_endpoint_here", data=payload)
```

# Need

A statement of need for a delivery service. Typically this will be sent by a user that is looking to deliver a package using a drone.

This request is sent to the decentralized discovery engine which responds with status `200` and a unique identifier for this request. The details of this request are then broadcasted to DAV entities that can provide this service. <a href="#bid">Bids</a> are later received as separate calls.

## Arguments

> Post request to a local/remote discovery endpoint

```shell
curl "discovery_endpoint_here" \
  --data "{ \
    \"pickup_at\": \"1513005534000\", \
    \"pickup_latitude\": \"32.787793\", \
    \"pickup_longitude\": \"-79.500593\", \
    \"dropoff_latitude\": \"32.937778\", \
    \"dropoff_longitude\": \"-79.500593\", \
    \"requester_name\": \"Jessie Bourne\", \
    \"requester_phone_number\": \"+1 415 123 5983\", \
    \"external_reference_id\": \"jb84723\", \
    \"cargo_type\": \"11\", \
    \"hazardous_goods\": \"8\", \
    \"ip_protection_level\": \"68\", \
    \"height\": \"8\", \
    \"width\": \"2\", \
    \"length\": \"2\", \
    \"weight\": \"50\", \
    \"insurance_required\": \"true\", \
    \"insured_value\": \"675\", \
    \"insured_value_currency\": \"USD\" \
  }"
```

```javascript
const discoveryEndPoint = "discovery_endpoint_here";

fetch(discoveryEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "pickup_at": "1513005534000",
    "pickup_latitude": "32.787793",
    "pickup_longitude": "-79.500593",
    "dropoff_latitude": "32.937778",
    "dropoff_longitude": "-79.500593",
    "requester_name": "Jessie Bourne",
    "requester_phone_number": "+1 415 123 5983",
    "external_reference_id": "jb84723",
    "cargo_type": "11",
    "hazardous_goods": "8",
    "ip_protection_level": "68",
    "height": "8",
    "width": "2",
    "length": "2",
    "weight": "50",
    "insurance_required": "true",
    "insured_value": "675",
    "insured_value_currency": "USD",
  })
});
```

```python
import requests
payload = {
    "pickup_at": "1513005534000",
    "pickup_latitude": "32.787793",
    "pickup_longitude": "-79.500593",
    "dropoff_latitude": "32.937778",
    "dropoff_longitude": "-79.500593",
    "requester_name": "Jessie Bourne",
    "requester_phone_number": "+1 415 123 5983",
    "external_reference_id": "jb84723",
    "cargo_type": "11",
    "hazardous_goods": "8",
    "ip_protection_level": "68",
    "height": "8",
    "width": "2",
    "length": "2",
    "weight": "50",
    "insurance_required": "true",
    "insured_value": "675",
    "insured_value_currency": "USD",
  }
requests.post("discovery_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">pickup_at</code>
      <div class="type">optional</div>
    </td>
    <td>
      The time at which the service requester would like the cargo to be picked up (if undefined, the pick up time will be ASAP). This should be Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a>
    </td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of the pickup location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of the pickup location</td>
  </tr>
  <tr>
    <td>
      <code class="field">dropoff_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of the dropoff destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">dropoff_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of the dropoff destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">requester_name</code>
      <div class="type">optional</div>
    </td>
    <td>The name of the person that is asking for the delivery service</td>
  </tr>
  <tr>
    <td>
      <code class="field">requester_phone_number</code>
      <div class="type">optional</div>
    </td>
    <td>The phone number of the person that is asking for the delivery service</td>
  </tr>
  <tr>
    <td>
      <code class="field">external_reference_id</code>
      <div class="type">optional</div>
    </td>
    <td>An identification string that might be needed for cargo dispatch</td>
  </tr>
  <tr>
    <td>
      <code class="field">cargo_type</code>
      <div class="type required">required</div>
    </td>
    <td>The type of cargo to be delivered. See the full list of options <a href="#cargo-types">here</a>
    </td>
  </tr>
  <tr>
    <td>
      <code class="field">hazardous_goods</code>
      <div class="type">optional</div>
    </td>
    <td>If the cargo contains hazardous goods, the hazardous goods class must be included. See the full list of options <a href="#hazardous-goods">here</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">ip_protection_level</code>
      <div class="type">optional</div>
    </td>
    <td>A certain level of protection to the cargo may be requested. See full list of options <a href="#ip-protection-level">here</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">height</code>
      <div class="type">optional</div>
    </td>
    <td>The height of the cargo. Specified as an integer representing centimeters</td>
  </tr>
  <tr>
    <td>
      <code class="field">width</code>
      <div class="type">optional</div>
    </td>
    <td>The width of the cargo. Specified as an integer representing centimeters</td>
  </tr>
  <tr>
    <td>
      <code class="field">length</code>
      <div class="type">optional</div>
    </td>
    <td>The length of the cargo. Specified as an integer representing centimeters</td>
  </tr>
  <tr>
    <td>
      <code class="field">weight</code>
      <div class="type">optional</div>
    </td>
    <td>The weight of the cargo. Specified as an integer representing grams</td>
  </tr>
  <tr>
    <td>
      <code class="field">insurance_required</code>
      <div class="type">optional</div>
    </td>
    <td>the service requester may require that the delivery be insured. Specified as a boolean (default is false)</td>
  </tr>
  <tr>
    <td>
      <code class="field">insured_value</code>
      <div class="type">optional</div>
    </td>
    <td>The declared value of the cargo to be insured</td>
  </tr>
  <tr>
    <td>
      <code class="field">insured_value_currency</code>
      <div class="type">optional</div>
    </td>
    <td>The currency in which the declared value is denoted. This should be specified as a 3-letter <a href="https://en.wikipedia.org/wiki/ISO_4217" target="blank">ISO 4217</a> code or <code>DAV</code></td>
  </tr>
</table>

# Bid

A bid to provide a delivery service. Typically sent from a delivery drone to the user who requested the service

## Arguments

> Post request to a local/remote bidding endpoint

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"need_id\": \"ae7bd8f67f3089c\", \
    \"expires_at\": \"1519093577681\", \
    \"price\": \"200000000000000\",20000000000000000”, \
    \"price_type\": \"secon\",flat”, \
    \"price_description\": \"Pric\" per second,Tax”, \
    \"eta_pickup\": \"1513005719000\", \
    \"eta_dropoff\": \"1513006460000\", \
    \"insured\": \"true\", \
    \"insurer_dav_id\": \"0x17325a469aef3472aa58dfdcf672881d79b31d58\", \
    \"drone_contact\": \"Megadronix\", \
    \"drone_manufacturer\": \"DXY\", \
    \"drone_model\": \"m6000\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "need_id": "ae7bd8f67f3089c",
    "expires_at": "1519093577681",
    "price": "2000000000000000,20000000000000000",
    "price_type": "second,flat",
    "price_description": "Price per second,Tax",
    "eta_pickup": "1513005719000",
    "eta_dropoff": "1513006460000",
    "insured": "true",
    "insurer_dav_id": "0x17325a469aef3472aa58dfdcf672881d79b31d58",
    "drone_contact": "Megadronix",
    "drone_manufacturer": "DXY",
    "drone_model": "m6000",
  })
});
```

```python
import requests
payload = {
    "need_id": "ae7bd8f67f3089c",
    "expires_at": "1519093577681",
    "price": "2000000000000000,20000000000000000",
    "price_type": "second,flat",
    "price_description": "Price per second,Tax",
    "eta_pickup": "1513005719000",
    "eta_dropoff": "1513006460000",
    "insured": "true",
    "insurer_dav_id": "0x17325a469aef3472aa58dfdcf672881d79b31d58",
    "drone_contact": "Megadronix",
    "drone_manufacturer": "DXY",
    "drone_model": "m6000",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">need_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'need' this bid is for. This ID arrives as part of the 'need' request</td>
  </tr>
  <tr>
    <td>
      <code class="field">expires_at</code>
      <div class="type required">required</div>
    </td>
    <td>This bid will expire at this time. Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">price</code>
      <div class="type required">required</div>
    </td>
    <td>A comma separated list of prices. Each price is specified as an integer representing Vinci (1 DAV token equals 1000000000000000000 Vinci equals 1e18 Vinci)</td>
  </tr>
  <tr>
    <td>
      <code class="field">price_type</code>
      <div class="type required">required</div>
    </td>
    <td>A list of price types describing the <code>price</code> parameter(s). Specified as a comma separated list. See <a href="#price-types">Price Types</a> for available values</td>
  </tr>
  <tr>
    <td>
      <code class="field">price_description</code>
      <div class="type required">required</div>
    </td>
    <td>A comma separated list of strings describing the <code>price</code> parameter(s) in human readable terms</td>
  </tr>
  <tr>
    <td>
      <code class="field">eta_pickup</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the pickup location. Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">eta_dropoff</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the dropoff location. Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">insured</code>
      <div class="type">optional</div>
    </td>
    <td>Is this delivery insured? Specified as a boolean (default is false)</td>
  </tr>
  <tr>
    <td>
      <code class="field">insurer_dav_id</code>
      <div class="type">optional</div>
    </td>
    <td>If this delivery is insured by another DAV Identity, include their ID here</td>
  </tr>
    <tr>
    <td>
      <code class="field">ip_protection_level</code>
      <div class="type">optional</div>
    </td>
    <td>A certain level of protection that a drone may provide to the cargo. See full list of options <a href="#ip-protection-level">here</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">drone_contact</code>
      <div class="type">optional</div>
    </td>
    <td>Human readable information regarding the drone (e.g <code>Megadronix Deliveries LTD. +31-338-594332</code>)</td>
  </tr>
  <tr>
    <td>
      <code class="field">drone_manufacturer</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the manufacturer of this drone</td>
  </tr>
  <tr>
    <td>
      <code class="field">drone_model</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the model of this drone</td>
  </tr>
</table>

# Select Bid

A selection of one bid that wins over the rest. Sent by the service requester (a user that is looking to deliver a package using a drone)

## Arguments

> Post request to a local/remote endpoint representing the service provider

```shell
curl "discovery_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\" \
  }"
```

```javascript
const discoveryEndPoint = "discovery_endpoint_here";

fetch(discoveryEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
  }
requests.post("discovery_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the selected 'bid'. This ID arrives as part of the 'bid' request</td>
  </tr>
</table>

# Starting

A message sent by the service provider (the delivery drone) to the service requester, notifying it that the delivery mission has started

## Arguments

> Post request to a local/remote endpoint representing the service requester

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\", \
    \"current_latitude\": \"32.785889\", \
    \"current_longitude\": \"-79.935569\", \
    \"current_altitude\": \"80\", \
    \"azimuth_angle\": \"15\", \
    \"eta_pickup\": \"1513005719000\", \
    \"eta_dropoff\": \"1513006460000\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
    "current_latitude": "32.785889",
    "current_longitude": "-79.935569",
    "current_altitude": "80",
    "azimuth_angle": "15",
    "eta_pickup": "1513005719000",
    "eta_dropoff": "1513006460000",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
    "current_latitude": "32.785889",
    "current_longitude": "-79.935569",
    "current_altitude": "80",
    "azimuth_angle": "15",
    "eta_pickup": "1513005719000",
    "eta_dropoff": "1513006460000",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'bid' that initiated the delivery mission</td>
  </tr>
  <tr>
    <td>
      <code class="field">current_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of of where the drone is currently located</td>
  </tr>
  <tr>
    <td>
      <code class="field">current_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of where the drone is currently located</td>
  </tr>
  <tr>
    <td>
      <code class="field">current_altitude</code>
      <div class="type required">required</div>
    </td>
    <td>The current altitude of the drone. Specified as meters above sea level. For example, if the drone is located 50 meters above sea level, the <code>current_altitude</code> will be <code>50</code></td>
  </tr>
  <tr>
    <td>
      <code class="field">azimuth_angle</code>
      <div class="type required">required</div>
    </td>
    <td>The horizontal angle the drone is currently aiming at. Specified as an integer representing degrees</td>
  </tr>
  <tr>
    <td>
      <code class="field">eta_pickup</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the pickup location. Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">eta_dropoff</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the dropoff location. Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
</table>

# Decline

A cancellation message sent by the service provider (the delivery drone) to the service requester, notifying it that the mission has been declined

## Arguments

> Post request to a local/remote endpoint representing the service requester

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'bid' that declined the delivery mission</td>
  </tr>
</table>

# Request Status

A request message sent by the service requester to the service provider, asking for a status update

## Arguments

> Post request to a local/remote endpoint representing the service provider

```shell
curl "discovery_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\" \
  }"
```

```javascript
const discoveryEndPoint = "discovery_endpoint_here";

fetch(discoveryEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
  }
requests.post("discovery_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'bid' that is currently on the delivery mission</td>
  </tr>
</table>

# Status

A status update sent by the service provider to the service requester

## Arguments

> Post request to a local/remote endpoint representing the service requester

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\", \
    \"current_latitude\": \"32.785889\", \
    \"current_longitude\": \"-79.935569\", \
    \"current_altitude\": \"80\", \
    \"azimuth_angle\": \"15\", \
    \"eta_pickup\": \"1513005719000\", \
    \"eta_dropoff\": \"1513006460000\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
    "current_latitude": "32.785889",
    "current_longitude": "-79.935569",
    "current_altitude": "80",
    "azimuth_angle": "15",
    "eta_pickup": "1513005719000",
    "eta_dropoff": "1513006460000",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
    "current_latitude": "32.785889",
    "current_longitude": "-79.935569",
    "current_altitude": "80",
    "azimuth_angle": "15",
    "eta_pickup": "1513005719000",
    "eta_dropoff": "1513006460000",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'bid' that is currently on the delivery mission</td>
  </tr>
  <tr>
    <td>
      <code class="field">current_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of of where the drone is currently located</td>
  </tr>
  <tr>
    <td>
      <code class="field">current_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of where the drone is currently located</td>
  </tr>
  <tr>
    <td>
      <code class="field">current_altitude</code>
      <div class="type required">required</div>
    </td>
    <td>The current altitude of the drone. Specified as meters above sea level. For example, if the drone is located 50 meters above sea level, the <code>current_altitude</code> will be <code>50</code></td>
  </tr>
  <tr>
    <td>
      <code class="field">azimuth_angle</code>
      <div class="type required">required</div>
    </td>
    <td>The horizontal angle the drone is currently aiming at. Specified as an integer representing degrees</td>
  </tr>
  <tr>
    <td>
      <code class="field">eta_pickup</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the pickup location. Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
  <tr>
    <td>
      <code class="field">eta_dropoff</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the dropoff location. Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
</table>

# Pickup Arrival

A message sent by the service provider to the service requester, notifying it that the drone has arrived at the pickup location (for the user to load the package)

## Arguments

> Post request to a local/remote endpoint representing the service requester

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'bid' that is currently on the delivery mission</td>
  </tr>
</table>

# Pickup Leave

A message sent by the service provider to the service requester, notifying it that the drone has left the pickup location

## Arguments

> Post request to a local/remote endpoint representing the service requester

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\", \
    \"eta_dropoff\": \"1513006460000\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
    "eta_dropoff": "1513006460000",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
    "eta_dropoff": "1513006460000",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'bid' that is currently on the delivery mission</td>
  </tr>
  <tr>
    <td>
      <code class="field">eta_dropoff</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the dropoff location. Specified as time in milliseconds since <a href="https://en.wikipedia.org/wiki/Unix_time" target="blank">Epoch/Unix Time</a></td>
  </tr>
</table>

# Dropoff Arrival

A message sent by the service provider to the service requester, notifying it that the drone has arrived at the dropoff location

## Arguments

> Post request to a local/remote endpoint representing the service requester

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'bid' that is currently on the delivery mission</td>
  </tr>
</table>

# Dropoff Leave

A message sent by the service provider to the service requester, notifying it that the drone has left the dropoff location

## Arguments

> Post request to a local/remote endpoint representing the service requester

```shell
curl "bidding_endpoint_here" \
  --data "{ \
    \"bid_id\": \"bv43nmw65eef03e\" \
  }"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "bid_id": "bv43nmw65eef03e",
  })
});
```

```python
import requests
payload = {
    "bid_id": "bv43nmw65eef03e",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">bid_id</code>
      <div class="type required">required</div>
    </td>
    <td>The unique identifier of the 'bid' that had just finished the delivery mission</td>
  </tr>
</table>

# Cargo Types

The following table describes the different types of cargo according to international delivery standards

<table class="cargo">
  <tr>
    <th>Class ID</th>
    <th>Type</th>
  </tr>
  <tr>
    <td><code>1</code></td>
    <td>Satchel 500g</td>
  </tr>
  <tr>
    <td><code>2</code></td>
    <td>Satchel 1kg</td>
  </tr>
  <tr>
    <td><code>3</code></td>
    <td>Satchel 3kg</td>
  </tr>
  <tr>
    <td><code>4</code></td>
    <td>Satchel 5kg</td>
  </tr>
  <tr>
    <td><code>5</code></td>
    <td>Unpackaged</td>
  </tr>
  <tr>
    <td><code>6</code></td>
    <td>Other/Misc</td>
  </tr>
  <tr>
    <td><code>7</code></td>
    <td>Envelope</td>
  </tr>
  <tr>
    <td><code>8</code></td>
    <td>Bag</td>
  </tr>
  <tr>
    <td><code>9</code></td>
    <td>Satchel</td>
  </tr>
  <tr>
    <td><code>10</code></td>
    <td>Crate</td>
  </tr>
  <tr>
    <td><code>11</code></td>
    <td>Tube</td>
  </tr>
  <tr>
    <td><code>12</code></td>
    <td>Pallet</td>
  </tr>
  <tr>
    <td><code>13</code></td>
    <td>Skid</td>
  </tr>
  <tr>
    <td><code>14</code></td>
    <td>Heavy Carton</td>
  </tr>
  <tr>
    <td><code>15</code></td>
    <td>Carton containing glass</td>
  </tr>
  <tr>
    <td><code>16</code></td>
    <td>Carton containing liquids</td>
  </tr>
  <tr>
    <td><code>17</code></td>
    <td>wine/beer/liquor carton</td>
  </tr>
  <tr>
    <td><code>18</code></td>
    <td>Carton</td>
  </tr>
</table>

# Hazardous Goods

The following table describes the different types of hazardous goods according to international delivery standards

<table class="hazardous">
  <tr>
    <th>Class ID</th>
    <th>Type</th>
  </tr>
  <tr>
    <td><code>1</code></td>
    <td>Explosives</td>
  </tr>
  <tr>
    <td><code>2</code></td>
    <td>Gases</td>
  </tr>
  <tr>
    <td><code>3</code></td>
    <td>Flammable and combustible liquids</td>
  </tr>
  <tr>
    <td><code>4</code></td>
    <td>Flammable solids</td>
  </tr>
  <tr>
    <td><code>5</code></td>
    <td>Oxidizing substances, organic peroxides</td>
  </tr>
  <tr>
    <td><code>6</code></td>
    <td>Toxic and infectious substances</td>
  </tr>
  <tr>
    <td><code>7</code></td>
    <td>Radioactive materials</td>
  </tr>
  <tr>
    <td><code>8</code></td>
    <td>Corrosives</td>
  </tr>
  <tr>
    <td><code>9</code></td>
    <td>Misc Hazardous</td>
  </tr>
</table>

# IP Protection Level

A drone may provide a certain level of protection from solids and/or liquids (mainly water and dust). The following table describes the standard levels of protection according to the International Protection Marking, IEC standard 60529.

The first digit indicates the level of protection that the enclosure provides against access to hazardous parts (e.g., electrical conductors, moving parts) and the ingress of solid foreign objects.

The second digit indicates the level of protection that the enclosure provides against harmful ingress of water.

For a full listing of all available codes, read more about <a href="https://en.wikipedia.org/wiki/IP_Code" target="_blank">International Protection Marking, IEC standard 60529</a>

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

# Price Types

Price types and their unique identifier

<table class="price_types">
  <tr>
    <th>Price Type</th>
    <th>Description</th>
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
