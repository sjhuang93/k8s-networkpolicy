# k8s-networkpolicy
This chart is for manage networkpolicy easily.

Automatically generate networkpolicy fulfill your policy.

## How to use

Example

```yaml
## Global variables
global:
  platform: on-premises
  environment: dev


basicNetworking:
  ## Allow access to vpcCidr traffic.
  vpcCidr: 0.0.0.0/0
  ## cloud_api_cidr only exists in the cloud, please enter cloud provider API address CIDR, allow access to cloud provider API traffic.
  cloudApiCidr: 10.2.0.0/16
  ## Deny access these CIDRs traffic, must be a strict subset of vpcCidr.
  blockCidrs: 
    - 10.0.0.0/8
    - 172.16.0.0/12
    - 192.168.0.0/16


## Allow traffic from special ingress controller to service.
## You can allow traffic to namespaces or pods or multiple service.
ingressControllerToServices:
  -  sourceControllerName: ingress-controller-1
     sourceControllerNamespace: kube-public
     targetNamespaces: 
       - ns1
       - ns2
  -  sourceControllerName: ingress-controller-2
     sourceControllerNamespace: kube-public
     targetNamespace: ns3
     targetPods:
       - pods1
  -  sourceControllerName: ingress-controller-3
     sourceControllerNamespace: kube-public
     targetNamespace: ns4
     targetPods:
       - pods2
       - pods3


## Allow namespace to namespaces one-way traffic networkpolicy.
namespaceToNamespaces:
  - sourceNamespace: ns1
    targetNamespaces: 
      - ns2
      - ns3


## Allow namespace to specific pods one-way traffic networkpolicy.
namespaceToPods:
  - sourceNamespace: ns1
    targetNamespace: ns2
    targetPods:
      - pods1


## Allow namespace to specific CIDRs one-way traffic networkpolicy.
namespaceToCidrs:
  - sourceNamespace: ns1
    targetCidrs:
      - 10.1.0.1/32
  - sourceNamespace: ns2
    targetCidrs:
      - 10.2.0.1/32
      - 10.3.0.1/32


## Allow specific pods of the namespace to specific namespaces one-way traffic networkpolicy.
podsToNamespace:
  - sourceNamespace: ns1
    sourcePods:
      - pods1
      - pods2
    targetNamespaces:
      - ns1
      - ns2


## Allow specific pods of the namespace to specific pods of namespace one-way traffic networkpolicy.
podsToPods:
  - sourceNamespace: ns1
    sourcePods:
      - pods1
      - pods2
    targetNamespace: ns3
    targetPods:
      - pods4
      - pods5


## Allow specific pods of the namespace to specific CIDRs one-way traffic networkpolicy.
podsToCidrs:
  - sourceNamespace: ns1
    podsourcePodss:
      - pods1
      - pods2
    targetCidrs:
      - 10.6.8.9/32
      - 10.6.9.6/32
```