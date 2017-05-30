# common

Install common packages and create all admin user accounts.

## Requirements

A Debian based distribution.

## Role Variables
```yml
admins:
  <username>: #admin user (login name)
    name: <full name>
    shell: <shell to use> #optional, default is "/bin/bash"
    keys:
      - <ssh key to deploy>
      ...
    passwd: <hashed passphrase> #optional set password for user, passphrase needs to be hashed (for more information see: http://docs.ansible.com/ansible/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module)
timezone: <timezone, default is UTC>
```

## Dependencies

A Debian based distribution.

## Example Playbook
### Playbook:

### Vars:
```yml
admins:
  max:
    name: Max Mustermann
    shell: /usr/bin/zsh
    keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5aaaaIEFmmHsB7LgVMmujy51QfoSS9hnN7GMEm+Mkcg1YVJnn max123
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5aaaaIEFmmHsB7LgVMmujy51QfoSS9hnN7GMEm+Mkcg1YVJnn max321
    passwd: $6$mDFWEb5pDY$C9ZTuNjTTSyh0uIBoZALAV6isFY4dO8gBN2/xJ0yX2rejvr2wKp/wMmHwvoC.gD8NaeozxjhWvNHp3rJEJdJj1
  lena:
    name: Lena Mustermann
    keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5aaaaIEFmmHsB7LgVMmujy51QfoSS9hnN7GMEm+Mkcg1YVJnn max123
timezone: Europe/Berlin
```
By default, ssh moduli will be generated. To omit this step, run ansible with the option `--skip-tags "ssh_moduli"`

## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

## Author Information
* [Markus Mroch (Mr. Pi)](https://github.com/Mr-Pi) _markus.mroch@stuvus.uni-stuttgart.de_
