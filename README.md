# Git Shorthash Tag Resource

A resource for retrieving shorthash from a remote git repo.

Conceptually it is based on the [concourse semver resource](https://github.com/concourse/semver-resource) but there is only git and it is focused on getting shorthash.

## Installing

Use this resource by adding the following to the `resource_types` section of a pipeline config:

```yaml
resource_types:
- name: concourse-git-shorthash
  type: docker-image
  source:
    repository: tekn0ir/concourse-git-shorthash-resource
```

## Source Configuration

* `uri`: *Required.* The repository URL.

* `branch`: *Required.* The branch all tags were made on.

* `private_key`: *Optional.* The SSH private key to use when pulling from/pushing to to the repository.

* `username`: *Optional.* Username for HTTP(S) auth when pulling/pushing.
   This is needed when only HTTP/HTTPS protocol for git is available (which does not support private key auth) and auth is required.

* `password`: *Optional.* Password for HTTP(S) auth when pulling/pushing.

### Example

With the following resource configuration:

``` yaml
resources:
- name: master-shorthash
  type: concourse-git-shorthash
  source:
    uri: git@github.com:concourse/concourse.git
    branch: master
    private_key: {{concourse-repo-private-key}}
```

Only `get` is suppoerted:

``` yaml
plan:
- get: master-shorthash
- task: a-thing-that-needs-a-version
```

## Behavior

### `in`: Provide the shorthash as a file.

Provides the shorthash of the branch as a `shorthash` file in the destination.


### `out`: Set the version or bump the current one.

Not supported.
