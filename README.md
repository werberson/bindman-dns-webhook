# Sandman DNS Webhook
This repository lays out the pieces of code of a sandman DNS manager webhook.

The libraries present here should be used in order to ease out integrations among listeners and managers.

# Listeners and Managers

The objective behind this project is to automate the management of DNS records of a number of DNS Servers that are sitting above another number of Sandman clusters.

- **Listeners** are pieces of software that listens to events that a specific Sandman cluster emits, filtering for events of new services being added. A Sandman cluster expects these events to be annotated with labels identifying which DNS hostname should be assigned to a service. Once these labels are identified, the modification of a DNS Record is delegated to its DNS manager.

- **Managers** are pieces of software that receives requests from listeners to modify its DNS Server records. 

# Samples
Two samples are provided in the `samples` folder.

- **client**: demonstrates how one can leverage the client library to communicate with the DNS manager webhook APIs. It is configured via the `MANAGER_ADDRESS` and `REVERSE_PROXY_ADDRESS` environment variables which, respectively, defines the address of the manager instance and the address of the reverse proxy that will handle requests to the Sandman services.

- **hook**: demonstrates how one can leverage the hook library to receive requests modifying the DNS records it manages. It expects the `TAGS` environment variable to be defined.

A postman collection is provided (`samples/sandman-dns-webhook-samples.postman_collection.json`) thats lays out the available apis and how to communicate with them.

To build and run the samples just type `docker-compose up` from the samples folder.

# Tags

When a Sandman cluster runs a new service, it is expected from this service to be annotaded with labels indicating what the hostname for that service should be.

More than that, a service should also be labeled with information regarding its exposure. In other words, which DNS Server should handle DNS queries to the hostname the service is expected to be attached to.

Sandman accomplishes that by adopting a Tag system, where each service launched by a Sandman cluster is annotated with a set of Tags and each DNS manager is also run with its own set of Tags.

**The intersection of these two Tag sets indicates to the Sandman which DNS Managers will manage which service hostname attribution.**