Beware!! I have only an Archer C9 v3, not a v1, so I haven't tested the v1 image included in this repository.

# First, go get the stock firmware from the [TP-Link site](https://static.tp-link.com/2018/201803/20180316/Archer%20C9(US)_V3_171215.zip).
# Then unzip it and cd into the resulting directory.

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

# Now go to the DD-WRT admin tab and flash c9-webflash.bin using the web UI.
# It'll take a few minutes, and in the end you'll have a stock Archer C9, without having to open the case.

===

## v1

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

===

## v4 and v5

(I don't know why v4 and v5 are different, but binwalk reported only a JFFS2 filesystem, which I don't recognize. I don't know how to handle that.)
