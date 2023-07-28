.. _proxmox:

Proxmox
=======

Proxmox Virtual Environment is a virtualization platform similar to :ref:`vmware` or :ref:`virtualbox`. You can read more about Proxmox VE at https://www.proxmox.com/en/proxmox-ve.

CPU
---

Proxmox defaults to a VM CPU which may not include all of the features of your host CPU. You may need to change this to ``host`` to pass through the host CPU type.

Display
-------

If you plan to use :ref:`networkminer` or other Mono-based applications in a Proxmox VM, then you may need to set the VM Display to ``VMware compatible (vmware)``.

NIC
---

If you're going to install Security Onion in Proxmox and sniff live network traffic, you may need to do some additional configuration in Proxmox itself (not the Security Onion VM). One option is to enable passthrough and pass the sniffing NIC through to the VM. For more information about Proxmox passthrough, please see:

https://www.servethehome.com/how-to-pass-through-pcie-nics-with-proxmox-ve-on-intel-and-amd/

https://pve.proxmox.com/wiki/PCI(e)_Passthrough

For other options, please see https://github.com/Security-Onion-Solutions/securityonion/discussions/8245.
