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

print """Python version: %s
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
)
```

## Outputs

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

#### Scientific Linux (SL) 5.8

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

#### Scientific Linux (SL) 6.2

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

#### SUSE Linux Enterprise Server (SLES) 11 SP1

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