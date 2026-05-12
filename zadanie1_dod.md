# Docker - Zadanie 1 - Dodatkowe (2.)

## Przygotowanie buildera
```bash
docker buildx create --name multiarch-builder --driver docker-container --use
docker buildx inspect --bootstrap
```

## Budowa
```bash
docker buildx build --platform linux/amd64,linux/arm64 \
  -t yeloh/weather-app:1.0 \
  --push \
  --cache-to=type=registry,ref=yeloh/weather-app:1.0-cache,mode=max \
  --cache-from=type=registry,ref=yeloh/weather-app:1.0-cache \
  --build-arg BUILDKIT_INLINE_CACHE=1 .
```

<details>
<summary>Wynik:</summary>

```
[+] Building 87.9s (29/29) FINISHED                                                  docker-container:multiarch-builder
 => [internal] load build definition from Dockerfile                                                               0.1s
 => => transferring dockerfile: 829B                                                                               0.0s
 => [linux/arm64 internal] load metadata for docker.io/library/python:3.14-alpine                                  2.2s
 => [linux/amd64 internal] load metadata for docker.io/library/python:3.14-alpine                                  1.9s
 => [auth] library/python:pull token for registry-1.docker.io                                                      0.0s
 => [internal] load .dockerignore                                                                                  0.1s
 => => transferring context: 2B                                                                                    0.1s
 => ERROR importing cache manifest from yeloh/weather-app:1.0-cache                                                1.3s
 => [linux/arm64 builder 1/4] FROM docker.io/library/python:3.14-alpine@sha256:a3de013592ea520507c1f18d880592338b  5.3s
 => => resolve docker.io/library/python:3.14-alpine@sha256:a3de013592ea520507c1f18d880592338bd21abfe706237e68ed41  0.1s
 => => sha256:c46342b95b0c55bda2532a29c88677602e5bc7aba310e0d1467f82ac1ab54f02 247B / 247B                         0.6s
 => => sha256:d17f077ada118cc762df373ff803592abf2dfa3ddafaa7381e364dd27a88fca7 4.20MB / 4.20MB                     2.0s
 => => sha256:3d8d402a4e0f76b57ba75563229ba99e4445b6d4f37bdbff3121b4d5a5291805 457.77kB / 457.77kB                 0.6s
 => => sha256:c752a20044121659cf133fc3e1b8d42aaf9725f0433f544402f93aca2793d581 13.53MB / 13.53MB                   2.8s
 => => extracting sha256:d17f077ada118cc762df373ff803592abf2dfa3ddafaa7381e364dd27a88fca7                          0.2s
 => => extracting sha256:3d8d402a4e0f76b57ba75563229ba99e4445b6d4f37bdbff3121b4d5a5291805                          0.2s
 => => extracting sha256:c752a20044121659cf133fc3e1b8d42aaf9725f0433f544402f93aca2793d581                          0.6s
 => => extracting sha256:c46342b95b0c55bda2532a29c88677602e5bc7aba310e0d1467f82ac1ab54f02                          0.0s
 => [linux/amd64 builder 1/4] FROM docker.io/library/python:3.14-alpine@sha256:a3de013592ea520507c1f18d880592338  15.8s
 => => resolve docker.io/library/python:3.14-alpine@sha256:a3de013592ea520507c1f18d880592338bd21abfe706237e68ed41  0.1s
 => => sha256:97f1e103f334e724ec9e5cdfa0eb63e35df78ba9c7d642ea67d973671c010416 13.45MB / 13.45MB                   2.0s
 => => sha256:ffe5e153acbf7f2a9712301e26d1a8e48447530facbe8b525fe380cb3c948ec9 247B / 247B                         0.4s
 => => sha256:bafef4f1a46c8f17ca44cfb33ae79bb07d54ce7abeba8618466a8144cea386c3 455.50kB / 455.50kB                 0.9s
 => => sha256:6a0ac1617861a677b045b7ff88545213ec31c0ff08763195a70a4a5adda577bb 3.86MB / 3.86MB                     8.0s
 => => extracting sha256:6a0ac1617861a677b045b7ff88545213ec31c0ff08763195a70a4a5adda577bb                          1.5s
 => => extracting sha256:bafef4f1a46c8f17ca44cfb33ae79bb07d54ce7abeba8618466a8144cea386c3                          2.0s
 => => extracting sha256:97f1e103f334e724ec9e5cdfa0eb63e35df78ba9c7d642ea67d973671c010416                          2.6s
 => => extracting sha256:ffe5e153acbf7f2a9712301e26d1a8e48447530facbe8b525fe380cb3c948ec9                          0.9s
 => [internal] load build context                                                                                  0.2s
 => => transferring context: 3.34kB                                                                                0.1s
 => [auth] yeloh/weather-app:pull token for registry-1.docker.io                                                   0.0s
 => [linux/arm64 builder 2/4] WORKDIR /app                                                                         3.5s
 => [linux/arm64 builder 3/4] COPY requirements.txt .                                                              3.6s
 => [linux/arm64 stage-1 3/7] RUN adduser -D appuser                                                               5.8s
 => [linux/arm64 builder 4/4] RUN pip install --no-cache-dir --prefix=/install -r requirements.txt                53.1s
 => [linux/amd64 builder 2/4] WORKDIR /app                                                                         3.6s
 => [linux/amd64 stage-1 3/7] RUN adduser -D appuser                                                               2.3s
 => [linux/amd64 builder 3/4] COPY requirements.txt .                                                              2.0s
 => [linux/amd64 builder 4/4] RUN pip install --no-cache-dir --prefix=/install -r requirements.txt                 6.9s
 => [linux/amd64 stage-1 4/7] COPY --from=builder /install /usr/local                                              0.2s
 => [linux/amd64 stage-1 5/7] COPY app.py .                                                                        0.0s
 => [linux/amd64 stage-1 6/7] COPY templates ./templates                                                           0.0s
 => [linux/amd64 stage-1 7/7] RUN chown -R appuser:appuser /app                                                    0.1s
 => [linux/arm64 stage-1 4/7] COPY --from=builder /install /usr/local                                              0.4s
 => [linux/arm64 stage-1 5/7] COPY app.py .                                                                        0.0s
 => [linux/arm64 stage-1 6/7] COPY templates ./templates                                                           0.0s
 => [linux/arm64 stage-1 7/7] RUN chown -R appuser:appuser /app                                                    0.1s
 => exporting to image                                                                                            17.0s
 => => exporting layers                                                                                            0.6s
 => => preparing layers for inline cache                                                                           0.0s
 => => exporting manifest sha256:52698a812ccadbd19a88dc70ce5f7fb254971c3c615348ef8b38973489b7f969                  0.0s
 => => exporting config sha256:cb65d9cbde5bee7549c6892308a6bd5a5a4566ad66101e5bbf47d9574784141c                    0.0s
 => => exporting attestation manifest sha256:ea3e55efc949e27351f2f11628f68f14a524f3e19496fb1c64a99c39e60fa6f6      0.0s
 => => exporting manifest sha256:ac46a8aac7730462c88d818670ca24175bf2800f0cad639ce8e1dacb4c6d1e16                  0.0s
 => => exporting config sha256:5874e33826cc7f6ad29449190a8bcde573dc0a6c82a812c10beabc672258f0ad                    0.0s
 => => exporting attestation manifest sha256:09ad29acd8ba56955a0dd2c7fa4b9a2899f37f1bf9781203772c6a017c0d8488      0.0s
 => => exporting manifest list sha256:a9dea6846465f41704e3a4b478550d73a740d5047dc7705c1bc6ae4844469fe9             0.0s
 => => pushing layers                                                                                             11.0s
 => => pushing manifest for docker.io/yeloh/weather-app:1.0@sha256:a9dea6846465f41704e3a4b478550d73a740d5047dc770  5.2s
 => exporting cache to registry                                                                                   16.9s
 => => preparing build cache for export                                                                            0.9s
 => => sending cache export                                                                                       16.0s
 => => writing layer sha256:3e7c7dc3dae243f240b9ac0a220c4e621b5687833014d26680c7ee0189c448a5                       9.6s
 => => writing layer sha256:1df69fdd02f41eb234c384fe37353d8b25df859fd3754b09d4b08de90457a421                      10.3s
 => => writing layer sha256:3d8d402a4e0f76b57ba75563229ba99e4445b6d4f37bdbff3121b4d5a5291805                      10.5s
 => => writing layer sha256:16630b40df8b64dd8000cd53eaf2bfb2562fc0fbf46cf699a4bab9529cfc9a45                      10.1s
 => => writing layer sha256:48cbc14ee46c797eebaf9d18553b158c6fff6d3ac7f1d4e829c14cb4e97db11d                       0.1s
 => => writing layer sha256:56da7eda43dae9538cc59298f52df29555f0b23922bbccc7d72e2227c9095c4f                       1.5s
 => => writing layer sha256:66f3061f43b67b8999d5a7115f822bf1dd258325d0f6ef96774605d5395efbdc                       1.7s
 => => writing layer sha256:6a0ac1617861a677b045b7ff88545213ec31c0ff08763195a70a4a5adda577bb                       0.2s
 => => writing layer sha256:6c38309b7976cf175845bf5561179b342de5d6455e36698dfca442499cc74ecd                       1.2s
 => => writing layer sha256:7766c37628f5f2ee9bc542f86ee4a947bc0661409d543a909fa721157877165d                       2.2s
 => => writing layer sha256:7a5d6f837da107150f1561e1bd19a53308debbd09af466305c7bd6dc8a15cfb2                       0.1s
 => => writing layer sha256:911d59d520d0c11e4912cf418fd225514386e4b4000303904c90802190742aa4                       0.1s
 => => writing layer sha256:97f1e103f334e724ec9e5cdfa0eb63e35df78ba9c7d642ea67d973671c010416                       0.2s
 => => writing layer sha256:a503c9f5e72f9427069192de54dde887e717309a36f7bbdbf58dd844a6952212                       0.1s
 => => writing layer sha256:a77726934dee752869c27c4fab9c8ad2a34165ad8553043f9dc338abf4a2dd78                       0.1s
 => => writing layer sha256:ad1bf1dd31c2a1bf4c29e7dcd3af61fae714a94f081878ccbd6bddcf28a55521                       0.1s
 => => writing layer sha256:bafef4f1a46c8f17ca44cfb33ae79bb07d54ce7abeba8618466a8144cea386c3                       0.1s
 => => writing layer sha256:c2da4b2687bc2538e4464239557d364767c3f694580ae154a9e5dda9f9db6232                       0.1s
 => => writing layer sha256:c46342b95b0c55bda2532a29c88677602e5bc7aba310e0d1467f82ac1ab54f02                       0.1s
 => => writing layer sha256:c752a20044121659cf133fc3e1b8d42aaf9725f0433f544402f93aca2793d581                       0.1s
 => => writing layer sha256:d17f077ada118cc762df373ff803592abf2dfa3ddafaa7381e364dd27a88fca7                       0.1s
 => => writing layer sha256:e69bc0a09841f1d851d651fb4dd0cdcf0480978edb34fe47bf3c895c8f448d60                       0.1s
 => => writing layer sha256:ee39fcf432c5ed8cd7699f7bde3a1ea32f21547364100884d5e78aede37a44e3                       0.1s
 => => writing layer sha256:ffe5e153acbf7f2a9712301e26d1a8e48447530facbe8b525fe380cb3c948ec9                       0.1s
 => => writing config sha256:c0963b7964f9720addd6af9aa18d0a6fb58dd2afba25a5102d5c114963150b18                      1.3s
 => => writing cache image manifest sha256:00066e8b530a5ca82394b2166f757e3b2ae81297cf4a97f384d139bc4a5cfe2f        2.0s
 => [auth] yeloh/weather-app:pull,push token for registry-1.docker.io                                              0.0s
------
 > importing cache manifest from yeloh/weather-app:1.0-cache:
------
```
</details>

## Sprawdzenie deklaracji dla platform
```bash
docker buildx imagetools inspect yeloh/weather-app:1.0
```

<details>
<summary>Wynik:</summary>

```
Name:      docker.io/yeloh/weather-app:1.0
MediaType: application/vnd.oci.image.index.v1+json
Digest:    sha256:a9dea6846465f41704e3a4b478550d73a740d5047dc7705c1bc6ae4844469fe9

Manifests:
  Name:        docker.io/yeloh/weather-app:1.0@sha256:52698a812ccadbd19a88dc70ce5f7fb254971c3c615348ef8b38973489b7f969
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    linux/amd64

  Name:        docker.io/yeloh/weather-app:1.0@sha256:ac46a8aac7730462c88d818670ca24175bf2800f0cad639ce8e1dacb4c6d1e16
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    linux/arm64

  Name:        docker.io/yeloh/weather-app:1.0@sha256:ea3e55efc949e27351f2f11628f68f14a524f3e19496fb1c64a99c39e60fa6f6
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    unknown/unknown
  Annotations:
    vnd.docker.reference.digest: sha256:52698a812ccadbd19a88dc70ce5f7fb254971c3c615348ef8b38973489b7f969
    vnd.docker.reference.type:   attestation-manifest

  Name:        docker.io/yeloh/weather-app:1.0@sha256:09ad29acd8ba56955a0dd2c7fa4b9a2899f37f1bf9781203772c6a017c0d8488
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    unknown/unknown
  Annotations:
    vnd.docker.reference.digest: sha256:ac46a8aac7730462c88d818670ca24175bf2800f0cad639ce8e1dacb4c6d1e16
    vnd.docker.reference.type:   attestation-manifest
```
</details>

## Sprawdzenie wykorzystania cache
Przy ponownym budowaniu obrazu wiele wpisów w logu zawiera hasło CACHED:
```bash
docker buildx build --platform linux/amd64,linux/arm64 \
  -t yeloh/weather-app:1.0 \
  --push \
  --cache-from=type=registry,ref=yeloh/weather-app:1.0-cache .
```

<details>
<summary>Wynik:</summary>

```
[+] Building 9.2s (26/26) FINISHED                                                   docker-container:multiarch-builder
 => [internal] load build definition from Dockerfile                                                               0.0s
 => => transferring dockerfile: 829B                                                                               0.0s
 => [linux/amd64 internal] load metadata for docker.io/library/python:3.14-alpine                                  0.3s
 => [linux/arm64 internal] load metadata for docker.io/library/python:3.14-alpine                                  0.3s
 => [internal] load .dockerignore                                                                                  0.0s
 => => transferring context: 2B                                                                                    0.0s
 => importing cache manifest from yeloh/weather-app:1.0-cache                                                      1.0s
 => => inferred cache manifest type: application/vnd.oci.image.manifest.v1+json                                    0.0s
 => [internal] load build context                                                                                  0.1s
 => => transferring context: 132B                                                                                  0.0s
 => [linux/amd64 builder 1/4] FROM docker.io/library/python:3.14-alpine@sha256:a3de013592ea520507c1f18d880592338b  0.0s
 => => resolve docker.io/library/python:3.14-alpine@sha256:a3de013592ea520507c1f18d880592338bd21abfe706237e68ed41  0.0s
 => [linux/arm64 builder 1/4] FROM docker.io/library/python:3.14-alpine@sha256:a3de013592ea520507c1f18d880592338b  0.0s
 => => resolve docker.io/library/python:3.14-alpine@sha256:a3de013592ea520507c1f18d880592338bd21abfe706237e68ed41  0.0s
 => CACHED [linux/arm64 builder 2/4] WORKDIR /app                                                                  0.0s
 => CACHED [linux/arm64 stage-1 3/7] RUN adduser -D appuser                                                        0.0s
 => CACHED [linux/arm64 builder 3/4] COPY requirements.txt .                                                       0.0s
 => CACHED [linux/arm64 builder 4/4] RUN pip install --no-cache-dir --prefix=/install -r requirements.txt          0.0s
 => CACHED [linux/arm64 stage-1 4/7] COPY --from=builder /install /usr/local                                       0.0s
 => CACHED [linux/arm64 stage-1 5/7] COPY app.py .                                                                 0.0s
 => CACHED [linux/arm64 stage-1 6/7] COPY templates ./templates                                                    0.0s
 => CACHED [linux/arm64 stage-1 7/7] RUN chown -R appuser:appuser /app                                             0.2s
 => CACHED [linux/amd64 builder 2/4] WORKDIR /app                                                                  0.0s
 => CACHED [linux/amd64 stage-1 3/7] RUN adduser -D appuser                                                        0.0s
 => CACHED [linux/amd64 builder 3/4] COPY requirements.txt .                                                       0.0s
 => CACHED [linux/amd64 builder 4/4] RUN pip install --no-cache-dir --prefix=/install -r requirements.txt          0.0s
 => CACHED [linux/amd64 stage-1 4/7] COPY --from=builder /install /usr/local                                       0.0s
 => CACHED [linux/amd64 stage-1 5/7] COPY app.py .                                                                 0.0s
 => CACHED [linux/amd64 stage-1 6/7] COPY templates ./templates                                                    0.0s
 => CACHED [linux/amd64 stage-1 7/7] RUN chown -R appuser:appuser /app                                             0.2s
 => exporting to image                                                                                             7.5s
 => => exporting layers                                                                                            0.0s
 => => exporting manifest sha256:fbf8679f053d866e2da965a326b2d4ddbd61909dfb32a0d8080f57acfd0bcd61                  0.0s
 => => exporting config sha256:a60747f3610497db48cf930abedd199cc76ae0d3208296fb3d9d07a2d61245aa                    0.0s
 => => exporting attestation manifest sha256:56e5603afec49dacaa7ef783e6e1e4cd1add48d4d2827972f7ebe4283d62a788      0.0s
 => => exporting manifest sha256:eb5d78e21f18fa1bb4c9613a1ba2136fc5f97ec6f8669b3b8f3f8dba2b3ab4ea                  0.0s
 => => exporting config sha256:ac187be5128fc4327a3fb2392e5cb82d756c6b28dff34a6a100ff246b3f9d794                    0.0s
 => => exporting attestation manifest sha256:d0750027cb15511be514366121eee70057df9741b7c49e91c9f08f6ec6c6ff93      0.1s
 => => exporting manifest list sha256:008cc31e3c584bce0ebefe21da28d0e3670b4cbdbf1ee731ab32a9193549f0ff             0.0s
 => => pushing layers                                                                                              2.7s
 => => pushing manifest for docker.io/yeloh/weather-app:1.0@sha256:008cc31e3c584bce0ebefe21da28d0e3670b4cbdbf1ee7  4.6s
 => [auth] yeloh/weather-app:pull,push token for registry-1.docker.io                                              0.0s
```
</details>

## Skanowanie podatności

```bash
trivy image -f table -o report.txt yeloh/weather-app:1.0
```

[Wynikowy raport](report.txt)
