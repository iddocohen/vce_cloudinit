# Description

It uses simple NoCloud (user-data/meta-data) files for cloud-init to provision the bare minimum.

What is the bare-minimum?:
- Hostname change to "vce" (default hostname is "vce-login")
- Give it a default password of "Velocloud123" (if not a random password is used)
- To force it not change password after first configuration
- And enable ssh

With the .iso already in the repository, one can easily use that to login to the VCE and do other configurations.

