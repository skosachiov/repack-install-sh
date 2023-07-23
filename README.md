# repack-install-sh

Repack Linux install.sh or install.run applications into rpm or deb packages.

There is quite a lot of software for linux in the "install.sh" format. Developers want to "simplify" the installation process and use a dirty trick (install.sh) that is unacceptable in a corporate environment. The purpose of this project is to show a pipeline for rebuilding such software into deb or rpm.

## environment

- You need to create one or more virtual machine (qcow2 image) to generate rpm/deb. VM guest OS defines resulting package type.
- The user from under which the ansible-playbook is called must be able to control the `virsh`.
- Create vm ansible user with certificate auth and nopassword sudo.
- Make snapshot for rollback to "clear state". `qemu-img snapshot -c snapshot-1 debian10-uefi.qcow2`

## check snapshot

<pre>
qemu-img snapshot -l debian10-uefi.qcow2
Snapshot list:
ID        TAG               VM SIZE                DATE     VM CLOCK     ICOUNT
1         snapshot-0            0 B 2023-01-30 21:29:40 00:00:00.000          0
2         snapshot-1            0 B 2023-07-22 17:32:12 00:00:00.000          0
</pre>

## process single dir

`ansible-playbook rebuild-pkg.yml -i inventories -u ansible -e "var_binaries_dir=/tmp/1c/8_3_21_1484 var_rebuild_role=rebuild-1c"`

## process dir of dirs

`find /tmp/1c -type d -exec ansible-playbook rebuild-pkg.yml -i inventories -u ansible -e "var_binaries_dir={} var_rebuild_role=rebuild-1c" \;`

