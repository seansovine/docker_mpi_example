# Docker Compose MPI Cluster

This project is a simple Docker Compose configuration for an MPI cluster,
with each node defined by a Dockerfile that provides the environment
for OpenMPI to run in. The purpose is to provide a test environment for
learning distributed computing with MPI on a single machine that is
easy to setup and reproduce.

What we have so far is based on
[this](https://codigos.ufsc.br/setic-hpc/openmpi/-/blob/master/Dockerfile),
with our own modifications and a few things we picked up from other sources,
including [this](https://mpitutorial.com/tutorials/mpi-hello-world/)
nice tutorial, from which we borrowed our example program.

## Notes on Docker

The following commands are useful:

```bash
# Bring up the containers detached from standard out.
docker compose up -d

# Bring down all the containers running as part of the compose session.
docker compose down

# "List running containers for a Compose project."
docker compose ps

# Destroy containers created by this project.
docker rmi docker_mpi_example-mpi_head && docker rmi docker_mpi_example-mpi_node
```

In `compose.yaml` we've given the containers and the network memorable names.
We also usually define:

```shell
alias dkc="docker compose"
```

To run an interactive shell in one of the running containers you can use:

```bash
docker exec -ti <container_name> /bin/bash
```

## Notes on Building and Running programs with OpenMPI

First start an interactive shell session in the `head` container with the
command above. Then in the container shell you can run the following commands
to build and run the test program with OpenMPI.

First, switch to the `mpirun` user and change to the code directory
that is mounted from the host:

```shell
su - mpirun && cd /host_code
```

Then we can build from inside the head container with

```shell
mpicc -o demo demo.c
```

Then we can run the built program with

```shell
mpirun -np 2 -host head,node1 ./demo
```

Note that in general we wouldn't be running our code also on the head node,
but for this simple example doing so shows that the MPI program is running on multiple nodes.

You can also, for example, run the MPI code from the host with the command:

```shell
docker exec -t head mpicc -o /build/demo /host_code/demo.c
docker exec -t --user mpirun head mpirun -np 2 -host head,node1 /build/demo
```

## To Do

Create a more interesting example program, maybe a basic implementation of some
standard distributed numerical algorithm for, say, matrix multiplication.

Update the MPI and/or configuration to allow using more than one processor per node.
Currently using more than one slot per node is giving a "not enough slots available" error.
