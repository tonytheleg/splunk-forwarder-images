# splunk-forwarder-images
Build and push container images for splunk universal forwarder.

## Local Build/Test
The following `make` variables affect building/pushing:
- `IMAGE_REGISTRY` (default `quay.io`)
- `IMAGE_REPOSITORY` (default `app-sre`)
- `FORWARDER_NAME` (default `splunk-forwarder`)
- `HEAVYFORWARDER_NAME` (default `splunk-heavyforwarder`)

Images will be tagged and pushed as `${IMAGE_REGISTRY}/${IMAGE_REPOSITORY}/${[HEAVY]FORWARDER_NAME}:${VERSION}-${HASH}`, where `${VERSION}` and `${HASH}` are gleaned from [.splunk-version](.splunk-version) and [.splunk-version-hash](.splunk-version-hash), respectively.
Therefore, for local building and testing, you should create personal image repositories and point to them by overriding at least `IMAGE_REPOSITORY`.

Build images using `make build-forwarder` and `make build-heavyforwarder`.

Push images using `make push-forwarder` and `push-heavyforwarder`.

Run vulnerability checks using `make vuln-check`.

## Versioning and Tagging
This repository builds container images around the splunk universal forwarder at the version and hash specified in the [.splunk-version](.splunk-version) and [.splunk-version-hash](.splunk-version-hash) files, respectively.
To build around a new version, simply commit a PR updating those files.

## CICD
After a PR merges, an integration job is run by app-sre triggering build/push of both images to `quay.io/app-sre/splunk-(heavy)forwarder`.
Images are only built and pushed if they do not already exist in the repository at the current version.
This is to avoid the "disappearing digest" problem, facilitating by-digest pulls of these images.

To test the app-sre pipeline:
- Create personal repositories and override variables as described [above](#local-buildtest).
- Obtain credentials from your personal repository and set the `QUAY_USER` and `QUAY_TOKEN` variables.
- Run `make build-push`.
