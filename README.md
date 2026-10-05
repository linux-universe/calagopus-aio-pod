# calagopus-aio-pod

The [calagopus aio compose file](https://raw.githubusercontent.com/calagopus/panel/refs/heads/main/compose.aio.yml) rewritten for podman quadlet

> [!NOTE]
> Requires **Podman 5.5+**.

## Steps

1. Clone the quadlet files to `/etc/containers/systemd/calagopus`:

   ```shell
   git clone https://github.com/linux-universe/calagopus-aio-pod.git /etc/containers/systemd/calagopus
   ```

   > For rootless setups, clone to `~/.config/containers/systemd/calagopus` instead.

2. Create the data directories:

   ```shell
   mkdir -p ~/data/calagopus/db ~/data/calagopus/panel/data ~/data/calagopus/panel/logs
   mkdir -p /etc/calagopus-wings /var/lib/calagopus-wings /var/log/calagopus-wings
   ```

   > Rootful: run as root, `~` is `/root`. Feel free to change the volume paths in the .container files
   >
   > Rootless: run the first line as your user. Run the second with `sudo`, then hand the directories to your user:
   >
   > ```shell
   > sudo chown "$(id -u):$(id -g)" /etc/calagopus-wings /var/lib/calagopus-wings /var/log/calagopus-wings
   > ```

3. Create `wings-config.yml` next to the quadlet files (adjust the path for rootless):

   ```shell
   echo 'app_name: Calagopus' > /etc/containers/systemd/calagopus/wings-config.yml
   ```

4. Set `APP_ENCRYPTION_KEY` in `calagopus.container`.

5. Reload systemd:

   ```shell
   systemctl daemon-reload
   ```

6. Then start Calagopus:

   ```shell
   systemctl start calagopus-pod
   ```

   > Run steps 5, and 6 with the `--user` argument for rootless setups.

7. In `wings-config.yml`:

   ```diff
   -  container_apply_seccomp: true
   +  container_apply_seccomp: false
      log_config:
   -    type: local
   +    type: json-file
   ```

The panel is available at `http://<host>:8000`, SFTP on port `2022`.

> When using extensions, switch the image to `:heavy-aio` and uncomment the `##heavy` volumes in `calagopus.container`.
