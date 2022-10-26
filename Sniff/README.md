# CTF 2022 - Spanning Tree Qualifications: Sniff

## Description

## Attached files
- file.sr
- sdcard.img
- Images of the chip and motherboard

## Flag
```cyber_TpM_LpC_bUs_Sn1fFiNg```

## Solution

From the given files, we could find that first we had to inspect .sr file. We opened it with Pulseview and from the image of the chip we found that it was LPC chip.
After searching online for the layout of the pins we figured the correct order, but the LPC decoder was broken.

![unknown](https://user-images.githubusercontent.com/111431985/198047333-7d59afc4-84e9-41f9-b20b-681297c46b63.png)

We had to patch it, using the following link https://www.mail-archive.com/sigrok-devel@lists.sourceforge.net/msg04267.html
After the decoding process succesfully finished, we were able to extract hex data from the output.
We used the command

```cat patch5of5.txt | grep -A 5 'Cycle type: I/O read' | grep -A 4 'Address: 0x0024'    | sed -rne 's/.DATA: 0x(.)/\1/p' | xxd -p -r | strings -a -8```

to extract the passphrase. Passphrase was: **Ua5ynBsnxqpJi6MF**.
Then we had to extract files from .img.

```bsdtar xf sdcard.img``` 
```xzcat boot/initrd | cpio -i```

There was file crypt1.bin which was very suspicious. After inspecting it we have found that it was luks container and it could be opened with passphrase. 
```cryptsetup luksOpen ./crypt1.bin tempcrypt && mount /dev/mapper/tempcrypt /mnt```
In the mounted folder we found the flag and flag2, which was important for the next challenge.
