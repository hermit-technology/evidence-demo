---
title: Evidence Demo
---

This application demonstrates how you can write raw SQL queries in Evidence and get nice, performant data visualization, along with CI. For an example of how easy this is, you can see the commit that created the map of the US [here.](https://github.com/hermit-technology/evidence-demo/commit/4b8d9f07f2e488b8a9356d2e8193536209c544c9)

As far as we know, no other visualization tool supports real CI/CD, the ability to potentially embed artefacts directly in other websites, or the ability to know if a dependency is broken (upstream teams can try to build this project with the `--strict` flag as a smoke test before pushing changes to database schemas, etc.)

```sql categories
  select
      category
  from needful_things.orders
  group by category
```

<Dropdown data={categories} name=category value=category>
    <DropdownOption value="%" valueLabel="All Categories"/>
</Dropdown>

<Dropdown name=year>
    <DropdownOption value=% valueLabel="All Years"/>
    <DropdownOption value=2019/>
    <DropdownOption value=2020/>
    <DropdownOption value=2021/>
</Dropdown>

```sql orders_by_category
  select 
      date_trunc('month', order_datetime) as month,
      sum(sales) as sales_usd,
      category
  from needful_things.orders
  where category like '${inputs.category.value}'
  and date_part('year', order_datetime) like '${inputs.year.value}'
  group by all
  order by sales_usd desc
```

<BarChart
    data={orders_by_category}
    title="Sales by Month, {inputs.category.label}"
    x=month
    y=sales_usd
    series=category
/>

```sql orders_by_state
select 
  sum(sales) as total_sales,
  state
from needful_things.orders
group by state
```

<USMap
    data={orders_by_state}
    state=state
    value=total_sales
/>
