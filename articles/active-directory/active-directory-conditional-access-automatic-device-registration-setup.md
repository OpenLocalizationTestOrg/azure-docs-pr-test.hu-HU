---
title: "aaaHow tooconfigure automatikus regisztrálása az Azure Active Directoryval Windows tartományhoz csatlakozó eszközök |} Microsoft Docs"
description: "Állítsa be a tartományhoz csatlakoztatott Windows-eszközök tooregister automatikusan és értesítések nélkül történik az Azure Active Directoryban."
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
ms.openlocfilehash: 6a1aab753f5456ed06ba7979ab05f70f29b4ddee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a><span data-ttu-id="26c78-103">Hogyan tooconfigure az automatikus regisztráció Windows tartományhoz csatlakoztatott eszközökre az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="26c78-103">How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory</span></span>

<span data-ttu-id="26c78-104">toouse [Azure Active Directory eszközalapú feltételes hozzáférési](active-directory-conditional-access-azure-portal.md), a számítógépek regisztrálni kell az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="26c78-104">toouse [Azure Active Directory device-based conditional access](active-directory-conditional-access-azure-portal.md), your computers must be registered with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="26c78-105">Hello segítségével a szervezet regisztrált eszközöket is elérhetővé [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) hello parancsmag [Azure Active Directory PowerShell-modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="26c78-105">You can get a list of registered devices in your organization by using hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in hello [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span> 

<span data-ttu-id="26c78-106">Ez a cikk nyújt hello lépéseket a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása hello konfigurálása az Azure AD a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="26c78-106">This article provides you with hello steps for configuring hello automatic registration of Windows domain-joined devices with Azure AD in your organization.</span></span>


<span data-ttu-id="26c78-107">További információ:</span><span class="sxs-lookup"><span data-stu-id="26c78-107">For more information about:</span></span>

- <span data-ttu-id="26c78-108">Feltételes hozzáférés, lásd: [Azure Active Directory eszközalapú feltételes hozzáférési](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="26c78-108">Conditional access, see [Azure Active Directory device-based conditional access](active-directory-conditional-access-azure-portal.md).</span></span> 
- <span data-ttu-id="26c78-109">Windows 10-eszközök hello munkahelyi és fokozott hello lép, amikor regisztrál az Azure ad-vel, lásd: [hello Enterprise Windows 10: eszközök használata munkára](active-directory-azureadjoin-windows10-devices-overview.md).</span><span class="sxs-lookup"><span data-stu-id="26c78-109">Windows 10 devices in hello workplace and hello enhanced experiences when registered with Azure AD, see [Windows 10 for hello enterprise: Use devices for work](active-directory-azureadjoin-windows10-devices-overview.md).</span></span>
- <span data-ttu-id="26c78-110">Windows 10 nagyvállalati E3 csomag a CSP-hez, lásd: hello [CSP áttekintése a Windows 10 nagyvállalati E3 csomag](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).</span><span class="sxs-lookup"><span data-stu-id="26c78-110">Windows 10 Enterprise E3 in CSP, see hello [Windows 10 Enterprise E3 in CSP Overview](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="26c78-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="26c78-111">Before you begin</span></span>

<span data-ttu-id="26c78-112">Mielőtt elkezdené a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása hello konfigurálása a környezetben, tanulmányozza át hello támogatott forgatókönyvek és hello korlátozásokkal.</span><span class="sxs-lookup"><span data-stu-id="26c78-112">Before you start configuring hello automatic registration of Windows domain-joined devices in your environment, you should familiarize yourself with hello supported scenarios and hello constraints.</span></span>  

<span data-ttu-id="26c78-113">tooimprove hello olvashatóságát hello leírásokat, ez a témakör a következő kifejezés hello használja:</span><span class="sxs-lookup"><span data-stu-id="26c78-113">tooimprove hello readability of hello descriptions, this topic uses hello following term:</span></span> 

- <span data-ttu-id="26c78-114">**Aktuális Windows-eszközök** -e kifejezés toodomain csatlakoztatott eszközök a Windows 10 vagy Windows Server 2016 rendszerű.</span><span class="sxs-lookup"><span data-stu-id="26c78-114">**Windows current devices** - This term refers toodomain-joined devices running Windows 10 or Windows Server 2016.</span></span>
- <span data-ttu-id="26c78-115">**Windows-kezelés régebbi eszközök** -e kifejezés tooall **támogatott** tartományhoz csatlakoztatott Windows-eszközök végrehajtott futó Windows 10 és Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="26c78-115">**Windows down-level devices** - This term refers tooall **supported** domain-joined Windows devices that are neither running Windows 10 nor Windows Server 2016.</span></span>  


### <a name="windows-current-devices"></a><span data-ttu-id="26c78-116">Aktuális Windows-eszközök</span><span class="sxs-lookup"><span data-stu-id="26c78-116">Windows current devices</span></span>

- <span data-ttu-id="26c78-117">Hello Windows asztali operációs rendszert futtató eszközök esetén ajánlott a Windows 10 évforduló frissítést (verzió: 1607) vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="26c78-117">For devices running hello Windows desktop operating system, we recommend using Windows 10 Anniversary Update (version 1607) or later.</span></span> 
- <span data-ttu-id="26c78-118">aktuális Windows-eszközök regisztrációja hello **van** nem összevont környezetekben, például a jelszó kivonatát szinkronizálási konfiguráció támogatott.</span><span class="sxs-lookup"><span data-stu-id="26c78-118">hello registration of Windows current devices **is** supported in non-federated environments such as password hash sync configurations.</span></span>  


### <a name="windows-down-level-devices"></a><span data-ttu-id="26c78-119">Régebbi Windows-eszközök</span><span class="sxs-lookup"><span data-stu-id="26c78-119">Windows down-level devices</span></span>

- <span data-ttu-id="26c78-120">a következő Windows-kezelés régebbi eszközök hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="26c78-120">hello following Windows down-level devices are supported:</span></span>
    - <span data-ttu-id="26c78-121">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="26c78-121">Windows 8.1</span></span>
    - <span data-ttu-id="26c78-122">Windows 7</span><span class="sxs-lookup"><span data-stu-id="26c78-122">Windows 7</span></span>
    - <span data-ttu-id="26c78-123">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="26c78-123">Windows Server 2012 R2</span></span>
    - <span data-ttu-id="26c78-124">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="26c78-124">Windows Server 2012</span></span>
    - <span data-ttu-id="26c78-125">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="26c78-125">Windows Server 2008 R2</span></span>
- <span data-ttu-id="26c78-126">Windows-kezelés régebbi eszközök regisztrációja hello **van** keresztül zökkenőmentes egyszeri bejelentkezés nem összevont környezetekben támogatott [Azure Active Directory zökkenőmentes egyszeri bejelentkezést](https://aka.ms/hybrid/sso).</span><span class="sxs-lookup"><span data-stu-id="26c78-126">hello registration of Windows down-level devices **is** supported in non-federated environments through Seamless Single Sign On [Azure Active Directory Seamless Single Sign-On](https://aka.ms/hybrid/sso).</span></span>
- <span data-ttu-id="26c78-127">Windows-kezelés régebbi eszközök regisztrációja hello **nem** eszközök központi profilok használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="26c78-127">hello registration of Windows down-level devices **is not** supported for devices using roaming profiles.</span></span> <span data-ttu-id="26c78-128">Ha a központi profilok vagy a beállítások, Windows 10 használata biztosítja.</span><span class="sxs-lookup"><span data-stu-id="26c78-128">If you are relying on roaming of profiles or settings, use Windows 10.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="26c78-129">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="26c78-129">Prerequisites</span></span>

<span data-ttu-id="26c78-130">Mielőtt engedélyezése hello automatikus regisztráció tartományhoz csatlakoztatott eszközök a szervezetben, szüksége van-e toomake, hogy az Azure AD legfrissebb változata fut-e csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="26c78-130">Before you start enabling hello auto-registration of domain-joined devices in your organization, you need toomake sure that you are running an up-to-date version of Azure AD connect.</span></span>

<span data-ttu-id="26c78-131">Az Azure AD Connect:</span><span class="sxs-lookup"><span data-stu-id="26c78-131">Azure AD Connect:</span></span>

- <span data-ttu-id="26c78-132">A helyszíni Active Directory (AD) és a hello eszközobjektumot az Azure AD hello számítógépfiók hello társítását tartja.</span><span class="sxs-lookup"><span data-stu-id="26c78-132">Keeps hello association between hello computer account in your on-premises Active Directory (AD) and hello device object in Azure AD.</span></span> 
- <span data-ttu-id="26c78-133">Lehetővé teszi, hogy más eszköz kapcsolódó szolgáltatások, például a vállalati Windows Hello.</span><span class="sxs-lookup"><span data-stu-id="26c78-133">Enables other device related features like Windows Hello for Business.</span></span>



## <a name="configuration-steps"></a><span data-ttu-id="26c78-134">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="26c78-134">Configuration steps</span></span>

<span data-ttu-id="26c78-135">Ez a témakör minden tipikus forgatókönyve hello szükséges lépéseket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="26c78-135">This topic includes hello required steps for all typical configuration scenarios.</span></span>  
<span data-ttu-id="26c78-136">A következő tábla tooget áttekintést hello a forgatókönyvhöz szükséges hello használata:</span><span class="sxs-lookup"><span data-stu-id="26c78-136">Use hello following table tooget an overview of hello steps that are required for your scenario:</span></span>  



| <span data-ttu-id="26c78-137">Lépések</span><span class="sxs-lookup"><span data-stu-id="26c78-137">Steps</span></span>                                      | <span data-ttu-id="26c78-138">A Windows jelenlegi és a jelszó Jelszókivonat-szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="26c78-138">Windows current and password hash sync</span></span> | <span data-ttu-id="26c78-139">A Windows jelenlegi és a következő összevonáshoz</span><span class="sxs-lookup"><span data-stu-id="26c78-139">Windows current and federation</span></span> | <span data-ttu-id="26c78-140">Windows-kezelés régebbi</span><span class="sxs-lookup"><span data-stu-id="26c78-140">Windows down-level</span></span> |
| :--                                        | :-:                                    | :-:                            | :-:                |
| <span data-ttu-id="26c78-141">1. lépés: A szolgáltatáskapcsolódási pont konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26c78-141">Step 1: Configure service connection point</span></span> | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| <span data-ttu-id="26c78-145">2. lépés: A jogcímek kiállítási beállítása</span><span class="sxs-lookup"><span data-stu-id="26c78-145">Step 2: Setup issuance of claims</span></span>           |                                        | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| <span data-ttu-id="26c78-148">3. lépés: A Windows 10-eszközök engedélyezése</span><span class="sxs-lookup"><span data-stu-id="26c78-148">Step 3: Enable non-Windows 10 devices</span></span>      |                                        |                                | ![Jelölőnégyzet][1]        |
| <span data-ttu-id="26c78-150">4. lépés: Központi telepítési és a bevezetés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="26c78-150">Step 4: Control deployment and rollout</span></span>     | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |
| <span data-ttu-id="26c78-154">5. lépés: A regisztrált eszközök ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="26c78-154">Step 5: Verify registered devices</span></span>          | ![Jelölőnégyzet][1]                            | ![Jelölőnégyzet][1]                    | ![Jelölőnégyzet][1]        |



## <a name="step-1-configure-service-connection-point"></a><span data-ttu-id="26c78-158">1. lépés: A szolgáltatáskapcsolódási pont konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26c78-158">Step 1: Configure service connection point</span></span>

<span data-ttu-id="26c78-159">hello szolgáltatás kapcsolódási pontjának (SCP) objektum hello regisztrációs toodiscover információkat az Azure AD bérlő során használatos az eszközök által.</span><span class="sxs-lookup"><span data-stu-id="26c78-159">hello service connection point (SCP) object is used by your devices during hello registration toodiscover Azure AD tenant information.</span></span> <span data-ttu-id="26c78-160">A helyszíni Active Directoryban (AD) hello SCP objektumot hello automatikus regisztráció a tartományhoz csatlakozó eszközök hello konfigurációs névhasználati környezet partíció hello számítógép erdő léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="26c78-160">In your on-premises Active Directory (AD), hello SCP object for hello auto-registration of domain-joined devices must exist in hello configuration naming context partition of hello computer's forest.</span></span> <span data-ttu-id="26c78-161">Csak egy konfigurációs névhasználati környezet minden erdőre van.</span><span class="sxs-lookup"><span data-stu-id="26c78-161">There is only one configuration naming context per forest.</span></span> <span data-ttu-id="26c78-162">Többerdős Active Directory-konfiguráció esetén a hello szolgáltatáskapcsolódási pont léteznie kell az összes olyan erdőben, a tartományhoz csatlakoztatott számítógépeket tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="26c78-162">In a multi-forest Active Directory configuration, hello service connection point must exist in all forests containing domain-joined computers.</span></span>

<span data-ttu-id="26c78-163">Használhatja a hello [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) parancsmag tooretrieve hello konfigurációelnevezési környezetében az erdőben.</span><span class="sxs-lookup"><span data-stu-id="26c78-163">You can use hello [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet tooretrieve hello configuration naming context of your forest.</span></span>  

<span data-ttu-id="26c78-164">Active Directory-tartomány nevét hello erdő *fabrikam.com*, hello konfigurációelnevezési környezetében van:</span><span class="sxs-lookup"><span data-stu-id="26c78-164">For a forest with hello Active Directory domain name *fabrikam.com*, hello configuration naming context is:</span></span>

`CN=Configuration,DC=fabrikam,DC=com`

<span data-ttu-id="26c78-165">Az erdő hello SCP objektum hello automatikus regisztráció a tartományhoz csatlakoztatott eszközök a következő helyen található:</span><span class="sxs-lookup"><span data-stu-id="26c78-165">In your forest, hello SCP object for hello auto-registration of domain-joined devices is located at:</span></span>  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

<span data-ttu-id="26c78-166">Attól függően, hogy telepítette az Azure AD Connect hello SCP objektum lehet, hogy már meg vannak adva.</span><span class="sxs-lookup"><span data-stu-id="26c78-166">Depending on how you have deployed Azure AD Connect, hello SCP object may have already been configured.</span></span>
<span data-ttu-id="26c78-167">Hello objektum hello létezésének ellenőrzése, és használja a következő Windows PowerShell-parancsfájl hello hello felderítési értékek lekérését:</span><span class="sxs-lookup"><span data-stu-id="26c78-167">You can verify hello existence of hello object and retrieve hello discovery values using hello following Windows PowerShell script:</span></span> 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

<span data-ttu-id="26c78-168">Hello **$scp. Kulcsszavak** az alábbiakat mutatja be hello Azure AD bérlői kapcsolatos információkat, például:</span><span class="sxs-lookup"><span data-stu-id="26c78-168">hello **$scp.Keywords** output shows hello Azure AD tenant information, for example:</span></span>

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

<span data-ttu-id="26c78-169">Ha hello szolgáltatáskapcsolódási pont nem létezik, létrehozhatja hello futtatásával `Initialize-ADSyncDomainJoinedComputerSync` parancsmag az Azure AD Connect-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="26c78-169">If hello service connection point does not exist, you can create it by running hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet on your Azure AD Connect server.</span></span> <span data-ttu-id="26c78-170">Vállalati rendszergazda hitelesítő adatai szükséges toorun van ennek a parancsmagnak.</span><span class="sxs-lookup"><span data-stu-id="26c78-170">Enterprise admin credential is required toorun this cmdlet.</span></span>  
<span data-ttu-id="26c78-171">hello parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="26c78-171">hello cmdlet:</span></span>

- <span data-ttu-id="26c78-172">Hello Active Directory-erdő az Azure AD Connect csatlakozik-e hello szolgáltatáskapcsolódási pontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="26c78-172">Creates hello service connection point in hello Active Directory forest Azure AD Connect is connected to.</span></span> 
- <span data-ttu-id="26c78-173">Toospecify hello használatához `AdConnectorAccount` paraméter.</span><span class="sxs-lookup"><span data-stu-id="26c78-173">Requires you toospecify hello `AdConnectorAccount` parameter.</span></span> <span data-ttu-id="26c78-174">Ez az hello fiók, amely az Active Directory connector fiók az Azure AD connect van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="26c78-174">This is hello account that is configured as Active Directory connector account in Azure AD connect.</span></span> 


<span data-ttu-id="26c78-175">hello következő parancsfájlt mutat egy példát a hello parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="26c78-175">hello following script shows an example for using hello cmdlet.</span></span> <span data-ttu-id="26c78-176">Ezt a parancsfájlt a `$aadAdminCred = Get-Credential` tootype felhasználónév megadása szükséges.</span><span class="sxs-lookup"><span data-stu-id="26c78-176">In this script, `$aadAdminCred = Get-Credential` requires you tootype a user name.</span></span> <span data-ttu-id="26c78-177">Tooprovide hello felhasználónév hello felhasználó egyszerű felhasználónév (UPN) formátumban van szüksége (`user@example.com`).</span><span class="sxs-lookup"><span data-stu-id="26c78-177">You need tooprovide hello user name in hello user principal name (UPN) format (`user@example.com`).</span></span> 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

<span data-ttu-id="26c78-178">Hello `Initialize-ADSyncDomainJoinedComputerSync` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="26c78-178">hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span></span>

- <span data-ttu-id="26c78-179">Hello Active Directory PowerShell-modul, a tartományvezérlőn futó Active Directory webszolgáltatások használja.</span><span class="sxs-lookup"><span data-stu-id="26c78-179">Uses hello Active Directory PowerShell module, which relies on Active Directory Web Services running on a domain controller.</span></span> <span data-ttu-id="26c78-180">Az Active Directory webszolgáltatások a Windows Server 2008 R2 rendszerű tartományvezérlők és újabb rendszer.</span><span class="sxs-lookup"><span data-stu-id="26c78-180">Active Directory Web Services is supported on domain controllers running Windows Server 2008 R2 and later.</span></span>
- <span data-ttu-id="26c78-181">Csak akkor hello támogatott **MSOnline PowerShell modul verziója 1.1.166.0**.</span><span class="sxs-lookup"><span data-stu-id="26c78-181">Is only supported by hello **MSOnline PowerShell module version 1.1.166.0**.</span></span> <span data-ttu-id="26c78-182">toodownload Ez a modul ezt [hivatkozás](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span><span class="sxs-lookup"><span data-stu-id="26c78-182">toodownload this module, use this [link](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span></span>   

<span data-ttu-id="26c78-183">A Windows Server 2008 vagy korábbi verzióit futtató tartományvezérlők parancsfájllal hello toocreate hello szolgáltatáskapcsolódási pont alatt.</span><span class="sxs-lookup"><span data-stu-id="26c78-183">For domain controllers running Windows Server 2008 or earlier versions, use hello script below toocreate hello service connection point.</span></span>

<span data-ttu-id="26c78-184">Egy Többerdős konfigurációs parancsfájl toocreate hello szolgáltatáskapcsolódási pont minden olyan erdőben, ahol a számítógépek létezik a következő hello kell használnia:</span><span class="sxs-lookup"><span data-stu-id="26c78-184">In a multi-forest configuration, you should use hello following script toocreate hello service connection point in each forest where computers exist:</span></span>
 
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


## <a name="step-2-setup-issuance-of-claims"></a><span data-ttu-id="26c78-185">2. lépés: A jogcímek kiállítási beállítása</span><span class="sxs-lookup"><span data-stu-id="26c78-185">Step 2: Setup issuance of claims</span></span>

<span data-ttu-id="26c78-186">Az összevont Azure Active Directory beállítása, a eszközök támaszkodnak az Active Directory összevonási szolgáltatások (AD FS) vagy 3. fél a helyi összevonási szolgáltatás tooauthenticate tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="26c78-186">In a federated Azure AD configuration, devices rely on Active Directory Federation Services (AD FS) or a 3rd party on-premises federation service tooauthenticate tooAzure AD.</span></span> <span data-ttu-id="26c78-187">Eszközök hitelesítéséhez tooget egy hozzáférési jogkivonat tooregister hello Azure Active Directory Eszközregisztrációs szolgáltatás (Azure DRS) ellen.</span><span class="sxs-lookup"><span data-stu-id="26c78-187">Devices authenticate tooget an access token tooregister against hello Azure Active Directory Device Registration Service (Azure DRS).</span></span>

<span data-ttu-id="26c78-188">A Windows jelenlegi eszközök hitelesítés integrált Windows-hitelesítés tooan aktív WS-Trust végpont (1.3 vagy 2005 verzió) hello a helyi összevonási szolgáltatás által üzemeltetett használatával.</span><span class="sxs-lookup"><span data-stu-id="26c78-188">Windows current devices authenticate using Integrated Windows Authentication tooan active WS-Trust endpoint (either 1.3 or 2005 versions) hosted by hello on-premises federation service.</span></span>

> [!NOTE]
> <span data-ttu-id="26c78-189">AD FS-ben vagy használatakor **adfs/services/megbízhatósági/13/windowstransport** vagy **adfs/services/megbízhatósági/2005/windowstransport** engedélyezve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="26c78-189">When using AD FS, either **adfs/services/trust/13/windowstransport** or **adfs/services/trust/2005/windowstransport** must be enabled.</span></span> <span data-ttu-id="26c78-190">Ha hello hitelesítési Webproxyt használ, bizonyosodjon meg arról, hogy a végpont hello proxy használatával van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="26c78-190">If you are using hello Web Authentication Proxy, also ensure that this endpoint is published through hello proxy.</span></span> <span data-ttu-id="26c78-191">Láthatja, hogy milyen végpontjainak engedélyezve vannak – hello AD FS felügyeleti konzolon a **szolgáltatás > végpontok**.</span><span class="sxs-lookup"><span data-stu-id="26c78-191">You can see what end-points are enabled through hello AD FS management console under **Service > Endpoints**.</span></span>
>
><span data-ttu-id="26c78-192">Ha a helyi összevonási szolgáltatás AD FS nincs telepítve, lépésekkel hello a szállító toomake meg arról, hogy a WS-Trust 1.3 vagy 2005 végpontjainak és, hogy ezek hello metaadatok Exchange fájl (MEX) keresztül közzétett támogatják a.</span><span class="sxs-lookup"><span data-stu-id="26c78-192">If you don’t have AD FS as your on-premises federation service, follow hello instructions of your vendor toomake sure they support WS-Trust 1.3 or 2005 end-points and that these are published through hello Metadata Exchange file (MEX).</span></span>

<span data-ttu-id="26c78-193">hello következő jogcímeket léteznie kell az eszköz regisztrációja toocomplete Azure DRS által kapott hello jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="26c78-193">hello following claims must exist in hello token received by Azure DRS for device registration toocomplete.</span></span> <span data-ttu-id="26c78-194">Azure DRS egy eszközobjektumot hoz létre az egyes ezeket az információkat, majd az Azure AD Connect tooassociate az újonnan létrehozott hello eszközobjektum hello számítógép fiók helyszíni által használt Azure AD.</span><span class="sxs-lookup"><span data-stu-id="26c78-194">Azure DRS will create a device object in Azure AD with some of this information which is then used by Azure AD Connect tooassociate hello newly created device object with hello computer account on-premises.</span></span>

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

<span data-ttu-id="26c78-195">Ha több ellenőrzött tartomány nevét, a következő számítógépek jogcím tooprovide hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="26c78-195">If you have more than one verified domain name, you need tooprovide hello following claim for computers:</span></span>

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

<span data-ttu-id="26c78-196">Ha egy ImmutableID jogcímet (pl. másodlagos bejelentkezési Azonosítóval) kiadása már meg van szüksége egy megfelelő jogcím tooprovide számítógépek:</span><span class="sxs-lookup"><span data-stu-id="26c78-196">If you are already issuing an ImmutableID claim (e.g., alternate login ID) you need tooprovide one corresponding claim for computers:</span></span>

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

<span data-ttu-id="26c78-197">A következő részekben hello akkor talál információkat:</span><span class="sxs-lookup"><span data-stu-id="26c78-197">In hello following sections, you find information about:</span></span>
 
- <span data-ttu-id="26c78-198">az egyes jogcímek hello értékeket kell rendelkeznie</span><span class="sxs-lookup"><span data-stu-id="26c78-198">hello values each claim should have</span></span>
- <span data-ttu-id="26c78-199">Hogyan definíció jelenne meg az AD FS-ben</span><span class="sxs-lookup"><span data-stu-id="26c78-199">How a definition would look like in AD FS</span></span>

<span data-ttu-id="26c78-200">hello definition segít tooverify e hello értékek jelen-e, vagy ha szüksége van-e toocreate őket.</span><span class="sxs-lookup"><span data-stu-id="26c78-200">hello definition helps you tooverify whether hello values are present or if you need toocreate them.</span></span>

> [!NOTE]
> <span data-ttu-id="26c78-201">Ha az AD FS nem adja meg a helyi összevonási kiszolgáló, kövesse a gyártói utasításokat toocreate hello megfelelő konfiguráció tooissue ezeket a jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="26c78-201">If you don’t use AD FS for your on-premises federation server, follow your vendor's instructions toocreate hello appropriate configuration tooissue these claims.</span></span>

### <a name="issue-account-type-claim"></a><span data-ttu-id="26c78-202">A probléma fiók típusa jogcím</span><span class="sxs-lookup"><span data-stu-id="26c78-202">Issue account type claim</span></span>

<span data-ttu-id="26c78-203">**`http://schemas.microsoft.com/ws/2012/01/accounttype`**– Ezt az igényt az értéket kell tartalmaznia, **DJ**, amely azonosítja a hello eszközt egy tartományhoz csatlakozó számítógép.</span><span class="sxs-lookup"><span data-stu-id="26c78-203">**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - This claim must contain a value of **DJ**, which identifies hello device as a domain-joined computer.</span></span> <span data-ttu-id="26c78-204">Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="26c78-204">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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

### <a name="issue-objectguid-of-hello-computer-account-on-premises"></a><span data-ttu-id="26c78-205">A probléma objectGUID hello számítógép fiók helyszínen</span><span class="sxs-lookup"><span data-stu-id="26c78-205">Issue objectGUID of hello computer account on-premises</span></span>

<span data-ttu-id="26c78-206">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**-A jogcím tartalmaznia kell a hello **objectGUID** hello értékének a helyi számítógépfiókot.</span><span class="sxs-lookup"><span data-stu-id="26c78-206">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - This claim must contain hello **objectGUID** value of hello on-premises computer account.</span></span> <span data-ttu-id="26c78-207">Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="26c78-207">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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
 
### <a name="issue-objectsid-of-hello-computer-account-on-premises"></a><span data-ttu-id="26c78-208">A probléma objectSID hello számítógép fiók helyszínen</span><span class="sxs-lookup"><span data-stu-id="26c78-208">Issue objectSID of hello computer account on-premises</span></span>

<span data-ttu-id="26c78-209">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**-A jogcím tartalmaznia kell a hello hello **objectSid** hello értékének a helyi számítógépfiókot.</span><span class="sxs-lookup"><span data-stu-id="26c78-209">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - This claim must contain hello hello **objectSid** value of hello on-premises computer account.</span></span> <span data-ttu-id="26c78-210">Az AD FS-ben adhat meg egy kiadási átalakítási szabálykészlet, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="26c78-210">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a><span data-ttu-id="26c78-211">Számítógép issuerID kibocsátani, ha több tartománynév ellenőrzése az Azure ad-ben</span><span class="sxs-lookup"><span data-stu-id="26c78-211">Issue issuerID for computer when multiple verified domain names in Azure AD</span></span>

<span data-ttu-id="26c78-212">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**-A jogcím hello egységes erőforrás-azonosító (URI) bármely hello tartományneveket, amelyekben hello csatlakozzon a helyi összevonási szolgáltatás (AD FS vagy 3. fél) kibocsátó hello token ellenőrizni kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="26c78-212">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - This claim must contain hello Uniform Resource Identifier (URI) of any of hello verified domain names that connect with hello on-premises federation service (AD FS or 3rd party) issuing hello token.</span></span> <span data-ttu-id="26c78-213">Az AD FS-ben hello azokat, az alábbi abban, hogy a megadott sorrendben után azokat a fenti hello nézzenek kiállítás-transzformációs szabályok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="26c78-213">In AD FS, you can add issuance transform rules that look like hello ones below in that specific order after hello ones above.</span></span> <span data-ttu-id="26c78-214">A felhasználók szükséges vegye figyelembe, hogy egy tooexplicitly probléma hello szabály.</span><span class="sxs-lookup"><span data-stu-id="26c78-214">Please note that one rule tooexplicitly issue hello rule for users is necessary.</span></span> <span data-ttu-id="26c78-215">Az alábbi hello szabályok és a számítógép-hitelesítés felhasználói azonosító első szabály jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="26c78-215">In hello rules below, a first rule identifying user vs. computer authentication is added.</span></span>

    @RuleName = "Issue account type with hello value User when its not a computer"
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
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
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


<span data-ttu-id="26c78-216">A fenti hello jogcímek</span><span class="sxs-lookup"><span data-stu-id="26c78-216">In hello claim above,</span></span>

- <span data-ttu-id="26c78-217">`$<domain>`hello AD FS szolgáltatás URL-címe</span><span class="sxs-lookup"><span data-stu-id="26c78-217">`$<domain>` is hello AD FS service URL</span></span>
- <span data-ttu-id="26c78-218">`<verified-domain-name>`a szükséges tooreplace egyik a ellenőrzött tartomány nevét az Azure AD helyőrző</span><span class="sxs-lookup"><span data-stu-id="26c78-218">`<verified-domain-name>` is a placeholder you need tooreplace with one of your verified domain names in Azure AD</span></span>



<span data-ttu-id="26c78-219">Ellenőrzött tartomány nevét kapcsolatos további tudnivalókért lásd: [hozzáadása egy egyéni tartomány nevét az Active Directory tooAzure](active-directory-add-domain.md).</span><span class="sxs-lookup"><span data-stu-id="26c78-219">For more details about verified domain names, see [Add a custom domain name tooAzure Active Directory](active-directory-add-domain.md).</span></span>  
<span data-ttu-id="26c78-220">a vállalat ellenőrzött tartományok listájának tooget, hello használható [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="26c78-220">tooget a list of your verified company domains, you can use hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span></span> 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a><span data-ttu-id="26c78-222">Számítógép ImmutableID ki, ha egy, a felhasználók létező (pl. másodlagos bejelentkezési azonosító beállítása)</span><span class="sxs-lookup"><span data-stu-id="26c78-222">Issue ImmutableID for computer when one for users exist (e.g. alternate login ID is set)</span></span>

<span data-ttu-id="26c78-223">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**-A jogcím számítógépek érvényes értéket kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="26c78-223">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - This claim must contain a valid value for computers.</span></span> <span data-ttu-id="26c78-224">Az AD FS-ben is létrehozhat egy kiadási átalakítási szabálykészlet bocsát az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="26c78-224">In AD FS, you can create an issuance transform rule as follows:</span></span>

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

### <a name="helper-script-toocreate-hello-ad-fs-issuance-transform-rules"></a><span data-ttu-id="26c78-225">Segítő parancsfájl toocreate hello AD FS kiadás átalakítási szabályai</span><span class="sxs-lookup"><span data-stu-id="26c78-225">Helper script toocreate hello AD FS issuance transform rules</span></span>

<span data-ttu-id="26c78-226">hello következő parancsfájl segít azzal hello létrehozott hello kiállítási átalakítási szabályok a fent leírt.</span><span class="sxs-lookup"><span data-stu-id="26c78-226">hello following script helps you with hello creation of hello issuance transform rules described above.</span></span>

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
    $rule4 = '@RuleName = "Issue account type with hello value User when it is not a computer"
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
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
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

### <a name="remarks"></a><span data-ttu-id="26c78-227">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="26c78-227">Remarks</span></span> 

- <span data-ttu-id="26c78-228">Ez a parancsfájl hello szabályok toohello meglévő szabály fűzi hozzá.</span><span class="sxs-lookup"><span data-stu-id="26c78-228">This script appends hello rules toohello existing rules.</span></span> <span data-ttu-id="26c78-229">Nem futnak hello parancsfájl kétszer mivel a szabályok készletét hello volna hozzá kétszer.</span><span class="sxs-lookup"><span data-stu-id="26c78-229">Do not run hello script twice because hello set of rules would be added twice.</span></span> <span data-ttu-id="26c78-230">Győződjön meg arról, hogy nem megfelelő szabályok tartoznak (körülmények hello megfelelő) jogcímek hello parancsfájl ismételt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="26c78-230">Make sure that no corresponding rules exist for these claims (under hello corresponding conditions) before running hello script again.</span></span>

- <span data-ttu-id="26c78-231">Ha több ellenőrzött tartomány nevét (ahogy hello Azure AD portálon vagy hello Get-MsolDomains parancsmag segítségével), állítsa hello **$multipleVerifiedDomainNames** hello a parancsfájl-túl**$true**.</span><span class="sxs-lookup"><span data-stu-id="26c78-231">If you have multiple verified domain names (as shown in hello Azure AD portal or via hello Get-MsolDomains cmdlet), set hello value of **$multipleVerifiedDomainNames** in hello script too**$true**.</span></span> <span data-ttu-id="26c78-232">Győződjön meg arról is, hogy távolítsa el a meglévő issuerid jogcím létrehozott előfordulhat, hogy az Azure AD Connect vagy más eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="26c78-232">Also make sure that you remove any existing issuerid claim that might have been created by Azure AD Connect or via other means.</span></span> <span data-ttu-id="26c78-233">Íme egy példa a ehhez a szabályhoz:</span><span class="sxs-lookup"><span data-stu-id="26c78-233">Here is an example for this rule:</span></span>


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- <span data-ttu-id="26c78-234">Ha már kiadott egy **ImmutableID** vonatkozó felhasználói fiókok, állítsa be hello **$immutableIDAlreadyIssuedforUsers** hello a parancsfájl-túl**$true**.</span><span class="sxs-lookup"><span data-stu-id="26c78-234">If you have already issued an **ImmutableID** claim  for user accounts, set hello value of **$immutableIDAlreadyIssuedforUsers** in hello script too**$true**.</span></span>

## <a name="step-3-enable-windows-down-level-devices"></a><span data-ttu-id="26c78-235">3. lépés: Engedélyezze a régebbi Windows-eszközök</span><span class="sxs-lookup"><span data-stu-id="26c78-235">Step 3: Enable Windows down-level devices</span></span>

<span data-ttu-id="26c78-236">Ha néhány, a tartományhoz csatlakoztatott eszközök a Windows-kezelés régebbi eszközöket, akkor:</span><span class="sxs-lookup"><span data-stu-id="26c78-236">If some of your domain-joined devices Windows down-level devices, you need to:</span></span>

- <span data-ttu-id="26c78-237">Házirend beállítása az Azure AD tooenable felhasználók tooregister eszközök.</span><span class="sxs-lookup"><span data-stu-id="26c78-237">Set a policy in Azure AD tooenable users tooregister devices.</span></span>
 
- <span data-ttu-id="26c78-238">Konfigurálja a helyi összevonási szolgáltatás tooissue jogcímek toosupport **integrált Windows-hitelesítéssel (IWA)** az eszközregisztrációhoz tartozó.</span><span class="sxs-lookup"><span data-stu-id="26c78-238">Configure your on-premises federation service tooissue claims toosupport **Integrated Windows Authentication (IWA)** for device registration.</span></span>
 
- <span data-ttu-id="26c78-239">Adja hozzá a hello Azure AD eszköz hitelesítési végpont toohello helyi intranetes zóna tooavoid tanúsítványt kér hello eszköz hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="26c78-239">Add hello Azure AD device authentication end-point toohello local Intranet zones tooavoid certificate prompts when authenticating hello device.</span></span>

### <a name="set-policy-in-azure-ad-tooenable-users-tooregister-devices"></a><span data-ttu-id="26c78-240">Az Azure AD tooenable házirend beállítása felhasználói tooregister eszköz</span><span class="sxs-lookup"><span data-stu-id="26c78-240">Set policy in Azure AD tooenable users tooregister devices</span></span>

<span data-ttu-id="26c78-241">tooregister Windows régebbi eszközöket kell toomake meg arról, hogy hello tooallow felhasználók tooregister eszközök beállítása az Azure AD-be van állítva.</span><span class="sxs-lookup"><span data-stu-id="26c78-241">tooregister Windows down-level devices, you need toomake sure that hello setting tooallow users tooregister devices in Azure AD is set.</span></span> <span data-ttu-id="26c78-242">A hello Azure-portálon a beállítás található:</span><span class="sxs-lookup"><span data-stu-id="26c78-242">In hello Azure portal, you can find this setting under:</span></span>

`Azure Active Directory > Users and groups > Device settings`
    
<span data-ttu-id="26c78-243">hello következő házirendet be kell állítani túl**összes**: **felhasználók előfordulhat, hogy regisztrálják az eszközeiket az Azure ad szolgáltatással**</span><span class="sxs-lookup"><span data-stu-id="26c78-243">hello following policy must be set too**All**: **Users may register their devices with Azure AD**</span></span>

![Eszközök regisztrálása](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a><span data-ttu-id="26c78-245">A helyi összevonási szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26c78-245">Configure on-premises federation service</span></span> 

<span data-ttu-id="26c78-246">A helyi összevonási szolgáltatás támogatnia kell a kiállító hello **authenticationmehod** és **wiaormultiauthn** jogcímek fogadására egy hitelesítési kérelmek betöltő függő fél toohello az Azure AD egy resouce_params paraméterhez a kódolt érték látható:</span><span class="sxs-lookup"><span data-stu-id="26c78-246">Your on-premises federation service must support issuing hello **authenticationmehod** and **wiaormultiauthn** claims when receiving an authentication request toohello Azure AD relying party holding a resouce_params parameter with an encoded value as shown below:</span></span>

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

<span data-ttu-id="26c78-247">Ha ilyen kérést, hello a helyi összevonási szolgáltatás hello felhasználói integrált Windows-hitelesítésen keresztül kell hitelesítenie, és akkor sikeres, a következő két jogcímek hello kiadja:</span><span class="sxs-lookup"><span data-stu-id="26c78-247">When such a request comes, hello on-premises federation service must authenticate hello user using Integrated Windows Authentication and upon success, it must issue hello following two claims:</span></span>

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

<span data-ttu-id="26c78-248">Az AD FS-ben hozzá kell adnia egy kiadási átalakítási szabálykészlet adott folyamaton keresztül hello hitelesítési módszert.</span><span class="sxs-lookup"><span data-stu-id="26c78-248">In AD FS, you must add an issuance transform rule that passes-through hello authentication method.</span></span>  

<span data-ttu-id="26c78-249">**tooadd Ez a szabály:**</span><span class="sxs-lookup"><span data-stu-id="26c78-249">**tooadd this rule:**</span></span>

1. <span data-ttu-id="26c78-250">A hello AD FS felügyeleti konzolon váltson túl`AD FS > Trust Relationships > Relying Party Trusts`.</span><span class="sxs-lookup"><span data-stu-id="26c78-250">In hello AD FS management console, go too`AD FS > Trust Relationships > Relying Party Trusts`.</span></span>
2. <span data-ttu-id="26c78-251">Kattintson a jobb gombbal a hello Microsoft Office 365 Identitásplatformmal függő entitás megbízhatósági objektum, majd válassza ki **Jogcímszabályok szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="26c78-251">Right-click hello Microsoft Office 365 Identity Platform relying party trust object, and then select **Edit Claim Rules**.</span></span>
3. <span data-ttu-id="26c78-252">A hello **kiadás átalakítási szabályai** lapon jelölje be **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="26c78-252">On hello **Issuance Transform Rules** tab, select **Add Rule**.</span></span>
4. <span data-ttu-id="26c78-253">A hello **jogcímszabály** sablon listáról válassza ki **jogcímek küldése egyéni szabály segítségével**.</span><span class="sxs-lookup"><span data-stu-id="26c78-253">In hello **Claim rule** template list, select **Send Claims Using a Custom Rule**.</span></span>
5. <span data-ttu-id="26c78-254">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="26c78-254">Select **Next**.</span></span>
6. <span data-ttu-id="26c78-255">A hello **Jogcímszabály nevének** mezőbe írja be **hitelesítési módszer Jogcímszabály**.</span><span class="sxs-lookup"><span data-stu-id="26c78-255">In hello **Claim rule name** box, type **Auth Method Claim Rule**.</span></span>
7. <span data-ttu-id="26c78-256">A hello **jogcímszabály** mezőbe, a következő szabály típusa hello:</span><span class="sxs-lookup"><span data-stu-id="26c78-256">In hello **Claim rule** box, type hello following rule:</span></span>

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. <span data-ttu-id="26c78-257">Az összevonási kiszolgálón, írja be az alábbi PowerShell-paranccsal hello cseréje után  **\<RPObjectName\>**  nevű hello függő entitás objektum az az Azure AD függő entitás megbízhatósági objektum.</span><span class="sxs-lookup"><span data-stu-id="26c78-257">On your federation server, type hello PowerShell command below after replacing **\<RPObjectName\>** with hello relying party object name for your Azure AD relying party trust object.</span></span> <span data-ttu-id="26c78-258">Ez az objektum neve általában **Microsoft Office 365 Identitásplatformmal**.</span><span class="sxs-lookup"><span data-stu-id="26c78-258">This object usually is named **Microsoft Office 365 Identity Platform**.</span></span>
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-hello-azure-ad-device-authentication-end-point-toohello-local-intranet-zones"></a><span data-ttu-id="26c78-259">Hello Azure AD eszköz hitelesítési végpont toohello Helyi Intranet zóna hozzáadása</span><span class="sxs-lookup"><span data-stu-id="26c78-259">Add hello Azure AD device authentication end-point toohello Local Intranet zones</span></span>

<span data-ttu-id="26c78-260">tooavoid tanúsítvány kérni fogja a felhasználók az eszközök regisztrálása a hitelesítéshez tooAzure AD tolható ki a házirend tooyour tartományhoz csatlakozó eszközök tooadd hello a következő URL-cím toohello helyi intranetzónához az Internet Explorerben:</span><span class="sxs-lookup"><span data-stu-id="26c78-260">tooavoid certificate prompts when users in register devices authenticate tooAzure AD you can push a policy tooyour domain-joined devices tooadd hello following URL toohello Local Intranet zone in Internet Explorer:</span></span>

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a><span data-ttu-id="26c78-261">4. lépés: Központi telepítési és a bevezetés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="26c78-261">Step 4: Control deployment and rollout</span></span>

<span data-ttu-id="26c78-262">Amikor befejezte a hello szükséges lépéseket, tartományhoz csatlakozó eszközök készen tooautomatically regisztrálása az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="26c78-262">When you have completed hello required steps, domain-joined devices are ready tooautomatically register with Azure AD.</span></span> <span data-ttu-id="26c78-263">Minden tartományhoz csatlakozó eszközök a Windows 10 évforduló Update és Windows Server 2016 rendszert futtató automatikusan regisztrálja az Azure AD-eszközön indítsa újra, vagy felhasználói bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="26c78-263">All domain-joined devices running Windows 10 Anniversary Update and Windows Server 2016 automatically register with Azure AD at device restart or user sign-in.</span></span> <span data-ttu-id="26c78-264">Új eszközök regisztrálása az Azure AD, ha hello eszköz újraindul hello tartomány illesztési művelet befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="26c78-264">New devices register with Azure AD when hello device restarts after hello domain join operation is completed.</span></span>

<span data-ttu-id="26c78-265">Olyan eszközök, amelyek túl volt korábban munkahelyhez tooAzure AD (például az Intune-hoz) átmenet"*tartományhoz csatlakoztatott, aad-ben regisztrált*"; azonban csak bizonyos idő az a folyamat toocomplete toohello normál miatt az összes eszközön tartomány- és felhasználói tevékenységek áramló.</span><span class="sxs-lookup"><span data-stu-id="26c78-265">Devices that were previously workplace-joined tooAzure AD (for example for Intune) transition too“*Domain Joined, AAD Registered*”; however it takes some time for this process toocomplete across all devices due toohello normal flow of domain and user activity.</span></span>

### <a name="remarks"></a><span data-ttu-id="26c78-266">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="26c78-266">Remarks</span></span>

- <span data-ttu-id="26c78-267">A csoportházirend objektumot toocontrol hello Bevezetés a Windows Server 2016 tartományhoz csatlakozó számítógépek és Windows 10 automatikus regisztráció is használhatja.</span><span class="sxs-lookup"><span data-stu-id="26c78-267">You can use a Group Policy object toocontrol hello rollout of automatic registration of Windows 10 and Windows Server 2016 domain-joined computers.</span></span>

- <span data-ttu-id="26c78-268">Windows 10 2015. November automatikus frissítés regiszterekben az Azure ad-val **csak** Ha hello bevezetés csoportházirend-objektum be van állítva.</span><span class="sxs-lookup"><span data-stu-id="26c78-268">Windows 10 November 2015 Update automatically registers with Azure AD **only** if hello rollout Group Policy object is set.</span></span>

- <span data-ttu-id="26c78-269">toorollout hello az automatikus regisztráció a Windows-kezelés régebbi rendszerű számítógépek, telepíthet egy [Windows Installer-csomag](#windows-installer-packages-for-non-windows-10-computers) toocomputers választott.</span><span class="sxs-lookup"><span data-stu-id="26c78-269">toorollout hello automatic registration of Windows down-level computers, you can deploy a [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) toocomputers that you select.</span></span>

- <span data-ttu-id="26c78-270">Ha ügyfélleküldéses hello csoportházirend objektum tooWindows 8.1 tartományhoz csatlakozó eszközök, regisztrációs megpróbálkozik az erdőfelderítéssel; azt ajánljuk, hogy használja-e hello [Windows Installer-csomag](#windows-installer-packages-for-non-windows-10-computers) tooregister összes Windows-kezelés régebbi rendszerű eszközre.</span><span class="sxs-lookup"><span data-stu-id="26c78-270">If you push hello Group Policy object tooWindows 8.1 domain-joined devices, registration will be attempted; however it is recommended that you use hello [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) tooregister all your Windows down-level devices.</span></span> 

### <a name="create-a-group-policy-object"></a><span data-ttu-id="26c78-271">A csoportházirend-objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="26c78-271">Create a Group Policy object</span></span> 

<span data-ttu-id="26c78-272">a jelenlegi Windows-számítógép az automatikus regisztráció toocontrol hello bevezetésének, telepítsen hello **eszközként regisztrálja a tartományhoz csatlakozó számítógépek** csoportházirend-objektum toohello eszközök tooregister szeretné.</span><span class="sxs-lookup"><span data-stu-id="26c78-272">toocontrol hello rollout of automatic registration of Windows current computers, you should deploy hello **Register domain-joined computers as devices** Group Policy object toohello devices you want tooregister.</span></span> <span data-ttu-id="26c78-273">Telepíthet például hello házirend tooan szervezeti egység vagy tooa biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="26c78-273">For example, you can deploy hello policy tooan organizational unit or tooa security group.</span></span>

<span data-ttu-id="26c78-274">**tooset hello házirend:**</span><span class="sxs-lookup"><span data-stu-id="26c78-274">**tooset hello policy:**</span></span>

1. <span data-ttu-id="26c78-275">Nyissa meg **Kiszolgálókezelő**, és folytassa a túl`Tools > Group Policy Management`.</span><span class="sxs-lookup"><span data-stu-id="26c78-275">Open **Server Manager**, and then go too`Tools > Group Policy Management`.</span></span>
2. <span data-ttu-id="26c78-276">Nyissa meg, amely megfelel a toohello tartományba, ahol tooactivate automatikus regisztráció a jelenlegi Windows-számítógépek toohello tartományi csomópontot.</span><span class="sxs-lookup"><span data-stu-id="26c78-276">Go toohello domain node that corresponds toohello domain where you want tooactivate auto-registration of Windows current computers.</span></span>
3. <span data-ttu-id="26c78-277">Kattintson a jobb gombbal **csoportházirend-objektumok**, majd válassza ki **új**.</span><span class="sxs-lookup"><span data-stu-id="26c78-277">Right-click **Group Policy Objects**, and then select **New**.</span></span>
4. <span data-ttu-id="26c78-278">Írja be a csoportházirend-objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="26c78-278">Type a name for your Group Policy object.</span></span> <span data-ttu-id="26c78-279">Például *az automatikus regisztráció tooAzure AD*.</span><span class="sxs-lookup"><span data-stu-id="26c78-279">For example, *Automatic Registration tooAzure AD*.</span></span> <span data-ttu-id="26c78-280">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="26c78-280">Select **OK**.</span></span>
5. <span data-ttu-id="26c78-281">Kattintson a jobb gombbal az új csoportházirend-objektumot, majd válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="26c78-281">Right-click your new Group Policy object, and then select **Edit**.</span></span>
6. <span data-ttu-id="26c78-282">Nyissa meg túl**számítógép konfigurációja** > **házirendek** > **felügyeleti sablonok** > **Windows Összetevők** > **Eszközregisztráció**.</span><span class="sxs-lookup"><span data-stu-id="26c78-282">Go too**Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Device Registration**.</span></span> <span data-ttu-id="26c78-283">Kattintson a jobb gombbal **eszközként regisztrálja a tartományhoz csatlakozó számítógépek**, majd válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="26c78-283">Right-click **Register domain-joined computers as devices**, and then select **Edit**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="26c78-284">Ez a csoportházirend-sablon hello Csoportházirend kezelése konzol korábbi verzióiból át lett nevezve.</span><span class="sxs-lookup"><span data-stu-id="26c78-284">This Group Policy template has been renamed from earlier versions of hello Group Policy Management console.</span></span> <span data-ttu-id="26c78-285">Egy korábbi hello konzol használatakor lépjen túl`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span><span class="sxs-lookup"><span data-stu-id="26c78-285">If you are using an earlier version of hello console, go too`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span></span> 

7. <span data-ttu-id="26c78-286">Válassza ki **engedélyezve**, majd válassza ki **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="26c78-286">Select **Enabled**, and then select **Apply**.</span></span>
8. <span data-ttu-id="26c78-287">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="26c78-287">Select **OK**.</span></span>
9. <span data-ttu-id="26c78-288">Hivatkozás hello csoportházirend objektum tooa tetszőleges helyre.</span><span class="sxs-lookup"><span data-stu-id="26c78-288">Link hello Group Policy object tooa location of your choice.</span></span> <span data-ttu-id="26c78-289">Például társíthatja az adott szervezeti egység tooa.</span><span class="sxs-lookup"><span data-stu-id="26c78-289">For example, you can link it tooa specific organizational unit.</span></span> <span data-ttu-id="26c78-290">Is sikertelen csatolás tooa meghatározott biztonsági számítógépek csoportja, amelyek automatikusan regisztrálja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="26c78-290">You also could link it tooa specific security group of computers that automatically register with Azure AD.</span></span> <span data-ttu-id="26c78-291">tooset minden tartományhoz csatlakoztatott Windows 10 és Windows Server 2016 a szervezet számítógépeire, hivatkozás hello csoportházirend objektum toohello tartományi házirendet.</span><span class="sxs-lookup"><span data-stu-id="26c78-291">tooset this policy for all domain-joined Windows 10 and Windows Server 2016 computers in your organization, link hello Group Policy object toohello domain.</span></span>

### <a name="windows-installer-packages-for-non-windows-10-computers"></a><span data-ttu-id="26c78-292">A Windows 10 számítógépek Windows Installer-csomag</span><span class="sxs-lookup"><span data-stu-id="26c78-292">Windows Installer packages for non-Windows 10 computers</span></span>

<span data-ttu-id="26c78-293">tooregister tartományhoz csatlakoztatott Windows-kezelés régebbi számítógépek összevont környezetben, töltse le és telepítse a Windows Installer-csomag (.msi) a letöltőközpontból: hello [Microsoft munkahelyi csatlakoztatás Windows 10 számítógépek](https://www.microsoft.com/en-us/download/details.aspx?id=53554) lap.</span><span class="sxs-lookup"><span data-stu-id="26c78-293">tooregister domain-joined Windows down-level computers in a federated environment, you can download and install these Windows Installer package (.msi) from Download Center at hello [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) page.</span></span>

<span data-ttu-id="26c78-294">Hello csomagot a szoftverek terjesztési rendszer például System Center Configuration Manager segítségével telepíthet.</span><span class="sxs-lookup"><span data-stu-id="26c78-294">You can deploy hello package by using a software distribution system like System Center Configuration Manager.</span></span> <span data-ttu-id="26c78-295">hello csomag támogatja hello szabványos beavatkozás nélküli telepítés beállításainak hello *csendes* paraméter.</span><span class="sxs-lookup"><span data-stu-id="26c78-295">hello package supports hello standard silent install options with hello *quiet* parameter.</span></span> <span data-ttu-id="26c78-296">A System Center Configuration Manager aktuális ágának további előnyökkel jár a korábbi verziók, például hello képes végrehajtani tootrack regisztráció.</span><span class="sxs-lookup"><span data-stu-id="26c78-296">System Center Configuration Manager Current Branch offers additional benefits from earlier versions, like hello ability tootrack completed registrations.</span></span> <span data-ttu-id="26c78-297">További információkért lásd: [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="26c78-297">For more information, see [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span></span>

<span data-ttu-id="26c78-298">hello telepítő ütemezett feladatot hoz létre hello felhasználó környezetében futó hello rendszeren.</span><span class="sxs-lookup"><span data-stu-id="26c78-298">hello installer creates a scheduled task on hello system that runs in hello user’s context.</span></span> <span data-ttu-id="26c78-299">hello feladat indítása tooWindows hello felhasználó bejelentkezésekor.</span><span class="sxs-lookup"><span data-stu-id="26c78-299">hello task is triggered when hello user signs in tooWindows.</span></span> <span data-ttu-id="26c78-300">hello feladat csendes regisztrálja hello eszköz hitelesítése az integrált Windows-hitelesítést használó után hello felhasználói hitelesítő adatokat az Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="26c78-300">hello task silently registers hello device with Azure AD with hello user credentials after authenticating using Integrated Windows Authentication.</span></span> <span data-ttu-id="26c78-301">toosee hello ütemezett feladat, hello eszközön Ugrás túl**Microsoft** > **munkahelyi csatlakoztatás**, és folytassa a toohello Feladatütemező könyvtár.</span><span class="sxs-lookup"><span data-stu-id="26c78-301">toosee hello scheduled task, in hello device, go too**Microsoft** > **Workplace Join**, and then go toohello Task Scheduler library.</span></span>

## <a name="step-5-verify-registered-devices"></a><span data-ttu-id="26c78-302">5. lépés: A regisztrált eszközök ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="26c78-302">Step 5: Verify registered devices</span></span>

<span data-ttu-id="26c78-303">Ellenőrizheti a sikeres regisztrált eszközöket a szervezet hello segítségével [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) hello parancsmag [Azure Active Directory PowerShell-modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="26c78-303">You can check successful registered devices in your organization by using hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in hello [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span>

<span data-ttu-id="26c78-304">Ez a parancsmag kimenete hello az Azure AD-ben regisztrált eszközök jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="26c78-304">hello output of this cmdlet shows devices registered in Azure AD.</span></span> <span data-ttu-id="26c78-305">tooget minden eszköz használata hello **-összes** paramétert, majd a szűrő őket hello segítségével **deviceTrustType** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="26c78-305">tooget all devices, use hello **-All** parameter, and then filter them using hello **deviceTrustType** property.</span></span> <span data-ttu-id="26c78-306">Tartományhoz csatlakozó eszközök értéket veheti fel **tartományhoz**.</span><span class="sxs-lookup"><span data-stu-id="26c78-306">Domain joined devices have a value of **Domain Joined**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="26c78-307">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26c78-307">Next steps</span></span>

* [<span data-ttu-id="26c78-308">Automatikus eszközregisztráció – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="26c78-308">Automatic device registration FAQ</span></span>](active-directory-device-registration-faq.md)
* [<span data-ttu-id="26c78-309">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD – Windows 10 és Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="26c78-309">Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)
* [<span data-ttu-id="26c78-310">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD – Windows 10</span><span class="sxs-lookup"><span data-stu-id="26c78-310">Troubleshooting auto-registration of domain joined computers tooAzure AD – non-Windows 10</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
* [<span data-ttu-id="26c78-311">Azure Active Directory feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="26c78-311">Azure Active Directory conditional access</span></span>](active-directory-conditional-access-azure-portal.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
