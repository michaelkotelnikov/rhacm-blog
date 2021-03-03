# RHACM Blog
A collection of policy examples for a Red Hat Advanced Cluster Management blog.

## Repo structure
The repository catagorizes Open Cluster Management policies to NIST Special Publication 800-53 security controls. Each security control is represented by a directory. Each directory has a number of Open Cluster Management policies associated with it.

Enabling the policies helps complying a multi cluster OpenShift environment with the defined NIST Special Publication 800-53 security control.

## Prerequisites
- Some of the policies presented in the repository apply on the [mariadb application](https://gitlab.com/michael.kot/rhacm-demo). In order to deploy the application, run the next command on the hub cluster.
```
$ oc apply -f https://raw.githubusercontent.com/michaelkotelnikov/rhacm-demo/main/rhacm-resources/application.yml
```
- The policies in this repository are deployed in the rhacm-policies namespace. Deploying the namespace can be done by running the next command on the hub cluster.
```
$ oc apply -f https://raw.githubusercontent.com/michaelkotelnikov/rhacm-blog/master/namespace.yml
```
- The policies in this repository are using a PlacementRule resource that maps policies to managed clusters with the 'dev' tag assigned to them. In order to deploy the PlacementRule resource, run the next command on the hub cluster.
```
$ oc apply -f https://raw.githubusercontent.com/michaelkotelnikov/rhacm-blog/master/placementrule.yml
```

## Security control catalog
- [AC - Access Control](#access-control)
- [SC - System and Communications Protection](#system-and-communications-protection)
- [SI - System and Information Integrity](#system-and-information-integrity)

### Access Control
Policy  | Description | Security Control
------- | ----------- | -------------
[policy-remove-kubeadmin](./policies/AC-2/kubeadmin-policy.yml) | Remove the temporary kubeadmin user. | AC-2 Account Management
[group-policy](./policies/AC-3/group-policy.yml) | Create a group with users associated with it. | AC-3 Access Enforcement
[role-policy](./policies/AC-3/role-policy.yml) | Ensure that a role exists with permissions as specified. | AC-3 Access Enforcement
[rolebinding-policy](./policies/AC-3/role-binding-policy.yml) | Ensure that a group is bound to a particular role. | AC-3 Access Enforcement
[disallow-self-provisioner-policy](./policies/AC-6-2/disallow-self-provisioner-policy.yml) | Disallow users to provision namespaces. | AC-6 Least Privilege
[disallowed-role-policy](./policies/AC-6-1/disallowed-role-policy.yml) | Disallow over privilleged roles. | AC-6 Least Privilege

### System and Communications Protection
Policy  | Description | Security Control
------- | ----------- | -------------
[restricted-scc-policy](./policies/SC-4/restricted-scc-policy.yml) | Ensure that the default restricted SCC does not change. | SC-4 Information in Shared Resources
[limitrange-policy](./policies/SC-6/limitrange-policy.yml) | Ensure that the default restricted SCC does not change. | SC-6 Resource Availability
[networkpolicy-denyall-policy](./policies/SC-7/networkpolicy-denyall-policy.yml) | Configures a Network Policy that denys all traffic into a defined namespace | SC-7 Boundary Protection
[networkpolicy-allow-3306-policy](./extra-policies/networkpolicy-allow-3306-policy.yml) | Configure a Network Policy that allows port 3306 traffic into a defined namespace | SC-7 Boundary Protection
[certificate-policy](./policies/SC-8/cert-policy.yml) | Ensure certificates are not expiring within a given minimum time frame | SC-8 Transmission Confidentiality and Integrity
[etcd-encryption-policy](./policies/SC-28/etcdencryption-policy.yml) | Ensure that the etcd database is encrypted | SC-28 (1) Cryptographic Protection

### System and Information Integrity
Policy  | Description | Security Control
------- | ----------- | -------------
[imagemanifestvuln-policy](./policies/SI-4/image-policy.yml) | Detect vulnerabilities in container images. Leverages the [Container Security Operator](https://github.com/quay/container-security-operator) and installs it on the managed cluster if not already present. | SI-4 Information System Monitoring


## Policy Deployment
The policies can be deployed by applying the `policy` YAML file to a hub OpenShift cluster. A policy YAML can be found in each directory in the repository. e.g -
```
oc apply -f https://raw.githubusercontent.com/michaelkotelnikov/rhacm-blog/master/policies/AC-2/kubeadmin-policy.yml
```