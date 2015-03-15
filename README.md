# Vagrant box index

## Overview

Vagrant-box-index is a simple single fie CGI script that creates a
list of Vagrant boxes hosted on a web server.  It is intended to help
maintain a private Vagrant box repository e.g. within your company.

The script walks through a directory structure under web server
document root and looks for Vagrant metadata `*.json` files.  These
files are parsed and the information is rendered into a table, making
it convenient to discover boxes.

Vagrant box [metadata
format](http://docs.vagrantup.com/v2/boxes/format.html) makes it
possible to maintain the boxes under [version
control](http://docs.vagrantup.com/v2/boxes/versioning.html).  Running
`vagrant up` on a project will check if the box is up-to-date by
comparing locally cached metadata to the metadata on the web server.
Vagrant will notify if the box can be updated.


## Installation

To install vagrant-box-index under Apache on Ubuntu, execute the
following commands:

    cp vagrant-box-index /usr/lib/cgi-bin/
    a2enmod cgi
    service apache2 restart


Next, create a directory structure for your boxes, upload them and
create `*.json` files for each one, and finally point your browser to
http://somewhere.com/cgi-bin/vagrant-box-index.


## Example repository

This is an imaginary example of a private repository.  Following
directory structure is created:

    /var/www/html/vagrant/base/rhel-6.json
    /var/www/html/vagrant/base/rhel-6.6.box
    /var/www/html/vagrant/base/sles-11.json
    /var/www/html/vagrant/base/sles-11sp3.box
    /var/www/html/vagrant/proj-bar/test-appliance.json
    /var/www/html/vagrant/proj-bar/test-appliance.box
    /var/www/html/vagrant/proj-foo/rhel-dev.json
    /var/www/html/vagrant/proj-foo/rhel-dev-3.box
  

As an example, the contents of `rhel-dev.json` is:

    {
      "name": "proj-foo/rhel-dev",
      "description": "Baseline for foo-2.3: rhel-6.6, db-5.1, some-other-comp-2.0-alpha1",
      "versions": [{
        "version": "3",
          "providers": [{
            "name": "virtualbox",
            "url": "http://somewhere.com/vagrant/proj-foo/rhel-dev-3.box"
        }]
      }]
    }

An user finds appropriate box by browsing the index and then clicks
"download".  A popup is shown with a command to fetch the box, for
example:

    vagrant box add --name "proj-foo/rhel-dev" http://somewhere.com/vagrant/proj-foo/rhel-dev.json


The following screenshot shows the index page of the example repository:

![screenshot](/images/screenshot.png?raw=true)
