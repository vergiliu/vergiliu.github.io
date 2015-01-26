MongoDB course (M101P)
- schemaless, document-oriented
- no joins, no transactions
- the mongo client has a javascript like interface, plus help :)
- API is based on BSON (binary - JSON)
- document limit of 16 MB



Access (client):
- db.<name>.insert({json_document})
    - an _id will be created if it's missing
- db.<name>.update( ... , {upsert: true}) - or upsert operation, update a document if it exists, otherwise create one
    - multi: true, otherwise mongo defaults to updating only one document in the collection
    - write operations are not isolated transactions, no guarantees for multi-documents updates, only single documents
- db.<name>.find() /* more elements */
    - curson kept open on server while iterating - with _it_ command - through the documents
    - $regex : regular expression used for find
- db.<name>.remove() - it needs a document as parameter
    - db.<name>.drop() - similar to sql drop operations
- db.<name>.findOne({document}, {fields: true, other_fields: false})
    - one element, we can specify with the extra parameters what is the query, what fields to return
    - _id is returned by default
- query operators {score : { $gt : 95, $lte : 80 } } - greater than, less than or equal, can be applied to text as well
    - $exist, $type - a particular type, $regex - as names implies
    - $or: [array of queries], similar also $and (less used)
    - $in: [subset] of the query out of which some may match; $all: [subset] of the query which must match
    - cursor = db.students.find()
    - db.students.find( {type: "exam"}).sort({score:-1}).skip(50).limit(20) - score reversed, skip, limit
    - $set : {field: value} just add a specific field, leaving all others intact
        - $inc
    - $unset : remove a specific field
    - Array operations: $push, $pop, $pushAll : add an array to the array, $pull : remove an element, $pullAll
- order is always Sort, Skip, Limit
    - i.e. cursor = things.find().skip(3).limit(1).sort('value',pymongo.DESCENDING)


Week3: - schema design

Python bottle:
- bottle page templates .tpl are in a folder called views/
- @bottle.post @bottle.route
