# Entryway

Once installed this module exposes two  global commands:

## Auth key:
It copies a key from a volume mapped in /root/ssh to /root.ssh, and changes its permissions to 400.
This is a workaround for the permission issues on windows machines on mapped volumes.

## checkout
Clones and updates a git repository. You can specify an address in the $REPO env var.
