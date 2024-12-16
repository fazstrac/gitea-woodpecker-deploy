# Deploy Gitea+PostgreSQL and Woodpecker-CI via Quadlet

This repository contain my take on how to deploy [Gitea](https://about.gitea.com/) + [PostgreSQL](https://www.postgresql.org/) and [Woodpecker-CI](https://woodpecker-ci.org/) into two separate [Podman](https://podman.io) PODs. I wrote as a document to myself and to remind me how to do this, should my homelab fail at some point.

## Instructions how to use

Gitea+Postgresql:

```shell
mkdir -p $HOME/.config/containers/systemd/
cp gitea/gitea.network $HOME/.config/containers/systemd/
systemctl --user --daemon-reload

cp gitea/gitea*container gitea/gitea.pod $HOME/.config/containers/systemd/
systemctl --user --daemon-reload
systemctl --user start gitea-pod.service
```

and similarly for Woodpecker-CI:

```shell
mkdir -p $HOME/.config/containers/systemd/
cp woodpecker-ci/woodpecker.network $HOME/.config/containers/systemd/
systemctl --user --daemon-reload

cp woodpecker-ci/woodpecker*container woodpecker/woodpecker-ci.pod $HOME/.config/containers/systemd/
systemctl --user --daemon-reload
systemctl --user start woodpecker-ci-pod.service
```

If you want these services to start on system startup, you will need to also to do this:
```shell
sudo loginctl enable-linger container-user
```
where `container-user` is the user you created to run the pods. This needed `systemd-container` to be installed.

## Tested

Using Debian 12 and Podman 5.3.1 (needs to enable testing repos).
