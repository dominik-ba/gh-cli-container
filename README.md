![Docker Image CI](https://github.com/dominik-ba/gh-cli-container/workflows/Docker%20Image%20CI/badge.svg)

# gh-cli-container
Dockerfile for GitHub CLI


## Usage

For GitHub Enterprise:

```
docker run -it \
    -u 1234:1234 \
    -e GH_ENTERPRISE_TOKEN=xyz \
    -e GH_HOST=git.company.com \
    -e GH_REPO=git.company.com/org/repo \
    -v $PWD:/home/work/repo dom/gh-cli:test
```

## more docs

the full github cli documentation can be found [here](https://cli.github.com/manual/)
