# proxsnapclone
Atomic clone of Proxmox LXC container filesystem

This script automatically creates snapshot of Proxmox LXC container and mounts
its whole filesystem (read only). Then it does whatever you want - just edit
do_your_job() function below. (The example code calls rclone - might be useful
for cloud backups.) Finally it destroys the snapshot.

This script might be useful when you need access to "atomic" state of
conatainer filesystem - for example in order to backup it to a cloud.

Note:
Whenever possible - avoid using this script!
It is generally bad idea to access container filesystem "from outside".
(Whole point of containerization is to access containerized data
from the container only.)
The problem is that you cannot create snapshot of the container filesystem
from inside of the container. This scrips solves the problem.

# Important:
This script works with ZFS only.
This scipit must be run on PVE node.
This script is to be run as root.

# Prerequisites:
Install fuse-overlayfs. ("apt install fuse-overlayfs")

# Usage:
Edit do_your_job() function below. You may use cmdline parameters $2 and following.

Run it:

    proxsnapclone <numeric_vmid> <your_args>
  
Example:

    proxsnapclone 100 --create-empty-src-dirs --transfers=24 --progress --dry-run srv/my/very/important/data remote:folder

