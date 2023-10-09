Input

```json
[
    {
        "data":"{\"address\":{\"street\":\"San Marino\",\"city\":\"Los Angeles\",\"isSingle\":true,\"phone\":98745},\"name\":\"John Wick\"}"
    },
    {
        "data":"{\"address\":{\"street\":\"Secoffee\",\"city\":\"Miami\",\"isSingle\":false,\"phone\":835745},\"name\":\"Paul White\"}"
    }
]
```

Expected Output

```json
[
    {
        "data":{
            "address":{
                "street":"San Marino",
                "city":"Los Angeles",
                "isSingle":true,
                "phone":98745
            },
            "name":"John Wick"
        }
    },
    {
        "data":{
            "address":{
                "street":"Secoffee",
                "city":"Miami",
                "isSingle":false,
                "phone":835745
            },
            "name":"Paul White"
        }
    }
]
```

Script
```groovy
import groovy.json.JsonSlurper
import org.apache.nifi.serialization.SimpleRecordSchema
import org.apache.nifi.serialization.record.MapRecord
import org.apache.nifi.serialization.record.Record

def slurper = new JsonSlurper()
Record result = new MapRecord(new SimpleRecordSchema([]),[:]) // empty record
record.toMap().each { k,v ->
    result.setValue(k,slurper.parseText(v as String)) // adding a field Map<String,String>
}
return result
```
Ref: https://stackoverflow.com/questions/70237395/nifi-converting-json-string-to-valid-json