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
