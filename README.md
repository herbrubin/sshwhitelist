# sshwhitelist
A script for Linux that manages an ssh whitelist similar to how hosts.deny used to do it. It uses firewalld. It works on AlmaLinux 8 and higher.

This perl script will help you manage your list of IPs or CIDRs for ssh access to your server without you having to remember all the firewalld commands.
The top section of the program should be edited before you run it because there are some ip addresses you will want to change.

# First time run to initialize
The first time you run it you will want to use the -i option to initialze a new firewalld zone to keep your whitelist in.


# sshwhitelist -h
  sshwhitelist [-hlLvV] -a 1.2.3.4 -r 1.2.3.4

  Allows you to manage a whitelist of ip addresses for ssh access on this server.
  It requires firewalld to be running. 

  It creates a new zone called ssh-whitelist and moves the ssh service to it from the public zone.

  It was written when tcp wrappers and /etc/hosts.deny was removed from Linux.

```
  -a 1.2.3.4   Add this ip address to the white list, CIDR values are allowed
  
  -h           Show this help message
  
  -i           Initialize/create a special zone in the firewall called ssh-whitelist
  
  -l           List the firewall rules related to the ssh whitelist zone ssh-whitelist
  
  -L           List only the ips and CIDRs in the firewall zone ssh-whitelist
  
  -r 1.2.3.4   Remove this ip address to the white list
  
  -V           show version 1.3 and exit

  Note: On a new system, you must initialize the special zone ssh-whitelist with:
```
    sshwhitelist -i

  but please edit the top of this program first!!!

```
# Begin User Defined Section
#----------------------------
my $zone = "ssh-whitelist";
my $lifeline = "101.121.32.210"; # whitelist init and cannot be removed
my $lan = "10.0.0.0/16"; # whitelist init with this in whitelist
my $safe = "10.0."; # cannot add or remove ips that start with this
my $service = "ssh";
my $logger = "logger -p authpriv.warning";
#----------------------------
# End User Defined Section
```

Example:

    sshwhitelist -a 192.168.1.0/24
    
    sshwhitelist -a 77.42.8.59
    
    sshwhitelist -r 77.42.8.59
    
    sshwhitelist -r 192.168.1.0/24

    sshwhitelist -L

# Logging
Activity is logged to /var/log/secure using rsyslogd and the logger command.
