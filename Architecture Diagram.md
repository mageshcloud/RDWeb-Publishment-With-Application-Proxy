## Architecture Diagram

```text
                 Internet Users
                        |
                        |
                HTTPS (443)
                        |
                        v
+----------------------------------+
| Azure Entra ID                   |
|                                  |
| - User Authentication            |
| - Conditional Access             |
| - MFA Verification               |
+---------------+------------------+
                |
                |
                v
+----------------------------------+
| Azure Application Proxy Service  |
| (External URL Publishing)        |
+---------------+------------------+
                |
                |
                v
+----------------------------------+
| Application Proxy Connector      |
| (Installed on Domain Joined VM)  |
| Internal Outbound Connection     |
+---------------+------------------+
                |
                |
                v
+----------------------------------+
| VMware Environment               |
|                                  |
| RDWeb Server                     |
| RD Gateway                       |
| Connection Broker                |
| Session Host                     |
+---------------+------------------+
                |
                |
                v
+----------------------------------+
| Internal Applications            |
| Remote Desktop                   |
| Published Apps                   |
| RemoteApp                        |
+----------------------------------+
```

### Architecture Flow

1. Internet users access the RDWeb portal using HTTPS (443).
2. Azure Entra ID validates user authentication.
3. Conditional Access policies and MFA are enforced.
4. Azure Application Proxy securely publishes the application externally.
5. Application Proxy Connector establishes an outbound connection to Azure.
6. Requests are forwarded to the on-premises VMware-hosted RDS infrastructure.
7. RDWeb authenticates and launches RemoteApps through RD Gateway.
8. Users gain secure access to internal desktops and applications.
