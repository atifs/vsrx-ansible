vsrx-ansible
============

vsrx-ansible is an [Ansible](https://github.com/ansible/ansible) playbook that's designed to build a Juniper VSRX-based network in a VMware Workstation or Fusion environment on Linux or OSX.  The goal was to create a PoC for automating a virtual network environment, but this project could be also used for labs, testing, or simulation.  It could be modified to work with vSphere/ESXi (VSRX's native environment), although you'd probably want to write Ansible modules to handle the VMware portion (99% of the effort).  Similar to other Ansible playbooks, this just provides one install scenario, but should save you HEAPS of time in figuring out how to do this your own way.

vsrx-ansible is also a base platform that could do just about **anything** you can imagine.  See the TODO file for ideas that will make your head spin. 

On Windows??? [rolr.io](https://rolr.io) will have a Ruby or Golang virtual network device configuration tool available soon!  Star the project at [github](https://github.com/rolr/rolr) to encourage rapid development :)

## Prerequisites / Notes

### Recommended System Specs

- 16GB+ RAM (although this will run with less - "30 Chrome tab" people like me will want 16)
- Core i5 or Core i7 (tested on first through fourth gen)
- SSD, although I run this with a hybrid SSD just fine on my laptop
- VSRX specs can be tuned down to run with less

### Software

- You'll need Linux, specifically Debian or RPM-based distributions
- ...or Mac OSX with [Homebrew](http://brew.sh/) or [MacPorts](http://macports.org) (coming very soon!)
- Requires [Ansible](https://github.com/ansible/ansible) 1.2 or newer
- Requires [ovftool](https://communities.vmware.com/community/vmtn/automationtools/ovf) for `ovftool`
- Requires [VMware VIX](http://www.vmware.com/support/developer/vix-api/), for `vmrun`
- [VMware Workstation 9](https://www.vmware.com/go/try-workstation) or [Fusion 5](https://www.vmware.com/go/tryfusion) - 30-day evals available for each!
- Installs [ISC dhcpd](http://www.isc.org/downloads/dhcp/) for your distro, because dnsmasq is quirky, and VMware's built-in dhcpd was buggy on my systems.  In the rare case you already run a non-VMware ISC dhcpd, you'll need to modify the playbooks.
- Installs *tftpd* for Junos [Autoinstallation](https://www.juniper.net/techpubs/en_US/junos12.1x45/topics/concept/security-autoinstallation-overview.html),  Your base configurations could also be installed on any other tftp/ftp/http location as well.
- vsrx-ansible uses `vmnet11-25` for intra-VSRX communication, and `vmnet10` for host-only communication with your desktop.  You're most likely using the default vmnets, and I picked higher numbers to avoid conflict.  These are arbitrary, and can be changed.

### Default Playbook Roles

- **`common`** - make the VM dirs (required for ovftool to work properly in this scenario)
- **`network`** - a very messy but working network configuration:
  - configure a known MAC address on each `Network Adapter 1` interface, aka `ge-0/0/0`
  - add four additional VM Networks (to the initial two), which map to `ge-0/0/2` through `ge-0/0/5`
  - create a full mesh between all of the devices on `ge-0/0/1` through `ge-0/0/5` (these don't have to be "live", and rely on Junos configuration to be operational)
- **`tftpd`** - instance to serve router configs
- **`ovftool-install`** - install VSRX from .OVAs
- **`vmx-mods`** - modifications to each .VMX file, "under the rug" section needs major work & testing
- **`dhcpd`** - provides DHCP for autoinstallation
- **`autoinstallation`**
  - copies router configs to tftp dir
  - will be used for config templating
  - deprecated VMware dhcpd section, could be handy if fixed
- **`superdebug`** (not enabled by default)
  - virtual serial port monitoring (installs `socat` - see upcoming docs on detailed usage instructions)
  - git repository creation in VM dirs and /etc/vmware - helpful!
- **`vix-vmrun`** - start the VSRXes
  - replicates VMware Workstation top toolbar functionality
  - may swap out with API tools at some point, but Ruby/Python/Perl would add to the requirements

### Getting started

1. Ensure you're not using `vmnet10` through `vmnet25` in VMware (there will be a fact check for this later)
2. Run VMware's Virtual Network Editor to create `vmnet10`, make it a host-only network of `192.168.69.0/24`, ensure "use local DHCP" is not checked, and "connect a host virtual adapter" is checked.
3. After that, modify `playbook/group_vars/all` in the project folder, patiently avoiding most changes for your first run (unless absolutely necessary for your environment).  Change to the 'playbook' directory, and run the following command:

    ansible-playbook -i ./hosts site.yml -vvv
    
 > Yes, it's that easy, assuming you didn't go and modify the playbook

4. For now, you'll just need to wait for the routers to come up.  I'll add better built-in notifications, e.g. libnotify and Growl.

5. It is recommended that you gracefully shutdown these VMs from Junos first, as there is no VMware Tools (client tools) functionality for VSRX... yet.  Once they're gracefully shutdown, use `stop.yml` for stopping them (or use Workstation client).  You can also use `pause.yml` to suspend them, and come back later.
 

### Next steps
- Devices are available at `192.168.69.201` through `192.168.69.206`
- Accessible via SSH, HTTPS, and Netconf over SSH
- **user**: `netdevops` **pass**: `netdevops1`
- You'll be able to add your SSH key to the playbooks shortly 

### Network diagram (virtual networks)

- IOU!

#### Other NetDevOps links

- [rolr.io](https://rolr.io) - a Ruby tool for NetDevOps, which is VSRX-friendly (by yours truly)
- [Juniper's Github](https://github.com/juniper)
- [Jeremy Schulman's Github](https://github.com/jeremyschulman) - also [Twitter](https://twitter.com/nwkautomaniac) and the excellent [Workflow Sherpas](http://workflowsherpas.com) blog.
- [My github](https://github.com/routelastresort) - star a project if you want me to work on it - that's how it goes (anything that's not a fork of another project)

### Contributing

vsrx-ansible is feature request-driven.  Push button & receive delicious bacon.

### Help!

Hit me up on [Twitter](https://twitter.com/routelastresort).  Use [Github's gist](https://gist.github.com) to share configs and logs

### Thanks

- Michael DeHaan, Tim Gerla, and the gang @ [AnsibleWorks](http://ansible.cc)
- [Jeremy Schulman](https://github.com/jeremyschulman) of Juniper Networks for the constant inspiration
- Matt Hite for OSX playbook testing, Jinja2, and generated MAC handling suggestions
- Your name, Twitter handle, website and/or blog could actually be listed here.  Just make a feature request, or contribute!

### TODO / Contribution ideas

See TODO.md!
