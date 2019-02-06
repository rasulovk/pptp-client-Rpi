# pptp-client-Rpi
VPN PPTP Client on Raspberry Pi

	

To start, you will need to install pptpclient, this can be achieved by:

sudo apt-get install pptp-linux

Next, Create a file in /etc/ppp/peers with arbitrary name and the following contents:

pty "pptp $VPNHOSTNAME --nolaunchpppd --debug"
name $USERNAME
password $PASSWORD
remotename PPTP
require-mppe-128
require-mschap-v2
refuse-eap
refuse-pap
refuse-chap
refuse-mschap
noauth
debug
persist
maxfail 0
defaultroute
replacedefaultroute
usepeerdns

Where $VPNHOSTNAME is your VPN host name, $PASSWORD is your VPN password and $USERNAME is your VPN username.

After you have done that, you should do:

sudo pon $FILENAME

where $FILENAME is the name of the file you saved earlier. Or you should do this to see debug messages (this command will display logs until connection off):

pon $FILENAME debug dump logfd 2 nodetach

To start your VPN client on boot, you can follow the instructions on http://pptpclient.sourceforge.net/howto-debian.phtml (point 8 or 9, Hand configuration section)

An alternate method to make your VPN client run on boot is to make a script /etc/init.d/pptp containing these contents:

#! /bin/sh

case "$1" in
  start)
sleep 10
pon $/etc/ppp/peers/FILENAME
    echo "PPTP Started"
    ;;
  stop)
    poff $/etc/ppp/peers/FILENAME
    echo "PPTP Stopped"
    ;;
  *)
    echo "Usage: /etc/init.d/pptp {start|stop}"
    exit 1
    ;;
esac

exit 0

Then run:

update-rc.d [filename of script] defaults

To make it run at startup.
