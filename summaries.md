
## Using `duckdb` on the Command Line

We create a temporary table by reading the `zstd`-compressed csv files
created from the log files of the two (main and alternate/initial)
servers. This requires only `duckdb`, and the current directory as the
starting directory.

### Create Table

```sql
create temp table r as select * from read_csv('r*/*.csv.zst');
```

### Count

```sql
select count(*) as n from r;
```

### Count by Month

```sql
select count(*) as n, strftime(date, '%Y-%m') as ym from r group by ym order by ym;
```

### Sub-Select

Adding `where dist='noble'` or `where arch='arm64'` can illustrate
which variants were downloaded when.
