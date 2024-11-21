# Approach

We aim to recreate every legacy table by a single dbt model. If necessary we use additional “intermediate” models to simplify maintenance and development.

Generally speaking all legacy logic manipulating data in legacy table ABC will be combined in the single dbt model. There is NO distinction between update and insert models. All models handle both updates and inserts.

# Steps

We migrate legacy table by legacy table. Take care of all outlined steps for a table to finish its migration.

Here is the default process to be used to migrate existing logic to dbt:

- [Code documentation](https://sagdigital.atlassian.net/wiki/spaces/Pythia/pages/688717925 "/wiki/spaces/Pythia/pages/688717925") (documentation: anyone, review: SAG)
    
    - Fill out the code documentation tables for the table.
        
    - Make sure to go through all stored procedures manipulating the table.
        
    - Add the tables to the [Code Translation pages](https://sagdigital.atlassian.net/wiki/spaces/Pythia/pages/655721813 "/wiki/spaces/Pythia/pages/655721813").
        
- Data export
    
    - As long as the full data ingestion solution is not running:
        
        - Export data from the source manually to the Sandbox Blob Storage (SAG)
            
        - Ingest the file into Snowflake external stages and tables (anyone)
            
    - As soon as the data ingestion setup is done
        
        - Store the relevant source tables as parquet files in the blob storage
            
- Code development
    
    - [Plan dbt models](https://sagdigital.atlassian.net/wiki/spaces/Pythia/pages/697729046 "/wiki/spaces/Pythia/pages/697729046") (taod)
        
        - Simplify and combine the transformations outlined in the code documentation
            
        - Plan the conditional application of logic by employing CASE WHEN statements or CTEs
            
        - Document assumptions about the referenced data
            
    - Write dbt pseudo-code (taod)
        
    - Write dbt code (SQL) (taod)
        
    - Turn the code into Jinja Macros if already applicable (taod)
        
        - Focusing on making the same Macro reusable for other data sources
            
- [Data Comparison](https://sagdigital.atlassian.net/wiki/spaces/Pythia/pages/682328154 "/wiki/spaces/Pythia/pages/682328154") (taod)
    
    - Compare the legacy table data with the dbt model data using the comparison macros
        
    - Document the findings in Confluence
        

# Priorities

Look at the following order to figure out which ticket you should focus on. Higher up means more important:

1. Removing blockers
    
    1. Any ticket that is created outside of the typical ticket designs and marked as a [blocker] has the highest priority. This ticket is preventing someone from progressing on their highest priority tasks.
        
2. Reviews
    
3. dbt Implementation
    
4. Model Planning
    
5. Code Documentation