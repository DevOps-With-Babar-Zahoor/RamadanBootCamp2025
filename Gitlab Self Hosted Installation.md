# OSFP DevOps Ramadan BootCamp 2025  

## GitLab Self-Hosted Installation  

### Step 1: Download Page  
Go to the download page of **Install self-managed GitLab** by visiting:  
🔗 [GitLab Install Page](https://about.gitlab.com/install/)  

### Step 2: Get installation instructions for Ubuntu  
Click on the **Ubuntu** card to get the instructions.  

### Step 3: Install and configure the necessary dependencies  
Install and configure the required dependencies:  

```bash
sudo apt-get update
sudo apt-get install -y curl openssh-server ca-certificates tzdata perl
```

Next, install Postfix (or Sendmail) to send notification emails. 

```bash
sudo apt install postfix
```

**Step 4: Add the GitLab package repository and install the package** 

> Make sure you have installed curl
```bash
curl
```

Add the GitLab package repository and install the package 

```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash 
```

**Step 5: Provide DNS URL for GitLab**

```bash
sudo EXTERNAL_URL="https://git.osfp.org.pk" apt-get install gitlab-ee
```


In case we encounter the following error, then make sure the DNS entry is properly linked with IP address.  

```yaml
[2025-03-04T10:23:02+00:00] FATAL: RuntimeError: letsencrypt_certificate[git.osfp.org.pk] (letsencrypt::http_authorization line 6) had an error: RuntimeError: acme_certificate[staging] (letsencrypt::http_authorization line 43) had an error: RuntimeError: ruby_block[create certificate for git.osfp.org.pk] (letsencrypt::http_authorization line 110) had an error: RuntimeError: [git.osfp.org.pk] Validation failed, unable to request certificate, Errors: [{url: https://acme-staging-v02.api.letsencrypt.org/acme/chall/187832474/16275866344/Is62zw, status: invalid, error: {"type"=>"urn:ietf:params:acme:error:dns", "detail"=>"no valid A records found for git.osfp.org.pk; no valid AAAA records found for git.osfp.org.pk", "status"=>400}} ] 

dpkg: error processing package gitlab-ee (--configure): 
 installed gitlab-ee package post-installation script subprocess returned error exit status 1 
Errors were encountered while processing: 
 gitlab-ee 
needrestart is being skipped since dpkg has failed 
E: Sub-process /usr/bin/dpkg returned an error code (1) 
```

**There could be another error.**  

```bash
sudo EXTERNAL_URL="https://git.osfp.org.pk" apt-get install gitlab-ee 
```

```bash
Reading package lists... Done 
Building dependency tree... Done 
Reading state information... Done 
gitlab-ee is already the newest version (17.9.1-ee.0). 
0 upgraded, 0 newly installed, 0 to remove and 100 not upgraded. 
1 not fully installed or removed. 
After this operation, 0 B of additional disk space will be used. 
Do you want to continue? [Y/n] y 
Setting up gitlab-ee (17.9.1-ee.0) ... 
It looks like there was a problem with public attributes; run gitlab-ctl reconfigure manually to fix. 
dpkg: error processing package gitlab-ee (--configure): 
 installed gitlab-ee package post-installation script subprocess returned error exit status 1 
Errors were encountered while processing: 
 gitlab-ee 
needrestart is being skipped since dpkg has failed 
E: Sub-process /usr/bin/dpkg returned an error code (1)  
```

**Then following commands will help to resolve.**

```bash
sudo gitlab-ctl reconfigure 
sudo apt-get -f install   
sudo gitlab-ctl status
```

**In case of Invalid SSL certificate  following steps can help:**

GitLab can auto-generate certificates. Ensure letsencrypt['enable'] is set to true in /etc/gitlab/gitlab.rb: 

```yaml
letsencrypt['enable'] = true 

letsencrypt['contact_emails'] = ['babar@osfp.org.pk']   

external_url "https://git.osfp.org.pk" 
```

```bash
sudo gitlab-ctl reconfigure 
```

To Get root password run below command

```bash
cat /etc/gitlab/initial_root_password
```
