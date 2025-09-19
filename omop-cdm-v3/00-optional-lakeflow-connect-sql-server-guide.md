# Databricks Lakeflow Connect: SQL Server to Databricks Ingestion Setup

## Overview

Lakeflow Connect provides managed, serverless data ingestion from SQL Server databases into Databricks with built-in governance, incremental processing, and Unity Catalog integration. This guide covers the complete setup process for organizations looking to establish managed ingestion pipelines.

## Prerequisites

Before setting up Lakeflow Connect, ensure you have:

- **Databricks workspace** with Unity Catalog enabled
- **Serverless compute** enabled for your workspace  
- **CREATE CONNECTION** privileges on the metastore
- **SQL Server instance** with appropriate access permissions
- **Cluster permissions** or custom policy allowing ingestion operations

## SQL Server Source Requirements

### Supported SQL Server Types
- Azure SQL databases
- Amazon RDS SQL databases  
- SQL Server on Azure VMs
- SQL Server on Amazon EC2
- On-premises SQL Server (via Azure ExpressRoute or AWS Direct Connect)

### Version Requirements
- **Change Tracking**: SQL Server 2012 or above
- **CDC**: SQL Server 2012 SP1 CU3 or above (Enterprise Edition required for pre-2016 versions)

### Change Data Capture Configuration

Databricks recommends **Change Tracking** over CDC when tables have primary keys, as it's lightweight and minimizes database performance impact. Key differences:

- **Change Tracking**: Captures that rows changed; requires primary key; minimal performance impact
- **CDC**: Captures every operation; doesn't require primary key; higher performance impact

If both are enabled, the connector uses Change Tracking by default.

## Setup Process

### Step 1: SQL Server Source Configuration

1. **Configure firewall settings** to allow Databricks connectivity
2. **Create dedicated database user** for Databricks ingestion with appropriate read privileges
3. **Enable Change Tracking or CDC** on the source database and tables

For detailed SQL Server configuration steps, refer to [Configure Microsoft SQL Server for ingestion](https://docs.databricks.com/aws/en/ingestion/lakeflow-connect/sql-server-source-setup).

### Step 2: Create Lakeflow Connect Pipeline
The instructions below are for setting up Lakeflow Connect using the Databricks UI. For alternatives methods such as Databricks Asset Bundles (DABs), the Databricks CLI or notebooks, refer to [Option 2: Other Interfaces](https://docs.databricks.com/aws/en/ingestion/lakeflow-connect/sql-server-pipeline?language=Notebook#option-2-other-interfaces).

**Via Databricks UI:**
1. Navigate to **Data Ingestion** in your Databricks workspace
2. Select **SQL Server** as the data source
3. **Name your ingestion gateway** 
4. **Select staging catalog/schema** for intermediate processing
5. **Name the pipeline** 
6. **Choose destination catalog** where data will be stored
7. **Configure connection credentials** (create new or select existing)
8. **Select tables** to ingest from the source database
9. **Configure destination schema** and table structure
10. **(Optional)** Set up scheduling and notification preferences
11. **Save and run** the pipeline

### Step 3: Verification and Monitoring

After pipeline creation:
- Review the **pipeline details page** for execution status
- Monitor **"Upserted records"** and **"Deleted records"** metrics
- Confirm successful data transfer to destination tables
- Set up **alerts and monitoring** for ongoing operations

## Key Features

### Incremental Ingestion
- **Initial load**: Complete historical data ingestion on first run
- **Change tracking**: Only processes data changes since last execution
- **Automated scheduling**: Configurable refresh intervals

### Integration Benefits
- **Unity Catalog governance**: Automatic data lineage and access controls
- **Delta Live Tables**: Powered by serverless compute for scalability
- **Databricks Workflows**: Native orchestration and dependency management

### Architecture Advantages
- **Serverless compute**: No infrastructure management required
- **Built-in monitoring**: Event logs, cluster logs, and data quality metrics
- **CI/CD support**: Databricks Asset Bundles for deployment automation

## Best Practices

- Use **smallest possible worker nodes** while ensuring minimum 8-core configuration
- Configure **Change Tracking** rather than CDC when tables have primary keys
- Follow SQL Server **source setup instructions** carefully for optimal performance
- Set up appropriate **scheduling** based on data freshness requirements

## Additional Resources

For complete implementation details, consult the official Databricks documentation:

- [Ingest data from SQL Server](https://docs.databricks.com/aws/en/ingestion/lakeflow-connect/sql-server-pipeline)
- [Configure Microsoft SQL Server for ingestion](https://docs.databricks.com/aws/en/ingestion/lakeflow-connect/sql-server-source-setup)  
- [Managed connectors in Lakeflow Connect](https://docs.databricks.com/aws/en/ingestion/lakeflow-connect/)

This setup establishes the foundation for bringing SQL Server data into Databricks, creating the necessary infrastructure for subsequent lakehouse implementations based on OMOP CDM or other data models.