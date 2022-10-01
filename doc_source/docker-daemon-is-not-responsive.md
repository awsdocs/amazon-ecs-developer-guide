# Docker daemon is not responsive<a name="docker-daemon-is-not-responsive"></a>

AWS Copilot can use a provided Dockerfile to build a container image for the application.
To do this a running Docker daemon is necessary.

If the following message appears after a *copilot init* command, it may indicate that a docker daemon is not available.

  ```
  Docker daemon is not responsive; Copilot won't build from a Dockerfile.
  ```

If copilot does not detect a running Docker daemon it will not be able to build a new docker image for the application.

If a docker image for the application already exists, the image location can be provided and the process continued.
Otherwise, start a docker daemon and retry the desired copilot command.
