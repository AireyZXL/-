# 日常项目经验收集
日常项目经验收集

###  一.mysql部分

####  1.1查找表中多余的重复记录，重复记录是根据单个字段（peopleId）来判断

```
SELECT
    *
FROM
    bk_bill_min_data
WHERE
        statistical_time IN (
        SELECT
            statistical_time
        FROM
            bk_bill_min_data
        GROUP BY
            statistical_time
        HAVING
                count(statistical_time) > 1
    );
```
#### 1.2 删除表中多余的重复记录，重复记录是根据单个字段（peopleId）来判断，只留有rowid最小的记录

```
DELETE
FROM
    bk_bill_min_data
WHERE
        statistical_time IN (

       select  a.statistical_time  from
            (SELECT
            statistical_time
        FROM
            bk_bill_min_data
        GROUP BY
            statistical_time
        HAVING
                count(statistical_time) > 1)
            a
    )
  AND statistical_time NOT IN (
      select b.id from
    ( SELECT
        min(id) id
    FROM
        bk_bill_min_data
    GROUP BY
        statistical_time
    HAVING
            count(statistical_time) > 1) b
);


```
