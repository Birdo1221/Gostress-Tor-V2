# Tor C2 Framework

* Tor C2 Framework with Tor Support* + Bot / CLient 

## 📌 Overview

> This does not require or support proxies by default. However, you can configure a proxy for the bot to connect through and then route all traffic through Tor to reach the domain. This adds an extra layer of anonymity, while also protecting the server's infrastructure from being directly exposed.
> 
>  On the downside, it introduces slower connection times, potential instability due to Tor exit node changes, and added complexity during setup and debugging. It may also cause some services to block traffic or flag it as suspicious. So while it enhances privacy and makes tracking harder, it also comes with trade-offs in terms of performance and reliability.

- **Encrypted TLS Communication** with client agents
- **Tor Hidden Service** integration for anonymous operation
- **Multi-user Role-Based Access Control**
- **Real-time Dashboard** with performance metrics
- **Multiple Network Testing Vectors** with adaptive rate limiting
- **Client Health Monitoring** with heartbeat system

> ⚠️ **Important**: This framework is intended solely for authorized security testing, educational purposes, and legitimate security research. Always obtain proper authorization before testing any system. Unauthorized use may violate laws and regulations.

| Startup | 
|---------|
| ![Startup](https://github.com/user-attachments/assets/9ff3101e-cea6-4a7c-8d13-af54f8c891cc) | 

| Login | Dashboard |
|-------|----------|
| ![Login](https://github.com/user-attachments/assets/26734671-aae5-4240-a7e6-ee6ed836e5f8) | ![Dashboard](https://github.com/user-attachments/assets/e7872d5c-e78b-47d8-b580-8dcf16b7a79a) |

| Users | Commands | Sys-Settings |
|-------|----------|--------------|
| ![Users](https://github.com/user-attachments/assets/5937982b-fe99-4e0f-a859-54ff94b187cd) | ![Commands](https://github.com/user-attachments/assets/092f24e0-f332-4496-b399-2b00b76908c9) | ![Sys-Setting](https://github.com/user-attachments/assets/50c66a61-8ca4-4def-b28c-cce4e08909c4) |

## 🔥 Features

### Core Architecture

- **Asynchronous Design**: Non-blocking I/O for 10,000+ concurrent clients
- **Modular Testing System**: Pluggable test modules with runtime validation
- **Ephemeral Clients**: Auto-cleanup of stale connections
- **HMAC Challenge-Response**: For client authentication

```mermaid
sequenceDiagram
    Client->>Server: TCP Handshake (TLS 1.3)
    Server->>Client: CHALLENGE:nonce
    Client->>Server: HMAC-SHA256(nonce)
    Server->>Client: AUTH_SUCCESS
    loop Heartbeat
        Client->>Server: PONG (30s interval)
    end
```

### Network Testing Capabilities

| Method | Layer | Description | Max Duration |
|--------|-------|-------------|--------------|
| UDP Test | 4 | High-volume UDP packet testing | 3600s |
| TCP Smart | 4 | Stateful TCP session analysis | 1800s |
| GRE Test | 3 | Protocol examination with GRE packets | 600s |
| HTTP Connection Test | 7 | Partial HTTP requests with keepalive | 300s |

### Security Features

- **Argon2id Password Hashing**: 128MB memory / 4 threads
- **JWT Authentication**: 15-minute expiry with refresh
- **CSRF Protection**: Per-session tokens
- **CSP Headers**: Strict Content Security Policy
- **WebRTC Disabled**: Prevents IP leakage

## 🛠️ Installation

### Prerequisites

```bash
sudo apt install tor golang-go postgresql
```

### Setup

Generate certificates:
```bash
go run main.go -gencert
```

Configure Tor (automatically done by server):
```bash
cat > /etc/tor/torrc <<EOF
HiddenServiceDir /var/lib/tor/scream_service/
HiddenServicePort 80 127.0.0.1:443
EOF
```

## 🖥️ Dashboard Features

```mermaid
pie
    title Dashboard Components
    "Client Network" : 35
    "Test Control" : 30
    "User Management" : 20
    "Analytics" : 15
```

### Real-time Monitoring

- Client geographic distribution
- CPU/RAM utilization heatmap
- Network throughput graphs
- Packet loss metrics

### User Roles

| Role | Concurrent Tests | Methods Available | Duration Limit |
|------|-----------------|-------------------|---------------|
| Owner | 5 | All | 60 min |
| Admin | 5 | No UPDATE command | 30 min |
| Pro | 3 | Basic protocols | 10 min |
| Basic | 1 | UDP/TCP only | 5 min |

## ⚡ Quick Start

Start the server:
```bash
go build -o scream && ./scream 
```

Access via Tor:
```bash
torify curl http://youronionaddress.onion
```

Default credentials:
```
Username: root
Password: [generated during first run]
```

## 📊 Performance Metrics

```mermaid
gantt
    title Test Lifecycle
    dateFormat  HH:mm:ss
    section Client Network
    Connection Establishment :a1, 00:00:00, 5s
    Challenge Response      :a2, after a1, 3s
    section Test
    Command Propagation     :a3, 00:00:08, 2s
    Traffic Generation      :a4, after a3, 30s
```

- Throughput: 1.2M packets/sec per client
- Latency: <200ms command propagation
- Scalability: Tested with 5,000 concurrent clients

## 🔒 Security Considerations

### Operational Security

- All traffic routed through Tor
- Server IP never exposed to clients
- Memory-safe implementations
- No persistent client identifiers

### Defensive Measures

```mermaid
graph LR
    A[Incoming Request] --> B{Rate Limited?}
    B -->|No| C[HMAC Validation]
    B -->|Yes| D[Drop Connection]
    C --> E[Command Whitelist Check]
    E --> F[Execute Command]
```

## 🚨 Legal & Ethical Considerations

This tool is provided for **educational and authorized security testing purposes only**. It is your responsibility to:

1. Obtain proper authorization before testing any system
2. Comply with all applicable laws and regulations
3. Use this tool only in environments you own or have explicit permission to test
4. Understand that the developers assume no liability for misuse

## 📜 License

GNU General Public License v3.0
