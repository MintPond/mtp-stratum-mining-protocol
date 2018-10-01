# Change Share Target #

The server will send jobs to the client via the `mining.notify` message.

__JSON Example__
```json
{
    "id": null,
    "method": "mining.notify",
    "params": [
        "00000001",
        "4d16b6f85af6e2198f44ae2a6de67f78487ae5611b77c6c0440b921e00000000",
        "01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff20020862062f503253482f04b8864e5008",
        "072f736c7573682f000000000100f2052a010000001976a914d23fcdf86f7e756a64a7a9688ef9903327048ed988ac00000000",
        [],
        "00000002",
        "1c2ac4af",
        "504e86b9",
        true
    ]
}
```

The params contain 9 items:
- [0] _Job_ID_ - The 32-bit job ID.
- [1] _PrevHash_ - The previous block hash.
- [2] _Coinbase1_ - The first part of the coinbase.
- [3] _Coinbase2_ - The final part of the coinbase.
- [4] _MerkleBranch_ - Array of hashes to be used to calculate merkle root.
- [5] _Version_ - Block version.
- [6] _Bits_ - Encoded current network difficulty.
- [7] _Time_ - Current nTime.
- [8] _CleanJobs_ - True to invalidate all previous jobs due to a new block.
False indicates the job is part of the same block as previous job.
Previous jobs in same block are still valid.

__Binary Example Overview__
```
> OBJECT
--> KEY_0         : UTF-8 string whose value is "id".
--> KEY_0_VALUE   : NULL
--> KEY_1         : UTF-8 string whose value is "method".
--> KEY_1_VALUE   : STRING whose value is "mining.notify".
--> KEY_2         : UTF-8 string whose value is "params".
--> KEY_2_VALUE   : ARRAY
-----> ARRAY[0]   : BYTES value containing ID of the Job.
-----> ARRAY[1]   : BYTES value containing 256-bit previous block hash.
-----> ARRAY[2]   : BYTES value containing first part of coinbase data.
-----> ARRAY[3]   : BYTES value containing final part of coinbase data.
-----> ARRAY[4]   : ARRAY of BYTES values that are hashes of transactions (merkle branch)
-----> ARRAY[5]   : BYTES value that is the 32-bit block version.
-----> ARRAY[6]   : BYTES value that is the 32-bit network difficulty bits.
-----> ARRAY[7]   : BYTES value that is the 32-bit current nTime value.
-----> ARRAY[8]   : BOOL value indicating new block or updated block job.
```

__Binary Example__

| Bytes                       | Value           | Type        |
|-----------------------------|-----------------|-------------|
|0xd6000000                   | 214             | DataSize    |
|0x0F                         | [OBJ](https://github.com/MintPond/bos/blob/master/FORMAT.md#obj) | DataType    |
|0x03                         | 3               | OBJ-> KeyCount|
|0x02                         | 2               | OBJ-> Key-0-> NameLength|
|0x6964                       | "id"            | OBJ-> Key-0-> Name|
|0x00                         | [NULL](https://github.com/MintPond/bos/blob/master/FORMAT.md#null) | OBJ-> Key-0-> Value-> DataType |
|0x06                         | 6               | OBJ-> Key-1-> NameLength|
|0x6d6574686f64               | "method"        | OBJ-> Key-1-> Name |
|0x00                         | [STRING](https://github.com/MintPond/bos/blob/master/FORMAT.md#string) | OBJ-> Key-1-> Value-> DataType |
|0x0D                         | 13              | OBJ-> Key-1-> Value-> StringLength |
|0x6d696e696e672e6e6f74696679 | "mining.notify" | OBJ-> Key-1-> Value-> StringValue |
|0x06                         | 6               | OBJ-> Key-2-> NameLength |
|0x726573756c74               | "params"        | OBJ-> Key-2-> Name |
|0x0E                         | [ARRAY](https://github.com/MintPond/bos/blob/master/FORMAT.md#array) | OBJ-> Key-2-> Value-> DataType|
|0x09                         | 9               | OBJ-> Key-2-> Value-> ArrayCount|
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[0]-> DataType |
|0x04                         | 4               | OBJ-> Key-2-> Value-> Array[0]-> BytesLength |
|0x01000000                   | 1               | OBJ-> Key-2-> Value-> Array[0]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[1]-> DataType |
|0x20                         | 32              | OBJ-> Key-2-> Value-> Array[1]-> BytesLength |
|0x000000001e920b44c0c6771b61e57a48787fe66d2aae448f19e2f65af8b6164d | | OBJ-> Key-2-> Value-> Array[1]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[2]-> DataType |
|0x3a                         | 58              | OBJ-> Key-2-> Value-> Array[2]-> BytesLength |
|0x0100000001000000000000000000000000000000000000000000000000000... | | OBJ-> Key-2-> Value-> Array[2]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[3]-> DataType |
|0x33                         | 51              | OBJ-> Key-2-> Value-> Array[3]-> BytesLength |
|0x072f736c7573682f000000000100f2052a010000001976a914d23fcdf86f7... | | OBJ-> Key-2-> Value-> Array[3]-> Bytes |
|0x0E                         | [ARRAY](https://github.com/MintPond/bos/blob/master/FORMAT.md#array) | OBJ-> Key-2-> Value-> Array[4]-> DataType|
|0x00                         | 0               | OBJ-> Key-2-> Value-> Array[4]-> ArrayCount|
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[5]-> DataType |
|0x04                         | 4               | OBJ-> Key-2-> Value-> Array[5]-> BytesLength |
|0x02000000                   | 2               | OBJ-> Key-2-> Value-> Array[5]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[6]-> DataType |
|0x04                         | 4               | OBJ-> Key-2-> Value-> Array[6]-> BytesLength |
|0xafc42a1c                   |                 | OBJ-> Key-2-> Value-> Array[6]-> Bytes |
|0x0D                         | [BYTES](https://github.com/MintPond/bos/blob/master/FORMAT.md#bytes) | OBJ-> Key-2-> Value-> Array[7]-> DataType |
|0x04                         | 4               | OBJ-> Key-2-> Value-> Array[7]-> BytesLength |
|0xb9864e50                   |                 | OBJ-> Key-2-> Value-> Array[7]-> Bytes |
|0x01                         | [BOOL](https://github.com/MintPond/bos/blob/master/FORMAT.md#bool) | OBJ-> Key-2-> Value-> Array[8]-> DataType |
|0x01                         | true            | OBJ-> Key-2-> Value-> Array[8]-> BoolValue |
