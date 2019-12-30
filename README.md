Role Name
=========

This role will install the Cowrie medium interaction honeypot on any Debian based system. By default the cowrie process will listen on port 2222. IPTables is then used to rewrite any requests coming to port 22 to port 2222. 

Requirements
------------

None

Role Variables
--------------

Defaults:
  - **cowrie_user**: System the process will be started as and file will be owned by. Default: cowrie
  - **cowrie_group**: System group the files will be owned by. Default: {{ cowrie_ user }}
  - **cowrie_repo**: The git repo to retrieve code from. Changing this allows the user to pull code from a fork of the code. Default: http://github.com/micheloosterhof/cowrie
  - **cowrie_dir**: Directory to clone the code to. Default: /home/{{ cowrie_user }}/cowrie
  - **cowrie_version**: The version tag to checkout. Default: v2.0.0
  - **cowrie_port_pub**: The public facing port that targets would connect to and IPTables rewrites. Default: 22
  - **cowrie_port_priv**: The port cowrie is actually listening on and that IPTables rewrites to. Default: 2222
  - **cowrie_hostname**: The hostname displayed within the cowrie environment. Default: srv02.
  - **cowrie_log_path**: Path relative to `cowrie_dir` for log files. (JSON, plain, tty, etc.). Default: var/log/cowrie.
  - **cowrie_download_path**: Path for cowrie to place downloaded/uploaded files for later analysis. Internal cowrie variables are also accepted here. Default: ${honeypot:state_path}/downloads
  - **cowrie_data_path**: Path relative to `cowrie_dir` for data. Default: data
  - **cowrie_share_path**: Path relative to `cowrie_dir` for shared data. Default: share/cowrie
  - **cowrie_state_path**: Path relative to `cowrie_dir` for state data. Default: var/lib/cowrie
  - **cowrie_etc_path**: Path relative to `cowrie_dir` for configuration files. Default: etc
  - **cowrie_contents_path**: Path relative to `cowrie_dir` that contains contents of files on the virtual filesystem. Default: honeyfs
  - **cowrie_txtcmds_path**: Path relative to `cowrie_dir` that contains the txtcmd files. Default: txtcmds
  - **cowrie_ttylog**: Boolean to control logging of tty session. Default: true
  - **cowrie_ttylog_path**: Path relative to `cowrie_dir` where tty logs should be stored. Default {{ cowrie_log_path }}/tty
  - **cowrie_interactive_timeout**: Timeout waiting for interactive logon in seconds. Default: 120
  - **cowrie_auth_class**: Authentication class setting. Value should be one of UserDB or AuthRandom. Default: UserDB
  - **cowrie_backend**: What kind of backend should be presented to the attacker. Value should be one of shell or proxy. Default: shell
  - **cowrie_filesystem**: Location of virtual filesystem contents. Internal cowrie variables are also accepted here. Default: "${honeypot:share_path}/fs.pickle"
  - **cowrie_processes**: Location relative to `cowrie_dir` of json file containing process information. Used with the `ps` command. Default: share/cowrie/cmdoutput.json
  - **cowrie_arch**: Fake architectures/OS of the honeypot. Default: linux-x64-lsb
  - **cowrie_kernel_version**: Kernel version displayed in the honeypot. Default: 3.2.0-4-amd64
  - **cowrie_kernel_build_string**: Kernal build string displayed in the honeypot. Default: #1 SMP Debian 3.2.68-1+deb7u1
  - **cowrie_hardware_platform**: Hardware platform displayed in the honeypot. Default: GNU/Linux
  - **cowrie_ssh_enabled**: Boolean to control if SSH is used to access the honeypot
  - **cowrie_rsa_public_key**: Location of public half of rsa host key. Internal cowrie variables are accepted here. Default: ${honeypot:state_path}/ssh_host_rsa_key.pub
  - **cowrie_rsa_private_key**: Location of private half of rsa host key. Internal cowrie variables are accepted here. Default: ${honeypot:state_path}/ssh_host_rsa_key
  - **cowrie_dsa_public_key**: Location of public half of dsa host key. Internal cowrie variables are accepted here. Default: ${honeypot:state_path}/ssh_host_dsa_key.pub
  - **cowrie_dsa_private_key**: Location of private half of dsa host key. Internal cowrie variables are accepted here. Default: ${honeypot:state_path}/ssh_host_dsa_key
  - **cowrie_ssh_version_string**: Version shared when connections are attempted. Default: SSH-2.0-OpenSSH_6.0p1 Debian-4+deb7u2
  - **cowrie_ssh_listen_endpoints**: Endpoints to listen for new connections on. This should be in the format accepted by [twisted](https://twistedmatrix.com/documents/current/core/howto/endpoints.html#servers). Default: tcp:{{ cowrie_port_priv }}:interface=0.0.0.0
  - **cowrie_sftp_enabled**: Flag to control if sftp connections are accepted to upload/download files from the honeypot.
  - **cowrie_ssh_forwarding**: Flag to control if ssh forwarding should be allowed. Default: false
  - **cowrie_forward_redirect**: Flag to forward ports to other address/honeypots. Further configuration of this functionality is required and not yet implemented. Default: false
  - **cowrie_userdb_location**: Location of the userdb file on the Ansible system. When overridden, it is relative the `files` directory in the playbook's directory. Default: userdb.txt
  - **cowrie_manager**: The service manager to use to start, stop, and restart the cowrie service. Accepted values are `native`, and `systemd`. Default: systemd

Dependencies
------------

None

Example Playbook
----------------

An example playbook that will install cowrie using all the default settings is.
```yml
---
# site.yml
- hosts: servers
  become: yes
  roles:
  - lksnyder0.cowrie
```

License
-------

[BSD 2-Clause (License FreeBSD/Simplified)](https://tldrlegal.com/l/freebsd)
