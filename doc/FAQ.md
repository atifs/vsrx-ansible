# vsrx-ansible FAQ #

## Common Issues ##

- The most common issue involves making changes to the group_vars/all file, since the way Ansible, jinja2, and anything Python-related all work is somewhat new to me (string interpolation, regexp, how Ansible handles anything, etc.)
- Even if you have your DISPLAY environmental variable set, there may be issues with vmrun commands.  Launch a clean terminal session, and you can then run `vmrun start </path/to/vsrx.vmx>`, `vmrun stop </path/to/vsrx.vmx>`, etc.  I'll write a command wrapper to make starting/stopping devices easier someday.
- If you want to run headless (like Vagrant devices do by default), pass the `nogui` flag to vmrun in the ansible playbook, or the command line.  You *DO NOT* want to accidentally run VSRX instances with sudo/as root because they will be more hidden, and a rogue VSRX DHCP server can ruin your day, especially if it ended up on a bridged network in VMware.  Future versions will run headless, once I feel the infrastructure is stable enough.

## My networking is screwed up / service won't start

- Run VMware's Network Editor once.  This will fix 99.99% of network issues
- Run `ansible playbook -i ./hosts unbork.yml` from the `playbook` directory.  This will reset your vmnet10-25 networks.
- Keep in mind that any time you run Virtual Network Editor and save, it could cause vsrx-ansible to break, and should be followed by an unbork.

## I'm a heavy VMware Workstation/Fusion user ##

- It's more than safe to say that you should not run this until it's more mature, specifically the management of vmnet adapters... unless you're very familiar with how the components work.  VMware's desktop products are poorly-documented, not conducive to automation, and are probably the reason that VirtualBox is so popular.  You could always move your regular VMs to VirtualBox, or install this on any desktop other than your own.

## OH MY GOD, THE HORROR... THAT... NETWORK SECTION... and VMX MODS... ##

- Yeah, I may have cleaned this up by the time you read this.  Pull/redownload the project ;)  I'm getting better at advanced Ansible by the minute, I promise.  I'm working on other things, and I'm still motivated to clean house, even though everything screams "way easier to refactor in Ruby".

## How can I get you to implement X feature?

- Star the project, and open an issue.  I like stars.

## Naming ##

- This is called vsrx-ansible, because it will look nice next to vsrx-chef, vsrx-puppet, and vsrx-salt.... :)