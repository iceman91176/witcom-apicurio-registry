THIRD_PARTY_LIBS=2.6.x

clean:
	rm -rf tmp
	rm -f .*.tmp
	rm -rf kafka-thirdparty-libs/*/target
	rm -f tmp/*.whitelist

.thirdparty-libs-$(THIRD_PARTY_LIBS).tmp: kafka-thirdparty-libs/$(THIRD_PARTY_LIBS)/pom.xml
	test -d tmp || mkdir tmp
	mvn dependency:copy-dependencies $(MVN_ARGS) -f kafka-thirdparty-libs/$(THIRD_PARTY_LIBS)/pom.xml
	touch .thirdparty-libs-$(THIRD_PARTY_LIBS).tmp

podman_build: .thirdparty-libs-$(THIRD_PARTY_LIBS).tmp
	podman build --build-arg THIRD_PARTY_LIBS=$(THIRD_PARTY_LIBS) -t witcom-apicurio-registry-kafka:latest .
