# Custom Apache Superset Docker Image

This repository contains a customized Docker image for [Apache Superset](https://superset.apache.org/) with additional database drivers and tools pre-installed.

## Features

- Based on the official `apache/superset:5.0.0` image
- Additional database drivers included:
    - Oracle
    - PostgreSQL (psycopg2-binary)
    - MySQL (mysqlclient)
    - Microsoft SQL Server (pymssql)
    - Google BigQuery (sqlalchemy-bigquery)
    - Elasticsearch (elasticsearch-dpapi)
    - Apache Solr (sqlalchemy-solr)
- Playwright for Alerts & Reports
- OpenPyXL for Excel file uploads
- Pillow for image processing
- Optimized for production use

## Prerequisites

- Docker 20.10.0 or higher
- Docker Compose (optional, for local development)
- At least 8GB RAM (16GB recommended)
- At least 4 CPU cores
- GitHub repository with GitHub Actions enabled
- Docker Hub account with repository access

## Quick Start

### Using Docker Compose (Recommended)

1. Clone this repository:
   ```bash
   git clone https://github.com/sujeeth62/apache-superset.git
   cd apache-superset
   ```

2. Copy the example environment file and update it with your configuration:
   ```bash
   cp .env.example .env
   # Edit .env file with your configuration
   ```

3. Start the services:
   ```bash
   docker-compose up -d
   ```

4. Initialize the database and create an admin user:
   ```bash
   docker-compose exec superset superset db upgrade
   docker-compose exec superset superset init
   docker-compose exec superset superset fab create-admin \
      --username admin \
      --firstname Superset \
      --lastname Admin \
      --email admin@example.com \
      --password admin
   ```

5. Access Superset at http://localhost:8088

### Using Docker Directly

```bash
docker run -d --name superset \
  -p 8088:8088 \
  -e SUPERSET_SECRET_KEY="your-secret-key" \
  -e SUPERSET_LOAD_EXAMPLES=yes \
  your-dockerhub-sujeeth62/apache-superset:latest
```

## Configuration

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `SUPERSET_SECRET_KEY` | - | A secret key for the Superset application |
| `SUPERSET_LOAD_EXAMPLES` | `no` | Load example dashboards and visualizations |
| `SUPERSET_ENABLE_PROXY_HEADER` | `true` | Enable proxy headers |
| `FLASK_APP` | `superset` | Flask application name |
| `FLASK_ENV` | `production` | Flask environment |
| `DATABASE_DIALECT` | `postgresql` | Database dialect |
| `DATABASE_USER` | `superset` | Database user |
| `DATABASE_PASSWORD` | `superset` | Database password |
| `DATABASE_HOST` | `db` | Database host |
| `DATABASE_PORT` | `5432` | Database port |
| `DATABASE_DB` | `superset` | Database name |
| `REDIS_HOST` | `redis` | Redis host |
| `REDIS_PORT` | `6379` | Redis port |

### Database Connections

This image includes drivers for multiple databases. Here are example connection strings:

- **PostgreSQL**: `postgresql://user:password@host:port/dbname`
- **MySQL**: `mysql://user:password@host:port/dbname`
- **Oracle**: `oracle+cx_oracle://user:password@host:port/?service_name=servicename`
- **MS SQL Server**: `mssql+pymssql://user:password@host:port/dbname`
- **BigQuery**: `bigquery://project-id`

For detailed configuration options and advanced database connection settings, refer to the [official Superset database configuration documentation](https://superset.apache.org/docs/configuration/databases/).

## Development

### Building the Image

```bash
docker build -t sujeeth62/apache-superset:latest -f docker/Dockerfile .
```

### Running Tests

```bash
docker-compose -f docker-compose-test.yml up --build --abort-on-container-exit
```

## CI/CD

This repository includes a GitHub Actions workflow that automatically builds and pushes the Docker image to Docker Hub on every push to the `main` branch.

### Manual Trigger

You can manually trigger the workflow from the GitHub Actions tab in your repository.

## Security

- Always use strong passwords and secrets
- Keep your Docker images updated
- Run containers with non-root users
- Use HTTPS in production
- Regularly backup your database

## Troubleshooting

### Common Issues

1. **Database connection issues**: Check your database credentials and ensure the database is accessible from the container.
2. **Permission issues**: Ensure the container has the correct permissions to write to mounted volumes.
3. **Memory issues**: Increase the Docker daemon's memory allocation if you encounter out-of-memory errors.

### Viewing Logs

```bash
docker-compose logs -f
```

## License

[Apache License 2.0](LICENSE)

## Contributing

Contributions are welcome! Please read our [contributing guidelines](CONTRIBUTING.md) for more information.

## Maintainers

- **Sujeet Hinge**  
  [![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/sujeeth62)  
  Big Data & Cloud Engineer  
  Email: [sujeeth62@gmail.com](mailto:sujeeth62@gmail.com)

## Contact Us

For support, questions, or contributions, please:

- Open an issue in the [GitHub repository](https://github.com/sujeeth62/apache-superset/issues)
- Email: [sujeeth62@gmail.com](mailto:sujeeth62@gmail.com)

---

*This project is not affiliated with the Apache Software Foundation or the Apache Superset project.*
