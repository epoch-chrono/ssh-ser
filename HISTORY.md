## 0.1.0b6#6 - 0.1.0b7#7

* Translated from Chinese to English and added zsh autocomplete support.

## 0.1.0b5#5 - 0.1.0b6#6

* Modify the `cp` command "The last parameter passed in is the target host", which is defined as the argument before the list of host names.

## 0.1.0b4#4 - 0.1.0b5#5

* Remove the mechanism for generating temporary files from the redirect input stream of the `o` command
* Modify the `o` command host list to multiple parameters, and pass in the original ssh option, the executed remote command is separated by `:`
* delete token

## 0.1.0b3#3 - 0.1.0b4#4

* Add the contents of the standard input stream to each host via a temporary file when executing an SSH remote command.
* Added support for creating socks5 forwarding
* Rewrite the cp command
* Add proxy directive

## 0.1.0b2#2 - 0.1.0b3#3

* Fixed an error when updating the command `update`: modified the script being executed by the bash shell.
* Modify usage mode (**PATTERNS**) The host name filled in by the prompt is not supported to ignore the corresponding configuration.
