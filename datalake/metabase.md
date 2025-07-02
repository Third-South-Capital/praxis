Here are some thoughts about Metabase

## Preamble

- We use metabase as 1) a data viewer, 2) data transformer, and 3) a nofiication system
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
- Models are a way of simply transforming data. The most common tranfsormation we do is take Stripe data in cents and make a custom field called revenue. Revenue is then used for visualization in this custom expression
- If you think a custom expression is going to be needed across a dataset, do it at the model level; if you think a custom expression just may be needed for a quick query, you can do one in a Question
- Sometimes we are simply formatting metadata. Stripe dates, for example, come in as UNIX and thus cannot be used for charting. Take a trip to Metabase Admin and table metadata. Search for your table. Click the gear. Change no semantic value to the appropriate item (e.g. creation timestamp) and in the cast field convert from UNIX to seconds. Perform this process BEFORE creating a model

### As a Notificaton System

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

