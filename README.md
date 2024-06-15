# absm
A BTRFS Snapshot Manager

PRESENTATION

This shell script is an accessible to the blind and easy to use btrfs snapshots manager, mostly
intended to allow booting and restoring a system in its previous state if can't boot or does not
work as expected otherwise. The "on demand" boot entries are associated with a hot key bearing their
number (most recent first) so users can just press this key to boot a wanted snapshot.

Each snapshot bears as name its creation date, and all creations, deletions and restorations
are logged.

This script allows to take snapshots of the root subvolume of a system
mounted as /, allowing to boot off one of them from the GRUB boot menu,
and also to rollback the system restoring one of them.

A new snapshot is taken just before a rollback to enable it being rolled back.
This is optional if the snapshot being rolled back is runnning.

It require that the directories / and /home be mounted as btrfs subvolumes.
Currently it takes snapshots of / as a whole, /home excluded.

WARNING AND LIMITATIONS

As the snapshots are stored in the same device as the root subvolume, this
script does not protect against a hardware failure. This is not a backup tool.

We assume a single-device file system, without RAID (other configurations have not been tested).

The snapshots are currently read-write, so not suitable "as-is" for incremental btrfs send (like for back-ups on external devices).

REQUIREMENTS

* The btrfs file system should be used for the "core" system (see below).
* The package btrfs-progs should be installed.
* GRUB should be the boot loader and manager in use.
* It is expected that the "core" system (including directories like /, /usr, /bin, /sbin, /etc, /lib*) be mounted in /etc/fstab as a subvolume, with Copy On Write (COW) enabled and the option "subvol=..."
* Directories that you do not want to include in the snapshots should be in their separate subvolumes, and not in the core system's subvolume (flat layout).
* The w3m browser/pager is recommended, especially to users relying on a braille device or a screen reader.

`absm` has been tested on several distributions: Slint-15.0, Ubuntu-22.04.1, Mint-21.1-cinnamon, Garuda-lxqt-22.10.19, Manjaro-xfce-22.0-221224, Endeavour OS_Cassini_22_12 and Fedora-Workstation-37-1.7.

Please let us know posting an issue if you have tried it in another one and the outcome of this test.

INSTALLATION

Just copy `absm` somewhere you deem appropriate. You may run it like `sh /path/to/absm` or make it executable and run it as `/path/to/absm`.

USAGE

If run without argument, the script starts by making some checks, then if
successful greets the user in a menu with entries allowing to:
. Create a snapshot.
. Delete a snapshot.
. Rollback the system to a previous state, recorded in a snapshot.
. List the existing snapshots.
. Display the GRUB boot menu including the snapshots, as it will appear after
  a reboot. Note: in some cases le menu displayed can differ from the one
  appearing after a reboot.
. Display this file.
. Display how to use snapshots and manage rollbacks.
. View the log of creations, deletions and restorations of snapshots.

The menu is context-driven: it does not propose to delete, list or restore
snapshots is there is none. If run from a snapshot it only allows to display the
GRUB boot menu or restore this snapshot.

The command "snapshots-manager c" creates a new snasphot unattended, generally
from a script. Then if the variable `SHORTDESC` is set, its value is displayed in
the entry for this snapshot in the GRUB boot menu. If the variable `LONGDESC` is
set it is displayed in the log.

NOTE

The command "absm c" creates a new snasphot unattended, usually from a script.
Then if the variable SHORTDESC is set, its value is displayed in the entry for
this snapshot in the GRUB boot menu. If the variable LONGDESC is set it is
displayed in the log.

