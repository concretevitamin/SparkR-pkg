
SCALA_VERSION := 2.10
SPARKR_VERSION := 0.1
JAR_NAME := sparkr-assembly-$(SPARKR_VERSION).jar
SBT_TARGET_NAME := target/scala-$(SCALA_VERSION)/$(JAR_NAME)

MAVEN_JAR_NAME := sparkr-$(SPARKR_VERSION)-assembly.jar
MAVEN_TARGET_NAME := target/$(MAVEN_JAR_NAME)

SCALA_SOURCE_DIR := src/main/scala/edu/berkeley/cs/amplab/sparkr
RESOURCE_DIR := src/main/resources

SCALA_FILES := $(wildcard $(SCALA_SOURCE_DIR)/*.scala)
RESOURCE_FILES := $(wildcard $(RESOURCE_DIR)/*)

SPARK_HADOOP_VERSION ?= 1.0.4
SPARK_VERSION ?= 1.1.0

ifdef USE_MAVEN
	TARGET_NAME := $(MAVEN_TARGET_NAME)
	BUILD_TOOL := mvn
else
	TARGET_NAME := $(SBT_TARGET_NAME)
	BUILD_TOOL := sbt/sbt
endif

all: $(TARGET_NAME)

$(SBT_TARGET_NAME): build.sbt $(SCALA_FILES) $(RESOURCE_FILES)
	./sbt/sbt assembly
	cp -f $(SBT_TARGET_NAME) ../inst/

$(MAVEN_TARGET_NAME): pom.xml $(SCALA_FILES) $(RESOURCE_FILES)
	mvn -Dhadoop.version=$(SPARK_HADOOP_VERSION) -Dspark.version=$(SPARK_VERSION) -DskipTests clean package shade:shade
	cp -f $(MAVEN_TARGET_NAME) ../inst/$(JAR_NAME)

clean:
	$(BUILD_TOOL) clean
	rm -rf target
	rm -rf project/target
	rm -rf project/project
	-rm sbt/sbt-launch-*.jar
	rm -f ../inst/$(JAR_NAME)

.PHONY: all clean
