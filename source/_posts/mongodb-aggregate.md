---
title: mongodb-aggregate
date: 2017-08-10 13:33:52
tags: [mongodb]
---
简单罗列了下mongodb的管道操作符
<!--more-->
## Pipeline Aggregation Stages
```
db.collection.aggregate( [ { <stage> }, ... ] )
```
+ $collStats
+ $project
+ $match
+ $redact
+ $limit
+ $skip
+ $unwind
+ $group
+ $sample
+ $sort
+ $geoNear
+ $lookup
+ $out
+ $indexStats
+ $facet
+ $bucket
+ $bucketAuto
+ $sortByCount
+ $addFields
+ $replaceRoot
+ $count
+ $graphLookup

## Expression Operators
```
{ <operator>: [ <argument1>, <argument2> ... ] }
# or
{ <operator>: <argument> }
```

### Boolean Operators
+ $and
+ $or
+ $not

### Set Operators

+ $setEquals
+ $setIntersection
+ $setUnion
+ $setDifference
+ $setIsSubset
+ $anyElementTrue
+ $allElementsTrue

### Comparison Operators

+ $cmp
+ $eq
+ $gt
+ $gte
+ $lt
+ $lte
+ $ne

### Arithmetic Operators

+ $abs
+ $add
+ $ceil
+ $divide
+ $exp
+ $floor
+ $ln
+ $log
+ $log10
+ $mod
+ $multiply
+ $pow
+ $sqrt
+ $subtract
+ $trunc

### String Operators

+ $concat
+ $indexOfBytes
+ $indexOfCP
+ $split
+ $strLenBytes
+ $strLenCP
+ $strcasecmp
+ $substr
+ $substrBytes
+ $substrCP
+ $toLower
+ $toUpper

### Text Search Operators

+ $meta

### Array Operators

+ $arrayElemAt
+ $arrayToObject
+ $concatArrays
+ $filter
+ $in
+ $indexOfArray
+ $isArray
+ $map
+ $objectToArray
+ $range
+ $reduce
+ $reverseArray
+ $size
+ $slice
+ $zip

### Variable Operators

+ $let

### Literal Operators

+ $literal

### Date Operators

+ $dayOfYear
+ $dayOfMonth
+ $dayOfWeek
+ $year
+ $month
+ $week
+ $hour
+ $minute
+ $second
+ $millisecond
+ $dateToString
+ $isoDayOfWeek
+ $isoWeek
+ $isoWeekYear

### Conditional Expressions

+ $cond
+ $ifNull
+ $switch

### Data Type Expressions

+ $type

## Group Accumulator Operators

+ $sum
+ $avg
+ $first
+ $last
+ $max
+ $min
+ $push
+ $addToSet
+ $stdDevPop
+ $stdDevSamp
+ $push
+ $push