---
sidebar_position: 2
---

# $add

Adds numbers together or adds numbers and a date. If one of the arguments is a date, `$add` treats the other arguments as milliseconds to add to the date.

The `$add` expression has the following syntax:

    { $add: [ <expression1>, <expression2>, ... ] }

The arguments can be any valid expression as long as they resolve to either all numbers or to numbers and a date. For more information on expressions, see Expressions.

## Examples

The following examples use a sales collection with the following documents:

    { "_id" : 1, "item" : "abc", "price" : 10, "fee" : 2, date: ISODate("2014-03-01T08:00:00Z") }
    { "_id" : 2, "item" : "jkl", "price" : 20, "fee" : 1, date: ISODate("2014-03-01T09:00:00Z") }
    { "_id" : 3, "item" : "xyz", "price" : 5,  "fee" : 0, date: ISODate("2014-03-15T09:00:00Z") }

### Add Numbers

The following aggregation uses the `$add` expression in the $project pipeline to calculate the total cost:

    db.sales.aggregate([
      { $project: { item: 1, total: { $add: [ "$price", "$fee" ] } } }
    ])

The operation returns the following results:

    { "_id" : 1, "item" : "abc", "total" : 12 }
    { "_id" : 2, "item" : "jkl", "total" : 21 }
    { "_id" : 3, "item" : "xyz", "total" : 5 }

### Perform Addition on a Date

The following aggregation uses the `$add` expression to compute the billing_date by adding 3*24*60\*60000 milliseconds (i.e. 3 days) to the date field :

    db.sales.aggregate([
      { $project: { item: 1, billing_date: { $add: [ "$date", 3*24*60*60000 ] } } }
    ])

The operation returns the following results:

    { "_id" : 1, "item" : "abc", "billing_date" : ISODate("2014-03-04T08:00:00Z") }
    { "_id" : 2, "item" : "jkl", "billing_date" : ISODate("2014-03-04T09:00:00Z") }
    { "_id" : 3, "item" : "xyz", "billing_date" : ISODate("2014-03-18T09:00:00Z") }
