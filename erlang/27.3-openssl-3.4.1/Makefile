.PHONY: all rhel-8.10-x86_64 rhel-9.5-x86_64 ubuntu-22.04-x86_64 ubuntu-24.04-x86_64 
ERL_VERSION=27.3-openssl-3.4.1

all: x86_64 arm64v8

x86_64: rhel-8.10-x86_64 rhel-9.5-x86_64 ubuntu-22.04-x86_64 ubuntu-24.04-x86_64

arm64v8: ubuntu-22.04-arm64v8 ubuntu-24.04-arm64v8

rhel-8.10-x86_64:
	cd rhel/8.10-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

rhel-9.5-x86_64:
	cd rhel/9.5-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-22.04-x86_64:
	cd ubuntu/22.04-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-22.04-arm64v8:
	cd ubuntu/22.04-arm64v8 && docker buildx build --platform=linux/arm64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-24.04-x86_64:
	cd ubuntu/24.04-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-24.04-arm64v8:
	cd ubuntu/24.04-arm64v8 && docker buildx build --platform=linux/arm64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-22.04-multi-arch:
	docker manifest create --amend ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04 \
		ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04-x86_64 \
		ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04-arm64v8
	docker manifest push ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04

ubuntu-24.04-multi-arch:
	docker manifest create --amend ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04 \
		ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04-x86_64 \
		ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04-arm64v8
	docker manifest push ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04
