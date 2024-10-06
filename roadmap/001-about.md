## Metal Automata

The metal-automata project is to provide automation and workflows for server firmware management along with hardware configuration and a few other areas mentioned in the scope below.

This project is intended to fill in the gaps when dealing with a fleet of servers and is based on patterns and insight gained from similar environments. The primary goal of this project is provide software functioning in a coheisive manner with the user interfaces that 
simplify dealing with a heterogenous server fleet, in a vendor agnostic manner.

The target audience for this project is Server adminstrators, operators or home lab owners.

### In scope

Software components/services listed in priority of implementation,

 - Server SKU, Component, Firmware inventory collection
 - Server TPM public key registration
 - Firmware downloads and storage
 - Firmware blob scanning and verification
 - Firmware install automation
 - Workflow orchestrator, scheduler, notifier
 - BIOS and BMC configuration
 - Hardware benchmarking and validation (post firmware install/BIOS update)
 - Metrics collection for,
  - Power consumption
  - Fans and thermal information
 - Hardware health monitoring
 - Admin/Operator dashboard to interface with the services and automation
 - Admin/Operator CLI to interface with the services
 - And finally a AI/ML based service that can debug servers broken down with the above tooling - to determine cause and take remidiation actions using the workflow system.

### Out of scope

The services out of scope for this project are,

 - Provision an OS on a server
 - Provide DHCP, TFTP services
 - Provide IPAM services
 - Implementing a DCIM store

Projects like ironic, MAAS, Tinkerbell, Sidero focus on enabling 
provisioning and management of server infrastructure and this project is not intended to a replacement, but to function standalone along with the possibility to integrate.

It's quite likely that some of these services will be required to enable the in-scope services - for example, the inband firmware update system will require an existing DHCP service. In such the cases this project will attempt to integrate services from existing project and only choose to build from scratch - if the integration efforts outweigh the efforts to build in.