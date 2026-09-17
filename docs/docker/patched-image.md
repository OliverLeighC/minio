# Building the patched server image

`Dockerfile.patched` builds MinIO from this checkout and intentionally does not
include `mc`. It currently targets Linux AMD64 only.

Build it with an ISO-8601 release timestamp and the source revision being
built:

```sh
docker buildx build --platform linux/amd64 \
  --build-arg VERSION=2025-04-22T22-12-26Z \
  --build-arg VCS_REF="$(git rev-parse HEAD)" \
  -f Dockerfile.patched \
  -t minio:patched \
  --load .
```

The image pins UBI Micro and static curl. Update either only with a new digest
or checksum, then rebuild and scan the resulting image. To restore `mc`, create
a `patched-mc` build stage that produces `/out/mc` and enable the marked copy in
the final stage only after scanning that binary.
