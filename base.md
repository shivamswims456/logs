# SQLAlchemy
```
pip install SQLAlchemy
```

## Version check


```
import sqlachemy
sqlalchemy.__version__
```

## Connection establishment


```
from sqlalchemy import create_engine
from urllib.parse import quote

engine = create_engine("postgresql+psycopg2://postgres:%s@localhost/test" % quote("P@$$w0rd"), echo=True)

```


## ORM connection interpretation

```
from sqlachemy import text

with engine.connect() as conn:
    result = conn.execute(text("show database;"))
    print(result.all())

```

* Committing Changes

```
with engine.connect() as conn:
    conn.execute(text("CREATE TABLE some_table (x int, y int)"))
        conn.execute(
            text("INSERT INTO some_table (x, y) VALUES (:x, :y)"),
            [{"x": 1, "y": 1}, {"x": 2, "y": 4}],
        )
        conn.commit()

```

* Begin Once

```
    with engine.begin() as conn:
        conn.execute(
            text("INSERT INTO some_table (x, y) VALUES (:x, :y)"),
            [{"x": 6, "y": 8}, {"x": 9, "y": 10}],
        )
```

* Parameter Inclusion

```
    with engine.connect() as conn:
     result = conn.execute(text("SELECT x, y FROM some_table WHERE y > :y"), {"y": 2})
     
     for row in result:
        print(f"x: {row.x}  y: {row.y}")
        
```

* Multiple Inclusion

```
    with engine.connect() as conn:
        text("INSERT INTO some_table (x, y) VALUES (:x, :y)"),[{"x":11, "y":2}, {"x":13, "y":14}])

        conn.commit()
```


## ORM session interpretation

* Multiple Inclusion

```
    from sqlalchemy.orm import Session

    with Session(engine) as conn:
        result = session.execute(text("INSERT INTO some_table (x, y) VALUES (:x, :y)"),[{"x":11, "y":2}, {"x":13, "y":14}]))

        session.commit()
```

## Tables can be constructed using
* Programmatically using table constructor
* ORM Mapped Classes
* There is also a possibility of loading existing tables using reflection.

Theses methods inturn store the *Tables* mapping in a Metadata (*dict like structure*)

```
from sqlalchemy import MetaData, Table, Column, Integer, String
metadata_obj = MetaData()

user_table = Table(
    "user_account",
    metadata_obj,
    Column("id", Integer, primary_key=True),
    Column("name", String(30)),
    Column("fullname", String(30))
)

# creation logic
# user_table.create(engine)
# for ifexists logic
# user_table.create(engine, checkfirst=True)

#Here the user_table will assign itself to MetaData object.

```


Components of a table.

```
# user_table.c.name will fetch the object the column.
# user_table.c.keys() will list out the column names.

```

#TODO101: https://docs.sqlalchemy.org/en/20/core/metadata.html



## ForeignKeyConstrain

```
address_table = Table("address",
                       metadata_obj,
                       Column("id", Integer, primary_key = True),
                       Column("user_id", ForeignKey("user_account.id"), nullable = False),
                       Column("email_address", string, nullable=False)


MetaData.create_all()
```


