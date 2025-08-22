# Nomad Enterprise features

Nomad Enterprise offers a range of advanced features designed to enhance
collaboration, operations, and governance for organizations managing workloads
at scale.

## **Platform Features**
1. **Advanced Autopilot Features**:
   - **Automated Upgrades**: Simplifies upgrading Nomad clusters with minimal disruption.[](https://developer.hashicorp.com/nomad/docs/enterprise)
   - **Enhanced Read Scalability**: Introduces non-voting server nodes to scale
     read operations and scheduling without impacting write
     latency.[](https://developer.hashicorp.com/nomad/docs/enterprise)
   - **Redundancy Zones**: Deploys non-voting servers as hot standbys per
     availability zone to enhance availability (e.g., one voter and one
     non-voter per zone in a multi-zone
     setup).[](https://developer.hashicorp.com/nomad/docs/enterprise)
2. **Snapshot Backup and Restore**: Provides atomic, point-in-time snapshots for
   automated backup and restoration of Nomad server states, aiding disaster
   recovery.[](https://developer.hashicorp.com/nomad/docs/enterprise)[](https://newreleases.io/project/github/hashicorp/nomad/release/v0.12.0)

## **Governance & Policy Features (Add-on Module)**
3. **Audit Logging**: Captures a complete record of user-issued actions via an
   HTTP API in JSON format, enabling proactive identification of access
   anomalies, enforcement of security policies, and cluster behavior
   diagnostics.[](https://developer.hashicorp.com/nomad/docs/enterprise)[](https://github.com/hashicorp/field-workshops-nomad/blob/main/docs/slides/multi-cloud/nomad-oss/nomad-1.md)
4. **Resource Quotas**: Limits resource consumption (CPU, memory) across teams
   or projects to reduce waste and align with
   budgets.[](https://developer.hashicorp.com/nomad/docs/enterprise)[](https://github.com/hashicorp/field-workshops-nomad/blob/main/docs/slides/multi-cloud/nomad-oss/nomad-1.md)[](https://medium.com/hashicorp-engineering/governance-for-multiple-teams-sharing-a-nomad-cluster-1ef9b7c77cb5)
5. **Sentinel Policies**: Enables fine-grained policy enforcement as code, such
   as restricting job submissions (e.g., no production jobs on Fridays) or
   limiting jobs to pre-authorized Docker images. Builds on the ACL system for
   compliance.[](https://developer.hashicorp.com/nomad/docs/enterprise)[](https://github.com/hashicorp/field-workshops-nomad/blob/main/docs/slides/multi-cloud/nomad-oss/nomad-1.md)[](https://atodorov.me/2021/02/27/why-you-should-take-a-look-at-nomad-before-jumping-on-kubernetes/)
6. **Namespaces**: Partitions a shared cluster to isolate jobs and objects for
   different teams, allowing unique rules per namespace and cross-namespace
   queries via the HTTP API and CLI. (Note: Namespaces are now also available in
   Nomad
   open-source.)[](https://github.com/hashicorp/field-workshops-nomad/blob/main/docs/slides/multi-cloud/nomad-oss/nomad-1.md)[](https://www.hashicorp.com/en/resources/why-should-i-consider-nomad-enterprise)[](https://www.hashicorp.com/en/resources/why-should-i-consider-nomad-enterprise)
7. **Access Control Lists (ACLs)**: Provides fine-grained control to manage
   access and permissions across teams and
   namespaces.[](https://www.hashicorp.com/en/resources/why-should-i-consider-nomad-enterprise)[](https://medium.com/hashicorp-engineering/governance-for-multiple-teams-sharing-a-nomad-cluster-1ef9b7c77cb5)

## **Multi-Cluster & Efficiency Features (Add-on Module)**
8. **Multiregion Deployments**: Enables deployment of a single job across
   multiple federated regions with configurable rollout and rollback strategies,
   supporting global-scale
   infrastructure.[](https://developer.hashicorp.com/nomad/docs/enterprise)[](https://newreleases.io/project/github/hashicorp/nomad/release/v0.12.0)
9. **Dynamic Application Sizing (DAS)**: Optimizes resource consumption with sizing recommendations, leveraging autoscaling capabilities, a new API/UI for reviewing job changes, and Nomad Autoscaler plugins based on statistical measures.[](https://developer.hashicorp.com/nomad/docs/enterprise)
10. **Horizontal Application Autoscaling**: Dynamically adjusts task group
    counts based on application performance metrics
    (APM).[](https://github.com/hashicorp/field-workshops-nomad/blob/main/docs/slides/multi-cloud/nomad-oss/nomad-1.md)
11. **Horizontal Cluster Autoscaling**: Dynamically scales the size of a Nomad
    cluster (number of clients), currently supported in
    AWS.[](https://github.com/hashicorp/field-workshops-nomad/blob/main/docs/slides/multi-cloud/nomad-oss/nomad-1.md)

## **Additional Features**
12. **Built-in Security**:
    - **Data Encryption**: Supports encryption of data in transit and at rest.
    - **Role-Based Access Control (RBAC)**: Enhances security with granular
      access management.
    - **Compliance with Industry Standards**: Ensures adherence to enterprise security requirements.[](https://filecr.com/windows/hashicorp-nomad-enterprise/)
13. **Performance Optimization**: Dynamically allocates resources and balances
    workloads to optimize application and service
    performance.[](https://filecr.com/windows/hashicorp-nomad-enterprise/)
14. **Multi-Cloud Support**: Facilitates deployment across multiple cloud
    platforms, including AWS, Microsoft Azure, and Google Cloud Platform, for
    hybrid and multi-cloud
    architectures.[](https://filecr.com/windows/hashicorp-nomad-enterprise/)
15. **Debug Log Archive**: Captures state and logs to facilitate support and
    troubleshooting.[](https://newreleases.io/project/github/hashicorp/nomad/release/v0.12.0)
16. **Scaling UI**: Allows quick adjustment of task group counts directly from
    the UI for task groups with scaling
    declarations.[](https://newreleases.io/project/github/hashicorp/nomad/release/v0.12.0)

## **Notes**
- Nomad Enterprise is available as a base Platform package with an optional
  Governance & Policy add-on module and Multi-Cluster & Efficiency add-on
  module.[](https://developer.hashicorp.com/nomad/docs/enterprise)
- A Nomad Enterprise cluster cannot be downgraded to the open-source version, as
  open-source servers will panic when joining an Enterprise cluster due to
  incompatible raft
  entries.[](https://developer.hashicorp.com/nomad/docs/enterprise)
- For pricing details or to request a demo, visit
  https://www.hashicorp.com/products/nomad/pricing.[](https://www.hashicorp.com/products/nomad/pricing)[](https://www.trustradius.com/products/hashicorp-nomad/pricing)
- The list above focuses on features exclusive to or enhanced in Nomad
  Enterprise compared to the open-source version, based on the provided
  references.

