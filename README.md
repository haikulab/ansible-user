ansible-user for Debian/Ubuntu
============

Ansible Role for adding admin users to the system.

> Depends on the **init** role

## Overwrite Default Variables

The `vars/main.yml` file should contain your list of packages you want to install in order to override defaults found in `defaults/main.yml`.

```yml
---
admin_username: some_admin
admin_password: some_password
admin_public_key: "{{ lookup('file', '/path/to/public/key' ) }}"
```

Additionally, you can overwrite the variables as part of your playbook.

```yml
---
...
  vars:
    admin_username: some_admin
    admin_password: some_password
    admin_public_key: "{{ lookup('file', '/path/to/public/key' ) }}"
...
```

## The lookup

This method, will get a value of a given file from anywhere on the system. Just replace the `/path/to/public/key` with an actual path to a ssh key file.

For example:

```
"{{ lookup('file', '/home/username/.ssh/id_rsa.key' ) }}"
```

## Generating User Passwords

__Originally copied from [Servers For Hackers - Ansible User Example](https://github.com/Servers-for-Hackers/ansible-user-example)__

Linux passwords are encrypted using SHA-512. Ansible's documentation on [generated encrypted passwords](http://docs.ansible.com/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module) point out the command `mkpasswd --method=SHA-512`. This asks for a password and returns the encrypted version of it after encrypting it used the SHA-512 method.

On Ubuntu 14.04, the `mkpasswd` command comes with package `whois`:

    sudo apt-get install -y whois

After installing `whois`, you can use the `mkpasswd` command.

```bash
mkpasswd --method=SHA-512
Password: <enter your password here>
$6$hCDK.2eB3VXD4$fz95AiqRvc7DHbFWYMbTiRWJza5SCHclueFkISsivF3u6dDkHQmIds1uNrVb5Fk6.6WEes6iQ25GuJx0Fteos/
```

This generated hash should be what you set as your `admin_password` and `deploy_password` values.

Alternatively, you can use `openssl passwd -salt <salt> -1 <plaintext>`

## Debugging

If you run into errors, uncomment the `- debug: msg="{{ ... }}"` statements.
