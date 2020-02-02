# Shadytel SIM Tools

This is a version of the Shadytel Shadysim tools modified and enhanced
for the use with the
[sysmoUSIM-SJS1 sim cards](https://osmocom.org/projects/cellular-infrastructure/wiki/SysmoUSIM-SJS1)

Contrary to the cards used by the original Shadtyel tools, the
sysmoUSIM-SJS1 have OTA security enabled and require the use of KIC/KID
to authenticate + encrypt the PDUs of the Remote Application Management
(RAM).

## GIT Repository

You can clone from the Osmocom sim-tools.git repository using

	git clone git://git.osmocom.org/sim/sim-tools.git

There is a cgit interface at <http://git.osmocom.org/sim/sim-tools/>

## Mailing List

Discussions related to sim-tools are happening on the
openbsc@lists.osmocom.org mailing list, please see
<https://lists.osmocom.org/mailman/listinfo/openbsc> for subscription
options and the list archive.

Please observe the [Osmocom Mailing List
Rules](https://osmocom.org/projects/cellular-infrastructure/wiki/Mailing_List_Rules)
when posting.


## Creating JavaCard STK Applets

Use the hello-stk example to get started.

	$ mkdir javacard
	$ cd javacard
	$ git clone https://git.osmocom.org/sim/sim-tools
	$ git clone https://git.osmocom.org/sim/hello-stk
	$ cd hello-stk
	$ make

To install the applet onto a SIM card, first set the type of reader you are using.

	# For PCSC readers:
    $ export SHADYSIM_OPTIONS="--pcsc"

	# For USB-serial readers:
    $ export SHADYSIM_OPTIONS="--serialport /dev/ttyUSB0"

and follow instructions at <https://osmocom.org/projects/cellular-infrastructure/wiki/Shadysimpy>

The shadysim tool has lots of other options.

    $ ./sim-tools/bin/shadysim --help


## install/delete toolkit command

       cd sim-tools/shady-sim

       # Load the applet
       python3 shadysim.py --pcsc -l /tmp/reset_sim.cap -i /tmp/reset_sim.cap \
          --enable-sim-toolkit --module-aid d07002ca00be5701 \
          --instance-aid d07002ca00be5701 \
          --nonvolatile-memory-required 0100 \
          --volatile-memory-for-install 0100 \
          --max-menu-entry-text 15 \
          --max-menu-entries 05 \
          --kic KIC1 \
          --kid KID1

       # Delete it
       python3 shadysim.py --pcsc -d d07002ca00be57 \
          --kic KIC1 \
          --kid KID1


       # install
       java -jar /tmp/gp.jar --key-enc KIC1 --key-mac KID1 --key-dek KIK1 --install /tmp/cp.cap

       # delete
       java -jar /tmp/gp.jar --key-enc KIC1 --key-mac KID1 --key-dek KIK1 --delete A00000015141434C
