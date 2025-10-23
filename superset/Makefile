IMAGE_NAME=1am3r/superset
IMAGE_VERSION=5.0.0

docker:
	docker build --pull -t $(IMAGE_NAME):$(IMAGE_VERSION) .

push:
	docker push $(IMAGE_NAME):$(IMAGE_VERSION)
