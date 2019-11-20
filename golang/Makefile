ALL := ./...
GO_LIST = $(go list ./... | grep -v /vendor/)
GO_FILES = $(find . -type f -name "*.go" -not -path "./vendor/*" -not -path "./.vendor-new/*")

lint:
	golangci-lint run $(GO_FILES)

install:
	go mod download

clean:
	go mod tidy && go clean $(ALL)

update_modules:
	go get -u

build:
	go build

test:
	go test $(GO_LIST)

test_one:
	go test -race

debug:
	dlv debug --listen=:2345