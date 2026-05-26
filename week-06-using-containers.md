# Week 06: Cloud Infrastructure & Embedded Systems – Using Containers

## Overview

This lab focused on deploying and managing Windows Server containers using
Docker and PowerShell on Windows Server 2019. Working as a security team
member at Structureality Inc., the objective was to build container images,
create and manage container instances, inspect user and group configurations
within containers, and evaluate the security implications of containerized
environments.

This exercise reinforced Security+ concepts related to architecture security
models, containerization as an isolation strategy, and secure configuration
of deployed environments.

## Environment

- Windows Server 2019 (MS10 virtual machine)
- Docker (pre-installed)
- Windows PowerShell (Administrator)
- Windows NanoServer container image (mcr.microsoft.com/windows/nanoserver:1809)

## CompTIA Security+ Objectives Covered

- 3.1 – Compare and contrast security implications of different architecture models
- 3.2 – Given a scenario, apply security principles to secure enterprise infrastructure

---

## Methodology

### Part 1 – Confirming Docker Installation and Host Environment

Verified that Docker was installed and running prior to container operations.

- Confirmed Docker Engine service was active via the Windows Services console
- Ran `ipconfig` to identify the host VM's network configuration
- Ran `winver` to confirm Windows Server 2019 version 1809
- Confirmed hostname using the `hostname` command

### Part 2 – Managing Docker Containers with PowerShell

Created, started, and interacted with containers using core Docker CLI commands.

- Listed available local images using `docker images`
- Created a non-running container with `docker create --name MyFirstContainer`
- Inspected container state using `docker ps -a`
- Started the container with `docker start` and observed it exit due to no
  active process or interactive session
- Created a second interactive container using `docker run -it --name TestContainer`
- Ran `ver`, `ipconfig`, `hostname`, and `netstat -aon` from within the
  container to confirm it has its own OS version, hostname, IP address, and
  port state independent from the host
- Verified network reachability by pinging both the container's default
  gateway and the host VM — both succeeded via Docker's default NAT network
- Exited the container session cleanly using `exit`

### Part 3 – Viewing Users and Groups in the Container

Inspected the default security configuration inside the container and performed
user management operations.

- Re-entered the container using `docker exec -it TestContainer cmd`
- Confirmed the default login username was `ContainerUser` using
  `echo %username%`
- Listed local users with `net user` and inspected the Administrator account
  with `net user Administrator`
- Confirmed the local Administrator account was enabled/active
- Listed local groups with `net localgroup` — confirmed no Docker
  Administrators group exists by default
- Re-entered the container as `ContainerAdministrator` using
  `docker exec -it --user ContainerAdministrator TestContainer cmd`
- Created a new user account (`testuser`) using `net user`
- Added `testuser` to the Power Users group using `net localgroup`
- Changed the Administrator password using `net user Administrator /passwordchg:yes`

### Part 4 – Creating and Managing Data in the Container

Tested container file system persistence behavior.

- Navigated the container directory structure using `dir`
- Created a new folder (`MyFolder`) using `md`
- Created a text file (`test.txt`) inside the folder using `echo` redirection
- Verified file contents using `type`
- Exited and re-entered the container to confirm that `MyFolder` persisted —
  the container file system is preserved across sessions unless the container
  is explicitly removed with `docker rm`
- Stopped the container cleanly using `docker stop TestContainer`

---

## Key Findings

### Containers Provide Isolated Execution Environments

Each container has its own hostname, IP address, OS version, user accounts,
groups, and file system — completely independent from the host and from other
containers. This isolation limits the blast radius of a compromise and supports
zero-trust principles in multi-service deployments.

### Default Container Attack Surface is Minimal

TCP port 5985 (WinRM) was not in a listening state inside the container,
demonstrating that Windows containers do not inherit the full service exposure
of a standard Windows Server installation. This reduced default surface is a
security advantage of containerized deployment.

### Least Privilege Applies Inside Containers

The default login account (`ContainerUser`) has limited privileges. Privileged
operations such as user creation and group modification require re-entering the
container as `ContainerAdministrator`. This separation mirrors least privilege
principles that apply in any enterprise environment.

### File System Persists Across Sessions, Not Deletions

Stopping a container preserves its file system. Data is only destroyed when the
container is explicitly removed with `docker rm`. This distinction matters for
both operational continuity and incident response — a stopped container is not
a cleaned container.

### Tagging Prevents Unintended Image Downloads

Appending `:1809` to the image name forces Docker to use the locally cached
image rather than attempting to pull the latest version from the internet. In
restricted or air-gapped environments, omitting the tag can cause failures or
unintended network egress.

---

## Skills Demonstrated

- Docker image and container lifecycle management
- Container session entry and interactive command execution
- User and group auditing inside a containerized OS
- User account creation and group membership management within a container
- File system navigation and persistence behavior evaluation
- Container network inspection and host reachability testing
- Application of least privilege in containerized environments
- Secure container configuration practices prior to production deployment

## Tools Used

- `docker images` — List locally available images
- `docker create` — Create a non-running container
- `docker run -it` — Create and start an interactive container session
- `docker start` / `docker stop` — Start and stop container instances
- `docker ps -a` — List all containers and their current state
- `docker exec -it` — Enter a running container session
- `net user` / `net localgroup` — User and group management within the container
- `netstat -aon` — Port state inspection within the container
- Windows PowerShell (Administrator)

---

## Lab Result

**Score: 12/12 — Passed**
Duration: 26 minutes, 6 seconds

---

## Supporting Documentation

- [Lab 11 – Assisted: Using Containers](https://github.com/user-attachments/files/28275990/Lab06P1.-.Using.Containers.pdf)
