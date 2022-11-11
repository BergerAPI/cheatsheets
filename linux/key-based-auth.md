Using private and public keys instead of passwords, is in many cases, more comfortable and especially more secure. 

## Generating public and private key
On both windows and unix systems, this is possible via OpenSSH.

```shell
ssh-keygen
```
It will ask for the passphrase, which is basically just an encryption for the private key. One should use it when they don't have their disk encrypted, or many people have access to their pc. Using it results in OpenSSH asking for the passphrase every time it needs it.
It will print the location of public and private key, if successful. 

## Telling the server
The server now needs to know about the public key, one just generated. This is done by entering it into the ``authorized_keys`` which is located at ``~/.ssh/``.

### Linux
The OpenSSH version that many Linux distributions provide by default, includes a handy command, that does exactly what we want.

```shell
ssh-copy-id user@address
```
After execution, it will prompt for the password of the user.

### Windows
Windows doesn't have a default command which automatically does that, so one needs to get creative.

See [Windows Commands](../windows/commands)

```powershell
cd ~/.ssh

scp id_rsa.pub user@address:~/temp_rsa.pub
# IPv6
scp -6 id_rsa.pub user@[address]:~/temp_rsa.pub
```
This will, again, ask for the password of the user, and then copy the local file to the server.
Once that is finished, one just needs to connect to the server and put the contents into the ``authorized_keys`` file.

```shell
ssh user@address

# On Server
cd /home/user # Or wherever one moved the file to
mkdir ~/.ssh/ # If it doesn't already exist
cat temp_rsa.pub >> ~/.ssh/authorized_keys
```
