---
title: Software Basics | Notes from Videos
description: "Short explainers from my video notes: APIs versus SDKs, virtual machines versus containers and where Kubernetes fits, Git versus GitHub and the pull request workflow, what Windows keeps in AppData's Roaming, Local and LocalLow folders, and how IPv4 addressing works, from address classes and subnet masks to private addresses, NAT and IPv6. Sources are listed at the end."
tags:
  - devlog
date: 2022-04-18
lastMod: 2026-10-06T00:00:00+08:00
---

Pairs of terms that are easy to mix up, and a few things worth knowing about Windows and IP
addresses, condensed from my notes on explainer videos. Sources are collected at the bottom.

## APIs and SDKs

A mobile app that needs a cloud service, for example to recognise what is in a photo, talks to it
through an API and usually calls that API through an SDK.[^api-sdk]

### API: Application Programming Interface

An API is a set of definitions and protocols that lets one app or service talk to another, a
bridge between the app and the service. It does three things:

- **Communicates** between apps and services.
- **Abstracts** the complicated logic away, so the caller only asks for the data it needs.
- **Follows a standard**, such as SOAP, GraphQL or REST (Representational State Transfer).

A REST call has two halves:

- **Request**, from the app to the service:
  - **Operation**: an HTTP method such as `GET`, `POST`, `PUT` or `DELETE`.
  - **Endpoint**: the URL of the service, such as `…/analyze`.
  - **Parameters**, optionally, such as a file name.
- **Response**: raw data back from the service, typically JSON.

### SDK: Software Development Kit

Building every request by hand and parsing raw JSON is tedious. An SDK is a toolbox of code, one
per language (Java, Node, Python and so on), that makes the API calls for you. In a Java app, one
method call sends the request:

```java
AnalyzeResponse response = visualRecognition.analyze("cat.jpg").getResult();
```

and the answer comes back as a native Java object instead of raw JSON:

```java
label.setText(response.name);
```

## Virtual machines and containers

The two differ in where the virtualisation happens:[^vm-containers]

|                    | Virtual machine                                                                                                   | Container                                                                                                                                              |
| ------------------ | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Virtualises**    | The hardware. A hypervisor creates virtual CPUs, memory, storage and network cards.                               | The operating system. Containers share the host's kernel.                                                                                              |
| **Isolates**       | Whole machines, each fairly independent of the others.                                                            | Processes. Each container sees only what it needs to run: its libraries, scripts and code.                                                             |
| **Uses resources** | Through what looks like hardware, mapped to the real hardware by the hypervisor.                                  | Through two kernel features: **namespaces**, which give each container its own view of the system, and **cgroups**, which meter and limit its resources. |
| **Strength**       | Flexibility: any hardware setup.                                                                                  | Portability: a container is defined in a single file, the Dockerfile.                                                                                  |

A type 1 hypervisor runs directly on the hardware; a type 2 hypervisor, such as VirtualBox or
Parallels, runs on top of an operating system. The two technologies combine well: lightweight
VMs for flexibility, containers inside them for portability. KubeVirt runs VMs and Kubernetes or
OpenShift runs containers on the same platform.

Docker and Kubernetes are not alternatives either: Kubernetes is what you add to a Docker
workflow when it has to scale.[^k8s]

## Git and GitHub

**Git** is the version control system that runs on your machine. **GitHub** and **GitLab** host
Git repositories in the cloud and add a web interface.[^git]

Together they give you:

- **History.** Git tracks every change and keeps snapshots you can go back to.
- **Team development.** Several people work on the same code through branches.
- **Automation.** Hosted repositories plug into DevOps pipelines for automated tests, builds and
  deployments.

The everyday workflow:

1. **Clone** the repository.
2. **Branch** off `main` and change the working copy.
3. **Commit** the changes to the branch, then **push** it to the hosted repository.
4. Meanwhile, someone else has merged into `main`. **Pull** and merge their changes, and resolve
   any **merge conflict**.
5. Open a **pull request** so the team can review, approve and merge the branch into `main`.

## Windows AppData

Installing a program puts the files it needs to run into **Program Files**. Everything it creates
or uses later, such as settings, temporary files and caches, goes into **AppData**, inside the
user's folder (`C:\Users\<name>\AppData`).[^appdata] Keeping the two apart means:

- One installed program can serve several users, each with their own settings.
- Users cannot read each other's settings.
- Program Files needs administrator rights to change; a user's own folder does not.

| Folder          | Shortcut          | Holds                                                                                                                               |
| --------------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **Roaming**     | `%AppData%`       | Data that follows the user across the computers of a Windows domain, such as a company or school network where any account works on any machine. |
| **Local**       | `%LocalAppData%`  | Data that stays on this computer.                                                                                                   |
| **LocalLow**    |                   | The same as Local, for programs running at a low integrity level.                                                                   |
| **ProgramData** |                   | At the top of the drive, not per user: resources a program shares between all users.                                                |

Integrity levels describe how far Windows trusts a program: **Low** can reach very few files and
folders, **Medium** has normal user rights, **High** has administrator rights and **System** can do
everything. There are also **Untrusted** and **Installer** levels. Not every program follows these
conventions: some keep their settings in **Documents** instead.

## IP addresses

### Address classes

IPv4 has \(2^{32}\), about 4.3 billion, addresses, and the internet has run on IPv4 since
1 January 1983. Addresses were first handed out in classes, each with a default subnet mask that
splits an address into a network part and a host part:[^ip-classes]

| Class | First octet | Default mask    | Networks  | Hosts per network |
| ----- | ----------- | --------------- | --------- | ----------------- |
| A     | 1–126       | `255.0.0.0`     | 126       | 16,777,214        |
| B     | 128–191     | `255.255.0.0`   | 16,384    | 65,534            |
| C     | 192–223     | `255.255.255.0` | 2,097,152 | 254               |
| D     | 224–239     | none            | multicast |                   |
| E     | 240–255     | none            | reserved for experimental use |   |

A class A network is huge: IBM, for example, holds `9.0.0.0`. IANA, the Internet Assigned Numbers
Authority, oversees the allocation.

A network that uses its class's default mask is **classful**. Using a longer mask slices it into
smaller **classless** networks, such as `9.1.9.0` with the mask `255.255.255.0`.

`127.0.0.0` to `127.255.255.255` is reserved for **loopback**: `ping 127.0.0.1` checks that your
machine can talk to itself, and every computer has about 16 million such addresses for that.

### Private addresses and NAT

The addresses ran out long ago. Two things from RFC 1918 kept IPv4 going:[^private]

- **Private addresses.** These ranges are not routed on the internet and can be reused by every
  network, so they are not unique:

  | Class | Range                           | Prefix           |
  | ----- | ------------------------------- | ---------------- |
  | A     | `10.0.0.0`–`10.255.255.255`     | `10.0.0.0/8`     |
  | B     | `172.16.0.0`–`172.31.255.255`   | `172.16.0.0/12`  |
  | C     | `192.168.0.0`–`192.168.255.255` | `192.168.0.0/16` |

- **Network Address Translation (NAT).** Your ISP gives your router one public address. NAT
  translates traffic between the private addresses at home and that public address, so every
  device on the network shares one public identity.

IPv6 removes the shortage with \(2^{128}\) addresses, and mobile phones already use public IPv6
addresses.

[^api-sdk]: [IBM Technology: API vs. SDK: What's the difference?](https://youtu.be/kG-fLp9BTRo)
[^vm-containers]: [IBM Technology: Containers vs VMs: What's the difference?](https://youtu.be/cjXI-yxqGTI)
[^k8s]: [IBM Technology: Kubernetes vs. Docker: It's Not an Either/Or Question](https://youtu.be/2vMEQ5zs1ko)
[^git]: [IBM Technology: Git vs. GitHub: What's the difference?](https://youtu.be/wpISo9TNjfU)
[^appdata]: [ThioJoe: What Are the Different Windows AppData Folders for?](https://youtu.be/3XjSTG-oIMw)
[^ip-classes]: [NetworkChuck: we ran OUT of IP Addresses!!](https://youtu.be/tcae4TSSMo8)
[^private]: [NetworkChuck: we're out of IP Addresses….but this saved us (Private IP Addresses)](https://youtu.be/8bhvn9tQk8o)
