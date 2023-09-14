---
title: "Gitlab"
discription: simple command to routing
date: 2022-09-01T21:29:01+08:00 
draft: false
type: post
tags: ["Git","Linux"]
showTableOfContents: true
--- 



![gitlab](images/gitlab.svg)

https://gitlab.com/rluna-gitlab/gitlab-ce

For Ubuntu 20.04 and 22.04, `arm64` packages are also available and will be automatically used on that platform when using the GitLab repository for installation.

```sh
sudo apt-get update
sudo apt-get install -y curl openssh-server ca-certificates tzdata perl
```

Next, install Postfix (or Sendmail) to send notification emails. If you want to use another solution to send emails please skip this step and configure an external SMTP server after GitLab has been installed
```sh
sudo apt-get install -y postfix
```
During Postfix installation a configuration screen may appear. Select 'Internet Site' and press enter. Use your server's external DNS for 'mail name' and press enter. If additional screens appear, continue to press enter to accept the defaults.
```sh
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
```
Next, install the GitLab package. Make sure you have correctly set up your DNS, and change https://gitlab.example.com to the URL at which you want to access your GitLab instance. Installation will automatically configure and start GitLab at that URL.

For https:// URLs, GitLab will automatically request a certificate with Let's Encrypt, which requires inbound HTTP access and a valid hostname. You can also use your own certificate or just use http:// (without the s ).

If you would like to specify a custom password for the initial administrator user ( root ), check the documentation. If a password is not specified, a random password will be automatically generated
```
sudo EXTERNAL_URL="https://gitlab.example.com" apt-get install gitlab-ee
# List available versions: apt-cache madison gitlab-ee
# Specifiy version: sudo EXTERNAL_URL="https://gitlab.example.com" apt-get install gitlab-ee=16.2.3-ee.0
# Pin the version to limit auto-updates: sudo apt-mark hold gitlab-ee
# Show what packages are held back: sudo apt-mark showhold
```
Set up your communication preferences
Visit our email subscription preference center to let us know when to communicate with you. We have an explicit email opt-in policy so you have complete control over what and how often we send you emails.
Twice a month, we send out the GitLab news you need to know, including new features, integrations, docs, and behind the scenes stories from our dev teams. For critical security updates related to bugs and system performance, sign up for our dedicated security newsletter.

Important Note If you do not opt-in to the security newsletter, you will not receive security alerts.
