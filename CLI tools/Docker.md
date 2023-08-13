### Keep a docker container running in the background

Start a docker container running in the background that we can do exec's against.

Run tail and use --detach flag to keep the container running in the background indefinitely or until killed manually.

```bash
image="my-image"
version="1.0.0"

run_container() {
  docker run --volume "$HOME":"$HOME" --detach --name $image $image:$version tail -f /dev/null
}
```

### Execute a command against the long-running container.

The container must be running first.

This will set the current working directory in your terminal as the current working directory inside the docker container. It will then append all other commands to the docker container.

```bash
exec_command() {
  docker exec --workdir "$PWD" "$image" "$@"
}
exec_command pwd && ls
exec_command <do stuff in the container>
```

### Spin up a container and attach to its terminal

```bash
debug_container() {
  docker run --rm -it \
    --volume "$PWD":"$PWD" \
    --workdir "$PWD" \
    --attach stdin \
    --attach stdout \
    --attach stderr \
    --label created-by=$image \
    $image:$version bash "$@"
}
```

### Check if Docker container is running

```bash
image="rp-build-tools"

is_rp_build_tools_running() {
  if [[ "$(docker container inspect --format '{{.State.Running}}' "$image")" == "true" ]]; then
    echo "true"
  else
    echo "false"
  fi
}
```

### Ensure a container is running

```bash
image="rp-build-tools"

rp_build_tools_ensure_running() {
  isRunning=$(is_rp_build_tools_running)
  if [[ "$isRunning" == "false" ]]; then
    docker container start "$(docker ps --all --filter name=$image --quiet)"
  fi
}
```

### Re-build container

```bash
image="rp-build-tools"
version="1.20.5"

rp_build_tools_rebuild() {
  echo "Containers for $image:"
  docker ps --filter "name=$image"

  echo "Images for $image:"
  docker image ls --filter "reference=$image"

  pid="$(docker ps --filter "name=$image" --quiet)"
  if [ -n "$pid" ]; then
    echo "Stopping $image:$version with ID $pid"
    docker stop "$pid"

    echo "Removing container $image:$version with ID $pid"
    docker container rm "$pid"
  fi

  imageId="$(docker image ls --filter reference="$image" --quiet)"
  if [ -n "$imageId" ]; then
    echo "Removing image $image:$version with ID $imageId"
    docker image rm "$imageId"
  fi

  echo "Re-building image $image:$version"
  rp_build_tools_build_image

  echo "Starting container $image:$version"
  rp_build_tools_run_container

  docker ps --filter "name=$image"
  echo "Done"
}
```
