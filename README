= IBM Tivoli Directory Integrator 7.1.1 Docker Container =

Suse 11.4 running IBM's TDI 7.1.1 also known as IBM Security Directory Integrator.
Runs the TDI Configuration Editor GUI when the container is started.
The build does not include TDI itself, which you must download from the IBM's software access catalog website.

== Building ==
This container is built from the scratcch, meaning that you need the OS first.

You can create Just enough OS image with SUSE Studio (https://susestudio.com), or do it using the included kiwi script. 'jeos-config.xml' contains JeOS configuration details.

Once you have the image convert it to a "file and folder" tgz by running 'sles2docker'

Download TDI 7.1.1 for Linux from IBM's Software access catalog. Save it to TDI_IDENTITY_E_V7.1.1_LIN-X86-64(CZUF3ML).tar
Download TDI 7.1.1 Fix Pack 5 from IBM's support site and save it to '7.1.1-TIV-TDI-FP0005.zip'
Now run 'build'

=== JeOS ===
If you are building your own "just enough os" note of the serveral packages are required:
* gawk, tar, libgtk-2_0-0 - to install and run TDI
* fastjar for jarring TDI adapters
* unzip for unzipping install media

Optional:
* vim for general editing/testing stuff from within a runing contianer

The size of the appliance could potentially be reduced by removing unneeded packages like this:
RUN rpm -ev --noscripts \
    bootsplash branding-SLES bridge-utils cron dhcpcd e2fsprogs elfutils file grub ifplugd initviocons iproute2 iputils irqbalance kbd kernel-default \
    kernel-default-base klogd lcms less libaio libasm1 libdw1 libelf1 libext2fs2 libgssglue1 libjpeg liblcms1 libmng libnet libnuma1 libtiff3 libtirpc1 \
    logrotate mdadm mkinitrd module-init-tools netcfg openldap2-client openssl-certs perl-Bootloader perl-satsolver perl-XML-Parser perl-XML-Simple postfix rpcbind suse-build-key \
    suse-sam suse-sam-data sysconfig sysfsutils syslog-ng tar timezone tunctl update-alternatives vim-base vlan xz
RUN zypper clean --all

and possibly other packages

== Running ==
Use the bundled 'run' command or
 docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v ~/Shared\ Folder/Docker:/opt/IBM/TDI/V7.1.1/workspace -u 1000 aiv/tdi711

When TDI starts it will prompt for the workplace location, just ok it.
TDI GUI is running nativelly via a shared X socket, no ssh is needed [http://stackoverflow.com/questions/25281992/alternatives-to-ssh-x11-forwarding-for-docker-containers/25334301#25334301].

The container is running under user developer that mimics your ID and group so that the shared volumes have the same permission structure

root password is Passw0rd, and so is the developer's password.

When ibmditk starts it creates the folder structure under /home/developer. To add server SSL certificates edit /home/developer/serverapi/testadmin.jsk keysotore. You can run java tools by adding them to the path
export PATH=$PATH:/opt/IBM/TDI/V7.1.1/jvm/jre/bin/

To add packages use susestudio to create a proper pacakge or add a OpenSUSE repos and update it from within the container. Note though that several opensuse updates break TDI.

RUN zypper ar http://download.opensuse.org/update/11.4/ Update
RUN zypper --gpg-auto-import-keys refresh -s
RUN zypper --non-interactive up

Other repos:
zypper ar http://download.opensuse.org/distribution/11.4/repo/oss/ OSS
zypper ar http://download.opensuse.org/source/distribution/11.4/repo/oss/ Src-OSS
zypper ar http://download.opensuse.org/repositories/openSUSE:/11.4:/Contrib/standard/ Contrib
zypper ar http://download.opensuse.org/repositories/Java:/packages/SLE_11/ Java

There are several GTK themes installed that you can use to change look-and-feel of TDI.
Set
GTK2_RC_FILES=/usr/share/themes/Glossy/gtk-2.0/gtkrc -  clearlooks with mac'y feel
GTK2_RC_FILES=/usr/share/themes/Gilouche/gtk-2.0/gtkrc -  washedout clearlooks
GTK2_RC_FILES=/usr/share/themes/ClearlooksClassic/gtk-2.0/gtkrc - simplified clearlooks
GTK2_RC_FILES=/usr/share/themes/Synchronicity/gtk-2.0/gtkrc - very washed out
GTK2_RC_FILES=/usr/share/themes/Glider/gtk-2.0/gtkrc -  square-ish
# other alternatives
#./themes/LargePrint/gtk-2.0/gtkrc
#./themes/LowContrastLargePrint/gtk-2.0/gtkrc
#./themes/ThinIce/gtk-2.0/gtkrc
#./themes/Redmond/gtk-2.0/gtkrc
#./themes/Simple/gtk-2.0/gtkrc
#./themes/LowContrast/gtk-2.0/gtkrc
#./themes/Crux/gtk-2.0/gtkrc
#./themes/Mist/gtk-2.0/gtkrc
#./themes/Industrial/gtk-2.0/gtkrc
