# Setting Up a VM All Through CLI

First step is to get access to your host machine via web console.

## Install Cockpit

```bash
sudo dnf install cockpit cockpit-machines
```

## Enable/Start Cockpit

```bash
sudo systemctl enable --now cockpit.socket
```

## Allow Cockpit Service via firewall-cmd

```bash
sudo firewall-cmd --add-service=cockpit --permanent
sudo firewall-cmd --reload
```

Now you should be able to access your host machine via web console.
In your browser, go to `https://your-hostname:9090` or `https://your-host-ip:9090`.

## Prerequisites

Make sure you have `virsh`, `virt-install`, and dependencies installed. You'll also need an ISO image.
```bash
sudo dnf install @virtualization
```

## Make Sure libvirtd Is Enabled and Running

```bash
sudo systemctl enable --now libvirtd
```

## Create the VM

```bash
virt-install --name=your-vm-name --vcpus=2 --memory=2048 --disk size=20,format=qcow2 --cdrom=/path/to/your/iso --os-variant=detect=on --network=default \
  --graphics vnc,listen=0.0.0.0 --noautoconsole
```

Once run, you should see the VM with `virsh`:

```bash
sudo virsh list --all
```

Now navigate back to the web console to complete the install:
`https://your-host-ip:9090`

Log in with your host credentials.

In the left pane you should see Virtual machines and from there you can console into the vm(s)
