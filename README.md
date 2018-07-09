# btools  
A series of scripts to automate the execution of commands within a cluster.</br>
Adapted from [BYOC](https://www.webmo.net/support/byoc.pdf) by [Vance](https://github.com/NathanRVance) and [Polik](https://github.com/poliklab) for use with BYOC++.</br>

## Add btools  
1. On the head node, run `add-btools`</br></br>
   `$ ./add-btools`</br></br>

2. Add the hostnames of your compute nodes to `/usr/local/sbin/bhosts`</br></br>
   ```
   node02
   node03
   node04
   ```  

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
</br>
