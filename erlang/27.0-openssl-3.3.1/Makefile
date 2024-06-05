.PHONY: all rhel-8.10-x86_64 rhel-9.4-x86_64 ubuntu-20.04-x86_64 ubuntu-22.04-x86_64 ubuntu-24.04-x86_64 
ERL_VERSION=27.0-openssl-3.3.1

all: x86_64 arm64v8

x86_64: rhel-8.10-x86_64 rhel-9.4-x86_64 ubuntu-20.04-x86_64 ubuntu-22.04-x86_64 ubuntu-24.04-x86_64

arm64v8: ubuntu-20.04-arm64v8 ubuntu-22.04-arm64v8 ubuntu-24.04-arm64v8

rhel-8.10-x86_64:
	cd rhel/8.10-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t shiguredo/erlang:otp-${ERL_VERSION}-$@ .
	docker push shiguredo/erlang:otp-${ERL_VERSION}-$@

rhel-9.4-x86_64:
	cd rhel/9.4-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t shiguredo/erlang:otp-${ERL_VERSION}-$@ .
	docker push shiguredo/erlang:otp-${ERL_VERSION}-$@

ubuntu-20.04-x86_64:
	cd ubuntu/20.04-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t shiguredo/erlang:otp-${ERL_VERSION}-$@ .
	docker push shiguredo/erlang:otp-${ERL_VERSION}-$@

ubuntu-20.04-arm64v8:
	cd ubuntu/20.04-arm64v8 && docker buildx build --platform=linux/arm64 --no-cache -t shiguredo/erlang:otp-${ERL_VERSION}-$@ .
	docker push shiguredo/erlang:otp-${ERL_VERSION}-$@

ubuntu-22.04-x86_64:
	cd ubuntu/22.04-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t shiguredo/erlang:otp-${ERL_VERSION}-$@ .
	docker push shiguredo/erlang:otp-${ERL_VERSION}-$@

ubuntu-22.04-arm64v8:
	cd ubuntu/22.04-arm64v8 && docker buildx build --platform=linux/arm64 --no-cache -t shiguredo/erlang:otp-${ERL_VERSION}-$@ .
	docker push shiguredo/erlang:otp-${ERL_VERSION}-$@

ubuntu-24.04-x86_64:
	cd ubuntu/24.04-x86_64 && docker buildx build --platform=linux/amd64 --no-cache -t shiguredo/erlang:otp-${ERL_VERSION}-$@ .
	docker push shiguredo/erlang:otp-${ERL_VERSION}-$@

ubuntu-24.04-arm64v8:
	cd ubuntu/24.04-arm64v8 && docker buildx build --platform=linux/arm64 --no-cache -t shiguredo/erlang:otp-${ERL_VERSION}-$@ .
	docker push shiguredo/erlang:otp-${ERL_VERSION}-$@

ubuntu-22.04-multi-arch:
	docker manifest create shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04-x86_64 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04-arm64v8
	docker manifest push shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-22.04
	
ubuntu-24.04-multi-arch:
	docker manifest create shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04-x86_64 \
		shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04-arm64v8
	docker manifest push shiguredo/erlang:otp-${ERL_VERSION}-ubuntu-24.04
