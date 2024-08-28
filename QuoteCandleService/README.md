# Quote Candlesticks Service

## Assumptions
#### In-Memory Window Management: 
The system stores candlestick data for a rolling 30-minute window in memory. When a new 31st window's data arrives, the oldest (first) window's data is removed from the storage, and the 31st window's data is added. If an ISIN appears in the first window and does not appear in subsequent windows, the previous window's candlestick data will be copied to the current window and so forth.

#### Delayed Visibility of New ISINs: 
If an ISIN appears for the first time in the 10th window, only the most recent 21 windows will be displayed. You will need to wait for 9 more minutes to see the complete 30-minute window for this ISIN.
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
