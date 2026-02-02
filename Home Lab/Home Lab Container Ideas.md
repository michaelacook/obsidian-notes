# Recommended Proxmox LXC container services

Below is a concise set of notes summarising the LXC container services highlighted in **BarMine Tech’s “You need these LXC containers on Proxmox”** (and echoed in the XDA article on indispensable Proxmox LXCs).  Each section explains what the service does, why you might want it in your Proxmox homelab and includes a link to the official project page or repository.

## Vaultwarden – self‑hosted password manager

* **What it is:** Vaultwarden is a Rust‑based server that implements the Bitwarden client API.  It offers almost all of Bitwarden’s features (personal vault, organisations, attachments, collections and multi‑factor authentication) but is much lighter weight and suitable for self‑hosting
* **Why use it:** Managing complex passwords for multiple services becomes painless; Vaultwarden provides most Bitwarden features without the heavy resource requirements.  The helper‑scripts for Proxmox automatically handle certificate generation, making it straightforward to deploy over HTTPS.  Self‑hosting means credentials stay on your server.
* **Project link:** <https://github.com/dani-garcia/vaultwarden>

## Pi‑hole – network‑wide ad blocker

* **What it is:** Pi‑hole is a DNS server that blocks adverts at the network level.  Once installed, all devices on your network use it as their DNS, preventing ads from being fetched.  It offers an intuitive web interface for statistics and management and includes a built‑in DHCP server and white/black‑list controls:contentReference[oaicite:3]{index=3}.
* **Why use it:** Running Pi‑hole in an LXC container gives your entire home network ad‑blocking (including mobile apps and smart TVs) without browser plugins.  Because ads are blocked before download, network performance can improve:contentReference[oaicite:4]{index=4}.
* **Project link:** <https://pi-hole.net>

## Nginx Proxy Manager – simple reverse‑proxy with SSL

* **What it is:** Nginx Proxy Manager (NPM) is a web‑UI front‑end to Nginx.  It lets you expose services on your network with free Let’s Encrypt SSL certificates, automatic renewal and user‑friendly configuration:contentReference[oaicite:5]{index=5}.  The interface is built on Tabler and can manage multiple users:contentReference[oaicite:6]{index=6}.
* **Why use it:** Running NPM in LXC provides a secure reverse‑proxy for all your homelab services, so you can access them via friendly domain names and encrypted connections without manually editing Nginx configs.
* **Project link:** <https://nginxproxymanager.com>

## Portainer – graphical Docker/Podman/Kubernetes manager

* **What it is:** Portainer offers a graphical management platform for Docker, Podman and Kubernetes.  The Portainer team highlights that it delivers enterprise‑grade container management without complexity and that it works across cloud, on‑prem and edge environments:contentReference[oaicite:7]{index=7}:contentReference[oaicite:8]{index=8}.
* **Why use it:** If you use containers inside your LXCs, Portainer provides an easy way to deploy, configure and monitor containers or Kubernetes clusters.  It’s particularly useful for people who prefer a GUI instead of the command line.
* **Project link:** <https://www.portainer.io>

## Tailscale – effortless secure networking

* **What it is:** Tailscale sets up a zero‑config, WireGuard‑based VPN that connects devices directly without opening firewall ports.  It advertises “fast, seamless device connectivity” and “point‑to‑point network connectivity that enforces least privilege”:contentReference[oaicite:9]{index=9}.  It deploys in minutes and eliminates single points of failure:contentReference[oaicite:10]{index=10}.
* **Why use it:** A Tailscale LXC container creates a secure mesh network among your devices.  You can access services (like Pi‑hole or Vaultwarden) when away from home without exposing ports to the internet, and network configuration is minimal.
* **Project link:** <https://tailscale.com>

## Homepage – customisable dashboard

* **What it is:** Homepage is a modern, fully static and secure application dashboard.  The project’s README describes it as a “highly customizable application dashboard” with integrations for over a hundred services, YAML‑based configuration, Docker label discovery and widgets for weather, search and system statistics:contentReference[oaicite:11]{index=11}.
* **Why use it:** It provides a central landing page that displays links to all your self‑hosted services, container status and useful widgets.  Running it in LXC keeps your Proxmox environment organised and provides a single place to view service health.
* **Project link:** <https://gethomepage.dev>

## OctoPrint – remote 3D‑printer control

* **What it is:** OctoPrint is an open‑source web interface for 3D printers.  It allows full remote control of print jobs and the printer’s temperature, provides real‑time webcam feeds and progress feedback, and offers a plug‑in system to extend functionality:contentReference[oaicite:12]{index=12}:contentReference[oaicite:13]{index=13}.  It’s released under the AGPL and has a vibrant plugin ecosystem:contentReference[oaicite:14]{index=14}.
* **Why use it:** Running OctoPrint in a container lets you monitor and manage 3D‑printing jobs without being tethered to the printer.  It can stream video, provide G‑code visualisation and even create timelapse recordings.  Plugins such as bed‑level visualisers and firmware updaters enhance the experience:contentReference[oaicite:15]{index=15}.
* **Project link:** <https://octoprint.org>

## Beszel – lightweight server monitoring

* **What it is:** Beszel is a free, open‑source monitoring tool for servers and containers.  The official site describes it as “simple, lightweight server monitoring” that tracks CPU, memory and network usage history, provides configurable alerts, multi‑user support, OAuth/OIDC authentication and automatic backups:contentReference[oaicite:16]{index=16}.  An XDA feature explains that it sits between Uptime Kuma and the Grafana/Prometheus stack; it records performance statistics and draws graphs while being easy to deploy:contentReference[oaicite:17]{index=17}.
* **Why use it:** Beszel offers a balance between ease of setup and insight.  It gathers detailed statistics from your Proxmox hosts and other servers, sends alerts on threshold breaches and can integrate with Discord or email notifications.  Running the central hub in LXC keeps monitoring separate from the hosts being monitored.
* **Project link:** <https://beszel.dev>

## NetBox – network “source of truth”

* **What it is:** NetBox is a network documentation platform that combines IP address management and datacenter infrastructure management.  The documentation calls it “the premier network source of truth” and notes that it provides object types for devices, racks, cabling, VMs, IP ranges, VLANs and more:contentReference[oaicite:18]{index=18}.  It also offers custom fields, templates, event rules and plugins, plus REST & GraphQL APIs:contentReference[oaicite:19]{index=19} and is fully open source:contentReference[oaicite:20]{index=20}.
* **Why use it:** For complex homelab networks, NetBox lets you record hardware inventory, IP allocations, connections and virtual machines in one place.  Its detailed data model and APIs make it ideal for network automation and documentation:contentReference[oaicite:21]{index=21}.
* **Project link:** <https://netboxlabs.com/docs/netbox>

## TriliumNext Notes – hierarchical note‑taking

* **What it is:** TriliumNext (a community continuation of Trilium) is a free, open‑source note‑taking application.  It focuses on building large knowledge bases with hierarchical note trees.  The README lists features such as a rich WYSIWYG editor supporting tables, images, math and code blocks, full‑text search, note versioning, attributes for organisation, note encryption, relation/mind/link maps, diagrams via Excalidraw and built‑in syncing:contentReference[oaicite:22]{index=22}.
* **Why use it:** TriliumNext provides a powerful, self‑hosted alternative to Evernote or Notion.  Its tree structure and features like Mermaid diagrams and mind maps make it ideal for documenting homelab setups or software development.  The LXC container can sync with desktop clients and serve a web UI.
* **Project link:** <https://github.com/TriliumNext/Trilium>

## YunoHost – all‑in‑one app host

* **What it is:** YunoHost turns a server into a self‑hosting platform with an app catalogue.  The project site explains that YunoHost installs on a server and lets you install digital services (cloud storage, mail, wikis, social networks etc.) with very little technical knowledge:contentReference[oaicite:23]{index=23}.  It offers a web administration panel, user portal and automatic setup of domains and certificates.:contentReference[oaicite:24]{index=24}
* **Why use it:** YunoHost is ideal for beginners who want to self‑host without managing Docker or command‑line tools.  It handles user management, domain names and updates, making it simple to deploy numerous apps quickly.
* **Project link:** <https://yunohost.org>

## Runtipi – personal homeserver orchestrator

* **What it is:** Runtipi is an open‑source personal homeserver orchestrator built on Docker.  Its introduction notes that Runtipi provides a user‑friendly web interface to manage and run multiple services on a single server without worrying about manual configuration or networking:contentReference[oaicite:25]{index=25}.  It includes an app store and aims to democratise self‑hosting:contentReference[oaicite:26]{index=26}.
* **Why use it:** Runtipi simplifies the deployment of Docker applications via a one‑click app store, making it a good starting point for people intimidated by container orchestration.  It integrates reverse proxy and domain configuration and is easier than raw Docker Compose.
* **Project link:** <https://runtipi.io>

## Cosmos – secure all‑in‑one self‑hosting platform

* **What it is:** Cosmos combines a reverse proxy, container manager and app store into one platform.  An article by its developer explains that Cosmos includes a full reverse proxy with Let’s Encrypt, a Docker container manager, a community‑maintained app‑store, security tools (anti‑bot/DDOS) and an identity provider with OpenID support:contentReference[oaicite:27]{index=27}.  It aims to provide a seamless experience where the reverse proxy, authentication and apps work together:contentReference[oaicite:28]{index=28}.
* **Why use it:** Cosmos suits homelab users who want an integrated solution with strong security.  It simplifies installation of popular apps, secures them behind SSO and 2FA, offers automatic updates and integrates backup and monitoring.  Because it’s home‑focused, you get enterprise‑style security without enterprise complexity.
* **Project link:** <https://cosmos-cloud.io>

## CasaOS – personal cloud OS

* **What it is:** CasaOS is an open‑source personal cloud system.  The project’s README highlights a friendly, form‑free UI, support for various hardware (x86, ARM, Raspberry Pi), an app store with one‑click installation of popular services (Nextcloud, Jellyfin, *arr apps), the ability to install any Docker app, an elegant file manager and dashboard widgets:contentReference[oaicite:29]{index=29}.  It aims to make personal cloud servers accessible and reduces reliance on SaaS.:contentReference[oaicite:30]{index=30}
* **Why use it:** CasaOS turns a small PC or single‑board computer into an easy‑to‑use home cloud.  It’s great for users who want a NAS‑like interface with quick app installation, file management and system widgets without learning Docker commands.
* **Project link:** <https://www.casaos.io>

## Other containers mentioned

The video and article also mention other self‑hosted services worth considering:

- **Home Assistant:** open‑source home automation platform that integrates with thousands of devices and lets you automate and control your smart home locally:contentReference[oaicite:31]{index=31}.  It keeps data local and offers dashboards, voice assistants and add‑ons:contentReference[oaicite:32]{index=32}.
- **Paperless‑ngx, Jellyfin, Immich, Calibre‑Web, Nextcloud, Frigate:** additional LXC containers suggested for homelab use; these cover document archiving, media streaming, photo management, ebook servers, cloud storage and camera monitoring.

By deploying these containers on Proxmox using community helper scripts you can build a powerful and secure self‑hosted environment tailored to your needs.
