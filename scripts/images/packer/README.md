# Packer.io Template

## Introduction

This is based on the [GPL-licenced](https://github.com/kornrunner/packer-arch/blob/12748faaa76933281046cc5206c59436ffd75434/LICENSE) [kornrunner/packer-arch](https://github.com/kornrunner/packer-arch/tree/12748faaa76933281046cc5206c59436ffd75434).

## Prerequisites

* [`aur/packer-io`](https://aur.archlinux.org/packages/packer-io/)

## Usage

First, we validate `template.json`.

    packer-io validate ./template.json

If the validation succeeds, we build the image.

    packer-io build ./template.json

After `packer-io` succeeds, we find output files unter `./output` and our Vagrant basebox image under `./blackarch_virtualbox.box`.

Now we can import the image.

    vagrant box add blackarch ./packer_blackarch_virtualbox.box

Finally, we run a Vagrant virtual machine based on the image.

    cd /tmp
    vagrant init --minimal blackarch
    vagrant up
    vagrant ssh