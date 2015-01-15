MongoDB course (M101P)
- schemaless, document-oriented
- no joins, no transactions
- the mongo client has a javascript like interface, plus help :)
- document limit of 16 MB

Access (client):
- db.<name>.insert({json_document})
- db.<name>.find() /* more elements */ or .findOne() /* one element */

Python bottle:
- bottle page templates .tpl are in a folder called views/
- @bottle.post @bottle.route
