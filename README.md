# ansible-role-user

Creates groups and local users and sets the ownership of their home directories.

## Variables

```yaml
user:
  groups:
    - name: examplegroup
      gid: 2000                  # optional
  users:
    - name: alice
      uid: 2001                  # optional
      group: examplegroup        # primary group, default: user name
      groups: [libvirt]          # secondary groups: exactly these, all others are removed (default: none)
      sudo: true                 # sudo rule in /etc/sudoers.d (default false)
      sudo_nopasswd: false       # sudo without password (default false)
      password: "{{ vault_alice_password_hash }}"   # optional, crypt hash
      update_password: on_create # optional
      shell: /bin/bash           # default /bin/bash
      home: /home/alice          # default /home/<name>
      create_home: true          # default true
      state: present             # default present
```

Secondary groups are desired state: a user gets exactly the groups listed in `groups`; groups added by hand,
by packages or by the installer (e.g. Agama's `wheel` for the first user) are removed on the next run. No
`groups` = no secondary groups.

`sudo: true` writes `/etc/sudoers.d/50-ansible-user-<name>` (validated with `visudo`):
`<name> ALL=(ALL:ALL) ALL` with the user's own password (`Defaults:<name> !targetpw`, needed on openSUSE where
the base sudoers asks for root's password). It works independently of distribution groups like `wheel` or
`sudo`. `sudo: false` or `state: absent` removes the file. The sudo files are written before the groups are
changed, so a user losing `wheel` keeps sudo.

The home directory is created/owned (mode `0700`) before the user exists, so it also works when the home
is a separate mount (e.g. a btrfs subvolume created by ansible-role-disk).

## License

Apache-2.0

Created with the help of AI
