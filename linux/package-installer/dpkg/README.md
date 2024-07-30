# DPKG

To list all listed packages
```sh
$ dpkg -l
```

To install a package `*.deb` with *dpkg*:
```sh
$ sudo dpkg --install package.deb
$ sudo dpkg -i package.deb
```

In case dependencies are not yet installed:
```sh
$ sudo apt-get install -f
```

```sh
$ sudo dpkg --remove package
$ sudo dpkg -r package
```

Querying information

```sh
# Querying package information
$ dpkg -s package
# List all files in package
$ dpkg -L package
```


