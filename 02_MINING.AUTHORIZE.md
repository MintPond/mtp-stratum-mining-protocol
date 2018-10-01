# Authorize Workers #

After receiving a successful response to `mining.subscribe`, the client worker
should authorize itself using the `mining.authorize` message.

__JSON Example__
```json
{
    "id": 2,
    "method": "mining.authorize",
    "params": ["myWorkerName", "mypwd"]
}
```

The params contain 2 items:
- [0] _WorkerName_ - The worker to authorize.
- [1] _Password_ - Optional password.

__Binary Example Overview__
```
> OBJECT
--> KEY_0         : UTF-8 string whose value is "id".
--> KEY_0_VALUE   : a unique NUMBER that is the "id" of the message.
--> KEY_1         : UTF-8 string whose value is "method".
--> KEY_1_VALUE   : STRING whose value is "mining.authorize".
--> KEY_2         : UTF-8 string whose value is "params".
--> KEY_2_VALUE   : ARRAY
-----> ARRAY[0]   : STRING whose value is the user name.
-----> ARRAY[1]   : STRING whose value is the password.
```

__Binary Example__

| Bytes                       | Value           | Type        |
|-----------------------------|-----------------|-------------|
|0x42000000                   | 66              | DataSize    |
|0x0F                         | [OBJ](https://github.com/MintPond/bos/blob/master/FORMAT.md#obj) | DataType    |
|0x03                         | 3               | OBJ-> KeyCount|
|0x02                         | 2               | OBJ-> Key-0-> NameLength|
|0x6964                       | "id"            | OBJ-> Key-0-> Name|
|0x06                         | [UINT8](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint8) | OBJ-> Key-0-> Value-> DataType |
|0x02                         | 2               | OBJ-> Key-0-> Value-> UInt8Value |
|0x06                         | 6               | OBJ-> Key-1-> NameLength|
|0x6d6574686f64               | "method"        | OBJ-> Key-1-> Name |
|0x00                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) | OBJ-> Key-1-> Value-> DataType |
|0x10                         | 16              | OBJ-> Key-1-> Value-> StringLength |
|0x6d696e696e672e617574686f72697a65 | "mining.authorize" | OBJ-> Key-1-> Value-> StringValue |
|0x06                         | 6               | OBJ-> Key-2-> NameLength |
|0x726573756c74               | "params"        | OBJ-> Key-2-> Name |
|0x0E                         | [ARRAY](https://github.com/MintPond/bos/blob/master/FORMAT.md#array) | OBJ-> Key-2-> Value-> DataType|
|0x02                         | 2               | OBJ-> Key-2-> Value-> ArrayCount|
|0x0C                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) | OBJ-> Key-2-> Value-> Array[0]-> DataType |
|0x0c                         | 12              | OBJ-> Key-2-> Value-> Array[0]-> StringLength |
|0x6d79576f726b65724e616d65   | "myWorkerName"  | OBJ-> Key-2-> Value-> Array[0]-> StringValue |
|0x0C                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) | OBJ-> Key-2-> Value-> Array[1]-> DataType |
|0x05                         | 5               | OBJ-> Key-2-> Value-> Array[1]-> StringLength |
|0x6d79507764                 | "myPwd"         | OBJ-> Key-2-> Value-> Array[1]-> StringValue |

## Response from server ##

__JSON Response Example__
```json
{
   "id": 2,
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
--> KEY_1_VALUE   : BOOL (true)
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
|0x00                         | NULL            | OBJ-> Key-2-> Value-> DataType |
