# common

- Installs common packages
- Creates user accounts and adds them to the `wheel` group
- Removes `root`s password
- Sets language, locales and keyboard layout
- Configures hostname settings

## Requirements

A Debian-based distribution.

## Role Variables

| Name                      |        Mandatory / Default        | Description                                                                              |
|:--------------------------|:---------------------------------:|:-----------------------------------------------------------------------------------------|
| `admins`                  |                `[]`               | list of admin users, see [User configuration](#User configuration)                       |
| `locales`                 |          `[en_US.UTF-8]`          | list of locales to install                                                               |
| `keyboard_layout`         |              `us,de`              | TTY keyboard layout                                                                      |
| `default_language`        |           `en_US.UTF-8`           | default language                                                                         |
| `apt_repository_versions` |         `[main, universe]`        | List of apt repository versions to enable                                                |
| `common_debian_sources`   |                                   | Verbatim sources.list definition for Debian. Overrides the usual sources.list generation |
| `common_ubuntu_sources`   |                                   | Verbatim sources.list definition for Ubuntu. Overrides the usual sources.list generation |
| `common_packages`         | see [defaults](defaults/main.yml) | Common packages to install                                                               |

### User configuration

Each entry in the `admins` list shall be a username (used to log in), which is a dict containing the following entries:

| Name     | Mandatory / Default | Description                                                                                                           |
|:---------|:-------------------:|:----------------------------------------------------------------------------------------------------------------------|
| `name`   |  :heavy_check_mark: | the full name of the admin user                                                                                       |
| `shell`  |     `/bin/bash`     | path to the shell that shall                                                                                          |
| `keys`   |         `[]`        | list of ssh keys that allow this user to login via SSH                                                                |
| `passwd` |                     | [hashed](http://docs.ansible.com/ansible/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module) passphrase |

## Example Playbook

```yml
- hosts: web01
  roles:
    - role: common
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
      default_language: de_DE.UTF-8
      keyboard_layout: de
      locales:
        - de_DE.UTF-8
        - en_US.UTF-8
```

## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

## Author Information
* [Markus Mroch (Mr. Pi)](https://github.com/Mr-Pi) _markus.mroch@stuvus.uni-stuttgart.de_
