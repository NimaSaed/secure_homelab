### Creating a systemd Service for a Podman Pod

To create a `systemd` service file for managing a Podman pod:

1. **Generate the systemd unit file**
   Use `podman generate systemd` with the `--new` flag to avoid generating a service for a currently running container. This ensures a create a new container out of the image:

   ```bash
   podman generate systemd --new --files --name <podName>
   ```

   > ⚠️ If you use the same name for both the pod and a container, only the container’s service will be generated. Make sure the pod and container names are unique if you want a pod-level service.

2. **Move the generated service files to the user systemd directory**
   Move the `.service` file(s) to the appropriate location in your user’s systemd configuration directory:

   ```bash
   mv generate_files ~/.config/systemd/user/
   ```

3. **Reload the systemd user daemon**
   Inform systemd about the new unit file:

   ```bash
   systemctl --user daemon-reload
   ```

4. **Enable and start the service**
   Use `--now` to both enable the service at boot and start it immediately:

   ```bash
   systemctl --user enable --now pod-<podName>.service
   ```
