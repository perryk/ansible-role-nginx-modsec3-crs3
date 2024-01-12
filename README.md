# Ansible role for Installing Nginx, compiling ModSecurity3, and installing the OWASP CRS v3 ruleset 

ModSecurity3 is a powerful open source cross-platform web application firewall (WAF).

https://modsecurity.org/

It goes hand in hand with a ruleset known as OWASP CRS.

https://modsecurity.org/crs/

Additionally both of these go hand in hand with a webserver, either Apache or Nginx, this role only supports Nginx however.

https://www.nginx.com

There are a number of libraries and packages which ModSecurity3 depends on and will be installed via this role.

This role will additionally install any compilers and other build tools required for compilation. It will then remove these tools if they were not previously installed. 

Nginx support is primarily provided by the dependent role `ansible-role-nginx` by jdauphant.

https://github.com/jdauphant/ansible-role-nginx

By default this role will install Nginx packages from OS provided repos, this is recommended to be changed to installing from the official Nginx repo instead.

This can be done by setting this variable:

```    nginx_official_repo: True```

## Requirements

Before running a playbook which calls this role:

Install any required [Ansible](https://www.ansible.com) roles from `requirements.yml` View [here](requirements.yml).

```bash
ansible-galaxy install -r requirements.yml
```

n.b in particular this role will call certain tasks from the nginx role so be sure to have it installed in the same location as this role and with a specific name of "ansible-role-nginx".

i.e this in the requirements.yml file for your project's playbook (not the requirements.yml file for this role) you will need to include both this role and the role mentioned above like this:

```yml
- src: perryk.nginx_modsec3_crs3

- src: https://github.com/jdauphant/ansible-role-nginx
  version: master
```


## Role Variables

Browse the role's [defaults/main.yml](defaults/main.yml) and [vars/main.yml](vars/main.yml) files to see if there is anything you would like to change or need to override by setting in your playbook.

There are currently no variables of note being set.

There are lots of variables however in the nginx role, perhaps the best explanation of these are all the examples in the role [README.md](https://github.com/jdauphant/ansible-role-nginx/blob/master/README.md) file.


## Example Playbook

Example playbook calling the role adding and enabling ModSecurity for the default Nginx site.

```yaml
- hosts: servers

  vars:

    nginx_pkgs:
      - nginx
    nginx_install_epel_repo: False
    nginx_official_repo: True
    nginx_official_repo_mainline: False
    nginx_module_configs:
      - ngx_http_modsecurity_module
    nginx_sites:
      default:
       - listen 80
       - server_name _
       - "modsecurity on"
       - "modsecurity_rules_file /etc/nginx/modsec/main.conf"
       - root "/usr/share/nginx/html"
       - index index.html

  roles:
    - perryk.nginx-modsec3-crs3
```

# License

MIT

## Author Information

Perry Kollmorgen - https://github.com/perryk

