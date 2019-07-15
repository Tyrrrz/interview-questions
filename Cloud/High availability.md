# High availability

___

**Q: How do you understand the term "high availability system"?**

"High availability" can be attributed to a system which is designed to ensure highest possible uptime in order to satisfy its operational requirements.

___

**Q: What can we do to ensure high availability?**

- Eliminate single points of failure, i.e. introduce redundancy
- Monitor system's vital signs to detect and eliminate failures before users notice them
- Implement reliable crossover to switch from broken nodes to healthy nodes without losing data

___

**Q: How is availability calculated?**

Service providers state their service level agreements as a minimal percentage of time a system is expected to be available, usually in a span of a month.

Guaranteed availabilities of 90% and higher are usually referred to as number of nines, e.g. 90% is "one nine", 99% is "two nines", 99.9% is "three nines" and so on.

Availability of a system is equal to the availability of a component with the lowest availability. In other words, if a system has 3 required components, one with 99.9% availability, one with 99.999% availability, and one with 99.999999% availability, the total availability will be 99.9% because it takes one component to bring the system down.

When we're dealing with redundancy, the total availability of a component can be calculated as reverse probability of all replicas being down at the same time. For example, if we replicate a component with 95% availability over 3 nodes, the probability that all nodes are down at the same time is `(1 - 0.95)^3 = 0.000125` which means the availability is `1 - 0.000125 = 0.999875`, so 99.9875%.
