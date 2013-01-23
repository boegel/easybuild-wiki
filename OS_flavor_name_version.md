We need a Python function in EasyBuild to figure out OS flavor/name/version. 

This wiki page collects outputs of the Python script below, to see whether this is sufficient/acceptable as a way of figuring out OS details.

## Script

Just start a `python` interpreter, and copy-paste the code below:

```python
import platform
import sys

def linux_distribution():
  try:
    return platform.linux_distribution()
  except:
    return "N/A"

print("""Python version: %s
dist: %s
linux_distrubution: %s
system: %s
machine: %s
platform: %s
uname: %s
version: %s
mac_ver: %s
""" % (
sys.version.split('\n'),
str(platform.dist()),
linux_distribution(),
platform.system(),
platform.machine(),
platform.platform(),
platform.uname(),
platform.version(),
platform.mac_ver(),
))
```

## Outputs

### CentOS

#### CentOS 5.7

```
Python version: ['2.4.3 (#1, Sep 21 2011, 19:55:41) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-51)]']
dist: ('redhat', '5.7', 'Final')
linux_distrubution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-274.12.1.el5-x86_64-with-redhat-5.7-Final
uname: ('Linux', 'nfs.gaia-cluster.uni.lux', '2.6.18-274.12.1.el5', '#1 SMP Tue Nov 29 13:37:46 EST 2011', 'x86_64', 'x86_64')
version: #1 SMP Tue Nov 29 13:37:46 EST 2011
mac_ver: ('', ('', '', ''), '')
```

#### CentOS 5.8

```
Python version: ['2.4.3 (#1, Jun 18 2012, 08:55:23) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-52)]']
dist: ('redhat', '5.8', 'Final')
linux_distrubution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-308.16.1.el5xen-x86_64-with-redhat-5.8-Final
uname: ('Linux', 'support', '2.6.18-308.16.1.el5xen', '#1 SMP Tue Oct 2 22:50:05 EDT 2012', 'x86_64', 'x86_64')
version: #1 SMP Tue Oct 2 22:50:05 EDT 2012
mac_ver: ('', ('', '', ''), '')
```

#### CentOS 5.9

```
Python version: ['2.4.3 (#1, Jan  9 2013, 06:47:03) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-54)]']
dist: ('redhat', '5.9', 'Final')
linux_distrubution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-308.4.1.el5xen-x86_64-with-redhat-5.9-Final
uname: ('Linux', 'test-centos-1', '2.6.18-308.4.1.el5xen', '#1 SMP Tue Apr 17 17:49:15 EDT 2012', 'x86_64', 'x86_64')
version: #1 SMP Tue Apr 17 17:49:15 EDT 2012
mac_ver: ('', ('', '', ''), '')
```

### Debian

#### Debian 6 Squeeze

```
fgeorgatos@gaia-10:~/arena/checkosversion$ python runme.py 
Python version: ['2.7.3 (default, Dec 18 2012, 07:01:29) ', '[GCC 4.6.3]']
dist: ('debian', '6.0.6', '')
linux_distrubution: ('debian', '6.0.6', '')
system: Linux
machine: x86_64
platform: Linux-2.6.32-5-amd64-x86_64-with-debian-6.0.6
uname: ('Linux', 'gaia-10', '2.6.32-5-amd64', '#1 SMP Sun Sep 23 10:07:46 UTC 2012', 'x86_64', '')
version: #1 SMP Sun Sep 23 10:07:46 UTC 2012
mac_ver: ('', ('', '', ''), '')
```

### Fedora

#### Fedora 16

```
Python version: ['2.7.3 (default, Jul 24 2012, 11:41:40) ', '[GCC 4.6.3 20120306 (Red Hat 4.6.3-2)]']
dist: ('fedora', '16', 'Verne')
linux_distrubution: ('Fedora', '16', 'Verne')
system: Linux
machine: x86_64
platform: Linux-3.6.7-4.fc16.x86_64-x86_64-with-fedora-16-Verne
uname: ('Linux', 'ca60c171', '3.6.7-4.fc16.x86_64', '#1 SMP Tue Nov 20 20:33:31 UTC 2012', 'x86_64', 'x86_64')
version: #1 SMP Tue Nov 20 20:33:31 UTC 2012
mac_ver: ('', ('', '', ''), '')
```

### Red Hat Enterprise Linux (RHEL)

### RHEL 5.6

```

Python version: ['2.4.3 (#1, Dec 10 2010, 17:24:35) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-50)]']
dist: ('redhat', '5.6', 'Tikanga')
linux_distrubution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-238.el5-x86_64-with-redhat-5.6-Tikanga
uname: ('Linux', 'xcat1', '2.6.18-238.el5', '#1 SMP Sun Dec 19 14:22:44 EST 2010', 'x86_64', 'x86_64')
version: #1 SMP Sun Dec 19 14:22:44 EST 2010
mac_ver: ('', ('', '', ''), '')
```



### Scientific Linux (SL)

#### SL 5.2

```
Python version: ['2.4.3 (#1, May  5 2011, 18:44:23) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-50)]']
dist: ('redhat', '5.2', 'Boron')
linux_distrubution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-194.3.1.el5xen-x86_64-with-redhat-5.2-Boron
uname: ('Linux', 'ui10', '2.6.18-194.3.1.el5xen', '#1 SMP Fri May 7 02:05:32 EDT 2010', 'x86_64', 'x86_64')
version: #1 SMP Fri May 7 02:05:32 EDT 2010
mac_ver: ('', ('', '', ''), '')
```

#### SL 5.4

```
Python version: ['2.4.3 (#1, Jun 18 2012, 09:40:07) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-52)]']
dist: ('redhat', '5.4', 'Boron')
linux_distrubution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-308.24.1.el5xen-x86_64-with-redhat-5.4-Boron
uname: ('Linux', 'ui01', '2.6.18-308.24.1.el5xen', '#1 SMP Tue Dec 4 17:16:08 EST 2012', 'x86_64', 'x86_64')
version: #1 SMP Tue Dec 4 17:16:08 EST 2012
mac_ver: ('', ('', '', ''), '')
```


#### SL 5.6

```
Python version: ['2.7.3 (default, Nov 14 2012, 12:23:28) ', '[GCC 4.6.3]']
dist: ('redhat', '5.6', 'Boron')
linux_distrubution: ('Scientific Linux SL', '5.6', 'Boron')
system: Linux
machine: x86_64
platform: Linux-2.6.18-194.11.4.el5-x86_64-with-redhat-5.6-Boron
uname: ('Linux', 'login01', '2.6.18-194.11.4.el5', '#1 SMP Tue Sep 21 06:46:41 EDT 2010', 'x86_64', 'x86_64')
version: #1 SMP Tue Sep 21 06:46:41 EDT 2010
mac_ver: ('', ('', '', ''), '')
```

#### SL 5.8

```
Python version: ['2.4.3 (#1, Feb 21 2012, 15:02:33) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-51)]']
dist: ('redhat', '5.8', 'Boron')
linux_distrubution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-308.4.1.el5.perfctr.2.6.42.ug.1.ug-x86_64-with-redhat-5.8-Boron
uname: ('Linux', 'gengar1.gengar.os', '2.6.18-308.4.1.el5.perfctr.2.6.42.ug.1.ug', '#1 SMP Thu May 10 15:40:46 CEST 2012', 'x86_64', 'x86_64')
version: #1 SMP Thu May 10 15:40:46 CEST 2012
mac_ver: ('', ('', '', ''), '')
```

# SL 5.8 (CERN SLC)

```
Python version: ['2.4.3 (#1, Jun 19 2012, 13:51:22) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-52)]']
dist: ('redhat', '5.8', 'Boron')
linux_distrubution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-308.20.1.el5-x86_64-with-redhat-5.8-Boron
uname: ('Linux', 'lxplus402.cern.ch', '2.6.18-308.20.1.el5', '#1 SMP Wed Nov 14 10:31:58 CET 2012', 'x86_64', 'x86_64')
version: #1 SMP Wed Nov 14 10:31:58 CET 2012
mac_ver: ('', ('', '', ''), '')
```

#### SL 6.2

```
Python version: ['2.6.6 (r266:84292, Jan  4 2012, 16:09:28) ', '[GCC 4.4.6 20110731 (Red Hat 4.4.6-3)]']
dist: ('redhat', '6.2', 'Carbon')
linux_distrubution: ('Scientific Linux', '6.2', 'Carbon')
system: Linux
machine: x86_64
platform: Linux-2.6.32-220.17.1.el6.vSMP.3.x86_64-x86_64-with-redhat-6.2-Carbon
uname: ('Linux', 'node701.dugtrio.os', '2.6.32-220.17.1.el6.vSMP.3.x86_64', '#1 SMP Wed Jun 6 15:52:18 PDT 2012', 'x86_64', 'x86_64')
version: #1 SMP Wed Jun 6 15:52:18 PDT 2012
mac_ver: ('', ('', '', ''), '')
```

### SUSE Linux Enterprise Server (SLES)

#### SLES 11 SP1

```
Python version: ['2.6.8 (unknown, May 29 2012, 22:30:44) ', '[GCC 4.3.4 [gcc-4_3-branch revision 152973]]']
dist: ('SuSE', '11', 'x86_64')
linux_distrubution: ('SUSE Linux Enterprise Server ', '11', 'x86_64')
system: Linux
machine: x86_64
platform: Linux-2.6.32.59-0.3-default-x86_64-with-SuSE-11-x86_64
uname: ('Linux', 'service0', '2.6.32.59-0.3-default', '#1 SMP 2012-04-27 11:14:44 +0200', 'x86_64', 'x86_64')
version: #1 SMP 2012-04-27 11:14:44 +0200
mac_ver: ('', ('', '', ''), '')
```

### Ubuntu

#### Ubuntu 10.04 (client distro)

```
Python version: ['2.6.5 (r265:79063, Apr 16 2010, 13:09:56) ', '[GCC 4.4.3]']
dist: ('Ubuntu', '10.04', 'lucid')
linux_distrubution: ('Ubuntu', '10.04', 'lucid')
system: Linux
machine: i686
platform: Linux-2.6.32-34-generic-pae-i686-with-Ubuntu-10.04-lucid
uname: ('Linux', 'laptop-sp', '2.6.32-34-generic-pae', '#77-Ubuntu SMP Tue Sep 13 21:16:18 UTC 2011', 'i686', '')
version: #77-Ubuntu SMP Tue Sep 13 21:16:18 UTC 2011
mac_ver: ('', ('', '', ''), '')
```

#### Ubuntu 12.04 (server distro)

```
Python version: ['2.7.3 (default, Aug  1 2012, 05:14:39) ', '[GCC 4.6.3]']
dist: ('Ubuntu', '12.04', 'precise')
linux_distrubution: ('Ubuntu', '12.04', 'precise')
system: Linux
machine: x86_64
platform: Linux-3.2.0-32-generic-x86_64-with-Ubuntu-12.04-precise
uname: ('Linux', 'dc02', '3.2.0-32-generic', '#51-Ubuntu SMP Wed Sep 26 21:33:09 UTC 2012', 'x86_64', 'x86_64')
version: #51-Ubuntu SMP Wed Sep 26 21:33:09 UTC 2012
mac_ver: ('', ('', '', ''), '')
```

#### Ubuntu 12.10

```
Python version: ['2.7.3 (default, Sep 26 2012, 21:51:14) ', '[GCC 4.7.2]']
dist: ('Ubuntu', '12.10', 'quantal')
linux_distrubution: ('Ubuntu', '12.10', 'quantal')
system: Linux
machine: x86_64
platform: Linux-3.5.0-22-generic-x86_64-with-Ubuntu-12.10-quantal
uname: ('Linux', 'stijn-W150HRM', '3.5.0-22-generic', '#34-Ubuntu SMP Tue Jan 8 21:47:00 UTC 2013', 'x86_64', 'x86_64')
version: #34-Ubuntu SMP Tue Jan 8 21:47:00 UTC 2013
mac_ver: ('', ('', '', ''), '')
```

### Mac OS X

#### OS X 10.6.8 (Snow Leopard)

```
Python version: ['2.6.5 (r265:79063, May 28 2010, 10:16:29) ', '[GCC 4.2.1 (Apple Inc. build 5659)]']
dist: ('', '', '')
linux_distrubution: ('', '', '')
system: Darwin
machine: i386
platform: Darwin-10.8.0-i386-64bit
uname: ('Darwin', 'laptop-fred.local', '10.8.0', 'Darwin Kernel Version 10.8.0: Tue Jun  7 16:33:36 PDT 2011; root:xnu-1504.15.3~1/RELEASE_I386', 'i386', 'i386')
version: Darwin Kernel Version 10.8.0: Tue Jun  7 16:33:36 PDT 2011; root:xnu-1504.15.3~1/RELEASE_I386
mac_ver: ('10.6.8', ('', '', ''), 'i386')
```

#### OS X 10.8.2 (Mountain Lion)

```
Python version: ['2.7.2 (default, Jun 20 2012, 16:23:33) ', '[GCC 4.2.1 Compatible Apple Clang 4.0 (tags/Apple/clang-418.0.60)]']
dist: ('', '', '')
linux_distrubution: ('', '', '')
system: Darwin
machine: x86_64
platform: Darwin-12.2.1-x86_64-i386-64bit
uname: ('Darwin', 'b245h132.ugent.be', '12.2.1', 'Darwin Kernel Version 12.2.1: Thu Oct 18 16:32:48 PDT 2012; root:xnu-2050.20.9~2/RELEASE_X86_64', 'x86_64', 'i386')
version: Darwin Kernel Version 12.2.1: Thu Oct 18 16:32:48 PDT 2012; root:xnu-2050.20.9~2/RELEASE_X86_64
mac_ver: ('10.8.2', ('', '', ''), 'x86_64')
```

### Other

#### Diskstation Manager (DSM) 4.1

```

Python version: ['2.7.3 (default, Nov  9 2012, 16:25:03) ', '[GCC
4.2.1]']
dist: ('', '', '')
linux_distrubution: ('', '', '')
system: Linux
machine: x86_64
platform: Linux-3.2.11-x86_64-with-glibc2.3.2
uname: ('Linux', 'Tank', '3.2.11', '#2668 SMP Tue Dec 11 13:04:20 CST
2012', 'x86_64', '')
version: #2668 SMP Tue Dec 11 13:04:20 CST 2012
mac_ver: ('', ('', '', ''), '')
```

## Alternate approaches

 * in bash, based on `/etc/*release`: https://gist.github.com/4603671