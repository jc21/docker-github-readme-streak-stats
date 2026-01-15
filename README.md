# docker-github-readme-streak-stats

This is a docker image that wraps the [github-readme-streak-stats](https://github.com/DenverCoder1/github-readme-streak-stats)
project, for self hosting.

## Usage

Refer to the [official repo](https://github.com/DenverCoder1/github-readme-streak-stats)
for env var usage and instructions on setting up your `TOKEN` personal github token.

```yml
---
services:
  github-readme-streak-stats:
    container_name: github-readme-streak-stats
    image: jc21/github-readme-streak-stats:latest
    ports:
      - '8080:80'
    environment:
      TOKEN: 'github_pat_11A....'
      WHITELIST: 'jc21'
    restart: unless-stopped
```


## Building

```bash
./scripts/buildx --push -t jc21/github-readme-streak-stats:latest
```
