# Tools for the UDP3305S / UDP3305S-E lab power supply

Currently, there are two items in this repository:

* `rec2csv` is a command line tool for converting `*.rec` files to csv format
* a TestController config file for these power supplies

# rec2csv

Convert `*.rec` files created by a UNI-T UDP3305S or UPD3305S-E lab power supply to
CSV format.

These lab power supplies have an undocumented `RECORDER` functionality that
allows logging readout data to a file. Unfortunately, the file format is binary
and undocumented, so a little reverse engineering was required to make use of
these files.

## Usage

Make the `rec2csv` python script executable and run it with the `*.rec` file as
the only argument.  The data is written to STDOUT.

    ./rec2csv foo.REC

To save the output to a file use shell redirection:

    ./rec2csv foo.REC > foo.csv

If you are not on LINUX, you may need to call the python interpreter explicitly:
    
    python rec2csv foo.REC > foo.csv


# TestController config file

The file under `testcontroller/` is a configuration file for the popular
[TestController](https://lygte-info.dk/project/TestControllerIntro%20UK.html)I
will submit it to the TestController Project.

# Contributing

If you find an error or have figured out a piece of data that I have missed,
please open an issue and let me know. Please don't submit pull-requests before
discussing your issue.

