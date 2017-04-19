GOPATH := $(shell pwd)
export GOPATH
PATH := ${PATH}:$(shell pwd)/bin
export PATH


build:
	go install github.com/ohsu-comp-bio/thales

download:
	go get -d github.com/ohsu-comp-bio/thales
	go get github.com/golang/protobuf/protoc-gen-go
	go get github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway/
