default: test

include ../../../helpers/examples.mk

RELEASE := helm-kibana-security

install:
	helm upgrade --wait --timeout=$(TIMEOUT) --install --values values.yaml $(RELEASE) ../../

test: secrets install goss

purge:
	kubectl --namespace monitoring delete secret kibana || true
	helm del $(RELEASE)

secrets:
	encryptionkey=$$(docker run --rm docker.elastic.co/kibana/kibana:$(STACK_VERSION) /bin/sh -c "< /dev/urandom tr -dc _A-Za-z0-9 | head -c50") && \
	kubectl --namespace monitoring create secret generic kibana --from-literal=encryptionkey=$$encryptionkey
