# Scala Query AST

The goal of this AST is to sufficiently generalize a querying AST such that it can be used for many platforms.

As it stands now, there is a quick 'n dirty SQL DSL, and a first pass implementation of a simple, high level AST.

The sample below uses the SQL DSL, and is a valid query.

```scala
import com.github.jacoby6000.query.ast._
import com.github.jacoby6000.query.interpreter
import com.github.jacoby6000.query.dsl.sql._

val q = select (
    p"foo" ++ 10 as "woozle",
    `*`
  ) from "bar" leftOuterJoin "baz" on (
    p"bar.id" eeq p"baz.barId"
  ) innerJoin "biz" on (
    p"biz.id" eeq p"bar.bizId"
  ) where (
    p"biz.name" eeq "LightSaber" and (
    p"biz.age" gt 27)
  ) orderBy p"biz.age".desc groupBy p"baz.worth".asc

interpreter.interpretSql(q)
```

The sql output of this would be

```sql
SELECT
    foo + 10 AS woozle,
    *
FROM
    bar
LEFT OUTER JOIN
    baz
        ON bar.id = baz.barId
INNER JOIN
    biz
        ON biz.id = bar.bizId
WHERE
    biz.name = “LightSaber”
    AND  biz.age > 27
ORDER BY
    biz.age DESC
GROUP BY
    baz.worth ASC```





