# MongoDB Cheat Sheet

## Show All Databases

```
show dbs
```

## Show Current Database

```
db
```

## Create Or Switch Database

```
use acme
```

## Drop

```
db.dropDatabase()
```

## Create Collection

```
db.createCollection('posts')
```

## Show Collections

```
show collections
```

## Insert Row

```
db.posts.insert({
  title: 'Post One',
  body: 'Body of post one',
  category: 'News',
  tags: ['news', 'events'],
  user: {
    name: 'John Doe',
    status: 'author'
  },
  date: Date()
})
```

## Insert Multiple Rows

```
db.posts.insertMany([
  {
    title: 'Post Two',
    body: 'Body of post two',
    category: 'Technology',
    date: Date()
  },
  {
    title: 'Post Three',
    body: 'Body of post three',
    category: 'News',
    date: Date()
  },
  {
    title: 'Post Four',
    body: 'Body of post three',
    category: 'Entertainment',
    date: Date()
  }
])
```

## Get All Rows

```
db.posts.find()
```

## Get All Rows Formatted

```
db.find().pretty()
```

## Find Rows

```
db.posts.find({ category: 'News' })
```

## Sort Rows

```
# asc
db.posts.find().sort({ title: 1 }).pretty()
# desc
db.posts.find().sort({ title: -1 }).pretty()
```

## Count Rows

```
db.posts.find().count()
db.posts.find({ category: 'news' }).count()
```

## Limit Rows

```
db.posts.find().limit(2).pretty()
```

## Chaining

```
db.posts.find().limit(2).sort({ title: 1 }).pretty()
```

## Foreach

```
db.posts.find().forEach(function(doc) {
  print("Blog Post: " + doc.title)
})
```

## Find One Row

```
db.posts.findOne({ category: 'News' })
```

## Find Specific Fields

```
db.posts.find({ title: 'Post One' }, {
  title: 1,
  author: 1
})
```

## Update Row

```
db.posts.update({ title: 'Post Two' },
{
  title: 'Post Two',
  body: 'New body for post 2',
  date: Date()
},
{
  upsert: true
})
```

## Update Specific Field

```
db.posts.update({ title: 'Post Two' },
{
  $set: {
    body: 'Body for post 2',
    category: 'Technology'
  }
})
```

## Increment Field (\$inc)

```
db.posts.update({ title: 'Post Two' },
{
  $inc: {
    likes: 5
  }
})
```

## Rename Field

```
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    likes: 'views'
  }
})
```

## Delete Row

```
db.posts.remove({ title: 'Post Four' })
```

## Sub-Documents

```
db.posts.update({ title: 'Post One' },
{
  $set: {
    comments: [
      {
        body: 'Comment One',
        user: 'Mary Williams',
        date: Date()
      },
      {
        body: 'Comment Two',
        user: 'Harry White',
        date: Date()
      }
    ]
  }
})
```

## Find By Element in Array (\$elemMatch)

```
db.posts.find({
  comments: {
     $elemMatch: {
       user: 'Mary Williams'
       }
    }
  }
)
```

## Add Index

```
db.posts.createIndex({ title: 'text' })
```

## Text Search

```
db.posts.find({
  $text: {
    $search: "\"Post O\""
    }
})
```

## Greater & Less Than

```
db.posts.find({ views: { $gt: 2 } })
db.posts.find({ views: { $gte: 7 } })
db.posts.find({ views: { $lt: 7 } })
db.posts.find({ views: { $lte: 7 } })
```
Result
```
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.lista.find({"numbers": {"$elemMatch": {"$gt":100, "$lt":1000 }}}, {"_id": false }).pretty()
{ "numbers" : [ 100, 100, 200, 400, 900 ] }
{ "numbers" : [ 100, 100, 200, 400, 900 ] }
{ "numbers" : [ 100, 100, 200, 400, 900 ] }
{ "numbers" : [ 100, 100, 200, 400, 900 ] }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```
```
db.transactions.find().sort({"transaction_count": 1}).pretty()
```
Show specific results
```
db.transactions.find().sort({"transaction_count": 1}).limit(2).pretty()
```
skip some results
```
db.transactions.find().sort({"transaction_count": 1}).skip(2).pretty()
```

Aggregation Framework
```
db.inventory.insertMany([
...    { item: "journal", qty: 25, tags: ["blank", "red"], size: { h: 14, w: 21, uom: "cm" } },
...    { item: "mat", qty: 85, tags: ["gray"], size: { h: 27.9, w: 35.5, uom: "cm" } },
...    { item: "mousepad", qty: 25, tags: ["gel", "blue"], size: { h: 19, w: 22.85, uom: "cm" } }
... ])


db.inventory.insertMany([
...    { item: "journal", qty: 25, tags: ["blank", "red"], size: { h: 14, w: 21, uom: "cm" } },
...    { item: "mat", qty: 85, tags: ["gray"], size: { h: 27.9, w: 35.5, uom: "cm" } },
...    { item: "mousepad", qty: 25, tags: ["gel", "blue"], size: { h: 19, w: 22.85, uom: "cm" } }
... ])

```
## Aggregate 
Aggregation operations process data records and return computed results. Aggregation operations group values from multiple documents together, and can perform a variety of operations on the grouped data to return a single result. 
```
db.inventory.aggregate([{"$group":{"_id":"$item" }}])
{ "_id" : "mousepad" }
{ "_id" : "journal" }
{ "_id" : "mat" }
```
Using Accumulators 
```
db.inventory.aggregate([{"$group":{"_id":"$item", "total": {"$sum":1} }}])
```
Result
```
{ "_id" : "mousepad", "total" : 2 }
{ "_id" : "mat", "total" : 2 }
{ "_id" : "journal", "total" : 2 }
```
```
use sample_airbnb
```
```
db.listingsAndReviews.aggregate( [{"$group":{"_id":"$property_type","total":{"$sum":1}}},{"$sort":{"total":-1}},{$limit:5}])
```
Result
```
{ "_id" : "Cabin", "total" : 15 }
{ "_id" : "Villa", "total" : 32 }
{ "_id" : "Hostel", "total" : 34 }
{ "_id" : "Camper/RV", "total" : 2 }
{ "_id" : "Boutique hotel", "total" : 53 }
{ "_id" : "Apartment", "total" : 3626 }
{ "_id" : "Bed and breakfast", "total" : 69 }
{ "_id" : "Chalet", "total" : 2 }
{ "_id" : "House", "total" : 606 }
{ "_id" : "Guest suite", "total" : 81 }
{ "_id" : "Casa particular (Cuba)", "total" : 9 }
{ "_id" : "Campsite", "total" : 1 }
{ "_id" : "Castle", "total" : 1 }
{ "_id" : "Nature lodge", "total" : 2 }
```
```
db.listingsAndReviews.aggregate( [{"$group":{"_id":"$property_type","total":{"$sum":1},"bedroom_total":{"$sum":"$bedrooms" }  }},{"$sort":{"total":-1}},{$limit:5}])
```
Result
```
{ "_id" : "Apartment", "total" : 3626, "bedroom_total" : 4865 }
{ "_id" : "House", "total" : 606, "bedroom_total" : 1234 }
{ "_id" : "Condominium", "total" : 399, "bedroom_total" : 529 }
{ "_id" : "Serviced apartment", "total" : 185, "bedroom_total" : 245 }
{ "_id" : "Loft", "total" : 142, "bedroom_total" : 142 }
```
```
db.listingsAndReviews.aggregate([{ "$match":{"property_type":"House"}}]).pretty()
```

```
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.listingsAndReviews.aggregate([{ "$match":{"property_type":"House"}},{"$group":{"_id":"$bedrooms","bed_count":{"$sum":"$beds"}}},{"$sort": {"bed_count":1}}]).pretty()
```
```
{ "_id" : null, "bed_count" : 0 }
{ "_id" : 9, "bed_count" : 15 }
{ "_id" : 7, "bed_count" : 16 }
{ "_id" : 0, "bed_count" : 26 }
{ "_id" : 6, "bed_count" : 44 }
{ "_id" : 5, "bed_count" : 133 }
{ "_id" : 2, "bed_count" : 245 }
{ "_id" : 3, "bed_count" : 365 }
{ "_id" : 1, "bed_count" : 416 }
{ "_id" : 4, "bed_count" : 434 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```
```
 db.listingsAndReviews.aggregate([{ "$match":{"address.country":"Mexico"}},{"$group": {"_id": "$maximum_nights", "bed_count": {"$sum": $beds"}}},{"$sort": {"numNoches":1}}])
 ```
 ```
 db.listingsAndReviews.aggregate([{
 "$match":{"address.country":"Spain"}},{"$group":{"_id":"$maximum_nights","bed_count":{"$sum":"$beds"}}},{"$sort": {"bed_count":1}}]).pretty()
 ```
 ```
 db.listingsAndReviews.aggregate([{"$match":{"address.country":"Spain"}},{"$group": {"_id":"$review_scores_rating","bed_count":{"$sum":"$beds"}}},{"$sort":{"_id":1}}]).pretty()
```
