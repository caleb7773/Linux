# Add memory through SWAP Files
> We are going to utilize a SWAP file in order to gain more useable memory for small form factor deployments or Virtual Private Servers (VPS).
```bash
# Check for SWAP files already being used, "sudo swapoff" should disable it
sudo swapon --show

# Create the SWAP file and specify where it should be saved, and how big you want it
sudo fallocate -l 4G /swapfile

# Modify permissions to allow it to be utilized as a SWAP file, anything else will not work
sudo chmod 600 /swapfile

# Format the new SWAP file as SWAP Space
sudo mkswap /swapfile

# Turn on the new SWAP file
sudo swapon /swapfile
sudo swapon --show

# Permanently mount the SWAP file with /etc/fstab
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

# Configuring SSH for Management Device
> In order to effeciently manage multiple devices it is best to setup an SSH host device that can quickly connect to different VPS's and Local Hosts

## SSH Folder Permissions
> <html>        Path 	                         Permission<br>
>.ssh directory (code) 	            0700 (drwx------)<br>
>private keys (ex: id_rsa) (code) 	  0600 (-rw-------)<br>
>config 	                            0600 (-rw-------)<br>
>public keys (*.pub ex: id_rsa.pub) 	0644 (-rw-r--r--)<br>
>authorized_keys (code) 	            0644 (-rw-r--r--)<br>
>known_hosts 	                      0644 (-rw-r--r--)<br></html>

## Creating SSH-Keys
> SSH keys will allow us to securely log into machine without using a password and ultimately allow you to disable all password logins on your remote machine

```bash
# Create ".ssh" directory and assign it the proper permissions
mkdir ~/.ssh
sudo chmod 700 ~/.ssh
cd ~/.ssh

# Generate a new Private / Public Key pair, it is best to have individual keys for each machine
ssh-keygen

# Remove the potentially sensitive information at the end of the public key (username / host of creator)
sudo sed -i 's/=.*./=/g' "{key_you_just_created}".pub

# Transfer the public key to your remote server or VPS
ssh-copy-id -p "{ssh_port_of_distant_end}" -i ~/.ssh/"{key_you_just_created}".pub "{remote_user}"@"{remote_IP_address}"
```

## Generating SSH Config File
> SSH Config files will allow you to save multiple SSH flags into a single alias name making your job easier

```bash
# Generate the SSH Config file
touch ~/.ssh/config


