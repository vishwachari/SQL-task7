# Task - 07 - SQL Views

## Overview
This task focuses on creating and using **SQL Views** in MySQL.  
It demonstrates how to create reusable query definitions, simplify access, enforce security, and manage complex joins.

## Tools Used
- MySQL Workbench  

## Objectives
- Understand and apply the concept of database views:
  - Create simple and complex views  
  - Use views to simplify reporting queries  
  - Apply `WITH CHECK OPTION` to enforce view constraints  
  - Understand limitations of updateable vs non-updateable views  
- Use views for aggregation, abstraction, and security.  

## Deliverables
- `task-7-views.sql` (SQL script of view definitions and examples)  
- Screenshots of query results from views (customer totals, product summaries, etc.)  
- Documentation in `README.md`  

## Example Views
- **vw_customer_order_totals**: Customer orders and total spent  
- **vw_product_order_summary**: Products with quantity sold & revenue  
- **vw_top_customers**: Customers above a sales threshold  
- **vw_employee_sales**: Sales by employee (via customers)  
- **vw_order_line_details**: Flattened order line details  
- **vw_productline_performance**: Revenue by product line  
- **vw_office_sales**: Revenue by office  
- **vw_customers_in_city**: Updateable view with `WITH CHECK OPTION`  

## Key Learnings
- Views make queries reusable and simplify access to complex joins.  
- Not all views are updateable; avoid aggregates, GROUP BY, DISTINCT if you need updates.  
- `WITH CHECK OPTION` enforces that inserted/updated rows still satisfy the view’s condition.  
- Views do not improve performance automatically — indexes on base tables are still required.  

## Author
Syed Ahmed Ali  
