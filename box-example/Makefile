python = python2.7
SHELL=/bin/bash
DOCKER_HELPER := $(shell dirname $(abspath $(lastword $(MAKEFILE_LIST))))/make_helper.sh

ROLE_VERSION=$(shell cat $(PWD)/../version.txt)
ROLE_NAME=$(shell . $(DOCKER_HELPER) ; getUpDirLevel2)

REGISTRY_HOST=docker.io
USERNAME=softasap

all: clean_build

clean_build: clean initialize build

build:  p-env/bin/ansible-container build_container
	@echo p-env/bin/ansible-container --debug --project-name $(ROLE_NAME) build --roles-path ./roles/ -- -vvv
	@p-env/bin/ansible-container --debug --project-name $(ROLE_NAME) build --roles-path ./roles/ -- -vvv
	@echo "Application docker image was build"

build_container:
	@echo Building "$(ROLE_NAME)" version $(ROLE_VERSION)
	@echo ===============================================
	@echo Is there way to skip conductor rebuild each time? Submit PR

run:  p-env/bin/ansible-container
	@echo p-env/bin/ansible-container --debug --project-name $(ROLE_NAME) run --roles-path ./roles/ -- -vvv
	@p-env/bin/ansible-container --debug --project-name $(ROLE_NAME) run --roles-path ./roles/ -- -vvv
	@echo "Application environment was started"

stop:   p-env/bin/ansible-container
	@echo p-env/bin/ansible-container --debug --project-name $(ROLE_NAME) stop
	@p-env/bin/ansible-container --debug --project-name $(ROLE_NAME) stop
	@echo "Application environment was stopped"

p-env/bin/pip: p-env/bin/python
	p-env/bin/pip install -r requirements.txt

p-env/bin/python:
	virtualenv -p $(python) --no-site-packages p-env
	@touch $@

p-env/bin/ansible-container: p-env/bin/pip
	@touch $@

clean:
	@rm -rf .Python p-env roles

initialize:
	@init_quick.sh

tag:
	@docker tag $(ROLE_NAME)-www:latest softasap/sa-container-box-examples:$(ROLE_NAME).$(ROLE_VERSION)
	@docker tag $(ROLE_NAME)-www:latest softasap/sa-container-box-examples:$(ROLE_NAME).latest

push:
	@docker push softasap/sa-container-box-examples:$(ROLE_NAME).$(ROLE_VERSION)
	@docker push softasap/sa-container-box-examples:$(ROLE_NAME).latest

.PHONY: all clean initialize build run stop

