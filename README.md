
# BLE Communication Module

**Module Name:** `movesense_class.py`  
**Description:** This module contains the `BLEClient` class, which provides methods to connect to and communicate with a Bluetooth Low Energy (BLE) GATT server. The module is primarily designed to interact with a Movesense device, capturing and processing various types of sensor data.

---

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Usage](#usage)
- [Class Details](#class-details)
- [TODO](#todo)
- [Author](#author)

---

## Overview

The `BLEClient` class facilitates connection and communication with a BLE GATT server, such as a Movesense device. It allows for capturing and processing data from various sensors, including ECG, heart rate, and IMU data. The class handles connecting, reading, writing, and notifying characteristics, making it a versatile tool for BLE device interactions.

## Installation

To install the necessary dependencies, run:

```bash
pip install -r requirements.txt
```

## Usage

### Connecting to a Device

To connect to a BLE device, initialize the `BLEClient` with the device's MAC address:

```python
from movesense_class import BLEClient

client = BLEClient(device_address="XX:XX:XX:XX:XX:XX")
await client.connect()
```

### Capturing Data

You can capture various types of data (ECG, heart rate, IMU) by sending appropriate requests:

```python
# Start capturing ECG data
await client.write_characteristic(request="ecg", hz=200, response=False)
await client.start_notify()
```

### Disconnecting

To disconnect from the device:

```python
await client.disconnect()
```

## Class Details

### BLEClient

- **Attributes:**
  - `device_address`: The MAC address of the BLE device.
  - `client`: The BleakClient object for BLE communication.
  - `battery_level`: The battery level of the device.
  - `is_connected`: Connection status.
  - `is_notifying`: Notification status.
  - `stop_capture_event`: Asyncio Event to stop capturing data.
  - `queue`: Asyncio FIFO queue to store captured data.
  - `case`: The request type (e.g., ECG, heart rate).
  - `hz`: The sample rate for the requests.

- **Methods:**
  - `connect()`: Connect to the specified BLE device.
  - `disconnect()`: Disconnect from the BLE device.
  - `set_battery_level()`: Read and set the device's battery level.
  - `write_characteristic()`: Perform write operations on the device.
  - `start_notify()`: Start receiving notifications from the device.
  - `stop_notify()`: Stop receiving notifications.
  - `get_active_services()`: Retrieve active GATT services on the device.

## TODO

- Process windows of 16 samples or more.
- Improve documentation using Sphinx.
- Remove file implementations and focus on in-memory data handling.

## Author

**Evangelos Katsoupis**

Date: [Insert Date]
