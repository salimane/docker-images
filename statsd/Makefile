VERSION := $(strip $(shell git describe --always --tags --dirty))
BUILD_DATE := $(shell date -u +"%Y-%m-%dT%H:%M:%S%Z")
VCS_URL := $(shell git config --get remote.origin.url)
VCS_REF := $(strip $(shell git rev-parse --short HEAD))
IS_TAG := $(shell git describe --exact-match HEAD)
VCS_REF_MSG := $(shell if [ "$(IS_TAG)" != "" ]; then git tag -l -n1 $(IS_TAG) | awk '{$$1 = ""; print $$0;}'; else git log --format='%s' -n 1 $(VCS_REF); fi)

default:
	docker build \
	    --build-arg BUILD_DATE="$(BUILD_DATE)" \
		--build-arg VERSION="$(VERSION)" \
		--build-arg VCS_URL="$(VCS_URL)" \
		--build-arg VCS_REF="$(VCS_REF)" \
		--build-arg VCS_REF_MSG="$(VCS_REF_MSG)" \
		--compress \
		--rm \
		-t salimane/statsd .

run:
	docker run -t salimane/statsd

int:
	docker run -it salimane/statsd

push:
	docker tag salimane/statsd:latest salimane/statsd:v0.8.1
	docker push salimane/statsd
