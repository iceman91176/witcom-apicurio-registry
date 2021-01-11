THIRD_PARTY_LIBS=2.5.x
APICURIO_RELEASE=1.3.2.Final
APICURIO_TAG=1.3.2.Final
VERSION=0.0.4

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
	podman build --build-arg THIRD_PARTY_LIBS=$(THIRD_PARTY_LIBS) --build-arg APICURIO_RELEASE=$(APICURIO_RELEASE) --build-arg APICURIO_TAG=$(APICURIO_TAG) -t witcom-apicurio-registry-streams:$(VERSION) .

docker_build: .thirdparty-libs-$(THIRD_PARTY_LIBS).tmp
	docker build --build-arg THIRD_PARTY_LIBS=$(THIRD_PARTY_LIBS) --build-arg APICURIO_RELEASE=$(APICURIO_RELEASE) --build-arg APICURIO_TAG=$(APICURIO_TAG) -t witcom-apicurio-registry-streams:$(VERSION) .

docker_testbuild: .thirdparty-libs-$(THIRD_PARTY_LIBS).tmp
	docker build --target builder --build-arg THIRD_PARTY_LIBS=$(THIRD_PARTY_LIBS) --build-arg APICURIO_RELEASE=$(APICURIO_RELEASE) --build-arg APICURIO_TAG=$(APICURIO_TAG) -t witcom-apicurio-registry-streams:test .

