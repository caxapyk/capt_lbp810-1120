# capt_lbp810-1120
Fork of Boichat Nicolas [http://www.boichat.ch/nicolas/capt/] Linux CAPT driver for Canon LBP810/LBP1120

Build PPD
---------
Be sure you Canon LBP810/LBP1120 on /dev/usb/lp0.
Otherwise change in capt.c line
	fd = open("/dev/usb/lp0", O_RDWR | O_NONBLOCK);
on
	fd = open("/dev/usb/lp<number>", O_RDWR | O_NONBLOCK);

# make

You need to have USB Printer support in your kernel. To install the
needed module, type, as root:
#sudo modprobe usblp

And a new device should appear (/dev/usb/lp0) or your lp<number>. Type, as root:
#sudo chmod a+rw /dev/usb/lp0

To give access to the printer to users (this is also needed for CUPS
to work.)

Install
-------

#sudo make install

Go to http://localhost:631/ and add local USB printer. Choose "Canon LBP-810 Foomatic/capt" driver.
Go to http://localhost:631/admin/#Miscellaneous and choose "AlwaysReset". 
This need to countinue printing after the first printer job. Otherwise you need to restart printer to print another file.

#sudo systemctl restart cups

When printing system must not send data to USB port.
#sudo vi /etc/cups/printers.conf

Replace somthing like
	DeviceURI usb://Canon/LASER%20SHOT%20LBP-1120?serial=0158030E
on
	DeviceURI serial:/dev/null

#sudo systemctl restart cups
