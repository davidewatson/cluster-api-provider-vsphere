# Makefile

VERSION ?= $(shell git describe --exact-match 2> /dev/null || \
                 git describe --match=$(git rev-parse --short=8 HEAD) --always --dirty --abbrev=8)
REGISTRY ?=gcr.io/cnx-cluster-api/cluster-api-provider-vsphere-ci-debug

all: build push clean
.PHONY : all

.PHONY : build
build: 
	cd ../../../../ && docker build . -f ./cluster-api-provider-vsphere/scripts/e2e/hack/Dockerfile --tag $(REGISTRY):$(VERSION)
	docker tag $(REGISTRY):$(VERSION) $(REGISTRY):debug
        
push: build
	@echo "logging into gcr.io registry with key file"
	@echo $$GCR_KEY_FILE | docker login -u _json_key --password-stdin gcr.io
	docker push $(REGISTRY):$(VERSION)
	docker push $(REGISTRY):debug
	@echo docker logout gcr.io

clean:
	docker image rm -f $(REGISTRY):$(VERSION)
	docker image rm -f $(REGISTRY):debug
