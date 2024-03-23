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
```
vim /etc/yum.repos.d/local.repo
[baseOS]
name=baseOS RHEL 9.3 DVD
baseurl=https://dtc.randifilan.id/rhel/BaseOS/
gpgcheck=0
enabled=1

[AppStream]
name=AppStream RHEL 9.3 DVD
baseurl=https://dtc.randifilan.id/rhel/AppStream/
gpgcheck=0
enabled=1
```
