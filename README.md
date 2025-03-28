# mac-ipsec-vpn-configuration

This repository contains the configuration files for setting up an IPSec VPN tunnel for Mac devices using FortiGate. The configuration is designed to create a secure communication channel between a FortiGate device and remote clients (MacOS devices) via IPSec VPN, utilizing a pre-shared key (PSK) for authentication and advanced encryption methods for securing data.

## Overview

This configuration defines the following settings:
- **VPN Type**: IPSec Tunnel
- **Supported Devices**: MacOS clients (via FortiClient or any compatible VPN client)
- **Authentication Method**: Pre-shared Key (PSK)
- **Encryption Protocols**: AES, SHA-256, SHA-1
- **IP Addressing**: IPv4 and IPv6 support
- **Phase 1 & Phase 2 Configurations**: Defines settings for establishing the VPN connection and securing data transmission.

## Configuration Details

### Phase 1 Configuration

The Phase 1 configuration establishes the initial secure communication between the FortiGate device and the Mac VPN client. It includes the settings for authentication, encryption, and key exchange.

```plaintext
config vpn ipsec phase1-interface
    edit "ipsec-macshare"
        set type dynamic
        set interface "port1"
        set ip-version 4
        set ike-version 1
        set local-gw 0.0.0.0
        set keylife 86400
        set authmethod psk
        set mode aggressive
        set peertype any
        set net-device disable
        set exchange-interface-ip disable
        set aggregate-member disable
        set mode-cfg enable
        set ipv4-wins-server1 0.0.0.0
        set ipv4-wins-server2 0.0.0.0
        set proposal aes128-sha256 aes256-sha256 aes128-sha1 aes256-sha1
        set add-route enable
        set localid ''
        set localid-type auto
        set negotiate-timeout 30
        set fragmentation enable
        set ip-fragmentation post-encapsulation
        set dpd on-demand
        set forticlient-enforcement disable
        set comments "VPN: ipsec-macshare (Created by VPN wizard)"
        set npu-offload enable
        set dhgrp 14 5
        set suite-b disable
        set wizard-type dialup-forticlient
        set xauthtype auto
        set reauth disable
        set authusrgrp "mac-users"
        set idle-timeout disable
        set ha-sync-esp-seqno enable
        set fgsp-sync disable
        set inbound-dscp-copy disable
        set auto-discovery-sender disable
        set auto-discovery-receiver disable
        set auto-discovery-forwarder disable
        set encapsulation none
        set nattraversal enable
        set rekey enable
        set enforce-unique-id disable
        set fec-egress disable
        set fec-ingress disable
        set link-cost 0
        set exchange-fgt-device-id disable
        set ems-sn-check disable
        set default-gw 0.0.0.0
        set default-gw-priority 0
        set assign-ip enable
        set assign-ip-from range
        set ipv4-start-ip 192.168.186.1
        set ipv4-end-ip 192.168.186.10
        set ipv4-netmask 255.255.255.255
        set dns-mode auto
        set ipv4-split-include "ipsec-macshare_split"
        set split-include-service ''
        set ipv6-start-ip ::
        set ipv6-end-ip ::
        set ipv6-prefix 128
        set ipv6-split-include ''
        set ip-delay-interval 0
        set unity-support enable
        set domain ''
        set banner ''
        set include-local-lan disable
        set ipv4-split-exclude ''
        set ipv6-split-exclude ''
        set save-password enable
        set client-auto-negotiate disable
        set client-keep-alive disable
        set psksecret PresharedKey
        set keepalive 10
        set distance 15
        set priority 1
        set dpd-retrycount 3
        set dpd-retryinterval 20
    next
end

####Phase 2 Configuration

The Phase 2 configuration secures the data transmission once the tunnel is established. It defines the encryption protocols, key lifetime, and other security parameters for protecting the data flow.

```plaintext
config vpn ipsec phase2-interface
    edit "ipsec-macshare"
        set phase1name "ipsec-macshare"
        set proposal aes128-sha1 aes256-sha1 aes128-sha256 aes256-sha256 aes128gcm aes256gcm chacha20poly1305
        set pfs enable
        set ipv4-df disable
        set dhgrp 14 5
        set replay enable
        set keepalive disable
        set add-route phase1
        set inbound-dscp-copy phase1
        set auto-discovery-sender phase1
        set auto-discovery-forwarder phase1
        set keylife-type seconds
        set single-source disable
        set route-overlap use-new
        set encapsulation tunnel-mode
        set comments "VPN: ipsec-macshare (Created by VPN wizard)"
        set diffserv disable
        set protocol 0
        set src-addr-type subnet
        set src-port 0
        set dst-addr-type subnet
        set dst-port 0
        set keylifeseconds 43200
        set src-subnet 0.0.0.0 0.0.0.0
        set dst-subnet 0.0.0.0 0.0.0.0
    next
end

