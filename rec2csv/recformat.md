# *.REC format

I have written a little [blog
post](https://techbotch.org/blog/udp3305s-recordings/index.html#udp3305s-recordings)
on the reverse engineering process (only in German right now).

The bottom line is this:

| Address | type        | Meaning                                                     |
|---------|-------------|-------------------------------------------------------------|
| 0x00    | 2 bytes     | 0x1006 magic number?                                        |
| 0x02    | 58 bytes    | Header "This is a RECORD file." plus zero padding and EOR   |
| 0x3c    | 4 bytes     | 0xffffffff                                                  |
| 0x40    | uint32      | Logging period [s]                                          |
| 0x44    | 12 bytes    | unknown (all 0xff)                                          |
| 0x50    | n*44 bytes  | n records of 44 byte each                                   |

Each record has exactly 44 Bytes:

| Address | type    | Meaning                                                     |
|---------|---------|-------------------------------------------------------------|
| 00      | uint32  | Channel 1 readout voltage                                   |
| 04      | uint32  | Channel 1 readout current                                   |
| 08      | uint32  | Channel 2 readout voltage                                   |
| 12      | uint32  | Channel 2 readout current                                   |
| 16      | uint32  | Channel 3 readout voltage                                   |
| 20      | uint32  | Channel 3 readout current                                   |
| 24      | uint32  | Serial setup readout voltage                                |
| 28      | uint32  | Serial setup readout current                                |
| 32      | uint32  | Parallel setup readout voltage                              |
| 36      | uint32  | Parallel setup readout current                              |
| 40      | 4 bytes | Checksum? Flags? Seems to always start with 0x1812          |


All readout values are 32 bit integers stored in *little endian* format.
These integers represent multiples of 100 µV and 100 µA, respectively. I.e. a
readout voltage of 5.00 Volts is stored as `50000`. Therefore, the program
divides them by 10000 to get Volts/Ampere.

