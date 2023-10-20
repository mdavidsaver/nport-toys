# Discover Moxa NPort devices

`nport-scan` discovers the presence of Moxa NPort serial terminal servers.
Tested with NP5110A.  May detect other NPort models.

`np5110a-config` uploads a previously downloaded (and edited) NP5110A configuration file and restarts.

The contents of this repository are not in any way informed or endorsed by Moxa.

While it is not believed to be able to change settings in an NPort, use is at your own risk.

## nport-scan

By default (`--probe`) only some specific known queries are made.

```
$ ./nport-scan -q
{
    "apid": 214223535,
    "model": "511a",
    "ethernet": "00:90:e8:aa:bb:cc",
    "source": "192.168.127.254:4800",
    "device_name": "NP5110A_1234",
    "address": "192.168.127.254",
    "netmask": "255.255.0.0",
    "gateway": "192.168.0.1",
    "firmware_version": 17170432,
    "sn": 1234
}
```

When trying to understand an unknown/unsupported NPort variant,
`--scan` will query all possible information in raw hex form.

```
$ ./nport-scan -q --scan
{
    "apid": 214223535,
    "model": "511a",
    "ethernet": "00:90:e8:aa:bb:cc",
    "replies": {
        "2": "8200001400000001afcac40cd2040090e8aabbcc",
...
```

## np5110a-config

Acts to automatically upload/import a previously manually (and edited) downloaded backup configuration file.

```sh
./np5110a-config http://192.168.127.254 path/to/NP5110A_9762.txt
```

Requires the Selenium web browser automation tool, with its WebKitGTK backend.

On RHEL9

```sh
sudo dnf install python3-selenium webkit2gtk3-devel
```

On Debian 11

```sh
sudo apt-get install python3-selenium webkit2gtk-driver
```

## Interpreting firmware_version

The decimal firmware version number reported by `nport-scan`
is best understood in hexidecimal form.
eg. `17170432 == 0x1060000`
This can be compared with:

The number shown on the web UI:

![screen capture](docs/nport5110a-fw-version.png)

The contents of a downloaded Backup file:

```
[Basic Information (not changeable)]
...
Firmware version=0x1060000
```
