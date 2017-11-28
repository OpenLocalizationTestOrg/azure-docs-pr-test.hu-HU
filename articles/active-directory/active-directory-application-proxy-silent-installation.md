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
# <a name="silently-install-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="46ab5-103">Az Azure AD alkalmazásproxy-összekötő telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="46ab5-103">Silently install the Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="46ab5-104">Érdemes lehet küldeni egy telepítési parancsfájlt, több Windows kiszolgálót vagy Windows Server kiszolgálókon, amelyek nem rendelkeznek a felhasználói felület engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="46ab5-104">You want to be able to send an installation script to multiple Windows servers or to Windows Servers that don't have user interface enabled.</span></span> <span data-ttu-id="46ab5-105">Ez a témakör segítségével hozhat létre, amely lehetővé teszi a felügyelet nélküli telepítése és regisztrálása az Azure AD alkalmazásproxy-összekötő a Windows PowerShell-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="46ab5-105">This topic helps you create a Windows PowerShell script that enables unattended installation and registration for your Azure AD Application Proxy Connector.</span></span>

<span data-ttu-id="46ab5-106">Ez a funkció akkor hasznos, ha azt szeretné, hogy:</span><span class="sxs-lookup"><span data-stu-id="46ab5-106">This capability is useful when you want to:</span></span>

* <span data-ttu-id="46ab5-107">Telepítse az összekötőt nincs felhasználói felület réteggel, vagy a gép nem lehet RDP gépek.</span><span class="sxs-lookup"><span data-stu-id="46ab5-107">Install the connector on machines with no UI layer or when you cannot RDP to the machine.</span></span>
* <span data-ttu-id="46ab5-108">Telepíti és regisztrálja egyszerre sok összekötők.</span><span class="sxs-lookup"><span data-stu-id="46ab5-108">Install and register many connectors at once.</span></span>
* <span data-ttu-id="46ab5-109">Integrálható a összekötő telepítése és regisztrálása egy másik művelet részeként.</span><span class="sxs-lookup"><span data-stu-id="46ab5-109">Integrate the connector installation and registration as part of another procedure.</span></span>
* <span data-ttu-id="46ab5-110">Hozzon létre egy szabványos kiszolgálói lemezképet, az összekötő bits tartalmaz, de nincs regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="46ab5-110">Create a standard server image that contains the connector bits but is not registered.</span></span>

<span data-ttu-id="46ab5-111">Alkalmazásproxy egy slim nevű az összekötő a hálózaton belüli Windows Server-szolgáltatás telepítése során.</span><span class="sxs-lookup"><span data-stu-id="46ab5-111">Application Proxy works by installing a slim Windows Server service called the Connector inside your network.</span></span> <span data-ttu-id="46ab5-112">Az alkalmazásproxy-összekötő működéséhez azt ki regisztrálni kell az Azure AD-címtár globális rendszergazdája és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="46ab5-112">For the Application Proxy Connector to work it has to be registered with your Azure AD directory using a global administrator and password.</span></span> <span data-ttu-id="46ab5-113">Általában ezt az információt is meg kell adni egy előugró párbeszédpanelen összekötő telepítése során.</span><span class="sxs-lookup"><span data-stu-id="46ab5-113">Ordinarily this information is entered during Connector installation in a pop-up dialog box.</span></span> <span data-ttu-id="46ab5-114">Azonban a Windows PowerShell használatával adja meg a termékregisztrációs adatokat hitelesítőadat-objektum létrehozása.</span><span class="sxs-lookup"><span data-stu-id="46ab5-114">However, you can use Windows PowerShell to create a credential object to enter your registration information.</span></span> <span data-ttu-id="46ab5-115">Vagy hozzon létre egy saját tokent, és adja meg a termékregisztrációs adatokat segítségével.</span><span class="sxs-lookup"><span data-stu-id="46ab5-115">Or you can create your own token and use it to enter your registration information.</span></span>

## <a name="install-the-connector"></a><span data-ttu-id="46ab5-116">Az összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="46ab5-116">Install the connector</span></span>
<span data-ttu-id="46ab5-117">Az összekötő MSIs telepítése nélkül az összekötő regisztrálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="46ab5-117">Install the Connector MSIs without registering the Connector as follows:</span></span>

1. <span data-ttu-id="46ab5-118">Nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="46ab5-118">Open a command prompt.</span></span>
2. <span data-ttu-id="46ab5-119">Futtassa a következő parancsot, amelyben a /q a csendes telepítés – azt jelenti, hogy a telepítés nem kéri a végfelhasználói licencszerződés elfogadásához.</span><span class="sxs-lookup"><span data-stu-id="46ab5-119">Run the following command in which the /q means quiet installation - the installation doesn't prompt you to accept the End-User License Agreement.</span></span>
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a><span data-ttu-id="46ab5-120">Az összekötő regisztrálására az Azure AD</span><span class="sxs-lookup"><span data-stu-id="46ab5-120">Register the connector with Azure AD</span></span>
<span data-ttu-id="46ab5-121">Az összekötő regisztrálása segítségével két módszer áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="46ab5-121">There are two methods you can use to register the connector:</span></span>

* <span data-ttu-id="46ab5-122">Egy Windows PowerShell hitelesítő objektumot használ a connector regisztrálása</span><span class="sxs-lookup"><span data-stu-id="46ab5-122">Register the connector using a Windows PowerShell credential object</span></span>
* <span data-ttu-id="46ab5-123">A létrehozott kapcsolat nélküli jogkivonat használatával connector regisztrálása</span><span class="sxs-lookup"><span data-stu-id="46ab5-123">Register the connector using a token created offline</span></span>

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a><span data-ttu-id="46ab5-124">Egy Windows PowerShell hitelesítő objektumot használ a connector regisztrálása</span><span class="sxs-lookup"><span data-stu-id="46ab5-124">Register the connector using a Windows PowerShell credential object</span></span>
1. <span data-ttu-id="46ab5-125">A Windows PowerShell hitelesítő objektumot létrehozni a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="46ab5-125">Create the Windows PowerShell Credentials object by running this command.</span></span> <span data-ttu-id="46ab5-126">Cserélje le  *\<felhasználónév\>*  és  *\<jelszó\>*  a felhasználónévvel és a címtár jelszava:</span><span class="sxs-lookup"><span data-stu-id="46ab5-126">Replace *\<username\>* and *\<password\>* with the username and password for your directory:</span></span>
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. <span data-ttu-id="46ab5-127">Ugrás a **C:\Program Files\Microsoft AAD App alkalmazásproxy-összekötő** és hitelesítő adatok használata a PowerShell parancsfájl futtatása objektum-létrehozott.</span><span class="sxs-lookup"><span data-stu-id="46ab5-127">Go to **C:\Program Files\Microsoft AAD App Proxy Connector** and run the script using the PowerShell credentials object you created.</span></span> <span data-ttu-id="46ab5-128">Cserélje le *$cred* nevű, a PowerShell hitelesítő objektumot hozott létre:</span><span class="sxs-lookup"><span data-stu-id="46ab5-128">Replace *$cred* with the name of the PowerShell credentials object you created:</span></span>
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-the-connector-using-a-token-created-offline"></a><span data-ttu-id="46ab5-129">A létrehozott kapcsolat nélküli jogkivonat használatával connector regisztrálása</span><span class="sxs-lookup"><span data-stu-id="46ab5-129">Register the connector using a token created offline</span></span>
1. <span data-ttu-id="46ab5-130">Hozzon létre egy kapcsolat nélküli token felhasználva a kódrészletet AuthenticationContext osztály használatával:</span><span class="sxs-lookup"><span data-stu-id="46ab5-130">Create an offline token using the AuthenticationContext class using the values in the code snippet:</span></span>

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


2. <span data-ttu-id="46ab5-131">Miután a jogkivonatot, hozzon létre egy SecureString a token használatával:</span><span class="sxs-lookup"><span data-stu-id="46ab5-131">Once you have the token, create a SecureString using the token:</span></span>

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. <span data-ttu-id="46ab5-132">Futtassa a következő Windows PowerShell-parancsot cseréje \<GUID bérlői\> a directory azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="46ab5-132">Run the following Windows PowerShell command, replacing \<tenant GUID\> with your directory ID:</span></span>

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a><span data-ttu-id="46ab5-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46ab5-133">Next steps</span></span> 
* [<span data-ttu-id="46ab5-134">Alkalmazások közzététele saját tartománynév használatával</span><span class="sxs-lookup"><span data-stu-id="46ab5-134">Publish applications using your own domain name</span></span>](active-directory-application-proxy-custom-domains.md)
* [<span data-ttu-id="46ab5-135">Egyszeri bejelentkezés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="46ab5-135">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="46ab5-136">Az alkalmazásproxy problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="46ab5-136">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)


