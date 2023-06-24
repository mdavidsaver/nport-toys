# Moxa UDP Protocol

Information in this file is not in any way informed or endorsed by Moxa.

Inferred from observing/inspecting an NPort 5110A.

UDP on port 4800.  May be sent by unicast or broadcast.

Basic message structure is an 8 byte header in MSBF,
a Device ID (with the exception of discovery),
and a variable payload (mostly in LSBF?).

## Header

```
0x8000_0000_0000_0000 - Direction: 0 - request, 1 - reply
0x7f00_0000_0000_0000 - Request ID
0x00ff_0000_0000_0000 - Status/flags?  Observed 0 on success, 0x04 on invalid request ID.  Ignored in requests?
0x0000_ffff_0000_0000 - Message length in bytes, including header.  Minimum value is 8
0x0000_0000_ffff_ffff - Transaction id?  Seems to be echoed from request to reply
```

## Device ID

With the exception of Request ID 1, header should be followed by
12 bytes which identify which device is being operated on.
Hereafter referred to as a "Device ID".
This case be copied from the payload of a response to a Request 1
into subsequent requests.

```
0xffff_ffff_0000_0000_0000_0000 - APID
0x0000_0000_ffff_0000_0000_0000 - HWID / Model in LSBF  Observed 0x511a for an NPort 5110A
0x0000_0000_0000_ffff_ffff_ffff - Ethernet MAC address
```

## Discovery

Request 0x01, Response 0x81

Request is header only.  No Device ID.

```
eg. 0x0100_0008_0000_0000
```

Response is header + Device ID + IP address in MSBF

```
eg. 0x8100_0018_0000_0000_0055_5555_1a51_0090_e8aa_aaaa_c0a8_7ffe
```

APID: 0x00555555
Model?: 0x1a51
MAC: 00:90:e8:aa:aa:aa
IP: 192.168.127.254

## Information

### Device Name

Request 0x10, Response 0x90.
Request is header and Device ID.
Response is header, Device ID, followed by device name as an ASCII string zero padded 40 bytes.

### IP Settings

Request 0x21, Response 0xa1.
Request is header and Device ID.
Response is header, Device ID, followed by IP address in MSBF.  (same as 0x81 response)

Request 0x22, Response 0xa2.
Request is header and Device ID.
Response is header, Device ID, followed by subnet mask in MSBF.

Request 0x23, Response 0xa3.
Request is header and Device ID.
Response is header, Device ID, followed by gateway IP in MSBF.

### Moxa Serial Number

Request 0x16, Response 0x96.
Request is header and Device ID.
Response is header, Device ID, followed by payload

```
0x0000000000000000ffff000000000000 - S/N in LSBF
```

### Other Information

A number of other requests produce positive responses with payloads which are not understood.


## Copyright

Copyright 2023 Michael Davidsaver

```
SPDX-License-Identifier: GFDL-1.3-or-later
```
