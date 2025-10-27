# ⚙️ Podman Technology Stack and Dependencies

## Overview

This document provides a comprehensive analysis of Podman's technology stack, including its
architecture, core dependencies, system requirements, and platform compatibility. Understanding
these components is essential for proper deployment and optimization of Podman in various
environments.

## 🏗️ Podman Architecture Overview

Podman follows a modular, daemonless architecture that sets it apart from traditional container runtimes:

### Core Architecture Principles

- **Daemonless Design**: No central daemon process required, reducing attack surface and complexity
- **Fork-Exec Model**: Direct process execution without intermediary daemon
- **User Namespace Support**: Native rootless container capabilities
- **OCI Compliance**: Full adherence to Open Container Initiative standards
- **Pod-based Organization**: Kubernetes-compatible pod concepts

### Component Hierarchy

```text
┌─────────────────────────────────────────┐
│           Podman CLI/API                │
├─────────────────────────────────────────┤
│              libpod                     │
├─────────────────────────────────────────┤
│        Runtime Components               │
│  ┌─────────────┬─────────────────────┐  │
│  │   conmon    │    runc/crun        │  │
│  └─────────────┴─────────────────────┘  │
├─────────────────────────────────────────┤
│           Kernel Features               │
│     (namespaces, cgroups, seccomp)     │
└─────────────────────────────────────────┘
```

## 🔧 Core Dependencies and Libraries

### libpod - The Core Library

**Purpose**: Primary library implementing Podman's container and pod management functionality

**Key Features**:

- Container lifecycle management
- Image handling and storage
- Pod orchestration
- Network management
- Volume management
- Registry interaction

**Dependencies**:

- containers/common
- containers/image
- containers/storage
- runtime-spec (OCI)

### conmon - Container Monitor

**Purpose**: Lightweight monitoring binary for OCI runtimes

**Responsibilities**:

- Container stdio handling
- Exit code collection
- Signal forwarding
- Log collection and forwarding
- TTY handling

**Implementation**: Written in C for minimal overhead

### OCI Runtime (runc/crun)

#### runc

- **Language**: Go
- **Origin**: Docker/containerd project
- **Features**: Standard OCI runtime implementation
- **Use Case**: Default runtime for most installations

#### crun

- **Language**: C
- **Origin**: Red Hat/containers project
- **Features**:
  - Faster startup times
  - Lower memory footprint
  - Better performance for short-lived containers
- **Use Case**: Optimized for Red Hat environments

### Storage Dependencies

#### containers/storage

- **Purpose**: Container and image storage management
- **Backends Supported**:
  - overlay2 (preferred)
  - fuse-overlayfs (rootless fallback)
  - vfs (compatibility)
  - zfs (experimental)
  - btrfs (experimental)

#### Image Management

- **containers/image**: Image format handling and registry interaction
- **containers/common**: Shared configuration and utilities

## 🖥️ Operating System Requirements

### Linux Distribution Compatibility

| Distribution | Minimum Version | Recommended Version | Notes                               |
| ------------ | --------------- | ------------------- | ----------------------------------- |
| RHEL/CentOS  | 8.0             | 9.0+                | Native support, optimal performance |
| Fedora       | 30              | 35+                 | Latest features, frequent updates   |
| Ubuntu       | 18.04 LTS       | 20.04 LTS+          | Official packages available         |
| Debian       | 10 (Buster)     | 11 (Bullseye)+      | Stable support                      |
| openSUSE     | 15.1            | 15.3+               | Good integration                    |
| Arch Linux   | Rolling         | Current             | Cutting-edge packages               |

### Kernel Requirements

#### Minimum Kernel Version

- **Linux Kernel**: 3.10+ (minimum)
- **Recommended**: 4.18+ for full feature support
- **Optimal**: 5.4+ for latest security and performance features

#### Required Kernel Features

```bash
# Essential features that must be enabled
CONFIG_NAMESPACES=y
CONFIG_NET_NS=y
CONFIG_PID_NS=y
CONFIG_IPC_NS=y
CONFIG_UTS_NS=y
CONFIG_CGROUPS=y
CONFIG_CGROUP_CPUACCT=y
CONFIG_CGROUP_DEVICE=y
CONFIG_CGROUP_FREEZER=y
CONFIG_CGROUP_SCHED=y
CONFIG_CPUSETS=y
CONFIG_MEMCG=y
CONFIG_SECCOMP=y
CONFIG_OVERLAY_FS=y
```

#### Optional but Recommended Features

```bash
CONFIG_USER_NS=y          # Rootless containers
CONFIG_CGROUP_PIDS=y      # PID limits
CONFIG_BLK_CGROUP=y       # Block device limits
CONFIG_CGROUP_NET_PRIO=y  # Network priority
CONFIG_NETFILTER_XT_MATCH_OWNER=y  # Network filtering
```

## 🔧 Hardware Prerequisites and Architecture Support

### Intel/AMD x86_64 Architecture

#### Specifications

- **Minimum RAM**: 1 GB
- **Recommended RAM**: 4 GB+
- **Storage**: 20 GB+ available space
- **CPU**: Any 64-bit x86 processor

#### Performance Characteristics

- **Container Startup**: 50-200ms typical
- **Memory Overhead**: ~10-20MB per container
- **Storage Efficiency**: Copy-on-write with overlay2
- **Network Performance**: Native bridge/host networking

#### Optimization Features

- **Intel VT-x/AMD-V**: Enhanced virtualization support
- **Intel VT-d/AMD IOMMU**: Device passthrough capabilities
- **Hardware Security**: Intel TXT, AMD SVM

### ARM64/AArch64 Architecture

#### ARM64 Specifications

- **Minimum RAM**: 1 GB
- **Recommended RAM**: 2 GB+
- **Storage**: 20 GB+ available space
- **CPU**: ARMv8-A 64-bit processor

#### ARM64 Performance Characteristics

- **Container Startup**: 100-300ms typical (slightly slower than x86_64)
- **Memory Overhead**: ~15-25MB per container
- **Power Efficiency**: 30-50% better power consumption
- **Thermal Profile**: Lower heat generation

#### ARM-Specific Considerations

- **Page Size**: 4KB/64KB page size support
- **Memory Management**: LPAE (Large Physical Address Extension)
- **Security Features**: ARM TrustZone support
- **Virtualization**: ARM Virtualization Extensions

### Multi-Architecture Support

#### Cross-Platform Container Images

```yaml
# Example multi-arch manifest
architecture_support:
  - linux/amd64
  - linux/arm64
  - linux/arm/v7
  - linux/ppc64le
  - linux/s390x
```

#### Build Considerations

- **buildah/podman build**: Native multi-arch builds
- **QEMU Integration**: Cross-compilation support
- **Registry Support**: Multi-platform image distribution

### Performance Comparison Matrix

| Metric           | x86_64 | ARM64 | ARM32 | Notes                       |
| ---------------- | ------ | ----- | ----- | --------------------------- |
| Boot Time        | 1.0x   | 1.2x  | 1.5x  | Relative to x86_64 baseline |
| Memory Usage     | 1.0x   | 1.1x  | 0.9x  | Per container overhead      |
| Disk I/O         | 1.0x   | 0.95x | 0.8x  | Storage performance         |
| Network          | 1.0x   | 0.98x | 0.85x | Throughput performance      |
| Power Efficiency | 1.0x   | 1.4x  | 1.6x  | Performance per watt        |

## 🔄 Runtime Dependencies

### systemd Integration

#### Required Components

- **systemd**: 219+ (minimum), 245+ (recommended)
- **systemd-journald**: Logging integration
- **systemd-logind**: Session management
- **systemd-machined**: Container registration

#### Capabilities Used

- **User Services**: `systemd --user` support
- **Transient Services**: Dynamic service creation
- **Service Dependencies**: Container ordering
- **Resource Limits**: Cgroup integration

### Control Groups (cgroups)

#### cgroups v1 (Legacy)

- **Controllers**: cpu, memory, blkio, devices, freezer, net_cls, pids
- **Hierarchy**: Multiple separate hierarchies
- **Status**: Deprecated but still supported

#### cgroups v2 (Unified Hierarchy)

- **Features**: Single unified hierarchy
- **Controllers**: cpu, memory, io, pids, rdma
- **Benefits**: Better resource coordination, improved performance
- **Requirement**: Kernel 4.5+ for basic support, 5.0+ recommended

### Namespace Requirements

#### Kernel Namespaces

```bash
# Required namespaces for container isolation
Mount (mnt)     # Filesystem isolation
Process (pid)   # Process ID isolation
Network (net)   # Network interface isolation
IPC (ipc)       # Inter-process communication isolation
UTS (uts)       # Hostname isolation
User (user)     # User ID isolation (rootless)
Cgroup (cgroup) # Control group isolation
```

#### User Namespace Configuration

```bash
# /etc/subuid and /etc/subgid configuration required
username:100000:65536
```

## 💾 Storage Drivers and Backends

### Storage Driver Comparison

| Driver         | Performance | Rootless Support | Stability   | Use Case               |
| -------------- | ----------- | ---------------- | ----------- | ---------------------- |
| overlay2       | Excellent   | Partial\*        | Stable      | Production (root)      |
| fuse-overlayfs | Good        | Full             | Stable      | Production (rootless)  |
| vfs            | Poor        | Full             | Very Stable | Testing/Compatibility  |
| zfs            | Good        | Limited          | Beta        | Advanced storage       |
| btrfs          | Good        | Limited          | Beta        | Copy-on-write features |

\*Requires kernel 5.11+ or compatible filesystem

### Storage Configuration

#### Default Storage Location

```bash
# Root user
/var/lib/containers/

# Rootless user
$HOME/.local/share/containers/
```

#### Storage Configuration File

```toml
# /etc/containers/storage.conf or ~/.config/containers/storage.conf
[storage]
driver = "overlay2"
runroot = "/run/containers/storage"
graphroot = "/var/lib/containers/storage"

[storage.options]
additionalimagestores = [
    "/usr/share/containers/storage"
]

[storage.options.overlay]
mountopt = "nodev,metacopy=on"
```

## 🌐 Network Stack Dependencies

### Core Network Components

#### CNI (Container Network Interface)

- **Version**: 0.4.0+ required, 1.0.0+ recommended
- **Plugins Required**:
  - bridge
  - host-local (IPAM)
  - loopback
  - portmap
  - firewall
  - tuning

#### Network Backends

```bash
# Default CNI configuration location
/etc/cni/net.d/
/usr/libexec/cni/
```

### Network Driver Support

| Network Type | Description                          | Use Case             | Performance |
| ------------ | ------------------------------------ | -------------------- | ----------- |
| bridge       | Default bridge network               | Single host          | Good        |
| host         | Host networking                      | Performance critical | Excellent   |
| none         | No networking                        | Isolated containers  | N/A         |
| container    | Share network with another container | Sidecar patterns     | Good        |
| custom CNI   | Custom network plugins               | Advanced networking  | Variable    |

### Firewall Integration

#### firewalld Support

- **Zones**: Container network zone management
- **Rules**: Automatic rule creation and cleanup
- **Integration**: Native systemd integration

#### iptables Interaction

- **Chains**: Automatic chain management
- **NAT**: Port forwarding and masquerading
- **Filtering**: Traffic filtering rules

## 📦 Container Registry Integration

### Registry Protocol Support

#### OCI Distribution API

- **Version**: v2 API support
- **Authentication**: Token-based auth, basic auth
- **Transport**: HTTPS/HTTP with TLS validation
- **Compression**: gzip, zstd support

#### Registry Compatibility Matrix

| Registry   | Authentication | Signature Verification | Multi-arch | Notes               |
| ---------- | -------------- | ---------------------- | ---------- | ------------------- |
| Docker Hub | ✅             | ⚠️                     | ✅         | Rate limits apply   |
| quay.io    | ✅             | ✅                     | ✅         | Red Hat registry    |
| ghcr.io    | ✅             | ⚠️                     | ✅         | GitHub packages     |
| gcr.io     | ✅             | ⚠️                     | ✅         | Google registry     |
| Azure ACR  | ✅             | ⚠️                     | ✅         | Microsoft registry  |
| Harbor     | ✅             | ✅                     | ✅         | Enterprise registry |

### Registry Configuration

#### Global Registry Settings

```toml
# /etc/containers/registries.conf
[registries.search]
registries = ['docker.io', 'quay.io']

[registries.insecure]
registries = ['localhost:5000']

[registries.block]
registries = ['untrusted-registry.com']
```

#### Credential Management

```bash
# System-wide credentials
/run/containers/auth.json

# User credentials
$XDG_RUNTIME_DIR/containers/auth.json
```

## 🔨 Development and Build Tool Dependencies

### Container Build Tools

#### Buildah Integration

- **Purpose**: Advanced container building
- **Features**: Multi-stage builds, rootless building
- **Compatibility**: Full Podman integration
- **Use Case**: Complex build scenarios

#### Build Dependencies

```bash
# Essential build tools
git                 # Source code management
make               # Build automation
gcc/clang          # Compilation
pkg-config         # Library configuration
```

### Development Libraries

#### Build-time Dependencies

```bash
# Development packages (example for RHEL/Fedora)
pkgconf-devel
systemd-devel
libseccomp-devel
gpgme-devel
```

#### Runtime Libraries

```bash
# Runtime packages
libseccomp
gpgme
systemd
```

## 🧪 Testing and Validation

### Dependency Verification

#### System Requirements Check

```bash
#!/bin/bash
# Check kernel features
echo "Checking namespace support..."
ls /proc/self/ns/

echo "Checking cgroup support..."
ls /sys/fs/cgroup/

echo "Checking overlay filesystem..."
modprobe overlay && echo "overlay: OK" || echo "overlay: FAILED"

echo "Checking user namespaces..."
unshare -U whoami 2>/dev/null && echo "user ns: OK" || echo "user ns: FAILED"
```

#### Podman Installation Verification

```bash
# Basic functionality test
podman info
podman run --rm alpine echo "Hello from Podman"

# Rootless test (as non-root user)
podman unshare cat /proc/self/uid_map
```

### Performance Benchmarking

#### Container Startup Benchmark

```bash
#!/bin/bash
# Measure container startup time
time_startup() {
    start=$(date +%s%N)
    podman run --rm alpine true
    end=$(date +%s%N)
    echo "Container startup: $(( (end - start) / 1000000 ))ms"
}

for i in {1..10}; do
    time_startup
done
```

## 📊 Architecture-Specific Optimizations

### x86_64 Optimizations

- **Compiler Flags**: `-march=native -O2`
- **Memory**: NUMA awareness
- **Storage**: NVMe optimization
- **Network**: SR-IOV support

### ARM64 Optimizations

- **Power Management**: CPU frequency scaling
- **Memory**: Memory compaction settings
- **Storage**: eMMC/SD optimization
- **Thermal**: Temperature monitoring

## 🔒 Security Dependencies

### SELinux Integration

- **Policy**: container-selinux package
- **Contexts**: Automatic label management
- **Enforcement**: Mandatory access control

### AppArmor Support

- **Profiles**: Container-specific profiles
- **Confinement**: Application sandboxing
- **Integration**: Runtime policy enforcement

### Seccomp Support

- **Filters**: System call filtering
- **Profiles**: Default and custom profiles
- **Performance**: Minimal overhead filtering

## 📈 Monitoring and Observability

### Logging Integration

- **systemd-journald**: Structured logging
- **Log Drivers**: Multiple output formats
- **Rotation**: Automatic log rotation

### Metrics Collection

- **cAdvisor**: Container metrics
- **Prometheus**: Metrics export
- **Custom Metrics**: Application-specific metrics

## 🔮 Future Dependencies and Roadmap

### Emerging Technologies

- **Podman 5.0**: Enhanced rootless support
- **crun Optimization**: Performance improvements
- **Kubernetes Integration**: Better CRI support
- **WebAssembly**: WASM container support

### Platform Expansion

- **RISC-V**: Emerging architecture support
- **Windows Containers**: Cross-platform compatibility
- **macOS**: Development environment support

## 📚 Resources and References

### Official Documentation

- [Podman Installation Guide](https://podman.io/docs/installation)
- [Container Storage Documentation](https://github.com/containers/storage)
- [CNI Specification](https://github.com/containernetworking/cni)

### Architecture-Specific Guides

- [Multi-Architecture Container Guide](https://www.redhat.com/en/topics/containers)
- [Performance Tuning Guide](https://docs.podman.io/en/latest/markdown/podman-system-service.1.html)

### Community Resources

- [Podman GitHub Issues](https://github.com/containers/podman/issues)
- [Red Hat Container Tools](https://catalog.redhat.com/software/containers/explore)

---

_This document represents the current state of Podman's technology stack as of October 2024.
For the most up-to-date information, always consult the official documentation and release
notes._
