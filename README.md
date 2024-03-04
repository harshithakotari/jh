# New York Taxi Data

## 1. What datetime range does your data cover?  How many rows are there total?

```
f = open('trip_data_9.csv', 'r') 
data = csv.DictReader(f)
c= 0 
for file in data:
    c+=1
    if c ==1:
        start_date = file[' pickup_datetime']
    else:
        end_date = file[' dropoff_datetime']
print('Total Number of rows: ',c)
print(f'Start Date:{start_date}, End Date: {end_date}')
```




## 2. What are the field names?  Give descriptions for each field.

```
Description:
medallion            : It is a unique identifier for the taxi cab
hack_license         : A unique license ID assigned for the taxi driver
vendor_id            : A unique identification provided to the taxi company
rate_code            : The rate code for the trip (e.g. 1=standard rate; 2=JFK                              airport rate; 3= Newark; 4=Nassau or Westchester; 5                                  =Negotiated fare; 6 =Group ride .)
store_and_fwd_flag   : A flag indicating whether the trip data was held in vehicle                          memory before sending to the vendor (Y=store and forward;                            N=not a store and forward trip)
pickup_datetime      : The date and time when the passengers were picked up
dropoff_datetime     : The date and time when the passengers were dropped off
passenger_coun       : Total number of passengers onboard the vehicle for each trip                         (driver entered value)
trip_time_in_secs    : Duration of each trip in seconds
trip_distance        : Each trip distance in miles
pickup_longitude     : Longitude coordinate of the pickup location
pickup_latitude      : Latitude coordinate of the pickup location
dropoff_longitude    : Longitude coordinate of the dropoff location
dropoff_latitude     : Latitude coordinate of the dropoff location
```
## 3. Give some sample data for each field.

```
f = open('trip_data_9.csv', 'r') 
data = csv.DictReader(f)
c = 0
for i in data:
    c+=1
    if c<6:
        print(i)
    else:
        break
```

```
Output :

```


## 4. What MySQL data types / len would you need to store each of the fields?
### a. int(xx), varchar(xx),date,datetime,bool, decimal(m,d)

```
medallion            :     VARCHAR(32)
hack_license         :     VARCHAR(32)
vendor_id            :     VARCHAR(3)
rate_code            :     INT(2)
store_and_fwd_flag   :     VARCHAR(1)
pickup_datetime      :     DATETIME,    format    :    YYYY-MM-DD HH:MM:SS
dropoff_datetime     :     DATETIME,    format    :    YYYY-MM-DD HH:MM:SS
passenger_coun       :     INT(2)
trip_time_in_secs    :     INT(6)
trip_distance        :     FLOAT(5,2)
pickup_longitude     :     DECIMAL(8,6)
pickup_latitude      :     DECIMAL(8,6)
dropoff_longitude    :     DECIMAL(8,6)
dropoff_latitude     :     DECIMAL(8,6)
```

## 5. What is the geographic range of your data (min/max - X/Y)?
### a. Plot this (approximately on a map)

```
5.
pickup_lat_min = 90
pickup_lat_max = -90
pickup_long_min = 180
pickup_long_max = -180
dropoff_lat_min = 90
dropoff_lat_max = -90
dropoff_long_min = 180
dropoff_long_max = -180
n = 0
with open('trip_data_9.csv', 'r') as file:
    r = csv.DictReader(file)
    for row in r:
        if n > 0:
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
        n+=1
        if n > 1000000000:
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


## 6. What is the average overall computed trip distance? (You should use Haversine Distance)
###   a. Draw a histogram of the trip distances binned anyway you see fit.

```
f = open('trip_data_9.csv', 'r') 
data = csv.DictReader(f)
avg_dist = []
for file in data:
    if file[' pickup_longitude'] == '' or file[' pickup_latitude'] == '' or file[' dropoff_longitude']=='' or file[' dropoff_latitude']=='':
        continue
    else:
        d = round(haversine(float(file[' pickup_longitude']),float(file[' pickup_latitude']),float(file[' dropoff_longitude']),float(file[' dropoff_latitude'])),2)
        avg_dist.append(d)

from math import radians, cos, sin, asin, sqrt
def haversine(lon1, lat1, lon2, lat2):
   
    # convert decimal degrees to radians 
    lon1, lat1, lon2, lat2 = map(radians, [lon1, lat1, lon2, lat2])

    # haversine formula 
    dlon = lon2 - lon1 
    dlat = lat2 - lat1 
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * asin(sqrt(a)) 
    r = 3958.8 # Radius of earth in kilometers. Use 3958.8 for miles.
    return c * r

import matplotlib.pyplot as plt

num_bins = 50  
plt.hist(avg_dist, bins=num_bins, edgecolor='k', range=(0.0, 5.02))
plt.xlabel('Average Distance')
plt.ylabel('Frequency')
plt.title('Histogram Plot')
plt.show()
```


Output :



## 7. What are the distinct values for each field? (If applicable) 

```
distinct_values = defaultdict(set)

with open('trip_data_9.csv', 'r') as csv_file:
    csv_reader = csv.DictReader(csv_file)
    c= 0
    for row in csv_reader:
        c+=1
        if c<1000:
            for key, value in row.items():
                distinct_values[key].add(value)
        else:
            break

# Print distinct values for each field
for field, values in distinct_values.items():
    print(f"{field}: {', '.join(values)}")
```

```
Output :
```


## 8. For other numeric types besides lat and lon, what are the min and max values?

```
numeric_fields = [' passenger_count', ' trip_time_in_secs', ' trip_distance']
min_max_values = {}

for field in numeric_fields:
    values = [float(row[field]) for row in csv.DictReader(open('trip_data_9.csv', 'r'))]
    min_max_values[field] = {'min': min(values), 'max': max(values)}

# Print min and max values for numeric fields
for field, values in min_max_values.items():
    print(f"{field}: Min - {values['min']}, Max - {values['max']}")
```

```
Output :
```


## 9. Create a chart which shows the average number of passengers each hour of the day. (X axis should have 24 hours)

```
passengers_per_hour = defaultdict(list)

with open('trip_data_9.csv', 'r') as csv_file:
    csv_reader = csv.DictReader(csv_file)
    
    for row in csv_reader:
        pickup_hour = int(row[' pickup_datetime'].split()[1].split(':')[0])
        passengers_per_hour[pickup_hour].append(int(row[' passenger_count']))

average_passengers_per_hour = {hour: sum(passengers) / len(passengers) for hour, passengers in passengers_per_hour.items()}

# Create chart
plt.bar(average_passengers_per_hour.keys(), average_passengers_per_hour.values())
plt.xlabel('Hour of the Day')
plt.ylabel('Average Number of Passengers')
plt.title('Average Number of Passengers Each Hour of the Day')
plt.xticks(range(24))
plt.show()
```

Output :



## 10. Create a new CSV file which has only one out of every thousand rows.

```
10.
with open('trip_data_9.csv', 'r') as input_file, open('reduced_data.csv', 'w', newline='') as output_file:
    csv_reader = csv.reader(input_file)
    csv_writer = csv.writer(output_file)
    
    for i, row in enumerate(csv_reader):
        if i % 1000 == 0:
            csv_writer.writerow(row)
```
## 11. Repeat step 9 with the reduced dataset and compare the two charts.

```
11.
passengers_per_hour = defaultdict(list)

with open('reduced_data.csv', 'r') as csv_file:
    csv_reader = csv.DictReader(csv_file)
    
    for row in csv_reader:
        pickup_hour = int(row[' pickup_datetime'].split()[1].split(':')[0])
        passengers_per_hour[pickup_hour].append(int(row[' passenger_count']))

average_passengers_per_hour = {hour: sum(passengers) / len(passengers) for hour, passengers in passengers_per_hour.items()}

# Create chart
plt.bar(average_passengers_per_hour.keys(), average_passengers_per_hour.values())
plt.xlabel('Hour of the Day')
plt.ylabel('Average Number of Passengers')
plt.title('Average Number of Passengers Each Hour of the Day')
plt.xticks(range(24))
plt.show()
```

Output :

