# Home directory
The home directory is always created on `egil-analyze`, and then mounted on all other nodes on the cluster using the NFS service. In practice, this means that the home directory is mirrored on all nodes, with the same contents and permissions. Similarly, all users are syncronized across the entire cluster with the same password, UID and GID.

## Syncronizing files and directories
### `rsync`
Files and directories can be syncronized from a remote (non-cluster) computer using `scp`or `rsync`. `rsync` should be the preferred command, as it supports more advanced filtering and is the successor of `scp`. To move a file `/home/user/myfile` from `node1` to `node2`, use

```bash
rsync -av node1:/home/user/myfile node2:/home/user/myfile
```
and similarly, a directory `mydir`can be moved using

```bash
rsync -av node1:/home/user/mydir node2:/home/user/mydir
```
If the directory already exists, only the files with the same name will be overwritten.

### `git`
The cluster has internet connection to the world, and provides permissions for `http`, `https` and `ssh` through a redsocks proxy. Therefore, `git` can be used to fetch external repositories directly to your home directory, typically using

```bash
git clone https://github.com/username/reponame.git
```

## Storage space
All users share the same storage space for the home directory. Please respect that and do not occupy unreasonably amount of space. If the file system gets filled up, all jobs will stop for all users across the entire cluster. To check how much space your home directory occupies, execute

```bash
du -lh --max-dept=1
```
in your home directory. This might take some time if you have many files. The home directories are physically stored on `egil-analyze`, which has 6 x 14TB disks set up with RAID 10. All data is actually stored twice for security reasons, and for that reason the total storage space is 42TB. To check how much space that is available in the disks, execute

```bash
df -lh
```
on `egil-analyze`.
