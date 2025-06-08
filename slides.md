---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
hideInToc: true
title: Welcome to Slidev
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# open graph
# seoMeta:
#  ogImage: https://cover.sli.dev
themeConfig:
  paginationX: r
  paginationY: t
  paginationPagesDisabled: [1]
---

# Chef Training

Mischa Taylor

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
hideInToc: true
routeAlias: toc
---

# Table of Contents

<Toc columns="2"/>

---
layout: section
---

# Setup Chef Workstation

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

# Cinc Workstation Install - Linux
Use the installation script provided by Cinc. The script detects your OS version and downloads the appropriate package.

```bash
curl -L https://omnitruck.cinc.sh/install.sh | sudo bash -s -- -P cinc-workstation
```

Verify the installation:

```bash
cinc --version
```

Should list the versions of all the Cinc-related tools:

```bash
Cinc Workstation version: 25.2.1075
Cinc CLI version: 5.6.16
Biome version: 1.6.821
Test Kitchen version: 3.6.0
Cookstyle version: 7.32.8
Cinc Client version: 18.6.2
Cinc Auditor version: 5.22.65
```

---
hideInToc: true
---

# Configure the Ruby environment (simple)

Configure the Ruby environment Cinc uses by adding the following commands to the configuration file for bash (~/.bashrc):

```bash
# Add the Cinc Workstation initialization content
echo 'eval "$(cinc shell-init bash)"' >> ~/.bashrc
```

Reload the config in the current shell with source ~/.bashrc or restart the current terminal.

Verify that the Ruby interpreter being used is the cin-workstation ruby instead of the system ruby:

```bash
% which ruby
/opt/cinc-workstation/embedded/bin/ruby
```

---
hideInToc: true
---

# Configure the Ruby environment (advanced)

```bash
mkdir -m 0755 ~/.bashrc.d
```

```bash
# User specific aliases and functions
if [[ -d ~/.bashrc.d ]]; then
  for rc in ~/.bashrc.d/*; do
    if [[ -f "$rc" ]]; then
      source "$rc"
    fi
  done
fi

unset rc
```

```bash
cat <<'EOF' > ~/.bashrc.d/100.cinc-workstation.sh
#!/bin/bash

eval "$(cinc shell-init bash)"
EOF
```

---
hideInToc: true
---

# Cinc Workstation Install - macOS

Download the latest version of the Cinc Workstation package from https://downloads.cinc.sh/files/stable/cinc-workstation/ for your version of macOS and CPU architecture.

Mount the DMG file and double-click the PKG file to start the install. The operation will warn you that it cannot verify the package. Click on the Done button. Then go to "System Settings > Privacy & Security". Scroll all the way down to view the "Security" section. You'll see that it says that the package was block to protect your Mac. Clcik on the "Open Anyway" button to run the install.

The bulk of the Cinc Workstation install is in /opt/cinc-workstation, with a few copies of the main program binaries in /usr/local/bin to be in the default PATH.

---
hideInToc: true
---

# Configure the Ruby environment (simple)

Configure the Ruby environment Cinc uses by adding the following commands to the configuration file for Z Shell (~/.zshrc):

```bash
# Load the zsh completion system - required to configure autocomplete
echo 'autoload -Uz compinit' >> ~/.zshrc
echo 'compinit' >> ~/.zshrc
# Add the Cinc Workstation initialization content
echo 'eval "$(cinc shell-init zsh)"' >> ~/.zshrc
```

Reload the config in the current shell with source ~/.zshrc or restart the current terminal.

Verify that the Ruby interpreter being used is the cinc-workstation ruby instead of the system ruby:

```bash
% which ruby
/opt/cinc-workstation/embedded/bin/ruby
```

---
hideInToc: true
---

# Configure the Ruby environment (advanced)

```bash
mkdir -m 0755 ~/.zshrc.d
```

```bash
# User specific aliases and functions
if [[ -d ~/.zshrc.d ]]; then
  for rc in ~/.zshrc.d/*; do
    if [[ -f "$rc" ]]; then
      source "$rc"
    fi
  done
fi

unset rc
```

```bash
cat <<'EOF' > ~/.zshrc.d/100.cinc-workstation.sh
#!/bin/bash

autoload -Uz compinit
compinit
eval "$(cinc shell-init zsh)"
EOF

```

---
hideInToc: true
---

# macOS Uninstall

To remove Cinc Workstation, run uninstall_chef_workstation (located in /usr/local/bin/uninstall_chef_workstation).


---
layout: section
---

# Setup Taste Tester

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

# Prerequisites - Ubuntu

```bash
sudo apt-get update
# to build rugged for taste-tester
sudo apt-get install pkg-config
# to build rugged extensions for taste-tester
sudo apt-get install cmake
# we need to install rugged with special openssl settings, or it will get
# linked against the system openssl and won't work properly
export OPENSSL_ROOT_DIR=/opt/cinc-workstation/embedded
```

---
hideInToc: true
---

# Prerequisites - macOS

```bash
# install xcode via the App Store (sorry!)
sudo xcodebuild -license accept
# install homebrew
brew install pkg-config
brew install cmake
brew install coreutils
```

---
hideInToc: true
---

# Install taste-tester gem

```bash
$ eval "$(cinc shell-init bash)"
$ which cinc
/opt/cinc-workstation/bin/cinc

cinc gem install taste_tester
```

---
hideInToc: true
---

```bash
sudo mkdir -p /usr/local/etc/taste-tester
# sudo cp \
  ~/github/boxcutter/boxcutter-chef-cookbooks/cookbooks/boxcutter_chef/files/taste-tester/taste-tester-plugin.rb \
  /usr/local/etc/taste-tester
# sudo cp \
  ~/github/boxcutter/boxcutter-chef-cookbooks/cookbooks/boxcutter_chef/files/taste-tester/taste-tester.conf \
  /usr/local/etc/taste-tester
```

---
hideInToc: true
---

```bash
sudo tee /usr/local/etc/taste-tester/taste-tester-plugin.rb <<'EOF'
def self.test_remote_client_rb_extra_code(_hostname)
  <<~EOF

    follow_client_key_symlink true
    client_fork false
    no_lazy_load false
    local_key_generation true
    json_attribs '/etc/cinc/run-list.json'
    ohai.critical_plugins ||= []
    ohai.critical_plugins += [:Passwd]
    ohai.critical_plugins += [:ShardSeed]
    ohai.optional_plugins ||= []
    ohai.optional_plugins += [:Passwd]
    ohai.optional_plugins += [:ShardSeed]
  EOF
end
EOF
```

---
hideInToc: true
---

```
sudo tee /usr/local/etc/taste-tester/taste-tester.conf <<EOF
repo File.join(ENV['HOME'], 'github', 'boxcutter', 'boxcutter-chef-cookbooks')
repo_type 'auto'
base_dir ''
cookbook_dirs ['cookbooks', '../chef-cookbooks/cookbooks']
# For now don't declare databag_dir - between meals seems to have a bug where it hardcodes debug_level=info
# databag_dir 'data_bags'
role_dir 'roles'
role_type 'rb'
chef_config_path '/etc/chef'
chef_config 'client.rb'
ref_file "#{ENV['HOME']}/.chef-cache/scale-taste-tester-ref.json"
checksum_dir "#{ENV['HOME']}/.chef-cache/checksums"
chef_client_command '/usr/local/sbin/chefctl -i'
use_ssl false
use_ssh_tunnels true
ssh_command '/usr/bin/ssh -o StrictHostKeyChecking=no'
chef_zero_path '/opt/cinc-workstation/bin/cinc-zero'
chef_zero_logging true
user ENV['USER']
plugin_path '/usr/local/etc/taste-tester/taste-tester-plugin.rb'
EOF
```

---
hideInToc: true
---

```bash
sudo mkdir -p /usr/local/etc/taste-tester
# sudo cp \
  ~/github/polymathrobotics/polymath-chef-cookbooks/cookbooks/polymath_chef/files/taste-tester/taste-tester-plugin.rb \
  /usr/local/etc/taste-tester
# sudo cp \
  ~/github/polymathrobotics/polymath-chef-cookbooks/cookbooks/polymath_chef/files/taste-tester/taste-tester.conf \
  /usr/local/etc/taste-tester
```

---
hideInToc: true
---

```bash
sudo tee /usr/local/etc/taste-tester/taste-tester-plugin.rb <<'EOF'
def self.test_remote_client_rb_extra_code(_hostname)
  <<~EOF

    follow_client_key_symlink true
    client_fork false
    no_lazy_load false
    local_key_generation true
    json_attribs '/etc/cinc/run-list.json'
    ohai.critical_plugins ||= []
    ohai.critical_plugins += [:Passwd]
    ohai.critical_plugins += [:ShardSeed]
    ohai.optional_plugins ||= []
    ohai.optional_plugins += [:Passwd]
    ohai.optional_plugins += [:ShardSeed]
  EOF
end
EOF
```

---
hideInToc: true
---

```
sudo tee /usr/local/etc/taste-tester/taste-tester.conf <<EOF
repo File.join(ENV['HOME'], 'github', 'polymathrobotics', 'polymath-chef-cookbooks')
repo_type 'auto'
base_dir ''
cookbook_dirs ['cookbooks', '../chef-cookbooks/cookbooks']
# For now don't declare databag_dir - between meals seems to have a bug where it hardcodes debug_level=info
# databag_dir 'data_bags'
role_dir 'roles'
role_type 'rb'
chef_config_path '/etc/chef'
chef_config 'client.rb'
ref_file "#{ENV['HOME']}/.chef-cache/scale-taste-tester-ref.json"
checksum_dir "#{ENV['HOME']}/.chef-cache/checksums"
chef_client_command '/usr/local/sbin/chefctl -i'
use_ssl false
use_ssh_tunnels true
ssh_command '/usr/bin/ssh -o StrictHostKeyChecking=no'
chef_zero_path '/opt/cinc-workstation/bin/cinc-zero'
chef_zero_logging true
user ENV['USER']
plugin_path '/usr/local/etc/taste-tester/taste-tester-plugin.rb'
EOF
```



---
layout: section
---

# Setup KVM/QEMU/Libvirt

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

# Configure bridged network with NetworkManager

```bash
# This is the network config written by 'subiquity'
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    eno1:
      dhcp4: false
      dhcp6: false
      optional: true
  bridges:
    br0:
      dhcp4: true
      dhcp6: false
      accept-ra: false
      link-local: []
      interfaces:
        - eno1
```

---
hideInToc: true
---

# Install QEMU/KVM on Ubuntu 24.04

Install QEMU/KVM and libvirtd

```bash
sudo apt-get update
sudo apt-get install qemu-kvm libvirt-daemon-system
sudo apt-get install virtinst
```

Make sure the current user is a member of the libvirt and kvm groups

```bash
$ sudo adduser $(id -un) libvirt
$ sudo adduser $(id -un) kvm
```

Be sure to reboot!!!

```bash
sudo reboot
```

---
hideInToc: true
---

# Create a definition for the host network (1 of 2)

Create XML definition

```bash
cat <<EOF > /tmp/host-network.xml
<network>
  <name>host-network</name>
  <forward mode="bridge"/>
  <bridge name="br0" />
</network>
EOF
```

Configure

```bash
$ sudo virsh net-define /tmp/host-network.xml
$ sudo virsh net-start host-network
$ sudo virsh net-autostart host-network
```

---
hideInToc: true
---

# Create a definition for the host network (2 of 2)

Verify

```bash
$ virsh net-list --all
 Name           State    Autostart   Persistent
-------------------------------------------------
 default        active   yes         yes
 host-network   active   yes         yes
```

---
hideInToc: true
---

# Image pool

```bash
$ virsh pool-define-as \
    --name default \
    --type dir \
    --target /var/lib/libvirt/images
$ virsh pool-build default
$ virsh pool-start default
$ virsh pool-autostart default
```

```bash
$ virsh pool-list --all
$ virsh vol-list --pool default --details
```

---
hideInToc: true
---

# cloud-init image pool

```bash
$ virsh pool-define-as \
    --name boot-scratch \
    --type dir \
    --target /var/lib/libvirt/boot
$ virsh pool-build boot-scratch
$ virsh pool-start boot-scratch
$ virsh pool-autostart boot-scratch
```

```bash
$ virsh pool-list --all
$ virsh vol-list --pool boot-scratch --details
```

---
hideInToc: true
---

# ISO image pool

```bash
$ virsh pool-define-as \
    --name iso \
    --type dir \
    --target /var/lib/libvirt/iso
$ virsh pool-build iso
$ virsh pool-start iso
$ virsh pool-autostart iso
```

```bash
$ virsh pool-list --all
$ virsh vol-list --pool iso --details
```

---
layout: section
---

# Create virtual machine

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

```bash
$ virsh nodeinfo
CPU model:           x86_64
CPU(s):              12
CPU frequency:       3916 MHz
CPU socket(s):       1
Core(s) per socket:  6
Thread(s) per core:  2
NUMA cell(s):        1
Memory size:         65627760 KiB
```

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           6.3G  2.4M  6.3G   1% /run
/dev/nvme2n1p2  1.8T   14G  1.7T   1% /
tmpfs            32G     0   32G   0% /dev/shm
tmpfs           5.0M   12K  5.0M   1% /run/lock
efivarfs        192K   70K  118K  38% /sys/firmware/efi/efivars
tmpfs            32G     0   32G   0% /run/qemu
/dev/nvme2n1p1  1.1G  6.2M  1.1G   1% /boot/efi
tmpfs           6.3G  112K  6.3G   1% /run/user/1000
/dev/sda1        58G   53G  5.0G  92% /media/automat/Ventoy
```

---
hideInToc: true
---

# Download cloud image template and resize

```bash
sudo apt get update
sudo apt-get install curl
```
```bash
curl -LO https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img
```

```bash
$ qemu-img info noble-server-cloudimg-amd64.img
$ sudo qemu-img convert \
    -f qcow2 -O qcow2 \
    noble-server-cloudimg-amd64.img \
    /var/lib/libvirt/images/ubuntu-server-2404.qcow2
$ sudo qemu-img resize -f qcow2 \
    /var/lib/libvirt/images/ubuntu-server-2404.qcow2 \
    64G
````

---
hideInToc: true
---

# Define login parameters for cloud-init ISO (1 of 2)

```bash
# Required for NoCloud module to function, uniquely identifies instance
cat >meta-data <<EOF
instance-id: ubuntu-server-2404
local-hostname: ubuntu-server-2404
EOF
```

---
hideInToc: true
---

# Define login parameters for cloud-init ISO (2 of 2)

```bash
# Main configuration script, tells cloud-init what to do when instance starts
cat >user-data <<EOF
#cloud-config
hostname: ubuntu-server-2404
users:
  - name: automat
    uid: 63112
    primary_group: users
    groups: users
    shell: /bin/bash
    plain_text_passwd: superseekret
    sudo: ALL=(ALL) NOPASSWD:ALL
    lock_passwd: false
chpasswd: { expire: False }
ssh_pwauth: True
package_update: False
package_upgrade: false
packages:
  - qemu-guest-agent
growpart:
  mode: auto
  devices: ['/']
power_state:
  mode: reboot
EOF
```

---
hideInToc: true
---

```bash
sudo apt-get update
sudo apt-get install genisoimage
```

```bash
$ genisoimage \
    -input-charset utf-8 \
    -output ubuntu-server-2404-cloud-init.img \
    -volid cidata -rational-rock -joliet \
    user-data meta-data

sudo cp ubuntu-server-2404-cloud-init.img \
  /var/lib/libvirt/boot/ubuntu-server-2404-cloud-init.iso
```

---
hideInToc: true
---

```bash
virt-install \
  --connect qemu:///system \
  --name ubuntu-server-2404 \
  --boot uefi \
  --memory 3096 \
  --cpu mode=host-passthrough \
  --vcpus 2 \
  --os-variant ubuntu24.04 \
  --disk /var/lib/libvirt/images/ubuntu-server-2404.qcow2,bus=virtio,cache=none,discard=unmap \
  --disk /var/lib/libvirt/boot/ubuntu-server-2404-cloud-init.iso,device=cdrom \
  --network network=host-network,model=virtio \
  --graphics none \
  --noautoconsole \
  --console pty,target_type=serial \
  --import \
  --debug
```

---
hideInToc: true
---

# Virtual machine console

```bash
# Command line console
virsh console ubuntu-server-2404

# Graphical console
virt-viewer ubuntu-server-2404
```

---
hideInToc: true
---

# Disable cloud-init and remove cloud-init ISO

In the guest:

```bash
# login with ubuntu user
$ cloud-init status --wait
status: done

# Disable cloud-init
$ sudo touch /etc/cloud/cloud-init.disabled
$ cloud-init status
status: disabled
$ sudo shutdown -h now
```

On the host
```bash
$ virsh domblklist ubuntu-server-2404
$ virsh change-media ubuntu-server-2404 sda --eject
$ sudo rm /var/lib/libvirt/boot/ubuntu-server-2404-cloud-init.iso
```

---
hideInToc: true
---

# Snapshots

```bash
$ virsh snapshot-create-as --domain ubuntu-server-2404 --name clean --description "Initial install"
$ virsh snapshot-list ubuntu-server-2404
$ virsh snapshot-revert ubuntu-server-2404 clean
$ virsh snapshot-delete ubuntu-server-2404 clean
```

# Cleanup
```bash
$ virsh shutdown ubuntu-server-2404
$ virsh undefine ubuntu-server-2404 --nvram --remove-all-storage
```

# Get IP of virtual machine
```bash
$ virsh domifaddr ubuntu-server-2404 --source agent
```

---
layout: section
---

# boxcutter chef-client

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

# boxcutter chef-client

```bash
# Login to the VM with automat/superseekret
virsh console ubuntu-server-2404
# login with automat/superseekret

# chefctl uses a shebang that points at /opt/chef, so make sure we have a link
# in place for compatibility
sudo mkdir -p /etc/cinc
# -n must be here in case /etc/chef already exists, otherwise
# it tries to create /etc/chef/cinc
# /etc/chef -> /etc/cinc
sudo ln -snf /etc/cinc /etc/chef

curl -L https://omnitruck.cinc.sh/install.sh | sudo bash
curl -L https://omnitruck.cinc.sh/install.sh | sudo bash -s -- -v 18.6.2


# /opt/chef -> /opt/cinc
sudo ln -snf /opt/cinc /opt/chef

## prime onepassword secret /etc/cinc/op_service_account_token
## Install 1Password cli
# sudo apt-get update
# sudo apt-get install unzip
# ARCH="<choose between 386/amd64/arm/arm64>"
# curl -o /tmp/op.zip https://cache.agilebits.com/dist/1P/op2/pkg/v2.30.3/op_linux_amd64_v2.30.3.zip
# sudo unzip /tmp/op.zip op -d /usr/local/bin/
# rm -f /tmp/op.zip
 
# op user get --me 

sudo tee /etc/cinc/client-prod.rb <<EOF
local_mode true
chef_repo_path '/var/chef/repos/boxcutter-chef-cookbooks'
cookbook_path ['/var/chef/repos/chef-cookbooks/cookbooks', '/var/chef/repos/boxcutter-chef-cookbooks/cookbooks']
follow_client_key_symlink true
client_fork false
no_lazy_load false
local_key_generation true
json_attribs '/etc/cinc/run-list.json'
ohai.critical_plugins ||= []
ohai.critical_plugins += [:Passwd]
ohai.optional_plugins ||= []
ohai.optional_plugins += [:Passwd]
EOF

sudo openssl genrsa -out /etc/cinc/client-prod.pem
sudo openssl genrsa -out /etc/cinc/validation.pem

sudo ln -sf /etc/cinc/client-prod.rb /etc/chef/client.rb
sudo ln -sf /etc/cinc/client-prod.pem /etc/chef/client.pem

sudo tee /etc/chef/chefctl_hooks.rb <<EOF
EOF

sudo tee /etc/chefctl-config.rb <<EOF
chef_client '/opt/cinc/bin/cinc-client'
chef_options ['--no-fork']
log_dir '/var/log/chef'
EOF

sudo tee /etc/chef/run-list.json <<EOF
{
  "run_list" : [
    "recipe[boxcutter_ohai]",
    "recipe[boxcutter_init]"
  ]
}
EOF

sudo apt-get install git
sudo mkdir -p /var/chef /var/chef/repos /var/log/chef
cd /var/chef/repos
sudo git clone https://github.com/boxcutter/chef-cookbooks.git \
  /var/chef/repos/chef-cookbooks
sudo git clone https://github.com/boxcutter/boxcutter-chef-cookbooks.git \
  /var/chef/repos/boxcutter-chef-cookbooks

sudo mkdir -p /usr/local/sbin
sudo curl -o /usr/local/sbin/chefctl.rb \
  https://raw.githubusercontent.com/facebook/chef-utils/main/chefctl/src/chefctl.rb
sudo chmod +x /usr/local/sbin/chefctl.rb
sudo ln -sf /usr/local/sbin/chefctl.rb /usr/local/sbin/chefctl

sudo touch /root/firstboot_os
echo "{\"tier\": \"robot\"}" | sudo tee /etc/boxcutter-config.json > /dev/null
sudo chefctl -iv

# Behind the scenes, it'sdoing this:
# /opt/cinc/bin/cinc-client -c /etc/cinc/client.rb -j /etc/chef/run-list.json
# /opt/cinc/bin/cinc-client --config /etc/cinc/client.rb --json-attributes /etc/chef/run-list.json
```

---
layout: section
---

# Polymath Robotics bootstrap

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

# polymathrobotics bootstrap

1. Make sure your ssh key is registered in Rippling: https://www.notion.so/polymathrobotics/Rippling-SSH-Public-Key-Management-4e36dabf29454e68b566ada3a7f0949a?pvs=4

2. Request a sandbox bootstrapping token from the #infra team, so that automation code can access secrets from 1Password. This will normally be a 1Password Service Account token with limited permissions to deploy to sandbox environments and not any production environments (sandbox tokens are limited tokens for testing).

3. Copy bootstrap_chef.sh to your target system. Most straightforward way until you learn more about our automation is to grab it from Github: https://github.com/polymathrobotics/polymath-chef-cookbooks/blob/main/cookbooks/polymath_chef/files/bootstrap_chef.sh


---
hideInToc: true
---

# polymathrobotics bootstrap

```bash
# Become root because the provisioning user will lose sudo privileges
sudo su -
export OP_SERVICE_ACCOUNT_TOKEN=<token_from_infra>

chmod +x bootstrap_chef.sh
./bootstrap_chef.sh

touch /root/firstboot_os
chefctl -i

# Once provisioned properly, remove firstboot cue
rm /root/firstboot_os
```
