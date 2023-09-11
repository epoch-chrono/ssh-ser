# ser
- Forked from [https://github.com/frimin/ser](https://github.com/frimin/ser)

Use the SSH login helper script under the terminal written by Bash Shell, and the tunnel process management script.

###  Installation & update

    # install
    $ curl -s https://raw.githubusercontent.com/vjunior1981/ser/master/update/install.sh | bash -
    $ echo 'export PATH="$HOME/.ser/bin:$PATH"' >> ~/.bash_profile
    $ source ~/.bash_profile

    # update
    $ ser update

###  Ready to work

See: [Setting up your SSH client configuration file.](docs/configure_ssh_client_options_chs.md)

##  SSH login function

###  Display all host names in the SSH configuration

    $ ser

    The host list is displayed in the following form:

    1) myhost1 - user@myhost1.com:22
    2) myhost2 - user@myhost2.com:22
    3) myhost3 - user@myhost3.com:22
    ...

###  SSH login

    ser [o] [ssh_options] <index|pattern ...> [: <command>]

The host in the list can be logged in via the index displayed in the host list. The following command is equivalent to ssh myhost1 , where the subcommand o is optional. If your host name conflicts with other subcommand names, you must use the subcommand o :

    $ ser 1
    $ ser o 1

By hostname or wildcard:


    $ ser myhost1
    $ ser 'myh*'

You can also add a command to execute on the host. Note that the command parameters located after the host list must be separated from the host list parameters by a colon:

    $ ser 1 2 myhost3 : 'ls -alh'

Or execute the same command on all hosts and pass the option directly to SSH:

    $ ser -2 -4 '*' : 'echo $HOSTNAME'

### Copy file

    ser cp [options] <source ...> [:] <destination_file ...>
    ser cp [options] <source ...> [:] <destination_directory ...>

Copy one or more files or directories from the machine to the target machine:

    $ ser cp "file1" "file2" "*:~/"
    $ ser cp -r "dir1" "dir2" "*:~/"

Or directly specify a few host indexes to specify the target machine:

    $ ser cp "file1" "file2" : {1,3}":~/"
    $ ser cp "file1" "file2" : 1:~/ 3:~/

**Note:** When multiple target parameters are passed in, they must be explicitly used : symbols are separated.

The curly braces {} here are interpreted by the Bash Shell, so they cannot be enclosed in quotes.

## Tunnel management function

The **local/remote forward** (LocalForward/RemoteForward) forwarding management function in SSH is integrated in the **ser**, and the forwarding tunnel can be conveniently stopped/started after the configuration is completed.

### Create a forwarding tunnel

    $ ser tunnel-add <tunnel-name> <host> local 80 80

Starting the tunnel after the creation is complete is equivalent to generating an instruction to start the SSH tunnel:

    ssh -f -N <host> <options> -L 127.0.0.1:5000:127.0.0.1:80

Of course you can also create multiple forwarding types in the same tunnel:

    $ ser tunnel-add <tunnel-name> <host> local 443 443
    $ ser tunnel-add <tunnel-name> <host> remote 22 8000
    $ ser tunnel-add <tunnel-name> <host> socks5 1080

The following tunnel start instructions are generated:

    ssh -f -N <tunnel-name> <options> \
    -L 127.0.0.1:5000:127.0.0.1:80 \
    -L 127.0.0.1:443:127.0.0.1:443 \
    -R 127.0.0.1:22:127.0.0.1:8000

### Remove forwarding

    $ ser tunnel-remove <tunnel-name> <forward-index>

### Start tunnel

Mark the tunnel to be enabled while opening the tunnel connection.

Start all tunnels:

    $ ser start

Start a tunnel:

    $ ser start <tunnel-name>
    $ ser tunnel-start <tunnel-name>

Start the tunnel with the pattern matching tunnel name:

    $ ser tunnel-start <tunnel-name-pattern>

After startup, each SSH tunnel runs in the background with the -f option.

When the startup fails, the end code of the SSH process is printed, and the last print error of the tunnel can be seen by the ser info command. In most cases, only this error is retained because the file redirected by the error stream is refreshed when the tunnel is started.

### Stop tunnel

    $ ser stop
    $ ser tunnel-stop
    $ ser tunnel-stop <tunnel-name-wildcarded>

### Restart the tunnel

    $ ser restart
    $ ser tunnel-restart
    $ ser tunnel-restart <tunnel-name-wildcarded>

### List all tunnels

    $ ser tl
    $ ser tunnel-list

The above command will only list the tunnel name, whether it is enabled, and whether it is connected, for example:

    host - [enabled] [connected]

For more details, please use `ser info`:

     # tunnel: my-forward
     - host: host
     - enable: yes
     - connect: yes
     - pid: 30103
     - out file: ~/.ser/tunnels/myhost1.out
     - error file: ~/.ser/tunnels/myhost1.err
     - forward #1: (local) 127.0.0.1:80 <= (remote) 127.0.0.1:80

### Check tunnel status

Check for tunnels that are enabled but not connected and start them.

    $ ser check
    $ ser tunnel-check

You can add this command to the crontab to implement a disconnected connection for automatic reconnection:

    crontab <<< "0-59 * * * * bash '/path/to/ser' tunnel-check"

The above commands can be listed by `ser help tunnel-check`.

##  SSH configuration file support

See: [SSH Profile Support.](docs/ssh_config_format_support_chs.md)


## Other help content

    $ ser help

## LICENSE

MIT
---
