Input

```json
[
    {
        "address":{
            "street":"San Marino",
            "city":"Los Angeles",
            "isSingle":true,
            "phone":98745
        },
        "name":"John Wick"
    },
    {
        "address":{
            "street":"Secoffee",
            "city":"Miami",
            "isSingle":false,
            "phone":835745
        },
        "name":"Paul White"
    }
]

```

Expected Output

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

Script
```groovy
import groovy.json.JsonOutput
import org.apache.nifi.serialization.SimpleRecordSchema;
import org.apache.nifi.serialization.record.MapRecord
import org.apache.nifi.serialization.record.RecordField
import org.apache.nifi.serialization.record.RecordFieldType


def jsonStr = JsonOutput.toJson(record.toMap(true))
if (jsonStr != null) {
	def fields = []
	fields.add(new RecordField("data", RecordFieldType.STRING.getDataType()))
	def schema = new SimpleRecordSchema(fields)

	def derivedMap = [data: jsonStr]
	return new MapRecord(schema, derivedMap)
}
return null
```