# 1. Data Lake Documentation

## 1.1. Introduction

This documentation is designed to help you understand and operate the data integration and analytics stack at Third South Capital. If you are new, start here. If you are troubleshooting, use the table of contents to jump to the relevant section. For any access issues, remember to check 1Password and AWS security group settings. If you have questions, reach out to Harrison.

This documentation should provide enough information for you to:

- Understand how the data lake is architected
- Grab claude.md files for each business to co-pilot with you as you evaluate existing Metabase resources or build new concepts

## 1.2. Table of Contents

- [1. Data Lake Documentation](#1-data-lake-documentation)
  - [1.1. Introduction](#11-introduction)
  - [1.2. Table of Contents](#12-table-of-contents)
  - [1.3. Metabase and How We Use it](#13-metabase-and-how-we-use-it)
    - [1.3.1. Preamble](#131-preamble)
    - [1.3.2. As a Data Viewer](#132-as-a-data-viewer)
    - [1.3.3. As a Transformer](#133-as-a-transformer)
    - [1.3.4. As a Notification System](#134-as-a-notification-system)
    - [1.3.5. Known Issues](#135-known-issues)
      - [1.3.5.1. Comments](#1351-comments)
      - [1.3.5.2. Custom Modeling - Why Clean Models?](#1352-custom-modeling---why-clean-models)
      - [1.3.5.3. \* ! Known Custom Models in Metabase ! \*](#1353---known-custom-models-in-metabase--)
    - [1.3.6. How to Use](#136-how-to-use)
  - [1.4. Datalake Infrastructure](#14-datalake-infrastructure)
    - [1.4.1. Commentary from Harrison](#141-commentary-from-harrison)
    - [1.4.2. Hevo Data Platform Documentation](#142-hevo-data-platform-documentation)
      - [1.4.2.1. Business Context](#1421-business-context)
      - [1.4.2.2. Data Flow Architecture](#1422-data-flow-architecture)
      - [1.4.2.3. Primary Account Details](#1423-primary-account-details)
      - [1.4.2.4. Access Control Strategy](#1424-access-control-strategy)
      - [1.4.2.5. Destination Database Configuration](#1425-destination-database-configuration)
      - [1.4.2.6. Network Access Requirements](#1426-network-access-requirements)
      - [1.4.2.7. Database Connection Tools](#1427-database-connection-tools)
      - [1.4.2.8. Current Pipeline Inventory](#1428-current-pipeline-inventory)
    - [1.4.3. Database Conventions](#143-database-conventions)
      - [1.4.3.1. Table Naming Convention](#1431-table-naming-convention)
      - [1.4.3.2. Source System Categories](#1432-source-system-categories)
        - [1.4.3.2.1. Native Integrations (Easy Setup)](#14321-native-integrations-easy-setup)
        - [1.4.3.2.2. Complex Integrations (Require Special Handling)](#14322-complex-integrations-require-special-handling)
        - [1.4.3.2.3. Intermediary Services](#14323-intermediary-services)
    - [1.4.4. Typical Hevo Problems](#144-typical-hevo-problems)
      - [1.4.4.1. 🚨 PRIMARY KEY DUPLICATION ISSUE](#1441--primary-key-duplication-issue)
      - [1.4.4.2. 🚨 TRANSFORMATION SCOPE ISSUE](#1442--transformation-scope-issue)
      - [1.4.4.3. 🚨 PADDLE API DUPLICATION](#1443--paddle-api-duplication)
    - [1.4.5. Pipeline Setup Process](#145-pipeline-setup-process)
      - [1.4.5.1. Pre-Setup Validation](#1451-pre-setup-validation)
      - [1.4.5.2. Initial Pipeline Configuration](#1452-initial-pipeline-configuration)
      - [1.4.5.3. Duplication Detection and Resolution](#1453-duplication-detection-and-resolution)
      - [1.4.5.4. API Key Management](#1454-api-key-management)
      - [1.4.5.5. Monitoring and Validation](#1455-monitoring-and-validation)
      - [1.4.5.6. EntryThingy](#1456-entrythingy)
      - [1.4.5.7. UploadThingy](#1457-uploadthingy)
      - [1.4.5.8. Stripe-Based Businesses](#1458-stripe-based-businesses)
      - [1.4.5.9. Clipboard (Complex Setup)](#1459-clipboard-complex-setup)
      - [1.4.5.10. Shopping Cart Apps (Most Complex)](#14510-shopping-cart-apps-most-complex)
      - [1.4.5.11. Inactive Pipelines](#14511-inactive-pipelines)
    - [1.4.6. Common Issues and Solutions](#146-common-issues-and-solutions)
      - [1.4.6.1. Pipeline Setup Difficulties](#1461-pipeline-setup-difficulties)
      - [1.4.6.2. Massive Data Imports (Over-ingestion)](#1462-massive-data-imports-over-ingestion)
      - [1.4.6.3. Authentication Expiration](#1463-authentication-expiration)
      - [1.4.6.4. Schema Changes at Source](#1464-schema-changes-at-source)
    - [1.4.7. Advanced Troubleshooting Techniques](#147-advanced-troubleshooting-techniques)
      - [1.4.7.1. Discovery Queries for Duplicate Detection](#1471-discovery-queries-for-duplicate-detection)
      - [1.4.7.2. API Connectivity Validation](#1472-api-connectivity-validation)
    - [1.4.8. Automated Monitoring](#148-automated-monitoring)
    - [1.4.9. Manual Monitoring Procedures](#149-manual-monitoring-procedures)
      - [1.4.9.1. Daily Checks (Recommended)](#1491-daily-checks-recommended)
      - [1.4.9.2. Weekly Checks](#1492-weekly-checks)
      - [1.4.9.3. Monthly Checks](#1493-monthly-checks)
    - [1.4.10. Performance Considerations](#1410-performance-considerations)
    - [1.4.11. First Login Checklist](#1411-first-login-checklist)
      - [1.4.11.1. Understand the Dashboard](#14111-understand-the-dashboard)
      - [1.4.11.2. Essential Settings to Understand](#14112-essential-settings-to-understand)
      - [1.4.11.3. What to Avoid](#14113-what-to-avoid)
      - [1.4.11.4. Safe Actions for New Users](#14114-safe-actions-for-new-users)
      - [1.4.11.5. When to Ask for Help](#14115-when-to-ask-for-help)
    - [1.4.12. Essential Knowledge for New Users](#1412-essential-knowledge-for-new-users)
      - [1.4.12.1. Understanding Data Flow](#14121-understanding-data-flow)
      - [1.4.12.2. Key Concepts](#14122-key-concepts)
      - [1.4.12.3. Red Flags to Watch For](#14123-red-flags-to-watch-for)
    - [1.4.13. Current Approach for Costs](#1413-current-approach-for-costs)
    - [1.4.14. Future Optimization Opportunities](#1414-future-optimization-opportunities)
    - [1.4.15. Hevo Support Strategy](#1415-hevo-support-strategy)
    - [1.4.16. Alternative Vendor Considerations](#1416-alternative-vendor-considerations)
  - [1.5. Known Business-Specific Issues](#15-known-business-specific-issues)
    - [1.5.1. CLAUDE.md - Shopping Cart Apps (SCA)](#151-claudemd---shopping-cart-apps-sca)
      - [1.5.1.1. Overview](#1511-overview)
      - [1.5.1.2. Key Tables to Know](#1512-key-tables-to-know)
      - [1.5.1.3. Common Query Patterns](#1513-common-query-patterns)
        - [1.5.1.3.1. Parsing Store Data](#15131-parsing-store-data)
      - [1.5.1.4. Revenue Calculations](#1514-revenue-calculations)
        - [1.5.1.4.1. Feed Quality Analysis](#15141-feed-quality-analysis)
      - [1.5.1.5. Business Logic to Remember](#1515-business-logic-to-remember)
        - [1.5.1.5.1. Tier Limits and Pricing](#15151-tier-limits-and-pricing)
        - [1.5.1.5.2. Important Metrics](#15152-important-metrics)
      - [1.5.1.6. Query Ideas to Explore](#1516-query-ideas-to-explore)
        - [1.5.1.6.1. Revenue Optimization](#15161-revenue-optimization)
        - [1.5.1.6.2. Product Analysis](#15162-product-analysis)
        - [1.5.1.6.3. Customer Behavior](#15163-customer-behavior)
      - [1.5.1.7. Data Gotchas](#1517-data-gotchas)
      - [1.5.1.8. Useful Explorations](#1518-useful-explorations)
      - [1.5.1.9. Testing Queries](#1519-testing-queries)
      - [1.5.1.10. Performance Tips](#15110-performance-tips)
      - [1.5.1.11. SCA S3 Data Integration](#15111-sca-s3-data-integration)
      - [1.5.1.12. Row Limit Management](#15112-row-limit-management)
    - [1.5.2. CLAUDE.md - EntryThingy (ET)](#152-claudemd---entrythingy-et)
      - [1.5.2.1. Overview](#1521-overview)
      - [1.5.2.2. Key Tables to Know](#1522-key-tables-to-know)
      - [1.5.2.3. Business Model Context](#1523-business-model-context)
        - [1.5.2.3.1. Token Economy Basics](#15231-token-economy-basics)
        - [1.5.2.3.2. Revenue Streams](#15232-revenue-streams)
      - [1.5.2.4. Critical Data Quality Filters](#1524-critical-data-quality-filters)
      - [1.5.2.5. Common Query Patterns](#1525-common-query-patterns)
        - [1.5.2.5.1. Monthly Token Flow Analysis](#15251-monthly-token-flow-analysis)
        - [1.5.2.5.2. Customer Health Analysis](#15252-customer-health-analysis)
        - [1.5.2.5.3. Revenue Analysis](#15253-revenue-analysis)
      - [1.5.2.6. Token Usage Categories](#1526-token-usage-categories)
      - [1.5.2.7. AWM Subscription Analysis](#1527-awm-subscription-analysis)
      - [1.5.2.8. Business Logic to Remember](#1528-business-logic-to-remember)
        - [1.5.2.8.1. Seasonal Patterns](#15281-seasonal-patterns)
      - [1.5.2.9. Data Gotchas](#1529-data-gotchas)
      - [1.5.2.10. Query Ideas to Explore](#15210-query-ideas-to-explore)
        - [1.5.2.10.1. Customer Health \& Churn](#152101-customer-health--churn)
        - [1.5.2.10.2. Revenue Analysis](#152102-revenue-analysis)
        - [1.5.2.10.3. Platform Activity](#152103-platform-activity)
        - [1.5.2.10.4. Operational Analysis](#152104-operational-analysis)
      - [1.5.2.11. Performance Tips](#15211-performance-tips)
      - [1.5.2.12. Integration Notes](#15212-integration-notes)
    - [1.5.3. CLAUDE.md - UploadThingy (UT)](#153-claudemd---uploadthingy-ut)
      - [1.5.3.1. Overview](#1531-overview)
      - [1.5.3.2. Key Tables to Know](#1532-key-tables-to-know)
      - [1.5.3.3. Business Model Context](#1533-business-model-context)
        - [1.5.3.3.1. Subscription Tiers](#15331-subscription-tiers)
        - [1.5.3.3.2. Platform Evolution](#15332-platform-evolution)
      - [1.5.3.4. Critical Business Context](#1534-critical-business-context)
        - [1.5.3.4.1. Top Active Domains](#15341-top-active-domains)
      - [1.5.3.5. Common Query Patterns](#1535-common-query-patterns)
        - [1.5.3.5.1. Monthly Revenue Tracking](#15351-monthly-revenue-tracking)
        - [1.5.3.5.2. Transfer Activity Analysis](#15352-transfer-activity-analysis)
        - [1.5.3.5.3. Customer Usage Analysis](#15353-customer-usage-analysis)
        - [1.5.3.5.4. Customer Account Analysis](#15354-customer-account-analysis)
      - [1.5.3.6. Transfer Pattern Analysis](#1536-transfer-pattern-analysis)
      - [1.5.3.7. Business Logic to Remember](#1537-business-logic-to-remember)
        - [1.5.3.7.1. File Transfer Economics](#15371-file-transfer-economics)
        - [1.5.3.7.2. Platform Complexity](#15372-platform-complexity)
        - [1.5.3.7.3. Customer Segmentation](#15373-customer-segmentation)
      - [1.5.3.8. Data Gotchas](#1538-data-gotchas)
      - [1.5.3.9. Query Ideas to Explore](#1539-query-ideas-to-explore)
        - [1.5.3.9.1. Revenue and Churn Analysis](#15391-revenue-and-churn-analysis)
        - [1.5.3.9.2. Platform Health Monitoring](#15392-platform-health-monitoring)
        - [1.5.3.9.3. Multi-Service Analysis](#15393-multi-service-analysis)
        - [1.5.3.9.4. Customer Success Analysis](#15394-customer-success-analysis)
      - [1.5.3.10. Performance Considerations](#15310-performance-considerations)
      - [1.5.3.11. Integration Notes](#15311-integration-notes)
      - [1.5.3.12. Testing Queries](#15312-testing-queries)
      - [1.5.3.13. Operational Priorities](#15313-operational-priorities)
    - [1.5.4. Claude.md - GitZen (GZ)](#154-claudemd---gitzen-gz)
      - [1.5.4.1. Summary](#1541-summary)
    - [1.5.5. Claude.md - TissueApp (TA)](#155-claudemd---tissueapp-ta)
      - [1.5.5.1. Summary](#1551-summary)
    - [1.5.6. CLAUDE.md - Clipboard History Pro (CHP)](#156-claudemd---clipboard-history-pro-chp)
      - [1.5.6.1. Overview](#1561-overview)
      - [1.5.6.2. Key Tables to Know](#1562-key-tables-to-know)
      - [1.5.6.3. Database Schema Essentials](#1563-database-schema-essentials)
        - [1.5.6.3.1. Webhook Table Structure \[HIGH CONFIDENCE\]](#15631-webhook-table-structure-high-confidence)
        - [1.5.6.3.2. Subscription Plans \& Pricing \[HIGH CONFIDENCE\]](#15632-subscription-plans--pricing-high-confidence)
      - [1.5.6.4. Common Query Patterns](#1564-common-query-patterns)
        - [1.5.6.4.1. Revenue Calculations \[HIGH CONFIDENCE\]](#15641-revenue-calculations-high-confidence)
        - [1.5.6.4.2. Extracting JSON Webhook Data](#15642-extracting-json-webhook-data)
      - [1.5.6.5. Business Logic to Remember](#1565-business-logic-to-remember)
        - [1.5.6.5.1. Key Metrics \[HIGH CONFIDENCE\]](#15651-key-metrics-high-confidence)
      - [1.5.6.6. Performance Tips](#1566-performance-tips)
        - [1.5.6.6.1. Query Optimization](#15661-query-optimization)
        - [1.5.6.6.2. Data Limitations](#15662-data-limitations)
      - [1.5.6.7. Testing Queries](#1567-testing-queries)
      - [1.5.6.8. Useful Explorations](#1568-useful-explorations)
      - [1.5.6.9. Integration Status](#1569-integration-status)
    - [1.5.7. Claude.md - ZenDesk Apps (ZDA)](#157-claudemd---zendesk-apps-zda)
      - [1.5.7.1. Summary](#1571-summary)
      - [1.5.7.2. Business Description](#1572-business-description)
        - [1.5.7.2.1. URL Builder Pro](#15721-url-builder-pro)
        - [1.5.7.2.2. PDF Viewer Pro - View PDFs in Zendesk](#15722-pdf-viewer-pro---view-pdfs-in-zendesk)
        - [1.5.7.2.3. Preview Image Attachments](#15723-preview-image-attachments)
        - [1.5.7.2.4. MP4 Viewer Pro](#15724-mp4-viewer-pro)
        - [1.5.7.2.5. Doc Viewer Pro Doc Viewer Pro](#15725-doc-viewer-pro-doc-viewer-pro)
        - [1.5.7.2.6. User Profile](#15726-user-profile)
        - [1.5.7.2.7. Download Attachments](#15727-download-attachments)
    - [1.5.8. Claude.md - Icon Horse (IH)](#158-claudemd---icon-horse-ih)
      - [1.5.8.1. Summary](#1581-summary)

## 1.3. Metabase and How We Use it

Here are some thoughts about Metabase

### 1.3.1. Preamble

- We use metabase as 1) a data viewer, 2) data transformer, and 3) a notification system
- Generally speaking, you are directly viewing SQL tables in our postgres database; you can write questions and save them to create dashboards (data viewing)
- As we pull out tables for use in queries, if they need modifications, models are created (data transforming)
- Key KPIs are sent to Slack at regular intervals
- Known Issues
- How to Use (!)

### 1.3.2. As a Data Viewer

- Each business is organized as a Collection. Collections are Metabase's way of keeping things organized
- Metabase sees the word in a certain language. A *question* is a GUI or SQL-driven interrogation of a table that preserves Metabase's access to underlying table data. We *like* questions
- Questions, if needed, can become Models. We use Models, in particular when data needs to be transformed. More on that later
- The answers to questions can be viewed as tables or charts. If they are important, they can be saved to a Dashboard for consolidated metrics
- If you must, use Metabase's *query* function. This is like writing a direct SQL command. It'll get you the information you want, but you won't be able to drill down into data sets and click through charts

### 1.3.3. As a Transformer

- From time to time, our data sources bring information into our ecosystem in disparate ways
- For example, in some data sources, revenue comes in USD in a $1.00 format. In others, it comes as 100 cents (Stripe). In others, it comes in original FX (some Paddle queries)
- Models are a way of simply transforming data. The most common transformation we do is take Stripe data in cents and make a custom field called revenue. Revenue is then used for visualization in this custom expression
- If you think a custom expression is going to be needed across a dataset, do it at the model level; if you think a custom expression just may be needed for a quick query, you can do one in a Question
- Sometimes we are simply formatting metadata. Stripe dates, for example, come in as UNIX and thus cannot be used for charting. Take a trip to Metabase Admin and table metadata. Search for your table. Click the gear. Change no semantic value to the appropriate item (e.g. creation timestamp) and in the cast field convert from UNIX to seconds. Perform this process BEFORE creating a model

### 1.3.4. As a Notification System

- Send queries, questions, etc. on a regular schedule to Slack - it is very easy

### 1.3.5. Known Issues
#### 1.3.5.1. Comments
- Due to API limitations, we have a few datasets which rely on manually imported CSV
- For example, we have a Paddle Webhook which captures all present transactions for Clipboard. However, the historical API is bad
- So, I created a Webhook. Then Claude and I studied that format and we imported historical CSV data to backfill. Now we have working historical data
- If you make changes to tables and models and you are impatient, go to the Admin, meta data, and discard the cached values (wait) then force a rescan
#### 1.3.5.2. Custom Modeling - Why Clean Models? 
- We use custom models to make it easier for users to frequently query tables that don't have data perfectly validated
- When making a custom model, you can modify a table and "save as model" - then add the word Clean after it to indicate to other users you are finished
#### 1.3.5.3. * ! Known Custom Models in Metabase ! *


- ET: Balance Model Clean highlights customers with no token use and declares them inactive
- UT: no current custom models for active use
- ZDA: Invoice Line Item Clean builds a column to save product type;  Stripe Charge Clean builds revenue to convert cents to dollars
- TA: Stripe Charge Clean builds revenue to convert cents to dollars
- GZ: Stripe Charge Clean builds revenue to convert cents to dollars
- SCA: Consol Clean buckets plan names by gross procing
- CHP: Unified ALL Transactions - Clean is doing a large join of historical CSV data and webhook data - and building new fields for combined analysis
- IH: Stripe Charge Clean clean builds revenue to convert cents to dollars and uses gross revenue to build plan identifier 



### 1.3.6. How to Use

- If I have done my job correctly, each of our collections has a simple overview.md and full.md file showing all our active queries in use
- We also have a context.md file showing a lot of what we know about the business, how APIs are connected to it, watch-outs, etc.
- So, if you launch Claude Code CLI and navigate to this repository, it should make a pretty good buddy for self-serve in exploring the databases, generating new queries, etc.
- Don't forget, Metabase also has CLI, so you can interact that way

Happy Dashboarding.

---

## 1.4. Datalake Infrastructure

### 1.4.1. Commentary from Harrison

The below documentation was generated in an iterative conversation between me and Claude. I tried to cover all the relevant points in our setup of Hevo. I think if you want to understand Hevo, the best way to do it is look at the database. Nearly every table in the destination postgres database came from a pipeline that Hevo has setup. If you are looking to add a new pipeline, I would work with LLM to understand source documentation. Unless it is a frequently-used integration like Stripe, most sources will require some configuring in REST. Primary watch-out for Hevo is ensuring it isn't setting its Hevo Ingested field as primary key - that makes a mess.

Not much to commenet on in terms of the postgres setup itself. I use DBeaver to get in. Any time you need to auth into the DB, remember to add your IP to the security group on Amazon RDS. 

### 1.4.2. Hevo Data Platform Documentation 

#### 1.4.2.1. Business Context
Third South Capital uses Hevo as the primary data integration platform to consolidate data from 30+ pipelines across multiple businesses into a single analytics database. Hevo was selected over Fivetran and Airbyte due to:
- **Ease of integration** with diverse source systems
- **Effectiveness of support team** for troubleshooting complex setups
- **Native capabilities** for handling various API endpoints and data sources

#### 1.4.2.2. Data Flow Architecture
```
[Multiple Source Systems] → [Hevo Pipelines] → [Consolidated PostgreSQL] → [Metabase Analytics]
```

**Philosophy**: Keep complexity in Metabase, not in Hevo transformations. This creates Metabase lock-in but provides better auditability and control over data processing.

#### 1.4.2.3. Primary Account Details
- **Admin Email**: harrison@thirdsouth.capital
- **Billing**: Annual plan, renews in June
- **Team Access**: Myles has full login access
- **Adding Users**: Team members can be easily added via email invitation

#### 1.4.2.4. Access Control Strategy
- **Hevo**: Email invitations for pipeline management access
- **AWS/Database**: Access via 1Password stored credentials
- **Metabase**: All team members have accounts
- **1Password**: Universal access for all team members (contains API keys)

#### 1.4.2.5. Destination Database Configuration
- **Provider**: Amazon Web Services (AWS)
- **Service**: RDS PostgreSQL
- **Instance**: tsc-analytics-sandbox
- **Access Method**: AWS root login via shopping@thirdsouth.capital
- **Credentials**: Stored in 1Password under "tsc_admin data lake"

#### 1.4.2.6. Network Access Requirements
- **Database Access**: Requires IP address whitelisting in AWS security groups
- **Setup Process**:
  1. Sign into AWS via root email
  2. Navigate to RDS → tsc-analytics-sandbox
  3. Click first link under security
  4. Edit inbound rules in security group
  5. Add your IP from https://whatismyipaddress.com/

#### 1.4.2.7. Database Connection Tools
- **Recommended**: DBeaver with IP whitelisting
- **Alternative**: Direct PostgreSQL connections via standard tools

#### 1.4.2.8. Current Pipeline Inventory
- **Total Active Pipelines**: ~30
- **Sync Frequency**: Daily (1x per day across all pipelines)
- **Data Freshness Requirements**: Daily sync meets all current business needs

### 1.4.3. Database Conventions

#### 1.4.3.1. Table Naming Convention
**Standard Format**: `[business]_[source]`

**Examples**:
- `ih_stripe` - Icon.Horse Stripe account data
- `et_mysql` - EntryThingy MySQL database
- `clipboard_paddle` - Clipboard Paddle API data

#### 1.4.3.2. Source System Categories

##### 1.4.3.2.1. Native Integrations (Easy Setup)
- **Stripe**: All businesses using Stripe have native integration with full table access
- **Standard APIs**: Most modern SaaS platforms with documented APIs

##### 1.4.3.2.2. Complex Integrations (Require Special Handling)
- **MySQL Databases**: Direct database connections (EntryThingy, UploadThingy)
- **Webhook-Based**: Real-time event data (Clipboard via Paddle webhooks)
- **CSV + API Hybrid**: Historical data via CSV, ongoing via API (PayPal scenarios)

##### 1.4.3.2.3. Intermediary Services
- **Shopify via Mantle**: Shopping Cart Apps uses www.heymantle.com to avoid GraphQL complexities
- **Third-Party Processors**: When direct API integration is problematic

### 1.4.4. Typical Hevo Problems

#### 1.4.4.1. 🚨 PRIMARY KEY DUPLICATION ISSUE
**Problem**: Hevo automatically sets `hevo_ingested_at` as the primary key for new tables, creating massive duplicates.

**Impact**: Can result in hundreds of thousands of duplicate records.

**Prevention**: 
- Disable auto-mapping for new tables, OR
- Monitor initial imports closely for record count anomalies

**Detection**: If a small business import shows 500k+ rows, immediately investigate.

#### 1.4.4.2. 🚨 TRANSFORMATION SCOPE ISSUE
**Problem**: Hevo transformations apply to ALL tables in a pipeline, regardless of UI selection.

**UI Deception**: Selecting "Charge" in the transformation UI does NOT restrict the transformation to only charge objects during execution.

**Our Policy**: **NEVER use Hevo transformations**. Handle all data transformations in Metabase models.

**If You Must Transform**: Wrap all transformation logic in conditional checks:
```python
eventName = event.getEventName()
if eventName == 'charge':
    # transformation logic here
```

#### 1.4.4.3. 🚨 PADDLE API DUPLICATION
**High-Risk Source**: Paddle API is a major offender for duplication issues when `hevo_ingested_at` is set as primary key.

**Special Handling Required**: Always disable auto-mapping for Paddle pipelines and manually configure primary keys.

### 1.4.5. Pipeline Setup Process

#### 1.4.5.1. Pre-Setup Validation
```bash
# Always test API connectivity before Hevo setup
curl -X GET "https://api.example.com/endpoint" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json"
```

**Purpose**: Validate authentication and API access before spending time in Hevo configuration.

**Troubleshooting Strategy**: If Hevo setup fails, you can isolate whether the issue is:
- Authentication problems (test with headers and no auth)
- API endpoint issues
- Hevo-specific configuration problems

#### 1.4.5.2. Initial Pipeline Configuration
1. **Let auto-mapper work initially** for first attempt
2. **Monitor import closely** for duplication signs
3. **Review destination documentation** for API-specific requirements
4. **Configure daily sync frequency** (standard across all pipelines)

#### 1.4.5.3. Duplication Detection and Resolution
**Detection Methods**:
- **Sanity Check**: Row count analysis (500k+ rows for small business = red flag)
- **Database Queries**: Run discovery queries in PostgreSQL to identify duplicates
- **Pattern Recognition**: Multiple records with same business data but different `hevo_ingested_at`

**Resolution Process**:
1. **Shut down pipeline** immediately
2. **Drop affected tables** completely from PostgreSQL
3. **Reconfigure entire pipeline** (not just table-level fixes)
4. **Disable auto-mapping** during reconfiguration
5. **Manually select proper primary key** (usually `id` field)
6. **Restart sync** with new configuration

**Why Complete Reconfiguration**: Ensures no residual configuration issues remain that could cause future problems.

#### 1.4.5.4. API Key Management
- **Storage**: All API keys stored in 1Password
- **Access Pattern**: Team members retrieve keys from 1Password as needed
- **Security**: No API keys stored in Hevo interface or other locations

#### 1.4.5.5. Monitoring and Validation
- **Daily**: Hevo sends email notifications for complete pipeline failures
- **Manual**: Periodic spot-checks of record counts and data freshness
- **Discovery Queries**: Regular PostgreSQL queries to validate data integrity

#### 1.4.5.6. EntryThingy
**Sources**:
- MySQL database (operational and financial data)
- PayPal API (attempted, limited to 31-day windows)

**Known Issues**:
- Missing AWM PayPal subscription data (requires CSV upload)
- PayPal webhook integration unsuccessful
- SFTP access request to PayPal pending response

**Current Workaround**: Historical PayPal data via CSV upload to Metabase

#### 1.4.5.7. UploadThingy
**Sources**:
- MySQL database (operational and financial data)
- PayPal API (same 31-day limitation as EntryThingy)

**Status**: Same PayPal integration challenges as EntryThingy

#### 1.4.5.8. Stripe-Based Businesses
**Active Integrations**:
- ZenDesk: Stripe financial data + web analytics
- GitZen: Stripe financial data + web analytics
- Tissue: Stripe financial data
- Icon.Horse: Stripe financial data

**Success Pattern**: Native Stripe integrations work reliably with full table access

#### 1.4.5.9. Clipboard (Complex Setup)
**Challenge**: Paddle API integration difficulties

**Solution Architecture**:
- **Webhook**: Recent events via Paddle webhook
- **CSV Upload**: Historical data via Metabase CSV import
- **Join Strategy**: Big join operation during analysis to combine datasets

**Special Pipeline**: `CHP Paddle Classic Transactions 909164` represents specific subscription plan data

#### 1.4.5.10. Shopping Cart Apps (Most Complex)
**Challenge**: Shopify GraphQL API integration issues

**Solution**: Intermediary service via www.heymantle.com
- **Heymantle Function**: Converts Shopify GraphQL to REST API
- **Data Flow**: Shopify → Heymantle → Hevo REST API → PostgreSQL

**Import Limitations**:
- Annual row limit restrictions (monitor 2025 usage)
- 2025 data consolidation via SQL into consolidated table for queries

**Database Architecture**:
- Raw imports with row limitations
- Consolidated table (created via SQL) for analysis queries

#### 1.4.5.11. Inactive Pipelines
**SCA S3 Bucket**: 
- Contains extensive customer feed and data
- **Status**: Inactive due to data volume
- **Reason**: Too much data for efficient processing
- **Future Consideration**: Selective data import strategies

### 1.4.6. Common Issues and Solutions

#### 1.4.6.1. Pipeline Setup Difficulties
**Symptoms**: 
- Authentication failures
- Connection timeouts
- Incomplete data imports

**Resolution Process**:
1. **Validate API connectivity** via curl commands
2. **Check API documentation** for endpoint changes or authentication updates
3. **Review source system limitations** (rate limits, data access restrictions)
4. **Test authentication methods** (headers vs. embedded auth)

#### 1.4.6.2. Massive Data Imports (Over-ingestion)
**Symptoms**:
- Hundreds of thousands of records from small business sources
- Import jobs running for excessive time periods
- Database storage consumption spikes

**Immediate Actions**:
1. **Stop pipeline** immediately
2. **Run discovery queries** to assess duplication extent
3. **Drop tables** and start fresh rather than attempting cleanup
4. **Reconfigure pipeline** with manual primary key selection

#### 1.4.6.3. Authentication Expiration
**Symptoms**:
- Previously working pipelines suddenly failing
- Authentication error messages in Hevo dashboard

**Resolution**:
1. **Check 1Password** for updated API credentials
2. **Test API access** with current credentials via curl
3. **Update Hevo pipeline** with new authentication details
4. **Resume sync** and monitor for success

#### 1.4.6.4. Schema Changes at Source
**Symptoms**:
- New columns not appearing in destination
- Data type mismatches
- Import failures after source system updates

**Resolution**:
1. **Review source system changelog** for recent updates
2. **Update Hevo pipeline** to accommodate new schema
3. **Test with small data sample** before full sync
4. **Update Metabase models** to incorporate new fields

### 1.4.7. Advanced Troubleshooting Techniques

#### 1.4.7.1. Discovery Queries for Duplicate Detection
```sql
-- Identify potential duplicates by business identifier
SELECT 
    business_field,
    COUNT(*) as record_count,
    MIN(__hevo__ingested_at) as first_ingested,
    MAX(__hevo__ingested_at) as last_ingested
FROM table_name 
GROUP BY business_field 
HAVING COUNT(*) > 1
ORDER BY record_count DESC;

-- Check for unreasonable record volumes
SELECT 
    DATE_TRUNC('day', __hevo__ingested_at) as ingestion_date,
    COUNT(*) as daily_records
FROM table_name 
GROUP BY DATE_TRUNC('day', __hevo__ingested_at)
ORDER BY daily_records DESC;
```

#### 1.4.7.2. API Connectivity Validation
```bash
# Basic connectivity test
curl -I "https://api.example.com/health"

# Authentication test
curl -X GET "https://api.example.com/test" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -v

# Rate limit testing
curl -X GET "https://api.example.com/endpoint" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -w "Time: %{time_total}s\n"
```

### 1.4.8. Automated Monitoring
- **Hevo Email Alerts**: Automatic notifications for complete pipeline failures
- **No Custom Alerting**: Relying on Hevo's built-in monitoring

### 1.4.9. Manual Monitoring Procedures

#### 1.4.9.1. Daily Checks (Recommended)
- Review Hevo dashboard for pipeline health status
- Check for any email alerts from previous day
- Spot-check high-volume or critical business pipelines

#### 1.4.9.2. Weekly Checks
- Validate data freshness in Metabase dashboards
- Review any pipelines showing warnings or unusual activity
- Monitor storage consumption in AWS RDS

#### 1.4.9.3. Monthly Checks
- Review all pipeline configurations for optimization opportunities
- Analyze data volume trends for capacity planning
- Update API credentials approaching expiration

### 1.4.10. Performance Considerations
- **Sync Frequency**: Daily sync sufficient for current business needs
- **Data Volume**: Monitor for unexpected volume increases
- **Storage Growth**: Track PostgreSQL storage consumption trends

### 1.4.11. First Login Checklist
When a new user logs into Hevo for the first time:

#### 1.4.11.1. Understand the Dashboard
- **30+ Pipelines**: Don't be overwhelmed by the number
- **Naming Convention**: `[business]_[source]` format
- **Status Indicators**: Green = healthy, Red = requires attention

#### 1.4.11.2. Essential Settings to Understand
- **Sync Frequency**: All pipelines set to daily
- **Transformations**: **NEVER USE** - handled in Metabase
- **Auto-mapping**: Default setting, but monitor for duplication issues

#### 1.4.11.3. What to Avoid
- **Transformation Features**: Can affect all tables despite UI suggestions
- **Primary Key Changes**: Don't modify without understanding duplication implications
- **Pipeline Restarts**: Try to avoid unless absolutely necessary

#### 1.4.11.4. Safe Actions for New Users
- **View pipeline status** and health indicators
- **Review sync logs** for understanding data flow
- **Monitor record counts** for anomaly detection
- **Access help documentation** within Hevo interface

#### 1.4.11.5. When to Ask for Help
- **Before creating new pipelines**: Get guidance on configuration
- **When seeing unusual record volumes**: 500k+ rows for small business
- **Before modifying existing pipelines**: Understand impact on downstream systems
- **When encountering authentication issues**: May require API key updates

### 1.4.12. Essential Knowledge for New Users

#### 1.4.12.1. Understanding Data Flow
```
Source System → Hevo Pipeline → PostgreSQL → Metabase
```

#### 1.4.12.2. Key Concepts
- **Daily Sync**: All data is updated once per day
- **No Real-time**: System designed for business analytics, not real-time operations
- **Single Destination**: All pipelines feed into one consolidated database
- **Metabase Integration**: Final analysis happens in Metabase, not Hevo

#### 1.4.12.3. Red Flags to Watch For
- Record counts in hundreds of thousands from small business sources
- Pipeline failures lasting more than one day
- Authentication errors across multiple pipelines
- Unusual data patterns in destination database

### 1.4.13. Current Approach for Costs
- **Annual billing** for cost savings
- **Daily sync frequency** balances freshness with resource consumption
- **Single destination database** reduces complexity and costs

### 1.4.14. Future Optimization Opportunities
- **Pipeline consolidation** where business logic allows
- **Data retention policies** for historical data management
- **Storage optimization** in PostgreSQL for large datasets

### 1.4.15. Hevo Support Strategy
- **Leverage support team effectiveness** (key vendor selection factor)
- **Document recurring issues** for faster resolution
- **Maintain good relationship** for complex integration assistance

### 1.4.16. Alternative Vendor Considerations
- **Fivetran and Airbyte** were evaluated but Hevo chosen for specific advantages
- **Maintain awareness** of competitive landscape for future decisions
- **Avoid over-dependency** while leveraging Hevo's strengths


---

## 1.5. Known Business-Specific Issues

### 1.5.1. CLAUDE.md - Shopping Cart Apps (SCA)

#### 1.5.1.1. Overview
This file helps Claude assist you with SCA data analysis and query development. SCA is a tiered SaaS business managing shopping feeds for Shopify/Wix stores. 

The main "issues" with this setup are as follows:
- We use www.heymantle.com as intermediary middleware to get around Shopify Graph QL issues; REST APIs go in there, get the data, put it in specific years due to row limits, and then we consolidate into a consol table using a SQL view in the postgres database
- There is lots of data in Amazon S3 around feeds and stores; we use Lambda functions to "grab" a summary of those daily
- You can find all this under the SCA_ prefix

#### 1.5.1.2. Key Tables to Know
When analyzing SCA data, these are your primary tables:
- `sca_mantle_consol` - All financial transactions (use `netamount` for revenue)
- `sca_s3data_lateststore` - Current store data (CSV format in single column)
- `sca_s3data_latestfeeds` - Product catalog data
- `mv_store_metrics` - Pre-calculated metrics (faster for dashboards)

#### 1.5.1.3. Common Query Patterns

##### 1.5.1.3.1. Parsing Store Data
Store data comes as CSV in a single column. Always parse it like this:
```sql
-- Fields: store_id,domain,store_name,email,country,creation_date,platform,revenue,product_count,id,plan_type,plan_limit,monthly_charge,expiration_date,guid,formatted_name
SELECT 
    SPLIT_PART(csv_column, ',', 1) as store_id,
    SPLIT_PART(csv_column, ',', 7) as platform,
    SPLIT_PART(csv_column, ',', 11) as plan_type,
    NULLIF(SPLIT_PART(csv_column, ',', 13), '')::NUMERIC as monthly_charge
FROM sca_s3data_lateststore
```

#### 1.5.1.4. Revenue Calculations
Always use `netamount` (not `grossamount`) for revenue:
```sql
-- Monthly recurring revenue
SELECT 
    DATE_TRUNC('month', date) as month,
    SUM(netamount) as mrr
FROM sca_mantle_consol
WHERE type = 'subscription_sale'
GROUP BY 1
```

##### 1.5.1.4.1. Feed Quality Analysis
```sql
-- Check feed completeness
SELECT 
    COUNT(*) as total_products,
    COUNT(title) as has_title,
    COUNT(description) as has_description,
    COUNT(image_link) as has_image,
    COUNT(price) as has_price
FROM sca_s3data_latestfeeds
```

#### 1.5.1.5. Business Logic to Remember

##### 1.5.1.5.1. Tier Limits and Pricing
- Trial: $0, 100,000 products
- Small: $9.95, 1,000 products
- Medium: $19.95, 10,000 products
- Large: $29.95, 100,000 products

##### 1.5.1.5.2. Important Metrics
1. **Zombie Customers**: Paying but no active feeds (17.8% currently)
2. **Tier Violations**: Stores exceeding their plan limits
3. **Platform Performance**: Shopify converts at 97%, Wix at 5.6%

#### 1.5.1.6. Query Ideas to Explore

##### 1.5.1.6.1. Revenue Optimization
- Find stores exceeding tier limits (upgrade opportunities)
- Identify zombie customers for recovery campaigns
- Platform-specific conversion analysis

##### 1.5.1.6.2. Product Analysis
- Feed quality scoring by store
- Product catalog growth trends
- Most common product categories

##### 1.5.1.6.3. Customer Behavior
- Trial to paid conversion rates
- Churn analysis by tier and platform
- Usage patterns by plan type

#### 1.5.1.7. Data Gotchas
1. **Store Data Format**: Always use SPLIT_PART for CSV parsing
2. **Empty Values**: Use NULLIF to handle empty strings in numeric fields
3. **Date Fields**: Transaction dates are in `date` column, not `createdat`
4. **Duplication**: Latest feeds/stores only - no historical duplication
5. **Pagination**: Mantle API data comes in 25-record pages

#### 1.5.1.8. Useful Explorations
When working with Claude, consider asking about:
- "Show me stores that should upgrade based on product count"
- "Calculate potential revenue from tier enforcement"
- "Analyze feed quality by platform"
- "Find patterns in zombie customer behavior"
- "Compare Shopify vs Wix performance metrics"

#### 1.5.1.9. Testing Queries
Always test with small samples first:
```sql
-- Add LIMIT when testing
SELECT * FROM sca_s3data_lateststore LIMIT 10;
```

#### 1.5.1.10. Performance Tips
- Use `mv_store_metrics` for dashboard queries (pre-calculated)
- Index on `customerid` and `date` for transaction queries
- Avoid large UNION queries in DBeaver (use INSERT instead)


#### 1.5.1.11. SCA S3 Data Integration
- This integration is inactive;  but refer to lambda functions on the server for how we create simple copies of store data and daily feeds to capture a small amount of this data

#### 1.5.1.12. Row Limit Management
- We have an row limitation in place with Mantle. This necessitates creating annual feeds (ish) because of limits on amount of rows that can be pulled in at once

### 1.5.2. CLAUDE.md - EntryThingy (ET)

#### 1.5.2.1. Overview
This file helps Claude assist you with EntryThingy data analysis and query development. EntryThingy is a token-based platform where art organizations purchase tokens consumed by platform actions (submissions, status changes, gallery usage).

The main "issues" here are as follows:

- We really only sync operational data from the ET database. Financial data from PayPal is on future roadmap
- Token economy is quite trackable in the SQL DB, but, we don't have strong data around AWM linkage to payments

#### 1.5.2.2. Key Tables to Know
When analyzing EntryThingy data, these are your primary tables:
- `et_sqldb_entrythingy_et_tokenhistory` - All token transactions (purchases and usage)
- `et_sqldb_entrythingy_et_payment` - PayPal token purchases
- `et_sqldb_entrythingy_et_widget` - Organizations using platform
- `et_sqldb_entrythingy_et_item` - Artwork submissions
- `et_sqldb_entrythingy_et_user` - Artists and users (includes AWM subscription data)
- `et_sqldb_entrythingy_et_show` - Art exhibitions and competitions

#### 1.5.2.3. Business Model Context

##### 1.5.2.3.1. Token Economy Basics
- **Pricing**: $3.00 per token (increased from $2.00 on January 16, 2024)
- **Usage**: 1 token per submission, 1 token per status change, 9 tokens per monthly gallery usage
- **Customers**: Art organizations (galleries, exhibitions) - NOT individual artists
- **Scale**: 89 active organizations, ~$120-150K annual revenue

##### 1.5.2.3.2. Revenue Streams
1. **Token Purchases** (60-70% of revenue): Variable, seasonal
2. **AWM Subscriptions** (~$32K annually): Hybrid B2B2C model
3. **UploadThingy** (~$15K annually): File transfer service

#### 1.5.2.4. Critical Data Quality Filters
**ALWAYS exclude administrative corrections** in token usage analysis:
```sql
WHERE th.reason != 'Double counted' 
AND th.reason != 'Accidental overcredit' 
AND th.reason NOT LIKE 'Correction%'
```

**Token calculations must account for price change**:
```sql
-- Token value calculation
CASE 
    WHEN th.create_date < '2024-01-16' THEN th.token_change * 2.00  -- $2 per token
    ELSE th.token_change * 3.00  -- $3 per token
END as token_value_dollars
```

#### 1.5.2.5. Common Query Patterns

##### 1.5.2.5.1. Monthly Token Flow Analysis
```sql
-- Token purchases vs usage with proper filtering
SELECT 
    DATE_TRUNC('month', th.create_date) AS month,
    SUM(CASE WHEN th.token_change > 0 THEN th.token_change ELSE 0 END) AS tokens_purchased,
    SUM(CASE 
        WHEN th.token_change < 0 
        AND th.reason NOT LIKE '%correction%'
        AND th.reason NOT LIKE '%double counted%'
        THEN ABS(th.token_change) 
        ELSE 0 
    END) AS tokens_used,
    COUNT(DISTINCT th.widget_id) AS active_orgs
FROM public.et_sqldb_entrythingy_et_tokenhistory th
WHERE th.create_date >= '2024-01-01'
GROUP BY month
ORDER BY month DESC;
```

##### 1.5.2.5.2. Customer Health Analysis
```sql
-- Token balance health by organization
SELECT 
    w.id,
    w.name,
    w.tokens AS current_balance,
    CASE 
        WHEN w.tokens < 0 THEN 'Negative Balance'
        WHEN w.tokens = 0 THEN 'Zero Balance'
        WHEN w.tokens <= 50 THEN 'Low Balance (1-50)'
        ELSE 'Healthy Balance'
    END AS balance_status,
    w.last_use
FROM public.et_sqldb_entrythingy_et_widget w
WHERE EXISTS (
    SELECT 1 FROM public.et_sqldb_entrythingy_et_tokenhistory th 
    WHERE th.widget_id = w.id 
    AND th.create_date >= CURRENT_DATE - INTERVAL '12 months'
)
ORDER BY w.tokens;
```

##### 1.5.2.5.3. Revenue Analysis
```sql
-- Top revenue customers with lifetime value
SELECT 
    p.widget_id,
    w.name,
    COUNT(*) AS transactions,
    SUM(p.amount)/100 AS total_revenue_dollars,
    MIN(p.payment_date) AS first_purchase,
    MAX(p.payment_date) AS latest_purchase
FROM public.et_sqldb_entrythingy_et_payment p
JOIN public.et_sqldb_entrythingy_et_widget w ON p.widget_id = w.id
WHERE p.payment_status = 'Completed'
GROUP BY p.widget_id, w.name
ORDER BY total_revenue_dollars DESC;
```

#### 1.5.2.6. Token Usage Categories
Track exactly what tokens are used for:
```sql
-- Token usage breakdown by category
SELECT 
    CASE
        WHEN th.reason LIKE '%submitted%entry%' THEN 'Submissions'
        WHEN th.reason LIKE '%changed status%' THEN 'Status Changes'
        WHEN th.reason LIKE 'Monthly usage of gallery%' THEN 'Gallery Usage'
        WHEN th.reason LIKE '%email alert%' THEN 'Email Alerts'
        ELSE 'Other'
    END AS usage_type,
    SUM(ABS(th.token_change)) AS tokens_consumed,
    COUNT(*) AS transaction_count
FROM public.et_sqldb_entrythingy_et_tokenhistory th
WHERE th.token_change < 0 
AND th.reason NOT LIKE '%correction%'
GROUP BY usage_type
ORDER BY tokens_consumed DESC;
```

#### 1.5.2.7. AWM Subscription Analysis
AWM is a complex hybrid model - both organizational purchases and individual subscriptions:
```sql
-- Individual AWM subscribers
SELECT 
    u.max_awm_artwork AS subscription_tier,
    COUNT(*) AS subscribers,
    CASE 
        WHEN u.max_awm_artwork = 50 THEN 4.90
        WHEN u.max_awm_artwork = 200 THEN 9.80
        WHEN u.max_awm_artwork = 500 THEN 19.80
    END AS monthly_price,
    COUNT(*) * CASE 
        WHEN u.max_awm_artwork = 50 THEN 4.90
        WHEN u.max_awm_artwork = 200 THEN 9.80
        WHEN u.max_awm_artwork = 500 THEN 19.80
    END AS monthly_revenue
FROM public.et_sqldb_entrythingy_et_user u
WHERE u.max_awm_artwork > 0 
AND u.last_use >= CURRENT_DATE - INTERVAL '6 months'
GROUP BY u.max_awm_artwork;
```

#### 1.5.2.8. Business Logic to Remember

##### 1.5.2.8.1. Seasonal Patterns
- **January**: Major token purchases (call cycles)
- **March-May**: High token usage (submission seasons) 
- **December**: Lowest activity period




#### 1.5.2.9. Data Gotchas
1. **Administrative Corrections**: Always exclude from usage metrics
2. **Price Change**: January 16, 2024 - $2 to $3 per token
3. **Token History Coverage**: Only goes back to October 2022 (missing 2+ years)
4. **AWM Complexity**: Individual subscriptions NOT in payment table
5. **PayPal Integration**: Some revenue streams require CSV upload (31-day API limit)

#### 1.5.2.10. Query Ideas to Explore

##### 1.5.2.10.1. Customer Health & Churn
- "Identify customers at risk of running out of tokens"
- "Find organizations with declining usage patterns"
- "Calculate customer lifetime value by organization type"
- "Analyze token purchase vs usage ratios"

##### 1.5.2.10.2. Revenue Analysis
- "Show revenue impact of the price increase"
- "Compare seasonal revenue patterns year-over-year"
- "Identify upselling opportunities for low-balance customers"
- "Calculate AWM vs token revenue distribution"

##### 1.5.2.10.3. Platform Activity
- "Correlate submission volume with token purchases"
- "Analyze show success rates by organization"
- "Track platform growth metrics (users, submissions, shows)"
- "Find most active vs dormant organizations"

##### 1.5.2.10.4. Operational Analysis
- "Validate token balance calculations vs payment history"
- "Identify data quality issues in transaction records"
- "Monitor for duplicate transactions or corrections"

#### 1.5.2.11. Performance Tips
- Token history table is large (109K+ records) - always filter by date
- Use EXISTS for checking organization activity vs JOIN
- Widget table (organizations) is smaller - good for lookups
- Payment table has complete history (2009+) but tokenhistory is limited

#### 1.5.2.12. Integration Notes
- **Metabase Models**: Use existing models for dashboard work
- **Cross-Platform**: Many users active in both EntryThingy and AWM
- **PayPal Data**: Historical data requires CSV import for complete picture
- **Hevo Pipeline**: Real-time sync from MySQL to PostgreSQL

### 1.5.3. CLAUDE.md - UploadThingy (UT)


#### 1.5.3.1. Overview
This file helps Claude assist you with UploadThingy data analysis and query development. UploadThingy is a multi-service file transfer platform that has evolved from simple file sharing into project management, podcasting, and social collaboration tools.

The key "issues" here -

- Lack of PayPal integration
- Otherwise, data should be pretty good

#### 1.5.3.2. Key Tables to Know
When analyzing UploadThingy data, these are your primary tables:
- `ut_sqldb_uploadthingy_p_payment` - Financial transactions and subscriptions
- `ut_sqldb_uploadthingy_p_widget` - Customer accounts and configurations  
- `ut_sqldb_uploadthingy_vs_transfer` - File transfer activity logs
- `ut_sqldb_uploadthingy_p_user` - Individual user accounts
- `ut_sqldb_uploadthingy_pt_*` - Project management system tables
- `ut_sqldb_uploadthingy_p_podcast*` - Podcasting platform tables

#### 1.5.3.3. Business Model Context

##### 1.5.3.3.1. Subscription Tiers
- **Basic**: $9/month (1GB transfer limit)
- **Standard**: $19/month (5GB transfer limit)  
- **Professional**: $35/month (20GB transfer limit)
- **Enterprise**: $79/month (40GB transfer limit)
- **Custom Enterprise**: $150+/month (150GB+ limits)

##### 1.5.3.3.2. Platform Evolution
- **Core Service**: File transfer and sharing
- **Extensions**: Project management, podcasting, social features
- **Customer Focus**: Business customers in printing, graphics, digital content
- **Scale**: Annual revenue ~$10-15K (declining trend)

#### 1.5.3.4. Critical Business Context

##### 1.5.3.4.1. Top Active Domains
1. Duggal.com (600GB limit) - Major printing business
2. Lacda.com - Professional services
3. Bigprintshop.com - Commercial printing

#### 1.5.3.5. Common Query Patterns

##### 1.5.3.5.1. Monthly Revenue Tracking
```sql
-- Subscription revenue analysis
SELECT
    DATE_TRUNC('month', payment_date) AS month,
    COUNT(*) AS transaction_count,
    SUM(amount)/100 AS total_revenue_dollars,
    AVG(amount)/100 AS avg_payment_size
FROM public.ut_sqldb_uploadthingy_p_payment
WHERE __hevo__marked_deleted = false 
AND payment_date >= '2024-01-01'
GROUP BY month
ORDER BY month DESC;
```

##### 1.5.3.5.2. Transfer Activity Analysis
```sql
-- File transfer volume by direction and time
SELECT 
    DATE_TRUNC('month', create_date) AS month,
    direction,
    ROUND(SUM(length)/1024.0/1024.0/1024.0, 2) AS total_gb_transferred,
    COUNT(*) AS transfer_count,
    ROUND(AVG(length)/1024.0/1024.0, 2) AS avg_file_size_mb
FROM public.ut_sqldb_uploadthingy_vs_transfer
WHERE create_date >= '2024-01-01'
GROUP BY month, direction
ORDER BY month DESC, direction;
```

##### 1.5.3.5.3. Customer Usage Analysis
```sql
-- Customer activity by domain
SELECT 
    widget_domain,
    COUNT(*) AS transfer_count,
    ROUND(SUM(length)/1024.0/1024.0/1024.0, 2) AS total_gb,
    MIN(create_date) AS first_activity,
    MAX(create_date) AS latest_activity
FROM public.ut_sqldb_uploadthingy_vs_transfer
WHERE create_date >= '2024-01-01'
GROUP BY widget_domain
ORDER BY total_gb DESC;
```

##### 1.5.3.5.4. Customer Account Analysis
```sql
-- Widget configuration and limits
SELECT 
    w.id,
    w.url AS customer_domain,
    w.month_max_data/1024/1024/1024 AS monthly_limit_gb,
    w.month_data/1024/1024/1024 AS current_usage_gb,
    w.last_use,
    w.active,
    w.usessl,
    w.storefiles
FROM public.ut_sqldb_uploadthingy_p_widget w
WHERE w.month_max_data > 0
ORDER BY w.month_max_data DESC;
```

#### 1.5.3.6. Transfer Pattern Analysis
```sql
-- Transfer patterns: in vs out
SELECT 
    widget_domain,
    SUM(CASE WHEN direction = 'in' THEN length ELSE 0 END)/1024/1024/1024 AS incoming_gb,
    SUM(CASE WHEN direction = 'out' THEN length ELSE 0 END)/1024/1024/1024 AS outgoing_gb,
    COUNT(CASE WHEN direction = 'in' THEN 1 END) AS incoming_count,
    COUNT(CASE WHEN direction = 'out' THEN 1 END) AS outgoing_count
FROM public.ut_sqldb_uploadthingy_vs_transfer
WHERE create_date >= '2024-01-01'
GROUP BY widget_domain
HAVING SUM(length) > 0
ORDER BY (incoming_gb + outgoing_gb) DESC;
```

#### 1.5.3.7. Business Logic to Remember

##### 1.5.3.7.1. File Transfer Economics
- **Revenue Model**: Tiered by monthly data transfer limits
- **Usage Pattern**: More outgoing than incoming transfers consistently
- **Average File Sizes**: 15-30MB typical
- **Customer Behavior**: Enterprise users drive bulk of activity

##### 1.5.3.7.2. Platform Complexity
- **Core Business**: File transfer (declining)
- **Value-Added Services**: Project management, podcasting, social features
- **Integration Challenge**: Multiple service lines using same infrastructure
- **Revenue Risk**: Core service decline affects entire platform

##### 1.5.3.7.3. Customer Segmentation
- **Enterprise Heavy Users**: High-limit customers (150GB+)
- **Professional Services**: Medium-tier regular users
- **Small Business**: Basic tier with occasional large files

#### 1.5.3.8. Data Gotchas
1. **Transfer Direction**: Always check 'in' vs 'out' - patterns differ significantly
2. **Data Volume Units**: Storage in bytes, need conversion to GB for analysis
3. **Customer Mapping**: `widget_domain` in transfers links to `url` in widgets
4. **Multi-Service Data**: Project/podcast tables may have different user patterns
5. **Declining Activity**: Historical comparisons show dramatic usage reduction
6. **Hevo Metadata**: Use `__hevo__marked_deleted = false` for clean data

#### 1.5.3.9. Query Ideas to Explore

##### 1.5.3.9.1. Revenue and Churn Analysis
- "Identify customers with declining transfer activity"
- "Calculate revenue per GB transferred by customer tier"
- "Find customers approaching their monthly limits"
- "Analyze subscription tier vs actual usage patterns"

##### 1.5.3.9.2. Platform Health Monitoring
- "Track month-over-month transfer volume decline"
- "Compare current activity to historical peaks"
- "Identify most active vs dormant customers"
- "Monitor storage vs transfer ratio by customer"

##### 1.5.3.9.3. Multi-Service Analysis
- "Correlate file transfer usage with project management activity"
- "Analyze podcast hosting adoption vs file transfer usage"
- "Find customers using multiple platform services"
- "Track service expansion success rates"

##### 1.5.3.9.4. Customer Success Analysis
- "Calculate customer lifetime value by tier and usage"
- "Identify upselling opportunities based on usage patterns"
- "Find enterprise customers with declining engagement"
- "Analyze customer onboarding and retention patterns"

#### 1.5.3.10. Performance Considerations
- **Transfer table size**: Large volume of activity logs
- **Date filtering**: Always use for performance on activity queries
- **Widget lookups**: Smaller table, good for customer information
- **Multi-service joins**: Be careful with cross-service analysis queries

#### 1.5.3.11. Integration Notes
- **AWS S3 Backend**: File storage managed through S3 (bucket references in data)
- **PayPal Integration**: All payments processed through PayPal
- **Multi-Service Architecture**: Shared user management across services
- **Affiliate System**: Referral tracking and commission structure

#### 1.5.3.12. Testing Queries
```sql
-- Always test with limits and recent dates
WHERE create_date >= '2024-01-01'
AND create_date < CURRENT_DATE
LIMIT 100;
```

#### 1.5.3.13. Operational Priorities
Given the declining usage trend, focus analysis on:
1. **Customer retention**: Who's still actively using the service?
2. **Revenue optimization**: Which customers provide steady revenue?
3. **Service value**: Are value-added services retaining customers?
4. **Cost analysis**: Is the platform still economically viable?

### 1.5.4. Claude.md - GitZen (GZ)
#### 1.5.4.1. Summary
- GitZen is a simple pull of Stripe, natively integrated via Hevo. There are not major issues related to data quality. Just remember like all Stripe stores that values are stored in cents, not dollars
- Business Description: Zendesk + Git Integration | Seamless Support & Development Sync Connect Zendesk with GitHub, Azure DevOps & GitLab in minutes. Bridge the gap between customer support and development teams. Automate ticket tracking, issue creation, and commit synchronization.

### 1.5.5. Claude.md - TissueApp (TA)
#### 1.5.5.1. Summary
- TissueApp is a simple pull of Stripe, natively integrated via Hevo. There are not major issues related to data quality. Just remember like all Stripe stores that values are stored in cents, not dollars
- Business Description: A Zendesk and GitHub synchronization tool for tickets, issues, and comments.

### 1.5.6. CLAUDE.md - Clipboard History Pro (CHP)

#### 1.5.6.1. Overview
This file helps Claude assist you with CHP data analysis and query development. CHP is a Chrome extension with subscription-based SaaS business model processing payments through Paddle Classic.

In many ways, this is our most complicated situation in terms of APIs, because we still connect to many Paddle APIs. But that is only really if you want "more data". At a simple level

- We use Padddle Webhook to bring in all real-time transactions
- Put CSV in behind the Paddle data to allow Metabase to build real joined queries via model
- APIs don't do FX, gross-to-net - so we have them for analysis but there really is no current use for them (see map of API numbers --> product plan)

#### 1.5.6.2. Key Tables to Know
When analyzing CHP data, these are your primary tables:
- `chp_paddle_webhook_api` - Real-time transaction data from Paddle webhooks (PRIMARY for revenue)
- `clipboard_paddle_api` - User subscription records (states, plan assignments)
- Historical CSV import tables - Legacy transaction data (pre-webhook implementation)
- Product-specific transaction tables (`chp_paddle_752111_api`, etc.) - Plan-specific historical data

#### 1.5.6.3. Database Schema Essentials

##### 1.5.6.3.1. Webhook Table Structure [HIGH CONFIDENCE]
```sql
-- Primary revenue data source
chp_paddle_webhook_api:
- alert_id (STRING, PRIMARY KEY) - Unique webhook identifier
- alert_name (STRING) - Event type (subscription_payment_succeeded, etc.)
- fields (JSON) - Complete transaction payload containing:
  ├── earnings (NUMERIC) - Net revenue after fees (~78-86% of gross)
  ├── sale_gross (NUMERIC) - Customer payment amount  
  ├── fee (NUMERIC) - Paddle platform fee
  ├── payment_tax (NUMERIC) - Tax amount
  ├── subscription_plan_id (STRING) - Plan identifier
  ├── user_id (STRING) - Customer identifier
  ├── email (STRING) - Customer email
  └── event_time (TIMESTAMP) - Transaction timestamp
```

##### 1.5.6.3.2. Subscription Plans & Pricing [HIGH CONFIDENCE]
```sql
-- Plan ID → Business Logic Mapping
752111 → Annual $23.99 (Premium Legacy)
705095 → Monthly $2.49 (Budget tier)  
649335 → Annual $14.99 (Standard tier)
631102 → Annual $47.90 (Premium tier)
596916 → Monthly $4.99 (Standard tier)
887004 → Monthly $5.99 (Current standard)
904538 → Annual $60.00 (Premium with discount)
905112 → Enterprise $195.69 (Business tier)
909164 → Weekly $1.99 (Trial tier)
```

#### 1.5.6.4. Common Query Patterns

##### 1.5.6.4.1. Revenue Calculations [HIGH CONFIDENCE]
**ALWAYS use `earnings` field for net revenue (not `sale_gross`):**
```sql
-- Monthly net revenue (accurate)
SELECT 
    DATE_TRUNC('month', TO_TIMESTAMP(fields->>'event_time', 'YYYY-MM-DD HH24:MI:SS')) as month,
    SUM((fields->>'earnings')::numeric) as net_revenue,
    COUNT(*) as transaction_count
FROM chp_paddle_webhook_api 
WHERE alert_name = 'subscription_payment_succeeded'
GROUP BY 1 
ORDER BY 1;
```

##### 1.5.6.4.2. Extracting JSON Webhook Data
```sql
-- Standard field extraction pattern
SELECT 
    alert_id,
    fields->>'order_id' as order_id,
    fields->>'subscription_plan_id' as plan_id,
    (fields->>'earnings')::numeric as net_revenue,
    (fields->>'sale_gross')::numeric as gross_revenue,
    fields->>'user_id' as customer_id,
    TO_TIMESTAMP(fields->>'event_time', 'YYYY-MM-DD HH24:MI:SS') as transaction_date
FROM chp_paddle_webhook_api
WHERE alert_name = 'subscription_payment_succeeded';
```



#### 1.5.6.5. Business Logic to Remember

##### 1.5.6.5.1. Key Metrics [HIGH CONFIDENCE]
- **Active Subscribers**: ~1,200 total (1,155 from recent analysis)
- **Monthly Revenue**: ~$3,000-4,000 USD equivalent  
- **Revenue Split**: Annual subscribers = ~25% of base, ~77% of revenue
- **Transaction Volume**: ~50-70 new transactions/month
- **Net Revenue Margin**: ~78-86% of gross (after Paddle fees)

```

##### Critical Business Rules
1. **Revenue Recognition**: Use `earnings` for actual revenue (post-fees)
2. **Customer Segmentation**: Monthly vs Annual billing patterns differ significantly
3. **Churn Analysis**: Monthly = 1-3% monthly, Annual = spikes during renewal periods
4. **ARPU Trends**: Monthly ~$2.65-2.70, Annual ~$25-26

#### Data Pipeline Architecture [HIGH CONFIDENCE]

##### Data Flow
```
Paddle Classic → Hevo ETL → PostgreSQL → Metabase Analytics
├── Webhooks (real-time) → chp_paddle_webhook_api
├── API polling (historical) → Product tables  
└── CSV imports (legacy) → Historical tables
```

##### Data Coverage
- **Webhook Data**: Last 90 days (rolling window limitation)
- **Historical CSV**: 2020-2025 (complete transaction history)
- **Unified Models**: Combine both sources with proper prioritization

#### Query Ideas to Explore

##### Revenue Optimization
- Identify paused subscribers for reactivation campaigns (1,977 delinquent)
- Plan performance analysis (which plans have best retention?)
- Conversion tracking from monthly to annual plans
- Currency analysis for international expansion

##### Customer Behavior Analysis  
- Cohort retention analysis by signup period
- ARPU trends over time by plan type
- Geographic revenue distribution
- Trial-to-paid conversion rates

##### Business Intelligence
- Year-over-year growth comparisons
- Seasonal revenue patterns
- Plan upgrade/downgrade patterns
- Customer lifetime value calculations

#### Data Gotchas [HIGH CONFIDENCE]

##### Critical Warnings
1. **Primary Key**: NEVER change webhook `alert_id` primary key - causes massive duplicates
2. **JSON Extraction**: Always cast to proper types: `(fields->>'earnings')::numeric`
3. **Date Handling**: Use `TO_TIMESTAMP(fields->>'event_time', 'YYYY-MM-DD HH24:MI:SS')`
4. **Revenue Field**: Use `earnings` NOT `sale_gross` for business calculations
5. **Deduplication**: Monitor for duplicates with `GROUP BY alert_id HAVING COUNT(*) > 1`

##### Data Quality Checks
```sql
-- Mandatory deduplication check (should return 0 rows)
SELECT alert_id, COUNT(*) 
FROM chp_paddle_webhook_api 
GROUP BY alert_id 
HAVING COUNT(*) > 1;

-- Revenue ratio validation (~0.78-0.86)
SELECT 
    SUM((fields->>'earnings')::numeric) / 
    SUM((fields->>'sale_gross')::numeric) as net_to_gross_ratio
FROM chp_paddle_webhook_api 
WHERE alert_name = 'subscription_payment_succeeded';
```

#### 1.5.6.6. Performance Tips

##### 1.5.6.6.1. Query Optimization
- Filter by `alert_name` first for webhook queries
- Use date ranges to limit JSON field extraction overhead
- Index on frequently queried JSON fields if performance issues arise
- Consider materialized views for complex unified queries

##### 1.5.6.6.2. Data Limitations
- **90-Day Webhook Window**: Cannot get historical net revenue via API alone
- **JSON Performance**: Large datasets may slow JSON field extraction
- **Currency Conversion**: Apply consistently across all revenue calculations from plan-specific data; webhook gets you gross to net
- **Pipeline Dependencies**: Monitor Hevo sync status for data gaps

#### 1.5.6.7. Testing Queries
Always test with small samples first:
```sql
-- Test webhook data structure
SELECT fields FROM chp_paddle_webhook_api LIMIT 5;

-- Verify plan mapping
SELECT DISTINCT fields->>'subscription_plan_id' as plan_id
FROM chp_paddle_webhook_api 
WHERE alert_name = 'subscription_payment_succeeded'
LIMIT 10;
```

#### 1.5.6.8. Useful Explorations
When working with Claude, consider asking about:
- "Calculate monthly recurring revenue from webhook data"
- "Analyze plan performance and customer retention"  
- "Find opportunities in paused subscriber reactivation"
- "Compare revenue trends year-over-year"
- "Identify highest-value customer segments"
- "Track trial-to-paid conversion rates by plan"

#### 1.5.6.9. Integration Status
- **Webhook Pipeline**: Active (real-time processing)
- **Historical Data**: Complete (CSV imports)  
- **Unified Models**: Implemented (Metabase)
- **Deduplication**: Resolved (proper primary keys)
- **Currency Conversion**: Standardized (USD equivalent)

### 1.5.7. Claude.md - ZenDesk Apps (ZDA)
#### 1.5.7.1. Summary
- ZDA is a simple pull of Stripe, natively integrated via Hevo. There are not major issues related to data quality. Just remember like all Stripe stores that values are stored in cents, not dollars

Known Issues

- The calculation of seats per invoice is needed to show trended seats calculation. This makes it different than other Stripe-based businesses as we do more than just the charge table model

#### 1.5.7.2. Business Description
We have a lot of products:

##### 1.5.7.2.1. URL Builder Pro
Convenient links right at your agents' fingertips. Create dynamic, context-aware URLs that show only relevant links based on the current ticket and agent role.

##### 1.5.7.2.2. PDF Viewer Pro - View PDFs in Zendesk
View PDF attachments instantly in Zendesk without downloads - streamline your support workflow with one-click document access.


##### 1.5.7.2.3. Preview Image Attachments
Preview images without downloading. View image attachments instantly in Zendesk with support for multiple formats.

##### 1.5.7.2.4. MP4 Viewer Pro
Watch video attachments directly in Zendesk. Stream MP4 videos without downloading or leaving the interface.

##### 1.5.7.2.5. Doc Viewer Pro Doc Viewer Pro
View Word documents without downloading. Read DOCX files directly in your Zendesk interface.

##### 1.5.7.2.6. User Profile
Enhanced user information at a glance. Get comprehensive customer data and quick action buttons.

##### 1.5.7.2.7. Download Attachments
Download all attachments with one click. Bulk download functionality for efficient file management.

### 1.5.8. Claude.md - Icon Horse (IH)
#### 1.5.8.1. Summary
- IH is a simple pull of Stripe, natively integrated via Hevo. There are not major issues related to data quality. Just remember like all Stripe stores that values are stored in cents, not dollars

- It's a simple business - a custom column has been added to Metabase to classify gross revenue amounts into the two plans
