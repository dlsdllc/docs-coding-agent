***

# WideWorldImporters Sample Database

A High Level Overview  

***

## Introduction

WideWorldImporters (WWI) is a Microsoft sample database designed to showcase realistic business operations for both OLTP (transactional) and OLAP (analytical) workloads. Unlike older sample databases that were more synthetic, WideWorldImporters models a plausible wholesale distribution company with detailed operational workflows, inventory management, purchasing, sales, warehousing, and associated historical and analytical data structures.

This document provides a high‑level summary of the WWI database and includes inline reference links to three external documentation sources:

*   Dataedo data dictionary  
    <https://dataedo.com/samples/html/WideWorldImporters/doc/WideWorldImporters_5/home.html>

*   MSSQLTips installation and business context overview  
    <https://www.mssqltips.com/sqlservertip/4391/installing-new-sql-server-sample-databases-wideworldimporters/>

*   DeepWiki architecture and system breakdown  
    <https://deepwiki.com/microsoft/sql-server-samples/5.1-wideworldimporters>

***

## What the Fictional Company Represents

Wide World Importers is a fictional wholesale novelty goods importer and distributor operating primarily out of the San Francisco Bay Area.  
The company model includes:

*   Customers that include retailers, specialty stores, supermarkets, computing shops, tourist shops, and some direct individual buyers.
*   Suppliers that include novelty manufacturers, toy producers, and other wholesalers.
*   Warehouse and inventory operations that include stocking, packaging materials, and fulfillment workflows.
*   Newer operational requirements such as temperature tracking for edible novelty goods.

A full data dictionary describing the entities, tables, modules, and relationships can be found in the Dataedo documentation:  
<https://dataedo.com/samples/html/WideWorldImporters/doc/WideWorldImporters_5/home.html>

For a narrative description of the company's business processes and purpose, see the MSSQLTips overview:  
<https://www.mssqltips.com/sqlservertip/4391/installing-new-sql-server-sample-databases-wideworldimporters/>

***

## Database Purpose and Design Goals

Microsoft built WideWorldImporters to serve as a modern example of how to:

*   Demonstrate SQL Server 2016 and later capabilities in a realistic scenario.
*   Provide a database with both OLTP and OLAP components.
*   Showcase real‑time analytics features.
*   Model operational workflows including sales, purchasing, invoicing, stock control, shipping, and temperature logging.
*   Support advanced SQL Server features such as:
    *   Temporal tables
    *   In‑memory OLTP
    *   Columnstore indexes
    *   JSON support
    *   Row‑level security
    *   Dynamic data masking
    *   Query Store
    *   Partitioning

Deep architectural notes describing the overall system components, including ETL, OLTP, OLAP, and supportive services, are available at DeepWiki:  
<https://deepwiki.com/microsoft/sql-server-samples/5.1-wideworldimporters>

***

## Main Components

WideWorldImporters is split into two principal databases:

### 1. WideWorldImporters (OLTP)

This is the primary transactional database used to model daily operations. It contains tables for customers, orders, stock items, suppliers, purchase orders, transactions, accounting data, and application support structures.

### 2. WideWorldImportersDW (OLAP / Data Warehouse)

This data warehouse contains dimensional models, fact tables, and historical aggregations derived from the OLTP database via scheduled ETL processes.

Both the OLTP and DW versions are included in the extended documentation and schema maps at the Dataedo link above.

***

## ETL, Analytics, and Supporting Artifacts

The complete WWI ecosystem includes:

*   SSIS packages for ETL from OLTP to DW
*   Sample SSAS models
*   Power BI dashboards
*   Demonstrations of real‑time analytics via columnstore indexes
*   Sample applications and scripts
*   SSDT projects for reproducible schema deployments

A structural breakdown of these components is documented at the DeepWiki link:  
<https://deepwiki.com/microsoft/sql-server-samples/5.1-wideworldimporters>

***

## Installation and Getting Started

A step‑by‑step installation walkthrough, including prerequisites and environment notes, is provided on MSSQLTips:  
<https://www.mssqltips.com/sqlservertip/4391/installing-new-sql-server-sample-databases-wideworldimporters/>

This guide explains:

*   Where to download OLTP and DW backups
*   How to restore them in SQL Server or Azure SQL
*   Additional configuration requirements such as enabling full‑text search or integration services

***

## Summary

WideWorldImporters serves as a comprehensive, modern SQL Server sample environment that models a realistic wholesale distribution business. It includes transactional processing, data warehousing, ETL pipelines, real‑time analytics, and advanced SQL Server capabilities.

For deeper reference, consult:

*   Technical data dictionary (Dataedo)  
    <https://dataedo.com/samples/html/WideWorldImporters/doc/WideWorldImporters_5/home.html>

*   Business context and installation guide (MSSQLTips)  
    <https://www.mssqltips.com/sqlservertip/4391/installing-new-sql-server-sample-databases-wideworldimporters/>

*   Architectural and systems overview (DeepWiki)  
    <https://deepwiki.com/microsoft/sql-server-samples/5.1-wideworldimporters>

***

