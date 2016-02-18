---
layout: post
title: "Saving Disks and Machines"
date: 2016-02-17 14:00:00
permalink: /blog/2016/02/17/
---

PCjs (v1.20.9) now offers some new, *much* easier ways to save disks and machines, thanks to the new
[Save Disk](/blog/2016/02/17/#saving-disks) and [Save Machine](/blog/2016/02/17/#saving-machines) features.
With one click, PCjs can now generate a single download containing everything you need to embed any of our
IBM PC demos on your own web page.

Saving Disks
---

Floppy disk images can now be saved to your desktop computer by simply clicking the **Save** button next
to the floppy disk controls.  Select the drive first, and then whatever diskette is shown as being "loaded"
in that drive will be saved in your local machine's Downloads folder when you click **Save**.

If you made any changes to that disk after it was loaded, those changes will be included, so if you want a pristine
copy of the disk, click the **Load** button first.  PCjs will ask you to confirm that you really want to reload the
disk and discard any changes.

Note that the disk *should* be downloaded as an **.img** file, which is nothing more than a sector-by-sector binary
dump of the disk.  For example, a 360Kb double-sided double-density (DSDD) disk contains 9 512-byte sectors on each
of the 40 tracks on each of its 2 sides, so when you **Save** a 360Kb disk, the downloaded file should be exactly
368,640 bytes large.

The Mac OS X operating system can automatically mount most **.img** files (from disks created by DOS 2.0 or later).
Older 1.x DOS disks, along with most non-DOS disks, do not have a BIOS Parameter Block (BPB) in the boot sector, so
most modern operating systems won't recognize the disk format.  Other operating systems, like Windows, may require
third-party software in order to mount an **.img** file, and some third-party software may prefer a different extension,
such as **.ima** or **.bin**.

It's also recommended that make your **.img** files *read-only*, so that if you do mount them on your desktop
computer, neither you nor the operating system will inadvertently modify the contents of the disk.  On OS X, this is
easily done with the **chmod** utility.

For example, if you saved the disk named "PC-DOS 2.00 (Disk 1)", it should have been downloaded as "PCDOS200-DISK2.img"
in your Downloads folder, so the OS X Terminal command `chmod -w PCDOS200-DISK2.img` will make it read-only, and
`chmod +w PCDOS200-DISK2.img` will make it writable again.

Saving Machines
---

Saving the entire state of any existing IBM PC machine is also possible with a single click.  You can choose to save
a machine in its initial state, or make changes to any of the machine's disks and then save it.  All your changes should
be preserved.

Under the bottom-left corner of any IBM PC on the PCjs [website](/), you should now see a
[**Save Machine**] link.  When you click that link, PCjs will generate a large "blob" of JavaScript containing
everything that machine needs to run, including:

 * The machine XML configuration file (eg, "machine.xml")
 * The machine XSL transformation file (eg, "components.xsl")
 * The machine CSS stylesheet file (eg, "components.css")
 * The machine state file (eg, "state.json")
 * The PCjs machine emulation script (eg, "pc.js")
 * Copies of all the disk images mounted by the machine

Let's say you want to save the IBM PC on the PCjs [home page](/).  When you click **Save Machine**, two things should
happen:

 * A file will be downloaded ("pc.json")
 * A popup will appear with some markup to copy-and-paste

The popup should provide the following information:

	Check your Downloads folder for "pc.json", copy it to your web server as "pc.js",
	and then add the following to your web page:

		<div id="ibm5150"></div>
		...
		<script type="text/javascript" src="pc.js"></script>
		<script type="text/javascript">embedPC("ibm5150","machine.xml","components.xsl");</script>
	
	The machine should appear where the <div> is located.

Rename the downloaded file from **pc.json** to **pc.js**, copy it to your own web server, then create or edit
a web page and insert the above text.  If **pc.js** and your web page are in different folders, then you'll also
need to change *src* to indicate the location of **pc.js**.

Some notes:

 * PCjs asks the browser to name the downloaded file **pc.json** instead of **pc.js** because a file with a ".js"
 extension may cause the Chrome web browser to block the download.  And not all browsers support named downloads;
 PCjs will attempt to open a new window/tab if that seems to be the case.
 
 * Your version of Chrome may also impose size limitations on the download.  If nothing downloads, the machine may
 be too large for your browser, so try a different browser (eg, Firefox or Safari).

 * If you have modified any floppy disks mounted by the machine *or* any of the machine's hard disks, those
 modifications should be saved along with the machine.
 
 * Any floppy disks mounted during the lifetime of the machine will be added to the machine's state.  So, for example,
 if you want your copy of the machine to include all the Windows 1.01 SDK disks, make sure you have loaded each disk
 once before clicking **Save Machine**.
 
 * Any floppy disks *not* mounted during the lifetime of the machine will be *removed* from the machine's list of
 available disks; we don't want machines running on other websites to be consuming our bandwidth.

 * The machine's current state, including memory and screen contents, are included as part of the saved
 machine state.  So, even if the original machine always powers on from scratch, the *copied* machine will always
 resume at the point it was saved.  This behavior, however, can be disabled by passing a *parms* object as the
 4th parameter to the *embedPC()* call, overriding the 'state' property:
 
		<script type="text/javascript">embedPC("ibm5150","machine.xml","components.xsl","{state:null}");</script>

While the [PCjs Documentation](/docs/pcjs/) explains how to create a *new* machine, by writing your own machine
XML file and manually copying all the other pieces, the new **Save Machine** feature is the best way to save
any *existing* IBM PC and embed it on any other website.

**WARNING**: While this feature is still "hot of the press," there will probably be some kinks to work out.  Every
browser seems to have its own idiosyncrasies in terms of what can be downloaded and/or how large it can be.  It's
also quite possible that certain machine features or modifications may not be properly preserved.

If you're having trouble with a particular browser or a particular machine, be sure to
[let me know](mailto:Jeff@pcjs.org), and then try another browser and/or machine.

*[@jeffpar](http://twitter.com/jeffpar)*  
*February 17, 2016*