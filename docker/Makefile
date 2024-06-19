include ../.env
export

default: build release

config:
	@echo "Current configuration:"
	@cat ../.env
	@DOCKER_ORG=$$(grep -m1 '^DOCKER_ORG=' ../.env | sed 's/^DOCKER_ORG=//'); \
	read -p "Enter new organization name (current: $$DOCKER_ORG, leave blank to keep current): " org; \
	if [ -n "$$org" ]; then \
		grep -q '^DOCKER_ORG=' ../.env && sed -i 's/^DOCKER_ORG=.*/DOCKER_ORG='"$$org"'/' ../.env || echo 'DOCKER_ORG='"$$org" >> ../.env; \
	fi
	@OPENAI_KEY=$$(grep -m1 '^OPENAI_KEY=' ../.env | sed 's/^OPENAI_KEY=//'); \
	read -p "Enter new OpenAI API key (current: $$OPENAI_KEY, leave blank to keep current): " key; \
	if [ -n "$$key" ]; then \
		grep -q '^OPENAI_KEY=' ../.env && sed -i 's/^OPENAI_KEY=.*/OPENAI_KEY='"$$key"'/' ../.env || echo 'OPENAI_KEY='"$$key" >> ../.env; \
	fi
build:
	echo "requires the docker buildkit"
	DOCKER_BUILDKIT=1 docker build --progress=plain -t isaacgym .
clean-build:
	DOCKER_BUILDKIT=1 docker build -t isaacgym . --no-cache=true
tag:
	docker tag isaacgym $(DOCKER_ORG)/isaacgym:"`date "+%F"`"
	docker tag isaacgym $(DOCKER_ORG)/isaacgym:latest
	docker push $(DOCKER_ORG)/isaacgym:"`date "+%F"`"
	docker push $(DOCKER_ORG)/isaacgym:latest
push:
	docker tag isaacgym $(DOCKER_ORG)/isaacgym:"`date "+%F"`"
	docker tag isaacgym $(DOCKER_ORG)/isaacgym:latest
	docker push $(DOCKER_ORG)/isaacgym:"`date "+%F"`"
	docker push $(DOCKER_ORG)/isaacgym:latest
release: build tag push
test: 
	echo "add docker cmd including mount for $(DOCKER_ORG)/isaacgym:latest"
