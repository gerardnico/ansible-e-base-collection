# Certbot

## About

A role to install certbot of let's encrypt in order to get certificate.

This role implements certbot for [yum based os](https://certbot.eff.org/lets-encrypt/centosrhel7-other) with 2 DNS provider:
* [cloudflare](#cloudflare)
* [ovh](#ovh)
and install a [renewal job](#renewal-job)

## Note

Certificate are used:

* in website for https (nginx)
* for the server (smtp).

## Vars

### Let's encrypt

* `ans_e_certbot_letsencrypt_email`: the email account mandatory

### Cloudflare

The [Cloudflare credentials](https://certbot-dns-cloudflare.readthedocs.io/en/stable/)
* `ans_e_certbot_cloudflare_api_key`: the cloudflare DNS API Key
* `ans_e_certbot_cloudflare_email`: the Cloudflare email account
* `ans_e_certbot_cloudflare_domains`: a list of domain managed by Cloudflare

### OVH
The [OVH configuration](https://certbot-dns-ovh.readthedocs.io/en/stable/).
You can create a token at [the API](https://eu.api.ovh.com/createToken/)

* `ans_e_certbot_ovh_application_key`: the application key
* `ans_e_certbot_ovh_application_secret`: the secret
* `ans_e_certbot_ovh_endpoint`: the endpoint (default to ovh-eu)
* `ans_e_certbot_ovh_domains`: a list of domain managed by ovh


### Renewal Job


* `ans_e_certbot_renewal_ping_url`: The url to ping after a successful job.
 

## Usage

For [ovh](#ovh)
```yml
- name: Installing Certbot
  ansible.builtin.include_role:
    name: ans_e.ans_e_base.ans_e_certbot
  vars:
      ans_e_certbot_ovh_application_key: xxxx
      ans_e_certbot_ovh_application_secret: xxx
      ans_e_certbot_ovh_endpoint: xxx
      ans_e_certbot_ovh_domains:
        - example.com
      ans_e_certbot_renewal_ping_url: https://hc-ping.com/47a5f7da-b905-4300-8bc8-4a9d165116d7 # with https://healthchecks.io/          
```


## Log/Debug/Troubleshooting

The log file is located at:
```
/var/log/letsencrypt/letsencrypt.log
```



## See also related certbot ACME project

### Ansible Acme Module
Ansible has also a module called [acme_certificate](https://docs.ansible.com/ansible/2.9/modules/acme_certificate_module.html#acme-certificate-module)

More see this [blog](https://www.digitalocean.com/community/tutorials/how-to-acquire-a-let-s-encrypt-certificate-using-ansible-on-ubuntu-18-04)

### Java
JAcme