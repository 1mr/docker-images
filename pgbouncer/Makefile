IMAGE_NAME=1am3r/pgbouncer
IMAGE_VERSION=1.23.1

docker:
	docker build --pull -t $(IMAGE_NAME):$(IMAGE_VERSION) .

push:
	docker push $(IMAGE_NAME):$(IMAGE_VERSION)
