# Access and login

## Getting access
The cluster is funded by the Norwegian Research Council for projects at the Center for Computing in Science Education (CCSE), History-dependent friction and BioAI. Simulations related to these projects will be prioritised. This includes mainly molecular dynamics simulations of geological processes and training machine learning algorithms.

If you need access to the cluster and work on one of the mentioned projects, please contact Henrik Andersen Sveinsson (h.a.sveinsson@fys.uio.no).

## Login
The cluster is on UiO's lab network and can only be reached through `lab-login.uio.no`. `lab-login.uio.no` does not allow SSH jumping (`ssh -J`, `ProxyJump`, `ProxyCommand`) or port forwarding (`ssh -L`), so you have to log in one step at a time.

First, log in to `login.uio.no` with your UiO credentials:
```bash
ssh <uio-username>@login.uio.no
```
Then, log in to `lab-login.uio.no`:
```bash
ssh <uio-username>@lab-login.uio.no
```
Then log in to `mn-fys-ccse-nat.lab.uio.no`:
```bash
ssh <uio-username>@mn-fys-ccse-nat.lab.uio.no
```
In the last step, you use your cluster credentials and not the UiO credentials.

You can also do all three steps in one command. You will be asked for each password in turn:
```bash
ssh -t <uio-username>@login.uio.no ssh -t lab-login.uio.no ssh -t mn-fys-ccse-nat.lab.uio.no
```

From `mn-fys-ccse-nat.lab.uio.no`, you reach the cluster nodes with `ssh <nodename>`. The main nodes that one usually wants to access are:

| Nodename     | Type | Specifications         | IP            |
|--------------|------|------------------------|---------------|
| egil-analyze | GPU  | 1x GeForce RTX 3090    | 10.255.80.12  |
| bigfacet     | GPU  | 8x P100-PCIE-16GB      | 10.255.80.13  |
| hugefacet    | GPU  | 4x A100-PCIE-40GB      | 10.255.80.11  |
| greatfacet   | GPU  | 6x A100-PCIE-40GB      | 10.255.80.14  |
| bioai        | GPU  | 2x GeForce RTX 2080    | 10.255.80.16  |
| neuroai      | GPU  | 1x GeForce RTX 3090    | 10.255.80.17  |
| egil         | CPU  | 72x Intel Xeon E5-2670 | 10.255.80.136 |
| griffith     | CPU  | 2x AMD EPYC 7763       | 10.255.80.18  |

The name `griffith` does not resolve on `mn-fys-ccse-nat.lab.uio.no`, so use its IP address: `ssh 10.255.80.18`.

A molecular dynamics benchmark of the various computational devices can be found on [lammps-bulk-benchmark](https://github.com/evenmn/lammps-bulk-benchmark).

The compute nodes can be reached with `ssh <uio-username>@compute-yxx` (ex. `ssh compute-a00`).

To get from your laptop/workstation to the cluster with a single `ssh egil-cluster`, add the following to your `~/.ssh/config`:
````{admonition} SSH config
    :class: dropdown, note

```bash
Host egil-cluster
    HostName login.uio.no
    User <uio-username>
    RequestTTY yes
    RemoteCommand ssh -t lab-login.uio.no ssh -t mn-fys-ccse-nat.lab.uio.no
```
````
This only logs you in. Because `lab-login.uio.no` does not allow SSH jumping, `ProxyJump` entries for the individual nodes do not work, and you cannot use `scp` or `rsync` directly between your laptop/workstation and the cluster.

To avoid typing your UiO password every time, you can copy your SSH key to your UiO home directory. If you do not already have a key, generate one with `ssh-keygen -t ed25519`. Then run
```bash
ssh-copy-id <uio-username>@login.uio.no
```
`mn-fys-ccse-nat.lab.uio.no` does not mount the UiO home directories, so you will still be asked for your cluster password in the last step.
