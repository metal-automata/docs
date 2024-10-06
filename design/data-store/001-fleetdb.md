
# FleetDB

Fleetdb is a persistent data store that holds inventory and configuration for the server fleet. The services within metal-automata rely on this infromation to execute their work.

Servers and other datacenter infrastructure that is required to be managed by the metal-automata project should be enrolled in FleetDB. FleetDB does not have to be the source of truth and acts as a dynamic and current view of the infrastructure.

This document lists the changes for fleetdb to be adapted within the metal-automata project - for usability, functionality and maintainability.

## Current state

FleetDB consists of the [API service](https://github.com/metal-automata/fleetdb/tree/main/pkg/api/v1) and the Database service. The database model is defined in [.sql](https://github.com/metal-automata/fleetdb/tree/main/db/migrations) files, the interfaces and [implementation is generated using sqlboiler](https://github.com/metal-automata/fleetdb/tree/main/internal/models).

### Database service

The current Database used is CockroachDB, since recently the license for CockroachDB has been updated
and the free use of [CockroachDB enterprise entails various requirements](https://www.cockroachlabs.com/enterprise-license-update/) which do not fit the directions of this project and its users. 

**Changes intended**

- fleetDB is to switch to Postgres, this is made easier with sqlboiler.

### Data model

In its current state, the fleetdb data model stores some information in relational tables along with
tables that just hold JSONB fields. While this method of storing information is flexible and was convienient for its time, it introduced various problems - mainly that JSONB data has no server side validation, which resulted in incorrect/inconsistent information being added/updated.

Filtering on JSONB data is not straight forward and there is no way to generate JSONB filter queries for a REST API query parameter. Maintaining REST API query parameters based on JSONB is non-trivial and takes a lot more effort than being able to generate relational queries. The above changes to the data model will enable 
improved filtering on the data.

**Changes intended**

Separate tables are to be added for data currently held in JSONB fields,
and the data held in the respective JSONB fields are to be moved into these tables.

 - BMC records
 - Vendors
 - Models
 - Installed firmware
 - Drive SMART data
 - Component chipsec information storage
 - Server primary identity information
 - Component inventory create, deletes table

### API query filter

FleetDB's [query filter parameters](https://github.com/metal-automata/fleetdb/blob/main/pkg/api/v1/server_component_list_params_test.go) is non-intuitive and often gets in the way when querying data.

Being able to build query parameters and execute searches is fundamental to the data collected being available
and for CLI/Web interfaces to be built around the system. In its current form the query filter built in is not extensible or maintainable. It would be cumbersome to port it to other languages systems that require to query FleetDB.

**Changes intended**

Move query filters to an existing library [go-filterparams](https://github.com/cbrand/go-filterparams)
which also includes its [javascript equivalent](https://github.com/cbrand/js-filterparams-client) which will enable building the frontend.