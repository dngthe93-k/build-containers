# Docker Image Build & Push Guide

## 1. Build
```bash
docker build -t ghcr.io/<user_id>/<image_name>:<tag> <image_name>
```
Example:
```bash
docker build -t ghcr.io/user1/linux-x64-aarch64:0.1 linux-x64-aarch64
```

## 2. Login
```bash
echo "<your_github_token>" | docker login ghcr.io -u <user_id> --password-stdin
```
> Generate a token at GitHub → Settings → Developer settings → Personal access tokens with `write:packages` permission.

## 3. Push
```bash
docker push ghcr.io/<user_id>/<image_name>:<tag>
```
Example:
```bash
docker push ghcr.io/user1/linux-x64-aarch64:0.1
```
