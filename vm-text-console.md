# Setting Up a VM All Through CLI (Text Console)

First step is to skip Cockpit entirely and do the install over a serial text console instead of a web console.

## Prerequisites

Make sure you have `virsh`, `virt-install`, and dependencies installed. You'll also need an ISO image.

```
sudo dnf install @virtualization
```

## Make Sure libvirtd Is Enabled and Running

```
sudo systemctl enable --now libvirtd
```

## Create the VM
> RHEL
```
virt-install --name=your-vm-name --vcpus=2 --memory=2048 --disk size=20,format=qcow2 \
--location=/path/to/your/iso --os-variant=detect=on,name=generic --network=default --graphics none \
--console pty,target_type=serial --extra-args="inst.text console=ttyS0,115200n8" --noautoconsole
```
>Debian
```
virt-install --name your-vm-name --memory 2048 --vcpus 2 --disk path=/path/to/your/qcow2,size=20,format=qcow2 \
--location /path/to/your/iso,kernel=casper/vmlinuz,initrd=casper/initrd --os-variant=detect=on,name=generic \
--network network=default,model=virtio --graphics none --extra-args "console=ttyS0,115200n8 serial"
```

Once run, you should see the VM with `virsh`:

```
sudo virsh list --all
```

## Attach to the Install

```
virsh console your-vm-name
```

Walk through the installer as normal, right in your terminal. To detach without killing the VM, use `Ctrl+]` (control + closing square bracket).

## Reconnect Later

```
virsh console your-vm-name
```

Note: `--extra-args="console=ttyS0"` only works with installers that accept kernel boot args (most ISOs do). Fully graphical live ISOs may not support this and would need the Cockpit/VNC route instead.
