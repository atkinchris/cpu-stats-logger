# statslogger

Tool to log system stats to stdout or a file

```sh
$ statslogger --help

statslogger 0.1.0
Tool to log system stats to stdout or a file

USAGE:
    statslogger [FLAGS] [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -j, --json       Output as JSON
    -V, --version    Prints version information

OPTIONS:
    -f, --frequency <frequency>    Set frequency in seconds [default: 5]
    -o, --output <output>          Output results to file: {timestamp}, {CPU}%, {temp}C, {MEM}%
```
