# spec_deploy

This ansible role is used to set up servers to be able to run SPECstorage2020.

#### Table of Contents

1. [Overview](#overview)
2. [Requirements](#requirements)
3. [Getting Started](#getting-started)
4. [Role Variables](#role-variables)
5. [Usage](#usage)
6. [Authors](#authors)

## Overview
This role completes a variety of tasks that need to be done before running SPECstorage2020. All of these requirements are laid out in the SPEC user guide. The first thing the role will do is set the performance tunings that the user guide requires. It will then install required packages. From there the role will disable firewalls and edit the hosts file to make it easier to connect between the systems that are in the testbed. From there it will do the rest of the SPEC setup, like creating a "spec" user, create a directory to contain all of the SPEC files and results, copy SPEC to all of the machines and unarchive it, mount the NFS shares that you plan on running, and install the python packages that SPEC requires. 

## Requirements

The requirements for running this role are the same basic requirements as running ansible. As long as the SSH keys are properly set up then the role should be able to configure the clients. 

## Getting Started
The first step to running this role is to configure a `spec_role_setup.yml` file. There is an example of what this file should look like below.

    ---
    - hosts: clients
      gather_facts: yes
      vars:
            spec_filename: SPECstorage2020_2529.tar     # The name of your SPECstorage2020 tar file
            spec_location: /home/Schmitt/               # The folder of your SPECstorage2020 tar file
            folder_name: Spec2020                       # The name of the folder will contain your SPECstorage2020 packages and folders
            username: spec
            key_file: ~/.ssh/id_rsa                     # The ssh key you want to copy to your hosts
            nfs_storage_mount_options: hard,rsize=262144,wsize=262144,nconnect=16,vers=3,tcp    # Your nfs mounting options
            local_nfs_share_location: /mnt/freenas      # The location to mount your FlexGroup on your servers
        roles:
            - spec_deploy

## Role Variables

| Variable                 | Default | Description                                                                                                       |
| ------------------------ | ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| spec_filename            | None                                                       | This setting is the name of the archived SPEC file that you would like to copy to the clients. |
| spec_location            | None                                                       | This setting is the folder location of the SPEC file you want to copy to the clients. (The location on the machine you are running ansible from) |
| folder_name              | Spec2020                                                   | This setting is the name of the folder that you want to contain all of the SPEC files and results. |
| username                 | spec                                                       | This is the username for the user that will run SPEC. |
| key_file                 | ~/.ssh/id_rsa                                              | This is the SSH key file that you want to copy to the new user used to run SPEC. While you already have SSH keys set up for the current ansible user, you also need to set them up for the new user. |
| nfs_storage_mount_options| hard,rsize=262144,wsize=262144,nconnect=16,vers=3,tcp      | This setting is the mount options used to mount the NFS share to the clients. |
| local_nfs_share_location | None                                                       | This setting is the location of the share on the remote clients. |
| edit_hosts               | no                                                         | This setting determines if the role will try to configure the hosts file to make it easier to access remote clients. |
| perf_settings            | yes                                                        | This setting determines if the role will set performance settings on the remote clients. |
| mount_nfs_share          | yes                                                        | This setting determines if the role will try to attach an NFS share separate from the NFS share you intend to test on. Can be useful if you have SPEC saved on an NFS share. |

## Usage

Once you have the above file configured, to use the role, you simply need to run `ansible-playbook spec_role_setup.yml`.

## Authors

- [Chris Schmitt](mailto:Chris.Schmitt@netapp.com)
