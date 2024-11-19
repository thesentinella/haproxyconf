# HAProxy Configuration for Multi-Site High Availability

This repository contains configuration files and documentation to set up HAProxy for a high-availability environment across multiple sites. The setup uses Keepalived for VIP management and supports failover between nodes within a site as well as between geographically separate sites.

---

## Features

- **High Availability**: Ensure uptime with automatic failover between HAProxy nodes within a site.
- **Multi-Site Failover**: Handle site-wide failures with a global failover mechanism.
- **DNS Integration**: Optional DNS failover for traffic redirection between sites.
- **Scalable Design**: Supports Docker Swarm nodes and other backend architectures.
- **Customizable**: Configurations are modular and can be adapted to your environment.

---

## Repository Structure

```plaintext
.
├── site_a/
│   ├── haproxy.cfg         # HAProxy configuration for Site A
│   ├── keepalived.conf     # Keepalived configuration for Site A
├── site_b/
│   ├── haproxy.cfg         # HAProxy configuration for Site B
│   ├── keepalived.conf     # Keepalived configuration for Site B
├── global_failover/
│   ├── dns_failover.md     # Guide for setting up DNS-based failover
│   ├── global_vip_guide.md # Instructions for configuring a global VIP
└── README.md               # This README file

## Requirements

### Software

    HAProxy (v2.0 or higher)
    Keepalived (v2.0 or higher)
    Optional: DNS provider with failover support (e.g., Amazon Route 53, Cloudflare)

### Hardware

    At least two HAProxy nodes per site.
    Reliable network connection between sites for global failover.

### Installation and Configuration
1. HAProxy Setup

    Install HAProxy:

sudo apt-get install haproxy

Copy the provided haproxy.cfg file from the respective site directory (site_a/ or site_b/) to /etc/haproxy/haproxy.cfg.
Restart HAProxy:

    sudo systemctl restart haproxy

2. Keepalived Setup

    Install Keepalived:

sudo apt-get install keepalived

Copy the keepalived.conf file from the respective site directory to /etc/keepalived/keepalived.conf.
Restart Keepalived:

    sudo systemctl restart keepalived

3. Global Failover

    For DNS-based failover, follow the instructions in global_failover/dns_failover.md.
    For global VIP configuration, refer to global_failover/global_vip_guide.md.

## Testing the Configuration

    Local Failover: Shut down one HAProxy node in a site and verify that traffic is redirected to the backup node using the local VIP.
    Site-Wide Failover: Simulate a complete site failure by shutting down all nodes in one site. Confirm traffic is redirected to the second site via DNS or global VIP.

### Contributing

We welcome contributions to improve these configurations. To contribute:

    Fork this repository.
    Create a feature branch.
    Submit a pull request with detailed explanations of changes.

## License

This project is licensed under the MIT License. See the LICENSE file for details.
Resources

    [HAProxy Documentation](https://docs.haproxy.org)
    [Keepalived Documentation](https://keepalived.readthedocs.io/en/latest/)
