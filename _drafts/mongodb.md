MongoDB course (M101P)
- schemaless, document-oriented
- no joins, no transactions
- the mongo client has a javascript like interface, plus help :)
- API is based on BSON (binary - JSON)
- document limit of 16 MB



Access (client):
- db.<name>.insert({json_document})
- db.<name>.find() /* more elements */
    - curson kept open on server while iterating - with _it_ command - through the documents
- db.<name>.findOne({document}, {fields: true, other_fields: false})
    - one element, we can specify with the extra parameters what is the query, what fields to retrurn
    - _id is returned by default
- query operators {score : { $gt : 95, $lte : 80 } } - greater than, less than or equal

Python bottle:
- bottle page templates .tpl are in a folder called views/
- @bottle.post @bottle.route
