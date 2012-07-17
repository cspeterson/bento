Bento is a project that encapsulates
[Veewee](https://github.com/jedi4ever/veewee/) definitions for
building [Vagrant](http://vagrantup.com) baseboxes. We use these boxes
internally at Opscode for testing Hosted Chef, Private Chef and
our open source [cookbooks](http://community.opscode.com/users/Opscode).

These basebox definitions are originally based on
[work done by Tim Dysinger](https://github.com/dysinger/basebox) to
make "Don't Repeat Yourself" (DRY) modular baseboxes. Thanks Tim!

The following baseboxes are publicly available and were built using this project.

* opscode-ubuntu-10.04 - http://opscode-vm.s3.amazonaws.com/vagrant/boxes/opscode-ubuntu-10.04.box
* opscode-ubuntu-11.04 - http://opscode-vm.s3.amazonaws.com/vagrant/boxes/opscode-ubuntu-11.04.box
* opscode-ubuntu-12.04 - http://opscode-vm.s3.amazonaws.com/vagrant/boxes/opscode-ubuntu-12.04.box
* opscode-centos-5.5 - http://opscode-vm.s3.amazonaws.com/vagrant/boxes/opscode-centos-5.5.box
* opscode-centos-5.7 - http://opscode-vm.s3.amazonaws.com/vagrant/boxes/opscode-centos-5.7.box
* opscode-centos-6.0 - http://opscode-vm.s3.amazonaws.com/vagrant/boxes/opscode-centos-6.0.box
* opscode-centos-6.2 - http://opscode-vm.s3.amazonaws.com/vagrant/boxes/opscode-centos-6.2.box

# Getting Started

First, clone the project, then install the required Gems with Bundler.

    $ git clone git://github.com/opscode/bento.git
    $ cd bento
    $ bundle install --binstubs

List available baseboxes that can be built:

    $ bundle exec vagrant basebox list

Build, for example, the ubuntu-12.04 basebox.

    $ bundle exec vagrant basebox build ubuntu-12.04

You can validate the basebox using Veewee's built in validator.
However note that the test for Ruby (and Puppet) will fail. The Ruby
installation is in `/opt/chef/embedded`, and we do not add the bin
directory to the `$PATH`, and we don't use Puppet internally.

    $ bundle exec vagrant basebox validate ubuntu-12.04

Aside from that, the basebox should be ready to use. Export it:

    $ bundle exec vagrant export ubuntu-12.04

Congratulations! You now have `./ubuntu-12.04.box`, a fully functional
basebox that you can then add to Vagrant and start testing cookbooks.

# How It Works

Veewee reads the definition specified and automatically builds a
VirtualBox machine. The VirtualBox guest additions and the target OS
ISO are downloaded into the `iso/` directory.

We use Veewee version 0.3.0.alpha+ because it contains fixes for
building CentOS boxes under certain circumstances.

# Definitions

The definitions themselves are split up into directories that get
symlinked into specific basebox directories.

Most of the files are symlinked for a particular box. The one
exception is the `definition.rb` file, which contains the specific
configuration for the Veewee session for a basebox, including the ISO
filename, its source URL, and the MD5 checksum of the file.

## Common

* `chef-client.sh`: Installs Chef and Ruby with
  [Opscode's full stack installer](http://opscode.com/chef/install)
* `minimize.sh`: Zeroes out the root disk to reduce file size of the box
* `ruby.sh`: **Deprecated** Use `chef-client.sh`
* `session.rb`: Baseline session settings for Veewee
* `vagrant.sh`: Installs VirtualBox Guest Additions, adds the Vagrant
  SSH key

## CentOS

* `cleanup.sh`: Removes unneeded packages, cleans up package cache,
  and removes the VBox ISO and Chef rpm
* `ks.cfg`: Kickstart file for automated OS installation
* `session.rb`: General CentOS session settings for Veewee

## Ubuntu

* `cleanup.sh`: Removes unneeded packages, cleans up package cache,
  and removes the VBox ISO and Chef deb
* `networking.sh`: Removes networking setup like udev that may
  interfere with Vagrant network setup
* `preseed.cfg`: The Debian Preseed file for automated OS installation
* `session.rb`: General Ubuntu session settings for Veewee
* `sudoers.sh`: Customization for `/etc/sudoers`
* `update.sh`: Ensures that the OS installation is updated

License and Authors
===================

Author:: Seth Chisamore (<schisamo@opscode.com>)
Author:: Stephen Delano (<stephen@opscode.com>)
Author:: Joshua Timberman (<joshua@opscode.com>)
Author:: Tim Dysinger (<tim@dysinger.net>)

Copyright:: 2012, Opscode, Inc (<legal@opscode.com>)
Copyright:: 2011-2012, Tim Dysinger (<tim@dysinger.net>)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.