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
linux_distribution: %s
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
linux_distribution: N/A
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
linux_distribution: N/A
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
linux_distribution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-308.4.1.el5xen-x86_64-with-redhat-5.9-Final
uname: ('Linux', 'test-centos-1', '2.6.18-308.4.1.el5xen', '#1 SMP Tue Apr 17 17:49:15 EDT 2012', 'x86_64', 'x86_64')
version: #1 SMP Tue Apr 17 17:49:15 EDT 2012
mac_ver: ('', ('', '', ''), '')
```

#### CentOS 6.3

```
Python version: ['2.6.6 (r266:84292, Sep 11 2012, 08:34:23) ', '[GCC 4.4.6 20120305 (Red Hat 4.4.6-4)]']
dist: ('centos', '6.3', 'Final')
linux_distribution: ('CentOS', '6.3', 'Final')
system: Linux
machine: x86_64
platform: Linux-2.6.32-279.9.1.el6.x86_64-x86_64-with-centos-6.3-Final
uname: ('Linux', 'build-centos-6-3.gmi.oeaw.ac.at', '2.6.32-279.9.1.el6.x86_64', '#1 SMP Tue Sep 25 21:43:11 UTC 2012', 'x86_64', 'x86_64')
version: #1 SMP Tue Sep 25 21:43:11 UTC 2012
mac_ver: ('', ('', '', ''), '')
```

### Debian

#### Debian 4.0

```
Python version: ['2.4.4 (#2, Jan 24 2010, 11:50:13) ', '[GCC 4.1.2 20061115 (prerelease) (Debian 4.1.1-21)]']
dist: ('debian', '4.0', '')
linux_distribution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-6-amd64-x86_64-with-debian-4.0
uname: ('Linux', 'master.cluster', '2.6.18-6-amd64', '#1 SMP Sat Feb 20 23:34:55 UTC 2010', 'x86_64', '')
version: #1 SMP Sat Feb 20 23:34:55 UTC 2010
mac_ver: ('', ('', '', ''), '')
```

#### Debian 5.0

```
Python version: ['2.5.2 (r252:60911, Jan 24 2010, 17:44:40) ', '[GCC 4.3.2]']
dist: ('debian', '5.0.8', '')
linux_distribution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.26-2-amd64-x86_64-with-debian-5.0.8
uname: ('Linux', 'master', '2.6.26-2-amd64', '#1 SMP Mon Jun 13 16:29:33 UTC 2011', 'x86_64', '')
version: #1 SMP Mon Jun 13 16:29:33 UTC 2011
mac_ver: ('', ('', '', ''), '')
```

#### Debian 6 Squeeze

```
fgeorgatos@gaia-10:~/arena/checkosversion$ python runme.py 
Python version: ['2.7.3 (default, Dec 18 2012, 07:01:29) ', '[GCC 4.6.3]']
dist: ('debian', '6.0.6', '')
linux_distribution: ('debian', '6.0.6', '')
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
linux_distribution: ('Fedora', '16', 'Verne')
system: Linux
machine: x86_64
platform: Linux-3.6.7-4.fc16.x86_64-x86_64-with-fedora-16-Verne
uname: ('Linux', 'ca60c171', '3.6.7-4.fc16.x86_64', '#1 SMP Tue Nov 20 20:33:31 UTC 2012', 'x86_64', 'x86_64')
version: #1 SMP Tue Nov 20 20:33:31 UTC 2012
mac_ver: ('', ('', '', ''), '')
```

#### Fedora 17

```
Python version: ['2.7.3 (default, Jul 24 2012, 10:05:38) ', '[GCC 4.7.0 20120507 (Red Hat 4.7.0-5)]']
dist: ('fedora', '17', 'Beefy Miracle')
linux_distribution: ('Fedora', '17', 'Beefy Miracle')
system: Linux
machine: x86_64
platform: Linux-3.6.11-1.fc17.x86_64-x86_64-with-fedora-17-Beefy_Miracle
uname: ('Linux', 'ca60c173', '3.6.11-1.fc17.x86_64', '#1 SMP Mon Dec 17 22:16:35 UTC 2012', 'x86_64', 'x86_64')
version: #1 SMP Mon Dec 17 22:16:35 UTC 2012
mac_ver: ('', ('', '', ''), '')
```


### Red Hat Enterprise Linux (RHEL)

### RHEL 5.6

```
Python version: ['2.4.3 (#1, Dec 10 2010, 17:24:35) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-50)]']
dist: ('redhat', '5.6', 'Tikanga')
linux_distribution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-238.el5-x86_64-with-redhat-5.6-Tikanga
uname: ('Linux', 'xcat1', '2.6.18-238.el5', '#1 SMP Sun Dec 19 14:22:44 EST 2010', 'x86_64', 'x86_64')
version: #1 SMP Sun Dec 19 14:22:44 EST 2010
mac_ver: ('', ('', '', ''), '')
```

#### RHEL 5.8

```
Python version: ['2.4.3 (#1, May  1 2012, 13:52:57) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-52)]']
dist: ('redhat', '5.8', 'Tikanga')
linux_distribution: N/A
system: Linux
machine: i686
platform: Linux-2.6.18-308.8.2.el5xen-i686-with-redhat-5.8-Tikanga
uname: ('Linux', 'xxx', '2.6.18-308.8.2.el5xen', '#1 SMP Tue May 29 12:36:24 EDT 2012', 'i686', 'i686')
version: #1 SMP Tue May 29 12:36:24 EDT 2012
mac_ver: ('', ('', '', ''), '')
```

#### RHEL 6.2

```
Python version: ['2.6.6 (r266:84292, May  1 2012, 13:52:17) ', '[GCC 4.4.6 20110731 (Red Hat 4.4.6-3)]']
dist: ('redhat', '6.2', 'Santiago')
linux_distribution: ('Red Hat Enterprise Linux Server', '6.2', 'Santiago')
system: Linux
machine: x86_64
platform: Linux-2.6.32-220.23.1.el6.x86_64-x86_64-with-redhat-6.2-Santiago
uname: ('Linux', 'psglogin.psg.cluster.zone', '2.6.32-220.23.1.el6.x86_64', '#1 SMP Tue Jun 12 11:20:15 EDT 2012', 'x86_64', 'x86_64')
version: #1 SMP Tue Jun 12 11:20:15 EDT 2012
mac_ver: ('', ('', '', ''), '')
```

#### RHEL 6.3

```
Python version: ['2.6.6 (r266:84292, Aug 28 2012, 10:55:56) ', '[GCC 4.4.6 20120305 (Red Hat 4.4.6-4)]']
dist: ('redhat', '6.3', 'Santiago')
linux_distribution: ('Red Hat Enterprise Linux Server', '6.3', 'Santiago')
system: Linux
machine: x86_64
platform: Linux-2.6.32-279.19.1.el6.x86_64-x86_64-with-redhat-6.3-Santiago
uname: ('Linux', 'build-rhel-6-3.gmi.oeaw.ac.at', '2.6.32-279.19.1.el6.x86_64', '#1 SMP Sat Nov 24 14:35:28 EST 2012', 'x86_64', 'x86_64')
version: #1 SMP Sat Nov 24 14:35:28 EST 2012
mac_ver: ('', ('', '', ''), '')
```


### Rocks

#### Rocks 5.4 (Maverick)

```
Python version: ['2.4.3 (#1, Sep  3 2009, 15:37:37) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-46)]']
dist: ('redhat', '5.5', 'Final')
linux_distribution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-194.17.4.el5-x86_64-with-redhat-5.5-Final
uname: ('Linux', 'xxx', '2.6.18-194.17.4.el5', '#1 SMP Mon Oct 25 15:50:53 EDT 2010', 'x86_64', 'x86_64')
version: #1 SMP Mon Oct 25 15:50:53 EDT 2010
mac_ver: ('', ('', '', ''), '')
```

#### Rocks 5.4.3 (Viper)

```
Python version: ['2.4.3 (#1, May  5 2011, 16:39:10) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-50)]']
dist: ('redhat', '5.6', 'Final')
linux_distribution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.34.7-1-x86_64-with-redhat-5.6-Final
uname: ('Linux', 'xxx', '2.6.34.7-1', '#1 SMP Thu Nov 24 11:27:26 PST 2011', 'x86_64', 'x86_64')
version: #1 SMP Thu Nov 24 11:27:26 PST 2011
mac_ver: ('', ('', '', ''), '')
```

with a more recent Python, that provides `platform.linux_distribution`:
```
Python version: ['2.7.2 (default, Aug  9 2011, 03:48:50) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-50)]']
dist: ('redhat', '5.6', 'Final')
linux_distribution: ('CentOS', '5.6', 'Final')
system: Linux
machine: x86_64
platform: Linux-2.6.34.7-1-x86_64-with-redhat-5.6-Final
uname: ('Linux', 'xxx', '2.6.34.7-1', '#1 SMP Thu Nov 24 11:27:26 PST 2011', 'x86_64', 'x86_64')
version: #1 SMP Thu Nov 24 11:27:26 PST 2011
mac_ver: ('', ('', '', ''), '')
```

#### Rocks 5.4.3 (Viper) on ScaleMP vSMP

```
Python version: ['2.4.3 (#1, May  5 2011, 16:39:10) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-50)]']
dist: ('redhat', '5.6', 'Final')
linux_distribution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.32-220.7.1.el6.vSMP.4.x86_64-x86_64-with-redhat-5.6-Final
uname: ('Linux', 'xxx', '2.6.32-220.7.1.el6.vSMP.4.x86_64', '#1 SMP Sun May 6 08:24:08 PDT 2012', 'x86_64', 'x86_64')
version: #1 SMP Sun May 6 08:24:08 PDT 2012
mac_ver: ('', ('', '', ''), '')
```

### Scientific Linux (SL)

#### SL 5.2

```
Python version: ['2.4.3 (#1, May  5 2011, 18:44:23) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-50)]']
dist: ('redhat', '5.2', 'Boron')
linux_distribution: N/A
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
linux_distribution: N/A
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
linux_distribution: ('Scientific Linux SL', '5.6', 'Boron')
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
linux_distribution: N/A
system: Linux
machine: x86_64
platform: Linux-2.6.18-308.4.1.el5.perfctr.2.6.42.ug.1.ug-x86_64-with-redhat-5.8-Boron
uname: ('Linux', 'gengar1.gengar.os', '2.6.18-308.4.1.el5.perfctr.2.6.42.ug.1.ug', '#1 SMP Thu May 10 15:40:46 CEST 2012', 'x86_64', 'x86_64')
version: #1 SMP Thu May 10 15:40:46 CEST 2012
mac_ver: ('', ('', '', ''), '')
```

#### SL 5.8 (CERN SLC)

```
Python version: ['2.4.3 (#1, Jun 19 2012, 13:51:22) ', '[GCC 4.1.2 20080704 (Red Hat 4.1.2-52)]']
dist: ('redhat', '5.8', 'Boron')
linux_distribution: N/A
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
linux_distribution: ('Scientific Linux', '6.2', 'Carbon')
system: Linux
machine: x86_64
platform: Linux-2.6.32-220.17.1.el6.vSMP.3.x86_64-x86_64-with-redhat-6.2-Carbon
uname: ('Linux', 'node701.dugtrio.os', '2.6.32-220.17.1.el6.vSMP.3.x86_64', '#1 SMP Wed Jun 6 15:52:18 PDT 2012', 'x86_64', 'x86_64')
version: #1 SMP Wed Jun 6 15:52:18 PDT 2012
mac_ver: ('', ('', '', ''), '')
```

### Solaris

#### Solaris 9 (on SPARC)

```
Python version: ['2.6.4 (r264:75706, Aug  4 2010, 16:53:32) [C]']
dist: ('', '', '')
linux_distribution: ('', '', '')
system: SunOS
machine: sun4u
platform: SunOS-5.9-sun4u-sparc-32bit-ELF
uname: ('SunOS', 'xxx', '5.9', 'Generic_122300-60', 'sun4u', 'sparc')
version: Generic_122300-60
mac_ver: ('', ('', '', ''), '')
```

### SUSE

#### OpenSUSE 11.3

```
Python version: ['2.6.5 (r265:79063, May  6 2011, 17:25:59) ', '[GCC 4.5.0 20100604 [gcc-4_5-branch revision 160292]]']
dist: ('SuSE', '11.3', 'x86_64')
linux_distribution: ('openSUSE ', '11.3', 'x86_64')
system: Linux
machine: x86_64
platform: Linux-2.6.34.10-0.6-desktop-x86_64-with-SuSE-11.3-x86_64
uname: ('Linux', 'homer', '2.6.34.10-0.6-desktop', '#1 SMP PREEMPT 2011-12-13 18:27:38 +0100', 'x86_64', 'x86_64')
version: #1 SMP PREEMPT 2011-12-13 18:27:38 +0100
mac_ver: ('', ('', '', ''), '')
```

#### SUSE Linux Enterprise Server (SLES) 10 (on Power6)

```
Python version: ['2.4.2 (#1, Apr 20 2012, 03:25:31) ', '[GCC 4.1.2 20070115 (SUSE Linux)]']
dist: ('SuSE', '10', 'ppc')
linux_distribution: N/A
system: Linux
machine: ppc64
platform: Linux-2.6.16.60-0.93.1-ppc64-ppc64-with-SuSE-10-ppc
uname: ('Linux', 'xxx', '2.6.16.60-0.93.1-ppc64', '#1 SMP Mon Jan 9 23:55:08 UTC 2012', 'ppc64', 'ppc64')
version: #1 SMP Mon Jan 9 23:55:08 UTC 2012
mac_ver: ('', ('', '', ''), '')
```

#### SLES 10 (on Itanium)

```
Python version: ['2.4.2 (#1, Jan 10 2008, 17:41:07) ', '[GCC 4.1.2 20070115 (prerelease) (SUSE Linux)]']
dist: ('SuSE', '10', 'ia64')
linux_distribution: N/A
system: Linux
machine: ia64
platform: Linux-2.6.16.60-0.39.3.PTF.506281.1-default-ia64-with-SuSE-10-ia64
uname: ('Linux', 'altix-450', '2.6.16.60-0.39.3.PTF.506281.1-default', '#1 SMP Mon May 11 11:46:34 UTC 2009', 'ia64', 'ia64')
version: #1 SMP Mon May 11 11:46:34 UTC 2009
mac_ver: ('', ('', '', ''), '')
```

#### SLES 11 initial (SP0)

```
Python version: ['2.6 (r26:66714, Mar 23 2010, 15:24:19) ', '[GCC 4.3.2 [gcc-4_3-branch revision 141291]]']
dist: ('SuSE', '11', 'x86_64')
linux_distribution: ('SUSE Linux Enterprise Server ', '11', 'x86_64')
system: Linux
machine: x86_64
platform: Linux-2.6.27.19-5-default-x86_64-with-SuSE-11-x86_64
uname: ('Linux', 'mgmt01', '2.6.27.19-5-default', '#1 SMP 2009-02-28 04:40:21 +0100', 'x86_64', 'x86_64')
version: #1 SMP 2009-02-28 04:40:21 +0100
mac_ver: ('', ('', '', ''), '')
```

#### SLES 11 SP1

```
Python version: ['2.6.8 (unknown, May 29 2012, 22:30:44) ', '[GCC 4.3.4 [gcc-4_3-branch revision 152973]]']
dist: ('SuSE', '11', 'x86_64')
linux_distribution: ('SUSE Linux Enterprise Server ', '11', 'x86_64')
system: Linux
machine: x86_64
platform: Linux-2.6.32.59-0.3-default-x86_64-with-SuSE-11-x86_64
uname: ('Linux', 'service0', '2.6.32.59-0.3-default', '#1 SMP 2012-04-27 11:14:44 +0200', 'x86_64', 'x86_64')
version: #1 SMP 2012-04-27 11:14:44 +0200
mac_ver: ('', ('', '', ''), '')
```

#### SLES 11 SP2

```
Python version: ['2.6.8 (unknown, May 29 2012, 22:30:44) ', '[GCC 4.3.4 [gcc-4_3-branch revision 152973]]']
dist: ('SuSE', '11', 'x86_64')
linux_distribution: ('SUSE Linux Enterprise Server ', '11', 'x86_64')
system: Linux
machine: x86_64
platform: Linux-3.0.38-0.5-default-x86_64-with-SuSE-11-x86_64
uname: ('Linux', 'service1', '3.0.38-0.5-default', '#1 SMP Fri Aug 3 09:02:17 UTC 2012 (358029e)', 'x86_64', 'x86_64')
version: #1 SMP Fri Aug 3 09:02:17 UTC 2012 (358029e)
mac_ver: ('', ('', '', ''), '')
```

### Ubuntu

#### Ubuntu 10.04 (client distro)

```
Python version: ['2.6.5 (r265:79063, Apr 16 2010, 13:09:56) ', '[GCC 4.4.3]']
dist: ('Ubuntu', '10.04', 'lucid')
linux_distribution: ('Ubuntu', '10.04', 'lucid')
system: Linux
machine: i686
platform: Linux-2.6.32-34-generic-pae-i686-with-Ubuntu-10.04-lucid
uname: ('Linux', 'laptop-sp', '2.6.32-34-generic-pae', '#77-Ubuntu SMP Tue Sep 13 21:16:18 UTC 2011', 'i686', '')
version: #77-Ubuntu SMP Tue Sep 13 21:16:18 UTC 2011
mac_ver: ('', ('', '', ''), '')
```

#### Ubuntu 10.04 Server

```
Python version: ['2.6.5 (r265:79063, Oct  1 2012, 22:04:36) ', '[GCC 4.4.3]']
dist: ('Ubuntu', '10.04', 'lucid')
linux_distribution: ('Ubuntu', '10.04', 'lucid')
system: Linux
machine: x86_64
platform: Linux-2.6.32-32-server-x86_64-with-Ubuntu-10.04-lucid
uname: ('Linux', 'xxx', '2.6.32-32-server', '#62-Ubuntu SMP Wed Apr 20 22:07:43 UTC 2011', 'x86_64', '')
version: #62-Ubuntu SMP Wed Apr 20 22:07:43 UTC 2011
mac_ver: ('', ('', '', ''), '')
```

#### Ubuntu 12.04 (server distro)

```
Python version: ['2.7.3 (default, Aug  1 2012, 05:14:39) ', '[GCC 4.6.3]']
dist: ('Ubuntu', '12.04', 'precise')
linux_distribution: ('Ubuntu', '12.04', 'precise')
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
linux_distribution: ('Ubuntu', '12.10', 'quantal')
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
linux_distribution: ('', '', '')
system: Darwin
machine: i386
platform: Darwin-10.8.0-i386-64bit
uname: ('Darwin', 'laptop-fred.local', '10.8.0', 'Darwin Kernel Version 10.8.0: Tue Jun  7 16:33:36 PDT 2011; root:xnu-1504.15.3~1/RELEASE_I386', 'i386', 'i386')
version: Darwin Kernel Version 10.8.0: Tue Jun  7 16:33:36 PDT 2011; root:xnu-1504.15.3~1/RELEASE_I386
mac_ver: ('10.6.8', ('', '', ''), 'i386')
```

#### OS X 10.8.2 (Mountain Lion)

(slight differences across systems)

```
Python version: ['2.7.2 (default, Jun 20 2012, 16:23:33) ', '[GCC 4.2.1 Compatible Apple Clang 4.0 (tags/Apple/clang-418.0.60)]']
dist: ('', '', '')
linux_distribution: ('', '', '')
system: Darwin
machine: x86_64
platform: Darwin-12.2.1-x86_64-i386-64bit
uname: ('Darwin', 'b245h132.ugent.be', '12.2.1', 'Darwin Kernel Version 12.2.1: Thu Oct 18 16:32:48 PDT 2012; root:xnu-2050.20.9~2/RELEASE_X86_64', 'x86_64', 'i386')
version: Darwin Kernel Version 12.2.1: Thu Oct 18 16:32:48 PDT 2012; root:xnu-2050.20.9~2/RELEASE_X86_64
mac_ver: ('10.8.2', ('', '', ''), 'x86_64')
```

```
Python version: ['2.7.2 (default, Jun 16 2012, 12:38:40) ', '[GCC 4.2.1 Compatible Apple Clang 4.0 (tags/Apple/clang-418.0.60)]']
dist: ('', '', '')
linux_distribution: ('', '', '')
system: Darwin
machine: x86_64
platform: Darwin-12.2.1-x86_64-i386-64bit
uname: ('Darwin', 'silenus.gmi.oeaw.ac.at', '12.2.1', 'Darwin Kernel Version 12.2.1: Thu Oct 18 12:13:47 PDT 2012; root:xnu-2050.20.9~1/RELEASE_X86_64', 'x86_64', 'i386')
version: Darwin Kernel Version 12.2.1: Thu Oct 18 12:13:47 PDT 2012; root:xnu-2050.20.9~1/RELEASE_X86_64
mac_ver: ('10.8.2', ('', '', ''), 'x86_64')
```

### Other

#### Diskstation Manager (DSM) 4.1

```

Python version: ['2.7.3 (default, Nov  9 2012, 16:25:03) ', '[GCC
4.2.1]']
dist: ('', '', '')
linux_distribution: ('', '', '')
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