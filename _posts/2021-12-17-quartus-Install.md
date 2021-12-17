---
layout: post
title:  "Quartus Lite 18.1 Installation"
date:   2021-12-17 15:55:00 +0000
categories: General
---

In one of my modules, I used the [Terasic DE10-Lite FPGA board](https://www.terasic.com.tw/cgi-bin/page/archive.pl?Language=English&CategoryNo=234&No=1021) for labs. 
This handy, little board can be programmed easily via a USB-A cable as it has
the Altera USB-Blaster programmer already. My students found it very useful to
verify their digital designs on board.

![DE10-Lite From Mouser.com](https://www.mouser.co.uk/images/marketingid/2016/microsites/132318264/terasic-de10-lite-board-layout.jpg "DE10-Lite image from mouser.com")

In this article, I will list the main steps to install the 
[Intel Quartus Lite](https://fpgasoftware.intel.com/?edition=lite) design software 
(version 18.1 that DE10-Lite originally asks for, despite newer versions are
known to work well) with ModelSim simulation tool and MAX-10 FPGA support.

First, download the following from the Intel website (link above):

1. `QuartusLiteSetup-18.1.0.625-windows.exe` (version may change) 
2. `ModelSimSetup-18.1.0.625-windows.exe` (version should match with Quartus Lite)
3. `max10-18.1.0625.qdz` (MAX-10 FPGA support files)

![Files Required](/images/QuartusInstall/1-Files_required.png)

Then start the installation by opening
`QuartusLiteSetup-18.1.0.625-windows.exe`. Choose the minimal set of components
to work with the DE10-Lite board, as shown.

![2](/images/QuartusInstall/2-Install_options.png)

Wait patiently till the installation finishes ...
![3](/images/QuartusInstall/3-Wait_toComplete.png)

Next, check the option to **launch USB Blaster II driver installation**.

![4](/images/QuartusInstall/4-Completed.png)

Accept the security warning and check the screen for a successful notice.

![5](/images/QuartusInstall/5-Drivers.png)

![6](/images/QuartusInstall/6-Drivers_done.png)

> Note: you may still need to manual locate the driver when the board is plugged
> in the first time to your PC/laptop. Check [this link](https://www.terasic.com.tw/wiki/Altera_USB_Blaster_Driver_Installation_Instructions) for steps.

When you run the software for the first time, you will be asked to buy a full
license. We will skip and run the basic lite version.

![7](/images/QuartusInstall/7-Run_first_time.png)

The installation is now complete. The design software is ready to compile
VHDL/Verilog designs and program them onto the FPGA boards.

![8](/images/QuartusInstall/8-Quartus_Window.png)

Next time, I will share an example of a simple design that demonstrate the basic
input and output devices available on the board.

