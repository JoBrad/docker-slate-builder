### Docker image for building [Slate](https://github.com/tripit/slate) static HTML

Just give the container two volumes:

|Host|Container|Description|
|----|---------|-----------|
|/some/source/dir|/app/source|The Slate Markdown sources|
|/some/build/dir|/app/build|The directory to build HTML into|

### Example

##### Build static HTML site

Build Slate Markdown sources from `/some/source/dir` on the host machine into static HTML in `/some/build/dir` on the host machine.

Example below creates a build directory first (if the host directory doesn't exist, docker will create it owned by root, which is less then desirable since we're just trying to build a static HTML site).  It also specifies `--user` so docker runs as the current user/group, so the generated files are owned by the current user.

```
mkdir -p /some/build/dir
docker run -it --rm \
 -v /some/source/dir:/app/source \
 -v /some/build/dir:/app/build \
 --user=$UID:$GID \
 grumpydocker/slate-builder
```

##### Serve built content with Nginx on port 8080

```
docker run -dt --name=some-docs -p :8080:80 \
 -v /some/build/dir:/usr/share/nginx/html:ro \
 nginx
```

### Why

Who wants to ruin their personal machine or build server with Ruby?
