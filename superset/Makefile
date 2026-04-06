IMAGE_NAME=1am3r/superset
IMAGE_VERSION=6.1.0rc1

docker:
	docker build --pull -t $(IMAGE_NAME):$(IMAGE_VERSION) .

push:
	docker push $(IMAGE_NAME):$(IMAGE_VERSION)
