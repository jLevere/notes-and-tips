## USEFUL THINGS I HAVE FOUND


# LINUXS

## ALIASES

there are better ways to do most of this stuff, but this is quick and easy
```
####### sudo alias #######
alias sudo='sudo ' # notice the space, it allows other aliases to use sudo

####### iptables #########
alias show='sudo iptables -nvL --line-numbers'
alias ipt='sudo iptables'

###### ssh ##############
# starts and attaches ssh agent
alias sshup='eval "$(ssh-agent)"'

##### show stuff ########
alias udplisten='ss -lu'
alias udp='ss -u state all'
alias tcplisten='ss -lt'
alias tcp='ss -t state all'
```
Here is a v smart person talking about it if you want to learn more [smart person article](https://blog.sanctum.geek.nz/custom-commands/)


## files and programs

To list all file associated with a package use 
```
dpkg -L {name}
```

and to find out what package a file belongs too use 
```
dpkg -S {/path/to/file}
```

Reading the aggregated logs of services in journalctl, remove the `-f` if you want the past parts.  This is also where stdout of services goes
```
sudo journalctl -f -u {name of service}
```

## processes and memory

### turning on mem swap in ubuntu
This is particularly helpful for things like aws instances where you bump the memory limits often.
```
sudo swapon --show   # if this doesnt show anything then its not on

sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo nano /etc/fstab  # append
  `/swapfile swap swap defaults 0 0`
sudo swapon --show  # or use free to check
```

Swappiness controls how likely the os is to use the swapfile
```
cat /proc/sys/vm/swappiness # 0 is almost only when going to die, 100 is all the time.  60 is default 10 is production.
sudo sysctl vm.swappiness=10  # set it to 10 if you want
```

To turn it back off:
```
sudo swapoff -v /swapfile
remove the line from the /etc/fstab file
sudo rm /swapfile
```

## NETWORKING STUFF

iptables list all the rules with counters and line numbers
```
sudo iptables -nvL --line-numbers
```

iptables list nat rules
```
sudo iptables -t nat -n -v -L
```

allow incoming icmp pings
```
sudo iptables -A INPUT -p icmp --icmp-type 8 -j ACCEPT
```

iptables log rule (4 is warning level)
```
sudo iptables -A INPUT -j LOG --log-level 4 --log-prefix 'my_prefix'
```

iptables insert a rule (insert a rule to be at position 2 in the INPUT chain.  the rule that was at position 2 gets pushed back to 3 and so on down the chain)
```
sudo iptables -I INPUT 2 -j DROP
```

iptables add a port
```
sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW,ESABLISHED -j ACCEPT
```

iptables prerouting rule
```
sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
```


List the open ports on modern systems:
    - for tcp use `ss -lt` for listening or `ss -t state listening`  use `ss -t state all` for everything
    - can do p much same for udp `ss -lu`

## ssh things
Escape dead ssh connections or tunnels 
<kbd>enter + ~ + .</kbd>


open ssh commands menu
<kbd>enter + ~ + ?</kbd>


Forward a port through ssh to your localhost. Where localhost is implied with port 2001 and the the remote end can point out to another server if you want too.
```
ssh remote@server -L 2001:127.0.0.1:80
```


## dot files
here is a smart persons ones, ive not really looked through them [good dots](https://sanctum.geek.nz/cgit/dotfiles.git/about/)


# EMAIL
## random
Add a <kbd> + </kbd> to your email just before the <kbd> @ </kbd>  with an arbitrary tag to tag your emails.  This can be used for filtering and other things.
