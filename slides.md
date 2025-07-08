---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
hideInToc: true
title: Chef Training
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

# Philosophy

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

# Meta/Facebook Chef cookbooks

- Facebook Chef cookbooks blog post - https://engineering.fb.com/2016/04/15/core-infra/facebook-chef-cookbooks/
- Scaling System Configuration at Facebook - https://www.youtube.com/watch?v=-YtZiVxEiJ8
- The Softer Side of DevOps https://www.youtube.com/watch?v=ry51Llzil1I]

---
hideInToc: true
---

# Design concepts

- Homogeneous vs. Hetereogeneous
  - JEOS boxes, xCAT - homogeneous
  - Chef - hetereogeneous - must have a way to express the delta in configuraton

---
hideInToc: true
---

# Hetereogeneous building blocks

- Service owners can own/adjust relevant settings
- Client based, otherwise you have to scale two systems instead of one
- Can produce the system you want in one pass (deterministic)
- Only make the necessary changes (idempotent)
- Easily extensible so it can be tied into other internal systems
- No dictated workflow (flexible)

---
hideInToc: true
---

# Configuration as Data

- Way to express configuration in a simple way, outside the configuration manageemnt code
  - In Chef, can use arrays and hashes in attributes
- Defaults are provided
- Engineers can munge the bits they want
- Various default and custom settings interpolated together when the configuration is materialized on a system

---
hideInToc: true
---

# Out of the box, Chef and Ansible support bad habits

- Most people hardcode their defaults making it difficult to share automation with other teams, much less other companies. Contract for consuming automation is not flexible.
- Multiple versions of the same code - version management too complicated and like branches
- Most examples limit themselves to a single action per run
- With ansible, most people don't converge on a schedule, just once when they set the machine up.
- Many examples don't limit the blast radius of change
- Don't promote deploying code everywhere at once instead of a subset (does not mean changes everywhere at once, just code)
- Can't easily test in production and revert

---
hideInToc: true
---

# Why Chef?

- Easier to see when there are problems with Chef itself
- Can change core assumptions of the tool with a small amount of code, when needed

---
hideInToc: true
---

# Workflow

- Provide API for anyone, anywhere to extend configs by munging data structures
- Engineers don't need to know what they're building on, just what they want to change
- Engineers can change their systems without fear of changing anything else
- Stable quality signal
- Testing needs to be easy (if testing isn't easy, people don't test)
- Branches are duplicate work, avoid
- Manage things in aggregate, not as individual pieces ("move idempotency up"). Managing 

---
hideInToc: true
---

# Configuration in aggregate

- Configure things as a group that gets handled all together, not individually
- Facilitates putting system into desired configuration in one pass
- Automates cleanup, avoids stale entries
- Managing things individually wastes people's time

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
sudo apt-get update
sudo apt-get install curl
```

```bash
mkdir ubuntu-server-2404
cd ubuntu-server-2404
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
packages: ['qemu-guest-agent']
growpart: { mode: auto, devices: ['/'] }
power_state: { mode: reboot }
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
  --vcpus 2 \
  --os-variant ubuntu24.04 \
  --disk /var/lib/libvirt/images/ubuntu-server-2404.qcow2,bus=virtio \
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
# login with automat user
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

# boxcutter chef-client - Install cinc client

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

# curl -L https://omnitruck.cinc.sh/install.sh | sudo bash
curl -L https://omnitruck.cinc.sh/install.sh | sudo bash -s -- -v 18.6.2

# /opt/chef -> /opt/cinc
sudo ln -snf /opt/cinc /opt/chef
```


---
hideInToc: true
---

```bash
## prime onepassword secret /etc/cinc/op_service_account_token
```

---
hideInToc: true
---

```bash
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
```

```bash
sudo openssl genrsa -out /etc/cinc/client-prod.pem
sudo openssl genrsa -out /etc/cinc/validation.pem

sudo ln -sf /etc/cinc/client-prod.rb /etc/chef/client.rb
sudo ln -sf /etc/cinc/client-prod.pem /etc/chef/client.pem
````

---
hideInToc: true
---

```bash
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
```

---
hideInToc: true
---

```bash
sudo apt-get update
sudo apt-get install git
```

```bash
sudo mkdir -p /var/chef /var/chef/repos /var/log/chef
cd /var/chef/repos
sudo git clone https://github.com/boxcutter/chef-cookbooks.git \
  /var/chef/repos/chef-cookbooks
sudo git clone https://github.com/boxcutter/boxcutter-chef-cookbooks.git \
  /var/chef/repos/boxcutter-chef-cookbooks
```

---
hideInToc: true
---

```bash
sudo mkdir -p /usr/local/sbin
sudo curl -o /usr/local/sbin/chefctl.rb \
  https://raw.githubusercontent.com/facebook/chef-utils/main/chefctl/src/chefctl.rb
sudo chmod +x /usr/local/sbin/chefctl.rb
sudo ln -sf /usr/local/sbin/chefctl.rb /usr/local/sbin/chefctl
```

---
hideInToc: true
---

```bash
sudo su -
touch /root/firstboot_os
echo "{\"tier\": \"minimal\"}" | sudo tee /etc/boxcutter-config.json > /dev/null
sudo chefctl -iv

# Behind the scenes, it's doing this:
# /opt/cinc/bin/cinc-client --config /etc/cinc/client.rb --json-attributes /etc/chef/run-list.json
# --config - specifies the configuration file
# --json-attributes - passes a JSON file containing node attributes, typically used for one-off runs
```

---
hideInToc: true
---

# Behind the scenes

```bash
# jq . /etc/chef/run-list.json
{
  "run_list": [
    "recipe[boxcutter_ohai]",
    "recipe[boxcutter_init]",
    "role[tier_minimal]"
  ]
}
```

---
hideInToc: true
---

# Behind the scenes

```bash
# less /etc/cinc/client.rb
local_mode true
chef_repo_path '/var/chef/repos/boxcutter-chef-cookbooks'
cookbook_path [
  '/var/chef/repos/chef-cookbooks/cookbooks',
  '/var/chef/repos/boxcutter-chef-cookbooks/cookbooks'
]
follow_client_key_symlink true
client_fork false
no_lazy_load false
local_key_generation true
json_attribs '/etc/cinc/run-list.json'
%w(
  attribute_changed_handler.rb
).each do |handler|
  handler_file = File.join('/etc/cinc/handlers', handler)
  if File.exist?(handler_file)
    require handler_file
  end
end
ohai.critical_plugins ||= []
ohai.critical_plugins += [:Passwd]
ohai.critical_plugins += [:ShardSeed]
ohai.optional_plugins ||= []
ohai.optional_plugins += [:Passwd]
ohai.optional_plugins += [:ShardSeed]

# these seem to incorrectly get set to /etc/cinc instead of /var/chef when
# local mode is used, confusing our chefctl config
file_backup_path File.join('/var/chef', 'backup')
file_cache_path File.join('/var/chef', 'cache')
```

---
layout: section
---

# Features of the boxcutter automation system

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

Tier is configured via `/etc/boxcutter_info.json`

```bash
apt-get update
apt-get install jq
```

```bash
$ jq . /etc/boxcutter-config.json
{
  "tier": "minimal"
}
```

---
hideInToc: true
---

Automation source is delivered to machines via git.

```bash
$ cd /var/chef/repos
$ ls -l
total 8
drwxr-xr-x 12 root root 4096 Jun 11 13:36 boxcutter-chef-cookbooks
drwxr-xr-x  8 root root 4096 Jun 11 13:32 chef-cookbooks
root@ubuntu-server-2404:/var/chef/repos#
```

---
hideInToc: true
---

```bash
$ less /etc/chef/chefctl_hooks.rb
module BoxcutterHook
  def pre_run(_output)
    unless ::File.exist?('/var/chef/repos/chef-cookbooks')
      Dir.chdir '/var/chef/repos' do
        Mixlib::ShellOut.new(
          'git clone https://github.com/boxcutter/chef-cookbooks',
          ).run_command
      end
    end
    unless ::File.exist?('/var/chef/repos/boxcutter-chef-cookbooks')
      Dir.chdir '/var/chef/repos' do
        Mixlib::ShellOut.new(
          'git clone https://github.com/boxcutter/boxcutter-chef-cookbooks',
          ).run_command
      end
    end
    [
      '/var/chef/repos/chef-cookbooks',
      '/var/chef/repos/boxcutter-chef-cookbooks',
    ].each do |repo|
      Dir.chdir repo do
        Chefctl.logger.info("Updating #{repo}")
        s = Mixlib::ShellOut.new('git fetch origin').run_command
        if s.error? return 'Failed to fetch git changes'
        s = Mixlib::ShellOut.new('git reset --hard origin/main').run_command
        if s.error? return 'Failed to update git repo'
      end
    end
  end
end

Chefctl::Plugin.register BoxcutterHook
```

---
hideInToc: true
---

# chefctl - Pluggable Chef controller

https://github.com/facebook/chef-utils/tree/main/chefctl

- System-level lock file
- Clean up old Chef processes
- Log file management
- Extensible hooks at all stages of the Chef run 
- Platform agnostic, tested to work on Linux, OSX and Windows

---
hideInToc: true
---

Chef-related timers

```bash
$ systemctl list-timers --all --output=json | jq -r '.[].unit'
```

- chef.timer - Runs `chefctl -q` every 15 minutes.
- cleanup_chef_logs.timer - Only keep 2 weeks of logs in `/var/log/chef`, run daily.
- remove_override_files.timer - Removes `/var/chef/cron.default.override` after an hour, controlled with `stop_chef_temporarily`
- taste-untester.timer` - Runs `/usr/local/sbin/taste-untester` to revert to production Chef an hour after `/etc/chef/test_timestamp` is last touched.
  
---
hideInToc: true
---

Chef is configured to run via chefctl every 15 minutes:

```bash
# cat /etc/systemd/system/chef.timer
# This file managed by chef.
# Local changes to this file will be overwritten.

[Unit]
Description=Run scheduled task chef

[Install]
WantedBy=timers.target

[Timer]
OnCalendar=*:0/15:0
AccuracySec=1s
Persistent=false
RandomizedDelaySec=0s
FixedRandomDelay=true
Unit=chef.service
```

---
hideInToc: true
---

Chef is configured to run via chefctl every 15 minutes:

```bash
# cat /etc/systemd/system/chef.service
# This file managed by chef.
# Local changes to this file will be overwritten.

[Unit]
Description=Run scheduled task chef
After=network.target

[Service]
Type=oneshot
Slice=system-timers-chef.slice
ExecStart=/bin/sh -c '/usr/bin/test -f /var/chef/cron.default.override -o -f /etc/chef/test_timestamp || /usr/local/sbin/chefctl -q'
TimeoutStopSec=90s
TimeoutStartSec=1d 6h
TimeoutStopSec=15m
```

---
hideInToc: true
---

Chef run logs are located in `/var/log/chef`:
- Current run: `cat /var/log/chef/chef.cur.out`
- Last run: `cat /var/log/chef/chef.last.out`

```bash
# ls -al /var/log/chef
total 184
drwxr-xr-x  2 root root     4096 Jun 11 13:45 .
drwxrwxr-x 11 root syslog   4096 Jun 11 13:37 ..
-rw-r--r--  1 root root     4381 Jun 11 13:33 chef.20250611.1333.1749648795.out
-rw-r--r--  1 root root     3483 Jun 11 13:35 chef.20250611.1335.1749648923.out
-rw-r--r--  1 root root   155905 Jun 11 13:37 chef.20250611.1336.1749649000.out
-rw-r--r--  1 root root       75 Jun 11 13:45 chef.20250611.1345.1749649501.out
lrwxrwxrwx  1 root root       47 Jun 11 13:45 chef.cur.out -> /var/log/chef/chef.20250611.1345.1749649501.out
-rw-r--r--  1 root root     3541 Jun 11 13:33 chef.first.out
lrwxrwxrwx  1 root root       47 Jun 11 13:37 chef.last.out -> /var/log/chef/chef.20250611.1336.1749649000.ou
```

---
hideInToc: true
---

Chef logs are retained for 2 weeks.

```bash
# cat /etc/systemd/system/cleanup_chef_logs.timer
# This file managed by chef.
# Local changes to this file will be overwritten.

[Unit]
Description=Run scheduled task cleanup_chef_logs

[Install]
WantedBy=timers.target

[Timer]
OnCalendar=*-*-* 01:01:00
AccuracySec=1s
Persistent=false
RandomizedDelaySec=0s
FixedRandomDelay=true
Unit=cleanup_chef_logs.service
```

---
hideInToc: true
---

Chef logs are retained for 2 weeks.

```bash
# cat /etc/systemd/system/cleanup_chef_logs.service
# This file managed by chef.
# Local changes to this file will be overwritten.

[Unit]
Description=Run scheduled task cleanup_chef_logs
After=network.target

[Service]
Type=oneshot
Slice=system-timers-cleanup_chef_logs.slice
ExecStart=/bin/sh -c '/usr/bin/find /var/log/chef -maxdepth 1 -name chef.2* -mtime +14 -exec /bin/rm -f {} \;'
TimeoutStopSec=90s
```

---
hideInToc: true
---

You can use the stop_chef_temporarily program to stop Chef runs (temporarily):

```bash
$ sudo /usr/local/sbin/stop_chef_temporarily 2
Stopped Chef for 2 hours. To re-enable 
 'rm -f /var/chef/cron.default.override'
```

---
hideInToc: true
---

```bash
# cat /etc/systemd/system/remove_override_files.timer
# This file managed by chef.
# Local changes to this file will be overwritten.

[Unit]
Description=Run scheduled task remove_override_files

[Install]
WantedBy=timers.target

[Timer]
OnCalendar=*:0/5:0
AccuracySec=1s
Persistent=false
RandomizedDelaySec=0s
FixedRandomDelay=true
Unit=remove_override_files.service
```

---
hideInToc: true
---

```bash
# cat /etc/systemd/system/remove_override_files.service
# This file managed by chef.
# Local changes to this file will be overwritten.

[Unit]
Description=Run scheduled task remove_override_files
After=network.target

[Service]
Type=oneshot
Slice=system-timers-remove_override_files.slice
ExecStart=/bin/sh -c '/usr/bin/find /var/chef/ -maxdepth 1 -name cron.default.override -mmin +60 -exec /bin/rm -f {} \;'
TimeoutStopSec=90s
```

---
hideInToc: true
---

Taste-untester set to run every 5 minutes

```bash
# cat /etc/systemd/system/taste-untester.timer
# This file managed by chef.
# Local changes to this file will be overwritten.

[Unit]
Description=Run scheduled task taste-untester

[Install]
WantedBy=timers.target

[Timer]
OnCalendar=*:0/5:0
AccuracySec=1s
Persistent=false
RandomizedDelaySec=0s
FixedRandomDelay=true
Unit=taste-untester.service
```

---
hideInToc: true
---

Taste-untester set to run every 5 minutes

```bash
# cat /etc/systemd/system/taste-untester.service
# This file managed by chef.
# Local changes to this file will be overwritten.

[Unit]
Description=Run scheduled task taste-untester
After=network.target

[Service]
Type=oneshot
Slice=system-timers-taste-untester.slice
ExecStart=/usr/local/sbin/taste-untester
TimeoutStopSec=90s
```

---
hideInToc: true
---

https://coderanger.net/two-pass/

---
layout: section
---

# chef-shell

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

```bash
tmux
```

https://hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/

```bash
ctrl+b % - split pane horizontally
ctrl+b " - split pane vertically
ctrl+b <arrow key>
ctrl+b c - create new window
ctrl+b n - next window
ctrl+b p - previous window
```

---
hideInToc: true
---

```bash
/usr/local/sbin/stop_chef_temporarily
eval "$(cinc shell-init bash)"
which ruby
cinc-zero

openssl genrsa -out local-zero.pem 2048
sudo tee knife.rb <<EOF
cookbook_path [
  '/var/chef/repos/chef-cookbooks/cookbooks',
  '/var/chef/repos/boxcutter-chef-cookbooks/cookbooks'
]
node_name 'local-zero'
client_key "#{File.dirname(__FILE__)}/local-zero.pem"  # dummy key
chef_server_url 'http://localhost:8889'
EOF

cd /var/chef/repos/chef-cookbooks/cookbooks
knife cookbook upload --all -c knife.rb
```

---
hideInToc: true
---

```bash
cinc-shell \
  --client \
  --server http://localhost:8889 \
  --config /etc/cinc/client.rb \
  --override-runlist 'recipe[boxcutter_ohai],recipe[boxcutter_init]'
```

Test Kitchen
```bash
cd /tmp/kitchen
cinc-shell c client.rb -j dna.json
```

---
hideInToc: true
---

```bash
cinc (18.7.10)> node.keys
 =>
["boxcutter_chef",
 "boxcutter_nfs",
 "boxcutter_docker",
 "boxcutter_onepassword",
```

```bash
> IO.popen('less', 'w') { |f| f.puts node.keys.sort }

def page(obj)
  IO.popen('less', 'w') { |f| f.puts obj }
end

page help
```

```bash
cinc (18.7.10)> node['virtualization']
 => {"systems"=>{"kvm"=>"guest"}, "system"=>"kvm", "role"=>"guest"}
cinc (18.7.10)> node['fqdn']
 => "ubuntu-server-2404"
```

```bash
node.attributes
node.methods
  node.automatic_attrs
  node.default_attrs
```

---
hideInToc: true
---

```bash
recipe_mode
resources
# Grab a single resource
resources("user[taylor]")
# Show the resource
puts resources("user[taylor]").to_text
exit
```

---
hideInToc: true
---

https://stevendanna.github.io/blog/2012/01/28/shef-debugging-tips-1/

https://discourse.chef.io/t/chef-event-handler-how-do-i-know-what-variables-classes-methods-etc-are-available-to-me/8758

https://github.com/chrisgit/chef-debugging

https://jtimberman.housepub.org/blog/2015/09/01/quick-tip-alternative-chef-shell-with-pry

https://jtimberman.housepub.org/blog/2014/07/21/load-current-resource-and-chef-shell

---
layout: section
---

# Taste tester

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
# install Xcode via the App Store (sorry, it does take a long time)
sudo xcodebuild -license accept
# install homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# Run these commands in your terminal to add Homebrew to your PATH:
    echo >> /Users/taylor/.zprofile
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/taylor/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"

brew install pkg-config
brew install cmake
brew install coreutils
```

---
hideInToc: true
---

# Install taste-tester gem

```bash
$ eval "$(conc shell-init bash)"
$ which cinc
/opt/cinc-workstation/bin/cinc

cinc gem install taste_tester
```

---
hideInToc: true
---

# Install patched between_meals

```bash
git clone https://github.com/facebook/between-meals.git
cd between-meals
cinc gem build between_meals.gemspec
cinc gem uninstall between_meals
cinc gem install between_meals-0.0.13.gem
```

---
hideInToc: true
---

```bash
sudo mkdir -p /usr/local/etc/taste-tester/boxcutter
# sudo cp \
  ~/github/boxcutter/boxcutter-chef-cookbooks/cookbooks/boxcutter_chef/files/taste-tester/taste-tester-plugin.rb \
  /usr/local/etc/taste-tester/boxcutter/
# sudo cp \
  ~/github/boxcutter/boxcutter-chef-cookbooks/cookbooks/boxcutter_chef/files/taste-tester/taste-tester.conf \
  /usr/local/etc/taste-tester/boxcutter/
```

---
hideInToc: true
---

```bash
sudo tee /usr/local/etc/taste-tester/boxcutter/taste-tester-plugin.rb <<'EOF'
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

```bash
sudo tee /usr/local/etc/taste-tester/boxcutter/taste-tester.conf <<EOF
repo File.join(ENV['HOME'], 'github', 'boxcutter', 'boxcutter-chef-cookbooks')
repo_type 'auto'
base_dir ''
cookbook_dirs ['cookbooks', '../chef-cookbooks/cookbooks']
databag_dir 'data_bags'
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
plugin_path '/usr/local/etc/taste-tester/boxcutter/taste-tester-plugin.rb'
EOF
```

---
hideInToc: true
---

```bash
cd ~/github/boxcutter/boxcutter-chef-cookbooks

export NODE_IP=10.63.33.23
taste-tester test \
  -s ${NODE_IP} \
  --user taylor \
  -c /usr/local/etc/taste-tester/boxcutter/taste-tester.conf
```

```bash
TLDR; Most common usage is:
  vi cookbooks/...              # Make your changes and commit locally
  taste-tester test -s [host]   # Put host in test mode
  ssh root@[host]               # Log on host
    chef-client                 # Run chef and watch it break
  vi cookbooks/...              # Fix your cookbooks
  taste-tester upload           # Upload the diff
  ssh root@[host]               # Log on host
    chef-client                 # Run chef and watch it succeed
  <Verify your changes were actually applied as intended!>
  taste-tester untest -s [host] # Put host back in production
                                #   (optional - will revert itself after 1 hour)

```

---
layout: section
---

# Ohai Integration

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

# Ohai plugin

https://docs.chef.io/ohai_custom/ 

---
layout: section
---

# Custom Handlers

<br>
<br>
<Link to="toc" title="Table of Contents"/>

---
hideInToc: true
---

# Handlers

- Handlers can hook events during the chef run
- Ruby code, loaded with `require` in the `client.rb`

---
hideInToc: true
---

# Event Handlers

- Use the Handler DSL to attach a callback to an event via the `on` method

```bash
Chef.event_handler do
  on :event_type do
    # some Ruby
  end
end
```

- List of event types: https://docs.chef.io/handlers/#event-types

---
hideInToc: true
---

# Attribute change handler

- Event handler that hooks the `:attribute_changed` event
- Used to troubleshoot/debug issues with attribute precedence
- Prints out where an attribute was set or overridden
- To not miss events, loaded via `client.rb` outside the Chef run
- Does no reporting, just prints to log

---
hideInToc: true
---

In `client.rb`
```bash
# /etc/chef/client.rb
require '/etc/cinc/handlers/attribute-change-handler.rb'
```

Attribute change handler:
```bash
# /etc/cinc/handlers/attribute-change-handler.rb
Chef.event_handler do
  on :attribute_changed do |precedence, keys, value| do
    # Since we just got called when an attribute was changed, we can wind
    # through the stack and look for a reference to our cookbook code. Dump
    # Grab the first reference highest in the stack - should be our attribute
    # setting code.
    stack_frame = caller.find { |line| line.include?('cookbooks/') }

    Chef::Log.debug("Attribute Changed: precedence=#{precedence}, keys=#{keys}, value=#{value}")
  end 
end
```

---
hideInToc: true
---

# Resource updated handler

- Event handler that hooks the `:resource_updated` event
- Records all resource updates
- Prints out details on the resource/source code location for a resource update
- To not miss delayed or indirect updates, avoids using  `run_status.updated_resources`, tracks updates manually

---
hideInToc: true
---

In `client.rb`
```bash
# /etc/chef/client.rb
require '/etc/cinc/handlers/resource-updated-handler.rb'
```

---
hideInToc: true
---
```bash
Chef.event_handler do
  resource_updates = []
  on :resource_updated do |resource, action|
    next if resource.nil?

    resource_updates << {
      name: "#{resource.resource_name}[#{resource.name}]",
      cookbook: resource.cookbook_name,
      recipe: resource.recipe_name,
      line: resource.source_line,
      action: action,
    }
  end

  on :run_completed do
    if resource_updates.empty?
      Chef::Log.info('No resources updated.')
    else
      Chef::Log.info("Updated #{resource_updates.size} resource(s):")
      resource_updates.each do |r|
        Chef::Log.info("  - #{r[:name]} (#{r[:cookbook]}::#{r[:recipe]} line #{r
[:line]}) via :#{r[:action]}")
      end
    end
  end
end
```
