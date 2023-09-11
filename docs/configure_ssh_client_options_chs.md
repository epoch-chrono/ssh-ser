## Setting up your SSH client configuration file

**ser** is just a SSH helper script, you must configure the SSH Config file correctly before using it.

You can choose one of the following **three** choices:

#### 1. Create a key for passwordless login

    $ [[ ! -d ~/.ssh ]] && mkdir ~/.ssh && chmod 700 .ssh; ssh-keygen -t rsa

Publish your public key to the remote machine:

    cat ~/.ssh/id_rsa.pub | ssh <username>@<hostname> "[[ ! -d ~/.ssh ]] && mkdir ~/.ssh && chmod 700 .ssh; cat >> ~/.ssh/authorized_keys"

To configure the SSH configuration file, add the following to your **~/.ssh/config** file:

    Host <hostname>
    HostName <host-address>
    User <login-username>
    Port <port>
    PreferredAuthentications publickey
    IdentityFile </path/to/key-file>

If the configuration is correct, you can directly log in to the corresponding host through the command `ssh <name>` and do not need to enter a password.

**ser** also works based on this configuration.

#### 2. Create a key pair and use a password

When you see the following prompt when creating a key, enter the password for the key:

    Enter passphrase (empty for no passphrase):

Please repeat the password once:

    Enter same passphrase again:

Each time you use the key, you will need to type the password to increase the security of the key (the password is not available and cannot be used directly when the key is stolen).

But there are ways to temporarily omit the password each time you type:

For **Mac** you can configure the UseKeychain item to save the password to the keychain.

    Host *
    UseKeychain yes

For various distributions of **Linux**, you can use **ssh-agent** to remember the password of the key in memory, which is not explained here.

#### 3. Only use passwords

For password-only situations, do not fill in the `PreferredAuthentications` entry in the configuration **ssh config**.
