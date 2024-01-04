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

| Address | length  | Meaning                                                     |
|---------|---------|-------------------------------------------------------------|
| 0x00    | 2       | 0x1006 magic number?                                        |
| 0x02    | 58      | Header "This is a RECORD file." plus zero padding and EOR   |
| 0x3c    | 4       | 0xffffffff                                                  |
| 0x40    | 4       | Logging period [s]; little-endian integer                   |
| 0x44    | 12      | unknown (all 0xff)                                          |
| 0x50    | n*44    | n records of 44 byte each                                   |

Each record has exactly 44 Bytes:

| Address | length  | Meaning                                                     |
|---------|---------|-------------------------------------------------------------|
| 00      | 4       | Channel 1 readout voltage                                   |
| 04      | 4       | Channel 1 readout current                                   |
| 08      | 4       | Channel 2 readout voltage                                   |
| 12      | 4       | Channel 2 readout current                                   |
| 16      | 4       | Channel 3 readout voltage                                   |
| 20      | 4       | Channel 3 readout current                                   |
| 24      | 4       | Serial setup readout voltage                                |
| 28      | 4       | Serial setup readout current                                |
| 32      | 4       | Parallel setup readout voltage                              |
| 36      | 4       | Parallel setup readout current                              |
| 40      | 4       | Checksum? Flags? Seems to always start with 0x1812          |

All of the data above is comprised of integers in little-endian format.

