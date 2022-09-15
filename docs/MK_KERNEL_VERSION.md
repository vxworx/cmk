# mk_kernel_version

Checkmk plugin to monitor if running kernel mach (last) installed version.

Tested with:
- FreeBSD
- Debian/Ubuntu
- Proxmox

## cli cmd to get installed and running kernel versions
### FreeBSD
#### running kernel:

    sysctl -n kern.osrelease 
    13.1-RELEASE-p1

#### installed kernel

    freebsd-version -k 
    13.1-RELEASE-p2

### Debian/Ubuntu
#### running kernel:

    dpkg -l | grep linux-image-$(uname -r) | awk '{print $3}'
    5.4.0-47.51

#### installed kernel

    # Different kernel versions may be installed, so we need to compare.
    # All available versions to find the latest.

    # Get the version of the currently running kernel
    latest=$(dpkg -l | grep linux-image-$(uname -r) | awk '{print $3}')

    for i in $(dpkg -l | grep linux-image-$(uname -r | cut -d '.' -f1) | awk '{print $3}')
    do
        dpkg --compare-versions $i gt $latest && latest=$i
    done
    echo $latest
    5.4.0-47.51


## Install

### OMD Server Install
```
cp checks/mk_kernel_version /omd/sites/$(OMD_SITE)/local/share/check_mk/checks/
cp plugins/mk_kernel_version /omd/sites/$(OMD_SITE)/local/share/check_mk/agents/plugins/
```

### Client Install
```
cp plugins/mk_kernel_version /usr/lib/check_mk_agent/plugins/mk_kernel_version
```
