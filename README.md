
Movesense_Bleak

A custom and simple wrapper API of BLEAK, to comunicate with a movesense BLE multisensor device.

```
from movesense_class import *
from util_fun import *
import asyncio
```

Scan for movesense ble devices

```
devices = await scan_movesense_address(timeout=5)
if devices:
    for i in devices:
        print("mac: " + str(i[0]))
        address = i[0]

# address = "0C:8C:DC:41:DB:EB"
```

Create and set a BLEClient with its address.

If client is setted, then  mv_client.set_client() sould return true

```
mv_client = BLEClient()
mv_client.set_device_address(address)

print(f"Client setted: {mv_client.set_client()}")
```

> ***NODATA**print(f"Client setted: {mv_client.set_client()}")

Set request type and rate if needed


| Data type     |Electrocardiogram  | HeartRate | Accelerometer | Magnetometer  | Gyroscope | Acc+Gyro  | Acc+Magn  | Acc+Gyro+Magn | Temperature   | Stop      |
| :---:           |:---:                | :---:       | :---:           | :---:           | :---:       | :---:       | :---:       | :---:           | :---:           | :---:       |
| Request name  | 'ecg'             | 'hr'      | 'acc'         | 'magn'        | 'gyro'    | 'imu6'    | 'imu6m'   | 'imu9'        | 'temp'        | 'stop'    |
| Valid rates   |         X_2         |          | X_1           | x_1           | x_1       | x_1       | x_1       | x_1           |               |           |

|X_1 Rates  | 13 | 26 | 52 | 104 | 208 | 416 | 833 | 1666|
| ---       |---|---|---|---|---|---|---|---|
|**X_2 Rates** | **125** | **128** | **200** | **250** | **256** | **500** | **512** | - |

```
request_type = "ecg"
rate = 200
```

Connect to the device and write the request.

Return true if succeed.

You can change the request later on but have to disconnect at the end.

```
# Connect
await mv_client.connect()
print(f"Connected : {mv_client.is_connected}")
await asyncio.sleep(0.1)

# Write Characteristic
res = await mv_client.write_characteristic(request_type,rate)
print(f"Wrote Characteristic: {res}")
```

Start recevied notifications.

While notifying data will be stored at the queue (mv_client.queue).

You have to stop the recieving later on (or just disconect).

```
await mv_client.start_notify()
print(f'Notifying: {mv_client.is_notifying}')
```

While notifying data are stored at the queue.

Run the below multiple times to see the size change

```
print(f"Current queue size: {mv_client.queue.qsize()}")
```

At this stage you can change the request by writing a new one and the new data recieved will be stored and the queue.

Beware the index of the new ones.

```
# request_type = "ecg"
rate = 250
res = await mv_client.write_characteristic(request_type,rate)
print(f"Wrote Characteristic: {res}")
```

Asuming you want to stop recieving notifications:

```
await mv_client.stop_notify()
print(f'Notifying: {mv_client.is_notifying}')
```

At the end of the process write a stop request and disconect from the device.

```
await mv_client.disconnect()
print(f"Connected : {mv_client.is_connected}")
```

Empty the current queue and plot the ecg data

```
import matplotlib.pyplot as plt

data = await ecg_from_queue(queue = mv_client.queue, save_timestamps = False)

plt.plot(data)
plt.show()
```

