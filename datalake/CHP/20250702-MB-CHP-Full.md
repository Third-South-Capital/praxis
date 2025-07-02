# CHP - Complete Documentation

*Generated: 2025-07-02*
*Metabase Instance: https://drafty-bail.metabaseapp.com*
*Collection ID: 30*

## Table of Contents
1. [Overview](#overview)
2. [Models/Datasets](#modelsdatasets)
3. [Questions/Cards](#questionscards)
4. [Dashboards](#dashboards)
5. [Data Dependencies](#data-dependencies)
6. [Maintenance Notes](#maintenance-notes)

## Overview

### Summary Statistics
- **Total Models/Datasets**: 15
- **Total Questions/Cards**: 2
- **Total Dashboards**: 1
- **Total Items**: 18

## Models/Datasets

Models are reusable data sources that can be used as starting points for questions.

### 1. CHP $14.99 Annual 649335

**Model ID**: `250`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_649335_api
WHERE status = 'completed'
```

### 2. CHP $195.69 Enterprise Clean 905112

**Model ID**: `257`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_905112_api
WHERE status = 'completed'
```

### 3. CHP $19.99 Annual Clean 705096

**Model ID**: `254`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_705096_api
WHERE status = 'completed'
```

### 4. CHP $1.99 Monthly Clean 649334

**Model ID**: `253`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_649334
WHERE status = 'completed'
```

### 5. CHP $1.99 Weekly Clean 909164

**Model ID**: `258`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_909164_api
WHERE status = 'completed'
```

### 6. CHP $23.99 Annual Clean 752111

**Model ID**: `248`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_752111_api
WHERE status = 'completed'
```

### 7. CHP $2.49 Monthly Clean 705095

**Model ID**: `249`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_705095_api
WHERE status = 'completed'
```

### 8. CHP $47.90 Annual Clean 631102

**Model ID**: `252`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_631102_api
WHERE status = 'completed'
```

### 9. CHP $4.99 Monthly Clean 596916

**Model ID**: `251`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_596916_api
WHERE status = 'completed'
```

### 10. CHP $5.99 Monthly Clean 887004

**Model ID**: `255`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_887004_api
WHERE status = 'completed'
```

### 11. CHP $60.00 Annual Clean 904538

**Model ID**: `256`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    *,
    amount::numeric as revenue,
    created_at::timestamp as transaction_date
FROM chp_paddle_904538_api
WHERE status = 'completed'
```

### 12. CHP CSV Import 06232025

**Model ID**: `265`
**Database**: TSC Datalake
**Created**: 2025-06-24
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

### 13. CHP Unified ALL Transactions - Clean

**Model ID**: `268`
**Database**: TSC Datalake
**Created**: 2025-06-24
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

### 14. CHP Unified ALL Transactions - Clean v2

**Model ID**: `269`
**Database**: TSC Datalake
**Created**: 2025-06-24
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

### 15. CHP Unified Clean Model

**Model ID**: `259`
**Database**: TSC Datalake
**Created**: 2025-06-23
**Creator**: Harrison Roday
**Last Updated**: 2025-06-24

**Source Query**:
```sql
SELECT 
    'Annual $23.99' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
    "user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#248}}

UNION ALL

SELECT 
    'Monthly $2.49' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#249}}

UNION ALL

SELECT 
    'Annual $14.99' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#250}}

UNION ALL

SELECT 
    'Annual $47.90' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#252}}

UNION ALL

SELECT 
    'Monthly $4.99' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#251}}

UNION ALL

SELECT 
    'Monthly $1.99' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#253}}

UNION ALL

SELECT 
    'Annual $19.99' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#254}}

UNION ALL

SELECT 
    'Monthly $5.99' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#255}}

UNION ALL

SELECT 
    'Annual $60.00' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#256}}

UNION ALL

SELECT 
    'Enterprise $195.69' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#257}}

UNION ALL

SELECT 
    'Weekly $1.99' as plan_type,
    revenue,
    currency,
    CASE 
        WHEN currency = 'USD' THEN revenue
        WHEN currency = 'EUR' THEN revenue * 1.08
        WHEN currency = 'GBP' THEN revenue * 1.27
        WHEN currency = 'CAD' THEN revenue * 0.73
        WHEN currency = 'AUD' THEN revenue * 0.65
        WHEN currency = 'JPY' THEN revenue * 0.0066
        WHEN currency = 'PLN' THEN revenue * 0.25
        WHEN currency = 'UAH' THEN revenue * 0.025
        WHEN currency = 'HUF' THEN revenue * 0.0028
        WHEN currency = 'BRL' THEN revenue * 0.19
        WHEN currency = 'SEK' THEN revenue * 0.096
        WHEN currency = 'MXN' THEN revenue * 0.057
        WHEN currency = 'ZAR' THEN revenue * 0.054
        WHEN currency = 'HKD' THEN revenue * 0.13
        WHEN currency = 'ILS' THEN revenue * 0.27
        WHEN currency = 'TWD' THEN revenue * 0.031
        WHEN currency = 'NOK' THEN revenue * 0.093
        WHEN currency = 'CHF' THEN revenue * 1.10
        WHEN currency = 'COP' THEN revenue * 0.00025
        WHEN currency = 'INR' THEN revenue * 0.012
        WHEN currency = 'VND' THEN revenue * 0.000041
        WHEN currency = 'NZD' THEN revenue * 0.61
        WHEN currency = 'TRY' THEN revenue * 0.052
        WHEN currency = 'CZK' THEN revenue * 0.043
        WHEN currency = 'ARS' THEN revenue * 0.0013
        WHEN currency = 'SGD' THEN revenue * 0.74
        WHEN currency = 'THB' THEN revenue * 0.028
        WHEN currency = 'RUB' THEN revenue * 0.011
        ELSE revenue
    END as revenue_usd,
    transaction_date,
    order_id,
	"user"::json->>'user_id' as user_id,
    "user"::json->>'email' as user_email
FROM {{#258}}
```

## Questions/Cards

These are the analytical queries and visualizations in this collection.

### 1. CHP CSV Import 06232025 - Clean

**Card ID**: `266`
**Visualization**: table
**Database**: TSC Datalake
**Query Type**: Query Builder
**Created**: 2025-06-24
**Creator**: Harrison Roday

### 2. CHP Monthly Revenue

**Card ID**: `270`
**Visualization**: bar
**Database**: TSC Datalake
**Query Type**: Query Builder
**Created**: 2025-06-24
**Creator**: Harrison Roday

## Dashboards

Interactive dashboards combining multiple visualizations.

### 1. CHP Prod Board

**Dashboard ID**: `29`
**Created**: 2025-06-23
**Creator**: Unknown

**Cards** (10 total):
- CHP Monthly Revenue (ID: 270) - bar
- CHP Revenue by Plan (ID: 271) - bar
- CHP Customers by Month (ID: 272) - bar
- CHP LTM Revenue (ID: 274) - bar
- CHP YoY (ID: 275) - table
- CHP Annualize (ID: 276) - table
- CHP Users Less Than or Equal to Zero Revenue (ID: 278) - bar
- CHP Webstore Visits (ID: 202) - bar
- CHP MAU (ID: 203) - bar
- CHP Net New Subs (Monthly) (ID: 204) - bar

## Data Dependencies

### Tables Referenced

The following tables are referenced in SQL queries:
- `chp_paddle_596916_api`
- `chp_paddle_631102_api`
- `chp_paddle_649334`
- `chp_paddle_649335_api`
- `chp_paddle_705095_api`
- `chp_paddle_705096_api`
- `chp_paddle_752111_api`
- `chp_paddle_887004_api`
- `chp_paddle_904538_api`
- `chp_paddle_905112_api`
- `chp_paddle_909164_api`
- `{{#248}}`
- `{{#249}}`
- `{{#250}}`
- `{{#251}}`
- `{{#252}}`
- `{{#253}}`
- `{{#254}}`
- `{{#255}}`
- `{{#256}}`
- `{{#257}}`
- `{{#258}}`

### Database Usage

| Database | Items Using |
|----------|-------------|
| TSC Datalake | 17 |

## Maintenance Notes

### Recently Updated Items
- CHP Monthly Revenue (question) - 2025-06-24
- CHP Unified ALL Transactions - Clean v2 (model) - 2025-06-24
- CHP Prod Board (dashboard) - 2025-06-24
- CHP $23.99 Annual Clean 752111 (model) - 2025-06-24
- CHP $14.99 Annual 649335 (model) - 2025-06-24

---

*This documentation was automatically generated on 2025-07-02*
*For the latest information, visit: https://drafty-bail.metabaseapp.com/collection/30*