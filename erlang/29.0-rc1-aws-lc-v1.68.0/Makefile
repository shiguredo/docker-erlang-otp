.PHONY: all ubuntu-22.04-x86_64 ubuntu-24.04-x86_64 ubuntu-26.04-x86_64
ERL_VERSION=29.0-rc1-aws-lc-v1.68.0

all: x86_64 arm64v8

x86_64: ubuntu-22.04-x86_64 ubuntu-24.04-x86_64 ubuntu-26.04-x86_64

arm64v8: ubuntu-22.04-arm64v8 ubuntu-24.04-arm64v8 ubuntu-26.04-arm64v8

ubuntu-22.04-x86_64:
	docker buildx build -f ubuntu/22.04-x86_64/Dockerfile --platform=linux/amd64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-22.04-arm64v8:
	docker buildx build -f ubuntu/22.04-arm64v8/Dockerfile --platform=linux/arm64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-24.04-x86_64:
	docker buildx build -f ubuntu/24.04-x86_64/Dockerfile --platform=linux/amd64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-24.04-arm64v8:
	docker buildx build -f ubuntu/24.04-arm64v8/Dockerfile --platform=linux/arm64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-26.04-x86_64:
	docker buildx build -f ubuntu/26.04-x86_64/Dockerfile --platform=linux/amd64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .

ubuntu-26.04-arm64v8:
	docker buildx build -f ubuntu/26.04-arm64v8/Dockerfile --platform=linux/arm64 --no-cache -t ghcr.io/shiguredo/erlang:otp-${ERL_VERSION}-$@ --push .


ubuntu-22.04-multi-arch:
	docker manifest create --amend shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04-x86_64 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04-arm64v8
	docker manifest push shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04

ubuntu-24.04-multi-arch:
	docker manifest create --amend shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04-x86_64 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04-arm64v8
	docker manifest push shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04

ubuntu-26.04-multi-arch:
	docker manifest create --amend shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-26.04 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-26.04-x86_64 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-26.04-arm64v8
	docker manifest push shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-26.04
