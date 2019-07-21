# common

- Installs commonly used packages
- Creates administrator accounts and adds them to the `wheel`, `ssh-users` and `sudo` groups
- Removes `root`s password
- Sets language, locales and keyboard layout
- Configures hostname settings (hostname and `/etc/hosts`)

## Requirements

A Debian-based distribution.

## Role Variables

| Name                                  | Mandatory / Default                        | Description                                                                                                                               |
|:--------------------------------------|:------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------|
| `common_admin_users`                  | `{}`                                       | Dict of admin users, see [User configuration](#User configuration)                                                                        |
| `common_users`                        | `{}`                                       | Dict of normal users, see [User configuration](#User configuration)                                                                       |
| `common_jumphost_users`               | `{}`                                       | Dict of users that are allowed to use this host as SSH proxy jump host, see [User configuration](#User configuration)                     |
| `common_default_admin_user_groups`    | `[adm,dialout,users,wheel,ssh-users,sudo]` | Default groups to set for admin users                                                                                                     |
| `common_default_user_groups`          | `[]`                                       | Default groups to set for normal users                                                                                                    |
| `common_default_jumphost_user_groups` | `[ssh-users]`                              | Default groups to set for JumpHost users                                                                                                  |
| `common_ensure_system_groups`         | `[ssh-users,wheel,sudo]`                   | List of groups to create                                                                                                                  |
| `common_domain`                       | `localdomain`                              | Search domain to set in `/etc/hosts`                                                                                                      |
| `common_locales`                      | `[en_US.UTF-8]`                            | List of locales to install                                                                                                                |
| `common_keyboard_layout`              | `us,de`                                    | TTY keyboard layout                                                                                                                       |
| `common_default_language`             | `en_US.UTF-8`                              | Default language                                                                                                                          |
| `common_core_packages`                | see [defaults](defaults/main.yml)          | Common core packages to install (like locales or iproute2)                                                                                |
| `common_extra_packages`               | see [defaults](defaults/main.yml)          | Common extra packages to install (like less or htop)                                                                                      |
| `common_custom_packages`              | `[]`                                       | Custom packages to install, useful if you want to have some extra packages installed without copying and overriding the two lists abvove  |
| `common_core_keys`                    | `[]`                                       | Common core repo key urls to install                                                                                                      |
| `common_extra_keys`                   | see [defaults](defaults/main.yml)          | Common extra repo key urls to install                                                                                                     |
| `common_custom_keys`                  | `[]`                                       | Custom repo key urls to install, useful if you want to have some extra keys installed without copying and overriding the two lists abvove |
| `common_core_repos`                   | `[]`                                       | Common core repo urls to install                                                                                                          |
| `common_extra_repos`                  | see [defaults](defaults/main.yml)          | Common extra repo urls to install                                                                                                         |
| `common_custom_repos`                 | `[]`                                       | Custom repo urls to install, useful if you want to have some extra repos installed without copying and overriding the two lists abvove    |

### User configuration

Each key in the respective users dict shall be a username (used to log in), with the following dict as a value:

| Name            | Mandatory / Default | Description                                                                                                                                                                       |
|:----------------|:-------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`          | :heavy_check_mark:  | The full name of the admin user                                                                                                                                                   |
| `shell`         | `/bin/bash`         | Path to the shell to set for this admin (this key is ignored for `common_jumphost_users`)                                                                                         |
| `keys`          | `[]`                | List of SSH keys that allow this user to login or proxy-jump via SSH                                                                                                              |
| `passwd`        |                     | [hashed](http://docs.ansible.com/ansible/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module) passphrase                                                             |
| `allowed_hosts` | `[]`                | List of hosts and ports to which the user is allowed to jump to (via the current host). The format is `{{ ip }}:{{ port }}`. This key is only parsed for `common_jumphost_users`. |
| `groups`        | `[]`                | List of groups the user will be added to. This key overrides the default groups mentioned above (`common_default_user_groups` etc.)`.                                             |

## Example Playbook

```yml
- hosts: web01
  roles:
    - role: common
      common_admin_users:
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
          groups:
            - users
            - ssh-users
            - wheel
      common_jumphost_users:
        tim:
          name: Tim Mustermann
          keys:
            - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5aaaaIEFmmHsB7LgVMmujy51QfoSS9hnN7GMEm+Mkcg1YVJnn timey
          allowed_hosts:
            - 196.168.1.5:22
      common_users:
        tom:
          name: Tom Haverford
          groups:
            - users
            - ssh-users
      common_default_language: de_DE.UTF-8
      common_keyboard_layout: de
      common_locales:
        - de_DE.UTF-8
        - en_US.UTF-8
```

## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

## Author Information
* [Markus Mroch (Mr. Pi)](https://github.com/Mr-Pi) _markus.mroch@stuvus.uni-stuttgart.de_
