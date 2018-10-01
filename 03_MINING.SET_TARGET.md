# Change Share Target #

The server will tell the client what target shares it will accept before
sending the first job. It can also change the target at anytime in order
to change the submission rate. It will do this by sending
the `mining.set_target` message to the client.

__JSON Example__
```json
{
    "id": null,
    "method": "mining.set_target",
    "params": ["00000000ffff0000000000000000000000000000000000000000000000000000"]
}
```

The params contain 1 item:
- [0] _Target_ - The 256-bit target value.

__Binary Example Overview__
```
> OBJECT
--> KEY_0         : UTF-8 string whose value is "id".
--> KEY_0_VALUE   : NULL
--> KEY_1         : UTF-8 string whose value is "method".
--> KEY_1_VALUE   : STRING whose value is "mining.set_target".
--> KEY_2         : UTF-8 string whose value is "params".
--> KEY_2_VALUE   : ARRAY
-----> ARRAY[0]   : BYTES value containing 256-bit target.
```

__Binary Example__

| Bytes                       | Value           | Type        |
|-----------------------------|-----------------|-------------|
|0x49000000                   | 73              | DataSize    |
|0x0F                         | [OBJ](https://github.com/MintPond/bos/blob/master/FORMAT.md#obj) | DataType    |
|0x03                         | 3               | OBJ-> KeyCount|
|0x02                         | 2               | OBJ-> Key-0-> NameLength|
|0x6964                       | "id"            | OBJ-> Key-0-> Name|
|0x00                         | [NULL](https://github.com/MintPond/bos/blob/master/FORMAT.md#null) | OBJ-> Key-0-> Value-> DataType |
|0x06                         | 6               | OBJ-> Key-1-> NameLength|
|0x6d6574686f64               | "method"        | OBJ-> Key-1-> Name |
|0x00                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) | OBJ-> Key-1-> Value-> DataType |
|0x11                         | 17              | OBJ-> Key-1-> Value-> StringLength |
|0x6d696e696e672e7365745f746172676574 | "mining.set_target" | OBJ-> Key-1-> Value-> StringValue |
|0x06                         | 6               | OBJ-> Key-2-> NameLength |
|0x726573756c74               | "params"        | OBJ-> Key-2-> Name |
|0x0E                         | [ARRAY](https://github.com/MintPond/bos/blob/master/FORMAT.md#array) | OBJ-> Key-2-> Value-> DataType|
|0x01                         | 1               | OBJ-> Key-2-> Value-> ArrayCount|
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[0]-> DataType |
|0x20                         | 32              | OBJ-> Key-2-> Value-> Array[0]-> BytesLength |
|0x0000000000000000000000000000000000000000000000000000ffff00000000 |  | OBJ-> Key-2-> Value-> Array[0]-> Bytes |
