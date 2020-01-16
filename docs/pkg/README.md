## pkg

Fetch, update and sync packages using git

![alt text][demo]

### Synopsis

`pkg` manages Resource configuration packages.

Packages are collections of Resource configuration stored in git repositories.
They may be an entire repo, or a subdirectory within a repo.

**Any git repository containing Resource configuration may be used as a package**,
no additional structure or formatting is necessary.

Fetched packages may be customized using various techniques such as [setters] and [functions].
Fetched packages may be applied to a cluster using [apply].

Example package workflow:

1. `kpt pkg get` to get a package
2. `kpt cfg set`, `kpt cfg run` or `vi` to modify configuration
3. `git add && git commit` to save package
4. `kpt svr apply` to a cluster
5. `kpt pkg update` to pull in new changes
6. `kpt svr apply` to apply the upstream changes

A collection of packages to fetch and update may also be specified declaratively using `kpt pkg sync`.

### Primary Commands

**[get](get.md)**:
- fetching packages from subdirectories stored in git to local copies

**[update](update.md)**:
- applying upstream package updates to a local copy

**[init](init.md)**:
- initialize an empty package

**[sync](sync.md)**:
- defining packages to sync from remote sources using a declarative file which
  maps remote packages (repo + path + version) to local directories

### Additional Commands

**[diff](diff.md)**:
- diff a locally modified package against upstream

**[desc](desc.md)**:
- print package origin

**[man](man.md)**:
- print package documentation

### Examples

    # get the package
    export SRC_REPO=git@github.com:GoogleContainerTools/kpt.git
    $ kpt pkg get $SRC_REPO/package-examples/helloworld-set@v0.1.0 helloworld
    fetching package /package-examples/helloworld-set from \
        git@github.com:GoogleContainerTools/kpt to helloworld

    # pull in upstream updates by merging Resources
    $ kpt pkg update helloworld@v0.2.0 --strategy=resource-merge
    updating package helloworld to v0.2.0

    # manage a collection of packages declaratively
    $ kpt pkg init ./ --description "my package"
    $ kpt pkg sync set $SRC_REPO/package-examples/helloworld-set@v0.1.0 \
        hello-world --strategy=resource-merge
    $ kpt pkg sync ./

    # update the package with sync
    $ kpt pkg sync set $SRC_REPO/package-examples/helloworld-set@v0.2.0 \
        hello-world --strategy=resource-merge
    $ kpt pkg sync ./

### 

[demo]: https://storage.googleapis.com/kpt-dev/docs/pkg.gif "kpt pkg"
[setters]: ../cfg/set.md
[functions]: ../fn
[apply]: ../svr/apply.md