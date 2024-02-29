<h1 align="center">Big Data Inspection - NYC Taxi Trips</h1>

### 1. What datetime range does your data cover?  How many rows are there total?

```
with open('trip_data_8.csv', 'r') as f:
    Taxi = csv.DictReader(f)
    h = 0
    for file in Taxi:
        h += 1
        if h == 1:
            start_date = file[' pickup_datetime']
        else:
            end_date = file[' dropoff_datetime']
# Print the results
print(f'Total rows: {h}')
print(f'Starting Date: {start_date}, Ending Date: {end_date}')
```

```
Output :
Total rows: 12597109
Starting Date: 2013-08-30 07:57:00
Ending Date: 2013-08-11 10:16:00
```

### 2. What are the field names?  Give descriptions for each field.

| Field Name  | Description |
| ------------- | ------------- |
| **medallion** | This field likely represents a unique identifier for a taxi medallion or license. |
| **hack_license**| This field might contain information related to the taxi driver's license or permit.|
| **vendor_id** | This field could indicate the identifier of the taxi service provider or vendor.|
| **rate_code** | This field probably corresponds to the rate code applied during the trip (e.g., standard rate, flat rate, etc.).|
| **store_and_fwd_flag** | This field may indicate whether the trip data was stored locally before being forwarded to a central system.|
| **pickup_datetime** | This field represents the date and time when the trip started (pickup time).|
| **dropoff_datetime**| This field indicates the date and time when the trip ended (dropoff time).|
| **passenger_count**| This field likely contains the number of passengers in the taxi during the trip.|
| **trip_time_in_secs**| This field records the duration of the trip in seconds.|
| **trip_distance**| This field represents the distance traveled during the trip (e.g., in miles or kilometers).|
| **pickup_longitude**| This field contains the longitude coordinate of the pickup location.|
| **pickup_latitude**| This field contains the latitude coordinate of the pickup location.|
| **dropoff_longitude**| This field contains the longitude coordinate of the dropoff location.|
| **dropoff_latitude**| This field contains the latitude coordinate of the dropoff location.|

### 3. Give some sample data for each field.

```
   with open('trip_data_8.csv', 'r') as f:
    Taxi = csv.DictReader(f)
    h = 0
    for i in Taxi:
        h += 1
        if h < 6:
            print(i)
        else:
            break
```

```
Output :
{'medallion': '3418135604CD3F357DD9577AF978C5C0', ' hack_license': 'B25386A1F259C87449430593E904FDBC', ' vendor_id': 'VTS', ' rate_code': '1', ' store_and_fwd_flag': '', ' pickup_datetime': '2013-08-30 07:57:00', ' dropoff_datetime': '2013-08-30 08:30:00', ' passenger_count': '5', ' trip_time_in_secs': '1980', ' trip_distance': '14.58', ' pickup_longitude': '-73.791359', ' pickup_latitude': '40.645657', ' dropoff_longitude': '-73.922501', ' dropoff_latitude': '40.758766'}
{'medallion': '6D3B2A7682C30DCF64F3F12976EF93B6', ' hack_license': 'A603A9D5FAA46E8FF2A97A143328D938', ' vendor_id': 'CMT', ' rate_code': '1', ' store_and_fwd_flag': 'N', ' pickup_datetime': '2013-08-30 23:26:23', ' dropoff_datetime': '2013-08-30 23:46:01', ' passenger_count': '2', ' trip_time_in_secs': '1177', ' trip_distance': '11.00', ' pickup_longitude': '-73.862724', ' pickup_latitude': '40.769062', ' dropoff_longitude': '-73.976845', ' dropoff_latitude': '40.764595'}
{'medallion': '6D49E494913752B75B2685E0019FF3D5', ' hack_license': '3F0BFE90A5D71741840B25600A89E225', ' vendor_id': 'CMT', ' rate_code': '1', ' store_and_fwd_flag': 'N', ' pickup_datetime': '2013-08-30 09:18:10', ' dropoff_datetime': '2013-08-30 09:24:08', ' passenger_count': '1', ' trip_time_in_secs': '357', ' trip_distance': '.80', ' pickup_longitude': '-73.991653', ' pickup_latitude': '40.750324', ' dropoff_longitude': '-73.98642', ' dropoff_latitude': '40.742924'}
{'medallion': '4C4A0AFC432A1A87E97ED8F18403FF6E', ' hack_license': 'BA20A20E2CF85EF7B00162D711394C7E', ' vendor_id': 'CMT', ' rate_code': '1', ' store_and_fwd_flag': 'N', ' pickup_datetime': '2013-08-26 23:27:11', ' dropoff_datetime': '2013-08-26 23:42:49', ' passenger_count': '4', ' trip_time_in_secs': '938', ' trip_distance': '7.70', ' pickup_longitude': '-73.975372', ' pickup_latitude': '40.756237', ' dropoff_longitude': '-73.867119', ' dropoff_latitude': '40.721886'}
{'medallion': '1258CA1DF5E2A9E9A9F7848408A7AAEB', ' hack_license': '8C14DCF69CAA2A9A0DFAFD99E00536A1', ' vendor_id': 'CMT', ' rate_code': '1', ' store_and_fwd_flag': 'N', ' pickup_datetime': '2013-08-29 10:57:56', ' dropoff_datetime': '2013-08-29 11:19:06', ' passenger_count': '2', ' trip_time_in_secs': '1270', ' trip_distance': '2.10', ' pickup_longitude': '-73.99102', ' pickup_latitude': '40.750912', ' dropoff_longitude': '-73.996727', ' dropoff_latitude': '40.767578'}
```

### 4. What MySQL data types / len would you need to store each of the fields?
#### a. int(xx), varchar(xx),date,datetime,bool, decimal(m,d)

| Field Name  | Data Type  |
| ------------- | ------------- |
|medallion| VARCHAR(50)|
|hack_license| VARCHAR(50)|
|vendor_id| VARCHAR(10)|
|rate_code| INT(2)|
|store_and_fwd_flag| VARCHAR(15)|
|pickup_datetime| DATETIME, format: YYYY-MM-DD HH:MM:SS|
|dropoff_datetime| DATETIME, format: YYYY-MM-DD HH:MM:SS|
|passenger_coun| INT(2)|
|trip_time_in_secs| INT(6)|
|trip_distance| FLOAT(5,2)|
|pickup_longitude| DECIMAL(8,6)|
|pickup_latitude| DECIMAL(8,6)|
|dropoff_longitude| DECIMAL(8,6)|
|dropoff_latitude| DECIMAL(8,6)|

### 5. What is the geographic range of your data (min/max - X/Y)?
#### a. Plot this (approximately on a map)

```
pickup_lat_min = 90
pickup_lat_max = -90
pickup_long_min = 180
pickup_long_max = -180
dropoff_lat_min = 90
dropoff_lat_max = -90
dropoff_long_min = 180
dropoff_long_max = -180
i = 0
with open('trip_data_8.csv', 'r') as df:
    h = csv.DictReader(df)
    for row in h:
        if i > 0:
            try:
                pickup_lat = float(row[' pickup_latitude'])
                pickup_long = float(row[' pickup_longitude'])
                dropoff_lat = float(row[' dropoff_latitude'])
                dropoff_long = float(row[' dropoff_longitude'])
                if (-74.4 <= pickup_long <= -72.05 and 40.4 <= pickup_lat<= 41.02):
                    pickup_lat_min = min(pickup_lat_min, pickup_lat)
                    pickup_lat_max = max(pickup_lat_max, pickup_lat)
                    pickup_long_min = min(pickup_long_min, pickup_long)
                    pickup_long_max = max(pickup_long_max, pickup_long)
                if dropoff_long is not None and (-74.5 <= dropoff_long <= -72.02 and 40.75 <= dropoff_lat<= 41):
                    dropoff_lat_min = min(dropoff_lat_min, dropoff_lat)
                    dropoff_lat_max = max(dropoff_lat_max, dropoff_lat)
                    dropoff_long_min = min(dropoff_long_min, dropoff_long)
                    dropoff_long_max = max(dropoff_long_max, dropoff_long)
            except ValueError:
                continue
        i+=1
        if i > 1000000000:
            break
print("pickup_latitude_min: " ,pickup_lat_min)
print("pickup_longitude_min: ",pickup_long_min)

print("pickup_latitude_max: ",pickup_lat_max)
print("pickup_longitude_max: ",pickup_long_max)

print("dropoff_latitude_min: ",dropoff_lat_min)
print("dropoff_longitude_min: ",dropoff_long_min)

print("dropoff_latitude_max: ",dropoff_lat_max)
print("dropoff_longitude_max: ",dropoff_long_max)
```

```
Output :
pickup_latitude_min:  40.402771
pickup_longitude_min:  -74.398354
pickup_latitude_max:  41.019707
pickup_longitude_max:  -72.113411
dropoff_latitude_min:  40.75
dropoff_longitude_min:  -74.5
dropoff_latitude_max:  40.999908
dropoff_longitude_max:  -72.307014
```
### 6. What is the average overall computed trip distance? (You should use Haversine Distance)
####   a. Draw a histogram of the trip distances binned anyway you see fit.
### 7. What are the distinct values for each field? (If applicable)
### 8. For other numeric types besides lat and lon, what are the min and max values?
### 9. Create a chart which shows the average number of passengers each hour of the day. (X axis should have 24 hours)
### 10. Create a new CSV file which has only one out of every thousand rows.
### 11. Repeat step 9 with the reduced dataset and compare the two charts.

