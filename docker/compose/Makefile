ROOT_DIR:=$(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))

ELK_FILE=$(ROOT_DIR)/docker-compose.elk.yml
GI_FILE=$(ROOT_DIR)/docker-compose.gi.yml
KB_FILE=$(ROOT_DIR)/docker-compose.kb.yml

BUILD_FILE=$(ROOT_DIR)/docker-compose.yml

COMPOSE=docker-compose

DOCKER_MACHINE=default
HOST_IP=$(shell docker-machine ip $(DOCKER_MACHINE))
export HOST_IP

default: all

elk:
	@$(COMPOSE) -p elk -f $(ELK_FILE) up -d

build-docker-compose:
	@ruby build_docker-compose.rb > $(BUILD_FILE)

# Logstash needs to be up first
all: elk build-docker-compose
	@until echo > /dev/tcp/${HOST_IP}/1514; do sleep 1; done
	@$(COMPOSE) -f $(BUILD_FILE) up -d
