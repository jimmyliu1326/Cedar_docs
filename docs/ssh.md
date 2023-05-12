# Password-free SSH connection with SSH keys

If you are lazy like me who does not enjoy typing password every time logging into some remote server, follow the steps below:

#### 1. Create a SSH key pair

```bash
ssh-keygen -t rsa -f ~/.ssh/id_rsa
```

> Press [Enter] when prompted for a passphrase for no passphrase

The command `ssh-keygen` will generate a public key and a private key called `id_rsa.pub` and `id_rsa`, respectively.

#### 2. Add your public SSH key to your Compute Canada account

* Navigate to https://ccdb.alliancecan.ca/ssh_authorized_keys
* Open up `id_rsa.pub` in any text editor e.g. NotePad
* Copy the entire file content and paste it into the SSH Key field
* Click [Add Key] to confirm

#### 3. Test SSH connection to Compute Canada Cluster using private key

```bash
ssh -i ~/.ssh/id_rsa username@cedar.computecanada.ca
```

> You may be prompted for password on the first usage of the private key. You should not be prompted for password on subsequent log-ins

#### 4. Simplify SSH connections by setting up a local ssh config file

If you ever get tired of repeatedly typing the entire domain name of the server, you can set up SSH short cuts by configuring a local SSH profile.

* Open up `~/.ssh/config`; if the file does not exist, create one by running `touch ~/.ssh/config`
* Add the following lines to the SSH config file
```
Host cedar
  HostName cedar.computecanada.ca
  User your-cc-username
  IdentityFile ~/.ssh/id_rsa
```
* Now you can connect to the Cedar cluster by running `ssh cedar`