# Encrypted Object
An encrypted object is an internal file format used to encrypt and compress
any raw file data

It is currently used in the following files:

 - Item Proto
 - Mob Proto
 - EterPack Index
 - EterPack Content (type 2 and 6)


The format consists in a header with 20 bytes followed by the data
(either encrypted or compressed)


## Header
![Encrypted Object](../../Topics/Formats/assets/encrypted_object_header.png)

| Name                                                | Bytes | Description                                    |
|-----------------------------------------------------|-------|------------------------------------------------|
| <span style="color:red">Cipher Magic</span>         | 4     | Magic code that specifies the object type      |
| <span style="color:royalblue">Encrypted Size</span> | 4     | Data size after being encrypted                |
| <span style="color:green">Compressed Size</span>    | 4     | Data size after being compressed               |
| <span style="color:yellow">Raw Size</span>          | 4     | Data size before being encrypted or compressed |
| <span style="color:grey">Black</span>               | 4     | Encrypted/Raw Magic Code                       |


## Data Compression

The data is compressed by with one of the following algorithms
depending on which Magic code is specified

### Known Compression Algorithms

| Magic Code | Algorithm |
|------------|-----------|
| MCOZ       | LZO 1x    |
| MCSP       | Snappy    |


## Data Encryption

The data is encrypted with the XTEA algorithm, so to decrypt it
is required a specific key for decrypting, a list of known keys follows below

### Knowing if the data is encrypted

If the bytes in <span style="color:royalblue">Encrypted Size</span> is >= 0 
and <span style="color:red">Cipher Magic</span> is
different from <span style="color:grey">Black</span>,
then the data is encrypted, meaning the bytes of <span style="color:grey">Black</span>
correspond to the Magic used to check if XTEA decryption was successful

Otherwise, <span style="color:grey">Black</span> will be
equal to <span style="color:red">Red</span>

[//]: # (TODO: Explain how and why the Last Magic is encrypted)

### Known XTEA keys
| Used In          | Key (16 bytes, Hexadecimal)                      |
|------------------|--------------------------------------------------|
| Item Proto       | 	A1 A4 02 00 AA 15 54 04 E7 8B 5A 18 AB D6 AA 01 |
| Mob Proto        | 	46 74 49 00 0B 4A 00 00 B7 6E 08 00 9D 18 68 00 |
| EterPack Index   | 	B9 9E B0 02 6F 69 81 05 63 98 9B 28 79 18 1A 00 |
| EterPack Content | 	22 B8 B4 04 64 B2 6E 1F AE EA 18 00 A6 F6 FB 1C |


## Parsing / Visualization

### Kaitai
A Kaitai structure file for reading the format is available
[here](https://metin2.dev/bin/?f3421ddb59ca59b1#6vPviQ8fcQn5gtALBfeqFoxA82w9tAScZbXVbziM45mF)

### Lyketo
You can use Lyketo-VFS for serialization and detection
which supports any de-facto object type, available
[here](https://metin2.dev/bin/?f3421ddb59ca59b1#6vPviQ8fcQn5gtALBfeqFoxA82w9tAScZbXVbziM45mF)

