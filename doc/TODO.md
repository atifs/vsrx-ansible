# TODO.md #

Amazingly enough, anything that's not an Ansible module will be dead simple to make, and Ansible modules will save so much hair pulling that they may be the path of least resistance.  #NetDevOps hackers' delight, mmmm yes.

 - [ ] Refactoring/cleaning up (this is uber alpha)
 - [ ] Idempotency testing
 - [ ] Host facts checking, specifically use of vmnet10-25
 - [ ] Document vm networks + diagram
 - [ ] superdebug mode additional work (socat daemon and init scripts, aka virtual terminal concentrator)
 - [ ] NTP playbook w/dnsmasq, since there are no VMware Tools in Junos (and time sync is important)
 - [X] RPM-based distro support (check out the neat code for conditional OS handling)
 - [ ] jinja2 up some junos configuration templating
 - [ ] Templates for common features like RE protection, etc.
 - [ ] IPv6 is fine between devices, but this should probably be more IPv6-friendly
 - [ ] OSX / VMware Fusion support for Homebrew and MacPorts
 - [ ] Jinja2 vmx files and /etc files
 - [ ] Test suites!
 - [ ] ESXi/vSphere support, including building ESXi servers with [Cobbler](https://github.com/cobbler/cobbler) 
 - [ ] Matt Hite: pull generated MACs from fresh VMs instead of using static MACs
 - [ ] Playbook: install Juniper's Ruby gems for NetConf testing (Serial & SSH with Ruby and/or Ansible)
 - [ ] Playbook: isolated dev/stage/prod vmnet/dhcp for developing & testing router configurations
 - [ ] Playbook: append superdebug w/packet sniffer and/or other tools
 - [ ] Playbook: for automatically installing vmrun + ovftool
 - [ ] Playbooks for http or ftp roles for autoinstallation
 - [ ] ssh public key variable for netdevops account (for templates)
 - [ ] Ansible: replacement of ovftool and VIX with modules
 - [ ] Ansible: Workstation + Fusion networking facts module
 - [ ] Author's request: advanced OVF properties for a less complex process, once Juniper updates VSRX
 - [ ] I'd love to make a lab builder (number of devices, networks, specifics), but this functionality will most likely be developed directly in [rolr](https://rolr.io).