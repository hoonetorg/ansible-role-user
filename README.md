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
      groups: [wheel]            # optional supplementary groups
      password: "{{ vault_alice_password_hash }}"   # optional, crypt hash
      update_password: on_create # optional
      shell: /bin/bash           # default /bin/bash
      home: /home/alice          # default /home/<name>
      create_home: true          # default true
      state: present             # default present
```

The home directory is created/owned (mode `0700`) before the user exists, so it also works when the home
is a separate mount (e.g. a btrfs subvolume created by ansible-role-disk).

## License

Apache-2.0

Created with the help of AI
