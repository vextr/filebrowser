# Docker Build Clarification

When working with this project, it's important to distinguish between building the application binaries and building the Docker image.

*   **`make build`**: This command will build the frontend (Vue.js application) and the backend (Go application) binaries. It compiles the source code into executable files and static assets. This command **does not** produce a Docker image.

*   **`make build-docker`**: Use this command to build the standard Docker image for the application. It packages the compiled application binaries and all necessary dependencies into a Docker container image.

*   **`make build-docker-slim`**: Use this command to build a smaller, slimmed-down Docker image. This is often preferred for production environments due to reduced image size and attack surface.

Always use `make build-docker` or `make build-docker-slim` when your intention is to create a deployable Docker image.

---

## Retagging and Removing Docker Images

After building a Docker image, you might need to retag it (e.g., to prepare it for pushing to your personal Docker Hub account) or remove older/unwanted tags.

### Retagging an Image

If you've built an image with a default tag (e.g., `gtstef/filebrowser` as specified in the `makefile`) but want to use a different tag (e.g., `your_username/filebrowser`) to push to your own Docker Hub, you can use the `docker tag` command:

```bash
docker tag <current_image_name> <new_image_name>
# Example: docker tag gtstef/filebrowser kmyram/filebrowser
```

This command creates a new tag that points to the same underlying image layers, effectively giving the image another name.

### Removing an Image Tag

To remove a specific tag from a local Docker image (e.g., to clean up local images that you've retagged or no longer need), use the `docker rmi` (remove image) command:

```bash
docker rmi <image_name_to_remove>
# Example: docker rmi gtstef/filebrowser
```

If multiple tags point to the same image, `docker rmi` will only remove the specified tag. The underlying image data will only be deleted if all tags pointing to it are removed. If the tag being removed is the only tag for an image, the image itself will be removed from your local system.

---

## Building Docker Images for Specific Architectures

If you are working on a machine with a different architecture than your deployment environment (e.g., an Apple Silicon Mac (`arm64`) wanting to deploy to an `amd64` server), you need to explicitly tell Docker to build the image for the target architecture.

You can achieve this by adding the `--platform` flag to your `docker build` commands. We have already updated the `makefile` targets for `build-docker` and `build-docker-slim` to include this for `linux/amd64`.

To build an `amd64` image:

```bash
# For the standard image
make build-docker # This will now build for linux/amd64 due to makefile modifications

# For the slim image
make build-docker-slim # This will now build for linux/amd64 due to makefile modifications
```

If you were to manually run `docker build` (which is generally not recommended if `make` targets exist), you would include the flag directly:

```bash
docker build --platform linux/amd64 --build-arg="VERSION=testing" -t your_tag -f _docker/Dockerfile .
```
This ensures that the resulting image is compatible with the `amd64` architecture.

To build for a Raspberry Pi 5 (`arm64`), use:

```bash
docker build --no-cache --platform linux/arm64 --build-arg="VERSION=testing" --build-arg="REVISION=n/a" -t gtstef/filebrowser:arm64 -f _docker/Dockerfile .
```
*Note: Using `--no-cache` is highly recommended to ensure frontend changes are correctly built into the Docker layers.*

---

## Configuration Settings

### Hiding/Showing Sidebar File Actions

The visibility of the "Upload" button (and previously the "File actions" menu) can be controlled via the `config.yaml` file.

*   **Field:** `userDefaults.hideSidebarFileActions`
*   **Default:** `false`

Set this to `false` to ensure the Upload button is visible by default for all users. Individual users can still toggle this in their own settings profile if they have the necessary permissions.

---

