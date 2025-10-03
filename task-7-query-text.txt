
create view vw_customer_order_totals as
select
  c.customernumber,
  c.customername,
  count(distinct o.ordernumber)     as total_orders,
  coalesce(sum(od.quantityordered * od.priceeach), 0) as total_spent
from customers c
left join orders o on c.customernumber = o.customernumber
left join orderdetails od on o.ordernumber = od.ordernumber
group by c.customernumber, c.customername;

select * from vw_customer_order_totals order by total_spent desc limit 20;



create view vw_product_order_summary as
select
  p.productcode,
  p.productname,
  p.productline,
  coalesce(sum(od.quantityordered), 0) as total_qty_sold,
  coalesce(sum(od.quantityordered * od.priceeach), 0) as total_revenue
from products p
left join orderdetails od on p.productcode = od.productcode
group by p.productcode, p.productname, p.productline;

select * from vw_product_order_summary where total_qty_sold = 0;  -- products never sold



create view vw_top_customers as
select t.customernumber, t.customername, t.total_spent
from vw_customer_order_totals t
where t.total_spent > 10000;   -- tune threshold to dataset scale

select * from vw_top_customers order by total_spent desc;



create view vw_employee_sales as
select
  e.employeenumber,
  concat(e.firstname, ' ', e.lastname) as employee_name,
  count(distinct c.customernumber) as customers_managed,
  coalesce(sum(od.quantityordered * od.priceeach), 0) as revenue_from_customers
from employees e
left join customers c on e.employeenumber = c.salesrepemployeenumber
left join orders o on o.customernumber = c.customernumber
left join orderdetails od on od.ordernumber = o.ordernumber
group by e.employeenumber, employee_name;

select * from vw_employee_sales order by revenue_from_customers desc limit 10;



create view vw_order_line_details as
select
  o.ordernumber,
  o.orderdate,
  o.status,
  od.orderlinenumber,
  od.productcode,
  p.productname,
  od.quantityordered,
  od.priceeach,
  (od.quantityordered * od.priceeach) as linetotal,
  o.customernumber
from orders o
join orderdetails od on o.ordernumber = od.ordernumber
join products p on p.productcode = od.productcode;

select * from vw_order_line_details where ordernumber = 10100;



create view vw_productline_performance as
select
  pl.productline,
  pl.textdescription,
  count(distinct p.productcode) as product_count,
  coalesce(sum(od.quantityordered), 0) as total_qty_sold,
  coalesce(sum(od.quantityordered * od.priceeach), 0) as total_revenue
from productlines pl
left join products p on p.productline = pl.productline
left join orderdetails od on od.productcode = p.productcode
group by pl.productline, pl.textdescription;

select * from vw_productline_performance order by total_revenue desc;



create view vw_office_sales as
select
  o.officecode,
  o.city,
  o.country,
  count(distinct c.customernumber) as customers_supported,
  coalesce(sum(od.quantityordered * od.priceeach), 0) as total_revenue
from offices o
left join employees e on e.officecode = o.officecode
left join customers c on c.salesrepemployeenumber = e.employeenumber
left join orders ord on ord.customernumber = c.customernumber
left join orderdetails od on od.ordernumber = ord.ordernumber
group by o.officecode, o.city, o.country;

select * from vw_office_sales order by total_revenue desc;




create view vw_customers_in_city as
select customernumber, customername, phone, city, country, creditlimit
from customers
where city = 'san francisco'
with check option;

select * from vw_customers_in_city;



select customernumber, customername, total_spent
from vw_customer_order_totals
order by total_spent desc
limit 10;

select p.productcode, p.productname
from vw_product_order_summary p
where p.total_qty_sold = 0;

select productline, total_revenue
from vw_productline_performance
order by total_revenue desc
limit 5;

select * from vw_office_sales order by total_revenue desc;

select l.* from vw_order_line_details l
where customernumber = 103
order by orderdate desc;
