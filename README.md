# rec2csv

Convert `*.rec` files created by a UNI-T UDP3305S or UPD3305S-E lab power supply to
CSV format.

These lab power supplies have an undocumented `RECORDER` functionality that
allows logging readout data to a file. Unfortunately, the file format is binary
and undocumented, so a little reverse engineering was required to make use of
these files.

# Usage

Make the `rec2csv` python script executable and run it with the `*.rec` file as
the only argument.  The data is written to STDOUT.

    ./rec2csv foo.REC

To save the output to a file use shell redirection:

    ./rec2csv foo.REC > foo.csv

If you are not on LINUX, you may need to call the python interpreter explicitly:
    
    python rec2csv foo.REC > foo.csv


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


All readout values are 32 bit integers stored in
[little-endian](https://en.wikipedia.org/wiki/Endianness) format. These
integers represent multiples of 100 µV and 100 µA, respectively. I.e. a readout
voltage of 5.00 Volts is stored as `50000`. Therefore, the program divides them
by 10000 to get Volts/Ampere.


# Contributing

If you find an error or have figured out a piece of data that I have missed,
please open an issue and let me know. Please don't submit pull-requests before
discussing your issue.
