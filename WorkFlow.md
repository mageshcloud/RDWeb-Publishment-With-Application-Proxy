# RDWeb Publication with Azure Entra Application Proxy & MFA

## Overview

This project demonstrates how to securely publish Remote Desktop Web Access (RDWeb) hosted on VMware using Azure Entra Application Proxy and Multi-Factor Authentication (MFA), enabling secure remote access without exposing internal infrastructure to the internet.

---

## Step 1: Deploy RDS Infrastructure on VMware

### Create RDS Virtual Machines

- RDWeb Server
- RD Gateway
- Connection Broker
- Session Host

### Install Remote Desktop Services Roles

**Server Manager**
```
Server Manager
→ Add Roles and Features
→ Remote Desktop Services
```

### Verification

```text
http://servername/RDWeb
```

---

## Step 2: Configure SSL Certificate

### Generate Certificate

- Internal CA Certificate
- Public CA Certificate (Recommended)

### Bind Certificates

- RDWeb
- RD Gateway
- RD Connection Broker

### Verification

```text
https://rdweb.company.com/RDWeb
```

---

## Step 3: Configure Azure Entra ID

### Prerequisites

- Azure Entra ID P1/P2 License
- Global Administrator Access
- Hybrid Identity Configuration

### Navigate To

```text
Entra Portal
→ Applications
→ Enterprise Applications
→ Application Proxy
```

### Enable

- Application Proxy Service

---

## Step 4: Install Application Proxy Connector

### Provision Connector Server

| Requirement | Specification |
|------------|--------------|
| OS | Windows Server |
| Domain Joined | Yes |
| CPU | 2 vCPU |
| RAM | 4 GB |

### Download Connector

```text
Entra Portal
→ Application Proxy
→ Download Connector
```

### Install Connector

```text
AADApplicationProxyConnectorInstaller.exe
```

### Authentication

- Sign in using Global Administrator Account

### Verification

```text
Connector Status: Active → Ready
```

---

## Step 5: Publish RDWeb

### Create Enterprise Application

```text
Enterprise Applications
→ New Application
→ On-Premises Application
```

### Configure Application

| Setting | Value |
|----------|--------|
| Internal URL | https://rdweb.company.local/RDWeb |
| External URL | https://rdweb-contoso.msappproxy.net |
| Connector Group | Default Connector Group |
| Protocol | HTTPS |

---

## Step 6: Enable Pre-Authentication

### Authentication Method

- Azure Entra ID

### Enable

- Conditional Access

### Authentication Flow

```text
User
 ↓
Azure Entra Authentication
 ↓
Application Proxy
 ↓
RDWeb
```

---

## Step 7: Configure Multi-Factor Authentication (MFA)

### Navigate To

```text
Entra ID
→ Conditional Access
```

### Create Policy

#### Target

- RDWeb Application

#### Grant Access

- Require MFA

#### Conditions

- Outside Corporate Network
- High-Risk Sign-In
- Privileged Users

### Authentication Flow

```text
Username
 ↓
Password
 ↓
MFA Verification
 ↓
RDWeb Access
```

---

## Step 8: Publish RD Gateway

### Create Application

Publish RD Gateway through Application Proxy.

### Configuration

| Setting | Value |
|----------|--------|
| Internal URL | https://rdgateway.company.local |
| External URL | https://rdgateway.msappproxy.net |
| Authentication | HTTP Passthrough |

### Purpose

- Secure RDP Traffic
- Remote Desktop Connectivity

---

## Step 9: Architecture Workflow

```text
Internet User
      │
      ▼
Azure Entra ID Authentication
      │
      ▼
Multi-Factor Authentication (MFA)
      │
      ▼
Azure Application Proxy
      │
      ▼
RDWeb Portal
      │
      ▼
RD Gateway
      │
      ▼
Session Host
```

---

## Step 10: Test End User Access

### Access URL

```text
https://rdweb-contoso.msappproxy.net/RDWeb
```

### End User Workflow

```text
User Login
     │
     ▼
MFA Verification
     │
     ▼
RDWeb Portal
     │
     ▼
Launch RemoteApp
     │
     ▼
RD Gateway
     │
     ▼
Session Host
```

---

## Benefits

- Secure Remote Access
- Azure Entra ID Authentication
- Multi-Factor Authentication (MFA)
- No Public Exposure of Internal Servers
- Conditional Access Integration
- Centralized Identity Management
- Enhanced Security and Compliance

---

## Technologies Used

- VMware vSphere
- Windows Server
- Remote Desktop Services (RDS)
- RDWeb
- RD Gateway
- Azure Entra ID
- Azure Application Proxy
- Multi-Factor Authentication (MFA)
- Conditional Access
