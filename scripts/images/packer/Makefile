BUILD_DIR ?= ./output
TEST_DIR ?= ./testing
BOX_NAME ?= blackarch
PROVIDER ?= virtualbox
BOX_FILENAME ?= $(BOX_NAME)_$(PROVIDER).box
BOX_DEFINITION ?= ./template.json
PACKER ?= packer-io

.PHONY: all clean deepclean validate build

all: build

build: $(BUILD_DIR)/$(BOX_FILENAME)

clean:
	rm -rf $(BUILD_DIR)
	-cd $(TEST_DIR)        && \
	    vagrant destroy -f && \
	    cd ..
	rm -rf $(TEST_DIR)
	rm -f *.{hwm,pwd,pwi}

deepclean: clean
	rm -rf ./packer_cache

validate:
	$(PACKER) validate $(BOX_DEFINITION)

$(BUILD_DIR)/$(BOX_FILENAME): validate
	$(PACKER) build -var 'git_revision=$(shell git rev-parse --abbrev-ref HEAD):$(shell git rev-parse --verify HEAD)' $(BOX_DEFINITION)

test:
	vagrant box add --force $(BOX_NAME) $(BUILD_DIR)/$(BOX_FILENAME) && \
	  mkdir -pv $(TEST_DIR) && \
	  cd $(TEST_DIR) && \
	  -vagrant destroy -f && \
	  -rm ./Vagrantfile && \
	  vagrant init --minimal $(BOX_NAME) && \
	  vagrant up && \
	  vagrant ssh -c uname -a && \
	  vagrant destroy -f
