# na_storage

This storage role can be used to set up a variety of storage components. It can create vservers, activates protocols, creates volumes, creates LIFs, and creates the appropriate components depending on the protocol. For NVME it can set up namespaces and subsystems and for FCP it can set up LUNs and igroups. In addition, it collects node and aggregate information that is used to set up the LIFs.

## Requirements

All of the requirements to run ONTAP modules are also required to run this role. Those requirements as well as additional requirements are listed below.

- Needs aggregates to already be created
- A physical or virtual clustered Data ONTAP system. The modules support Data ONTAP 9.1 and onward.
- Ansible 2.6
- Ansible 2.9 or later is strongly recommended as it enables the new collection delivery system.
- Python2 netapp-lib (2017.10.30) or later. Install using ‘pip install netapp-lib’
- Python3 netapp-lib (2018.11.13) or later. Install using ‘pip install netapp-lib’

## YAML Role Variables

The storage role uses a YAML variable setup to create the components used to set up storage. An example of this format is included below.

    ---
    - hosts: storage
      vars:
        ansible_python_interpreter: /root/anaconda3/bin/python3
        ipspaces:
          - name: data
            state: present
        broadcast_domains:
          - name: data
            state: present
            ipspace: data
            mtu: 9000
            ports: 
              - example_node1:e0b
              - example_node1:e1b
              - example_node1:e3a
              - example_node1:e3b
              - example_node1:e4a
              - example_node1:e4b
              - example_node2:e0b
              - example_node2:e1b
              - example_node2:e3a
              - example_node2:e3b
              - example_node2:e4a
              - example_node2:e4b
        licenses: [example-license-here]
        vservers:
          - name: example_eda
            state: present
            count: 1
            ipspace: Default
            security_style: unix
            snap_policy: none
            allowed_protocols: nfs
            export_policy:
              - name: example_eda
                state: present
                rule:
                  - name: example_eda
                    state: present
                    rule_index: 100
                    client_match: 0.0.0.0/0
                    rw_rule: any
                    ro_rule: any
                    protocol: nfs,nfs3
                    super_user_security: any
                    anonymous_user_id: 65534
                    ntfs_unix_security: ignore
            lifs:
              - name: example_eda
                state: present
                protocol: nfs
                netmask: '255.255.255.0'
                ips: 
                  - node_name: example_node1
                    ports:
                    - port: e3a
                      ips:
                      - '192.168.2.23'
                  - node_name: example_node1
                    ports:
                    - port: e3b
                      ips:
                      - '192.168.2.24'
                  - node_name: example_node1
                    ports:
                    - port: e4a
                      ips:
                      - '192.168.2.25'
                  - node_name: example_node1
                    ports:
                    - port: e4b
                      ips:
                      - '192.168.2.26'
                  - node_name: example_node2
                    ports:
                    - port: e3a
                      ips:
                      - '192.168.2.29'
                  - node_name: example_node2
                    ports:
                    - port: e3b
                      ips:
                      - '192.168.2.30'
                  - node_name: example_node2
                    ports:
                    - port: e4a
                      ips:
                      - '192.168.2.31'
                  - node_name: example_node2
                    ports:
                    - port: e4b
                      ips:
                      - '192.168.2.32'
            nfs:
              tcp_max_xfer_size: 262144
            flexgroups: 
              - name: example_eda
                state: present
                count: 4
                multiplier: 8
                size: 2
                size_unit: "tb"
                junction_path: "/example_eda_fg"
                export_policy: example_eda_vserver_1_export_policy
                policy: default
                snapshot_policy: none
                space_guarantee: none
                qos_policy_group: none
                percent_snapshot_space: '0'

## Dependencies

This role is not dependent on any other Ansible roles. Only dependency is the netapp.ontap collection.

## Host File Example

Inventory host file example. This one is set up to be able to reference two HA pairs at the same time.

    [storage:children]
    storage1
    storage2

    [storage:vars]
    ntap_username=username
    ntap_password=password

    [storage1]
    10.0.0.1 name=epic_storage1 ansible_connection=local

    [storage2]
    10.0.0.2 name=epic_storage2 ansible_connection=local

## License

BSD

## Author Information

- [Chris Schmitt](mailto:Chris.Schmitt@netapp.com)
