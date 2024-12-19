To Deploy the Neutron Opflex Agent, Opflex Agent, DHCP Agent & LLDP Agent

- Apply the services

oc apply -f neutron-opflex-agent-service.yaml -n openstack
oc apply -f ciscoaci-lldp-agent-service.yaml -n openstack
oc apply -f cisco-opflex-agent-service.yaml -n openstack

- Make sure that they're up

oc get openstackdataplaneservice -n openstack

- Create the nodeset_patch.yaml based on the templated included.
	* edpm_cisco_opflex_agent_aci_apic_systemid should be set
	* Make sure the images given in this file are the correct ones

- Create the nodeset_patch_deploy.yaml based on the template included.

- Apply the patch

oc patch openstackdataplanenodeset openstack-data-plane -n openstack --type merge --patch-file nodeset_patch.yaml

- Apply the deployment file

oc apply -f nodeset_patch_deploy.yaml

- Deployment should now be running

$ oc get pod -l app=openstackansibleee
NAME                                                              READY   STATUS      RESTARTS   AGE
bootstrap-data-plane-deploy-openstack-data-plane-kz9g7            0/1     Completed   0          53m
configure-network-data-plane-deploy-openstack-data-plane-c5xlp    0/1     Completed   0          52m
configure-os-data-plane-deploy-openstack-data-plane-9kq27         0/1     Completed   0          50m
install-certs-data-plane-deploy-openstack-data-plane-hgrkx        0/1     Completed   0          49m
install-os-data-plane-deploy-openstack-data-plane-7wjtr           0/1     Completed   0          51m
libvirt-data-plane-deploy-openstack-data-plane-lvddl              0/1     Completed   0          48m
neutron-metadata-data-plane-deploy-openstack-data-plane-vxz79     0/1     Completed   0          48m
neutron-opflex-agent-custom-service-deploy-new-service-opeq5z8g   1/1     Running     0          11s
nova-data-plane-deploy-openstack-data-plane-56chl                 0/1     Completed   0          46m
ovn-data-plane-deploy-openstack-data-plane-qlnm2                  0/1     Completed   0          49m
reboot-os-data-plane-deploy-openstack-data-plane-rl5f6            0/1     Completed   0          49m
run-os-data-plane-deploy-openstack-data-plane-lmgsc               0/1     Completed   0          49m
ssh-known-hosts-data-plane-deploy-jlqwf                           0/1     Completed   0          50m
validate-network-data-plane-deploy-openstack-data-plane-rgv8q     0/1     Completed   0          51m
