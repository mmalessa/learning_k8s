DOCKER = docker
.DEFAULT_GOAL = build



.PHONY: help
help:
	@echo -e '\033[1m make [TARGET] \033[0m'
	@grep -E '(^[a-zA-Z0-9_-]+:.*?##.*$$)|(^##)' Makefile | awk 'BEGIN {FS = ":.*?## "}{printf "\033[32m%-30s\033[0m %s\n", $$1, $$2}' | sed -e 's/\[32m##/[33m/'

.PHONY: build
build: ## Build go-web-server image
	@$(DOCKER) build -t go-web-server .
	$(MAKE) list-images

.PHONY: list-images
list-images: ## List docker go-web-server images
	@$(DOCKER) image ls |grep go-web-server

.PHONY: tag-image
tag-image: ## Tag image
	@$(DOCKER) tag go-web-server:latest localhost:5000/go-web-server:v1.0

.PHONY: push-to-registry
push-to-registry: ## Push go-web-server image to registry
	@$(DOCKER) push localhost:5000/go-web-server:v1.0

