# kanikoctl-buildkite-plugin

Build images using [GoogleContainerTools/kaniko](http://github.com/GoogleContainerTools/kaniko)
running in a separate container.

See [keithduncan/kaniko-socat](https://github.com/keithduncan/kaniko-socat) for
a Docker image with the kaniko binaries and `socat` installed.

## Plugin Parameters

* **[verbosity](https://github.com/GoogleContainerTools/kaniko#--verbosity)**:
Optional, one of: panic, fatal, error, warn, info, debug
* **[context_path](https://github.com/GoogleContainerTools/kaniko#kaniko-build-contexts)**:
a path relative to the [`BUILDKITE_BUILD_CHECKOUT_PATH`](https://buildkite.com/docs/pipelines/environment-variables#buildkite-environment-variables)
for the image context
* **dockerfile**: Optional, a path relative to the `context_path`
directory for the `Dockerfile`, defaults to `Dockerfile`.
* **[build_args](https://github.com/GoogleContainerTools/kaniko#--build-arg)**: 
Optional, a list of strings e.g. 'COMMIT=01234sha' for `Dockerfile` ARG
instructions
* **[destination](https://github.com/GoogleContainerTools/kaniko#pushing-to-different-registries)**: 
Optional, Docker image registry to push to the built image to. If absent the
image is built but not pushed. If no `tags` are specified the `latest` tag is
used by default.
* **tags**: Optional, list of strings, if `destination` is given the image is
pushed to these registry tags e.g. `tags: ['0.1.1', '0.1', '0']`. Tags also
supports a ref syntax: `tags: { Ref: version }` where `version` is
looked up using `buildkite-agent meta-data get version` and the string is
split on comma to generate a list of tags.

## Example

```yaml
agents:
  queue: your-on-demand-queue

steps:
  - label: ":docker: :kangaroo:"
    plugins:
      - "keithduncan/kanikoctl#261d24e5f25e01ba0ee8f2b406c5ff7c260d2cc5":
          destination: keithduncan/hello-world
    agents:
      task-definition: kaniko
      task-role: DockerHubPublish
```
