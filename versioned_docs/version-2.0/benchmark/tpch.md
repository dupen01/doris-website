---
{
    "title": "TPC-H Benchmark",
    "language": "en"
}
---

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

# TPC-H Benchmark

TPC-H is a decision support benchmark (Decision Support Benchmark), which consists of a set of business-oriented special query and concurrent data modification. The data that is queried and populates the database has broad industry relevance. This benchmark demonstrates a decision support system that examines large amounts of data, executes highly complex queries, and answers key business questions. The performance index reported by TPC-H is called TPC-H composite query performance index per hour (QphH@Size), which reflects multiple aspects of the system's ability to process queries. These aspects include the database size chosen when executing the query, the query processing capability when the query is submitted by a single stream, and the query throughput when the query is submitted by many concurrent users.

This document mainly introduces the performance of Doris on the TPC-H 100G test set.

> Note 1: The standard test set including TPC-H is usually far from the actual business scenario, and some tests will perform parameter tuning for the test set. Therefore, the test results of the standard test set can only reflect the performance of the database in a specific scenario. We suggest users use actual business data for further testing.
>
> Note 2: The operations involved in this document are all tested on CentOS 7.x.
>
> Note 3: Doris starting from version 1.2.2, the page cache is turned off by default to reduce memory usage, which has a certain impact on performance. For performance testing, enable the page cache by adding disable_storage_page_cache=false to be.conf.

On 22 queries on the TPC-H standard test data set, we conducted a comparison test based on Apache Doris 1.2.0-rc01, Apache Doris 1.1.3 and Apache Doris 0.15.0 RC04 versions. Compared with Apache Doris 1.1.3, the overall performance of Apache Doris 1.2.0-rc01 has been improved by nearly 3 times, and by nearly 11 times compared with Apache Doris 0.15.0 RC04.

## 1. Hardware Environment

| Hardware           | Configuration Instructions                                   |
| -------- | ------------------------------------ |
| Number of mMachines | 4 Tencent Cloud Virtual Machine（1FE，3BEs） |
| CPU      | Intel Xeon(Cascade Lake) Platinum 8269CY  16C  (2.5 GHz/3.2 GHz) |
| Memory | 64G                                  |
| Network | 5Gbps                              |
| Disk   | ESSD Cloud Hard Disk  |

## 2. Software Environment

- Doris Deployed 3BEs and 1FE
- Kernel Version: Linux version 5.4.0-96-generic (buildd@lgw01-amd64-051)
- OS version: CentOS 7.8
- Doris software version: Apache Doris 1.2.0-rc01、 Apache Doris 1.1.3 、 Apache Doris 0.15.0 RC04
- JDK: openjdk version "11.0.14" 2022-01-18

## 3. Test Data Volume

The TPCH 100G data generated by the simulation of the entire test are respectively imported into Apache Doris 1.2.0-rc01, Apache Doris 1.1.3 and Apache Doris 0.15.0 RC04 for testing. The following is the relevant description and data volume of the table.

| TPC-H Table Name | Rows          | Size after Import  | Annotation |
| :--------------- |:--------------| ---------- | :----- |
| REGION           | 5             | 400KB      | Region       |
| NATION           | 25            | 7.714 KB   | Nation       |
| SUPPLIER         | 1,000,000     | 85.528 MB  | Supplier       |
| PART             | 20,000,000    | 752.330 MB | Parts       |
| PARTSUPP         | 20,000,000    | 4.375 GB   | Parts Supply       |
| CUSTOMER         | 15,000,000    | 1.317 GB   | Customer        |
| ORDERS           | 1,50,000,000  | 6.301 GB   | Orders        |
| LINEITEM         | 6,00,000,000  | 20.882 GB  | Order Details       |

## 4. Test SQL

TPCH 22 test query statements ： [TPCH-Query-SQL](https://github.com/apache/doris/tree/master/tools/tpch-tools/queries)

**Notice:**

The following four parameters in the above SQL do not exist in Apache Doris 0.15.0 RC04. When executing, please remove:

```
1. enable_vectorized_engine=true,
2. batch_size=4096,
3. disable_join_reorder=false
4. enable_projection=true
```

## 5. Test Results

Here we use Apache Doris 1.2.0-rc01, Apache Doris 1.1.3 and Apache Doris 0.15.0 RC04 for comparative testing. In the test, we use Query Time(ms) as the main performance indicator. The test results are as follows:

| Query    | Apache Doris 1.2.0-rc01 (ms) | Apache Doris 1.1.3 (ms) | Apache Doris 0.15.0 RC04 (ms) |
| -------- | --------------------------- | ---------------------- | ---------------------------- |
| Q1       | 2.12                        | 3.75                   | 28.63                        |
| Q2       | 0.20                        | 4.22                   | 7.88                         |
| Q3       | 0.62                        | 2.64                   | 9.39                         |
| Q4       | 0.61                        | 1.5                    | 9.3                          |
| Q5       | 1.05                        | 2.15                   | 4.11                         |
| Q6       | 0.08                        | 0.19                   | 0.43                         |
| Q7       | 0.58                        | 1.04                   | 1.61                         |
| Q8       | 0.72                        | 1.75                   | 50.35                        |
| Q9       | 3.61                        | 7.94                   | 16.34                        |
| Q10      | 1.26                        | 1.41                   | 5.21                         |
| Q11      | 0.15                        | 0.35                   | 1.72                         |
| Q12      | 0.21                        | 0.57                   | 5.39                         |
| Q13      | 2.62                        | 8.15                   | 20.88                        |
| Q14      | 0.16                        | 0.3                    |                              |
| Q15      | 0.30                        | 0.66                   | 1.86                         |
| Q16      | 0.38                        | 0.79                   | 1.32                         |
| Q17      | 0.65                        | 1.51                   | 26.67                        |
| Q18      | 2.28                        | 3.364                  | 11.77                        |
| Q19      | 0.20                        | 0.829                  | 1.71                         |
| Q20      | 0.21                        | 2.77                   | 5.2                          |
| Q21      | 1.17                        | 4.47                   | 10.34                        |
| Q22      | 0.46                        | 0.9                    | 3.22                         |
| **Total** | **19.64**                   | **51.253**             | **223.33**                   |

![image-20220614114351241](/images/tpch.png)

- **Result Description**
    - The data set corresponding to the test results is scale 100, about 600 million.
    - The test environment is configured as the user's common configuration, with 4 cloud servers, 16-core 64G SSD, and 1 FE 3 BEs deployment.
    - Select the user's common configuration test to reduce the cost of user selection and evaluation, but the entire test process will not consume so many hardware resources.
    - Apache Doris 0.15 RC04 failed to execute Q14 in the TPC-H test, unable to complete the query.

## 6. Environmental Preparation

Please refer to the [official document](../install/standard-deployment.md) to install and deploy Doris to obtain a normal running Doris cluster (at least 1 FE 1 BE, 1 FE 3 BE is recommended).

## 7. Data Preparation

### 7.1 Download and Install TPC-H Data Generation Tool

Execute the following script to download and compile the [tpch-tools](https://github.com/apache/doris/tree/master/tools/tpch-tools) tool.

```shell
sh build-tpch-dbgen.sh
```

After successful installation, the `dbgen` binary will be generated under the `TPC-H_Tools_v3.0.0/` directory.

### 7.2 Generating the TPC-H Test Set

Execute the following script to generate the TPC-H dataset:

```shell
sh gen-tpch-data.sh
```

> Note 1: Check the script help via `sh gen-tpch-data.sh -h`.
>
> Note 2: The data will be generated under the `tpch-data/` directory with the suffix `.tbl`. The total file size is about 100GB and may need a few minutes to an hour to generate.
>
> Note 3: A standard test data set of 100G is generated by default.

### 7.3 Create Table

#### 7.3.1 Prepare the `doris-cluster.conf` File

Before import the script, you need to write the FE’s ip port and other information in the `doris-cluster.conf` file.

The file is located under `${DORIS_HOME}/tools/tpch-tools/conf/` .

The content of the file includes FE's ip, HTTP port, user name, password and the DB name of the data to be imported:

```shell
# Any of FE host
export FE_HOST='127.0.0.1'
# http_port in fe.conf
export FE_HTTP_PORT=8030
# query_port in fe.conf
export FE_QUERY_PORT=9030
# Doris username
export USER='root'
# Doris password
export PASSWORD=''
# The database where TPC-H tables located
export DB='tpch1'
```

#### Execute the Following Script to Generate and Create TPC-H Table

```shell
sh create-tpch-tables.sh
```
Or copy the table creation statement in [create-tpch-tables.sql](https://github.com/apache/doris/tree/master/tools/tpch-tools/ddl/create-tpch-tables.sql) and excute it in Doris.


### 7.4 Import Data

Please perform data import with the following command:

```shell
sh ./load-tpch-data.sh
```

### 7.5 Check Imported Data

Execute the following SQL statement to check that the imported data is consistent with the above data.

```sql
select count(*)  from  lineitem;
select count(*)  from  orders;
select count(*)  from  partsupp;
select count(*)  from  part;
select count(*)  from  customer;
select count(*)  from  supplier;
select count(*)  from  nation;
select count(*)  from  region;
select count(*)  from  revenue0;
```

### 7.6 Query Test

#### 7.6.1 Executing Query Scripts

Execute the above test SQL or execute the following command

```
./run-tpch-queries.sh
```

>Notice:
>
>1. At present, the query optimizer and statistics functions of Doris are not so perfect, so we rewrite some queries in TPC-H to adapt to the execution framework of Doris, but it does not affect the correctness of the results
>
>2. Doris' new query optimizer will be released in future versions
>3. Set `set mem_exec_limit=8G` before executing the query

#### 7.6.2 Single SQL Execution

The following is the SQL statement used in the test, you can also get the latest SQL from the code base.

```SQL
--Q1
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=8, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=false, enable_projection=false) */
    l_returnflag,
    l_linestatus,
    sum(l_quantity) as sum_qty,
    sum(l_extendedprice) as sum_base_price,
    sum(l_extendedprice * (1 - l_discount)) as sum_disc_price,
    sum(l_extendedprice * (1 - l_discount) * (1 + l_tax)) as sum_charge,
    avg(l_quantity) as avg_qty,
    avg(l_extendedprice) as avg_price,
    avg(l_discount) as avg_disc,
    count(*) as count_order
from
    lineitem
where
    l_shipdate <= date '1998-12-01' - interval '90' day
group by
    l_returnflag,
    l_linestatus
order by
    l_returnflag,
    l_linestatus;

--Q2
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=1, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=false, enable_projection=true) */
    s_acctbal,
    s_name,
    n_name,
    p_partkey,
    p_mfgr,
    s_address,
    s_phone,
    s_comment
from
    partsupp join
    (
        select
            ps_partkey as a_partkey,
            min(ps_supplycost) as a_min
        from
            partsupp,
            part,
            supplier,
            nation,
            region
        where
            p_partkey = ps_partkey
            and s_suppkey = ps_suppkey
            and s_nationkey = n_nationkey
            and n_regionkey = r_regionkey
            and r_name = 'EUROPE'
            and p_size = 15
            and p_type like '%BRASS'
        group by a_partkey
    ) A on ps_partkey = a_partkey and ps_supplycost=a_min ,
    part,
    supplier,
    nation,
    region
where
    p_partkey = ps_partkey
    and s_suppkey = ps_suppkey
    and p_size = 15
    and p_type like '%BRASS'
    and s_nationkey = n_nationkey
    and n_regionkey = r_regionkey
    and r_name = 'EUROPE'

order by
    s_acctbal desc,
    n_name,
    s_name,
    p_partkey
limit 100;

--Q3
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=8, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=false, enable_projection=true, runtime_filter_wait_time_ms=10000) */
    l_orderkey,
    sum(l_extendedprice * (1 - l_discount)) as revenue,
    o_orderdate,
    o_shippriority
from
    (
        select l_orderkey, l_extendedprice, l_discount, o_orderdate, o_shippriority, o_custkey from
        lineitem join orders
        where l_orderkey = o_orderkey
        and o_orderdate < date '1995-03-15'
        and l_shipdate > date '1995-03-15'
    ) t1 join customer c 
    on c.c_custkey = t1.o_custkey
    where c_mktsegment = 'BUILDING'
group by
    l_orderkey,
    o_orderdate,
    o_shippriority
order by
    revenue desc,
    o_orderdate
limit 10;

--Q4
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=4, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=false, enable_projection=true) */
    o_orderpriority,
    count(*) as order_count
from
    (
        select
            *
        from
            lineitem
        where l_commitdate < l_receiptdate
    ) t1
    right semi join orders
    on t1.l_orderkey = o_orderkey
where
    o_orderdate >= date '1993-07-01'
    and o_orderdate < date '1993-07-01' + interval '3' month
group by
    o_orderpriority
order by
    o_orderpriority;

--Q5
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=8, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=false, enable_projection=true) */
    n_name,
    sum(l_extendedprice * (1 - l_discount)) as revenue
from
    customer,
    orders,
    lineitem,
    supplier,
    nation,
    region
where
    c_custkey = o_custkey
    and l_orderkey = o_orderkey
    and l_suppkey = s_suppkey
    and c_nationkey = s_nationkey
    and s_nationkey = n_nationkey
    and n_regionkey = r_regionkey
    and r_name = 'ASIA'
    and o_orderdate >= date '1994-01-01'
    and o_orderdate < date '1994-01-01' + interval '1' year
group by
    n_name
order by
    revenue desc;

--Q6
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=1, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=false, enable_projection=true) */
    sum(l_extendedprice * l_discount) as revenue
from
    lineitem
where
    l_shipdate >= date '1994-01-01'
    and l_shipdate < date '1994-01-01' + interval '1' year
    and l_discount between .06 - 0.01 and .06 + 0.01
    and l_quantity < 24;

--Q7
select /*+SET_VAR(exec_mem_limit=458589934592, parallel_fragment_exec_instance_num=2, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=false, enable_projection=true) */
    supp_nation,
    cust_nation,
    l_year,
    sum(volume) as revenue
from
    (
        select
            n1.n_name as supp_nation,
            n2.n_name as cust_nation,
            extract(year from l_shipdate) as l_year,
            l_extendedprice * (1 - l_discount) as volume
        from
            supplier,
            lineitem,
            orders,
            customer,
            nation n1,
            nation n2
        where
            s_suppkey = l_suppkey
            and o_orderkey = l_orderkey
            and c_custkey = o_custkey
            and s_nationkey = n1.n_nationkey
            and c_nationkey = n2.n_nationkey
            and (
                (n1.n_name = 'FRANCE' and n2.n_name = 'GERMANY')
                or (n1.n_name = 'GERMANY' and n2.n_name = 'FRANCE')
            )
            and l_shipdate between date '1995-01-01' and date '1996-12-31'
    ) as shipping
group by
    supp_nation,
    cust_nation,
    l_year
order by
    supp_nation,
    cust_nation,
    l_year;

--Q8

select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=8, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=false, enable_projection=true) */
    o_year,
    sum(case
        when nation = 'BRAZIL' then volume
        else 0
    end) / sum(volume) as mkt_share
from
    (
        select
            extract(year from o_orderdate) as o_year,
            l_extendedprice * (1 - l_discount) as volume,
            n2.n_name as nation
        from
            lineitem,
            orders,
            customer,
            supplier,
            part,
            nation n1,
            nation n2,
            region
        where
            p_partkey = l_partkey
            and s_suppkey = l_suppkey
            and l_orderkey = o_orderkey
            and o_custkey = c_custkey
            and c_nationkey = n1.n_nationkey
            and n1.n_regionkey = r_regionkey
            and r_name = 'AMERICA'
            and s_nationkey = n2.n_nationkey
            and o_orderdate between date '1995-01-01' and date '1996-12-31'
            and p_type = 'ECONOMY ANODIZED STEEL'
    ) as all_nations
group by
    o_year
order by
    o_year;

--Q9
select/*+SET_VAR(exec_mem_limit=37179869184, parallel_fragment_exec_instance_num=2, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=false, enable_projection=true, enable_remove_no_conjuncts_runtime_filter_policy=true, runtime_filter_wait_time_ms=100000) */
    nation,
    o_year,
    sum(amount) as sum_profit
from
    (
        select
            n_name as nation,
            extract(year from o_orderdate) as o_year,
            l_extendedprice * (1 - l_discount) - ps_supplycost * l_quantity as amount
        from
            lineitem join orders on o_orderkey = l_orderkey
            join[shuffle] part on p_partkey = l_partkey
            join[shuffle] partsupp on ps_partkey = l_partkey
            join[shuffle] supplier on s_suppkey = l_suppkey
            join[broadcast] nation on s_nationkey = n_nationkey
        where
            ps_suppkey = l_suppkey and 
            p_name like '%green%'
    ) as profit
group by
    nation,
    o_year
order by
    nation,
    o_year desc;

--Q10

select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=4, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=false, enable_projection=true) */
    c_custkey,
    c_name,
    sum(t1.l_extendedprice * (1 - t1.l_discount)) as revenue,
    c_acctbal,
    n_name,
    c_address,
    c_phone,
    c_comment
from
    customer,
    (
        select o_custkey,l_extendedprice,l_discount from lineitem, orders
        where l_orderkey = o_orderkey
        and o_orderdate >= date '1993-10-01'
        and o_orderdate < date '1993-10-01' + interval '3' month
        and l_returnflag = 'R'
    ) t1,
    nation
where
    c_custkey = t1.o_custkey
    and c_nationkey = n_nationkey
group by
    c_custkey,
    c_name,
    c_acctbal,
    c_phone,
    n_name,
    c_address,
    c_comment
order by
    revenue desc
limit 20;

--Q11
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=2, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=true, enable_projection=true) */
    ps_partkey,
    sum(ps_supplycost * ps_availqty) as value
from
    partsupp,
    (
    select s_suppkey
    from supplier, nation
    where s_nationkey = n_nationkey and n_name = 'GERMANY'
    ) B
where
    ps_suppkey = B.s_suppkey
group by
    ps_partkey having
        sum(ps_supplycost * ps_availqty) > (
            select
                sum(ps_supplycost * ps_availqty) * 0.000002
            from
                partsupp,
                (select s_suppkey
                 from supplier, nation
                 where s_nationkey = n_nationkey and n_name = 'GERMANY'
                ) A
            where
                ps_suppkey = A.s_suppkey
        )
order by
    value desc;

--Q12

select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=2, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=true, enable_projection=true) */
    l_shipmode,
    sum(case
        when o_orderpriority = '1-URGENT'
            or o_orderpriority = '2-HIGH'
            then 1
        else 0
    end) as high_line_count,
    sum(case
        when o_orderpriority <> '1-URGENT'
            and o_orderpriority <> '2-HIGH'
            then 1
        else 0
    end) as low_line_count
from
    orders,
    lineitem
where
    o_orderkey = l_orderkey
    and l_shipmode in ('MAIL', 'SHIP')
    and l_commitdate < l_receiptdate
    and l_shipdate < l_commitdate
    and l_receiptdate >= date '1994-01-01'
    and l_receiptdate < date '1994-01-01' + interval '1' year
group by
    l_shipmode
order by
    l_shipmode;

--Q13
select /*+SET_VAR(exec_mem_limit=45899345920, parallel_fragment_exec_instance_num=16, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=true, enable_projection=true) */
    c_count,
    count(*) as custdist
from
    (
        select
            c_custkey,
            count(o_orderkey) as c_count
        from
            orders right outer join customer on
                c_custkey = o_custkey
                and o_comment not like '%special%requests%'
        group by
            c_custkey
    ) as c_orders
group by
    c_count
order by
    custdist desc,
    c_count desc;

--Q14

select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=8, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=true, enable_projection=true, runtime_filter_mode=OFF) */
    100.00 * sum(case
        when p_type like 'PROMO%'
            then l_extendedprice * (1 - l_discount)
        else 0
    end) / sum(l_extendedprice * (1 - l_discount)) as promo_revenue
from
    part,
    lineitem
where
    l_partkey = p_partkey
    and l_shipdate >= date '1995-09-01'
    and l_shipdate < date '1995-09-01' + interval '1' month;

--Q15
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=8, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=true, enable_projection=true) */
    s_suppkey,
    s_name,
    s_address,
    s_phone,
    total_revenue
from
    supplier,
    revenue0
where
    s_suppkey = supplier_no
    and total_revenue = (
        select
            max(total_revenue)
        from
            revenue0
    )
order by
    s_suppkey;

--Q16
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=8, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=true, enable_projection=true) */
    p_brand,
    p_type,
    p_size,
    count(distinct ps_suppkey) as supplier_cnt
from
    partsupp,
    part
where
    p_partkey = ps_partkey
    and p_brand <> 'Brand#45'
    and p_type not like 'MEDIUM POLISHED%'
    and p_size in (49, 14, 23, 45, 19, 3, 36, 9)
    and ps_suppkey not in (
        select
            s_suppkey
        from
            supplier
        where
            s_comment like '%Customer%Complaints%'
    )
group by
    p_brand,
    p_type,
    p_size
order by
    supplier_cnt desc,
    p_brand,
    p_type,
    p_size;

--Q17
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=1, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=true, enable_projection=true) */
    sum(l_extendedprice) / 7.0 as avg_yearly
from
    lineitem join [broadcast]
    part p1 on p1.p_partkey = l_partkey
where
    p1.p_brand = 'Brand#23'
    and p1.p_container = 'MED BOX'
    and l_quantity < (
        select
            0.2 * avg(l_quantity)
        from
            lineitem join [broadcast]
            part p2 on p2.p_partkey = l_partkey
        where
            l_partkey = p1.p_partkey
            and p2.p_brand = 'Brand#23'
            and p2.p_container = 'MED BOX'
    );

--Q18

select /*+SET_VAR(exec_mem_limit=45899345920, parallel_fragment_exec_instance_num=4, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=true, enable_projection=true) */
    c_name,
    c_custkey,
    t3.o_orderkey,
    t3.o_orderdate,
    t3.o_totalprice,
    sum(t3.l_quantity)
from
customer join
(
  select * from
  lineitem join
  (
    select * from
    orders left semi join
    (
      select
          l_orderkey
      from
          lineitem
      group by
          l_orderkey having sum(l_quantity) > 300
    ) t1
    on o_orderkey = t1.l_orderkey
  ) t2
  on t2.o_orderkey = l_orderkey
) t3
on c_custkey = t3.o_custkey
group by
    c_name,
    c_custkey,
    t3.o_orderkey,
    t3.o_orderdate,
    t3.o_totalprice
order by
    t3.o_totalprice desc,
    t3.o_orderdate
limit 100;

--Q19

select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=2, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=false, enable_cost_based_join_reorder=false, enable_projection=true) */
    sum(l_extendedprice* (1 - l_discount)) as revenue
from
    lineitem,
    part
where
    (
        p_partkey = l_partkey
        and p_brand = 'Brand#12'
        and p_container in ('SM CASE', 'SM BOX', 'SM PACK', 'SM PKG')
        and l_quantity >= 1 and l_quantity <= 1 + 10
        and p_size between 1 and 5
        and l_shipmode in ('AIR', 'AIR REG')
        and l_shipinstruct = 'DELIVER IN PERSON'
    )
    or
    (
        p_partkey = l_partkey
        and p_brand = 'Brand#23'
        and p_container in ('MED BAG', 'MED BOX', 'MED PKG', 'MED PACK')
        and l_quantity >= 10 and l_quantity <= 10 + 10
        and p_size between 1 and 10
        and l_shipmode in ('AIR', 'AIR REG')
        and l_shipinstruct = 'DELIVER IN PERSON'
    )
    or
    (
        p_partkey = l_partkey
        and p_brand = 'Brand#34'
        and p_container in ('LG CASE', 'LG BOX', 'LG PACK', 'LG PKG')
        and l_quantity >= 20 and l_quantity <= 20 + 10
        and p_size between 1 and 15
        and l_shipmode in ('AIR', 'AIR REG')
        and l_shipinstruct = 'DELIVER IN PERSON'
    );

--Q20
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=2, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=true, enable_projection=true, runtime_bloom_filter_size=551943) */
s_name, s_address from
supplier left semi join
(
    select * from
    (
        select l_partkey,l_suppkey, 0.5 * sum(l_quantity) as l_q
        from lineitem
        where l_shipdate >= date '1994-01-01'
            and l_shipdate < date '1994-01-01' + interval '1' year
        group by l_partkey,l_suppkey
    ) t2 join
    (
        select ps_partkey, ps_suppkey, ps_availqty
        from partsupp left semi join part
        on ps_partkey = p_partkey and p_name like 'forest%'
    ) t1
    on t2.l_partkey = t1.ps_partkey and t2.l_suppkey = t1.ps_suppkey
    and t1.ps_availqty > t2.l_q
) t3
on s_suppkey = t3.ps_suppkey
join nation
where s_nationkey = n_nationkey
    and n_name = 'CANADA'
order by s_name;

--Q21
select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=4, enable_vectorized_engine=true, batch_size=4096, disable_join_reorder=true, enable_cost_based_join_reorder=true, enable_projection=true) */
s_name, count(*) as numwait
from
  lineitem l2 right semi join
  (
    select * from
    lineitem l3 right anti join
    (
      select * from
      orders join lineitem l1 on l1.l_orderkey = o_orderkey and o_orderstatus = 'F'
      join
      (
        select * from
        supplier join nation
        where s_nationkey = n_nationkey
          and n_name = 'SAUDI ARABIA'
      ) t1
      where t1.s_suppkey = l1.l_suppkey and l1.l_receiptdate > l1.l_commitdate
    ) t2
    on l3.l_orderkey = t2.l_orderkey and l3.l_suppkey <> t2.l_suppkey  and l3.l_receiptdate > l3.l_commitdate
  ) t3
  on l2.l_orderkey = t3.l_orderkey and l2.l_suppkey <> t3.l_suppkey 

group by
    t3.s_name
order by
    numwait desc,
    t3.s_name
limit 100;

--Q22

with tmp as (select
                    avg(c_acctbal) as av
                from
                    customer
                where
                    c_acctbal > 0.00
                    and substring(c_phone, 1, 2) in
                        ('13', '31', '23', '29', '30', '18', '17'))

select /*+SET_VAR(exec_mem_limit=8589934592, parallel_fragment_exec_instance_num=4,runtime_bloom_filter_size=4194304) */
    cntrycode,
    count(*) as numcust,
    sum(c_acctbal) as totacctbal
from
    (
	select
            substring(c_phone, 1, 2) as cntrycode,
            c_acctbal
        from
             orders right anti join customer c on  o_custkey = c.c_custkey join tmp on c.c_acctbal > tmp.av
        where
            substring(c_phone, 1, 2) in
                ('13', '31', '23', '29', '30', '18', '17')
    ) as custsale
group by
    cntrycode
order by
    cntrycode;
```