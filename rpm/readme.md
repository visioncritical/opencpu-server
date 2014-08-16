# OpenCPU rpm package

*Instructions for building OpenCPU as an rpm package.*

**warning**: Because Redhat systems do not support AppArmor, OpenCPU runs without the advanced security policies on these platforms. Instead, it runs in the standard SElinux `httpd_modules_t` context. This is fine for internal use, but it is not recommended to expose your Fedora/EL OpenCPU server to the web without further configuring SELinux for your application.

## Binary packages (Fedora only)

Ready-to-go rpm packages of opencpu are available for recent versions of Fedora [here](http://software.opensuse.org/download.html?project=home:jeroenooms:opencpu-1.4&package=opencpu).

## Building from source

Steps to build rpm packages on Fedora, CentOS or RHEL are in this script: [buildscript.sh](buildscript.sh).

## Debugging SELinux

If you get mysterious permission denied errors, the problem is most likely SELinux. See also this [blog post](https://www.opencpu.org/posts/opencpu-fedora-centos/). [SELinux for mere mortals](http://www.redhat.com/resourcelibrary/videos/selinux-for-mere-mortals) gives a nice introduction. To customize security policies for your needs, start by inspecting messages in `/var/log/messages` and `/var/log/audit/audit.log`. To get more verbose and readable logging, install the setroubleshoot packages:

    yum install setroubleshoot setroubleshoot-server

Most problems can be resolved by turning a SELinux "boolean" on or off. A boolean in SElinux is the term for a global flag that enables/disables a particular privilege within a particular context. The [httpd_selinux](http://linux.die.net/man/8/httpd_selinux) man page lists some important booleans for httpd that you might want to turn on/off. See also [this help page](https://docs.fedoraproject.org/en-US/Fedora/19/html/Security_Guide/sect-Managing_Confined_Services-The_Apache_HTTP_Server.html). To list all booleans available on your system:

    /usr/sbin/getsebool -a | grep httpd

SELinux can also be disabled completely by editing `/etc/selinux/config` and then rebooting.

## Firewall

On most standard installations of RHEL and CentOS, the default firewall configuration is to block HTTP/HTTPS traffic from external hosts. To open port 80 (HTTP) use something like:

    sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
    sudo service iptables save

Google is your friend.