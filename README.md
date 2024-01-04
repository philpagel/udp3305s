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
| 0x00    | 58 byte | Header "This is a REC file"                                 |
| 0x40    | 4 byte  | Logging period [s]                                          |
| 0x50    |         | n recoords of 44 byte each                                  |

