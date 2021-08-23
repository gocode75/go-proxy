# Basics

Modern Unix-like operating systems offer a centralized mechanism for finding and installing software. Software is usually distributed in the form of packages, kept in repositories. Working with packages is known as package management. Packages provide the basic components of an operating system, along with shared libraries, applications, services, and documentation.

While their functionality and benefits are broadly similar, packaging formats and tools vary by platform:

| Operating System      | Format        | Tool(s)                       |
| :---                  | :----:        |  ---:                         |
| Debian	            | .deb	        | apt, apt-cache, apt-get, dpkg |
| Ubuntu	            | .deb	        | apt, apt-cache, apt-get, dpkg |
| CentOS	            | .rpm	        | yum                           |
| Fedora	            | .rpm	        | dnf                           |
| FreeBSD	            | Ports, .txz	| make, pkg                     |

## Yum Variables

You can use and reference the following built-in variables in yum commands and in all Yum configuration files (that is, /etc/yum.conf and all .repo files in the /etc/yum.repos.d/ directory):

**$releasever**
You can use this variable to reference the release version of Red Hat Enterprise Linux. Yum obtains the value of $releasever from the distroverpkg=value line in the /etc/yum.conf configuration file. If there is no such line in /etc/yum.conf, then yum infers the correct value by deriving the version number from the redhat-release-server package. The value of $releasever typically consists of the major release number and the variant of Red Hat Enterprise Linux, for example 6Client, or 6Server.

**$arch**
You can use this variable to refer to the system's CPU architecture as returned when calling Python's os.uname() function. Valid values for $arch include i686 and x86_64.

**$basearch**
You can use $basearch to reference the base architecture of the system. For example, i686 machines have a base architecture of i386, and AMD64 and Intel 64 machines have a base architecture of x86_64.

**$YUM0-9**
These ten variables are each replaced with the value of any shell environment variables with the same name. If one of these variables is referenced (in /etc/yum.conf for example) and a shell environment variable with the same name does not exist, then the configuration file variable is not replaced.

To define a custom variable or to override the value of an existing one, create a file with the same name as the variable (without the “$” sign) in the ```/etc/yum/vars/``` directory, and add the desired value on its first line.

For example, repository descriptions often include the operating system name. To define a new variable called $osname, create a new file with “Red Hat Enterprise Linux” on the first line and save it as /etc/yum/vars/osname:

```
~]# echo "Red Hat Enterprise Linux" > /etc/yum/vars/osname
```

Instead of *“Red Hat Enterprise Linux 6”*, you can now use the following in the ```.repo``` files:

```
name=$osname $releasever
```

## Plugins

Yum provides plug-ins that extend and enhance its operations. Certain plug-ins are installed by default. Yum always informs you which plug-ins, if any, are loaded and active whenever you call any yum command. For example:

```
~]# yum info yum
Loaded plugins: product-id, refresh-packagekit, subscription-manager
[output truncated]
```

### fastestmirror
The fastest mirror plugin is designed for use in repository configurations where you have more than 1 mirror in a repo configuration. It makes a connection to each mirror, timing the connection and then sorts the mirrors by fastest to slowest for use by yum.

Refer to the [link](https://wiki.centos.org/PackageManagement/Yum/FastestMirror)

# Commands

* yum -qf yum.conf (query file)
* yum -qi yum-3.2.22-39.el5.centos (query information)

* yum search nmap
* yum provides nmap
* yum provides all nmap  <-- matches all strings in description
* yum install nmap
* yum info nmap
* yum grouplist
* yum groupinstall Tomboy

* cd /etc; ls yum*

* cd yum.repos.d/
* cat CentOS-Base.repo
    - name of the CentOS-$releasever
    - mirrorlist 
* cd ..

* cat yum.conf
   * location of cachedir for yum cache
   * logging location and level

## Configure YUM Repository in Linux Step By Step 

YUM stand for (Yellowdog Updater Modified) is an opensource command line package management tool which is available for Redhat and other Linux systems. Yum allow system administrator to install, update and uninstall packages from linux systems. YUM Repositories contains the Linux RPM package.YUM Repositories hold the RPM package files and enable download and installation of new software on our systems. YUM Repositories can hold RPM package files locally i.e on local disk or remotely like on FTP or HTTP.

System default yum repos are in **/etc/yum.repos.d/** with extension **repo**

* Update from local repo
* mkdir /mylocalrepo1 
* mount redhat DVD on Linux system (mount /dev/cdrw /mnt/)
* cd /mnt
* copy all the contents to local directory (/mylocalrepo1) cp -rv * /mylocalrepo1/

To create and initialize local yum repository, we need create-repo package on our system
* rpm -ivh /mnt/Packages/createrepo-0.9.9-18.el6.noarch.rpm

Note that you need to install dependencies manually as we are using rpm as of now.

Yum uses a simple database that help to keep information of all packages and derivative packages to speed up installation. Initialization step creates the database and create the directory to work as repository (e.g. mylocalrepo1).

Creating the local yum repository using
* createrep -v /mylocalrepo1/Packages

Configure local yum repository to be used for installation
* rename default repositories
  ** cd /etc/yum.repos.d/
  ** mv Centos-Base.repo Centos-Base.repo.old
  ** similar rename for all .repo

* cd /etc/yum.repos.d/
* vim mylocalrepo1.repo
[mylocalrepo1]
name=mylocalrepo1 Repository
baseurl=file:///mylocalrepo1/Packages
gpgcheck=0
enabled=1

* yum clean all  <-- clears cache
* yum repolist
* yum install telnet


Even simpler way is to follow red-hat instructions [here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/ch-yum#:~:text=across%20package%20upgrades.-,8.1.4.%C2%A0Upgrading%20the%20System%20Off-line%20with%20ISO%20and%20Yum,-For%20systems%20that)


# Glossary

| Acronymn      | Full Form                     |
| -----------   | -----------                   |
| apt           | Advanced Packaging Tool       |
| yum           | Yellowdog Updater Modified    |
| RPM           | Red-hat Package Manager       |

# References

* [Package Management Basics: apt, yum, dnf, pkg](https://www.digitalocean.com/community/tutorials/package-management-basics-apt-yum-dnf-pkg)
* Redhat website - Installing and Managing Software [Yum](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/ch-yum)

* Package Management with RPM and YUM [Youtube](https://www.youtube.com/watch?v=pPRLLcF7KRU)
* How to Configure YUM Repository in Linux Step By Step [Youtube](https://www.youtube.com/watch?v=hw2xRj-lkmg)
