# Lumberjack

Lumberjack is a logging interface for shell scripts.

## About

Lumberjack handles your logs for you. When you set a logfile and log level, it is linked to the process ID of the script in which it is run, so that further calls need only contain the message to append to the logfile.

## Installation

### Requirements

Although lumberjack can be used within any script, ZSH 5+ must be installed and in your $PATH in order to run it.

### [Zulu](https://github.com/zulu-zsh/zulu)

```sh
zulu install lumberjack
```

### Manual

Simply move the `lj` executable to somewhere in your $PATH.

```sh
git clone https://github.com/molovo/lumberjack
mv lumberjack/lj /usr/local/bin
```

## Usage

Lumberjack should be called a first time to set the log file and level.

```sh
lj --file /usr/local/var/log/myawesomescript.log --level critical
```

Once done, further calls need only contain the level at which that log applies, and the message.

```sh
lj critical ‘Something went wrong’
```

The level is any one of `emergency`, `alert`, `critical`, `error`, `warning`, `notice`, `info` or `debug`. If the level is not set, the default is `notice`. Likewise, when logging you can omit the first parameter, and a level of `notice` is assumed.

```sh
lj 'This is a notice'
```

If a message is logged at a level lower than that which is set, it is gracefully ignored. This allows you to dynamically set the log level based on parameters passed to your script.

```sh
#!/usr/bin/env zsh

# Parse CLI options
zparseopts -D v=verbose -verbose=verbose

# Set the log level
local level=critical
[[ $verbose ]] && level=notice

# Initialise lumberjack
lj --file ~/log --level $level

lj 'This is a notice'
```

In the above example, running `script.zsh` would result in nothing being output to the log. Running `script.zsh --verbose` would increase the log level, allowing the notice to be appended to the log file.

## TODO

* Allow log format to be modified
* Add additional drivers for send logs to services like [Sentry](http://sentry.io) etc.

## License

Copyright (c) 2016 James Dinsdale <hi@molovo.co> (molovo.co)

Lumberjack is licensed under The MIT License (MIT)

## Team

* [James Dinsdale](http://molovo.co)
