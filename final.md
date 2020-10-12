# Intensive Coursework Final

[TOC]

## CM Install Lab

### 1. 접속

* Host1 : `ssh -i SKCC_Final.pem centos@13.124.143.184`

* Host2 : `ssh -i SKCC_Final.pem centos@3.35.149.106`

* Host3 : `ssh -i SKCC_Final.pem centos@3.35.14.86`

* Host4 : `ssh -i SKCC_Final.pem centos@3.34.40.60`

* Host5 : `ssh -i SKCC_Final.pem centos@13.125.129.106`

### 2. 환경 세팅

* 호스트 세팅

  `sudo hostnamectl set-hostname [node-name].bdai.com`

  `sudo vi /etc/hosts`

  ```
  10.0.0.96   cm.bdai.com cm
  10.0.0.227  m1.bdai.com m1
  10.0.0.165  d1.bdai.com d1
  10.0.0.81   d2.bdai.com d2
  10.0.0.147  d3.bdai.com d3
  ```

* yum 업데이트

  `sudo yum update`

### 3. System Configuration Checks

#### 1. Check vm.swappiness on all your nodes

`cat /proc/sys/vm/swappiness`

* Set the value to 1 if necessary

  1. `sudo vi /etc/sysctl.conf` -> `vm.swappiness=1`

  2. `sudo sysctl -w vm.swappiness=1`

#### 2. Show the mount attributes of your volume(s)

`cat /proc/mounts`

#### 3. If you have ext-based volumes, list the reserve space setting

`cat /etc/fstab`

#### 4. Disable transparent hugepage support

* transparent hugepage가 enable 되어있는지 확인

  `cat /sys/kernel/mm/transparent_hugepage/enabled`

`sudo vi /etc/rc.d/rc.local`

```shell
touch /var/lock/subsys/local
echo "never" > /sys/kernel/mm/transparent_hugepage/enabled
echo "never" > /sys/kernel/mm/transparent_hugepage/defrag
```

#### 5. List your network interface configuration

`cat /etc/hosts`

#### 6. Show that forward and reverse host lookups are correctly resolved

`getent hosts` 

#### 7. Show the nscd service is running

* Install nscd service

  `sudo yum install -y nscd`

* Run nscd service

  `sudo systemctl start nscd`

  `sudo systemctl enable nscd`

* Check the nscd service is running

  `sudo systemctl status nscd`

#### 8. Show the ntpd service is running

* Install ntpd service

  `sudo yum install -y ntp`

* Run nscd service

  `sudo systemctl start ntpd`

  `sudo systemctl enable ntpd`

* Check the nscd service is running

  `sudo systemctl status ntpd`

## Cloudera Manager Install Lab

**Path B install using CM 5.15.x**

### 1. Configure a Repository for Cloudera Manager

`sudo yum install -y wget`

`sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo -P /etc/yum.repos.d/`

`sudo rpm --import https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera`

### 2. Install a supported Oracle JDK

#### 1. 자바 설치

`sudo yum install -y oracle-j2sdk1.7`

#### 2. 자바 홈 환경 변수 추가

`sudo vi /etc/profile`

```shell
export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
export PATH=$PATH:$JAVA_HOME/bin
```

