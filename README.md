# docker-cp
Copy docker volumes between machines

This simple bash script allows you to copy docker volumes between remote/local machines!

Syntax:

`docker-cp [source_hostname:]<source_volume> [dest_hostname:]<dest_volume>`

Requirements:

You must be ablet to ssh into any remote hosts you name!

This script transfers volume files with a fairly simple mechanism:
- It spins up an alpine container on the source machine, mounting the source volume
- The alpine container writes a `tar`ball to stdout, which is written to your local machine in a temporary directory
- The temporary `tar`ball is then fed into another alpine instance in the destination machine, which also mounts the named volume
- The new alpine instance then extracts the `tar`ball into the destination volume

Either (or both) the source or destination machines may be local, which is assumed if no hostname is specified for them. This is probably not the most efficient method for creating local-only copies of docker volumes but it will do that just fine.
