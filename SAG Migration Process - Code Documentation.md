To standardize and simplify the planning of our dbt models, we fill out a number of tables.

Fill this set of tables for each legacy table. Make sure to check all stored procedures manipulating the table.

Italics are only for explanations. You don’t need to keep them in your documentation.

# Step by Step

## Code Documentation

1. Fill out the overview table fields **Company Code, Source System & Related Stored Procedures**
    
2. For the first stored procedure, create the References and Transformations table **Output Column, Source Reference, Transformational Steps, Stored Procedure**
    
    1. The Output Column has to be the exact same name as in the legacy asset
        
    2. The source reference has to be referenced from outside the asset and stored procedure if possible. If the reference is a temporary table, find the upstream asset of that temporary table.
        
3. Add the transformations from the remaining stored procedures to the table (repeat the same step)
    
4. _[DEPRECATED, handled in model planning]_ Add the **Assumptions** column.
    
5. List the joins, rename the primary tables as _primary_table_. Primary tables are the primary entity used or built in the stored procedures. They are the ones getting upserts.
    
6. Plan the **Data Scope of Transformations** based on internal references, required multi-step transformations and multiple occurences of the same output column. Make sure to add the info to the joins as well.
    
7. _[DEPRECATED, we assume that all columns are relevant]_ Add the column **In use**.
    

If possible, ask for a review now. You can start the model planning without a review if the planning is high priority. But be aware that this might cause you to do unnecessary work.

# Code documentation

## Overview

|                           |                                                                                                                                                                                                                                |                                                                                                                                                                                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Documented legacy table   | TurnoverDetailAX                                                                                                                                                                                                               | _Name of the table_                                                                                                                                                                                         |
| Company Code              | SAGCZ                                                                                                                                                                                                                          | _Company Code as used in the Naming convention_                                                                                                                                                             |
| Source System             | D365                                                                                                                                                                                                                           | _Source System Code as used in the Naming Convention_                                                                                                                                                       |
| Related Stored Procedures | - (1_base) AX10_DataImport_Customers_TurnoverDetailAX<br>    <br>- (2_recids) AX10_DataImport_Customers_TurnoverDetailAX_UpdateRecIDs<br>    <br>- (3_transport) AX10_DataImport_Customers_TurnoverDetailAX_TransportationCost | _Stored Procedures that manipulate the output table. Prefix by an alias and ordered. The digit at the beginning represents the order in which the stored procedures run. (**1**_base runs first and so on)_ |
| Description               | This table stores invoice line items for SAGCZ                                                                                                                                                                                 | _Description of the table, to be re-used in dbt._                                                                                                                                                           |
| dbt Model name            | `bc_turnoverdetailax_sagcz`                                                                                                                                                                                                    | _Model Name in dbt_                                                                                                                                                                                         |
| dbt Directory             | models/70_business_core/sales/sales_sagcz/                                                                                                                                                                                     | _Directory for the model in dbt_                                                                                                                                                                            |
| Snowflake target schema   | */sc_biz                                                                                                                                                                                                                       | _Namespace in Snowflake_                                                                                                                                                                                    |

## References and Transformations

|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
|**Output Column**|**Source Reference**|**Transformational Steps**|**Stored Procedure**|**Assumptions**|**Data Scope of Transformations**|**In use**|
|_Name of the column in the legacy table_|_List of referenced tables and columns or functions_|_List of the transformational steps performed on the referenced columns to get to the output column_|_Name or Alias of the Stored Procedure handling the transformation_|_Data Quality Assumptions about the output. Add if a column is a key._|_Name of the scope to define which records are handled. (Descr. below)_||
|.Turnover_RecID|AX10_GwsBICustInvoiceTransStaging.DW_CLRECID|Unchanged|1_base|**(primary key)**<br><br>not null<br><br>unique|10_base_upserts|TRUE|
|Turnover_RecID|AX10_GwsBICustInvoiceTransStaging.DW_CLRECID|Max value (grouped by AX10_GwsBICustInvoiceTransStaging.INVOICEID)|3_transport|see other entry|05_default_inserts|FALSE|
|PostingDate|N/A|Static value: '19000101'|1_base||10_base_upserts|TRUE|
|PostingDate|`Customers_Ledger`.PostingDate|Unchanged|2_recids||10_base_upserts|TRUE|
|List_Value|AX10_GwsBICustInvoiceTransStaging.QTY, AX10_GwsBICustInvoiceTransStaging.SALESPRICE, AX10_GwsBICustInvoiceTransStaging.LINEAMOUNT, AX10_GwsBICustInvoiceTransStaging.LINEAMOUNTMST|DM.QTY * DM.SALESPRICE * case when DM.LINEAMOUNT = 0 then 1 else DM.LINEAMOUNTMST/DM.LINEAMOUNT end|1_base||10_base_upserts|TRUE|
|Warehouse_RecID|N/A|Static value: '0'|1_base|**(foreign key)**<br><br>relationship to Warehouse table|10_base_upserts|TRUE|

## Data Scope of Transformations

- _scope name (incl prefix to reflect the order of operations)_
    
    - _description_
        
- 05_default_inserts
    
    - Adds default values
        
    - Filtered for AX10_GwsBICustInvoiceTransStaging.DW_CLRECID = 0
        
- 10_base_upserts
    
    - Basic inserts and upserts
        
    - Handling all records (no filters)
        

## Joins & Keys

_We assume that for every upstream reference there’s only one join to the primary reference. E.g. there’s one join involving AX10_GwsBILogisticsPostalAddressStaging so it is easy to recreate the join logic._

### Primary table/s:

- _name of table that is the primary source for the model_
    
- _all joins have to be joined to the primary table/s_
    
- AX10_GwsBICustInvoiceTransStaging
    
- TurnoverDetailAX
    

### Join tables

`Reference: table_name`

_Scope: data scope_

`join definition primary table aliased as primary_table`

`Reference: Customers_OrderLines`

Scope: 10_base_upserts

`inner join AX10_GwsBISalesLineStaging sl on sl.DATAAREAID = primary_table.DATAAREAID and sl.SALESID = primary_table.SALESLINE_SALESORDER_SALESID and sl.INVENTTABLE_ITEM_ITEMID = cit.INVENTTABLE_ITEM_ITEMID and sl.LINENUM = cit.LINENUM inner join Customers_OrderLines col on col.ERPRecID = @gERPID and col.SourcePKID = sl.RECID_PK`