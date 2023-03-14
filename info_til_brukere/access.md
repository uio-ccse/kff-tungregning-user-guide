# Access and login

## Getting access
The cluster is funded by the Norwegian Research Council for projects at the Center for Computing in Science Education (CCSE), History-dependent friction and BioAI. Simulations related to these projects will be prioritised. This includes mainly molecular dynamics simulations of geological processes and training machine learning algorithms.

If you need access to the cluster and work on one of the mentioned projects, please contact Henrik Andersen Sveinsson (h.a.sveinsson@fys.uio.no) or Even Marius Nordhagen (evenmn@mn.uio.no).

## Login
The cluster can be reached through UiO's main login node for those who have access. First, login to `login.uio.no`:
```bash
ssh <uio-username>@login.uio.no
```
Then, login to `fys-lab-ccse` via port 51965 on `labbetuss`:
```bash
ssh <uio-username>@labbetuss -p 51965
```
Now, all cluster nodes should be reachable. The main nodes that one usually wants to access are:

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

A molecular dynamics benchmark of the various computational devices can be found on [lammps-bulk-benchmark](https://github.com/evenmn/lammps-bulk-benchmark).

Alternatively, one can go directly from `login.uio.no` to one of the cluster nodes (nodename) by typing:

```bash
ssh -J labbetuss:51965 <nodename>
```

The compute nodes can be reached with `ssh <uio-username>@compute-yxx` (ex. `ssh compute-a00`).

To ease the navigation between your laptop/workstation and the cluster, add the following script to your `~/.ssh/config`:
````{admonition} SSH config
    :class: dropdown, note 

```bash
Host uio
    HostName login.uio.no
    User <uio-username>

Host fys-lab-ccse
    HostName labbetuss
    ProxyJump uio
    Port 51965
    User <uio-username>

Host hugefacet
    HostName 10.255.80.11
    ProxyJump fys-lab-ccse
    User <uio-username>

Host egil-analyze
    HostName 10.255.80.12
    ProxyJump fys-lab-ccse
    User <uio-username>

Host bigfacet
    HostName 10.255.80.13
    ProxyJump fys-lab-ccse
    User <uio-username>

Host greatfacet
    HostName 10.255.80.14
    ProxyJump fys-lab-ccse
    User <uio-username>

Host bioai
    HostName 10.255.80.16
    ProxyJump fys-lab-ccse
    User <uio-username>

Host neuroai
    HostName 10.255.80.17
    ProxyJump fys-lab-ccse
    User <uio-username>

Host egil
    HostName 10.255.80.136
    ProxyJump fys-lab-ccse
    User <uio-username>

Host griffith
    HostName 10.255.80.18
    ProxyJump fys-lab-ccse
    User <uio-username>
```
````
Then a node on the cluster can be reached directly from your laptop/workstation by running
```bash
ssh <nodename>
```

To avoid typing password every time you login to the cluster, the ssh key can be copied from your laptop/workstation your UiO home directory and the cluster home directory. To do this, you have to generate an RSA key by running `ssh-keygen -b 4096 -t rsa` if you do not already have a key. At the moment of writing, `fys-lab-ccse` has not mounted the UiO home directories, so you will have to type your password once to access the cluster.

To copy your ssh key to your UiO home directory, run
```bash
ssh-copy-id <uio-username>@login.uio.no
```

To copy your ssh key to the cluster home directory, run
```bash
ssh-copy-id bigfacet
```
The last command only works if you have updated your SSH config.

