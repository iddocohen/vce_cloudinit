# Description

Another example of NoCloud (user-data/meta-data), with some more configurations then 01 folder had:

- In the meta-data:
  - we change the hostname to be vedge1
  - we configure interface GE3 to be static with IP 10.1.0.2/24 and gateway 10.1.0.1
- In the user-data:
  - we run a command to sleep for 60 seconds (to ensure network configuration has taken place)
  - we remove the option 1 from dhcp configuration
  - and restart the network configuration


The outcome is, that after GE3 is configured, all other interfaces will be configurated with DHCP IP. 

**Credits to Saqeb Akther** how gave me that configuration.


