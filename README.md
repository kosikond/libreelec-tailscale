# Tailscale on LibreELEC
Tailscale static x86_64 client binary on LibreELEC . If you need your 

LibreELEC (LE) has empheremeal root read only filesystem, only `/storage` is persistent. systemd service file from this repo reflects that and keeps the state config in `/storage/.config/tailscale/state` bind mounted into `/var/lib/tailscale/`

## Ansible playbook

As LibreELEC by default with Python 3.11 (as of 2023), you can use Ansible to take care of necessary steps to get the LE machine set up as Tailscale node. Playbook file contains couple of necessary tasks and assumes valid *Auth key* as variable `tailscale_auth_key`, and SSH enabled with default credentials on target machine. If you changed credentials or your inventory is different then adjust as needed.

1. Clone the repo
2. Obtain Auth key from https://login.tailscale.com/admin/settings/keys
3. Amend the `tailscale_auth_key` variable in `vars_file.yml` accordingly (including inventory.ini if you have more machines)
4. Execute playbook against your LE host:
 `ansible-playbook -i <host>, --extra-vars "ansible_user=<ssh_username> ansible_password=<ssh_password>" -f libreelec-tailscale.yml`

## TODO

- install notes
- Ansible playbook

## Notes for development environment for LibreELEC in KVM

By default LibreELEC is coming with SSH **disabled**, notes below are simple howto setup KVM guest for testing/dev with SSH enabled.

#### Requirements

- Virtualbox installed
- KVM machine with system QEMU

#### How to create LibreELEC KVM guest 

1. Download x86_64 .ova image from from https://libreelec.tv/downloads/generic/
2. Import .ova into Virtualbox
3. Add additional .vmdk storage drive to VM, give it at least 1GiB of RAM
4. Set graphics controller to VMSVGA and enable 3D acceleration
5. Boot the VM and run LibreELEC installer to install on the vmdk drive, reboot the VM
6. Setup the KODI instance and enable SSH (default user is `root` with password `libreelec`)
7. Power off VM. Export VM into another .ova appliance 
8. (copy over to KVM host)
9. Export vmdk with `tar xvf xxxxxx.ova`
10. Convert vmdk into qcow:  `qemu-img convert -f vmdk -O qcow2 xxxxxx.vmdk xxxxx.qcow2`
11. Setup you KVM guest as needed.
