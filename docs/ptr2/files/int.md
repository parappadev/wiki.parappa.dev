INT files are an archive format the game uses to store audiovisual assets for game levels, the title screen, and logo.
It contains blocks of data with filenames and size values organized in sections.

# Format
A .INT file comprises multiple "sections" containing a header and LZSS-compressed file data.

## Resource types
| ID  | Name       | Description                                                    |
|-----|------------|----------------------------------------------------------------|
| 0   | End        | A final chunk placed at the end of the INT file for alignment. |
| 1   | Textures   | Contains tm0/1/2 format texture data.                          |
| 2   | Sounds     | Contains sound data.                                           |
| 3   | Stage      | Contains SPM format 3D model data.                             |
| 4   | Red Hat    | Contains data for the red hat character model.                 |
| 5   | Blue Hat   | Contains data for the blue hat character model.                |
| 6   | Pink Hat   | Contains data for the pink hat character model.                |
| 7   | Yellow Hat | Contains data for the yellow hat character model.              |


## Header
The header is 32 bytes long and appears at the start of every section.
```hexdump
00000000  11 22 33 44 10 00 00 00  01 00 00 00 70 00 00 00  |."3D........p...|
00000010  90 07 00 00 00 b8 03 00  00 00 00 00 00 00 00 00  |................|
```
It contains a list of 4-byte values. All the values except for Magic are little-endian `long`s.

| Position | Name            | Description                                                                               |
|----------|-----------------|-------------------------------------------------------------------------------------------|
| `00-03`  | Magic           | Present at the start of every header. Always `11 22 33 44`.                               |
| `04-07`  | File Count      | Amount of files present in this section.                                                  |
| `08-0b`  | Resource Type   | The [resource type](#resource-types) of this section.                                     |
| `0c-0f`  | Info Offset     | Offset pointing to where the "info" data is located, relative to the start of the header. |
| `10-13`  | Contents Offset | Offset pointing to where the "content" data is located, relative to the info offset.      |
| `14-17`  | Compressed Size | The size of the "content" data.                                                           |
| `18-1b`  | Empty           |                                                                                           |
| `1c-1f`  | Empty           |                                                                                           |


## Implementations
https://github.com/posesix/ptr2tools/blob/master/sources/ptr2int/ptr2int.cpp
https://github.com/pahaze/pwf2tools-cpp/blob/master/execs/sources/pwf2int/pwf2int.cpp
https://github.com/jmkd3v/ptr2tools-python/blob/main/ptr2tools/int.py
