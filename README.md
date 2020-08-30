# Ansible Playbook: Windows 3.11 VM on Uberspace

This playbook will set up an Uberspace to host a Windows 3.11 VM running a webserver on port 80 and make it available behind an NGINX service. QEMU will be used for the virtulizaton.

**You will need to provide your own HDD image for the VM.** It’s not included here, but the playbook will upload it for you if you specify the path.

## Run Playbook

```
ansible-playbook site.yml
```

This will prompt for the path to `c.img` (the HDD image for the VM). Alternatively, you can specify the path on the command-line to avoid the prompt:

```
ansible-playbook site.yml -e 'local_drive_c_img="/Users/aaron/Documents/Projekte/2020/Windows 3.11 Webserver/Image/c.img"'
```

**Note:** The single quotes are only required if your path contains spaces.

## VNC to Windows VM

Establish SSH tunnel to Uberspace:

```
ssh -L 5901:localhost:5901 win311.aaron.cc
```

Then use local VNC client and connect to `localhost:5901`. On macOS, this works with RealVNC (VNC Viewer.app), but not with the integrated screen sharing.

**Warning:** The mouse cursor won’t work properly—but with some practicing, you can manage. For newer operating systems, this can be fixed with `-usbdevice tablet`, but this requires USB which Windows 3.11 obviously doesn’t support.

