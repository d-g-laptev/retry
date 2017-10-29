.PHONY: cmd-deps
cmd-deps:
	docker run --rm \
	           -v '${GOPATH}/src/${GO_PACKAGE}':'/go/src/${GO_PACKAGE}' \
	           -w '/go/src/${GO_PACKAGE}/cmd/retry' \
	           kamilsk/go-tools:latest \
	           dep ensure

.PHONY: cmd-test
cmd-test:
	docker run --rm \
	           -v '$(GOPATH)/src/$(GO_PACKAGE)':'/go/src/$(GO_PACKAGE)' \
	           -w '/go/src/$(GO_PACKAGE)' \
	           golang:1.7 \
	           /bin/sh -c 'go install -ldflags "-X 'main.Version=test'" ./cmd/retry \
	                       && retry -limit=3 -backoff=lin{10ms} -- /bin/sh -c "echo 'trying...'; exit 1"'

.PHONY: cmd-deps-local
cmd-deps-local:
	if ! [ -e main.go ]; then cd cmd/retry && dep ensure; else dep ensure; fi

.PHONY: cmd-install
cmd-install:
	if ! [ -e main.go ]; then go install ./cmd/retry; else go install .; fi

.PHONY: cmd-test-local
cmd-test-local: cmd-install
cmd-test-local:
	'${GOPATH}'/bin/retry -limit=3 -backoff=lin{10ms} -- /bin/sh -c "echo 'trying...'