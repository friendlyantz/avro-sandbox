# Action Plan

- [ ] install and study [Avro (Ruby Gem)](https://rubygems.org/gems/avro) `bundle add avro`
- [x] install and study [Avro Turf(Ruby Gem)](https://github.com/dasch/avro_turf) `bundle add avro_turf`
- [x] deserialize Avro binaries encoded by other languages (Ruby <=> Python)
- [ ] AvroTurf - figure out how to decode multiple items from a payload
- [ ] modify schema and play with schema evolution, backward and forward compatibility
- [ ] install protobuf and compare results

# Notes 

## Avro Turf

Decided to ignore Avro and go straight into AvroTurf

### 1. I hardcoded Avro schemas for `person` that includes another schema for `address`

> Ruby AvroTrift allows to store it under multiple files, but it was challanging to read in Python, so ended up storing schemas in 1 file

```json
[
{
    "name": "address",
    "type": "record",
    "fields": [
    {
        "name": "street",
        "type": "string"
    },
    {
        "name": "city",
        "type": "string"
    }
    ]
},
{
    "name": "person",
    "type": "record",
    "fields": [
    {
        "name": "name",
        "type": "string"
    },
    {
        "name": "age",
        "type": "int"
    },
    {
        "name": "address",
        "type": "address"
    }
    ]
}
]
```

Basic data packed will be smth alike this:
```ruby
{
    "name" => "Ruby",
    "age" => 77, 
    "address" => {
        "city" => "Melbourne",
        "street" => "Collins Street"
    }
}
```

### 2. And then experimented in simple avro ruby script to encode/decode data in avro and store binaries under `db` dir (for potential comparison with protobuf)

---

# Python Avro deserializer

[https://avro.apache.org/docs/1.11.1/getting-started-python/](https://avro.apache.org/docs/1.11.1/getting-started-python/)

- `pip install avro==1.11.1`
- had to combine `person` and `address` schemas into 1-file as in Python loading multiple Avro schemas from separate files seems tricky

Python successfully decodes Ruby encoded files. 
```sh
py avro_python.py
{'name': 'Python🐍', 'age': 256, 'address': {'street': 'Green Street', 'city': 'San Francisco'}}
{'name': 'Mojo🐍', 'age': 1, 'address': {'street': 'Blue Street', 'city': 'Saturn🪐'}}
=========================================
{'name': 'Ruby💎', 'age': 77, 'address': {'street': 'Collins Street', 'city': 'Melbourne'}}
{'name': 'Anton', 'age': 36, 'address': {'street': 'Esplanade', 'city': 'Melbourne'}}
```
---

Resources:

1. https://avro.apache.org/
1. [https://cwiki.apache.org/confluence/display/AVRO/Index](https://cwiki.apache.org/confluence/display/AVRO/Index)
1. [avro-rpc-quickstart](https://github.com/phunt/avro-rpc-quickstart) - RPC examples for Python, Ruby and Java

