# COVID-19 Data Tracker

A comprehensive COVID-19 data processing pipeline that analyzes COVID-19 data from multiple sources. This project implements a batch processing architecture using Spring Boot, Apache Spark, HDFS, and Hive for data processing and analysis.

## 🏗️ Architecture Overview

```
Data Sources → Data Ingestion → HDFS (Raw) → Spark Processing → Hive → API → React Frontend
     ↓              ↓              ↓              ↓              ↓        ↓         ↓
   Our World    Spring Boot    HDFS Storage   Apache Spark   Apache    REST API   React UI
   in Data      Ingestion      (Data Lake)    (ETL Jobs)     Hive      (JSON)     (Charts)
```

### Key Components

- **Data Sources**: Our World in Data COVID-19 dataset
- **Data Ingestion**: Spring Boot service for downloading and storing data in HDFS
- **Data Processing**: Apache Spark jobs for ETL processing
- **Data Storage**: HDFS for raw data storage
- **Data Warehouse**: Apache Hive for analytical queries
- **API Layer**: Spring Boot REST API
- **Frontend**: React with TypeScript and Recharts

## 🚀 Quick Start

### Prerequisites
- Docker and Docker Compose
- Java 17+
- Node.js 18+ (for local development)

### Running with Docker Compose

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd Covid19_Data_Tracker
   ```

2. **Start all services:**
   ```bash
   docker-compose up -d
   ```

3. **Run the Spark job to process data:**
   ```bash
   docker exec spark-job /opt/spark/run-covid-job.sh
   ```

4. **Access the applications:**
   - **Frontend**: http://localhost:3000
   - **Backend API**: http://localhost:8081/api
   - **HDFS NameNode**: http://localhost:9870
   - **Hive Server**: http://localhost:10000
   - **Spark Master**: http://localhost:8080

### Local Development

1. **Start the backend:**
   ```bash
   mvn spring-boot:run
   ```

2. **Start the frontend:**
   ```bash
   cd covid19-visualization
   npm install
   npm start
   ```

## 📊 Features

### Data Processing
- **Batch Data Ingestion**: Automated data collection from Our World in Data
- **ETL Pipeline**: Apache Spark jobs for data transformation and cleaning
- **Data Quality**: Validation and error handling for data integrity
- **Schema Enforcement**: Consistent data schema across all countries

### Analytics
- **COVID-19 Metrics**: Cases, deaths, recoveries, and trends
- **Country-wise Analysis**: Data analysis by country
- **Time Series Analysis**: Trend visualization over time
- **Statistical Analysis**: Summary statistics and data insights

### Visualization
- **Interactive Dashboards**: Real-time charts and graphs
- **Time Series Analysis**: Trend visualization over time
- **Geographic Data**: Country and regional comparisons
- **Comparative Analysis**: Side-by-side comparisons of different metrics

## 🔧 Configuration

### Application Properties
The application configuration is in `src/main/resources/application.yml`:

```yaml
# Data Sources
data-sources:
  our-world-in-data:
    url: https://covid.ourworldindata.org/data/owid-covid-data.json

# HDFS Configuration
hdfs:
  namenode: hdfs://namenode:9000
  base-path: /covid19-data

# Hive Configuration
hive:
  jdbc-url: jdbc:hive2://hive-server:10000/default
```

### Environment Variables
- `SPRING_PROFILES_ACTIVE`: Active Spring profile (dev, prod, docker)
- `HDFS_NAMENODE`: HDFS NameNode URL
- `HIVE_JDBC_URL`: Hive JDBC connection URL

## 📈 API Endpoints

### COVID-19 Data
- `GET /api/covid19/latest` - Latest COVID-19 data
- `GET /api/covid19/country/{country}` - Data by country
- `GET /api/covid19/range?start={date}&end={date}` - Data by date range
- `GET /api/covid19/summary` - Summary statistics

### System
- `GET /api/health` - Health check
- `POST /api/ingest` - Trigger data ingestion

## 🗄️ Data Models

### COVID-19 Data
```java
public class Covid19Data {
    private LocalDate date;
    private String country;
    private Integer totalCases;
    private Integer totalDeaths;
    private Integer newCases;
    private Integer newDeaths;
    private Double totalCasesPerMillion;
    private Double totalDeathsPerMillion;
    private String dataSource;
}
```

## 🔄 Data Processing

### Spark Job
- **COVID-19 Data Processing**: Processes Our World in Data JSON and creates Hive tables
- **Schema Enforcement**: Ensures consistent data structure across all countries
- **Data Validation**: Validates and cleans incoming data

### Job Execution
```bash
# Run the Spark job manually
docker exec spark-job /opt/spark/run-covid-job.sh

# Check job status
docker logs spark-job
```

## 🧪 Testing

### Unit Tests
```bash
mvn test
```

### Integration Tests
```bash
mvn verify
```

### API Tests
```bash
# Test health endpoint
curl http://localhost:8081/api/health

# Test data endpoints
curl http://localhost:8081/api/covid19/latest
curl http://localhost:8081/api/covid19/summary
```

## 📝 Development

### Project Structure
```
src/
├── main/
│   ├── java/com/covid19_tracker/
│   │   ├── config/          # Configuration classes
│   │   ├── model/           # Data models
│   │   ├── repository/      # Data access layer
│   │   ├── service/         # Business logic
│   │   ├── batch/           # Spring Batch jobs
│   │   ├── ingestion/       # Data ingestion services
│   │   ├── hive/            # Hive data service
│   │   └── api/             # REST controllers
│   └── resources/
│       └── application.yml  # Application configuration
└── test/                    # Test classes

spark-jobs/
├── src/main/java/
│   └── com/covid19_tracker/spark/
│       └── Covid19DataProcessor.java  # Spark ETL job
└── Dockerfile

covid19-visualization/
├── src/
│   ├── components/          # React components
│   └── services/            # API services
└── package.json
```

### Adding New Data Sources
1. Update `DataSourcesConfig.java`
2. Add ingestion method in `DataIngestionService.java`
3. Update Spark job in `Covid19DataProcessor.java`
4. Update API endpoints in `RestApiController.java`

## 🚀 Deployment

### Production Deployment
1. **Build the application:**
   ```bash
   mvn clean package -DskipTests
   ```

2. **Deploy with Docker:**
   ```bash
   docker-compose up -d
   ```

3. **Run data processing:**
   ```bash
   docker exec spark-job /opt/spark/run-covid-job.sh
   ```

4. **Monitor the application:**
   ```bash
   docker-compose logs -f covid19-tracker
   ```

## 📊 Monitoring and Logging

### Health Checks
- Application health: `/api/health`
- HDFS health: HDFS NameNode web UI
- Hive health: Hive Server web UI
- Spark health: Spark Master web UI

### Logging
- Application logs: Spring Boot logging
- Spark job logs: Docker logs
- Container logs: Docker logs

### Metrics
- Spring Boot Actuator metrics
- Custom business metrics
- Performance monitoring

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Data sources: Our World in Data
- Technologies: Spring Boot, Apache Spark, Apache Hadoop, Apache Hive, React
- Community: Open source contributors and researchers
