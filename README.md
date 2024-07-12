# Docker Compose MPI Cluster

This project will be a simple Docker Compose configuration for an MPI cluster,
with each node defined by a Dockerfile that provides the environment
for OpenMPI to run in. The purpose is to provide a test environment for
learning distributed computing with MPI on a single machine that is
easy to setup and reproduce.

What we have so far is based on
[this](https://codigos.ufsc.br/setic-hpc/openmpi/-/blob/master/Dockerfile),
but it will need to be modified further to remove configurations that
are specific to their setup and to provide our own example program, since
theirs

## To Do

There's quite a bit to do here. This is just a list of things in no particular order:

__Generally, figure out how to set up an MPI cluster:__

It's fine if this is in a more basic configuration, e.g., just a few computers on a LAN.
This is most likely what you'll find by searching. [This](https://mpitutorial.com/) site
and the books it links to, especially
[this one](https://www.amazon.com/Using-MPI-Programming-Message-Passing-Engineering/dp/0262527391/),
seem like very good places to start.

__Figuring out how to Dockerize this:__

So we need to figure out what things need to be installed into the underlying Linux
container to support MPI and anything that it requires.
Networking will almost certainly be a part of this.

[Here](https://codigos.ufsc.br/setic-hpc/openmpi/-/blob/master/Dockerfile) is an example
of a Dockerfile for an OpenMPI setup in Docker compose, that uses a Python setup for MPI.
This is probably a good starting point for our own efforts at Dockerizing, but we need
to understand how OpenMPI is configured and works in a more standard scenario.

__Some more specific questions:__

+ How do MPI nodes communicate over the network?
+ What configuration needs done for this?
  + E.g., do they use SSH and how is it configured?
  + What software needs to be installed inside the node containers?

__Networking in particular:__

It looks like the example setup we grabbed from the link above is using SSH.
This is something we might need to think about a bit, in reference to keys
and such, and how to configure them inside the containers, which will be created
and destroyed multiple times with a scripted setup and teardown process.

## Thoughts

This is looking like it might be relatively straightforward. Once we've gotten through
this initial part, then we'll need to create an example program to run inside of the
cluster. For that we can probably look at some of the books we have on parallel and
distributed scientific computing. We have several good ones.
