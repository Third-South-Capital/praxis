# Data Lake Roadmap

*Last Updated: 2025-07-02*

## Priority 1: Fix PayPal Integration Issues

### Problem
- EntryThingy and UploadThingy PayPal data limited to 31-day API windows
- Missing historical subscription data (AWM)
- PayPal webhook integration unsuccessful
- SFTP access request to PayPal pending

### Solution
1. Set up automated daily API pull for 31-day rolling window
2. Backfill historical data via CSV upload to Metabase
3. Create unified model combining API + CSV data
4. Follow up on PayPal SFTP access request

### Affected Businesses
- EntryThingy (ET)
- UploadThingy (UT)

## Priority 2: Migrate Stripe Data from Gross to Net Revenue

### Problem
- Currently using `stripe_charge` table showing gross revenue
- Need to switch to `stripe_balance_transactions` for net revenue (after fees)

### Solution
1. Create new models using `stripe_balance_transactions` table
2. Update all revenue calculations to use net amounts
3. Migrate dashboards to new net revenue models
4. Archive old gross revenue models

### Affected Businesses
- Icon.Horse (IH)
- GitZen (GZ)
- TissueApp (TA)
- ZenDesk Apps (ZDA)

## Timeline

### Q3 2025
- [ ] Fix PayPal integration for ET/UT
- [ ] Begin Stripe net revenue migration

### Q4 2025
- [ ] Complete Stripe net revenue migration
- [ ] Archive deprecated models