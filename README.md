# boot2docker Vagrant Box

This repository contains the scripts necessary to create a Vagrant-compatible
[boot2docker](https://github.com/boot2docker/boot2docker) box and is compatible with Docker v1.5. 

If you work solely
with Docker, this box lets you keep your Vagrant workflow and work in the
most minimal Docker environment possible.

## Usage

The box is available on
[Vagrant Cloud](https://vagrantcloud.com/dduportal/boot2docker), making
it very easy to use it:

    $ vagrant init dduportal/boot2docker
    $ vagrant up

If you want the actual box file, you can download it from the
[tags page](https://github.com/dduportal/boot2docker-vagrant-box/tags).

On OS X, to use the docker client, follow the directions here:
http://docs.docker.io/installation/mac/#docker-os-x-client (you'll need to
export `DOCKER_HOST`). You should then be able to to run `docker version` from
the host.

![Vagrant Up Boot2Docker](https://raw.github.com/mitchellh/boot2docker-vagrant-box/master/readme_image.gif)

## Tips & tricks

* Vagrant synced folder has been tested with :
  * vbox shared folder
  * NFS

* If you want to tune contents (custom profile, install tools inside the VM) that do not fit into the "vagrant provisionning" lifecycle combinded with the un-persistence of boot2docker, the "bootlocal" system has been extended :
  * The [boot2docker FaQ](https://github.com/boot2docker/boot2docker/blob/master/doc/FAQ.md) says that you can provide a custom script, named bootlocal.sh to execute things at the end of the boot.
  * We customize in order to run that script from the /vagrant share when mounted, at the end of the boot.
  * So : just place a "bootlocal.sh" script alongside your Vagrantfile to customize what's inside your b2d VM.


* If you use the VM as a remote Docker daemon in a private networked docker server you need to add in your bootlocal.sh :
(Thanks to @Freyskeyd)

```
# Regenerate certs for the newly created Iprivate network IP
sudo /etc/init.d/docker restart
# Copy tls certs to the vagrant share to allow host to use it
sudo cp -r /var/lib/boot2docker/tls /vagrant/
```

  Next, you need to configure your Docker environment :
  * Export cert path: ```export DOCKER_CERT_PATH=<Absolute path to your vagrant share>/tls```
  * Export Docker host path : ```export DOCKER_HOST=tcp://192.168.10.10:2376``` (You can also use localhost if the NAT is OK)

## Building the Box

If you want to recreate the box, rather than using the binary, then
you can use the scripts and Packer template within this repository to
do so in seconds.

To build the box, first install the following prerequisites:

  * [Packer](http://www.packer.io) (at least version 0.7.2)
  * [VirtualBox](http://www.virtualbox.org) (at least version 4.3), VMware, or Parallels
  * [Make](http://www.gnu.org/software/make/)
  * [wget](http://www.gnu.org/software/wget/)

Then follow the steps:

```
$ make
...
```
