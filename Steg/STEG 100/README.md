<h1 align="center">STEG 100</h1>
  <p align="center">
     Introduction to Image Steganography
  </p>

### Table of contents

- [Introduction](#introduction)
- [Text Steg](#text-steg)
- [Image Steg](#image-steg)
	- [Metadata](#metadata)
	- [Hidden In Raw Data](#hidden-in-raw-data)
	- [LSB](#least-significant-bit)
- [Practice](#practice)
- [More Resources](#more-resources)
- [Creators](#creators)

## Introduction
#### `./background`
Steganography is the art of concealing information within something inocuous, so those who are not meant to see it would not know it is even there. 

The communication of secret information has been around for as long as humanity, and the methods of doing so have been constantly evolving. Usually, one would think of the transmission of secrets as a verbal act, such as whispering into a confidant’s ear or making sure you are alone before telling your secrets. But what if the two people can not be in close physical proximity? To address this issue, two main solutions were created: cryptography and steganography. The difference between them is that cryptography aims at concealing the *meaning* of a message while steganography aims to hide the *existence* of the message itself. To demonstrate this, let us use the secret message *`attack at dawn.`* One simple method of cryptography is called the Caesar Cipher, which takes a letter and shifts it down the alphabet a certain amount. If we set the shift at 13 letters, `“a”` becomes `“n”`, `“b”` becomes `“o”`, and so on. If we do this to every letter in our secret message, *`attack at dawn`* becomes *`nggnpx ng qnja`* As we can see, the meaning of the initial message has been concealed (only to a certain degree, as it can be deciphered), but not the existence of the message itself. If we want to conceal the *existence* of this message, we would use methods of steganography, such invisible ink. If we write *`attack at dawn`* with invisible ink on some document and someone intercepts it, they will not know that the document contains some sort of secret message. If the message gets to the intended receiver, they would know they must heat the paper to reveal the secret message written in invisible ink.

The word “steganography” comes from the Greek roots “steganos” and “-graphy” which together mean “covered writing.” Some of the oldest forms of steganography date back to the Greeks and Spartans, such as tattooing a message on a slave’s head and waiting for their hair to grow back or covering a message with a fresh layer of wax. As technology improved, so did the methods of concealing information, which we will discuss in this course. 

- - -
#### `./types-of-steganography`

There are several types of steganography, which are usually sorted by the medium in which the secret information is concealed. The main types of steganography are:
- image
- audio
- video
- text
- network

Image steg involves hiding information inside of another image, known as the "cover image." Text steg involves hiding informatin within a text file or some other body of text. Video and image steg are self-explanatory. Finally, network steg, also known as "protocol steganography", involves hiding information within network traffic.  We will discuss the basics of each type of steganography in this course.

## Text Steg

Text steganography is aimed at hiding a message within another body of text. There are three main categories: format-based, random and statistical generation, and linguistic steganography. We will discuss only format-based text steg, as the other two are a bit out of scope for this course. Format-based text steg modifies existing text with the insertion of spaces, misspellings, punctuation, and font changes. An example of this is inserting an extra space to represent the binary digit “0” and two extra spaces to represent the binary digit “1” into some body of text. Let us take a look at the following piece of text (extra spaces are replaced with a `+` for readability):<br><br>
**`This +is a ++normal ++message. ++Nothing +to +see ++here ++.`**<br><br>
If we count the extra spacing between the words and convert those to 1s and 0s respectively, we get `01110011`, which is the letter “s” in binary. This is one of the more obvious ways of hiding information in text, but there are much more sophisticated methods such as random and statistical generation and linguistic steganography, however those will not be discussed in this paper.

Text steganography is not the most common, so you will probably won't see it that often in CTFs, but they do show up. 


## Image Steg

Image steganography deals with hiding information inside of an image, which is known as the "cover image." This is one of the most popular forms of steganography, especially in CTFs. CTFs generally use the following methods of concealing information within an image: hiding it in the metadata, hiding it in the raw data, using image/pixel manipulation techniques (such as least-significant bit steganography), and embedding files within the main file (this is not only specific to images, however).
- - -
#### `./metadata`
Starting off super easy; hiding information in the metadata of an image. Metadata is an set of data desciribing important aspects of the image, such as copyright information, file statistics, descriptions, and other things such as GPS location data of where the image was taken, camera information, and so on (all of these are optional, you can remove all metadata from an image if you want to). On linux, there is a command-line tool called `exiftool` which can be used to inspect the metadata of files. You can follow along by using the `exif-raw-example.jpeg` file.

```console
$ exiftool exif-raw-example.jpeg
ExifTool Version Number : 12.16<br>
File Name : airport.jpeg<br>
Directory : .<br>
File Size : 4.1 MiB<br>
File Permissions : rwxrwxrwx<br>
File Type : JPEG<br>
Make : Apple<br>
Camera Model Name : iPhone 11<br>
Lens Make : Apple<br>
Lens Model : iPhone 11 back dual wide camera 4.25mm f/1.8<br>
Image Width : 4032<br>
Image Height : 3024<br>
Lens ID : iPhone 11 back dual wide camera 4.25mm f/1.8<br>
Create Date : 2020:10:27 19:31:26.671-04:00<br>
Date/Time Original : 2020:10:27 19:31:26.671-04:00<br>
Modify Date : 2020:10:27 19:31:26-04:00<br>
...
```

Metadata can contain useful information when figuring out a challenge, especially if it is part of an OSINT challenge and the metadata has GPS coordinates. Easy steganography challenges can put their flag inside of the metadata, so all you have to do to retrieve it is run `exiftool` or a similar tool on the provided file. For example:

```console
$ exiftool exif-raw-example.jpeg
...
Current IPTC Digest             : d41d8cd98f00b204e9800998ecf8427e
IPTC Digest                     : d41d8cd98f00b204e9800998ecf8427e
Comment                         : flag{ez_m0n3y}
Image Width                     : 4032
Image Height                    : 3024
...
```
- - -
#### `./hidden-in-raw-data`
If you want to look at the raw data of a file, the best way is through a tool that outputs the binary in hexadecimal representation such as `xxd` on linux. Here is some example output: 
```console
$ xxd exif-raw-example.jpeg 
00000000: ffd8 ffe1 343f 4578 6966 0000 4d4d 002a  ....4?Exif..MM.*
00000010: 0000 0008 000c 010f 0002 0000 0006 0000  ................
00000020: 009e 0110 0002 0000 000a 0000 00a4 0112  ................
00000030: 0003 0000 0001 0006 0000 011a 0005 0000  ................
00000040: 0001 0000 00ae 011b 0005 0000 0001 0000  ................
00000050: 00b6 0128 0003 0000 0001 0002 0000 0131  ...(...........1
00000060: 0002 0000 0007 0000 00be 0132 0002 0000  ...........2....
00000070: 0014 0000 00c6 0142 0004 0000 0001 0000  .......B........
00000080: 0200 0143 0004 0000 0001 0000 0200 0213  ...C............
00000090: 0003 0000 0001 0001 0000 8769 0004 0000  ...........i....
000000a0: 0001 0000 00da 0000 082c 4170 706c 6500  .........,Apple.
000000b0: 6950 686f 6e65 2031 3100 0000 0048 0000  iPhone 11....H..
000000c0: 0001 0000 0048 0000 0001 3134 2e30 2e31  .....H....14.0.1
000000d0: 0000 3230 3230 3a31 303a 3237 2031 393a  ..2020:10:27 19:
000000e0: 3331 3a32 3600 0024 829a 0005 0000 0001  31:26..$........
...
```

Some easy steganography challenges may attempt to hide a message by adding it to the raw data of an image. Sometimes this results in weird distorions in the actual image, but it can also be done in a way that does not impact the image whatsoever. There are two main ways to find hidden messages in data: manually looking through or grepping hexdump data, or using a tool called `strings`.

Using `xxd` with `grep` (assuming we know the flag starts with a "flag{" string value):
```console
$ xxd exif-raw-example.jpeg | grep -B 5 flag{
0000aa60: db00 00c0 7d64 6174 6100 0000 0001 0000  ....}data.......
0000aa70: 0009 0000 00ff ed00 3850 686f 746f 7368  ........8Photosh
0000aa80: 6f70 2033 2e30 0038 4249 4d04 0400 0000  op 3.0.8BIM.....
0000aa90: 0000 0038 4249 4d04 2500 0000 0000 10d4  ...8BIM.%.......
0000aaa0: 1d8c d98f 00b2 04e9 8009 98ec f842 7eff  .............B~.
0000aab0: fe00 1066 6c61 677b 657a 5f6d 306e 3379  ...flag{ez_m0n3y
--
00415b50: 7cca e791 5f06 681a 65d7 843e 2cc1 e0eb  |..._.h.e..>,...
00415b60: 82df d997 3320 2fb8 850a 7a03 8f7a fbf6  ....3 /...z..z..
00415b70: c7c7 1a35 c7c4 26d0 e671 0c1e 6101 73c1  ...5..&..q..a.s.
00415b80: c7e9 5caf c56f 00e8 5a9f 8c6c ae34 9996  ..\..o..Z..l.4..
00415b90: 2919 9dde 45e8 a074 e6ae 8d6d 3df1 37a1  )...E..t...m=.7.
00415ba0: ffd9 666c 6167 7b6e 3074 5f73 7573 7d0a  ..flag{n0t_sus}.
```

We see two flags, one of which we already saw from the metadata. This is because metadata is part of the raw data of the image, which you would usually find towards the top of the file. 

Using `strings` with `grep`:
```console
$ strings -n 5 exif-raw-example.jpeg | grep -B 5 flag{
TO H5
:kVg&
ax2:e
}data
8Photoshop 3.0
flag{ez_m0n3y}
--
w$Gk0<
=E~~j:
C)$`*
8/7w-
2:}):w
flag{n0t_sus}
```

`strings` will return any **printable** strings within a given file, which is helpful in trying to find a flag string hidden in file's data. This is an incredibly useful tool in reverse engineering as well for this very reason. 
- - - 
#### `./least-significant-bit`
The next level up in difficulty for image steganography involes the actual manipulation of the image itself. By this I mean changing the values of pixels in a certain way to encode information. One popular way is called **least-significant bit steganography** -- LSB for short.

Images, as with all data, are really just a collection of bits -- 1s and 0s. Eight of these bits comprise a byte, which in the case of a simple RGB image, will represent a color, either red, green, or blue (as you can see in the image below). The bit on the right end of a byte is called the least significant bit because it represents the smallest value, a change of 1 (circled on the image in brown). This is the exact premise that LSB steganography makes use of when attempting to hide information in images. If you can change the least significant bit of each byte of data without altering the overall data to a noticeable amount, then you can successfully hide information without getting noticed. 

<p align="center"><img src="https://github.com/ursusShooock/CTF-by-eldiablo/blob/main/images/steg/rgb-bytes.png" width=40%  height=40%></p>

An image is composed of pixels, which (in basic RGB images) are each three bytes of information, with each byte corresponding to either red, green, or blue. The value of each byte corresponds to the amount of each color in the composition of the overall color of the pixel. So if the values of the three bytes are `255, 0, 0`, then the pixel’s color will be 100% red. Let us use the pixel color we see in image below as an example. The decimal values for the pixel are `63, 212, 184` (for red, green, and blue respectively), which in binary are `00111111, 11010100, 10111000`.

<p align="center">
	<img src="https://github.com/ursusShooock/CTF-by-eldiablo/blob/main/images/steg/cover-pixel.png" width=10%  height=10%><br>
	<em>original color</em>
</p>

This pixel will act as our cover image in the following example of LSB steganography. Let us say that we want to hide a 3-bit secret message “010” in this pixel, or cover image. Our task would now be to replace the least significant bits of each byte with the bits of our secret message. Our first secret bit “0” will replace the least significant bit in the first byte of the pixel `00111111`, so this byte will become `00111110`. We complete this process for the remaining to bits and their corresponding bytes of the pixel. Our final result after replacing the least significant bits will be `00111110, 11010101, 10111000` which in decimal are the values `62, 213, 184`. These byte values correspond to the pixel color seen below.

<p align="center">
	<img src="https://github.com/ursusShooock/CTF-by-eldiablo/blob/main/images/steg/steg-pixel.png" width=10%  height=10%><br>
	<em>after steg</em>
</p>

Comparing the two pixels, there is practically no visible or noticeable difference between the two, so we have successfully encoded a secret message into this “image.” This is the basic process by which LSB steganography works when applied to larger secret messages and full-sized images. Although these changes might be invisible to the human eye, with enough hidden information, various steganalysis and statistical analysis techniques can start to detect the presence of LSB encoded data. However, there are still methods of LSB steganography that are more resilient to steganalysis. 

**Example:**
There is a secret message hidden in this image using LSB:
<p align="center">
<img src="https://github.com/ursusShooock/CTF-by-eldiablo/blob/main/Steg/STEG 100/lsb-chal-steg.png" width=50%  height=50%><br>
	<em>lsb-chal-steg.png</em>
</p>

There are several tools that I will demonstrate here. First, let's use https://stylesuxx.github.io/steganography. After uploading our file and pressing "Decode," we get the following output: 
```console
ZmxhZ3tpX3dhbnRfMl9iX3NpZ25pZmljYW50fQ==É%¶I$¶Ûm¶ÿí¶Km¶Ûd$ÛHI$HÛm¶Û-¶Ûm[m¶Ûm·ûmöÛm¶	$Ûm¶Ûm¶Im¶Ù-¶Ûm¶ÿý¶ÛmÛDÛ-¶Ûm¶Im¶ÉdKm¶ImIm¶Ù$Ûm@$Ûm¶I$[m¶Ûm¶Û$Km¶@$Ûm¶Ûm¶ÙdKm¶Ûm¶Ûm¶@$Kd	$-¶Ûm¶Ûm¶[e¶Ûm¶Ûm¶Ù-KmÿÛí¶Ù Û}·ÿÿþÛm¶Û-¶Ûm¶Ém²Ù-¶I$¶Ûm¶Ûm¶ÛdI,É Im¶Ûm¶Ûd$Ie¶Ûm¶Ûl¶ÛmI-ÿÿdöÛ%¶Im¶Ûd
```

We see that it starts with some normal looking characters which end in a `==`. This means that it is base64 encoded. Copy that into a base64 decoder of your choice (cyberchef or https://www.base64decode.org/ will work) and you should get **`flag{i_want_2_b_significant}`**. Yay! 

Let's try another method, cyberchef. Go to this link http://icyberchef.com/#recipe=Extract_LSB('R','G','B','','Row',0) and upload our image. We should see the same string, `ZmxhZ3tpX3dhbnRfMl9iX3NpZ25pZmljYW50fQ==` in the output.

Finally, let's try using [stegoveritas](https://github.com/bannsec/stegoVeritas), a linux tool. Follow the installation instructions from the github and you should be ready to roll. Run the following command `stegoveritas -extractLSB -red=0 -green=0 -blue=0 lsb-chal-steg.png` (the 0 just specifies thay we want to extract the 0'th bit, aka the LSB, of each byte) and it should output the extracted data to `results/LSBExtracted.bin`. If you examine the contents of that file, you should again see `ZmxhZ3tpX3dhbnRfMl9iX3NpZ25pZmljYW50fQ==`. 

Awesome! We just solved this easy LSB challenge with three different tools! More image steganography techniques will be introduced in the 200-level course.

As long as your cover file is large enough, you can encode any type of information within it, such as another image. There are many steganogrphy-related tools, most of which are compiled here: https://github.com/DominicBreuker/stego-toolkit

There is also:
- https://stylesuxx.github.io/steganography/
- http://icyberchef.com/#recipe=Extract_LSB('R','G','B','','Row',0)
- https://github.com/ragibson/Steganography#LSBSteg
- many, many more -- google is your friend!

Another way to learn is to write a program that actually does the LSB encoding and decoding. That's how I first got the hang of how it works. You can check out the code I had written while trying to learn here: https://github.com/NihilistPenguin/LSBstego. Please feel free to improve it, as it lacks normal functionality. Here are a few things you can do:
- make it so you can encode the secret message bits on more than the first row, aka as much of the image space it needs to take up
- allow encoding another image, so turn an image into bytes, encode it, then decode it, and re-create the image agian

I'm sure you can even clean up the main code as well with some super efficient python magic. Getting hands on and scripting things just for the heck of it will benefit you in the long run. 


## Practice:
- TCTF: TODO
- picoCTF: TODO
- TryHackMe: TODO

## More Resources:
- https://towardsdatascience.com/hiding-data-in-an-image-image-steganography-using-python-e491b68b1372
- https://itnext.io/steganography-101-lsb-introduction-with-python-4c4803e08041

Enjoy :metal:

