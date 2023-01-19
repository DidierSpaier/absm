# absm
A BTRFS Snapshot Manager

PRESENTATION

This shell script is an accessible to the blind and easy to use btrfs snapshots manager, mostly
intended to allow booting and restoring a system in its previous state if can't boot or does not
work as expected otherwise. The "on demand" boot entries are associated with a hot key bearing their
number (most recent first) so users can just press this key to boot a wanted snapshot.

Each snapshot bears as as name its creation date, and all creations, deletions and restorations
are logged.

`absm` allows to rescue a system damaged because of a faulty update or a
user error, booting off a snapshot taken before the incident from the GRUB boot
menu.

The snapshots can be created on demand running the script on the command line,
or by another script running it to create snapshots upon events like after
booting or before a package update, or periodically through a cron job.

WARNING AND LIMITATIONS

As the snapshots are stored in the same device as the root subvolume, this
script does not protect against a hardware failure. This is not a backup tool.

We assume a single-device file system, without RAID (at least other configurations have not been tested).

The snapshots are currently read-write, so not suitable "as-is" for incremental btrfs send (like for back-ups on external devices). This may change.

REQUIREMENTS

* The btrfs file system should be used for the "core" system (see below).
* The package btrfs-progs should be installed.
* GRUB should be the boot loader and manager in use.
* It is expected that the "core" system (including directories like /, /usr, /bin, /sbin, /etc, /lib*) be mounted in /etc/fstab as a subvolume, with Copy On Write (COW) enabled and the option "subvol=..."
* Directories that you do not want to include in the snapshots should be in their separate subvolumes, and not in the core system's subvolume (flat layout).
* The w3m browser/pager is recommended, especially to users relying on a braille device or a screen reader. 

INSTALLATION

Just copy `absm` somewhere you deem appropriate. You may run it like `sh /path/to/absm` or make it executable and run it as `/path/to/absm`.

USAGE

If run without argument, the script starts by making some checks, then if
successful greets the user in a menu with entries allowing to:
* Create a snapshot.
* Delete a snapshot.
* Restore a snapshot (put the system back in a previous state).
* List the existing snapshots.
* Display the GRUB boot menu including the snapshots, as it will appear after
  a reboot.
* Display most of the information you are reading.
* View the log of creations, deletions and restorations of snapshots. 

The menu is context-driven: it does not propose to delete, list or restore
snapshots is there is none. If run from a snapshot it only allows to display the
GRUB boot menu or restore this snapshot.

The command "snapshots-manager c" creates a new snasphot unattended, generally
from a script. Then if the variable `SHORTDESC` is set, its value is displayed in
the entry for this snapshot in the GRUB boot menu. If the variable `LONGDESC` is
set it is displayed in the log.

NOTE

A snapshot is always taken before restoring another snapshot. This allows to get
back after restoration updates of files that occured since the creation of the
restored snapshot, like for instance databases or system logs, gathering them
from the snapshot taken before restoration.

To access the snapshots, type as root:
    `mount <root device> /mnt -o subvolid=0`
The snapshots are in /mnt/SNAP_DIR, with their creation's date as name, and the log
is /mnt/log. Do not to modify the content of a snapshot, so copy files from a
snapshot to the restored system, not the other way round.

TODO

* Provide an "How to" to create from scripts periodic or upon event snapshots.
* provide an how-to to compare the content of subvolumes and copy files and directories
from a snapshot to a running system. 
