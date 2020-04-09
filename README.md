# statslogger

Tool to log system stats to stdout or a file

## Installing (macOS only)

Install using [Homebrew](https://brew.sh/).

```sh
brew install atkinchris/tools/statslogger
```

## Usage

After running the tool, the first emitted result will only appear after the set frequency has passed. Note, at short frequencies, CPU stats may not be accurate, as enough time may not have passed to determine an average percentage.

```sh
$ statslogger --help
statslogger 0.5.0
Tool to log system stats to stdout or a file

USAGE:
    statslogger [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -f, --format <format>          Output format [default: JSON]  [possible values: Plain, JSON]
    -o, --output <output>          Output logs to a folder, in files grouped by current date and hour
    -p, --processes <processes>    Number of processes to log [default: 10]
    -t, --time <time>              Set frequency time in seconds [default: 5]
```

## Formats

All formats log to a single line, for easy piping into other tools like `jq`.

### JSON

This will log the results as a JSON object. Note, the example below is prettified to ease of reading, but in reality will be a single line.

```json
{
  "cpu_temp": 48.0625,
  "cpu_usage": 3.3916273,
  "mem_usage": 84.902954,
  "timestamp": "2020-04-07T16:29:50.616+01:00",
  "hostname": "hostname",
  "username": "user",
  "top_processes": [
    { "name": "iTerm2", "cpu_usage": 3.8789282 },
    { "name": "Google Chrome Helper (Renderer)", "cpu_usage": 1.6953138 },
    { "name": "Code Helper (Renderer)", "cpu_usage": 0.60841334 },
    { "name": "Finder", "cpu_usage": 0.49784723 },
    { "name": "statslogger", "cpu_usage": 0.27276957 },
    { "name": "Core Sync", "cpu_usage": 0.23297158 },
    { "name": "Google Chrome", "cpu_usage": 0.17192204 },
    { "name": "Dropbox", "cpu_usage": 0.15859371 },
    { "name": "Adobe Desktop Service", "cpu_usage": 0.14895986 },
    { "name": "Google Chrome Helper (Renderer)", "cpu_usage": 0.111545086 }
  ]
}
```

### Plain

This will log the results in a comma delimited, plain text format, suitable for use a CSV. The columns are `{hostname}, {timestamp}, {CPU}%, {temp}C, {MEM}%, [{process} ({process CPU}%),]`

```csv
hostname, user, 2020-04-07T16:33:05.239188+01:00, 8%, 48C, 86%, iTerm2 (7.8%),Google Chrome (2.5%),Google Chrome Helper (Renderer) (2.1%),Google Chrome Helper (Renderer) (1.5%),Finder (0.6%),Code Helper (Renderer) (0.6%),Google Chrome Helper (0.4%),statslogger (0.2%),Code Helper (Renderer) (0.2%),Core Sync (0.2%)
```

## Releasing (macOS only)

To release a version of this tool, run `./release.sh`. This will build a production copy of the binary, package it, and produce a [Homebrew](https://brew.sh/) formula. Then, do the following steps outside of this repo:

- Paste the contents of the new formula at `./releases/statslogger.rb` into the relevant file in `atkinchris/homebrew-tools` repo.
- Create a GitHub release for the newly created version tag (which was pushed by `release.sh`) and attach the compiled binary package to it, found at `./releases/statslogger-<version>.tar.gz`.

## As a Service (macOS only)

This tool can be run as a persistent service on macOS. After installing, run `brew services start statslogger` to start the service. This will run the tool on a `time` of 15 seconds, outputting to Homebrew's default `var/log` directory, usually `/usr/local/var/log/statslogger_<hostname>_<date>_<hour>`.

After updating or reinstalling, you will need to run `brew services restart statslogger`.

### Error Logging

If running as a service, error logs will be written to Homebrew's default `var/log` directory, usually `/usr/local/var/log/statslogger_err.log`.
