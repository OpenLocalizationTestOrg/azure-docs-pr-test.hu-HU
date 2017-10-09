---
title: "aaaSilent Azure AD alkalmazás-Proxy összekötőjének telepítése |} Microsoft Docs"
description: "Ismerteti, hogyan tooperform az Azure AD alkalmazásproxy-összekötő tooprovide biztonságos távoli hozzáférés tooyour felügyelet nélküli telepítés a helyszíni alkalmazások."
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
ms.openlocfilehash: ce796ff45a65ba7d5f0f63c02085bdc6af494548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="silently-install-hello-azure-ad-application-proxy-connector"></a>Csendes telepítéséhez hello Azure AD alkalmazásproxy-összekötő
Azt szeretné, hogy egy telepítési parancsfájl toomultiple Windows-kiszolgálók vagy tooWindows kiszolgálók, amelyekhez nincs engedélyezve a felhasználói felület toobe képes toosend. Ez a témakör segítségével hozhat létre, amely lehetővé teszi a felügyelet nélküli telepítése és regisztrálása az Azure AD alkalmazásproxy-összekötő a Windows PowerShell-parancsfájl.

Ez a funkció akkor hasznos, ha azt szeretné, hogy:

* A gépeken, nincs felhasználói felület réteg vagy RDP toohello gépet nem lehet hello összekötő telepítéséhez.
* Telepíti és regisztrálja egyszerre sok összekötők.
* Integrálni hello összekötő telepítése és regisztrálása egy másik művelet részeként.
* Hello összekötő bits tartalmaz, de nincs regisztrálva szabványos kiszolgálói lemezképének létrehozásához.

Alkalmazásproxy egy slim nevű hello összekötő a hálózaton belüli Windows Server-szolgáltatás telepítése során. Az alkalmazásproxy-összekötő toowork hello toobe regisztrálva az Azure AD-címtár globális rendszergazdája és jelszóval rendelkezik. Általában ezt az információt is meg kell adni egy előugró párbeszédpanelen összekötő telepítése során. Azonban használható Windows PowerShell toocreate a hitelesítő objektum tooenter a regisztrációs adatait. Vagy hozzon létre egy saját tokent, és tooenter használja a regisztrációs adatait.

## <a name="install-hello-connector"></a>Hello összekötő telepítése
Hello összekötő MSIs telepítése nélkül hello összekötő regisztrálása az alábbiak szerint:

1. Nyisson meg egy parancssort.
2. Futtassa a következő parancs melyik hello /q jelenti csendes telepítést hello – hello telepítés nem kéri a végfelhasználói licencszerződés tooaccept hello.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-hello-connector-with-azure-ad"></a>Hello összekötő regisztrálására az Azure AD
Tooregister hello összekötővel két módszer áll rendelkezésre:

* Egy Windows PowerShell hitelesítő objektumot használ hello connector regisztrálása
* A létrehozott kapcsolat nélküli tokent hello connector regisztrálása

### <a name="register-hello-connector-using-a-windows-powershell-credential-object"></a>Egy Windows PowerShell hitelesítő objektumot használ hello connector regisztrálása
1. Hello Windows PowerShell hitelesítő objektumot létrehozni a következő parancs futtatásával. Cserélje le  *\<felhasználónév\>*  és  *\<jelszó\>*  hello felhasználónévvel és jelszóval a címtáron:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Nyissa meg túl**C:\Program Files\Microsoft AAD App alkalmazásproxy-összekötő** és hitelesítő adatok használatával hello PowerShell hello parancsprogrammal objektum-létrehozott. Cserélje le *$cred* hello PowerShell hello nevű hitelesítő adatok objektumot hozott létre:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-hello-connector-using-a-token-created-offline"></a>A létrehozott kapcsolat nélküli tokent hello connector regisztrálása
1. Hozzon létre egy kapcsolat nélküli token hello kódrészletet hello értékekkel hello AuthenticationContext osztály használatával:

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// hello AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// hello application ID of hello connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// hello reply address of hello connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// hello AppIdUri of hello registration service in AAD
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


2. Miután hello jogkivonat, hozzon létre egy SecureString hello token használatával:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. A következő Windows PowerShell-parancsot, hogy futási hello \<GUID bérlői\> a directory azonosítójú:

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a>Következő lépések 
* [Alkalmazások közzététele saját tartománynév használatával](active-directory-application-proxy-custom-domains.md)
* [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
* [Az alkalmazásproxy problémák elhárítása](active-directory-application-proxy-troubleshoot.md)


