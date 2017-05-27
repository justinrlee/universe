REPO_BASE_DIR ?= $(shell pwd -P)/../..
.PHONY: certs base clean static

certs:
	mkdir certs && openssl req					\
		-newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key	\
		-x509 -days 365 -out certs/domain.crt			\
		-subj "/CN=master.mesos"

base: static
	docker build -t universe-base -f Dockerfile.base .

static:
	docker build -t universe-static -f Dockerfile.static .

online-static:
	docker pull justinrlee/universe-static:0.1

clean:
	rm -rf certs &&							\
	rm -f local-universe.tar.gz || 0

local-universe: clean
	python3 $(REPO_BASE_DIR)/scripts/local-universe.py			\
		--repository $(REPO_BASE_DIR)/repo/packages/	\
		--selected &&							\
	docker save -o local-universe.tar mesosphere/universe:latest &&		\
	gzip local-universe.tar
