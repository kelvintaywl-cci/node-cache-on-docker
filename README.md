# Investigating Node dependency cache restores

We explore a publicly-available Docker image based off cimg/node Docker image.

Specifically, we are looking at https://hub.docker.com/r/joinswoop/circle-ci-cypress

It appears this image has USER set to `root`, rather than `circleci` (as per cimg/node image).

```console
$ docker run --rm joinswoop/circle-ci-cypress:1.4 whoami
root

$ docker run --rm cimg/node:18.18.2 whoami
circleci

# both images' Node & Yarn versions are the same otherwise

$ docker run --rm joinswoop/circle-ci-cypress:1.4 node --version && yarn --version
v18.18.2
1.22.18

$ docker run --rm cimg/node:18.18.2 node --version && yarn --version
v18.18.2
1.22.18
```

This seems to have caused `yarn install --no-check-cache --immutable` to not be able to retrieve the cached node_modules IF user is set to `root`.

