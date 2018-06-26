```SQL
-- TPC-H Query 2

select
        s_acctbal,
        s_name,
        n_name,
        p_partkey,
        p_mfgr,
        s_address,
        s_phone,
        s_comment
from
        part,
        supplier,
        partsupp,
        nation,
        region
-- so many criteria...
-- In EUROPE, the one with min 'ps_supplycost' and p_size, p_type...
where
        p_partkey = ps_partkey
        and s_suppkey = ps_suppkey
        and p_size = 15
        and p_type like '%BRASS'
        and s_nationkey = n_nationkey
        and n_regionkey = r_regionkey
        and r_name = 'EUROPE'
        and ps_supplycost = (
                select
                        min(ps_supplycost)
                from
                        partsupp,
                        supplier,
                        nation,
                        region
                -- to make sure the corresponding region of the record is 'EUROPE'
                where
                        p_partkey = ps_partkey
                        and s_suppkey = ps_suppkey
                        and s_nationkey = n_nationkey
                        and n_regionkey = r_regionkey
                        and r_name = 'EUROPE'
        )
-- Sort using 4 factors, the first 's_acctbal' in decreasing order.
order by
        s_acctbal desc,
        n_name,
        s_name,
        p_partkey
-- Only shows the top 100 records
limit 100
```
