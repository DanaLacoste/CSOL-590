## Forensic ToolKit (FTK) Imager

This is a tool which is no longer supported on anything but x86 (32/64) Windows.

The only Windows I have is a test VM running Windows 11 ARM.  Not really a good place to do Windows x86 low level device access!

So here we go!  I have translated the steps from the FTK Assignment and present my results below.

### Reference Note

I used two primary tools to make this work:

1. The Internet Archive's Way Back Machine lets you download web artifacts which are no longer available.  This was critical in obtaining the Mac / Linux versions of the FTK Imager tool.  https://web.archive.org/web/20140922052248/http://www.accessdata.com/support/product-downloads (Yes, from 2014, but the tools were last released in 2012: what are you gonna do?)  NOTE: YOU SHOULD ALWAYS COMPARE THE MD5 SIGNATURE OF RANDOM DOWNLOADS AGAINST THE [OFFICIAL VERSIONS PUBLISHED BY THE VENDOR](https://www.exterro.com/ftk-product-downloads/mac-os-10-5-and-10-6x-version-3-1-1).

2. [This blog post](https://bromiley.medium.com/imaging-with-apple-ftkimager-c529c174497a) proved to be both really well written and gets right to the point: the command line syntax that lets FTK-Imager do the right thing are not necessarily trivial.

## Installation

This tool is not really "installed" : it is just a single, simple binary which knows how to read block device data and translate it into file system structures which can produce forensic data for separate analysis.  Therefore you don't necessarily "install" it on Linux/Mac, you just download it and uncompress the package, make the result executable, and ta-da!

1. Obtain the software from the vendor: https://www.exterro.com/ftk-product-downloads is the official site, but you will see only Windows downloads.  On the left side, you can select the "Command Line" versions of the tool.  Note that only the support information and download MD5 signature remain on the official site.
2. Go to the Internet Archive for the original site (before AccessData became Exterro) and you can see the same content, but the download links will work: https://web.archive.org/web/20140922052248/http://www.accessdata.com/support/product-downloads and select the appropriate operating system/architecture version that you require from the "Command Line" versions section.
3. Uncompress the binary: you may have to additionally mark it executable with `chmod a+x ftkimage`

## Running the tool

Note: I performed this under MacOS 13.4 on an M1 Mac Mini: you may have to utilize different device (`/dev/`) names under Operating System/Architectures.

1. Find the mount name of your device.  The "Disk Utility" app in MacOS will show you the device for everything that is mounted.  In my case, I named the partition "CSOL-590" and I can see it clearly by running `diskutil list` (output below filtered to show only this disk)
   ```
   % diskutil list
   /dev/disk6 (external, physical):
      #:                       TYPE NAME                    SIZE       IDENTIFIER
      0:     FDisk_partition_scheme                        *123.0 GB   disk6
      1:               Windows_NTFS CSOL-590                123.0 GB   disk6s1
   ```
2. After you have prepared the files for your disk, you should not unmount the partition (but leave the USB drive connected) : `sudo diskutil umount /dev/disk6s1` (you need the `s1` here)
3. From the command line, you can now run the imager with your desired disk: `% sudo ftkimager /dev/disk6 CSOL-590.image --e01 --case-number CSOL590M2 --evidence-number CSOL590-01 --description "SanDisk_Ultra_128GB" --examiner "Dana Lacoste" --notes "Mod2 Ex2" --frag 1G --compress 7` (Note that I have a 128GB disk and multiple GB of data on it: if you have a smaller disk or less data, you probably want to correspondingly update the `--frag` parameter to be smaller.

## Conclusion

Many of the tools which do this "professionally" are many thousands of dollars to buy, and are sold almost entirely to governments and large enterprise organizations.  It is a good thing to have access to free tools which still work (this one has not been updated since 2012!)

## References

- Bromiley, M. (2014, October 16).  Imagine with Apple + FTKImager.  Medium.  Retrieved from https://bromiley.medium.com/imaging-with-apple-ftkimager-c529c174497a
