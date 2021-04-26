# General tools
THIS_FILE								:= $(lastword $(MAKEFILE_LIST))
export THIS_FILE
TIME									:= $(shell date +%s)
export TIME



ifeq ($(staging),)
ipfs_staging:=~/.ipfs_staging
export ipfs_staging
else
ipfs_staging:=$(staging)
export ipfs_staging=$(staging)
endif

ifeq ($(data),)
ipfs_data:=~/.ipfs_data
export ipfs_data
else
ipfs_data:=$(data)
export ipfs_data=$(data)
endif

ifeq ($(port),)
public_port:=8080
export public_port
else
public_port:=$(port)
export public_port
endif

SHELL=PATH='$(PATH)' /bin/sh

# enable second expansion
.SECONDEXPANSION:

.PHONY: host
host:
	docker run -d --name ipfs_host_$(TIME) -v $(ipfs_staging):/export -v $(ipfs_data):/data/ipfs -p 4001:4001 -p 4001:4001/udp -p 127.0.0.1:$(port):8080 -p 127.0.0.1:8081:8081 -p 127.0.0.1:5001:5001 ipfs/go-ipfs:latest

include Rules.mk
