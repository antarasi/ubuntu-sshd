# ubuntu-sshd

Dockerized SSH service, built on top of [official Ubuntu](https://registry.hub.docker.com/_/ubuntu/) images.

## Image tags

- antarasi/ubuntu-sshd:14.04 (trusty)
- antarasi/ubuntu-sshd:16.04 (xenial)
- antarasi/ubuntu-sshd:18.04 (bionic)

## Installed packages

Base:

- [Trusty (14.04) minimal](http://packages.ubuntu.com/trusty/ubuntu-minimal)
- [Xenial (16.04) minimal](http://packages.ubuntu.com/xenial/ubuntu-minimal)
- [Bionic (18.04) minimal](http://packages.ubuntu.com/bionic/ubuntu-minimal)

Image specific:
- [openssh-server](https://help.ubuntu.com/community/SSH/OpenSSH/Configuring)

Config:

  - exposed port 22
  - default command: `/usr/sbin/sshd -D`
  - root user password: `root`
  - ubuntu user password: `ubuntu`
  - root login not permitted (login as ubuntu and type `su -` to access root account)

## Run example

```bash
$ sudo docker run -d -P --name ubuntu-sshd antarasi/ubuntu-sshd:14.04
$ sudo docker port ubuntu-sshd 22
  0.0.0.0:49154

$ ssh ubuntu@localhost -p 49154
  Password: ubuntu
$ ubuntu@ubuntu-sshd:~$ su -
  Password: root
$ root@ubuntu-sshd:~#
```

## Security

If you are making the container accessible from the internet you'll probably want to secure it bit.
You can do one of the following two things after launching the container:

- Change the root password: `docker exec -ti ubuntu-sshd passwd`
- Disallow password login for ubuntu user, use SSH keys instead:

```bash
# copy local ssh public key to server authorized keys store
  $ ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@localhost -p 49154
    Password: ubuntu

# delete ubuntu user password
  $ docker exec ubuntu-sshd passwd -d ubuntu

# password is not required now
  $ ssh ubuntu@localhost -p 49154
```

## Issues

If you run into any problems with this image, please check (and potentially file new) issues on the [antarasi/ubuntu-sshd](https://github.com/antarasi/ubuntu-sshd/issues) repo, which is the source for this image.

## Attribution

The original author: [rastasheep](https://github.com/rastasheep/ubuntu-sshd)
