# Docker Networking — Interview-Oriented Theory Notes

---

# 1. Why Networking Exists in Docker

Containers are isolated processes.  
By default, they cannot communicate with:

- Other containers  
- The host system  
- External networks  

Docker networking provides controlled communication channels between:

- Container ↔ Container  
- Container ↔ Host  
- Container ↔ Internet  

---

# 2. Default Docker Networks

List networks:

```
docker network ls
```

Typical default networks:

- bridge  
- host  
- none  

Each network is implemented via a **network driver**.

---

# 3. Network Drivers Overview

| Driver | Scope | Primary Use |
|-------|------|-------------|
bridge | Single host | Container-to-container communication |
host | Single host | Direct host networking |
overlay | Multi-host | Distributed systems / clusters |
macvlan | External LAN | Assign container real IP |
none | Isolated | No networking |

---

# 4. Bridge Networking

## Definition

Bridge is the **default Docker network**.

It creates a private internal network on the host where containers can communicate.

Characteristics:

- Containers get private IPs  
- Can communicate with each other  
- Can reach internet via NAT  
- Host can communicate with containers (via port mapping)

---

## Default Bridge

When running a container without specifying a network:

```
docker run <image>
```

It attaches to the default bridge network.

---

## Custom Bridge Network

Recommended in real systems for isolation and service discovery.

Create:

```
docker network create -d bridge my_bridge
```

List networks:

```
docker network ls
```

---

## Run Container on Custom Network

```
docker run -d --net=my_bridge --name db postgres
```

Now the container belongs to `my_bridge`.

Containers on different bridge networks **cannot communicate** unless connected.

---

## Connect Existing Container to Network

```
docker network connect my_bridge web
```

This attaches container `web` to `my_bridge`.

After this:
Containers on the same network can communicate via container name (DNS-based resolution).

---

## Interview Insight

Default bridge vs custom bridge:

Default bridge:
- No automatic DNS resolution between containers  
- Legacy behavior  

Custom bridge:
- Built-in DNS  
- Better isolation  
- Recommended for production  

---

# 5. Host Networking

## Definition

Host mode removes network isolation.

Container shares the host’s network stack.

No separate IP.  
No NAT.  
No port mapping needed.

---

## Run Container with Host Network

```
docker run --network="host" <image>
```

Effects:

- Container uses host IP
- Uses host ports directly
- Highest performance
- Lowest isolation

---

## Advantages

- No port mapping overhead
- Lower latency
- Useful for high-performance networking apps

---

## Risks

- Reduced security isolation
- Port conflicts possible
- Container has full network access

Use cautiously in production.

---

# 6. Overlay Networking

## Definition

Overlay network connects containers across multiple Docker hosts.

Used in:

- Docker Swarm  
- Kubernetes  
- Distributed microservices  

Creates a virtual network spanning multiple machines.

Containers on different hosts behave as if on same network.

---

## Key Properties

- Multi-host communication  
- Encrypted traffic (optional)  
- Service discovery across nodes  

Primary use case:
Clustered container environments.

---

# 7. Macvlan Networking

## Definition

Macvlan assigns a container its own MAC address and IP from the physical network.

Container appears as a separate device on the LAN.

---

## Characteristics

- Container gets real network identity
- Accessible directly from LAN
- Bypasses Docker NAT

Use cases:

- Legacy applications requiring direct network presence
- Network appliances
- Monitoring tools

---

# 8. None Network

```
docker run --network none <image>
```

Container has:

- No internet
- No external communication
- Full isolation

Used for:

- Security-sensitive workloads
- Testing isolation

---

# 9. Container Communication Model

Same network → communicate via container name  
Different networks → isolated  
Host network → shared stack  

DNS resolution works automatically inside user-defined networks.

---

# 10. Key Interview Questions

## Q1: How do containers communicate?

Via Docker networks.

They must be attached to the same network.

---

## Q2: What is default network driver?

Bridge.

---

## Q3: Difference between bridge and host?

Bridge:
- Private network
- NAT
- Port mapping required

Host:
- Shares host network
- No isolation
- No port mapping

---

## Q4: When to use overlay?

When containers run on multiple hosts and must communicate.

---

## Q5: When to use macvlan?

When container must appear as physical device on network.

---

## Q6: Can containers on different networks communicate?

No, unless manually connected to same network.

---

# 11. Best Practices

- Use custom bridge networks for applications  
- Avoid default bridge in production  
- Use overlay for clusters  
- Use host mode only when necessary  
- Use volumes with networked services (databases, etc.)  

---

# 12. Core Mental Model

Container network = Virtual network namespace  

Bridge → private LAN on host  
Host → share host network  
Overlay → multi-host virtual LAN  
Macvlan → real LAN identity  

---

# End of Notes
