---
title: "Azure AD alkalmazás alkalmazásproxy-összekötő csendes telepítése |} Microsoft Docs"
description: "Bemutatja, hogyan adhat az Azure AD alkalmazásproxy-összekötő a helyszíni alkalmazások biztonságos távoli hozzáférést biztosítanak a felügyelet nélküli telepítést."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3aa1c7f2-fb2a-4693-abd5-95bb53700cbb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 9e28c89d8f64f0ae3d4150017ca544e606075c45
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="silently-install-the-azure-ad-application-proxy-connector"></a>Az Azure AD alkalmazásproxy-összekötő telepítéséhez
Érdemes lehet küldeni egy telepítési parancsfájlt, több Windows kiszolgálót vagy Windows Server kiszolgálókon, amelyek nem rendelkeznek a felhasználói felület engedélyezve van. Ez a témakör segítségével hozhat létre, amely lehetővé teszi a felügyelet nélküli telepítése és regisztrálása az Azure AD alkalmazásproxy-összekötő a Windows PowerShell-parancsfájl.

Ez a funkció akkor hasznos, ha azt szeretné, hogy:

* Telepítse az összekötőt nincs felhasználói felület réteggel, vagy a gép nem lehet RDP gépek.
* Telepíti és regisztrálja egyszerre sok összekötők.
* Integrálható a összekötő telepítése és regisztrálása egy másik művelet részeként.
* Hozzon létre egy szabványos kiszolgálói lemezképet, az összekötő bits tartalmaz, de nincs regisztrálva.

Alkalmazásproxy egy slim nevű az összekötő a hálózaton belüli Windows Server-szolgáltatás telepítése során. Az alkalmazásproxy-összekötő működéséhez azt ki regisztrálni kell az Azure AD-címtár globális rendszergazdája és jelszóval. Általában ezt az információt is meg kell adni egy előugró párbeszédpanelen összekötő telepítése során. Azonban a Windows PowerShell használatával adja meg a termékregisztrációs adatokat hitelesítőadat-objektum létrehozása. Vagy hozzon létre egy saját tokent, és adja meg a termékregisztrációs adatokat segítségével.

## <a name="install-the-connector"></a>Az összekötő telepítése
Az összekötő MSIs telepítése nélkül az összekötő regisztrálása az alábbiak szerint:

1. Nyisson meg egy parancssort.
2. Futtassa a következő parancsot, amelyben a /q a csendes telepítés – azt jelenti, hogy a telepítés nem kéri a végfelhasználói licencszerződés elfogadásához.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Az összekötő regisztrálására az Azure AD
Az összekötő regisztrálása segítségével két módszer áll rendelkezésre:

* Egy Windows PowerShell hitelesítő objektumot használ a connector regisztrálása
* A létrehozott kapcsolat nélküli jogkivonat használatával connector regisztrálása

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Egy Windows PowerShell hitelesítő objektumot használ a connector regisztrálása
1. A Windows PowerShell hitelesítő objektumot létrehozni a következő parancs futtatásával. Cserélje le  *\<felhasználónév\>*  és  *\<jelszó\>*  a felhasználónévvel és a címtár jelszava:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Ugrás a **C:\Program Files\Microsoft AAD App alkalmazásproxy-összekötő** és hitelesítő adatok használata a PowerShell parancsfájl futtatása objektum-létrehozott. Cserélje le *$cred* nevű, a PowerShell hitelesítő objektumot hozott létre:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-the-connector-using-a-token-created-offline"></a>A létrehozott kapcsolat nélküli jogkivonat használatával connector regisztrálása
1. Hozzon létre egy kapcsolat nélküli token felhasználva a kódrészletet AuthenticationContext osztály használatával:

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }


2. Miután a jogkivonatot, hozzon létre egy SecureString a token használatával:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Futtassa a következő Windows PowerShell-parancsot cseréje \<GUID bérlői\> a directory azonosítójú:

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a>Következő lépések 
* [Alkalmazások közzététele saját tartománynév használatával](active-directory-application-proxy-custom-domains.md)
* [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
* [Az alkalmazásproxy problémák elhárítása](active-directory-application-proxy-troubleshoot.md)


