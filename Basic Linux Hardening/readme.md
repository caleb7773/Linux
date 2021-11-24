# Add memory through SWAP Files
> We are going to utilize a SWAP file in order to gain more useable memory for small form factor deployments or Virtual Private Servers (VPS).
```
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
