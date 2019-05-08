BEWARE!
=======

These images will get you back to stock, but according to
https://github.com/sowbug/archer-c9-back-to-stock/issues/4 you won't
be able to upgrade to anything else after that, including via
tftp. Don't use these images unless you're OK being on this single
stock version forever! (Note that you *can* reinstall dd-wrt or other
non-stock firmware, but for some reason these images are too picky
about stock images.)

Disclaimer: I have only an Archer C9 v3, not a v1 or v2, so I haven't tested the v1/v2 images included in this repository.

I just want to get my C9 back to stock. I don't care how you made these images
==============================================================================

1. Figure out which version of c9 you have.
2. Go to the DD-WRT admin tab.
3. *Read the warning* above about how you'll be permanently on this one stock version forever.
4. Flash the correct webflash image (c9v1-webflash.bin for v1, etc.).
5. Done

How to make the images yourself
===============================

Do the same thing I did below for whichever version of the router you have. Then go to the DD-WRT admin tab and flash c9-webflash.bin using the web UI. It'll take a few minutes, and in the end you'll have a stock Archer C9, without having to open the case.


v1
---

```
$ binwalk archer_c9v1_us-up-ver3-17-1-P1\[20180125-rel56387\].bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
155672        0x26018         LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 299376 bytes
234363        0x3937B         TRX firmware header, little endian, image size: 1728512 bytes, CRC32: 0xF4A39915, flags: 0x0, version: 1, header size: 28 bytes, loader offset: 0x1C, linux kernel offset: 0x0, rootfs offset: 0x0
234391        0x39397         LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 4240704 bytes
1962876       0x1DF37C        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 9178593 bytes, 771 inodes, blocksize: 131072 bytes, created: 2018-01-25 07:39:46

$ dd if=archer_c9v1_us-up-ver3-17-1-P1\[20180125-rel56387\].bin of=c9v1-loader.bin skip=234391 iflag=skip_bytes bs=1728485 count=1
1+0 records in
1+0 records out
1728485 bytes (1.7 MB, 1.6 MiB) copied, 0.00392719 s, 440 MB/s
$ dd if=archer_c9v1_us-up-ver3-17-1-P1\[20180125-rel56387\].bin of=c9v1-squashfs.bin skip=1962876 iflag=skip_bytes
17928+1 records in
17928+1 records out
9179453 bytes (9.2 MB, 8.8 MiB) copied, 0.0359788 s, 255 MB/s
$ ~/src/source-master/tools/firmware-utils/src/a.out -f c9v1-loader.bin -f c9v1-squashfs.bin > c9v1-webflash.bin
mjn3's trx replacement - v0.81.1
$ binwalk c9v1-webflash.bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             TRX firmware header, little endian, image size: 10911744 bytes, CRC32: 0x64CC457C, flags: 0x0, version: 1, header size: 28 bytes, loader offset: 0x1C, linux kernel offset: 0x1A6004, rootfs offset: 0x0
28            0x1C            LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 4240704 bytes
1728516       0x1A6004        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 9178593 bytes, 771 inodes, blocksize: 131072 bytes, created: 2018-01-25 07:39:46

$ shasum archer_c9v1_us-up-ver3-17-1-P1\[20180125-rel56387\].bin c9v1-loader.bin c9v1-squashfs.bin c9v1-webflash.bin
254ec5916cdd613c55cd6c26d2e64abc344495a8  archer_c9v1_us-up-ver3-17-1-P1[20180125-rel56387].bin
8044020e597ff5680c04fceceda66ac20181431c  c9v1-loader.bin
fb9f591bef55bc7a88ca93b8ef339db6a54fdd3c  c9v1-squashfs.bin
22e2d7d3a1b45da0348820f03a47f99a63aae08f  c9v1-webflash.bin
```

v2
---

source https://static.tp-link.com/res/down/soft/Archer_C9_V2_160315.zip

```
$ binwalk c9v2_un-up-ver4-0-0-P24\[20160315-rel34536\].bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
155672        0x26018         LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 299260 bytes
234269        0x3931D         TRX firmware header, little endian, image size: 1916928 bytes, CRC32: 0xFD45640D, flags: 0x0, version: 1, header size: 28 bytes, loader offset: 0x1C, linux kernel offset: 0x0, rootfs offset: 0x0
234297        0x39339         LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 4570816 bytes
2151198       0x20D31E        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 12505569 bytes, 2511 inodes, blocksize: 131072 bytes, created: 2016-03-15 01:17:48
14660496      0xDFB390        XML document, version: "1.0"
14668042      0xDFD10A        XML document, version: "1.0"
14669474      0xDFD6A2        Unix path: /var/run/appflow/tccpipe</listen_path>
14673514      0xDFE66A        Unix path: /usr/share/miniupnpd/firewall.include</path>
14677564      0xDFF63C        Unix path: /cover.jpg/AlbumArtSmall.jpg/albumartsmall.jpg/AlbumArt.jpg/albumart.jpg/Album.jpg/album.jpg/Folder.jpg/folder.jpg/Thumb.jpg/thu
14682556      0xE009BC        Unix path: /usr/local/bin/jiggle_firewall</exec>
14683402      0xE00D0A        Unix path: /usr/local/bin/apply_appflow</exec>

$ dd if=c9v2_un-up-ver4-0-0-P24\[20160315-rel34536\].bin of=c9v2-loader.bin skip=234297 iflag=skip_bytes bs=1916901 count=1
1+0 records in
1+0 records out
1916901 bytes (1.9 MB, 1.8 MiB) copied, 0.0051192 s, 374 MB/s
$ dd if=c9v2_un-up-ver4-0-0-P24\[20160315-rel34536\].bin of=c9v2-squashfs.bin skip=2151198 iflag=skip_bytes bs=12509298 count=1
1+0 records in
1+0 records out
12509298 bytes (13 MB, 12 MiB) copied, 0.0189874 s, 659 MB/s
$ ../lede-project-source/tools/firmware-utils/src/a.out -f c9v2-loader.bin -f c9v2-squashfs.bin > c9v2-webflash.bin
mjn3's trx replacement - v0.81.1
$ binwalk c9v2-webflash.bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             TRX firmware header, little endian, image size: 14430208 bytes, CRC32: 0xBAF67D77, flags: 0x0, version: 1, header size: 28 bytes, loader offset: 0x1C, linux kernel offset: 0x1D4004, rootfs offset: 0x0
28            0x1C            LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 4570816 bytes
1916932       0x1D4004        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 12505569 bytes, 2511 inodes, blocksize: 131072 bytes, created: 2016-03-15 01:17:48

$ shasum c9v2_un-up-ver4-0-0-P24\[20160315-rel34536\].bin c9v2-loader.bin c9v2-squashfs.bin c9v2-webflash.bin
b588c06fb1035bff22340d7475cf0ef7a1132b8a  c9v2_un-up-ver4-0-0-P24[20160315-rel34536].bin
c880be5648a71579880b0a633b7c6a50488333e4  c9v2-loader.bin
40dec777ae504f46d85ff78bdaf863011d9fad37  c9v2-squashfs.bin
a168a3808d52a920d734cc2d8e73f54a6f8e998e  c9v2-webflash.bin
```

Source for 170511 version: https://static.tp-link.com/Archer%20C9(UN)_V2_170511.zip

```
$ binwalk Archer\ C9\(UN\)_V2_170511.bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
155672        0x26018         LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 299316 bytes
234301        0x3933D         TRX firmware header, little endian, image size: 1916928 bytes, CRC32: 0x5AC9D15A, flags: 0x0, version: 1, header size: 28 bytes, loader offset: 0x1C, linux kernel offset: 0x0, rootfs offset: 0x0
234329        0x39359         LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 4570816 bytes
2151230       0x20D33E        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 13073427 bytes, 2671 inodes, blocksize: 131072 bytes, created: 2017-05-11 02:17:25
15225776      0xE853B0        XML document, version: "1.0"
15233457      0xE871B1        XML document, version: "1.0"
15234889      0xE87749        Unix path: /var/run/appflow/tccpipe</listen_path>
15238929      0xE88711        Unix path: /usr/share/miniupnpd/firewall.include</path>
15242979      0xE896E3        Unix path: /cover.jpg/AlbumArtSmall.jpg/albumartsmall.jpg/AlbumArt.jpg/albumart.jpg/Album.jpg/album.jpg/Folder.jpg/folder.jpg/Thumb.jpg/thu
15247985      0xE8AA71        Unix path: /usr/local/bin/jiggle_firewall</exec>
15248831      0xE8ADBF        Unix path: /usr/local/bin/apply_appflow</exec>

$ dd if=Archer\ C9\(UN\)_V2_170511.bin of=c9-loader.bin skip=234329 iflag=skip_bytes bs=1916901 count=1
1+0 records in
1+0 records out
1916901 bytes (1.9 MB, 1.8 MiB) copied, 0.00412536 s, 465 MB/s
$ dd if=Archer\ C9\(UN\)_V2_170511.bin of=c9-squashfs.bin skip=2151230 iflag=skip_bytes bs=13074546 count=1
1+0 records in
1+0 records out
13074546 bytes (13 MB, 12 MiB) copied, 0.0151488 s, 863 MB/s
$ ~/src/lede-project/tools/firmware-utils/src/a.out -f c9-loader.bin -f c9-squashfs.bin > c9v3-170511-webflash.bin
mjn3's trx replacement - v0.81.1
miket@siren-grit:~/src/archer-c9-back-to-stock$ shasum *.bin
669838435b4552c3ee60cf8a6758ff04b3236ae8  Archer C9(UN)_V2_170511.bin
3649170d575bf6663830c66f50ffcf4a58a11668  c9-loader.bin
f27a9d633c4a112ab5ea15ff24cbff010b9033f4  c9-squashfs.bin
7c3528754c2ba68bef8b0600321f6136cf9356ff  c9v3-170511-webflash.bin
```

v3
---

Go get the stock firmware from the [TP-Link site](https://static.tp-link.com/2018/201803/20180316/Archer%20C9(US)_V3_171215.zip). Then unzip it and cd into the resulting directory.

```
$ ls -l
total 14956
-rw-r--r-- 1 sowbug sowbug 14820016 Dec 15  2017  c9v4_eu-us-ca_up_1-3-1-20171215rel35219.bin
-rw-r--r-- 1 sowbug sowbug   112046 Sep 14  2015 'GPL License Terms.pdf'
-rw-r--r-- 1 sowbug sowbug   373590 Oct 14  2016 'How to upgrade TP-LINK Wireless AC Router.pdf'
$ binwalk c9v4_eu-us-ca_up_1-3-1-20171215rel35219.bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
155672        0x26018         LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 327460 bytes
241100        0x3ADCC         TRX firmware header, little endian, image size: 1916928 bytes, CRC32: 0xF7F42AEE, flags: 0x0, version: 1, header size: 28 bytes, loader offset: 0x1C, linux kernel offset: 0x0, rootfs offset: 0x0
241128        0x3ADE8         LZMA compressed data, properties: 0x5D, dictionary size: 65536 bytes, uncompressed size: 4562560 bytes
2158029       0x20EDCD        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 12413672 bytes, 2645 inodes, blocksize: 262144 bytes, created: 2017-12-15 01:46:56
14573322      0xDE5F0A        XML document, version: "1.0"
14581037      0xDE7D2D        XML document, version: "1.0"
14582469      0xDE82C5        Unix path: /var/run/appflow/tccpipe</listen_path>
14586651      0xDE931B        Unix path: /usr/share/miniupnpd/firewall.include</path>
14590726      0xDEA306        Unix path: /cover.jpg/AlbumArtSmall.jpg/albumartsmall.jpg/AlbumArt.jpg/albumart.jpg/Album.jpg/album.jpg/Folder.jpg/folder.jpg/Thumb.jpg/thu
14595966      0xDEB77E        Unix path: /usr/local/bin/jiggle_firewall</exec>
14596812      0xDEBACC        Unix path: /usr/local/bin/apply_appflow</exec>

# The 1916901 figure is 2158029 - 241128
$ dd if=c9v4_eu-us-ca_up_1-3-1-20171215rel35219.bin of=c9-loader.bin skip=241128 iflag=skip_bytes bs=1916901 count=1
1+0 records in
1+0 records out
1916901 bytes (1.9 MB, 1.8 MiB) copied, 0.00746417 s, 257 MB/s

# 12415293 is 14573322 - 2158029
$ dd if=c9v4_eu-us-ca_up_1-3-1-20171215rel35219.bin of=c9-squashfs.bin skip=2158029 iflag=skip_bytes bs=12415293 count=1
1+0 records in
1+0 records out
12415293 bytes (12 MB, 12 MiB) copied, 0.0318187 s, 390 MB/s

# This repo came from https://github.com/lede-project/source/, and then I hacked on it until
# I could `gcc trx.c`. I had to change to `#define TRX_MAX_LEN	0xF20000` to get it to handle the larger file sizes.
$ ~/src/source-master/tools/firmware-utils/src/a.out -f c9-loader.bin -f c9-squashfs.bin > c9v3-webflash.bin
mjn3's trx replacement - v0.81.1

# You can check and see whether your files turn out the same as mine.
$ shasum *.bin
ce5b20da5dc60a10935ad3f743f8d12bc9664aa2  c9-loader.bin
da1a28a6ab4b233dda104e7f7bcb00d55bee65e0  c9-squashfs.bin
71b876578be3d42446778633602689b4f1ed5afd  c9v4_eu-us-ca_up_1-3-1-20171215rel35219.bin
d362c4288a4b870d305d41180488f0bc88091192  c9v3-webflash.bin
```

v4 and v5
---

(I don't know why v4 and v5 are different, but binwalk reported only a JFFS2 filesystem, which I don't recognize. I don't know how to handle that.)
