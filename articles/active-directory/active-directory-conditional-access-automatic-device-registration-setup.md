---
title: "Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása konfigurálása az Azure Active Directoryval |} Microsoft Docs"
description: "A tartományhoz csatlakoztatott Windows-eszközök beállítása automatikusan és a csendes regisztrálása az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: dccd7df6a5f85df4179c7ea7cfc476cfb57f48c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a><span data-ttu-id="d6f92-103">Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directory konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6f92-103">How to configure automatic registration of Windows domain-joined devices with Azure Active Directory</span></span>

<span data-ttu-id="d6f92-104">Használandó [Azure Active Directory eszközalapú feltételes hozzáférési](active-directory-conditional-access-azure-portal.md), a számítógépek regisztrálni kell az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d6f92-104">To use [Azure Active Directory device-based conditional access](active-directory-conditional-access-azure-portal.md), your computers must be registered with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="d6f92-105">Kaphat regisztrált eszközöket a szervezet használja a [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) a parancsmag a [Azure Active Directory PowerShell-modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="d6f92-105">You can get a list of registered devices in your organization by using the [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in the [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span> 

<span data-ttu-id="d6f92-106">Ez a cikk nyújt a lépéseket a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása konfigurálásához a szervezet Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="d6f92-106">This article provides you with the steps for configuring the automatic registration of Windows domain-joined devices with Azure AD in your organization.</span></span>


<span data-ttu-id="d6f92-107">További információ:</span><span class="sxs-lookup"><span data-stu-id="d6f92-107">For more information about:</span></span>

- <span data-ttu-id="d6f92-108">Feltételes hozzáférés, lásd: [Azure Active Directory eszközalapú feltételes hozzáférési](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d6f92-108">Conditional access, see [Azure Active Directory device-based conditional access](active-directory-conditional-access-azure-portal.md).</span></span> 
- <span data-ttu-id="d6f92-109">Windows 10-eszközöket a munkahelyen és a továbbfejlesztett lép, amikor regisztrál az Azure ad-vel, lásd: [a vállalati Windows 10: eszközök használata munkára](active-directory-azureadjoin-windows10-devices-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d6f92-109">Windows 10 devices in the workplace and the enhanced experiences when registered with Azure AD, see [Windows 10 for the enterprise: Use devices for work](active-directory-azureadjoin-windows10-devices-overview.md).</span></span>
- <span data-ttu-id="d6f92-110">Windows 10 nagyvállalati E3 csomag a CSP-hez, tekintse meg a [CSP áttekintése a Windows 10 nagyvállalati E3 csomag](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).</span><span class="sxs-lookup"><span data-stu-id="d6f92-110">Windows 10 Enterprise E3 in CSP, see the [Windows 10 Enterprise E3 in CSP Overview](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="d6f92-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d6f92-111">Before you begin</span></span>

<span data-ttu-id="d6f92-112">A környezetben a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása konfigurálása előtt tanulmányozza át a támogatott forgatókönyveket és a korlátozásokkal.</span><span class="sxs-lookup"><span data-stu-id="d6f92-112">Before you start configuring the automatic registration of Windows domain-joined devices in your environment, you should familiarize yourself with the supported scenarios and the constraints.</span></span>  

<span data-ttu-id="d6f92-113">A leírások olvashatóságának, ez a témakör a következő kifejezést használja:</span><span class="sxs-lookup"><span data-stu-id="d6f92-113">To improve the readability of the descriptions, this topic uses the following term:</span></span> 

- <span data-ttu-id="d6f92-114">**Aktuális Windows-eszközök** -a Windows 10 vagy Windows Server 2016 rendszert futtató, tartományhoz csatlakoztatott eszközökre vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d6f92-114">**Windows current devices** - This term refers to domain-joined devices running Windows 10 or Windows Server 2016.</span></span>
- <span data-ttu-id="d6f92-115">**Régebbi Windows-eszközök** -e az összes kifejezés **támogatott** tartományhoz csatlakoztatott Windows-eszközök végrehajtott futó Windows 10 és Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="d6f92-115">**Windows down-level devices** - This term refers to all **supported** domain-joined Windows devices that are neither running Windows 10 nor Windows Server 2016.</span></span>  


### <a name="windows-current-devices"></a><span data-ttu-id="d6f92-116">Aktuális Windows-eszközök</span><span class="sxs-lookup"><span data-stu-id="d6f92-116">Windows current devices</span></span>

- <span data-ttu-id="d6f92-117">A Windows asztali operációs rendszert futtató eszközökhöz, azt javasoljuk, Windows 10 évforduló frissítés (verzió: 1607) vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d6f92-117">For devices running the Windows desktop operating system, we recommend using Windows 10 Anniversary Update (version 1607) or later.</span></span> 
- <span data-ttu-id="d6f92-118">Aktuális Windows-eszközök regisztrációjának **van** nem összevont környezetekben, például a jelszó kivonatát szinkronizálási konfiguráció támogatott.</span><span class="sxs-lookup"><span data-stu-id="d6f92-118">The registration of Windows current devices **is** supported in non-federated environments such as password hash sync configurations.</span></span>  


### <a name="windows-down-level-devices"></a><span data-ttu-id="d6f92-119">Régebbi Windows-eszközök</span><span class="sxs-lookup"><span data-stu-id="d6f92-119">Windows down-level devices</span></span>

- <span data-ttu-id="d6f92-120">A következő Windows-kezelés régebbi eszközöket támogatja:</span><span class="sxs-lookup"><span data-stu-id="d6f92-120">The following Windows down-level devices are supported:</span></span>
    - <span data-ttu-id="d6f92-121">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="d6f92-121">Windows 8.1</span></span>
    - <span data-ttu-id="d6f92-122">Windows 7</span><span class="sxs-lookup"><span data-stu-id="d6f92-122">Windows 7</span></span>
    - <span data-ttu-id="d6f92-123">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="d6f92-123">Windows Server 2012 R2</span></span>
    - <span data-ttu-id="d6f92-124">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="d6f92-124">Windows Server 2012</span></span>
    - <span data-ttu-id="d6f92-125">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="d6f92-125">Windows Server 2008 R2</span></span>
- <span data-ttu-id="d6f92-126">A régebbi Windows-eszközök regisztrálását **van** keresztül zökkenőmentes egyszeri bejelentkezés nem összevont környezetekben támogatott [Azure Active Directory zökkenőmentes egyszeri bejelentkezést](https://aka.ms/hybrid/sso).</span><span class="sxs-lookup"><span data-stu-id="d6f92-126">The registration of Windows down-level devices **is** supported in non-federated environments through Seamless Single Sign On [Azure Active Directory Seamless Single Sign-On](https://aka.ms/hybrid/sso).</span></span>
- <span data-ttu-id="d6f92-127">A régebbi Windows-eszközök regisztrálását **nem** eszközök központi profilok használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="d6f92-127">The registration of Windows down-level devices **is not** supported for devices using roaming profiles.</span></span> <span data-ttu-id="d6f92-128">Ha a központi profilok vagy a beállítások, Windows 10 használata biztosítja.</span><span class="sxs-lookup"><span data-stu-id="d6f92-128">If you are relying on roaming of profiles or settings, use Windows 10.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="d6f92-129">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6f92-129">Prerequisites</span></span>

<span data-ttu-id="d6f92-130">A tartományhoz csatlakoztatott eszközök a szervezetben az automatikus-regisztráció engedélyezése előtt győződjön meg arról, hogy futnak-e az Azure AD egy naprakész verzióját kell csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="d6f92-130">Before you start enabling the auto-registration of domain-joined devices in your organization, you need to make sure that you are running an up-to-date version of Azure AD connect.</span></span>

<span data-ttu-id="d6f92-131">Az Azure AD Connect:</span><span class="sxs-lookup"><span data-stu-id="d6f92-131">Azure AD Connect:</span></span>

- <span data-ttu-id="d6f92-132">A számítógépfiók a helyszíni Active Directory (AD) és a eszközobjektumot az Azure AD közötti társítás tartja.</span><span class="sxs-lookup"><span data-stu-id="d6f92-132">Keeps the association between the computer account in your on-premises Active Directory (AD) and the device object in Azure AD.</span></span> 
- <span data-ttu-id="d6f92-133">Lehetővé teszi, hogy más eszköz kapcsolódó szolgáltatások, például a vállalati Windows Hello.</span><span class="sxs-lookup"><span data-stu-id="d6f92-133">Enables other device related features like Windows Hello for Business.</span></span>



## <a name="configuration-steps"></a><span data-ttu-id="d6f92-134">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="d6f92-134">Configuration steps</span></span>

<span data-ttu-id="d6f92-135">Ez a témakör minden tipikus forgatókönyve a szükséges lépéseket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d6f92-135">This topic includes the required steps for all typical configuration scenarios.</span></span>  
<span data-ttu-id="d6f92-136">Az alábbi táblázat segítségével áttekintheti a forgatókönyvhöz szükséges lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d6f92-136">Use the following table to get an overview of the steps that are required for your scenario:</span></span>  



| <span data-ttu-id="d6f92-137">Lépések</span><span class="sxs-lookup"><span data-stu-id="d6f92-137">Steps</span></span>                                      | <span data-ttu-id="d6f92-138">A Windows jelenlegi és a jelszó Jelszókivonat-szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="d6f92-138">Windows current and password hash sync</span></span> | <span data-ttu-id="d6f92-139">A Windows jelenlegi és a következő összevonáshoz</span><span class="sxs-lookup"><span data-stu-id="d6f92-139">Windows current and federation</span></span> | <span data-ttu-id="d6f92-140">Windows-kezelés régebbi</span><span class="sxs-lookup"><span data-stu-id="d6f92-140">Windows down-level</span></span> |
| :--                                        | :-:                                    | :-:                            | :-:                |
| <span data-ttu-id="d6f92-141">1. lépés: A szolgáltatáskapcsolódási pont konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6f92-141">Step 1: Configure service connection point</span></span> | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| <span data-ttu-id="d6f92-145">2. lépés: A jogcímek kiállítási beállítása</span><span class="sxs-lookup"><span data-stu-id="d6f92-145">Step 2: Setup issuance of claims</span></span>           |                                        | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| <span data-ttu-id="d6f92-148">3. lépés: A Windows 10-eszközök engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d6f92-148">Step 3: Enable non-Windows 10 devices</span></span>      |                                        |                                | ![Jelölőnégyzet][1]        |
| <span data-ttu-id="d6f92-150">4. lépés: Központi telepítési és a bevezetés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d6f92-150">Step 4: Control deployment and rollout</span></span>     | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| <span data-ttu-id="d6f92-154">5. lépés: A regisztrált eszközök ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d6f92-154">Step 5: Verify registered devices</span></span>          | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |



## <a name="step-1-configure-service-connection-point"></a><span data-ttu-id="d6f92-158">1. lépés: A szolgáltatáskapcsolódási pont konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6f92-158">Step 1: Configure service connection point</span></span>

<span data-ttu-id="d6f92-159">A szolgáltatás kapcsolódási pontjának (SCP) objektum által az eszközök regisztrálásakor felderítésére szolgál információkat az Azure AD bérlői.</span><span class="sxs-lookup"><span data-stu-id="d6f92-159">The service connection point (SCP) object is used by your devices during the registration to discover Azure AD tenant information.</span></span> <span data-ttu-id="d6f92-160">A helyszíni Active Directoryban (AD) a szolgáltatáskapcsolódási pont objektumot a tartományhoz csatlakozó eszközök automatikus regisztrációja konfigurációs környezet partíciójára a számítógép erdő léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="d6f92-160">In your on-premises Active Directory (AD), the SCP object for the auto-registration of domain-joined devices must exist in the configuration naming context partition of the computer's forest.</span></span> <span data-ttu-id="d6f92-161">Csak egy konfigurációs névhasználati környezet minden erdőre van.</span><span class="sxs-lookup"><span data-stu-id="d6f92-161">There is only one configuration naming context per forest.</span></span> <span data-ttu-id="d6f92-162">Többerdős Active Directory-konfiguráció esetén a szolgáltatáskapcsolódási pont léteznie kell az összes olyan erdőben, a tartományhoz csatlakoztatott számítógépeket tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="d6f92-162">In a multi-forest Active Directory configuration, the service connection point must exist in all forests containing domain-joined computers.</span></span>

<span data-ttu-id="d6f92-163">Használhatja a [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) parancsmag használatával kérhet le az erdő konfigurációelnevezési környezetében.</span><span class="sxs-lookup"><span data-stu-id="d6f92-163">You can use the [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet to retrieve the configuration naming context of your forest.</span></span>  

<span data-ttu-id="d6f92-164">Az Active Directory tartományi nevű erdő *fabrikam.com*, konfigurációelnevezési környezetében van:</span><span class="sxs-lookup"><span data-stu-id="d6f92-164">For a forest with the Active Directory domain name *fabrikam.com*, the configuration naming context is:</span></span>

`CN=Configuration,DC=fabrikam,DC=com`

<span data-ttu-id="d6f92-165">Az erdőben az SCP objektumot a tartományhoz csatlakozó eszközök automatikus regisztrációja a következő helyen található:</span><span class="sxs-lookup"><span data-stu-id="d6f92-165">In your forest, the SCP object for the auto-registration of domain-joined devices is located at:</span></span>  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

<span data-ttu-id="d6f92-166">Attól függően, hogy telepítette az Azure AD Connect az SCP objektum lehet, hogy már meg vannak adva.</span><span class="sxs-lookup"><span data-stu-id="d6f92-166">Depending on how you have deployed Azure AD Connect, the SCP object may have already been configured.</span></span>
<span data-ttu-id="d6f92-167">Ellenőrizze az objektum létezését, és a következő Windows PowerShell-parancsfájl használatával felderítési értékeinek beolvasásához:</span><span class="sxs-lookup"><span data-stu-id="d6f92-167">You can verify the existence of the object and retrieve the discovery values using the following Windows PowerShell script:</span></span> 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

<span data-ttu-id="d6f92-168">A **$scp. Kulcsszavak** az alábbiakat mutatja be az Azure AD bérlői kapcsolatos információkat, például:</span><span class="sxs-lookup"><span data-stu-id="d6f92-168">The **$scp.Keywords** output shows the Azure AD tenant information, for example:</span></span>

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

<span data-ttu-id="d6f92-169">Ha a szolgáltatáskapcsolódási pont nem létezik, létrehozhatja futtatásával a `Initialize-ADSyncDomainJoinedComputerSync` parancsmag az Azure AD Connect-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d6f92-169">If the service connection point does not exist, you can create it by running the `Initialize-ADSyncDomainJoinedComputerSync` cmdlet on your Azure AD Connect server.</span></span> <span data-ttu-id="d6f92-170">Ez a parancsmag futtatásához szükséges vállalati rendszergazda hitelesítő adatai.</span><span class="sxs-lookup"><span data-stu-id="d6f92-170">Enterprise admin credential is required to run this cmdlet.</span></span>  
<span data-ttu-id="d6f92-171">A parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="d6f92-171">The cmdlet:</span></span>

- <span data-ttu-id="d6f92-172">Az Active Directory-erdőben, az Azure AD Connect csatlakozik-e a szolgáltatáskapcsolódási pontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d6f92-172">Creates the service connection point in the Active Directory forest Azure AD Connect is connected to.</span></span> 
- <span data-ttu-id="d6f92-173">Meg kell adja meg a `AdConnectorAccount` paraméter.</span><span class="sxs-lookup"><span data-stu-id="d6f92-173">Requires you to specify the `AdConnectorAccount` parameter.</span></span> <span data-ttu-id="d6f92-174">Ez az a fiók, amely az Active Directory connector fiók az Azure AD connect van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="d6f92-174">This is the account that is configured as Active Directory connector account in Azure AD connect.</span></span> 


<span data-ttu-id="d6f92-175">Az alábbi parancsfájlt mutat be a parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="d6f92-175">The following script shows an example for using the cmdlet.</span></span> <span data-ttu-id="d6f92-176">Ezt a parancsfájlt a `$aadAdminCred = Get-Credential` meg kell írjon be egy felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="d6f92-176">In this script, `$aadAdminCred = Get-Credential` requires you to type a user name.</span></span> <span data-ttu-id="d6f92-177">Meg kell adnia a felhasználónevet, a felhasználó egyszerű felhasználónév (UPN) formátumban (`user@example.com`).</span><span class="sxs-lookup"><span data-stu-id="d6f92-177">You need to provide the user name in the user principal name (UPN) format (`user@example.com`).</span></span> 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

<span data-ttu-id="d6f92-178">A `Initialize-ADSyncDomainJoinedComputerSync` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="d6f92-178">The `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span></span>

- <span data-ttu-id="d6f92-179">Használja az Active Directory PowerShell modult, amely a tartományvezérlőn futó Active Directory webszolgáltatások támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="d6f92-179">Uses the Active Directory PowerShell module, which relies on Active Directory Web Services running on a domain controller.</span></span> <span data-ttu-id="d6f92-180">Az Active Directory webszolgáltatások a Windows Server 2008 R2 rendszerű tartományvezérlők és újabb rendszer.</span><span class="sxs-lookup"><span data-stu-id="d6f92-180">Active Directory Web Services is supported on domain controllers running Windows Server 2008 R2 and later.</span></span>
- <span data-ttu-id="d6f92-181">Csak akkor támogatott a **MSOnline PowerShell modul verziója 1.1.166.0**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-181">Is only supported by the **MSOnline PowerShell module version 1.1.166.0**.</span></span> <span data-ttu-id="d6f92-182">Ez a modul letöltéséhez használjon ez [hivatkozás](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span><span class="sxs-lookup"><span data-stu-id="d6f92-182">To download this module, use this [link](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span></span>   

<span data-ttu-id="d6f92-183">A Windows Server 2008 vagy korábbi verzióit futtató tartományvezérlők a szolgáltatáskapcsolódási pont létrehozásához használja az alábbi parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d6f92-183">For domain controllers running Windows Server 2008 or earlier versions, use the script below to create the service connection point.</span></span>

<span data-ttu-id="d6f92-184">Többerdős konfiguráció esetén a következő parancsfájlt kell használnia minden olyan erdőben, ahol a számítógépek létezik a szolgáltatáskapcsolódási pont létrehozása:</span><span class="sxs-lookup"><span data-stu-id="d6f92-184">In a multi-forest configuration, you should use the following script to create the service connection point in each forest where computers exist:</span></span>
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC

    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()


## <a name="step-2-setup-issuance-of-claims"></a><span data-ttu-id="d6f92-185">2. lépés: A jogcímek kiállítási beállítása</span><span class="sxs-lookup"><span data-stu-id="d6f92-185">Step 2: Setup issuance of claims</span></span>

<span data-ttu-id="d6f92-186">Az összevont Azure Active Directory beállítása, a eszközök támaszkodnak az Active Directory összevonási szolgáltatások (AD FS) vagy 3. fél a helyi összevonási szolgáltatás az Azure AD felé történő hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="d6f92-186">In a federated Azure AD configuration, devices rely on Active Directory Federation Services (AD FS) or a 3rd party on-premises federation service to authenticate to Azure AD.</span></span> <span data-ttu-id="d6f92-187">Eszközök regisztrálása az Azure Active Directory Eszközregisztrációs szolgáltatás (Azure DRS) szemben olyan hozzáférési jogkivonatot beolvasni hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6f92-187">Devices authenticate to get an access token to register against the Azure Active Directory Device Registration Service (Azure DRS).</span></span>

<span data-ttu-id="d6f92-188">A Windows jelenlegi eszközök hitelesítés integrált Windows-hitelesítés egy aktív WS-Trust végponthoz (1.3 vagy 2005 verzió) a helyi összevonási szolgáltatás által üzemeltetett használatával.</span><span class="sxs-lookup"><span data-stu-id="d6f92-188">Windows current devices authenticate using Integrated Windows Authentication to an active WS-Trust endpoint (either 1.3 or 2005 versions) hosted by the on-premises federation service.</span></span>

> [!NOTE]
> <span data-ttu-id="d6f92-189">AD FS-ben vagy használatakor **adfs/services/megbízhatósági/13/windowstransport** vagy **adfs/services/megbízhatósági/2005/windowstransport** engedélyezve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d6f92-189">When using AD FS, either **adfs/services/trust/13/windowstransport** or **adfs/services/trust/2005/windowstransport** must be enabled.</span></span> <span data-ttu-id="d6f92-190">Ha a hitelesítési Webproxyt használ, bizonyosodjon meg arról, hogy ezt a végpontot a proxy használatával van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="d6f92-190">If you are using the Web Authentication Proxy, also ensure that this endpoint is published through the proxy.</span></span> <span data-ttu-id="d6f92-191">Láthatja, hogy mely végpontokat engedélyezve vannak a az AD FS felügyeleti konzolon keresztül **szolgáltatás > végpontok**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-191">You can see what end-points are enabled through the AD FS management console under **Service > Endpoints**.</span></span>
>
><span data-ttu-id="d6f92-192">Ha a helyi összevonási szolgáltatás AD FS nem rendelkezik, az utasítások a szállító győződjön meg arról, hogy a WS-Trust 1.3 vagy 2005 végpontjainak és, hogy ezek a metaadatok Exchange fájlt (MEX) keresztül közzétett támogatják.</span><span class="sxs-lookup"><span data-stu-id="d6f92-192">If you don’t have AD FS as your on-premises federation service, follow the instructions of your vendor to make sure they support WS-Trust 1.3 or 2005 end-points and that these are published through the Metadata Exchange file (MEX).</span></span>

<span data-ttu-id="d6f92-193">A következő jogcímeket befejezéséhez az eszközregisztrációhoz tartozó Azure DRS által kapott jogkivonat léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="d6f92-193">The following claims must exist in the token received by Azure DRS for device registration to complete.</span></span> <span data-ttu-id="d6f92-194">Azure DRS egy eszközobjektumot hoz létre az Azure AD Connect objektumokkal való társítás az újonnan létrehozott eszköz a számítógép fiók a helyi majd használt információk az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d6f92-194">Azure DRS will create a device object in Azure AD with some of this information which is then used by Azure AD Connect to associate the newly created device object with the computer account on-premises.</span></span>

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

<span data-ttu-id="d6f92-195">Ha több ellenőrzött tartomány nevét, meg kell adnia a következő jogcímet ad ki a számítógépeket:</span><span class="sxs-lookup"><span data-stu-id="d6f92-195">If you have more than one verified domain name, you need to provide the following claim for computers:</span></span>

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

<span data-ttu-id="d6f92-196">Ha a rendszer már kiállító egy ImmutableID jogcímet (pl. másodlagos bejelentkezési Azonosítóval) meg kell adnia egy megfelelő jogcím számítógépek:</span><span class="sxs-lookup"><span data-stu-id="d6f92-196">If you are already issuing an ImmutableID claim (e.g., alternate login ID) you need to provide one corresponding claim for computers:</span></span>

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

<span data-ttu-id="d6f92-197">A következő szakaszokban azt talál információkat:</span><span class="sxs-lookup"><span data-stu-id="d6f92-197">In the following sections, you find information about:</span></span>
 
- <span data-ttu-id="d6f92-198">Az értékeket az egyes jogcímek kell rendelkeznie</span><span class="sxs-lookup"><span data-stu-id="d6f92-198">The values each claim should have</span></span>
- <span data-ttu-id="d6f92-199">Hogyan definíció jelenne meg az AD FS-ben</span><span class="sxs-lookup"><span data-stu-id="d6f92-199">How a definition would look like in AD FS</span></span>

<span data-ttu-id="d6f92-200">A definíció segítségével győződjön meg arról, hogy találhatók-e az értékeket, vagy ha szeretné-e létre.</span><span class="sxs-lookup"><span data-stu-id="d6f92-200">The definition helps you to verify whether the values are present or if you need to create them.</span></span>

> [!NOTE]
> <span data-ttu-id="d6f92-201">Ha a helyi összevonási kiszolgáló AD FS nem használja, lépésekkel a gyártója által biztosított jogcímek ki a megfelelő konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="d6f92-201">If you don’t use AD FS for your on-premises federation server, follow your vendor's instructions to create the appropriate configuration to issue these claims.</span></span>

### <a name="issue-account-type-claim"></a><span data-ttu-id="d6f92-202">A probléma fiók típusa jogcím</span><span class="sxs-lookup"><span data-stu-id="d6f92-202">Issue account type claim</span></span>

<span data-ttu-id="d6f92-203">**`http://schemas.microsoft.com/ws/2012/01/accounttype`**– Ezt az igényt az értéket kell tartalmaznia, **DJ**, amely azonosítja, hogy az eszközt egy tartományhoz csatlakozó számítógép.</span><span class="sxs-lookup"><span data-stu-id="d6f92-203">**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - This claim must contain a value of **DJ**, which identifies the device as a domain-joined computer.</span></span> <span data-ttu-id="d6f92-204">Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="d6f92-204">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a><span data-ttu-id="d6f92-205">A számítógép fiók helyszínen probléma objectGUID</span><span class="sxs-lookup"><span data-stu-id="d6f92-205">Issue objectGUID of the computer account on-premises</span></span>

<span data-ttu-id="d6f92-206">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**-A jogcím tartalmaznia kell a **objectGUID** értéket a helyi fiók.</span><span class="sxs-lookup"><span data-stu-id="d6f92-206">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - This claim must contain the **objectGUID** value of the on-premises computer account.</span></span> <span data-ttu-id="d6f92-207">Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="d6f92-207">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a><span data-ttu-id="d6f92-208">A számítógép fiók helyszínen probléma objectSID</span><span class="sxs-lookup"><span data-stu-id="d6f92-208">Issue objectSID of the computer account on-premises</span></span>

<span data-ttu-id="d6f92-209">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**-A jogcím tartalmaznia kell a a **objectSid** értéket a helyi fiók.</span><span class="sxs-lookup"><span data-stu-id="d6f92-209">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - This claim must contain the the **objectSid** value of the on-premises computer account.</span></span> <span data-ttu-id="d6f92-210">Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="d6f92-210">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a><span data-ttu-id="d6f92-211">Számítógép issuerID kibocsátani, ha több tartománynév ellenőrzése az Azure ad-ben</span><span class="sxs-lookup"><span data-stu-id="d6f92-211">Issue issuerID for computer when multiple verified domain names in Azure AD</span></span>

<span data-ttu-id="d6f92-212">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**– A jogcím az ellenőrzött tartomány nevét, amelyek kapcsolódnak a helyszíni összevonási szolgáltatással (AD FS vagy 3. fél) bármelyikének egységes erőforrás azonosítója (URI) kell tartalmaznia a jogkivonatot kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="d6f92-212">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - This claim must contain the Uniform Resource Identifier (URI) of any of the verified domain names that connect with the on-premises federation service (AD FS or 3rd party) issuing the token.</span></span> <span data-ttu-id="d6f92-213">Az AD FS-ben a kiadás átalakítási szabályai, hasonló meghatározott sorrendben alatt megfelelően fenti megfelelően után is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="d6f92-213">In AD FS, you can add issuance transform rules that look like the ones below in that specific order after the ones above.</span></span> <span data-ttu-id="d6f92-214">Ne feledje, hogy egy szabály kifejezetten ki a szabály a felhasználók számára a szükséges.</span><span class="sxs-lookup"><span data-stu-id="d6f92-214">Please note that one rule to explicitly issue the rule for users is necessary.</span></span> <span data-ttu-id="d6f92-215">Az alábbi szabályokkal szemben, a felhasználók és számítógépek hitelesítését azonosító első szabály jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d6f92-215">In the rules below, a first rule identifying user vs. computer authentication is added.</span></span>

    @RuleName = "Issue account type with the value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


<span data-ttu-id="d6f92-216">A fenti, jogcímek</span><span class="sxs-lookup"><span data-stu-id="d6f92-216">In the claim above,</span></span>

- <span data-ttu-id="d6f92-217">`$<domain>`az AD FS szolgáltatás URL-címe</span><span class="sxs-lookup"><span data-stu-id="d6f92-217">`$<domain>` is the AD FS service URL</span></span>
- <span data-ttu-id="d6f92-218">`<verified-domain-name>`ki kell cserélni az egyik az ellenőrzött tartomány nevét az Azure AD helyőrző</span><span class="sxs-lookup"><span data-stu-id="d6f92-218">`<verified-domain-name>` is a placeholder you need to replace with one of your verified domain names in Azure AD</span></span>



<span data-ttu-id="d6f92-219">Ellenőrzött tartomány nevét kapcsolatos további tudnivalókért lásd: [egy egyéni tartománynév hozzáadása az Azure Active Directory](active-directory-add-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d6f92-219">For more details about verified domain names, see [Add a custom domain name to Azure Active Directory](active-directory-add-domain.md).</span></span>  
<span data-ttu-id="d6f92-220">A vállalat ellenőrzött tartományok listájának lekéréséhez használja a [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d6f92-220">To get a list of your verified company domains, you can use the [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span></span> 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a><span data-ttu-id="d6f92-222">Számítógép ImmutableID ki, ha egy, a felhasználók létező (pl. másodlagos bejelentkezési azonosító beállítása)</span><span class="sxs-lookup"><span data-stu-id="d6f92-222">Issue ImmutableID for computer when one for users exist (e.g. alternate login ID is set)</span></span>

<span data-ttu-id="d6f92-223">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**-A jogcím számítógépek érvényes értéket kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="d6f92-223">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - This claim must contain a valid value for computers.</span></span> <span data-ttu-id="d6f92-224">Az AD FS-ben is létrehozhat egy kiadási átalakítási szabálykészlet bocsát az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d6f92-224">In AD FS, you can create an issuance transform rule as follows:</span></span>

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a><span data-ttu-id="d6f92-225">Az AD FS kiállítási átalakítási szabályok létrehozásához segítő parancsfájl</span><span class="sxs-lookup"><span data-stu-id="d6f92-225">Helper script to create the AD FS issuance transform rules</span></span>

<span data-ttu-id="d6f92-226">A következő parancsfájl segítségével létrehozását a kiállítási átalakítási szabályok a fent leírt.</span><span class="sxs-lookup"><span data-stu-id="d6f92-226">The following script helps you with the creation of the issuance transform rules described above.</span></span>

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a><span data-ttu-id="d6f92-227">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d6f92-227">Remarks</span></span> 

- <span data-ttu-id="d6f92-228">Ezt a parancsfájlt a szabályok hozzáfűzi a meglévő szabályok.</span><span class="sxs-lookup"><span data-stu-id="d6f92-228">This script appends the rules to the existing rules.</span></span> <span data-ttu-id="d6f92-229">Ne futtassa a parancsfájlt kétszer mivel szabálykészlet kétszer volna adni.</span><span class="sxs-lookup"><span data-stu-id="d6f92-229">Do not run the script twice because the set of rules would be added twice.</span></span> <span data-ttu-id="d6f92-230">Győződjön meg arról, hogy nem megfelelő szabályok tartoznak ezeket a jogcímeket (megfelelő feltételekkel) a parancsfájl ismételt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="d6f92-230">Make sure that no corresponding rules exist for these claims (under the corresponding conditions) before running the script again.</span></span>

- <span data-ttu-id="d6f92-231">Ha több ellenőrzött tartomány nevét (ahogy az Azure AD portálon vagy a Get-MsolDomains parancsmag segítségével), állítsa be a **$multipleVerifiedDomainNames** a parancsfájl **$true**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-231">If you have multiple verified domain names (as shown in the Azure AD portal or via the Get-MsolDomains cmdlet), set the value of **$multipleVerifiedDomainNames** in the script to **$true**.</span></span> <span data-ttu-id="d6f92-232">Győződjön meg arról is, hogy távolítsa el a meglévő issuerid jogcím létrehozott előfordulhat, hogy az Azure AD Connect vagy más eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="d6f92-232">Also make sure that you remove any existing issuerid claim that might have been created by Azure AD Connect or via other means.</span></span> <span data-ttu-id="d6f92-233">Íme egy példa a ehhez a szabályhoz:</span><span class="sxs-lookup"><span data-stu-id="d6f92-233">Here is an example for this rule:</span></span>


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- <span data-ttu-id="d6f92-234">Ha már kiadott egy **ImmutableID** vonatkozó felhasználói fiókok, állítsa be a **$immutableIDAlreadyIssuedforUsers** a parancsfájl **$true**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-234">If you have already issued an **ImmutableID** claim  for user accounts, set the value of **$immutableIDAlreadyIssuedforUsers** in the script to **$true**.</span></span>

## <a name="step-3-enable-windows-down-level-devices"></a><span data-ttu-id="d6f92-235">3. lépés: Engedélyezze a régebbi Windows-eszközök</span><span class="sxs-lookup"><span data-stu-id="d6f92-235">Step 3: Enable Windows down-level devices</span></span>

<span data-ttu-id="d6f92-236">Ha néhány, a tartományhoz csatlakoztatott eszközök a Windows-kezelés régebbi eszközöket, akkor:</span><span class="sxs-lookup"><span data-stu-id="d6f92-236">If some of your domain-joined devices Windows down-level devices, you need to:</span></span>

- <span data-ttu-id="d6f92-237">Házirend beállítása az Azure AD-engedélyezése a felhasználóknak, hogy regisztrálják eszközeiket.</span><span class="sxs-lookup"><span data-stu-id="d6f92-237">Set a policy in Azure AD to enable users to register devices.</span></span>
 
- <span data-ttu-id="d6f92-238">Konfigurálja a helyi összevonási szolgáltatás támogatása jogcímek kiállítására **integrált Windows-hitelesítéssel (IWA)** az eszközregisztrációhoz tartozó.</span><span class="sxs-lookup"><span data-stu-id="d6f92-238">Configure your on-premises federation service to issue claims to support **Integrated Windows Authentication (IWA)** for device registration.</span></span>
 
- <span data-ttu-id="d6f92-239">Az eszköz az Azure AD hitelesítési végpont hozzáadása a Helyi Intranet zóna tanúsítványokra elkerülésére, ha az eszköz hitelesítésére.</span><span class="sxs-lookup"><span data-stu-id="d6f92-239">Add the Azure AD device authentication end-point to the local Intranet zones to avoid certificate prompts when authenticating the device.</span></span>

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a><span data-ttu-id="d6f92-240">Házirend beállítása az Azure AD-eszközök regisztrálása a felhasználók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d6f92-240">Set policy in Azure AD to enable users to register devices</span></span>

<span data-ttu-id="d6f92-241">Régebbi Windows-eszközök regisztrálásához szükséges győződjön meg arról, hogy a felhasználók regisztrálják az eszközeiket az Azure AD beállítása.</span><span class="sxs-lookup"><span data-stu-id="d6f92-241">To register Windows down-level devices, you need to make sure that the setting to allow users to register devices in Azure AD is set.</span></span> <span data-ttu-id="d6f92-242">Az Azure-portálon találja meg a beállítás:</span><span class="sxs-lookup"><span data-stu-id="d6f92-242">In the Azure portal, you can find this setting under:</span></span>

`Azure Active Directory > Users and groups > Device settings`
    
<span data-ttu-id="d6f92-243">A következő házirend értékre kell állítani **minden**: **felhasználók előfordulhat, hogy regisztrálják az eszközeiket az Azure ad szolgáltatással**</span><span class="sxs-lookup"><span data-stu-id="d6f92-243">The following policy must be set to **All**: **Users may register their devices with Azure AD**</span></span>

![Eszközök regisztrálása](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a><span data-ttu-id="d6f92-245">A helyi összevonási szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6f92-245">Configure on-premises federation service</span></span> 

<span data-ttu-id="d6f92-246">A helyi összevonási szolgáltatás támogatnia kell a kiállító a **authenticationmehod** és **wiaormultiauthn** jogcímeket a függő entitás tekintetében az Azure AD, kódolt érték resouce_params paraméterrel rendelkező alább látható módon a hitelesítési kérelem fogadása közben:</span><span class="sxs-lookup"><span data-stu-id="d6f92-246">Your on-premises federation service must support issuing the **authenticationmehod** and **wiaormultiauthn** claims when receiving an authentication request to the Azure AD relying party holding a resouce_params parameter with an encoded value as shown below:</span></span>

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

<span data-ttu-id="d6f92-247">Ha ilyen kérelem érkezik, a helyi összevonási szolgáltatás kell hitelesíteni a felhasználót, integrált Windows-hitelesítés használatával, és sikeres, akkor a következő két jogcímek kiadja:</span><span class="sxs-lookup"><span data-stu-id="d6f92-247">When such a request comes, the on-premises federation service must authenticate the user using Integrated Windows Authentication and upon success, it must issue the following two claims:</span></span>

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

<span data-ttu-id="d6f92-248">Az AD FS-ben hozzá kell adnia egy kiadási átalakítási szabálykészlet, amely folyamaton keresztül a hitelesítési módszert.</span><span class="sxs-lookup"><span data-stu-id="d6f92-248">In AD FS, you must add an issuance transform rule that passes-through the authentication method.</span></span>  

<span data-ttu-id="d6f92-249">**Ez a szabály hozzáadása:**</span><span class="sxs-lookup"><span data-stu-id="d6f92-249">**To add this rule:**</span></span>

1. <span data-ttu-id="d6f92-250">A az AD FS felügyeleti konzolon váltson `AD FS > Trust Relationships > Relying Party Trusts`.</span><span class="sxs-lookup"><span data-stu-id="d6f92-250">In the AD FS management console, go to `AD FS > Trust Relationships > Relying Party Trusts`.</span></span>
2. <span data-ttu-id="d6f92-251">Kattintson a jobb gombbal a Microsoft Office 365 Identitásplatformmal függő entitás megbízhatósági objektum, majd válassza ki **Jogcímszabályok szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-251">Right-click the Microsoft Office 365 Identity Platform relying party trust object, and then select **Edit Claim Rules**.</span></span>
3. <span data-ttu-id="d6f92-252">Az a **kiadás átalakítási szabályai** lapon jelölje be **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-252">On the **Issuance Transform Rules** tab, select **Add Rule**.</span></span>
4. <span data-ttu-id="d6f92-253">Az a **jogcímszabály** sablon listáról válassza ki **jogcímek küldése egyéni szabály segítségével**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-253">In the **Claim rule** template list, select **Send Claims Using a Custom Rule**.</span></span>
5. <span data-ttu-id="d6f92-254">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-254">Select **Next**.</span></span>
6. <span data-ttu-id="d6f92-255">Az a **Jogcímszabály nevének** mezőbe írja be **hitelesítési módszer Jogcímszabály**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-255">In the **Claim rule name** box, type **Auth Method Claim Rule**.</span></span>
7. <span data-ttu-id="d6f92-256">Az a **jogcímszabály** mezőbe írja be a következő szabályt:</span><span class="sxs-lookup"><span data-stu-id="d6f92-256">In the **Claim rule** box, type the following rule:</span></span>

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. <span data-ttu-id="d6f92-257">Az összevonási kiszolgálón, írja be az alábbi PowerShell-paranccsal cseréje után  **\<RPObjectName\>**  az Azure AD függő entitás megbízhatósági objektum a függő entitás objektum névvel.</span><span class="sxs-lookup"><span data-stu-id="d6f92-257">On your federation server, type the PowerShell command below after replacing **\<RPObjectName\>** with the relying party object name for your Azure AD relying party trust object.</span></span> <span data-ttu-id="d6f92-258">Ez az objektum neve általában **Microsoft Office 365 Identitásplatformmal**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-258">This object usually is named **Microsoft Office 365 Identity Platform**.</span></span>
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a><span data-ttu-id="d6f92-259">Az eszköz az Azure AD hitelesítési végpont hozzáadása a Helyi Intranet zóna</span><span class="sxs-lookup"><span data-stu-id="d6f92-259">Add the Azure AD device authentication end-point to the Local Intranet zones</span></span>

<span data-ttu-id="d6f92-260">Tanúsítvány elkerülése érdekében felkéri a felhasználók az eszközök regisztrálása az Azure AD-házirend leküldése a tartományhoz csatlakozó eszközök a következő URL-cím hozzáadása a Helyi Intranet zónához, az Internet Explorer hitelesítéshez:</span><span class="sxs-lookup"><span data-stu-id="d6f92-260">To avoid certificate prompts when users in register devices authenticate to Azure AD you can push a policy to your domain-joined devices to add the following URL to the Local Intranet zone in Internet Explorer:</span></span>

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a><span data-ttu-id="d6f92-261">4. lépés: Központi telepítési és a bevezetés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d6f92-261">Step 4: Control deployment and rollout</span></span>

<span data-ttu-id="d6f92-262">Befejezése után végezze el a szükséges lépéseket, a tartományhoz csatlakoztatott eszközök készen áll az Azure AD való automatikus regisztrációt.</span><span class="sxs-lookup"><span data-stu-id="d6f92-262">When you have completed the required steps, domain-joined devices are ready to automatically register with Azure AD.</span></span> <span data-ttu-id="d6f92-263">Minden tartományhoz csatlakozó eszközök a Windows 10 évforduló Update és Windows Server 2016 rendszert futtató automatikusan regisztrálja az Azure AD-eszközön indítsa újra, vagy felhasználói bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="d6f92-263">All domain-joined devices running Windows 10 Anniversary Update and Windows Server 2016 automatically register with Azure AD at device restart or user sign-in.</span></span> <span data-ttu-id="d6f92-264">Új eszközök regisztrálása az Azure AD, ha az eszköz újraindul, a tartományhoz csatlakoztatás művelet befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="d6f92-264">New devices register with Azure AD when the device restarts after the domain join operation is completed.</span></span>

<span data-ttu-id="d6f92-265">Eszközök, melyeket korábban munkahelyhez áttérni az Azure AD (például az Intune-hoz) "*tartományhoz csatlakoztatott, aad-ben regisztrált*"; azonban a folyamat minden eszközön miatt a normál folyamat tartomány és a felhasználói tevékenység befejezése némi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="d6f92-265">Devices that were previously workplace-joined to Azure AD (for example for Intune) transition to “*Domain Joined, AAD Registered*”; however it takes some time for this process to complete across all devices due to the normal flow of domain and user activity.</span></span>

### <a name="remarks"></a><span data-ttu-id="d6f92-266">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d6f92-266">Remarks</span></span>

- <span data-ttu-id="d6f92-267">Csoportházirend-objektum segítségével szabályozhatja a Windows 10 és Windows Server 2016 tartományhoz csatlakoztatott számítógépekre az automatikus regisztráció bevezetésének.</span><span class="sxs-lookup"><span data-stu-id="d6f92-267">You can use a Group Policy object to control the rollout of automatic registration of Windows 10 and Windows Server 2016 domain-joined computers.</span></span>

- <span data-ttu-id="d6f92-268">Windows 10 2015. November automatikus frissítés regiszterekben az Azure ad-val **csak** Ha a bevezetés csoportházirend-objektum be van állítva.</span><span class="sxs-lookup"><span data-stu-id="d6f92-268">Windows 10 November 2015 Update automatically registers with Azure AD **only** if the rollout Group Policy object is set.</span></span>

- <span data-ttu-id="d6f92-269">Bevezetés a Windows-kezelés régebbi rendszerű számítógépek automatikus regisztráció, központilag telepítheti egy [Windows Installer-csomag](#windows-installer-packages-for-non-windows-10-computers) a kiválasztott számítógépekre.</span><span class="sxs-lookup"><span data-stu-id="d6f92-269">To rollout the automatic registration of Windows down-level computers, you can deploy a [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) to computers that you select.</span></span>

- <span data-ttu-id="d6f92-270">Ha Windows 8.1-tartományhoz csatlakoztatott eszközökre küldje le a csoportházirend-objektumot, regisztrációs megpróbálkozik az erdőfelderítéssel; azt ajánljuk, hogy használja a [Windows Installer-csomag](#windows-installer-packages-for-non-windows-10-computers) a Windows-kezelés régebbi eszközöket regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="d6f92-270">If you push the Group Policy object to Windows 8.1 domain-joined devices, registration will be attempted; however it is recommended that you use the [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) to register all your Windows down-level devices.</span></span> 

### <a name="create-a-group-policy-object"></a><span data-ttu-id="d6f92-271">A csoportházirend-objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6f92-271">Create a Group Policy object</span></span> 

<span data-ttu-id="d6f92-272">A szabályozáshoz automatikus regisztráció a jelenlegi Windows-számítógépek, telepítenie kell a **eszközként regisztrálja a tartományhoz csatlakozó számítógépek** csoportházirend-objektumot a regisztrálni kívánt eszközöket.</span><span class="sxs-lookup"><span data-stu-id="d6f92-272">To control the rollout of automatic registration of Windows current computers, you should deploy the **Register domain-joined computers as devices** Group Policy object to the devices you want to register.</span></span> <span data-ttu-id="d6f92-273">Telepíthet például a házirend kapcsolását egy szervezeti egységhez vagy a biztonsági csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="d6f92-273">For example, you can deploy the policy to an organizational unit or to a security group.</span></span>

<span data-ttu-id="d6f92-274">**A házirend beállítása:**</span><span class="sxs-lookup"><span data-stu-id="d6f92-274">**To set the policy:**</span></span>

1. <span data-ttu-id="d6f92-275">Nyissa meg **Kiszolgálókezelő**, majd lépjen `Tools > Group Policy Management`.</span><span class="sxs-lookup"><span data-stu-id="d6f92-275">Open **Server Manager**, and then go to `Tools > Group Policy Management`.</span></span>
2. <span data-ttu-id="d6f92-276">Nyissa meg a tartomány csomópontot, amely megfelel a tartományhoz, ahol szeretné aktiválni automatikus regisztráció a jelenlegi Windows-számítógépek.</span><span class="sxs-lookup"><span data-stu-id="d6f92-276">Go to the domain node that corresponds to the domain where you want to activate auto-registration of Windows current computers.</span></span>
3. <span data-ttu-id="d6f92-277">Kattintson a jobb gombbal **csoportházirend-objektumok**, majd válassza ki **új**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-277">Right-click **Group Policy Objects**, and then select **New**.</span></span>
4. <span data-ttu-id="d6f92-278">Írja be a csoportházirend-objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="d6f92-278">Type a name for your Group Policy object.</span></span> <span data-ttu-id="d6f92-279">Például *az automatikus regisztráció az Azure AD*.</span><span class="sxs-lookup"><span data-stu-id="d6f92-279">For example, *Automatic Registration to Azure AD*.</span></span> <span data-ttu-id="d6f92-280">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6f92-280">Select **OK**.</span></span>
5. <span data-ttu-id="d6f92-281">Kattintson a jobb gombbal az új csoportházirend-objektumot, majd válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-281">Right-click your new Group Policy object, and then select **Edit**.</span></span>
6. <span data-ttu-id="d6f92-282">Ugrás a **számítógép konfigurációja** > **házirendek** > **felügyeleti sablonok** > **Windows-összetevők** > **Eszközregisztráció**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-282">Go to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Device Registration**.</span></span> <span data-ttu-id="d6f92-283">Kattintson a jobb gombbal **eszközként regisztrálja a tartományhoz csatlakozó számítógépek**, majd válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-283">Right-click **Register domain-joined computers as devices**, and then select **Edit**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d6f92-284">Ez a csoportházirend-sablon át lett nevezve a Csoportházirend kezelése konzol korábbi verzióihoz képest.</span><span class="sxs-lookup"><span data-stu-id="d6f92-284">This Group Policy template has been renamed from earlier versions of the Group Policy Management console.</span></span> <span data-ttu-id="d6f92-285">Egy korábbi verzióját a konzol használatakor Ugrás `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span><span class="sxs-lookup"><span data-stu-id="d6f92-285">If you are using an earlier version of the console, go to `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span></span> 

7. <span data-ttu-id="d6f92-286">Válassza ki **engedélyezve**, majd válassza ki **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-286">Select **Enabled**, and then select **Apply**.</span></span>
8. <span data-ttu-id="d6f92-287">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6f92-287">Select **OK**.</span></span>
9. <span data-ttu-id="d6f92-288">A csoportházirend-objektum csatolása a megfelelő helyre.</span><span class="sxs-lookup"><span data-stu-id="d6f92-288">Link the Group Policy object to a location of your choice.</span></span> <span data-ttu-id="d6f92-289">Például társíthatja azt egy adott szervezeti egység.</span><span class="sxs-lookup"><span data-stu-id="d6f92-289">For example, you can link it to a specific organizational unit.</span></span> <span data-ttu-id="d6f92-290">Is sikerült csatolható egy adott biztonsági számítógépek csoportja, amelyek automatikusan regisztrálja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="d6f92-290">You also could link it to a specific security group of computers that automatically register with Azure AD.</span></span> <span data-ttu-id="d6f92-291">Az ezzel a házirend-beállítását minden tartományhoz csatlakoztatott Windows 10 és Windows Server 2016 a szervezet számítógépeire, kapcsolja a csoportházirend-objektumot a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="d6f92-291">To set this policy for all domain-joined Windows 10 and Windows Server 2016 computers in your organization, link the Group Policy object to the domain.</span></span>

### <a name="windows-installer-packages-for-non-windows-10-computers"></a><span data-ttu-id="d6f92-292">A Windows 10 számítógépek Windows Installer-csomag</span><span class="sxs-lookup"><span data-stu-id="d6f92-292">Windows Installer packages for non-Windows 10 computers</span></span>

<span data-ttu-id="d6f92-293">Tartományhoz csatlakoztatott Windows régebbi-számítógépeket regisztrálhat egy összevont környezetben, töltse le és telepítse a Windows Installer-csomag (.msi) letöltőközpontján a [Microsoft munkahelyi csatlakoztatás Windows 10 számítógépek](https://www.microsoft.com/en-us/download/details.aspx?id=53554) lap.</span><span class="sxs-lookup"><span data-stu-id="d6f92-293">To register domain-joined Windows down-level computers in a federated environment, you can download and install these Windows Installer package (.msi) from Download Center at the [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) page.</span></span>

<span data-ttu-id="d6f92-294">A csomagot a szoftverek terjesztési rendszer például System Center Configuration Manager segítségével telepíthet.</span><span class="sxs-lookup"><span data-stu-id="d6f92-294">You can deploy the package by using a software distribution system like System Center Configuration Manager.</span></span> <span data-ttu-id="d6f92-295">A csomag támogatja a szabványos beavatkozás nélküli telepítés beállításainak a *csendes* paraméter.</span><span class="sxs-lookup"><span data-stu-id="d6f92-295">The package supports the standard silent install options with the *quiet* parameter.</span></span> <span data-ttu-id="d6f92-296">A System Center Configuration Manager aktuális ágának további előnyökkel korábbi verzióiról, például a befejezett regisztrációk követését.</span><span class="sxs-lookup"><span data-stu-id="d6f92-296">System Center Configuration Manager Current Branch offers additional benefits from earlier versions, like the ability to track completed registrations.</span></span> <span data-ttu-id="d6f92-297">További információkért lásd: [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="d6f92-297">For more information, see [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span></span>

<span data-ttu-id="d6f92-298">A telepítő ütemezett feladatot hoz létre, amely a felhasználó környezetében fut a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="d6f92-298">The installer creates a scheduled task on the system that runs in the user’s context.</span></span> <span data-ttu-id="d6f92-299">A feladat lesz kiváltva, ha a felhasználó bejelentkezik a Windowsba.</span><span class="sxs-lookup"><span data-stu-id="d6f92-299">The task is triggered when the user signs in to Windows.</span></span> <span data-ttu-id="d6f92-300">A feladat beavatkozás nélkül regisztrálja az eszközt, miután hitelesítése az integrált Windows-hitelesítés használatával a felhasználói hitelesítő adatokat az Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="d6f92-300">The task silently registers the device with Azure AD with the user credentials after authenticating using Integrated Windows Authentication.</span></span> <span data-ttu-id="d6f92-301">Az eszközt, az ütemezett feladat megtekintéséhez lépjen **Microsoft** > **munkahelyi csatlakoztatás**, és folytassa a Feladatütemező könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="d6f92-301">To see the scheduled task, in the device, go to **Microsoft** > **Workplace Join**, and then go to the Task Scheduler library.</span></span>

## <a name="step-5-verify-registered-devices"></a><span data-ttu-id="d6f92-302">5. lépés: A regisztrált eszközök ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d6f92-302">Step 5: Verify registered devices</span></span>

<span data-ttu-id="d6f92-303">Ellenőrizheti a sikeres regisztrált eszközöket a szervezet használatával a [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) a parancsmag a [Azure Active Directory PowerShell-modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="d6f92-303">You can check successful registered devices in your organization by using the [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in the [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span>

<span data-ttu-id="d6f92-304">Ez a parancsmag kimenete az Azure AD-ben regisztrált eszközök jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d6f92-304">The output of this cmdlet shows devices registered in Azure AD.</span></span> <span data-ttu-id="d6f92-305">Minden eszköz használatához a **-minden** paraméter, és majd szűréséhez használja a **deviceTrustType** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d6f92-305">To get all devices, use the **-All** parameter, and then filter them using the **deviceTrustType** property.</span></span> <span data-ttu-id="d6f92-306">Tartományhoz csatlakozó eszközök értéket veheti fel **tartományhoz**.</span><span class="sxs-lookup"><span data-stu-id="d6f92-306">Domain joined devices have a value of **Domain Joined**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6f92-307">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6f92-307">Next steps</span></span>

* [<span data-ttu-id="d6f92-308">Automatikus eszközregisztráció – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="d6f92-308">Automatic device registration FAQ</span></span>](active-directory-device-registration-faq.md)
* [<span data-ttu-id="d6f92-309">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépeit az Azure AD – a Windows 10 és Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="d6f92-309">Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)
* [<span data-ttu-id="d6f92-310">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépeit az Azure AD – Windows 10</span><span class="sxs-lookup"><span data-stu-id="d6f92-310">Troubleshooting auto-registration of domain joined computers to Azure AD – non-Windows 10</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
* [<span data-ttu-id="d6f92-311">Azure Active Directory feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="d6f92-311">Azure Active Directory conditional access</span></span>](active-directory-conditional-access-azure-portal.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
