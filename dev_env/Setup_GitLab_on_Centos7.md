## GitLab Guide

https://about.gitlab.com/downloads/#centos7

## Upgrade system

`yum update`

## Open Http and SSH access in firewall

```shell
sudo yum install curl policycoreutils openssh-server openssh-clients
sudo systemctl enable sshd
sudo systemctl start sshd
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
yum install firewalld
systemctl start firewalld
systemctl unmask firewalld
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
```

### Fail to start postfix?

`vi /etc/postfix/main.cf`

```diff
- inet_interfaces = localhost
+ inet_interfaces = all
```

### Mail cannot be sent?

`vi /etc/postfix/main.cf`

```diff
- inet_protocols = all
+ inet_protocols = ipv4
```

## Add the GitLab package server and install the package

```shell
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo yum install gitlab-ce
```

## Configure GitLab CE

### Setup external url

`vi /etc/gitlab/gitlab.rb`

```diff
- external_url 'http://localhost'
+ external_url 'http://mygitlabname.comxxx'
```

### Setup Mail address

`echo "Test mail from postfix" | mail -s "Test Postfix" luventa@outlook.com`

Find the mail in your mail box and copy the sender addr.

> root@xxxx.localdomain

Change the gitlab rails to it

`vi /etc/gitlab/gitlab.rb`

```diff
- # gitlab_rails['gitlab_email_from'] = 'example@example.com'
+ gitlab_rails['gitlab_email_from'] = 'root@VM_240_212_centos.localdomain'
```

### Setup avatar url

`vi /etc/gitlab/gitlab.rb`

```diff
- # gitlab_rails['gravatar_plain_url'] = 'http://gravatar.duoshuo.com/avatar/%{hash}?s=%{size}&d=identicon'
+ gitlab_rails['gravatar_plain_url'] = 'http://gravatar.duoshuo.com/avatar/%{hash}?s=%{size}&d=identicon'
```

### Run reconfigure

`sudo gitlab-ctl reconfigure`