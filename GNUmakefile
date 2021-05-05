# General tools
THIS_FILE								:= $(lastword $(MAKEFILE_LIST))
export THIS_FILE
TIME									:= $(shell date +%s)
export TIME
################################################################################
ifeq ($(user),)
## USER retrieved from env, UID from shell.
HOST_USER								?=  $(strip $(if $(USER),$(USER),nodummy))
HOST_UID								?=  $(strip $(if $(shell id -u),$(shell id -u),4000))
else
# allow override by adding user= and/ or uid=  (lowercase!).
# uid= defaults to 0 if user= set (i.e. root).
HOST_USER								= $(user)
HOST_UID								= $(strip $(if $(uid),$(uid),0))
endif
export HOST_USER
export HOST_UID
################################################################################
ifeq ($(alpine),)
ALPINE_VERSION							:= 3.11.10
else
ALPINE_VERSION							:= $(alpine)
endif
export ALPINE_VERSION
################################################################################
#GIT CONFIG
GIT_USER_NAME							:= $(shell git config user.name)
export GIT_USER_NAME
GIT_USER_EMAIL							:= $(shell git config user.email)
export GIT_USER_EMAIL
GIT_SERVER								:= https://github.com
export GIT_SERVER
GIT_PROFILE								:= bitcoincore-dev
export GIT_PROFILE
GIT_BRANCH								:= $(shell git rev-parse --abbrev-ref HEAD)
export GIT_BRANCH
GIT_HASH								:= $(shell git rev-parse HEAD)
export GIT_HASH
GIT_REPO_ORIGIN							:= $(shell git remote get-url origin)
export GIT_REPO_ORIGIN
GIT_REPO_NAME							:= $(PROJECT_NAME)
export GIT_REPO_NAME
GIT_REPO_PATH							:= ~/$(GIT_REPO_NAME)
export GIT_REPO_PATH
################################################################################
ifeq ($(no-cache),true)
NO_CACHE								:= --no-cache
else
NO_CACHE								:=	
endif
export NO_CACHE
################################################################################
ifeq ($(verbose),true)
VERBOSE									:= --verbose
else
VERBOSE									:=	
endif
export VERBOSE
################################################################################
ifeq ($(port),)
PUBLIC_PORT								:= 80
else
PUBLIC_PORT								:= $(port)
endif
export PUBLIC_PORT
################################################################################
ifneq ($(passwd),)
PASSWORD								:= $(passwd)
else 
PASSWORD								:= changeme
endif
export PASSWORD
################################################################################
ifeq ($(cmd),)
CMD_ARGUMENTS							:= 	
else
CMD_ARGUMENTS							:= $(cmd)
endif
export CMD_ARGUMENTS
################################################################################
# PROJECT_NAME defaults to name of the current directory.
PROJECT_NAME							:= $(notdir $(PWD))
export PROJECT_NAME
################################################################################
.PHONY: commands-report
commands-report: report
	@echo ''
	@echo '	[USAGE]:	make [BUILD] run [EXTRA_ARGUMENTS]	'
	@echo ''
	@echo '		make all run user=root uid=0 no-cache=true verbose=true'
	@echo ''
	@echo '	[DEV ENVIRONMENT]:	'
	@echo ''
	@echo '		shell user=root'
	@echo '		shell user=$(HOST_USER)'
	@echo ''
	@echo '	[BUILD]:	'
	@echo ''
	@echo '		all'
	@echo '		            	compile statoshi source code'
	@echo ''
	@echo '	[EXTRA_ARGUMENTS]:	set build variables	'
	@echo ''
	@echo '		no-cache=true'
	@echo '		            	add --no-cache to docker command and apk add $(NO_CACHE)'
	@echo '		port=integer'
	@echo '		            	set PUBLIC_PORT default 80'
	@echo ''
	@echo '		            	TODO'
	@echo ''
	@echo '	[DOCKER COMMANDS]:	push a command to the container	'
	@echo ''
	@echo '		cmd=command 	'
	@echo '		cmd="command"	'
	@echo '		             	send CMD_ARGUMENTS to the [TARGET]'
	@echo ''
	@echo '	[EXAMPLE]:'
	@echo ''
	@echo '		make all run user=root uid=0 no-cache=true verbose=true'
	@echo '		make report all run user=root uid=0 no-cache=true verbose=true cmd="top"'
	@echo '		make a r port=80 no-cache=true verbose=true cmd="ls"'
	@echo ''
	@echo '	[COMMAND_LINE]:'
	@echo ''
	@echo '	stats-console              # container command line'
	@echo '	stats-bitcoind             # start container bitcoind -daemon'
	@echo '	stats-debug                # container debug.log output'
	@echo '	stats-whatami              # container OS profile'
	@echo ''
	@echo '	stats-cli -getmininginfo   # report mining info'
	@echo '	stats-cli -gettxoutsetinfo # report txo info'
	@echo ''
	@echo '	#### WARNING: (effects host datadir) ####'
	@echo '	'
	@echo '	stats-prune                # default in bitcoin.conf is prune=1 - start pruning node'
	@echo '	'
################################################################################
ifeq ($(staging),)
ipfs_staging:=~/.ipfs_staging
else
ipfs_staging:=$(staging)
endif
export ipfs_staging
################################################################################
ifeq ($(data),)
ipfs_data:=~/.ipfs_data
export ipfs_data
else
ipfs_data:=$(data)
endif
export ipfs_data
################################################################################
ifeq ($(port),)
public_port:=8080
else
public_port:=$(port)
endif
export public_port
################################################################################
SHELL=PATH='$(PATH)' /bin/sh
################################################################################
# enable second expansion
.SECONDEXPANSION:
################################################################################
.PHONY: report
report:
	@echo ''
	@echo '	[ARGUMENTS]	'
	@echo '      args:'
	@echo '        - PWD=${PWD}'
	@echo '        - UMBREL=${UMBREL}'
	@echo '        - THIS_FILE=${THIS_FILE}'
	@echo '        - TIME=${TIME}'
	@echo '        - HOST_USER=${HOST_USER}'
	@echo '        - HOST_UID=${HOST_UID}'
	@echo '        - PUBLIC_PORT=${PUBLIC_PORT}'
	@echo '        - SERVICE_TARGET=${SERVICE_TARGET}'
	@echo '        - ALPINE_VERSION=${ALPINE_VERSION}'
	@echo '        - PROJECT_NAME=${PROJECT_NAME}'
	@echo '        - GIT_USER_NAME=${GIT_USER_NAME}'
	@echo '        - GIT_USER_EMAIL=${GIT_USER_EMAIL}'
	@echo '        - GIT_SERVER=${GIT_SERVER}'
	@echo '        - GIT_PROFILE=${GIT_PROFILE}'
	@echo '        - GIT_BRANCH=${GIT_BRANCH}'
	@echo '        - GIT_HASH=${GIT_HASH}'
	@echo '        - GIT_REPO_ORIGIN=${GIT_REPO_ORIGIN}'
	@echo '        - GIT_REPO_NAME=${GIT_REPO_NAME}'
	@echo '        - GIT_REPO_PATH=${GIT_REPO_PATH}'
################################################################################
.PHONY: init
init:
	$( shell [ -d "/usr/local/bin/ipfs" ]        && install -v bin/ipfs /usr/local/bin/ipfs)
	$( shell [ -d "/usr/local/bin/ipfs-init" ]   && ln -s /usr/local/bin/ipfs /usr/local/bin/ipfs-init)
	$( shell [ -d "/usr/local/bin/ipfs-host" ]   && ln -s /usr/local/bin/ipfs /usr/local/bin/ipfs-host)
	$( shell [ -d "/usr/local/bin/ipfs-start" ]  && ln -s /usr/local/bin/ipfs /usr/local/bin/ipfs-start)
	$( shell [ -d "/usr/local/bin/ipfs-webui" ]  && ln -s /usr/local/bin/ipfs /usr/local/bin/ipfs-webui)
.PHONY: build-host
build-host:
	docker build -f Dockerfile 
.PHONY: host
host: init
	#docker run -d --name ipfs_host_$(TIME) -v $(ipfs_staging):/export -v $(ipfs_data):/data/ipfs -p 127.0.0.1:$(port):8080 -p 4001:4001 -p 4001:4001/udp -p 127.0.0.1:8081:8081 -p 127.0.0.1:5001:5001 ipfs/go-ipfs:latest
	#docker-compose $(VERBOSE) -p $(PROJECT_NAME)_$(HOST_UID) run -d --name ipfs_host_$(TIME) -v $(ipfs_staging):/export -v $(ipfs_data):/data/ipfs -p 127.0.0.1:$(public_port):8080 -p 4001:4001 -p 4001:4001/udp -p 127.0.0.1:8081:8081 -p 127.0.0.1:5001:5001 host
	docker-compose $(VERBOSE) -p $(PROJECT_NAME)_$(HOST_UID) run -d --name ipfs_host_$(TIME) -v $(ipfs_staging):/export -v $(ipfs_data):/data/ipfs -p 127.0.0.1:8080:8080 -p 4001:4001 -p 4001:4001/udp -p 127.0.0.1:8081:8081 -p 127.0.0.1:5001:5001 -p 80:8080 host
################################################################################
package-all:
	bash -c 'cat ~/GH_TOKEN.txt | docker login docker.pkg.github.com -u RandyMcMillan --password-stdin'
	bash -c 'docker tag $(PROJECT_NAME):root docker.pkg.github.com/bitcoincore-dev/stats.bitcoincore.dev/$(notdir $(PROJECT_NAME)).all:root'
	bash -c 'docker push docker.pkg.github.com/bitcoincore-dev/stats.bitcoincore.dev/$(notdir $(PROJECT_NAME)):root'
################################################################################
package-slim:
	bash -c 'cat ~/GH_TOKEN.txt | docker login docker.pkg.github.com -u RandyMcMillan --password-stdin'
	bash -c 'docker tag $(PROJECT_NAME):root docker.pkg.github.com/bitcoincore-dev/stats.bitcoincore.dev/$(notdir $(PROJECT_NAME)).slim:root'
	bash -c 'docker push docker.pkg.github.com/bitcoincore-dev/stats.bitcoincore.dev/$(notdir $(PROJECT_NAME)).slim:root'
################################################################################
include Rules.mk
