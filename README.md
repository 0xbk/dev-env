# Ubuntu Development Environment

This repository contains the files needed to quickly create my development environment virtual machine (VM). The main files are:

* [`Vagrantfile`](./Vagrantfile) - used by the [workflow](#workflow) to create and configure the VM.
* [`Vagranfile.package`](./Vagrantfile.package) - packaged with the vagrant box. Defines settings used during `vagrant up`.

## Requirements

A few things need to be installed before using the VM:

* Windows 11
* [VirtualBox](https://www.virtualbox.org) v7.0.6 - [download](https://download.virtualbox.org/virtualbox/7.0.6/VirtualBox-7.0.6-155176-Win.exe)
* [Vagrant](https://developer.hashicorp.com/vagrant) v2.3.7 - [download](https://releases.hashicorp.com/vagrant/2.3.7/vagrant_2.3.7_windows_amd64.msi)
* [Git for Windows](https://git-scm.com) v2.42.0 - [download](https://github.com/git-for-windows/git/releases/download/v2.42.0.windows.2/Git-2.42.0.2-64-bit.exe)

The versions listed are what I use and know work. Older or newer versions may work but are untested.

## Quick start

Open a Git Bash terminal and run the following commands:

```bash
# Create a directory
mkdir dev-env && cd dev-env
# Initialize the environment
vagrant init 0xbk/dev-env
# Create the VM
vagrant up
```

The first time the `vagrant up` command is run it may take a while to download the ~5GB box file. Subsequent runs will be much quicker.

## Workflow

The workflow creates and publishes the vagrant box to [Vagrant Cloud](https://app.vagrantup.com/0xbk/boxes/dev-env) using a self-hosted runner. Configuration of the runner on your local machine is done with the following steps:

1. Ensure all [Requirements](#requirements) have been installed.
1. Add the Git Bash `bin` directory as the __first entry__ to the __system environment variable__ named  `Path`.
    * If not modified during installation, the default directory is `C:\Program Files\Git\bin`.
    * After adding, ensure the `bash` command starts Git Bash and not Windows Subsystem for Linux (WSL).
1. Add the runner to your repository, see [Adding self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners#adding-a-self-hosted-runner-to-a-repository).

The workflow is triggered by a `workflow_dispatch` event and must be manually started. See [the workflow file](./.github/workflows/create-publish-vagrant-box.yml) for all the details.

