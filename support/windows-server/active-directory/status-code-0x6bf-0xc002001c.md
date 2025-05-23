---
title: Status Code 0x6bf or 0xc002001c
description: Provides troubleshooting steps for resolving the remote procedure call (RPC) status code 0x6bf or 0xc002001c when you join a workgroup computer to a domain.
ms.date: 04/24/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: kaushika, raviks, herbertm, dennhu, eriw, v-lianna
ms.custom:
- sap:active directory\on-premises active directory domain join
- pcy:WinComm Directory Services
---
# Status code 0x6bf or 0xc002001c: The remote procedure call failed and did not execute

This article provides troubleshooting steps for resolving the remote procedure call (RPC) status code 0x6bf or 0xc002001c when you join a workgroup computer to a domain.

When you join a workgroup computer to a domain, you receive the following error message:

> The remote procedure call failed and did not execute.

When you check the **NetSetup.log** file, you see the following entries. For example:

```output
NetpGetLsaHandle: LsaOpenPolicy on \\<DC name>.<domain>.<tld> failed: 0xc002001c
NetpGetLsaPrimaryDomain: status: 0xc002001c
NetpJoinDomain: initiaing a rollback due to earlier errors
NetpJoinDomain: status of disconnecting from '\\<DC name>.<domain>.<tld>': 0x0
NetpDoDomainJoin: status: 0x6bf
```

## Network device rejects network packets

This error occurs when a network device (router, firewall, or virtual private network (VPN) device) rejects network packets between the client being joined and the domain controller (DC).

Error 0x6ba (RPC_S_SERVER_UNAVAILABLE) is different. When error 0x6ba occurs, the device can't create the TCP session to the server port. Error 0x6bf indicates that the TCP session can be created, but the RPC request message can't be delivered successfully, and the TCP session is reset.

Another variation of an RPC network session problem is error 0x6be (RPC_S_CALL_FAILED). In this case, the RPC request message can be delivered, but the TCP session is reset before the response is received.

## Verify and test the connection

To troubleshoot this issue, use the following steps:

1. Verify the connectivity between the client being joined and the target DC over the required ports and protocols.

    |Server port  |Service/Protocol  |
    |---------|---------|
    |Transmission Control Protocol (TCP) 135     |RPC Endpoint Mapper         |
    |TCP 49152 - 65535     |RPC (dynamic ports allocation)         |
    |TCP 445     |Server Message Block (SMB)         |
    |User Datagram Protocol (UDP)/TCP 389     |Lightweight Directory Access Protocol (LDAP)         |

    Refer to the list of required ports in [How to configure a firewall for Active Directory domains and trusts](config-firewall-for-ad-domains-and-trusts.md).

2. Test the connection between the client and the DC by running the following cmdlet:

    ```powershell
    Test-NetConnection <IP_address_of_the_DC> -Port 389
    ```

    The expected output is shown as follows:

    ```output
    ComputerName            : <ComputerName>
    RemoteAddress           : <RemoteAddress>
    RemotePort              : 389
    InterfaceAlias          : Ethernet 2
    SourceAddress           : <SourceAddress>
    TcpTestSucceeded        : True
    ```

    The output indicates that the LDAP port TCP 389 is open between the client and the DC.

3. Identify if a port (TCP/UDP) is blocked on a DC by using the [PortQry](https://www.microsoft.com/download/details.aspx?id=17148) command-line tool. For more information, see [Using the PortQry command-line tool](../networking/portqry-command-line-port-scanner-v2.md).

    Here are some example syntaxes:

    - `portqry -n <problem_server> -e 135`
    - `portqry -n <problem_server> -e 445`
    - `portqry -n <problem_server> -e 389`
    - `portqry -n <problem_server> -p UDP -e 389`
    - `portqry -n <problem_server> -r 49152:65535`

    Here are some example outputs:

    If the connection to TCP 135 port on the DC is blocked, you see the following output:

    ```output
    C:\PortQryV2>portqry -n dc2 -e 135
    Querying target system called:
    Dc2
    Attempting to resolve name to IP address…
    Name resolved to 192.168.1.2
    querying...
    TCP port 135 <epmap service>: FILTERED
    ```

    If the connection to TCP 389 port on the DC is successful, you see the following output:

    ```output
    C:\PortQryV2>portqry -n dc2 -e 389
    Querying target system called:
    Dc2
    Attempting to resolve name to IP address…
    Name resolved to 192.168.1.2
    querying...
    TCP port 389 <ldap service>: LISTENING
    ```

To determine if there are any further network connectivity problems, collect a network trace if necessary when reproducing the issue. You can use `netsh trace` to generate an ETL file, and [convert the ETL file to a PCAP file](https://techcommunity.microsoft.com/blog/coreinfrastructureandsecurityblog/converting-etl-files-to-pcap-files/1133297), which Wireshark can read.
