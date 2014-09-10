---
title: Release Notes
---

## v1.3

- **Syslog forwarding**: Syslogs are now streamed to the same host and port configured in Elastic Runtime settings
- **Trusty stemcells**: Server and broker are now deployed on Ubuntu “Trusty” 14.04 LTS stemcells, providing improved security, performance, and a smaller resource footprint.

### Known Issues

- Garbage collection is not yet functional. When objects are deleted, they are no longer listed for bucket contents, but they are not deleted from disk. This can lead to persistent disk filling up. Until this issue is resolved, operators should increase the persistent disk allocated to Riak CS nodes.
