# Optional

## Installing sshd for remote access
This one is short. `sudo apt install openssh-server`.

## Authenticating with SSH-keys.
Authenticating using ssh-keys is more secure and convenient. As long as the
private key is safe. No one can guess password that isn't in use, and using
password for the keys is optional.

Basically to use key-authentication one has to generate private-key,
public-key pair. Public key goes to remote host(s), and private stays
on local computer.

### WSL

#### Using SSH-keys 
Keys are generated with command `ssh-keygen -t ed25519`. The command will
prompt where to save the key-pair, and suggests _$HOME/.ssh/algo_ and
_$HOME/.ssh/algo.pub_. Accept the defaults. Next, copy the public key to
the server:
```bash
wsl$ # Making sure there exists ~/.ssh directory for the user on server.
wsl$ ssh -l login server_ip
<enter password>

server$ mkdir -p ~/.ssh # Creates the dir, if it doesn't exist.
server$ chmod 700 ~/.ssh # Change permissions.
server$ logout

wsl$ scp ~/.ssh/id_ed25519.pub username@server:~/.ssh
<enter password>
```
Unless a password was used with the keys, login shouldn't ask for one now
either.

#### Disabling password-login
Once login with keys works, it is possible to disable logging in using
password. Use nano to edit */etc/sshd_config* and change the
`PasswordAuthentication yes` line to `PasswordAuthentication no` and
uncomment it.

### Windows
[Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
can also be used to generate keys, and it can also work as a client program
to the server.
