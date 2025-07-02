# Shopping Cart Apps (SCA) Context

## Business Overview
- **Full Name**: Shopping Cart Apps
- **Business Model**: Tiered SaaS subscriptions based on product count
- **Primary Product/Service**: Shopping feed management for Shopify/Wix stores
- **Launch Date**: 2016
- **Current Status**: Active

## Data Sources
- **Primary Payment Processor**: Shopify (via Mantle API)
- **Database Tables**: 
  - `sca_mantle_consol` - Consolidated transaction data (2016-2025, 36,926+ records)
  - `sca_s3data_lateststore` - Store information in CSV format (475 stores)
  - `sca_s3data_latestfeeds` - Product catalog data (72,081+ products)
  - `mv_store_metrics` - Materialized view for performance optimization
- **API Integrations**: 
  - Mantle API (handles Shopyif data consolidation)
  - Shopify App Store
  - Wix App Market
- **AWS Infrastructure**:
  - Lambda functions for data preprocessing (CopyLatestFeeds, CopyLatestStoreReport)
  - S3 bucket (sca-feed-storage) for data staging
  - IAM roles configured for Lambda-S3 access

## Key Metrics
- **Revenue Model**: Monthly tiered subscriptions
- **Pricing Tiers**: 
  - Trial: $0/month (100,000 product limit) - 48.63% of stores
  - Small: $9.95/month (1,000 product limit) - 48.42% of stores
  - Medium: $19.95/month (10,000 product limit) - 2.53% of stores
  - Large: $29.95/month (100,000 product limit) - 0.42% of stores
- **Key KPIs**: 
  - Active stores: 475
  - Total products managed: 72,081
  - Zombie customers: 17.8% (paying but no feeds)
  - Platform conversion: Shopify 97.19% vs Wix 5.58%

## Data Pipeline Details
- **Hevo Pipeline Name**: SCA S3 Latest Store/Feeds
- **Sync Frequency**: Daily after Lambda preprocessing
- **Processing Flow**: S3 Raw → Lambda → S3 Latest → Hevo → PostgreSQL
- **Known Issues**: 
  - Mantle API limited to 25 records per page (resolved with pagination)
  - Historical feed duplication (resolved by keeping latest only)
- **Data Transformations**: 
  - CSV parsing for store data using SPLIT_PART
  - Revenue in USD (netamount field is key metric)
  - Lambda preprocessing to deduplicate and extract latest data

## Important Business Rules
- **Revenue Recognition**: Monthly charge from active (non-trial) plans
- **Customer Definition**: Store with active subscription
- **Active/Inactive Criteria**: Based on plan_type and feed activity
- **Special Calculations**: 
  - Feed quality score: 0-100 based on field completeness
  - Tier recommendation: Based on product count vs plan limits
  - Zombie detection: Paying customers without active feeds

## Technical Notes
- **Time Zone**: UTC for all timestamps
- **Currency**: USD
- **Fiscal Year**: Calendar year
- **Data Retention**: Current snapshot only for stores/feeds, full history for transactions

## Contacts & Ownership
- **Technical Contact**: Harrison
- **Data Questions**: Harrison

## Known Issues & Gotchas
- **Tier Enforcement Gap**: $7,200+ annual revenue opportunity from enforcing limits
- **CSV Format**: Store data in single column requires SPLIT_PART parsing
- **DBeaver Limitation**: Cannot handle large UNION queries
- **Zombie Customers**: 17.8% paying but not using feeds
- **Platform Performance**: Significant conversion difference between Shopify and Wix

## Future Improvements
- Implement automatic tier enforcement based on product count
- Develop zombie customer recovery campaign
- Focus growth efforts on Shopify platform given higher conversion
- Consider read replicas for database scaling

## Related Documentation
- Overview: `20250702-MB-SCA-Overview.md`
- Full Documentation: `20250702-MB-SCA-Full.md`
- Metabase Collection: https://drafty-bail.metabaseapp.com/collection/31