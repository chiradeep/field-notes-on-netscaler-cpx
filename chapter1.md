# Getting to know Citrix NetScaler CPX

Citrix NetScaler is a commercial load balancer that competes with open source load balancers such as HAProxy and Nginx. NetScaler is available as a container as well, in a product version known as NetScaler CPX. Even better, there is a free version available, called the CPX Express. For the rest of the blog post, I’m using my Mac laptop running OS X Sierra, and Docker for Mac version 1.12.

CPX Express can be downloaded from [https://microloadbalancer.com]([https://microloadbalancer.com). Once you download the compressed file, you can create a docker image:

```
$ docker load -i ~/Downloads/cpx-11.1-48.10.gz
```
You should now have the CPX image available:

```
$ docker images cpx
REPOSITORY          TAG                 IMAGE ID                        
cpx                 11.1-48.10          9c5a5e94c333        
```

Create a container from this image:

```
$ docker run -dt -p 22  --ulimit core=-1 --privileged=true --name=cpx cpx:11.1-48.10
275626b96389755a88362c8df3ae4851f13e45d22f502b29900612fc2da28444
```

Determine the SSH port of this running container:

```
$ docker port cpx 22
0.0.0.0:32849
```

Login to the NetScaler CPX using the id/password combination of `root/linux`:

```
$ ssh -p 32849 root@127.0.0.1 
The authenticity of host '[127.0.0.1]:32849 ([127.0.0.1]:32849)' can't be established.
ECDSA key fingerprint is SHA256:N3dlhNYbvYjOPkbt9ogrg2fwwUDWPVs8JwvLuE64/RQ.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[127.0.0.1]:32849' (ECDSA) to the list of known hosts.
root@127.0.0.1's password: 
Welcome to Ubuntu 14.04 LTS (GNU/Linux 3.19.0-39-generic x86_64)
```

Once logged in, you can execute almost any regular NetScaler CLI command by passing the command to the `cli_script.sh` script. For example:

```
root@275626b96389:~# cli_script.sh 'show ip'   
exec: show ip
    Ipaddress        Traffic Domain  Type             Mode     Arp      Icmp     Vserver  State
    ---------        --------------  ----             ----     ---      ----     -------  ------
1)  172.17.0.2       0               NetScaler IP     Active   Enabled  Enabled  NA       Enabled
2)  192.0.0.1        0               SNIP             Active   Enabled  Enabled  NA       Enabled
Done
```

In the next chapter we’ll see how to exercise the CPX’s primary function: load balancing


