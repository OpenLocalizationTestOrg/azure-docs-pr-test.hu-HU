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
# <a name="silently-install-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="21a4b-103">Csendes telepítéséhez hello Azure AD alkalmazásproxy-összekötő</span><span class="sxs-lookup"><span data-stu-id="21a4b-103">Silently install hello Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="21a4b-104">Azt szeretné, hogy egy telepítési parancsfájl toomultiple Windows-kiszolgálók vagy tooWindows kiszolgálók, amelyekhez nincs engedélyezve a felhasználói felület toobe képes toosend.</span><span class="sxs-lookup"><span data-stu-id="21a4b-104">You want toobe able toosend an installation script toomultiple Windows servers or tooWindows Servers that don't have user interface enabled.</span></span> <span data-ttu-id="21a4b-105">Ez a témakör segítségével hozhat létre, amely lehetővé teszi a felügyelet nélküli telepítése és regisztrálása az Azure AD alkalmazásproxy-összekötő a Windows PowerShell-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="21a4b-105">This topic helps you create a Windows PowerShell script that enables unattended installation and registration for your Azure AD Application Proxy Connector.</span></span>

<span data-ttu-id="21a4b-106">Ez a funkció akkor hasznos, ha azt szeretné, hogy:</span><span class="sxs-lookup"><span data-stu-id="21a4b-106">This capability is useful when you want to:</span></span>

* <span data-ttu-id="21a4b-107">A gépeken, nincs felhasználói felület réteg vagy RDP toohello gépet nem lehet hello összekötő telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="21a4b-107">Install hello connector on machines with no UI layer or when you cannot RDP toohello machine.</span></span>
* <span data-ttu-id="21a4b-108">Telepíti és regisztrálja egyszerre sok összekötők.</span><span class="sxs-lookup"><span data-stu-id="21a4b-108">Install and register many connectors at once.</span></span>
* <span data-ttu-id="21a4b-109">Integrálni hello összekötő telepítése és regisztrálása egy másik művelet részeként.</span><span class="sxs-lookup"><span data-stu-id="21a4b-109">Integrate hello connector installation and registration as part of another procedure.</span></span>
* <span data-ttu-id="21a4b-110">Hello összekötő bits tartalmaz, de nincs regisztrálva szabványos kiszolgálói lemezképének létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="21a4b-110">Create a standard server image that contains hello connector bits but is not registered.</span></span>

<span data-ttu-id="21a4b-111">Alkalmazásproxy egy slim nevű hello összekötő a hálózaton belüli Windows Server-szolgáltatás telepítése során.</span><span class="sxs-lookup"><span data-stu-id="21a4b-111">Application Proxy works by installing a slim Windows Server service called hello Connector inside your network.</span></span> <span data-ttu-id="21a4b-112">Az alkalmazásproxy-összekötő toowork hello toobe regisztrálva az Azure AD-címtár globális rendszergazdája és jelszóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="21a4b-112">For hello Application Proxy Connector toowork it has toobe registered with your Azure AD directory using a global administrator and password.</span></span> <span data-ttu-id="21a4b-113">Általában ezt az információt is meg kell adni egy előugró párbeszédpanelen összekötő telepítése során.</span><span class="sxs-lookup"><span data-stu-id="21a4b-113">Ordinarily this information is entered during Connector installation in a pop-up dialog box.</span></span> <span data-ttu-id="21a4b-114">Azonban használható Windows PowerShell toocreate a hitelesítő objektum tooenter a regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="21a4b-114">However, you can use Windows PowerShell toocreate a credential object tooenter your registration information.</span></span> <span data-ttu-id="21a4b-115">Vagy hozzon létre egy saját tokent, és tooenter használja a regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="21a4b-115">Or you can create your own token and use it tooenter your registration information.</span></span>

## <a name="install-hello-connector"></a><span data-ttu-id="21a4b-116">Hello összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="21a4b-116">Install hello connector</span></span>
<span data-ttu-id="21a4b-117">Hello összekötő MSIs telepítése nélkül hello összekötő regisztrálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="21a4b-117">Install hello Connector MSIs without registering hello Connector as follows:</span></span>

1. <span data-ttu-id="21a4b-118">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="21a4b-118">Open a command prompt.</span></span>
2. <span data-ttu-id="21a4b-119">Futtassa a következő parancs melyik hello /q jelenti csendes telepítést hello – hello telepítés nem kéri a végfelhasználói licencszerződés tooaccept hello.</span><span class="sxs-lookup"><span data-stu-id="21a4b-119">Run hello following command in which hello /q means quiet installation - hello installation doesn't prompt you tooaccept hello End-User License Agreement.</span></span>
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-hello-connector-with-azure-ad"></a><span data-ttu-id="21a4b-120">Hello összekötő regisztrálására az Azure AD</span><span class="sxs-lookup"><span data-stu-id="21a4b-120">Register hello connector with Azure AD</span></span>
<span data-ttu-id="21a4b-121">Tooregister hello összekötővel két módszer áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="21a4b-121">There are two methods you can use tooregister hello connector:</span></span>

* <span data-ttu-id="21a4b-122">Egy Windows PowerShell hitelesítő objektumot használ hello connector regisztrálása</span><span class="sxs-lookup"><span data-stu-id="21a4b-122">Register hello connector using a Windows PowerShell credential object</span></span>
* <span data-ttu-id="21a4b-123">A létrehozott kapcsolat nélküli tokent hello connector regisztrálása</span><span class="sxs-lookup"><span data-stu-id="21a4b-123">Register hello connector using a token created offline</span></span>

### <a name="register-hello-connector-using-a-windows-powershell-credential-object"></a><span data-ttu-id="21a4b-124">Egy Windows PowerShell hitelesítő objektumot használ hello connector regisztrálása</span><span class="sxs-lookup"><span data-stu-id="21a4b-124">Register hello connector using a Windows PowerShell credential object</span></span>
1. <span data-ttu-id="21a4b-125">Hello Windows PowerShell hitelesítő objektumot létrehozni a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="21a4b-125">Create hello Windows PowerShell Credentials object by running this command.</span></span> <span data-ttu-id="21a4b-126">Cserélje le  *\<felhasználónév\>*  és  *\<jelszó\>*  hello felhasználónévvel és jelszóval a címtáron:</span><span class="sxs-lookup"><span data-stu-id="21a4b-126">Replace *\<username\>* and *\<password\>* with hello username and password for your directory:</span></span>
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. <span data-ttu-id="21a4b-127">Nyissa meg túl**C:\Program Files\Microsoft AAD App alkalmazásproxy-összekötő** és hitelesítő adatok használatával hello PowerShell hello parancsprogrammal objektum-létrehozott.</span><span class="sxs-lookup"><span data-stu-id="21a4b-127">Go too**C:\Program Files\Microsoft AAD App Proxy Connector** and run hello script using hello PowerShell credentials object you created.</span></span> <span data-ttu-id="21a4b-128">Cserélje le *$cred* hello PowerShell hello nevű hitelesítő adatok objektumot hozott létre:</span><span class="sxs-lookup"><span data-stu-id="21a4b-128">Replace *$cred* with hello name of hello PowerShell credentials object you created:</span></span>
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-hello-connector-using-a-token-created-offline"></a><span data-ttu-id="21a4b-129">A létrehozott kapcsolat nélküli tokent hello connector regisztrálása</span><span class="sxs-lookup"><span data-stu-id="21a4b-129">Register hello connector using a token created offline</span></span>
1. <span data-ttu-id="21a4b-130">Hozzon létre egy kapcsolat nélküli token hello kódrészletet hello értékekkel hello AuthenticationContext osztály használatával:</span><span class="sxs-lookup"><span data-stu-id="21a4b-130">Create an offline token using hello AuthenticationContext class using hello values in hello code snippet:</span></span>

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


2. <span data-ttu-id="21a4b-131">Miután hello jogkivonat, hozzon létre egy SecureString hello token használatával:</span><span class="sxs-lookup"><span data-stu-id="21a4b-131">Once you have hello token, create a SecureString using hello token:</span></span>

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. <span data-ttu-id="21a4b-132">A következő Windows PowerShell-parancsot, hogy futási hello \<GUID bérlői\> a directory azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="21a4b-132">Run hello following Windows PowerShell command, replacing \<tenant GUID\> with your directory ID:</span></span>

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a><span data-ttu-id="21a4b-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21a4b-133">Next steps</span></span> 
* [<span data-ttu-id="21a4b-134">Alkalmazások közzététele saját tartománynév használatával</span><span class="sxs-lookup"><span data-stu-id="21a4b-134">Publish applications using your own domain name</span></span>](active-directory-application-proxy-custom-domains.md)
* [<span data-ttu-id="21a4b-135">Egyszeri bejelentkezés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="21a4b-135">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="21a4b-136">Az alkalmazásproxy problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="21a4b-136">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)


