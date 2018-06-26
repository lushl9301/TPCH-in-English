```SQL
-- TPC-H Query 3
-- this query should be simple after reading q1 and q2
select
        l_orderkey,
        sum(l_extendedprice * (1 - l_discount)) as revenue,
        o_orderdate,
        o_shippriority
from
        customer,
        orders,
        lineitem
-- Simple criteria
where
        c_mktsegment = 'BUILDING'
        and c_custkey = o_custkey
        and l_orderkey = o_orderkey
        and o_orderdate < date '1995-03-15'
        and l_shipdate > date '1995-03-15'

-- group rows that with the same (l_orderkey,o_orderdate,,o_shippriority)
group by
        l_orderkey,
        o_orderdate,
        o_shippriority
order by
        revenue desc,
        o_orderdate

-- TOP 10
limit 10
```
