# Intro from Harrison

This documentation is designed to help you understand and operate the data integration and analytics stack at Third South Capital. If you are new, start here. If you are troubleshooting, use the table of contents to jump to the relevant section. For any access issues, remember to check 1Password and AWS security group settings. If you have questions, reach out to Harrison or Myles.

---

# Metabase Background

Here are some thoughts about Metabase

## Preamble

- We use metabase as 1) a data viewer, 2) data transformer, and 3) a notification system
- Generally speaking, you are directly viewing SQL tables in our postgres database; you can write questions and save them to create dashboards (data viewing)
- As we pull out tables for use in queries, if they need modifications, models are created (data transforming)
- Key KPIs are sent to Slack at regular intervals
- Known Issues
- How to Use (!)

### As a Data Viewer

- Each business is organized as a Collection. Collections are Metabase's way of keeping things organized
- Metabase sees the word in a certain language. A *question* is a GUI or SQL-driven interrogation of a table that preserves Metabase's access to underlying table data. We *like* questions
- Questions, if needed, can become Models. We use Models, in particular when data needs to be transformed. More on that later
- The answers to questions can be viewed as tables or charts. If they are important, they can be saved to a Dashboard for consolidated metrics
- If you must, use Metabase's *query* function. This is like writing a direct SQL command. It'll get you the information you want, but you won't be able to drill down into data sets and click through charts

### As a Transformer

- From time to time, our data sources bring information into our ecosystem in disparate ways
- For example, in some data sources, revenue comes in USD in a $1.00 format. In others, it comes as 100 cents (Stripe). In others, it comes in original FX (some Paddle queries)
- Models are a way of simply transforming data. The most common transformation we do is take Stripe data in cents and make a custom field called revenue. Revenue is then used for visualization in this custom expression
- If you think a custom expression is going to be needed across a dataset, do it at the model level; if you think a custom expression just may be needed for a quick query, you can do one in a Question
- Sometimes we are simply formatting metadata. Stripe dates, for example, come in as UNIX and thus cannot be used for charting. Take a trip to Metabase Admin and table metadata. Search for your table. Click the gear. Change no semantic value to the appropriate item (e.g. creation timestamp) and in the cast field convert from UNIX to seconds. Perform this process BEFORE creating a model

### As a Notification System

- Send queries, questions, etc. on a regular schedule to Slack - it is very easy

### Known Issues

- Due to API limitations, we have a few datasets which rely on manually imported CSV
- For example, we have a Paddle Webhook which captures all present transactions for Clipboard. However, the historical API is bad
- So, I created a Webhook. Then Claude and I studied that format and we imported historical CSV data to backfill. Now we have working historical data
- If you make changes to tables and models and you are impatient, go to the Admin, meta data, and discard the cached values (wait) then force a rescan

### How to Use

- If I have done my job correctly, each of our collections has a simple overview.md and full.md file showing all our active queries in use
- We also have a context.md file showing a lot of what we know about the business, how APIs are connected to it, watch-outs, etc.
- So, if you launch Claude Code CLI and navigate to this repository, it should make a pretty good buddy for self-serve in exploring the databases, generating new queries, etc.
- Don't forget, Metabase also has CLI, so you can interact that way

Happy Dashboarding.

---

# Hevo-Postgres Setup

# Commentary from Harrison

The below documentation was generated in an iterative conversation between me and Claude. I tried to cover all the relevant points in our setup of Hevo. I think if you want to understand Hevo, the best way to do it is look at the database. Nearly every table in the destination postgres database came from a pipeline that Hevo has setup. If you are looking to add a new pipeline, I would work with LLM to understand source documentation. Unless it is a frequently-used integration like Stripe, most sources will require some configuring in REST. Primary watch-out for Hevo is ensuring it isn't setting its Hevo Ingested field as primary key - that makes a mess.

Not much to commenet on in terms of the postgres setup itself. I use DBeaver to get in. Any time you need to auth into the DB, remember to add your IP to the security group on Amazon RDS. 

# Hevo Data Platform Documentation - Third South Capital

## Table of Contents
1. [Platform Overview](#platform-overview)
2. [Account & Access Management](#account--access-management)
3. [Infrastructure Architecture](#infrastructure-architecture)
4. [Pipeline Management](#pipeline-management)
5. [Critical Operational Warnings](#critical-operational-warnings)
6. [Standard Operating Procedures](#standard-operating-procedures)
7. [Business-Specific Pipeline Configurations](#business-specific-pipeline-configurations)
8. [Troubleshooting & Problem Resolution](#troubleshooting--problem-resolution)
9. [Monitoring & Maintenance](#monitoring--maintenance)
10. [New User Onboarding Guide](#new-user-onboarding-guide)
11. [Future Development Areas](#future-development-areas)

## Platform Overview

### Business Context
Third South Capital uses Hevo as the primary data integration platform to consolidate data from 30+ pipelines across multiple businesses into a single analytics database. Hevo was selected over Fivetran and Airbyte due to:
- **Ease of integration** with diverse source systems
- **Effectiveness of support team** for troubleshooting complex setups
- **Native capabilities** for handling various API endpoints and data sources

### Data Flow Architecture
```
[Multiple Source Systems] → [Hevo Pipelines] → [Consolidated PostgreSQL] → [Metabase Analytics]
```

**Philosophy**: Keep complexity in Metabase, not in Hevo transformations. This creates Metabase lock-in but provides better auditability and control over data processing.

## Account & Access Management

### Primary Account Details
- **Admin Email**: harrison@thirdsouth.capital
- **Billing**: Annual plan, renews in June
- **Team Access**: Myles has full login access
- **Adding Users**: Team members can be easily added via email invitation

### Access Control Strategy
- **Hevo**: Email invitations for pipeline management access
- **AWS/Database**: Access via 1Password stored credentials
- **Metabase**: All team members have accounts
- **1Password**: Universal access for all team members (contains API keys)

## Infrastructure Architecture

### Destination Database Configuration
- **Provider**: Amazon Web Services (AWS)
- **Service**: RDS PostgreSQL
- **Instance**: tsc-analytics-sandbox
- **Access Method**: AWS root login via shopping@thirdsouth.capital
- **Credentials**: Stored in 1Password under "tsc_admin data lake"

### Network Access Requirements
- **Database Access**: Requires IP address whitelisting in AWS security groups
- **Setup Process**:
  1. Sign into AWS via root email
  2. Navigate to RDS → tsc-analytics-sandbox
  3. Click first link under security
  4. Edit inbound rules in security group
  5. Add your IP from https://whatismyipaddress.com/

### Database Connection Tools
- **Recommended**: DBeaver with IP whitelisting
- **Alternative**: Direct PostgreSQL connections via standard tools

## Pipeline Management

### Current Pipeline Inventory
- **Total Active Pipelines**: ~30
- **Sync Frequency**: Daily (1x per day across all pipelines)
- **Data Freshness Requirements**: Daily sync meets all current business needs

### Table Naming Convention
**Standard Format**: `[business]_[source]`

**Examples**:
- `ih_stripe` - Icon.Horse Stripe account data
- `et_mysql` - EntryThingy MySQL database
- `clipboard_paddle` - Clipboard Paddle API data

### Source System Categories

#### Native Integrations (Easy Setup)
- **Stripe**: All businesses using Stripe have native integration with full table access
- **Standard APIs**: Most modern SaaS platforms with documented APIs

#### Complex Integrations (Require Special Handling)
- **MySQL Databases**: Direct database connections (EntryThingy, UploadThingy)
- **Webhook-Based**: Real-time event data (Clipboard via Paddle webhooks)
- **CSV + API Hybrid**: Historical data via CSV, ongoing via API (PayPal scenarios)

#### Intermediary Services
- **Shopify via Mantle**: Shopping Cart Apps uses www.heymantle.com to avoid GraphQL complexities
- **Third-Party Processors**: When direct API integration is problematic

## Critical Operational Warnings

### 🚨 PRIMARY KEY DUPLICATION ISSUE
**Problem**: Hevo automatically sets `hevo_ingested_at` as the primary key for new tables, creating massive duplicates.

**Impact**: Can result in hundreds of thousands of duplicate records.

**Prevention**: 
- Disable auto-mapping for new tables, OR
- Monitor initial imports closely for record count anomalies

**Detection**: If a small business import shows 500k+ rows, immediately investigate.

### 🚨 TRANSFORMATION SCOPE ISSUE
**Problem**: Hevo transformations apply to ALL tables in a pipeline, regardless of UI selection.

**UI Deception**: Selecting "Charge" in the transformation UI does NOT restrict the transformation to only charge objects during execution.

**Our Policy**: **NEVER use Hevo transformations**. Handle all data transformations in Metabase models.

**If You Must Transform**: Wrap all transformation logic in conditional checks:
```python
eventName = event.getEventName()
if eventName == 'charge':
    # transformation logic here
```

### 🚨 PADDLE API DUPLICATION
**High-Risk Source**: Paddle API is a major offender for duplication issues when `hevo_ingested_at` is set as primary key.

**Special Handling Required**: Always disable auto-mapping for Paddle pipelines and manually configure primary keys.

## Standard Operating Procedures

### Pipeline Setup Process

#### 1. Pre-Setup Validation
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

#### 2. Initial Pipeline Configuration
1. **Let auto-mapper work initially** for first attempt
2. **Monitor import closely** for duplication signs
3. **Review destination documentation** for API-specific requirements
4. **Configure daily sync frequency** (standard across all pipelines)

#### 3. Duplication Detection and Resolution
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

### API Key Management
- **Storage**: All API keys stored in 1Password
- **Access Pattern**: Team members retrieve keys from 1Password as needed
- **Security**: No API keys stored in Hevo interface or other locations

### Monitoring and Validation
- **Daily**: Hevo sends email notifications for complete pipeline failures
- **Manual**: Periodic spot-checks of record counts and data freshness
- **Discovery Queries**: Regular PostgreSQL queries to validate data integrity

## Business-Specific Pipeline Configurations

### EntryThingy
**Sources**:
- MySQL database (operational and financial data)
- PayPal API (attempted, limited to 31-day windows)

**Known Issues**:
- Missing AWM PayPal subscription data (requires CSV upload)
- PayPal webhook integration unsuccessful
- SFTP access request to PayPal pending response

**Current Workaround**: Historical PayPal data via CSV upload to Metabase

### UploadThingy
**Sources**:
- MySQL database (operational and financial data)
- PayPal API (same 31-day limitation as EntryThingy)

**Status**: Same PayPal integration challenges as EntryThingy

### Stripe-Based Businesses
**Active Integrations**:
- ZenDesk: Stripe financial data + web analytics
- GitZen: Stripe financial data + web analytics
- Tissue: Stripe financial data
- Icon.Horse: Stripe financial data

**Success Pattern**: Native Stripe integrations work reliably with full table access

### Clipboard (Complex Setup)
**Challenge**: Paddle API integration difficulties

**Solution Architecture**:
- **Webhook**: Recent events via Paddle webhook
- **CSV Upload**: Historical data via Metabase CSV import
- **Join Strategy**: Big join operation during analysis to combine datasets

**Special Pipeline**: `CHP Paddle Classic Transactions 909164` represents specific subscription plan data

### Shopping Cart Apps (Most Complex)
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

### Inactive Pipelines
**SCA S3 Bucket**: 
- Contains extensive customer feed and data
- **Status**: Inactive due to data volume
- **Reason**: Too much data for efficient processing
- **Future Consideration**: Selective data import strategies

## Troubleshooting & Problem Resolution

### Common Issues and Solutions

#### 1. Pipeline Setup Difficulties
**Symptoms**: 
- Authentication failures
- Connection timeouts
- Incomplete data imports

**Resolution Process**:
1. **Validate API connectivity** via curl commands
2. **Check API documentation** for endpoint changes or authentication updates
3. **Review source system limitations** (rate limits, data access restrictions)
4. **Test authentication methods** (headers vs. embedded auth)

#### 2. Massive Data Imports (Over-ingestion)
**Symptoms**:
- Hundreds of thousands of records from small business sources
- Import jobs running for excessive time periods
- Database storage consumption spikes

**Immediate Actions**:
1. **Stop pipeline** immediately
2. **Run discovery queries** to assess duplication extent
3. **Drop tables** and start fresh rather than attempting cleanup
4. **Reconfigure pipeline** with manual primary key selection

#### 3. Authentication Expiration
**Symptoms**:
- Previously working pipelines suddenly failing
- Authentication error messages in Hevo dashboard

**Resolution**:
1. **Check 1Password** for updated API credentials
2. **Test API access** with current credentials via curl
3. **Update Hevo pipeline** with new authentication details
4. **Resume sync** and monitor for success

#### 4. Schema Changes at Source
**Symptoms**:
- New columns not appearing in destination
- Data type mismatches
- Import failures after source system updates

**Resolution**:
1. **Review source system changelog** for recent updates
2. **Update Hevo pipeline** to accommodate new schema
3. **Test with small data sample** before full sync
4. **Update Metabase models** to incorporate new fields

### Advanced Troubleshooting Techniques

#### Discovery Queries for Duplicate Detection
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

#### API Connectivity Validation
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

## Monitoring & Maintenance

### Automated Monitoring
- **Hevo Email Alerts**: Automatic notifications for complete pipeline failures
- **No Custom Alerting**: Relying on Hevo's built-in monitoring

### Manual Monitoring Procedures

#### Daily Checks (Recommended)
- Review Hevo dashboard for pipeline health status
- Check for any email alerts from previous day
- Spot-check high-volume or critical business pipelines

#### Weekly Checks
- Validate data freshness in Metabase dashboards
- Review any pipelines showing warnings or unusual activity
- Monitor storage consumption in AWS RDS

#### Monthly Checks
- Review all pipeline configurations for optimization opportunities
- Analyze data volume trends for capacity planning
- Update API credentials approaching expiration

### Performance Considerations
- **Sync Frequency**: Daily sync sufficient for current business needs
- **Data Volume**: Monitor for unexpected volume increases
- **Storage Growth**: Track PostgreSQL storage consumption trends

## New User Onboarding Guide

### First Login Checklist
When a new user logs into Hevo for the first time:

#### 1. Understand the Dashboard
- **30+ Pipelines**: Don't be overwhelmed by the number
- **Naming Convention**: `[business]_[source]` format
- **Status Indicators**: Green = healthy, Red = requires attention

#### 2. Essential Settings to Understand
- **Sync Frequency**: All pipelines set to daily
- **Transformations**: **NEVER USE** - handled in Metabase
- **Auto-mapping**: Default setting, but monitor for duplication issues

#### 3. What to Avoid
- **Transformation Features**: Can affect all tables despite UI suggestions
- **Primary Key Changes**: Don't modify without understanding duplication implications
- **Pipeline Restarts**: Try to avoid unless absolutely necessary

#### 4. Safe Actions for New Users
- **View pipeline status** and health indicators
- **Review sync logs** for understanding data flow
- **Monitor record counts** for anomaly detection
- **Access help documentation** within Hevo interface

#### 5. When to Ask for Help
- **Before creating new pipelines**: Get guidance on configuration
- **When seeing unusual record volumes**: 500k+ rows for small business
- **Before modifying existing pipelines**: Understand impact on downstream systems
- **When encountering authentication issues**: May require API key updates

### Essential Knowledge for New Users

#### Understanding Data Flow
```
Source System → Hevo Pipeline → PostgreSQL → Metabase
```

#### Key Concepts
- **Daily Sync**: All data is updated once per day
- **No Real-time**: System designed for business analytics, not real-time operations
- **Single Destination**: All pipelines feed into one consolidated database
- **Metabase Integration**: Final analysis happens in Metabase, not Hevo

#### Red Flags to Watch For
- Record counts in hundreds of thousands from small business sources
- Pipeline failures lasting more than one day
- Authentication errors across multiple pipelines
- Unusual data patterns in destination database

## Future Development Areas

### PayPal Integration Enhancement
**Current Limitation**: 31-day data retrieval limit

**Development Opportunity**:
1. **Setup automated API** to pull 31-day windows continuously
2. **Backfill historical data** via CSV upload to create complete dataset
3. **Combine datasets** to achieve ongoing updated PayPal information
4. **Apply to both EntryThingy and UploadThingy** businesses

### Webhook Integration Improvements
**Current Status**: Some webhook integrations unsuccessful

**Development Areas**:
- **PayPal webhook** setup for real-time transaction data
- **Standardized webhook handling** across multiple business sources
- **Error handling and retry logic** for webhook failures

### Pipeline Optimization
**Monitoring Enhancements**:
- **Automated duplication detection** beyond manual discovery queries
- **Capacity planning** for storage and processing requirements
- **Performance optimization** for large data volume pipelines

### SCA S3 Data Integration
**Current Status**: Inactive due to data volume

**Future Consideration**:
- **Selective data import** strategies
- **Data sampling** approaches for manageable analysis
- **Cost-benefit analysis** of full data integration

### Row Limit Management
**Shopping Cart Apps Challenge**: Annual row limit restrictions

**Development Needs**:
- **Monitoring system** for approaching row limits
- **Data archival strategies** for historical data
- **Alternative data access methods** when limits reached

## Cost Optimization Strategies

### Current Approach
- **Annual billing** for cost savings
- **Daily sync frequency** balances freshness with resource consumption
- **Single destination database** reduces complexity and costs

### Future Optimization Opportunities
- **Pipeline consolidation** where business logic allows
- **Data retention policies** for historical data management
- **Storage optimization** in PostgreSQL for large datasets

## Vendor Relationship Management

### Hevo Support Strategy
- **Leverage support team effectiveness** (key vendor selection factor)
- **Document recurring issues** for faster resolution
- **Maintain good relationship** for complex integration assistance

### Alternative Vendor Considerations
- **Fivetran and Airbyte** were evaluated but Hevo chosen for specific advantages
- **Maintain awareness** of competitive landscape for future decisions
- **Avoid over-dependency** while leveraging Hevo's strengths

---

## Conclusion

This documentation provides comprehensive guidance for operating Hevo as Third South Capital's primary data integration platform. The key to success with this setup is:

1. **Understanding the limitations** and working within them
2. **Following established procedures** for common operations
3. **Monitoring proactively** for issues before they become critical
4. **Leveraging Hevo's strengths** while handling complexity in Metabase

The architecture successfully consolidates data from diverse sources into a single analytics environment, enabling comprehensive business intelligence across all Third South Capital businesses while maintaining operational simplicity and cost effectiveness.

**Critical Success Factors**:
- Daily sync frequency meets business needs
- Single destination database simplifies analysis
- Avoiding Hevo transformations prevents complexity
- Comprehensive monitoring prevents major issues
- Clear procedures enable team scalability

**Primary Risk Mitigation**:
- Duplication detection and resolution procedures
- API connectivity validation before pipeline setup
- Conservative approach to pipeline modifications
- Team knowledge sharing and documentation maintenance

---

# Known Business-Specific Issues

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed non risus. Suspendisse lectus tortor, dignissim sit amet, adipiscing nec, ultricies sed, dolor. Cras elementum ultrices diam. Maecenas ligula massa, varius a, semper congue, euismod non, mi. Proin porttitor, orci nec nonummy molestie, enim est eleifend mi, non fermentum diam nisl sit amet erat. Duis semper. Duis arcu massa, scelerisque vitae, consequat in, pretium a, enim. Pellentesque congue. Ut in risus volutpat libero pharetra tempor. Cras vestibulum bibendum augue. Praesent egestas leo in pede. Praesent blandit odio eu enim. Pellentesque sed dui ut augue blandit sodales. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Aliquam nibh. Mauris ac mauris sed pede pellentesque fermentum. Maecenas adipiscing ante non diam sodales hendrerit.
