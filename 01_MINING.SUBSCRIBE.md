# Connecting to the Server #

After connecting the client will send the `mining.subscribe` message.

__JSON Example__
```json
{
    "id": 1,
    "method": "mining.subscribe",
    "params": ["userAgent/1.0", "deadbeefcafebabe0f831e346a3e900f"]
}
```

The params contain 2 optional items:
- [0] _UserAgent_ - The user agent string of the miner software.
- [1] _SubscriptionID_ - 128 bit subscription ID of previous connection if previous connection was recently lost - used to re-establish server side parameters of a lost connection. Server may or may not use this.

__Binary Example Overview__
```
> OBJECT
--> KEY_0         : UTF-8 string whose value is "id".
--> KEY_0_VALUE   : a NUMBER that is the "id" of the message.
--> KEY_1         : UTF-8 string whose value is "method".
--> KEY_1_VALUE   : STRING whose value is "mining.subscribe".
--> KEY_2         : UTF-8 string whose value is "params".
--> KEY_2_VALUE   : ARRAY
-----> ARRAY[0]   : STRING whose value is the user agent string.
-----> ARRAY[1]   : BYTES whose value is the subscription ID.
```

__Binary Example__

| Bytes                       | Value           | Type        |
|-----------------------------|-----------------|-------------|
|0x4a000000                   | 74              | DataSize    |
|0x0F                         | [OBJ](https://github.com/MintPond/bos/blob/master/FORMAT.md#obj) | DataType    |
|0x03                         | 3               | OBJ-> KeyCount|
|0x02                         | 2               | OBJ-> Key-0-> NameLength|
|0x6964                       | "id"            | OBJ-> Key-0-> Name|
|0x06                         | [UINT8](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint8) | OBJ-> Key-0-> Value-> DataType |
|0x01                         | 1               | OBJ-> Key-0-> Value-> Number |
|0x06                         | 6               | OBJ-> Key-1-> NameLength|
|0x6d6574686f64               | "method"        | OBJ-> Key-1-> Name |
|0x00                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) | OBJ-> Key-1-> Value-> DataType |
|0x10                         | 16              | OBJ-> Key-1-> Value-> StringLength |
|0x6d696e696e672e737562736372696265 | "mining.subscribe" | OBJ-> Key-1-> Value-> StringValue |
|0x06                         | 6               | OBJ-> Key-2-> NameLength |
|0x706172616d73               | "params"        | OBJ-> Key-2-> Name |
|0x0E                         | [ARRAY](https://github.com/MintPond/bos/blob/master/FORMAT.md#array) | OBJ-> Key-2-> Value-> DataType|
|0x02                         | 2               | OBJ-> Key-2-> Value-> ArrayCount|
|0x0C                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) | OBJ-> Key-2-> Value-> Array[0]-> DataType |
|0x09                         | 9               | OBJ-> Key-2-> Value-> Array[0]-> StringLength |
|0x757365724167656e74         | "userAgent"     | OBJ-> Key-2-> Value-> Array[0]-> StringValue |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[1]-> DataType |
|0x10                         | 16              | OBJ-> Key-2-> Value-> Array[1]-> BytesLength |
|0x0f903e6a341e830fbebafecaefbeadde |           | OBJ-> Key-2-> Value-> Array[1]-> Bytes |

## Response from server ##

__JSON Response Example__
```json
{
   "id": 1,
   "result": ["deadbeefcafebabe0f831e346a3e900f", "0800000800000002"],
   "error": null
}
```

The result contains 3 items:
- [0] _SubscriptionID_ - 128 bit unique connection ID assigned by server.
- [1] _ExtraNonce1_ - 64-bit extra nonce 1 value assigned by server. This is a unique number assigned per connection and used within the coinbase to make the work each miner does unique.

__Binary Response Example Overview__
```
> OBJECT
--> KEY_0         : UTF-8 string whose value is "id".
--> KEY_0_VALUE   : a unique NUMBER that is the "id" of the message being replied to.
--> KEY_1         : UTF-8 string whose value is "result".
--> KEY_1_VALUE   : ARRAY
-----> ARRAY[0]   : The subscription Id BYTES.
-----> ARRAY[1]   : The assigned extraNonce1 BYTES.
--> KEY_2         : UTF-8 string whose value is "error".
--> KEY_2_VALUE   : NULL
```

__Binary Response Example__

| Bytes                       | Value           | Type        |
|-----------------------------|-----------------|-------------|
|0x37000000                   | 55              | DataSize    |
|0x0F                         | [OBJ](https://github.com/MintPond/bos/blob/master/FORMAT.md#obj) | DataType    |
|0x03                         | 3               | OBJ-> KeyCount|
|0x02                         | 2               | OBJ-> Key-0-> NameLength|
|0x6964                       | "id"            | OBJ-> Key-0-> Name|
|0x06                         | [UINT8](https://github.com/MintPond/bos/blob/master/FORMAT.md#uint8) | OBJ-> Key-0-> Value-> DataType |
|0x01                         | 1               | OBJ-> Key-0-> Value-> Number |
|0x06                         | 6               | OBJ-> Key-1-> NameLength|
|0x726573756c74               | "result"        | OBJ-> Key-1-> Name |
|0x0E                         | [ARRAY](https://github.com/MintPond/bos/blob/master/FORMAT.md#array) | OBJ-> Key-1-> Value-> DataType |
|0x02                         | 2               | OBJ-> Key-1-> Value-> ArrayCount|
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-1-> Value-> Array[0]-> DataType |
|0x10                         | 16              | OBJ-> Key-1-> Value-> Array[0]-> ByteLength |
|0x0f903e6a341e830fbebafecaefbeadde |           | OBJ-> Key-1-> Value-> Array[0]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-1-> Value-> Array[1]-> DataType |
|0x08                         | 8               | OBJ-> Key-1-> Value-> Array[1]-> BytesLength |
|0x0200000008000008           |                 | OBJ-> Key-1-> Value-> Array[1]-> Bytes |
|0x05                         | 5               | OBJ-> Key-2-> NameLength|
|0x6572726F72                 | "error"         | OBJ-> Key-2-> Name|
|0x00                         | [NULL](https://github.com/MintPond/bos/blob/master/FORMAT.md#null) | OBJ-> Key-2-> Value-> DataType |
