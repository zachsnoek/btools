# btools  
A series of scripts to automate the execution of commands within a cluster.</br></br>
Adapted from BYOC (Vance, Poublon, & Polik, 2016) for use with BYOC++.
</br></br>

## Conventions
* The head node is identified as `node01`
* The compute nodes are identified as `node02` and `node03`</br></br>

## Installation
1. On all of the nodes, install and configure `rsh`</br></br>

   a. Install the required packages</br></br>
      `$ yum install -y rsh rsh-server`</br></br>

   b. In `/etc/securetty`, add the following lines</br></br>
      `rsh`</br>
      `rexec`</br>
      `rlogin`</br></br>

   c. In `/root/.rhosts`, add the following lines</br></br>
      `node01 root`</br>
      `node02 root`</br>
      `node03 root`</br></br>

   d. In `/etc/hosts.equiv`, add the following lines</br></br>
      `node01`</br>
      `node02`</br>
      `node03`</br></br>

   e. Enable and start the sockets</br></br>
      `$ systemctl enable rsh.socket`</br>
      `$ systemctl enable rexec.socket`</br>
      `$ systemctl enable rlogin.socket`</br>
      `$ systemctl start rsh.socket`</br>
      `$ systemctl start rexec.socket`</br>
      `$ systemctl start rlogin.socket`</br></br>

   f. Disable SELinux by changing `SELINUX=enforcing` in `/etc/sysconfig/selinux`</br></br>
      `SELINUX=disabled`</br></br>
      
   g. Reboot all of the nodes</br></br>
      `$ init 6`</br></br>

2. On the head node, run `btools` to create all of the scripts on your machine</br></br>
   `$ ./btools`</br></br>

3. Add the hostnames of your compute nodes to `/usr/local/sbin/bhosts`</br></br>
   `node02`</br>
   `node03`</br></br>

4. Enable passwordless rsh to remove password prompts for the `bsync` command</br></br>

   a. Generate a public/private rsa key pair; leave all of the prompts blank and hit enter for each</br></br>
      `$ ssh-keygen`</br>
      `Generating public/private rsa key pair.`</br>
      `Enter file in which to save the key (/root/.ssh/id_rsa):`</br>
      `Enter passphrase (empty for no passphrase):`</br>
      `Enter same passphrase again:`</br>
      `Your identification has been saved in /root/.ssh/id_rsa.`</br>
      `Your public key has been saved in /root/.ssh/id_rsa.pub.`</br></br>

   b. Copy the public ssh key to all of the compute nodes and enter the passwords for each machine when prompted</br></br>
      `$ ssh-copy-id -i /root/.ssh/id_rsa.pub node02`</br>
      `$ ssh-copy-id -i /root/.ssh/id_rsa.pub node03`</br></br>
   
## Description
**bhosts** is a file that contains the hostnames of all of the compute nodes. For example:</br></br>
```
node02
node03
node04
```
</br>

**brsh** loops through all of the hostnames in `bhosts`, executing `rsh <command>` for each.
</br></br>

**bexec** executes a command over the hostnames in `bhosts`, but it does not wait for each node to finish like `brsh` does. `bexec` starts the command on each node and then waits for them to finish; additionally, it retrieves and displays the logs for the operations. If Slurm is set up on the cluster, `bexec` will check will status of the compute nodes through Slurm.
</br></br>

**bpush**
`bpush` copies a file to all of the hostnames in `bhosts`. Similarly to `bexec`, it executes the command across the nodes simultaneously, while also retrieving and displaying the logs
</br></br>

**bfiles**
`bfiles` is a file that contains a list of files to be copied to all of the compute nodes. It contains:</br></br>
```
/etc/passwd/
/etc/group/
/etc/shadow/
/etc/gshadow/
```
</br>

**bsync**
`bsync` copies the files defined in `bfiles` to all of the compute nodes. Thus, the users on the head node will exist on all of the compute nodes.
</br></br>

## References
Nathan R. Vance, Michael L. Poublon and William F. Polik, "BYOC: Build Your Own Cluster, Part III - Configuration",</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Linux Journal, July 2016, Issue 279, 70-98.
</br>
