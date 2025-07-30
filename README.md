# AIVA - AI Virtual Agent

AIVA (AI Virtual Agent) is a secure microVM environment for running AI agents and MCP (Model Context Protocol) servers locally. It provides hardware-level isolation using Firecracker microVMs while maintaining a simple, Docker-like user experience.

## Features

- **Strong Security Isolation**: Hardware-level virtualization using Firecracker
- **Cross-Platform Support**: Works on Linux, macOS (via Lima), and Windows (via WSL2)
- **Simple CLI**: Docker-like commands for managing AI agents
- **Resource Management**: Fine-grained control over CPU, memory, and storage
- **Network Isolation**: Secure networking with NAT and port forwarding
- **Data Persistence**: Managed storage volumes for models and data

## Architecture

AIVA uses a layered architecture:

1. **CLI Layer**: User-friendly command-line interface
2. **Orchestration Layer**: VM lifecycle management
3. **Platform Abstraction Layer (PAL)**: Cross-platform compatibility
4. **Virtualization Layer**: Firecracker, Lima, or WSL2

## Requirements

### Linux
- KVM support (check with `ls /dev/kvm`)
- Firecracker and jailer binaries
- Root or sudo access for network configuration

### macOS
- Lima installed (`brew install lima`)
- macOS 11.0 or later
- At least 8GB RAM recommended

### Windows
- Windows 11 (recommended) or Windows 10 with WSL2
- WSL2 with nested virtualization enabled
- At least 8GB RAM recommended

## Installation

```bash
# Clone the repository
git clone https://github.com/tyrchen/aiva.git
cd aiva

# Build and install
make install

# Initialize development environment
make dev-init
```

## Quick Start

```bash
# Initialize a new AI agent
aiva init my-agent

# Start the agent with custom resources
aiva start my-agent --cpus 4 --memory 8GB --port 8080:80

# Check status
aiva status my-agent

# View logs
aiva logs my-agent

# Stop the agent
aiva stop my-agent
```

## Commands

### Core Commands

- `aiva init <name>` - Initialize a new AI agent/MCP server
- `aiva start <name>` - Start an agent
- `aiva stop <name>` - Stop an agent
- `aiva status [name]` - Show status of agents
- `aiva logs <name>` - View agent logs
- `aiva deploy <name>` - Deploy new image to agent

### Configuration

- `aiva config get <name> <key>` - Get configuration value
- `aiva config set <name> <key> <value>` - Set configuration value
- `aiva config list <name>` - List all configuration

### Data Management

- `aiva data sync <name>` - Sync data between host and VM
- `aiva data list <name>` - List data volumes

## Configuration

AIVA stores its configuration in `~/.aiva/config.yaml`:

```yaml
version: "1.0"
defaults:
  cpus: 4
  memory: "8GB"
  disk: "50GB"
  cache_strategy: "writeback"

platform:
  linux:
    firecracker_binary: "/usr/bin/firecracker"
    jailer_binary: "/usr/bin/jailer"
  macos:
    lima_instance: "aiva-host"
    lima_cpus: 8
    lima_memory: "16GB"
  windows:
    wsl_distro: "aiva-wsl"
    nested_virtualization: true

networking:
  bridge_name: "aiva-br0"
  subnet: "172.16.0.0/24"
  dns_servers:
    - "8.8.8.8"
    - "1.1.1.1"
```

## Resource Profiles

AIVA provides predefined resource profiles:

- **minimal**: 2 CPUs, 4GB RAM, 20GB disk (lightweight MCP servers)
- **standard**: 4 CPUs, 8GB RAM, 50GB disk (standard AI agents)
- **performance**: 8 CPUs, 16GB RAM, 100GB disk (large model inference)

## Development

```bash
# Run in development mode
make run ARGS="status"

# Run tests
make test

# Format code
make fmt

# Run linter
make lint

# Generate documentation
make doc
```

## Security Considerations

1. **Isolation**: Each AI agent runs in its own microVM with hardware-level isolation
2. **Networking**: Agents are isolated on a private network with explicit port mappings
3. **Storage**: Write-back caching ensures data integrity (with performance trade-offs)
4. **Permissions**: The jailer process provides additional security through privilege dropping

## Troubleshooting

### Linux

- **KVM not available**: Ensure KVM modules are loaded and `/dev/kvm` has proper permissions
- **Firecracker not found**: Install Firecracker from the official releases

### macOS

- **Lima issues**: Ensure Lima is properly installed and the VM can be created
- **Performance**: Nested virtualization may impact performance

### Windows

- **WSL2 issues**: Ensure WSL2 is properly installed and nested virtualization is enabled
- **Performance**: Multiple virtualization layers may impact performance

## License

This project is distributed under the terms of MIT.

See [LICENSE](LICENSE.md) for details.

Copyright 2025 AIVA Development Team
