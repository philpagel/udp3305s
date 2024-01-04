# rec2csv

Convert `*.rec` files created by a UNI-T UDP3305s or UPD3305S-E lab power supply to
CSV format.

These lab powre supplies have an undocumented `RECORDER` functionality that
allows logging readout data to a file. Unfortunately, the file format is binary
and undocumented, so a little reverse engineering was required to make use of
these filed.

# Usage

Make the `rec2csv` python script executable and run it with the `*.rec` file as
the only argument.  The data is written to STDOUT.

    ./rec2csv foo.REC

To redirect it to a file use shell redirection:

    ./rec2csv foo.REC > foo.csv

If you are not on LINUX, you may need to call the python interpreter explicitly:
    
    python rec2csv foo.REC > foo.csv


# *.REC format


