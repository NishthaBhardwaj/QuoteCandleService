# Quote Candlesticks Service

## Assumptions

## Solution Overview
#### In-Memory Window Management: 
The system stores candlestick data for a rolling 30-minute window in memory. When a new 31st window's data arrives, the oldest (first) window's data is removed from the storage, and the 31st window's data is added. If an ISIN appears in the first window and does not appear in subsequent windows, the previous window's candlestick data will be copied to the current window and so forth.

#### Delayed Visibility of New ISINs: 
If an ISIN appears for the first time in the 10th window, only the most recent 21 windows will be displayed. You will need to wait for 9 more minutes to see the complete 30-minute window for this ISIN.

#### Processing Quote Data Stream
The application continuously reads quotes from a live quote stream. These quotes are grouped into 1-minute windows and further organized by ISIN. After grouping, the quotes within each ISIN for the 1-minute window are processed to generate candlestick data.

Example of Grouping Quotes by ISIN within a 1-Minute Window:

DT4504734175 = [  QuoteEvent[data=Quote[isin=DT4504734175, price=1520.0769], type=QUOTE],
  QuoteEvent[data=Quote[isin=DT4504734175, price=1554.3462], type=QUOTE],
  QuoteEvent[data=Quote[isin=DT4504734175, price=1549.6154], type=QUOTE],
  QuoteEvent[data=Quote[isin=DT4504734175, price=1522.8846], type=QUOTE],
  QuoteEvent[data=Quote[isin=DT4504734175, price=1547.1538], type=QUOTE],
  QuoteEvent[data=Quote[isin=DT4504734175, price=1558.4231], type=QUOTE],
  QuoteEvent[data=Quote[isin=DT4504734175, price=1530.6923], type=QUOTE]
]

MP784T366845 = [  QuoteEvent[data=Quote[isin=MP784T366845, price=889.2943], type=QUOTE],
  QuoteEvent[data=Quote[isin=MP784T366845, price=897.4415], type=QUOTE],
  QuoteEvent[data=Quote[isin=MP784T366845, price=905.5886], type=QUOTE],
  QuoteEvent[data=Quote[isin=MP784T366845, price=896.7358], type=QUOTE],
  QuoteEvent[data=Quote[isin=MP784T366845, price=914.8829], type=QUOTE],
  QuoteEvent[data=Quote[isin=MP784T366845, price=890.0301], type=QUOTE],
  QuoteEvent[data=Quote[isin=MP784T366845, price=912.1773], type=QUOTE],
  QuoteEvent[data=Quote[isin=MP784T366845, price=891.3244], type=QUOTE],
  QuoteEvent[data=Quote[isin=MP784T366845, price=916.4716], type=QUOTE]
]

#### Processing to Generate Candlestick Data
After grouping the quotes by ISIN, they are passed downstream to process and generate the candlestick data. The candlestick data is calculated as follows:

{
  "openTimestamp": "2024-08-29T00:13:00",
  "openPrice": 1520.0769,
  "highPrice": 1558.4231,
  "lowPrice": 1520.0769,
  "closePrice": 1530.6923,
  "closeTimestamp": "2024-08-29T00:14:00"
}
{
  "openTimestamp": "2024-08-29T00:13:00",
  "openPrice": 889.2943,
  "highPrice": 916.4716,
  "lowPrice": 889.2943,
  "closePrice": 916.4716,
  "closeTimestamp": "2024-08-29T00:14:00"
}

### Building and deploying the application



### Building the application

The project uses [Gradle](https://gradle.org/) as a build tool. It already contains ./gradlew wrapper script, so there's no need to install Gradle.

To build the project execute the following command:

```bash
  ./gradlew build
```

#### Running the application

```bash
  ./gradlew bootRun
```


### Quick Start Guide

To skip the setup and build process, follow these steps:

1. **Navigate to the project home directory:**

   Open your terminal and go to the project home directory.

2. **Change to the `docker` directory:**
   Move to the `docker` directory within your project where your `docker-compose.yml` file is located.

   ```bash
   cd /path/to/project
   ```
   
   ```bash
   cd docker
   ```
3. **Start the services:**
   Run the following command to start the Partner app and the Quote service using Docker Compose.


  ```bash
  docker-compose up
  ```

**Note:** Make sure Docker and Docker Compose are installed and configured on your system before running these commands.


This will start the API container exposing the application's port
(set to `9990`).

### Testing the Application

1. First, access `http://localhost:9990/candlesticks/isins` to get a list of processed ISINs.
2. Copy one of the ISINs from the list and use it to make a request to `http://localhost:9990/candlesticks?isin=UY40736536O8`.
3. **Alternatively**, you can use the Swagger UI at `http://localhost:9990/swagger-ui.html` to test the endpoints interactively.

Use the following commands to clean up:

```bash
docker-compose rm
```

It clears stopped containers correctly. Might consider removing clutter of images too, especially the ones fiddled with:

```bash
docker images

docker image rm <image-id>
```
