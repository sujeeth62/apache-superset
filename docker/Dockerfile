# =============================================================================
# Base Image
# =============================================================================
FROM apache/superset:5.0.0

# =============================================================================
# Labels
# =============================================================================
LABEL maintainer="Sujeet Hinge <sujeeth62@gmail.com>" \
      description="Apache Superset with Oracle and other database drivers" \
      version="1.0.0"

# =============================================================================
# System Dependencies
# =============================================================================
USER root

RUN set -ex && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        python3-dev \
        default-libmysqlclient-dev \
        build-essential \
        pkg-config \
        libaio1 \
        alien \
        curl && \
    rm -rf /var/lib/apt/lists/*

# =============================================================================
# Oracle Client Installation
# =============================================================================
WORKDIR /tmp/oracle

# Download and install Oracle Instant Client
RUN set -ex && \
    curl -L -O https://download.oracle.com/otn_software/linux/instantclient/2390000/oracle-instantclient-basic-23.9.0.25.07-1.el8.x86_64.rpm && \
    alien -i --scripts oracle-instantclient-basic-23.9.0.25.07-1.el8.x86_64.rpm && \
    curl -L -O https://download.oracle.com/otn_software/linux/instantclient/2390000/oracle-instantclient-sqlplus-23.9.0.25.07-1.el8.x86_64.rpm && \
    alien -i --scripts oracle-instantclient-sqlplus-23.9.0.25.07-1.el8.x86_64.rpm && \
    rm -f /tmp/oracle/*.rpm

# =============================================================================
# Environment Variables
# =============================================================================
ENV ORACLE_HOME=/usr/lib/oracle/23/client64
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/oracle/23/client64/lib
ENV TNS_ADMIN=/usr/lib/oracle/23/client64/lib/network/admin
ENV PLAYWRIGHT_BROWSERS_PATH=/usr/local/share/playwright-browsers

# Configure Oracle client
RUN echo "$ORACLE_HOME" > /etc/ld.so.conf.d/oracle-instantclient.conf && \
    ldconfig && \
    mkdir -p $ORACLE_HOME/lib/network/{admin,logs}

# =============================================================================
# Python Dependencies
# =============================================================================
# Install database drivers and other Python packages
RUN . /app/.venv/bin/activate && \
    uv pip install --no-cache-dir \
        psycopg2-binary \
        cx_Oracle \
        sqlalchemy-solr \
        sqlalchemy-bigquery[bqstorage] \
        mysqlclient \
        pymssql \
        Authlib \
        openpyxl \
        Pillow

# =============================================================================
# Playwright Installation
# =============================================================================
# Install Playwright for Alerts & Reports (screenshots)
RUN . /app/.venv/bin/activate && \
    uv pip install --no-cache-dir playwright && \
    playwright install-deps && \
    playwright install chromium

# =============================================================================
# Final Setup
# =============================================================================
# Switch back to the superset user
USER superset

# Default command
CMD ["/app/docker/entrypoints/run-server.sh"]
