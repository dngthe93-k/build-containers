# Docker Image Build & Push Guide

## 1. Login
```bash
echo "<your_github_token>" | docker login ghcr.io -u <user_id> --password-stdin
```
> Token: GitHub → Settings → Developer settings → Personal access tokens (`write:packages`).

## 2. Build & Push
```bash
docker buildx build --push -t ghcr.io/<user_id>/<image_name>:<tag> <image_name>
```
> Use `--load` instead of `--push` to keep the image locally. The two flags cannot be combined.
