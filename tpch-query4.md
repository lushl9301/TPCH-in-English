```SQL
-- TPC-H Query 4
-- Select and count total number within a group
select
        o_orderpriority,
        count(*) as order_count
from
        orders
-- orderdate within a range
where
        o_orderdate >= date '1993-07-01'
        and o_orderdate < date '1993-10-01'
        -- if there is a record that is in table 'lineitem'
        --   and commmit date is earlier than receipt date.
        and exists (
                select
                        *
                from
                        lineitem
                where
                        l_orderkey = o_orderkey
                        and l_commitdate < l_receiptdate
        )
group by
        o_orderpriority
order by
        o_orderpriority
```
