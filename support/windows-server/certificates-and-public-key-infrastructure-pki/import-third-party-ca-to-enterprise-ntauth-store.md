---
title: Import third-party certification authorities (CAs) into Enterprise NTAuth store
description: Describes two methods you can use to import the certificates of third-party CAs into the Enterprise NTAuth store. You can use the public key infrastructure (PKI) Health Tool, or Certutil.exe.
ms.date: 01/15/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: kaushika, slight, flbelea
ms.custom:
- sap:certificates and public key infrastructure (pki)\active directory certificate services (adcs)
- pcy:WinComm Directory Services
---
# How to import third-party certification authority (CA) certificates into the Enterprise NTAuth store

You can use two methods to import the certificates of third-party CAs into the Enterprise NTAuth store. This process is required if you're using a third-party CA to issue smart card logon or domain controller certificates. By publishing the CA certificate to the Enterprise NTAuth store, the Administrator indicates that the CA is trusted to issue certificates of these types and that issued certificates from these CA's can be used for authentication.  Windows Enterprise domain joined CAs automatically publish their own CA certificates to the NTAuth store.

_Original KB number:_ &nbsp; 295663

## More information

The NTAuth store is an Active Directory directory service object that is located in the Configuration container of the forest. The Lightweight Directory Access Protocol (LDAP) distinguished name is similar to the following example:

> CN=NTAuthCertificates,CN=Public Key Services,CN=Services,CN=Configuration,DC=MyDomain,DC=com

Certificates that are published to the NTAuth store are written to the cACertificate multiple-valued attribute. There are two supported methods to append a certificate to this attribute.

## Method 1 - Import a certificate by using the PKI Health Tool (PKIView)

PKIView is an MMC snap-in component. It displays the status of one or more Microsoft Windows CAs that comprise a PKI.

The PKIView is included in the install of the certificate services role. The tool is installed by default when you install the Windows Active Directory Certificate Services Role and has been re-branded as "Enterprise PKI". The tool is implemented as a snap-in for the Microsoft Management Console.

PKIView can be launched from the command line via PKIView.msc or by adding Enterprise PKI to the MMC snap-in.

PKIView gathers information about the CA certificates and certificate revocation lists (CRLs) from each CA in the enterprise. Then it validates the certificates and CRLs to ensure that they're working correctly. If they aren't working correctly, or they're about to fail or expire, PKIView provides a detailed warning or some error information.

PKIView displays the status of Windows Server CAs that are installed in an Active Directory forest. You can use PKIView to discover all PKI components, including subordinate and root CAs that are associated with an enterprise CA. The tool can also manage important PKI containers, such as root CA trust and NTAuth stores, that are contained in the configuration partition of an Active Directory forest. This article discusses this latter functionality. 

To import a CA certificate into the Enterprise NTAuth store, follow these steps:

1. Export the certificate of the CA to a .cer file. The following file formats are supported:
   - DER encoded binary X.509 (.cer)
   - Base-64 encoded X.509 (.cer)

2. Sign in to Windows Server Enterprise CAs or to an administrative workstation with RSAT installed.
3. Start Microsoft Management Console (Mmc.exe), and then add the Enterprise PKI snap-in:

   1. On the Console menu, select **Add/Remove Snap-in**.
   2. Select the **Standalone** tab, and then select the **Add** button.
   3. In the list of snap-ins, select **Enterprise PKI**.
   4. Select **Add**, and then select **Close**.
   5. Select **OK**.

4. Right-click **Enterprise PKI**, and then select **Manage AD Containers**.
5. Select the **NTAuthCertificates** tab, and then select **Add**.
6. On the **File** menu, select **Open**.
7. Locate and then select the CA certificate, and then select **OK** to complete the import.

## Method 2 - Import a certificate by using Certutil.exe

Certutil.exe is a command-line utility for managing a Windows CA, which you can use to publish certificates to Active Directory. For more information, see [certutil](/windows-server/administration/windows-commands/certutil).

To import a CA certificate into the Enterprise NTAuth store, follow these steps:

1. Export the certificate of the CA to a .cer file. The following file formats are supported:

   - DER encoded binary X.509 (.cer)
   - Base-64 encoded X.509 (.cer)
2. At a command prompt, type the following command, and then press ENTER:

   ```console
   certutil -dspublish -f filename NTAuthCA
   ```

The contents of the NTAuth store are cached in the following registry location:  
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\EnterpriseCertificates\NTAuth\Certificates`

This registry key should be automatically updated to reflect the certificates that are published to the NTAuth store in the Active Directory configuration container. This behavior occurs when Group Policy settings are updated and when the client-side extension that's responsible for computer auto-enrollment executes. In certain scenarios, such as Active Directory replication latency or when the **Do not enroll certificates automatically** policy setting is enabled, the registry isn't updated. In such scenarios, run the following command manually to insert the certificate into the registry location:

```console
certutil -enterprise -addstore NTAuth CA_CertFilename.cer
```
