# WebRTC (Web Real-Time Communication)

WebRTC is a standardized API that enables peer-to-peer (P2P) communication between browsers, mobile apps, and IoT devices, allowing efficient and low-latency exchange of audio, video, and data.

---

## Key Features

- **Peer-to-Peer Communication**: Direct connection between clients to minimize latency.
- **Standardized API**: Supported by major browsers and platforms.
- **Real-Time Media**: Audio/video and arbitrary data streaming.
- **Cross-Platform**: Works across web, mobile, and embedded devices.

---

## Connection Establishment Flow (A ↔ B)

1. **A gathers ICE candidates**  
   - A finds all possible public paths (IP/port combinations) it can be reached through.

2. **B gathers ICE candidates**  
   - B does the same.

3. **Session Signaling (Out-of-Band)**  
   - A and B exchange signaling information (SDP, ICE candidates) through external channels like:
     - WebSockets
     - HTTP Fetch
     - QR code
     - WhatsApp
     - Tweets

4. **ICE Candidate Selection**  
   - A and B test the available paths and select the most optimal one.

5. **Media & Security Exchange**  
   - A and B agree on supported media formats (codecs) and encryption parameters.

---

## WebRTC Deep Dive: NAT Traversal

### What is NAT?

- **NAT (Network Address Translation)** allows multiple devices in a local network to share a single public IP.
- Most devices are behind a NAT router and don’t directly hold a public IP.
- Routers modify IP packet headers to map internal (private) IPs to a public IP and track connections.

### NAT Translation Workflow

1. Device checks if destination IP is in the same subnet.
2. If not, sends the packet to the router.
3. Router rewrites the **source IP and port** to its public IP and a mapped port.
4. Router tracks the internal IP/port in a NAT table.
5. When response comes back, router translates the destination IP/port back to the original internal device.

---

## Types of NAT

| NAT Type             | Behavior                                                                 | WebRTC Compatibility |
|----------------------|--------------------------------------------------------------------------|----------------------|
| **Full Cone NAT**    | One-to-one mapping; allows incoming packets from any external address.   | ✅ Best support      |
| **Address Restricted** | Allows external packets only if the source IP matches previous communication. | ⚠️ Mostly supported |
| **Port Restricted**  | Requires both source IP and source port to match previous request.       | ⚠️ Supported with STUN/TURN |
| **Symmetric NAT**    | Each outbound request from same internal IP/port gets a unique external port. Only allows inbound packets from that exact destination. | ❌ Problematic for WebRTC |

---

## Key Concepts

- **STUN (Session Traversal Utilities for NAT)**  
  Helps clients discover their public-facing IP/port.

- **TURN (Traversal Using Relays around NAT)**  
  Fallback relay server for relaying media when direct P2P is not possible (e.g., symmetric NAT).

- **ICE (Interactive Connectivity Establishment)**  
  Protocol for gathering and testing multiple candidate paths (STUN, TURN, local) to choose the best connection.

- **SDP (Session Description Protocol)**  
  Describes media capabilities and connection info for negotiation.

---
