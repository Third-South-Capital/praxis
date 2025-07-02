# CLAUDE.md - Shopping Cart Apps (SCA)

## Overview
This file helps Claude assist you with SCA data analysis and query development. SCA is a tiered SaaS business managing shopping feeds for Shopify/Wix stores.

## Key Tables to Know
When analyzing SCA data, these are your primary tables:
- `sca_mantle_consol` - All financial transactions (use `netamount` for revenue)
- `sca_s3data_lateststore` - Current store data (CSV format in single column)
- `sca_s3data_latestfeeds` - Product catalog data
- `mv_store_metrics` - Pre-calculated metrics (faster for dashboards)

## Common Query Patterns

### Parsing Store Data
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

### Revenue Calculations
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

### Feed Quality Analysis
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

## Business Logic to Remember

### Tier Limits and Pricing
- Trial: $0, 100,000 products
- Small: $9.95, 1,000 products
- Medium: $19.95, 10,000 products
- Large: $29.95, 100,000 products

### Important Metrics
1. **Zombie Customers**: Paying but no active feeds (17.8% currently)
2. **Tier Violations**: Stores exceeding their plan limits
3. **Platform Performance**: Shopify converts at 97%, Wix at 5.6%

## Query Ideas to Explore

### Revenue Optimization
- Find stores exceeding tier limits (upgrade opportunities)
- Identify zombie customers for recovery campaigns
- Platform-specific conversion analysis

### Product Analysis
- Feed quality scoring by store
- Product catalog growth trends
- Most common product categories

### Customer Behavior
- Trial to paid conversion rates
- Churn analysis by tier and platform
- Usage patterns by plan type

## Data Gotchas
1. **Store Data Format**: Always use SPLIT_PART for CSV parsing
2. **Empty Values**: Use NULLIF to handle empty strings in numeric fields
3. **Date Fields**: Transaction dates are in `date` column, not `createdat`
4. **Duplication**: Latest feeds/stores only - no historical duplication
5. **Pagination**: Mantle API data comes in 25-record pages

## Useful Explorations
When working with Claude, consider asking about:
- "Show me stores that should upgrade based on product count"
- "Calculate potential revenue from tier enforcement"
- "Analyze feed quality by platform"
- "Find patterns in zombie customer behavior"
- "Compare Shopify vs Wix performance metrics"

## Testing Queries
Always test with small samples first:
```sql
-- Add LIMIT when testing
SELECT * FROM sca_s3data_lateststore LIMIT 10;
```

## Performance Tips
- Use `mv_store_metrics` for dashboard queries (pre-calculated)
- Index on `customerid` and `date` for transaction queries
- Avoid large UNION queries in DBeaver (use INSERT instead)