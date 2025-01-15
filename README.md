# Podman, powered by Ansible
This role provides you with a minimal installation of Podman, ready for use:

* If enabled, configures a useraccount to host your containers
* Configure storage
* Configure custom registires

# Setup
* Include role in project
* Set variables (see defaults/main.yml)
* Deploy role
* ???
* Profit!

## Controlling containers in a rootless setting
By default the podman user is created without a shell. Which can make accessing logs and controlling containers a bit difficult as not all components play nice when using ```sudo -u podman bash``` to create a new session.

If you want to control podman or systemd services while running rootless, consider adding the following functions to your shell

* SystemD user services wrapper
```
function podsystemctl() {
  sudo systemctl --machine=podman@.host --user $@
}
```
* Podman command wrapper
```
function pod() {
  cd /tmp
  sudo -u podman podman $@
  cd - &>/dev/null
}
```
* JournalD logging, note that you need to UID of your podman user
```
function podjournalctl() {
  sudo journalctl _UID=$(id podman -u) $@
}
```
