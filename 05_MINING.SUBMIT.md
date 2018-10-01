# Submit a Share #

The client will submit shares to the server by sending the `mining.submit` message.

__JSON Example__
```json
{
    "id": 3,
    "method": "mining.submit",
    "params": [
        "myWorkerName",
        "00000001",
        "00000001",
        "504e86b9",
        "b2957c02",
        "c2c57c02c2504e86b9c57c02c2c57c02",
        "86b9c57c86b9c57c86b9c57c86b9c57c86b9c57c...",
        "57c86b9c56b95ac028b9ca7ef600c11be6beef9f...",
    ]
}
```

The params contain 8 items:
- [0] _WorkerName_ - The user name to authorize.
- [1] _JobID_ - The ID of the Job.
- [2] _ExtraNonce2_ - The extraNonce2 value used by the miner.
- [3] _Time_ - The nTime value used by the miner.
- [4] _Nonce_ - The 32-bit nonce used by the miner.
- [5] _MtpHashRoot_ - The MTP Hash Root proof.
- [6] _MtpBlock_ - The MTP Block proof.
- [7] _MtpProof_ - The MTP Proof proof.

__Binary Example Overview__
```
> OBJECT
--> KEY_0         : UTF-8 string whose value is "id".
--> KEY_0_VALUE   : a unique NUMBER that is the "id" of the message.
--> KEY_1         : UTF-8 string whose value is "method".
--> KEY_1_VALUE   : STRING whose value is "mining.submit".
--> KEY_2         : UTF-8 string whose value is "params".
--> KEY_2_VALUE   : ARRAY
-----> ARRAY[0]   : STRING whose value is the worker name.
-----> ARRAY[1]   : BYTES value that is the ID of the Job.
-----> ARRAY[2]   : BYTES value that is the ExtraNonce2 value.
-----> ARRAY[3]   : BYTES value that is the nTime.
-----> ARRAY[4]   : BYTES value that is the 32-bit Nonce.
-----> ARRAY[5]   : BYTES value that is the MTP Hash Root.
-----> ARRAY[6]   : BYTES value that is the MTP Block.
-----> ARRAY[7]   : BYTES value that is the MTP Proof.
```

__Binary Example__

| Bytes                       | Value           | Type        |
|-----------------------------|-----------------|-------------|
|0x2d090300                   | 198957          | DataSize    |
|0x0F                         | [OBJ](https://github.com/MintPond/bos/blob/master/FORMAT.md#obj) | DataType    |
|0x03                         | 3               | OBJ-> KeyCount|
|0x02                         | 2               | OBJ-> Key-0-> NameLength|
|0x6964                       | "id"            | OBJ-> Key-0-> Name|
|0x06                         | [UINT8](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint8) | OBJ-> Key-0-> Value-> DataType |
|0x03                         | 3               | OBJ-> Key-0-> Value-> UInt8Value |
|0x06                         | 6               | OBJ-> Key-1-> NameLength|
|0x6d6574686f64               | "method"        | OBJ-> Key-1-> Name |
|0x00                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string)| OBJ-> Key-1-> Value-> DataType |
|0x0d                         | 13              | OBJ-> Key-1-> Value-> StringLength |
|0x6d696e696e672e7375626d6974 | "mining.submit" | OBJ-> Key-1-> Value-> StringValue |
|0x06                         | 6               | OBJ-> Key-2-> NameLength |
|0x726573756c74               | "params"        | OBJ-> Key-2-> Name |
|0x0E                         | [ARRAY](https://github.com/MintPond/bos/blob/master/FORMAT.md#array) | OBJ-> Key-2-> Value-> DataType|
|0x08                         | 8               | OBJ-> Key-2-> Value-> ArrayCount|
|0x0C                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) | OBJ-> Key-2-> Value-> Array[0]-> DataType |
|0x0c                         | 12              | OBJ-> Key-2-> Value-> Array[0]-> StringLength |
|0x6d79576f726b65724e616d65   | "myWorkerName"  | OBJ-> Key-2-> Value-> Array[0]-> StringValue |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[1]-> DataType |
|0x04                         | 4               | OBJ-> Key-2-> Value-> Array[1]-> BytesLength |
|0x01000000                   | 1               | OBJ-> Key-2-> Value-> Array[1]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[2]-> DataType |
|0x04                         | 4               | OBJ-> Key-2-> Value-> Array[2]-> BytesLength |
|0x01000000                   | 1               | OBJ-> Key-2-> Value-> Array[2]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[3]-> DataType |
|0x04                         | 4               | OBJ-> Key-2-> Value-> Array[3]-> BytesLength |
|0xb9864e50                   |                 | OBJ-> Key-2-> Value-> Array[3]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[4]-> DataType |
|0x04                         | 4               | OBJ-> Key-2-> Value-> Array[4]-> BytesLength |
|0x027c95b2                   |                 | OBJ-> Key-2-> Value-> Array[4]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[5]-> DataType |
|0x0f                         | 16              | OBJ-> Key-2-> Value-> Array[5]-> BytesLength |
|0xc2c57c02c2504e86b9c57c02c2c57c02 |           | OBJ-> Key-2-> Value-> Array[5]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[6]-> DataType |
|0xfe00000200                 | 131072          | OBJ-> Key-2-> Value-> Array[6]-> BytesLength |
|0x86b9c57c86b9c57c86b9c57c86b9c57c86b9c57c... |  | OBJ-> Key-2-> Value-> Array[6]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[7]-> DataType |
|0xfec0080100                 | 67776           | OBJ-> Key-2-> Value-> Array[7]-> BytesLength |
|0x57c86b9c56b95ac028b9ca7ef600c11be6beef9f... | | OBJ-> Key-2-> Value-> Array[7]-> Bytes |


## Response from server ##

__JSON Response Example__
```json
{
   "id": 3,
   "result": true,
   "error": null
}
```
__Binary Response Example Overview__
```
> OBJECT
--> KEY_0         : UTF-8 string whose value is "id".
--> KEY_0_VALUE   : a NUMBER that is the "id" of the message being replied to.
--> KEY_1         : UTF-8 string whose value is "result".
--> KEY_1_VALUE   : BOOLEAN (true)
--> KEY_2         : UTF-8 string whose value is "error".
--> KEY_2_VALUE   : NULL
```

__Binary Response Example__

| Bytes                       | Value           | Type        |
|-----------------------------|-----------------|-------------|
|0x1B000000                   | 27              | DataSize    |
|0x0F                         | [OBJ](https://github.com/MintPond/bos/blob/master/FORMAT.md#obj) | DataType    |
|0x03                         | 3               | OBJ-> KeyCount|
|0x02                         | 2               | OBJ-> Key-0-> NameLength|
|0x6964                       | "id"            | OBJ-> Key-0-> Name|
|0x06                         | [UINT8](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint8) | OBJ-> Key-0-> Value-> DataType |
|0x02                         | 2               | OBJ-> Key-0-> Value-> UInt8Value |
|0x06                         | 6               | OBJ-> Key-1-> NameLength|
|0x6D6574686F64               | "result"        | OBJ-> Key-1-> Name |
|0x01                         | [BOOL](https://github.com/MintPond/bos/blob/master/FORMAT.md#bool) | OBJ-> Key-1-> Value-> DataType |
|0x01                         | true            | OBJ-> Key-1-> Value-> BoolValue |
|0x05                         | 5               | OBJ-> Key-2-> NameLength|
|0x6572726F72                 | "error"         | OBJ-> Key-2-> Name|
|0x00                         | [NULL](https://github.com/MintPond/bos/blob/master/FORMAT.md#null) | OBJ-> Key-2-> Value-> DataType |
