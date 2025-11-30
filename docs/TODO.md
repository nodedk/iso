# TODO

- [ ] Create @nodedk/pkg (with NodeJS SEA)

- [ ] Create @nodedk/iso
  Simpler than before: no nodejs, no git

  - [ ] Create Debian Linux image
  - [ ] Set up user for SSH and apps
    - [ ] Disable root login
  - [ ] Install CouchDB
  - [ ] Install ssh
  - [ ] Install Caddy
  - [ ] Install ufw
  - [ ] Install fail2ban
  - [ ] Install MongoDB (enable should be)
  - [ ] Verify that pkg-app runs

- Create eldoy/infra
  - config for each server (like terraform, but simpler)
  - scripts that operate on infrastructure over SSH
    - kubernetes "light"
    - should produce reproducable server environments
    - just add some config and run "apply"
    - commands:
      - apply
      - upgrade
      - stats
      - verify

- Update @nodedk/cli
  - [ ] build should use @nodedk/pkg to write app file
  - [ ] deploy should use rsync dist
    - [ ] zero-downtime caddy port switch
      - use port(-1) to auto-assign port

- [ ] Move all apps over to this
- [ ] New servers for all apps