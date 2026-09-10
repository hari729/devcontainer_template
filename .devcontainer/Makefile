IMAGE_NS    ?= ros2dev
ROS_DISTRO  ?= jazzy
DEPS_TAG    ?= nav2gz
USER_UID    ?= $(shell id -u)
USER_GID    ?= $(shell id -g)

.PHONY: base dev deps all clean

base:
	docker build -f Dockerfile.base \
		--build-arg ROS_DISTRO=$(ROS_DISTRO) \
		--build-arg USER_UID=$(USER_UID) \
		--build-arg USER_GID=$(USER_GID) \
		-t $(IMAGE_NS)/base:$(ROS_DISTRO) .

dev: base
	docker build -f Dockerfile.dev \
		--build-arg BASE_IMAGE=$(IMAGE_NS)/base:$(ROS_DISTRO) \
		-t $(IMAGE_NS)/dev:$(ROS_DISTRO) .

deps: dev
	docker build -f Dockerfile.deps \
		--build-arg BASE_IMAGE=$(IMAGE_NS)/dev:$(ROS_DISTRO) \
		--build-arg ROS_DISTRO=$(ROS_DISTRO) \
		-t $(IMAGE_NS)/deps:$(ROS_DISTRO)-$(DEPS_TAG) .

all: deps
	@echo "Built:"
	@echo "  $(IMAGE_NS)/base:$(ROS_DISTRO)"
	@echo "  $(IMAGE_NS)/dev:$(ROS_DISTRO)"
	@echo "  $(IMAGE_NS)/deps:$(ROS_DISTRO)-$(DEPS_TAG)"

# Optional: push base/dev to a registry so other project repos can just
# `FROM ros2dev/dev:jazzy` without rebuilding it locally.
push-base:
	docker push $(IMAGE_NS)/base:$(ROS_DISTRO)

push-dev:
	docker push $(IMAGE_NS)/dev:$(ROS_DISTRO)

clean:
	docker rmi -f \
		$(IMAGE_NS)/deps:$(ROS_DISTRO)-$(DEPS_TAG) \
		$(IMAGE_NS)/dev:$(ROS_DISTRO) \
		$(IMAGE_NS)/base:$(ROS_DISTRO) 2>/dev/null || true
