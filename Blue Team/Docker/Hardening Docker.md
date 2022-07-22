# Hardening Docker

Docker by default is pretty secure, but bare in mind that most vulnerabilities originate from mistakes done by the user, such as running a container using the `--privileged` flag. Here are some methods of securing your containers.

## A secure Host Operating System

Since the containers are on the same operating system as the host, the containers are susceptible to vulnerabilities that the OS kernel has. A simple example would be that if the host OS is vulnerable to CVE-2016-5195 (Dirty Cow) for example, an exploit for dirty cow would enable the container user to gain root.

## Update the host OS and Docker

Docker also has vulnerabilites that are OS-independant, which means that even if you update your OS to be up-to-date, not updating docker can also leave you with security vulnerabilities which can compromise the host machine. Here is a [list of vulnerabilities](https://www.cvedetails.com/product/28125/Docker-Docker.html?vendor_id=13534) that were found in docker.


## Disable root access inside the container

Disabling the root user inside the container further secures our container and our host. To do this, we use the following command in our `Dockerfile`:

```
RUN chsh -s /usr/sbin/nologin root
```

Bare in mind that this only prevents logging in as root from a shell; it does not prevent a root process from creating a malicious process, like a reverse shell. It is important to run services or servers as a user other than root.

## Disable setuid binaries in containers

This further secures the container from being privilege escalated to root. We use the flag `--security-opt no-new-privileges` when running a container. This option, even if there is a setuid binary on the container, will not escalate the user running the setuid binary to root.

## Disable all unnecessary capabilities

This can be done by using the `--cap-dropall` flag. We can add the capabilities that the docker container has by appending `--cap-add <CAPABILTY>` flags. It is also important to not use `--privileged` flag, since this mode bypasses all capability checks and most vulnerabilities, such as **mounting the host disk** to the container and gaining access to the whole filesystem.

## Set container filesystem to read-only mode

This prevents the attacker from modifying the content inside the container filesystem. This is useful when you dont want a user or service to modify files that should not have access to. 

## Setup a network that prevents inter-container communication

Containers that are created are setup inside the same network bridge by default, which is in `172.17.0.0/16` range. The default network allows inter-container communication, which can be seen by using the `docker network inspect bridge`. This default setting can increase the attack surface to oher containers, which can potentially make a lot of damage. To prevent inter-container communication, we can disable the `com.docker.network.bridge.enable_icc` option or create another network bridge with this option disabled if we don't want to modify the default bridge. To create a network bridge with no icc, use this command: `docker network create --driver bridge -o "com.docker.network.bridge.enable_icc"="false" <name_of_network>`.

## Docker bench security

This [github script]() automatically lists the security measures that are needed or are in place.

## Resources

- [Hackersploit -  Docker Security Essentials | How To Secure Docker Containers](https://www.youtube.com/watch?v=KINjI1tlo2w)