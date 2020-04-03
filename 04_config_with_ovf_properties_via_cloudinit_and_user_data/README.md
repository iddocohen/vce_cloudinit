# Description

Also here using the same ``VCOVF``as in folder 03 but now, we also put the user-data file inside the ``VCOVF``such that, after the data source ``VCOVF`` has been used by cloud-init it uses the ``NoCloud`` data source to continue.

The use-case is, consider ``VCOVF`` configures the VCE not enough, one can additionally use ``NoCloud``to configure it even further.

The user-data is encode as base64 (``base64 --wrap=0 user-data``). 

**Please remember** to generate a valid .iso for cloud-init to understand the ``VCOVF``data source one needs to use the following:

``genisoimage -o cloud-init.iso -r ovf-env.xml``
