# What is this spec?

This spec is an attempt to push for a stable replacement of Ruby 1.8.x with 1.9.3+ on RHEL based systems. I have based it off of the work of [FrameOS](http://www.frameos.org) specs for Ruby 1.9.3 and Ruby Enterprise Edition.

### How to install

#### RHEL/CentOS 5/6

```shell
# Remove existing Ruby
rpm -qa | grep -i ruby | xargs rpm -e

# Install dependencies
yum -y install rpm-build redhat-rpm-config rpmdevtools mock

# Create a "builder" user for creating RPM using mock
sudo adduser builder --home-dir /home/builder --create-home --user-group --groups mock --shell /bin/bash --comment "rpm package builder"

# Login as "builder" user
su -l builder

# Create tree of folders for RPM
# Download files for RPM
rpmdev-setuptree
cd ~/rpmbuild/SOURCES
wget ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p385.tar.gz
cd ~/rpmbuild/SPECS
wget https://raw.github.com/lancelakey/ruby-1.9.3-rpm/master/ruby19.spec

# Create RPM using mock
mock --init --resultdir=/home/builder/mock/.
mock --buildsrpm --spec=/home/builder/rpmbuild/SPECS/ruby19.spec --sources=/home/builder/rpmbuild/SOURCES --resultdir=/home/builder/mock/.
mock --rebuild /home/builder/mock/ruby*src.rpm --resultdir=/home/builder/mock/.


# Example
# Optional
# Install the RPM you just made
yum -y install /home/builder/mock/ruby-1.9.3p385-1.el6.x86_64.rpm
```


### What it does

+ Creates RPM

### What it does **not** do

+ Split packages into ruby-libs, ruby-devel, etc (looking for help here)
+ Install alongside Ruby 1.8.x

###

+ If you upgrade from an already installed 1.8.x, you will need to re-install all of your gems. If anyone has a decent way to do this programatically, i'll add it to the doc.

### Requirements

+ EPEL Yum repository (for rpmdev-setuptree)

### Distro support

Tested working on:

* CentOS 6.3 (Final) x86_64

### Personal thoughts

This is by no means, correct, or sane. Nor does it follow any sort of policy for packaging. I leave that to the people who are most familiar with such things, and will willingly accept patches that add those features.
