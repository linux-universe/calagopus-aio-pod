# calagopus-aio-pod

The [calagopus aio compose file](https://raw.githubusercontent.com/calagopus/panel/refs/heads/main/compose.aio.yml) rewritten for podman quadlet

1. `mkdir -p ~/data/calagopus/db ~/data/calagopus/panel/data ~/data/calagopus/panel/logs`

2. `sudo mkdir /etc/calagopus-wings /var/lib/calagopus-wings /var/log/calagopus-wings`
   `sudo chown $(id -u):$(id -g) /etc/calagopus-wings /var/lib/calagopus-wings /var/log/calagopus-wings` (rootless)

3. `echo 'app_name: Calagopus' > wings-config.yml`

4. in `wings-config.yml`:

   ```diff
   -  container_apply_seccomp: true
   +  container_apply_seccomp: false
      log_config:
   -    type: local
   +    type: json-file
   ```
