# Question
- Configure yum repository with the given link (2 repo, BaseOS and AppStream)
- Base_url = https://dtc.randifilan.id/rhel/BaseOS
- AppStream = https://dtc.randifilan.id/rhel/AppStream
---

Create file /etc/yum.repos.d/local.repo
```
touch /etc/yum.repos.d/local.repo
```

Edit Local repo
- vim /etc/yum.repos.d/local.repo
```
[rhel-9-for-x86_64-appstream-rpms]
name=Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)
baseurl=https://dtc.randifilan.id/rhel/BaseOS/
gpgcheck=0
enabled=1

[rhel-9-for-x86_64-baseos-rpms]
name=Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)
baseurl=https://dtc.randifilan.id/rhel/AppStream/
gpgcheck=0
enabled=1
```

Verify
- dnf clean all
```
dnf clean all
Updating Subscription Management repositories.
61 files removed
```

- dnf repolist
dnf repolist
Updating Subscription Management repositories.
repo id                                             repo name
rhel-9-for-x86_64-appstream-rpms                    Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)
rhel-9-for-x86_64-baseos-rpms                       Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)

- dnf check-update
```
dnf check-update
Updating Subscription Management repositories.
Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)                                           7.1 MB/s |  30 MB     00:04    
Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)                                              5.9 MB/s |  19 MB     00:03    
  
bpftool.x86_64                                     7.2.0-362.24.1.el9_3                            rhel-9-for-x86_64-baseos-rpms   
kernel.x86_64                                      5.14.0-362.24.1.el9_3                           rhel-9-for-x86_64-baseos-rpms   
kernel-headers.x86_64                              5.14.0-362.24.1.el9_3                           rhel-9-for-x86_64-appstream-rpms
kernel-tools.x86_64                                5.14.0-362.24.1.el9_3                           rhel-9-for-x86_64-baseos-rpms   
kernel-tools-libs.x86_64                           5.14.0-362.24.1.el9_3                           rhel-9-for-x86_64-baseos-rpms   
python3-perf.x86_64                                5.14.0-362.24.1.el9_3                           rhel-9-for-x86_64-baseos-rpms   
Security: kernel-core-5.14.0-362.24.1.el9_3.x86_64 is an installed security update
Security: kernel-core-5.14.0-362.18.1.el9_3.x86_64 is the currently running version
```
