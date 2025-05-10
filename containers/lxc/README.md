# LXC

## Install lxc

On a debian like OS, install lxc with the followign command
```cmd
sudo apt update
sudo apt install lxc lxc-templates uidmap
```

You will see a new interface call lxcbr0 has been created
```cmd
$ ip a
```

Create a container
```cmd
$ sudo lxc-create -t download -n my-container
```
You will be prompted to answer some questions like the distribution, the release and the architecture. You just need to look at the list and chose one row then write the first, the second and the third colmn.

Then start the container
```cmd
$ sudo lxc-start -n my-container

# Check it's running
$ sudo lxc-ls --fancy
```
Execute a shell inside the container
```cmd
sudo lxc-attach -n my-container
```

Stop the container
```cmd
$ sudo lxc-stop -n my-container
```

Delete the container
```cmd
sudo lxc-destroy -n my-container 
```
Add the -f argument in case the container is still running.

The configuration file and the file system are located in this directory: `/var/lib/lxc/<your container name>`
