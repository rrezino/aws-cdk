## The CDK Construct Library for AWS Glue
This module is part of the [AWS Cloud Development Kit](https://github.com/awslabs/aws-cdk) project.

### Database

A `Database` is a logical grouping of `Tables` in the Glue Catalog.

```ts
new glue.Database(stack, 'MyDatabase', {
  databaseName: 'my_database'
});
```

By default, a S3 bucket is created where the Database is stored under  `s3://<bucket-name>/`, but you can manually specify another location:

```ts
new glue.Database(stack, 'MyDatabase', {
  databaseName: 'my_database',
  locationUri: 's3://explicit-bucket/some-path/'
});
```

### Table

A Glue table describes the structure (column names and types), location of data (S3 objects with a common prefix in a S3 bucket) and format of the files (Json, Avro, Parquet, etc.):

```ts
new glue.Table(stack, 'MyTable', {
  database: myDatabase,
  tableName: 'my_table',
  columns: [{
    name: 'col1',
    type: glue.Schema.string
  }]
  storageType: glue.StorageType.Json
});
```

By default, a S3 bucket will created to store the table's data but you can manually pass the `bucket` and `prefix`:

```ts
new glue.Table(stack, 'MyTable', {
  bucket: myBucket,
  prefix: 'my-table/'
  ...
});
```

#### Partitions

To improve query performance, a table can specify `partitionKeys` on which data is stored and queried separately. For example, you might partition a table by `year` and `month` to optimize queries based on a time window:

```ts
new glue.Table(stack, 'MyTable', {
  database: myDatabase,
  tableName: 'my_table',
  columns: [{
    name: 'col1',
    type: glue.Schema.string
  }],
  partitionKeys: [{
    name: 'year',
    type: glue.Schema.smallint
  }, {
    name: 'month',
    type: glue.Schema.smallint
  }],
  storageType: glue.StorageType.Json
});
```

### Types

A table's schema is a collection of columns, each of which have a `name` and a `type`. Types are recursive structures, consisting of primitive and complex types:

#### Primitive

Numeric:
* `bigint`
* `float`
* `integer`
* `smallint`
* `tinyint`

Date and Time:
* `date`
* `timestamp`

String Types:

* `string`
* `decimal`
* `char`
* `varchar`

Misc:
* `boolean`
* `binary`

#### Complex

* `array` - array of some other type.
* `map` - map of some primitive key type to any value type.
* `struct` - nested structure containing individually named and typed columns.