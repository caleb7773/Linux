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
> <html><pre>        Path 	                         Permission<br>
>.ssh directory (code) 	              0700 (drwx------)
>private keys (ex: id_rsa) (code) 	  0600 (-rw-------)
>config 	                            0600 (-rw-------)
>public keys (*.pub ex: id_rsa.pub) 	0644 (-rw-r--r--)
>authorized_keys (code) 	            0644 (-rw-r--r--)
>known_hosts 	                        0644 (-rw-r--r--)</pre></html>

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
# Generate the SSH Config file and assign it proper permissions
touch ~/.ssh/config
sudo chmod 600 ~/.ssh/config

# Create the initial file "THIS WILL OVERWRITE AN EXISTING FILE"!
sudo tee ~/.ssh/config << EOF

### default for all ###
Host *
     ForwardAgent no
     ForwardX11 no
     ForwardX11Trusted yes
     User root
     Port 22
     Protocol 2
     ServerAliveInterval 60
     ServerAliveCountMax 30
     
 ### Host Specific Overrides ###
 EOF

# Create Host specific overrides to use different ports or usernames
sudo tee -a ~/.ssh/config << EOF

Host bastion
     Hostname 192.168.1.2
     User administrator
     Port 22
     IdentityFile ~/.ssh/bastian.key
     
Host nas01
     HostName 192.168.1.100
     User myusername
     Port 22
     IdentityFile ~/.ssh/nas01.key
     ProxyJump bastion
EOF
```

