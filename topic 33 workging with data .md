### InfluxDB Administration POC Document (36-Hour Course)


---

## **3. Working with Data (6 Hours)**

### **What:** Managing data operations in InfluxDB including writing, querying, retention policies, aggregation, downsampling, continuous queries, and visualization.

### **How:** Using CLI, HTTP API, client libraries (Python, Java, Node.js), InfluxDB dashboards, and Grafana.

### **Where:** CLI, InfluxDB Python client, Java client, Node.js client, HTTP API, Grafana dashboards.

### **Architecture Workflow:**
```
                     +-------------------+
                     |    InfluxDB CLI   |
                     +-------------------+
                              |
                              V
                     +-------------------+
                     |   Write API / CLI |
                     +-------------------+
                              |
                              V
                     +-------------------+
                     |  Storage Engine   |
                     | (Shards & Retention)|
                     +-------------------+
                              |
                              V
                     +-------------------+
                     |  Query Engine     |
                     |  (Flux Processing)|
                     +-------------------+
                              |
                              V
                     +-------------------+
                     |  Visualization    |
                     | (Grafana / Custom)|
                     +-------------------+
```

### **Writing Data**

#### **1. Using CLI:**
```bash
influx write -b example-bucket -o example-org 'temperature,location=office value=25.5'
```

#### **2. Using HTTP API:**
```bash
curl -XPOST "http://localhost:8086/api/v2/write?org=example-org&bucket=example-bucket&precision=s" \
    --header "Authorization: Token my-token" \
    --data-raw "temperature,location=office value=25.5"
```

#### **3. Using Python Client:**
```python
from influxdb_client import InfluxDBClient, Point

client = InfluxDBClient(url="http://localhost:8086", token="my-token", org="example-org")
write_api = client.write_api()

point = Point("temperature").tag("location", "office").field("value", 25.5)
write_api.write(bucket="example-bucket", org="example-org", record=point)
```

#### **4. Using Java Client:**
```java
InfluxDBClient client = InfluxDBClientFactory.create("http://localhost:8086", "my-token".toCharArray());
WriteApiBlocking writeApi = client.getWriteApiBlocking();

Point point = Point.measurement("temperature")
                  .addTag("location", "office")
                  .addField("value", 25.5);

writeApi.writePoint("example-bucket", "example-org", point);
```

#### **5. Using Node.js Client:**
```javascript
const { InfluxDB, Point } = require('@influxdata/influxdb-client')

const client = new InfluxDB({ url: 'http://localhost:8086', token: 'my-token' })
const writeApi = client.getWriteApi('example-org', 'example-bucket')

const point = new Point('temperature')
  .tag('location', 'office')
  .floatField('value', 25.5)

writeApi.writePoint(point)
writeApi.close()
```

### **Explanation:**
- InfluxDB supports data writing through various clients including **CLI, HTTP API, Python, Java, Node.js**.
- Data is stored in **buckets**, organized by **measurements** (tables) and **tags** (metadata).
- Various clients support batch writing for improved efficiency.

### **Live Use Case:**
- A **smart home monitoring system** that collects temperature, humidity, and energy consumption data from various sensors.
- Data is written to InfluxDB via various clients (Python, Java, Node.js) for visualization in Grafana.

### **Outcome:**
- Understanding of writing data via CLI, HTTP API, Python, Java, and Node.js.
- Practical knowledge of bucket creation, data writing, and client interaction.
- Ability to monitor real-time data using Grafana.

---
