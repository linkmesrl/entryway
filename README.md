# Entryway

Simple startup scripts for docker-compose'd node services. Also the first level in Doom.

Once installed this module exposes two  global commands:

## Auth key:
```
auth_key
```
It copies a key from a volume mapped in /root/ssh to /root.ssh, and changes its permissions to 400.
This is a workaround for the permission issues on windows machines on mapped volumes.

## Checkout
```
checkout
```
Clones and updates a git repository. You can specify an address in the $REPO env var.

## Troubleshooting
"auth_key / checkout: command not found": Check that you actually have this installed on your sistem.
 If you are installing this via Dockerfile try rebuilding your service with --no-cache

```
 docker-compose build <service>
 ```

## Sample usage (docker-compose file)

```yml
version: '2'
services:
  mysrv:
    build: .
    volumes:
      - .:/src
      - ./auth_keys:/root/ssh
    ports:
      - "3000:3000"
    depends_on:
      - "mysrv2"
    environment:
    # remove rollback if you want to preserve data

    command: bash -c "auth_key #just import the out key
      && npm run rollback
      && npm run migrate
      && npm install
      && npm start"

  mysrv2:
    build: .
    volumes:
      - ./auth_keys:/root/ssh
    ports:
      - "3001:3001"
    depends_on:
    environment:

      # config target repo and branch in env vqr
      REPO: git@myorg.com:repo.git
      BRANCH: master

    #cuse the chekout command to import keys ANd checkout current repo and branch
    command: bash -c "checkout && npm install && node index.js -c config/local"
```
