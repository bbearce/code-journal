# CouchDB

## Good delete script

> Source [moduliertersingvogel](https://moduliertersingvogel.de/2018/03/15/bulk-delete-in-couchdb/)

> Supplementary Material [tutorialspoint](https://www.tutorialspoint.com/couchdb/couchdb_deleting_a_document.htm)

*Keep in mind that your view determines how you structure the ```object``` in todelete.append(```object```)*
```python
#!/usr/bin/env python3
# coding: utf-8
import json
import requests
import sys
 
database=sys.argv[1]
if len(database)==0:
    sys.exit(1)


# You can use views 
r=requests.get("http://localhost:5984/{}/_all_docs".format(database))
rows=json.loads(r.text)['rows']
print(len(rows))
 
todelete=[]
for doc in rows:
    # original
    # todelete.append({"_deleted": True, "_id": doc["id"], "_rev": doc["value"]["rev"]})
    todelete.append({"_deleted": True, "_id": doc["id"], "_rev": doc["value"][0]})
 
r=requests.post("http://localhost:5984/{}/_bulk_docs".format(database), json={"docs": todelete})
print(r.status_code)
```

