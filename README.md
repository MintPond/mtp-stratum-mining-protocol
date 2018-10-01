MTP Stratum Mining Protocol Draft
=================================

The MTP Stratum Mining Protocol is a modification of the original
Stratum Mining Protocol. Where there is missing information,
please refer to the original specification for clarification:
- [SlushPool - Stratum Mining Protocol](https://slushpool.com/help/manual/stratum-protocol)
- [Bitcoin Wiki - Stratum Mining Protocol](https://en.bitcoin.it/wiki/Stratum_mining_protocol)

### Why Modify the Stratum Protocol? ###
There are 2 primary reasons to modify the protocol. The first is that
MTP requires additional data (proofs) to submit shares. The second is
that the additional data is quite large.

The MTP proofs are about 200kb per share submission. The current
use of UTF-8 hex strings doubles the size of this data. Given the
non-fixed and non-trivial expenses of high bandwidth usage, it is
necessary to be more efficient with regards to serialization of the data.

### How does this new protocol reduce bandwidth usage? ###
Switching to binary serialization will prevent the doubling of
bandwidth usage associated with using hex values to represent
binary data. The binary serialization format used comes from the BOS
serializer.

### Why use BOS Serializer Format? ###
The BOS serializer uses a relatively simple format that allows for the same
flexibility as JSON. It allows implementing the MTP stratum protocol into
existing stratum's more easily and can accept future changes and additions
to the protocol without causing incompatibility with older implementations or
requiring rewrites to the serializer to accommodate.

## Specification ##

- 01 - [mining.subscribe](01_MINING.SUBSCRIBE.md) - After connecting the client will send the `mining.subscribe` message.
- 02 - [mining.authorize](02_MINING.AUTHORIZE.md) - The client should authorize itself using the `mining.authorize` message.
- 03 - [mining.set_target](03_MINING.SET_TARGET.md) - The server will set the miners share target using the `mining.set_target` message.
- 04 - [mining.notify](04_MINING.NOTIFY.md) - The server will send the client `mining.notify` messages containing new and updated jobs.
- 05 - [mining.submit](05_MINING.SUBMIT.md) - The client submits shares using the `mining.submit` message.

### Serialization Changes ###

In order to prevent the doubling of data size inherent with using hex
strings, the MTP Stratum Protocol uses a binary serialization rather
than JSON string serialization. The binary serialization format is based
on the [BOS (Binary Object Serializer)](https://github.com/MintPond/bos) format.

The examples below represent the same exception message. The first example
is the _JSON Representation_ and is the JSON string that would be sent
using the original stratum protocol. The second example,
_Binary Overview_, shows an abstract overview of how the same data is
laid out in binary. The table below that shows the actual byte values to
represent the JSON value in binary. Note that all binary values are
sent in little endian byte order.

__JSON Representation__
```json
{
    "id": 1,
    "result": null,
    "error": [21, "Job not found", null]
}
```

__Binary Overview__
```
> OBJECT
--> KEY_0         : UTF-8 string whose value is "id".
--> KEY_0_VALUE   : a NUMBER that is the "id" of the message.
--> KEY_1         : UTF-8 string whose value is "result".
--> KEY_1_VALUE   : NULL
--> KEY_2         : UTF-8 string whose value is "error".
--> KEY_2_VALUE   : ARRAY
-----> ARRAY[0]   : a NUMBER that is the error code of the exception.
-----> ARRAY[1]   : STRING that is a human readable message.
-----> ARRAY[2]   : NULL or traceback data for debugging.
```

__Binary__

| Bytes                       | Value           | Type        |
|-----------------------------|-----------------|-------------|
|0x2d000000                   | 46              | DataSize    |
|0x0F                         | [OBJ](https://github.com/MintPond/bos/blob/master/FORMAT.md#obj) | DataType    |
|0x03                         | 3               | OBJ-> KeyCount|
|0x02                         | 2               | OBJ-> Key-0-> NameLength|
|0x6964                       | "id"            | OBJ-> Key-0-> Name|
|0x06                         | [UINT8](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint8) | OBJ-> Key-0-> Value-> DataType|
|0x01                         | 1               | OBJ-> Key-0-> Value-> IntValue|
|0x06                         | 6               | OBJ-> Key-1-> NameLength|
|0x726573756c74               | "result"        | OBJ-> Key-1-> Name|
|0x00                         | [NULL](https://github.com/MintPond/bos/blob/master/FORMAT.md#null) | OBJ-> Key-1-> Value-> DataType|
|0x05                         | 5               | OBJ-> Key-2-> NameLength|
|0x6572726f72                 | "error"         | OBJ-> Key-2-> Name|
|0x0E                         | [ARRAY](https://github.com/MintPond/bos/blob/master/FORMAT.md#array) | OBJ-> Key-2-> Value-> DataType|
|0x03                         | 3               | OBJ-> Key-2-> Value-> ArrayCount|
|0x06                         | [UINT8](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint8) | OBJ-> Key-2-> Value-> Array[0]-> DataType|
|0x15                         | 21              | OBJ-> Key-2-> Value-> Array[0]-> IntValue|
|0x0C                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string)          | OBJ-> Key-2-> Value-> Array[1]-> DataType|
|0x0d                         | 15              | OBJ-> Key-2-> Value-> Array[1]-> StringLength|
|0x4a6f62206e6f7420666f756e64 | "Job not found" | OBJ-> Key-2-> Value-> Array[1]-> StringValue|
|0x00                         | [NULL](https://github.com/MintPond/bos/blob/master/FORMAT.md#null) | OBJ-> Key-2-> Value-> Array[2]-> DataType|

The complete binary value for the above exception message represented in hex is:
`0x2d0000000F03026964060106726573756c7400056572726f720E0306150C0d4a6f62206e6f7420666f756e6400`

### Message changes ###
There are changes to some of the messages to be aware of:

- The response to the `mining.subscribe` message has been changed. The first element contains the subscription ID and the 3rd element, which contained the ExtraNonce2 size, has been removed. The ExtraNonce2 size should be 8 bytes.
- The `mining.set_difficulty` message has been changed to `mining.set_target` along with the type of data it contains.
- The `mining.submit` message has 3 additional elements appended to the "params" array for MTP proofs.

### Binary Layout ###
The key/value pairs in an object may be in any order. Do no assume that the
binary order of key/value pairs as shown in examples is the way the
data will always be received. Optional object properties or array
elements may not even be present.

### NUMBER Data Types ###
The [DataType](https://github.com/MintPond/bos/blob/master/FORMAT.md#data-types) used for numbers is the smallest data type that the value will fit into. A larger data type such as UINT16 or UINT32 may be used as required. A larger fixed size may also be used.

NUMBER data types include:
 - [UINT8](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint8)
 - [UINT16](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint16)
 - [UINT32](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint32)

### Hex String vs Bytes ###
Where the original protocol uses UTF-8 hex strings, the [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes)
value should be used rather than [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) value in the MTP
stratum protocol. The byte order should be little endian.

### ExtraNonce ###
The ExtraNonce1 and ExtraNonce2 values are both 64-bits (8-bytes).

## Resources ##
- [BOS Binary Format](https://github.com/MintPond/bos/blob/master/FORMAT.md) - Binary Format Specification.
- [BOS Serializer](https://github.com/MintPond/bos) - NodeJS binary object serializer.