# SSH configuration file support

The SSH configuration supports hostnames based on matching mode and the **match** field. The final configuration of a host can actually be configured in multiple **host** configurations, for example:

    Host *host
    User user
    HostName myhost1.com

    Host myhost1
    Port 1234

You can then log in via **SSH**:

    $ ssh myhost1

Equivalent to:

    $ ssh user@myhost1.com -p 1234

Since the script is written by the Bash Shell, it can only support the format of the simple SSH Config. Currently only the full configuration for a single configuration can be identified, so the configuration should look like this:

    Host myhost1
    HostName myhost1.com
    User user
    [Port 22]
    [Other configurations ...]

    Host myhost2
    HostName myhost2.com
    User user
    [Port 22]
    [Other configurations ...]

    # ...

**ser** will read the following fields under the host configuration, other fields will be ignored:

* **Host**
* **User**
* **HostName**
* **Port** [optional]

Also, the globally valid configuration `Host *` is ignored because it is the default configuration that exists in ssh_config.
