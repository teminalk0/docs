---
editable: false
---

# LOG

_Mathematical functions_

#### Syntax


```
LOG( value, base )
```

#### Description
Returns the logarithm of `value` to base `base`. Returns 'NULL' if the number `value` is less than or equal to 0.

**Argument types:**
- `value` — `Number`
- `base` — `Number`


**Return type**: `Number (decimal)`

#### Examples

```
LOG(1, 2.6) = 0.0
```

```
LOG(1024, 2) = 10.0
```

```
LOG(100, 10) = 2.0
```


#### Data source support

`Materialized Dataset`, `ClickHouse 1.1`, `Microsoft SQL Server 2017 (14.0)`, `MySQL 5.6`, `PostgreSQL 9.3`.