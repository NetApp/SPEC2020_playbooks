# SPECstorage2020 Setup

This repository contains ansible roles and playbooks to help with the setup of SPECstorage2020. 

#### Table of Contents

1. [Overview](#overview)
2. [Requirements](#requirements)
3. [Getting Started](#getting-started)
4. [Usage](#usage)
5. [Authors](#authors)

## Overview

There are two main folders in this repository, playbooks and roles. As a user you should not have to make changes in the roles folders, all of your changes should be done within the playbooks folder. 

## Requirements

## Getting Started

The first thing you need to do to start using thes tools is to configure your host file. Your host file contains all of the information ansible needs to be able to configure your servers and storage. 

1. Copy ssh keys to your servers using `ssh-copy-id`
2. Connect to your servers using the SSH keys to verify connection
3. Configure your `hosts` file with your storage IP and server IPs
4. Configure your storage setup playbook (which uses the na_storage role) to configure your netapp storage. There is an example EDA setup in the `playbooks/storage/` folder called `eda_setup.yml`
5. Add your LIF IPs to your clients servers in your `hosts` file. 
6. Configure and run the `spec_role_setup.yml`
7. Configure and run `run_auto_test.yml` to run your first test and generate a report.

## Usage

`ssh-copy-id`
`ansible-playbook playbooks/storage/eda_setup.yml`
`ansible-playbook playbooks/servers/spec_role_setup.yml`
`ansible-playbook playbooks/servers/run_auto_test.yml`

## Authors

- [Chris Schmitt](mailto:Chris.Schmitt@netapp.com)
