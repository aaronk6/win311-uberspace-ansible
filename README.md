# Ansible Playbook: Windows 3.11 on Uberspace

This playbook will set up an Uberspace to host a Windows 3.11 VM running a web server on port 80 and make it available behind a per-user NGINX service. QEMU will be used for virtualization.

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

Establish SSH tunnel to Uberspace (assuming your Uberspace is win311.example.com):

```
ssh -L 5901:localhost:5901 win311.example.com
```

Then use a local VNC client to connect to `localhost:5901`. On macOS, this works with RealVNC (VNC Viewer.app), but not with the integrated screen sharing app.

**Warning:** The mouse cursor won’t work properly—but with some practicing, you can manage. For newer operating systems, this can be fixed by adding `-usbdevice tablet` to the QEMU command line, but this requires USB support in the guest which Windows 3.11 obviously doesn’t have.

## Telnet to QEMU Monitor

Use this for comitting changes to `c.img`. By default, changes are discarded on exit (snapshot mode).

```
ssh -L 4444:localhost:4444 win311.example.com
```

Then locally:

```
telnet localhost 4444
commit ide0-hd0
```

Download image:

```
scp win311.example.cc:win311/c.img .
```

