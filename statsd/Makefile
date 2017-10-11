BUILD_DATE := $(shell date -u +"%Y-%m-%dT%H:%M:%SZ")
VCS_URL := $(shell [ -d .git ] && git config --get remote.origin.url)
VCS_REF := $(strip $(shell [ -d .git ] && git rev-parse --short HEAD))
IS_TAG := $(shell [ -d .git ] && git describe --exact-match HEAD)
VCS_REF_MSG := $(shell if [ "$(IS_TAG)" != "" ]; then git tag -l -n1 $(IS_TAG) | awk '{$$1 = ""; print $$0;}'; else git log --format='%s' -n 1 $(VCS_REF); fi)

default:
	docker build \
	    --build-arg BUILD_DATE="$(BUILD_DATE)" \
		--build-arg VERSION="$(BEAT_CLASSIFIER_API_VERSION)" \
		--build-arg VCS_URL="$(VCS_URL)" \
		--build-arg VCS_REF="$(VCS_REF)" \
		--build-arg VCS_REF_MSG="$(VCS_REF_MSG)" \
		--compress \
		--rm \
		-t salimane/statsd:v0.8.0 .

run:
	docker run -t salimane/statsd:v0.8.0

int:
	docker run -it salimane/statsd:v0.8.0

push:
	docker push salimane/statsd:v0.8.0
