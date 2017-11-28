---
title: "Hibaelhárítás az automatikus regisztráció az Azure AD-tartomány csatlakoztatott számítógépek Windows 10 és Windows Server 2016 |} Microsoft Docs"
description: "A Windows 10 és Windows Server 2016 automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 5b7f95f302f716d9221b5fae59aa2df5c956a524
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad--windows-10-and-windows-server-2016"></a><span data-ttu-id="f982b-103">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépeit az Azure AD – a Windows 10 és Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="f982b-103">Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016</span></span>

<span data-ttu-id="f982b-104">Ez a témakör a következő alkalmazható a következő ügyfelekre:</span><span class="sxs-lookup"><span data-stu-id="f982b-104">This topic is applicable to the following clients:</span></span>

-   <span data-ttu-id="f982b-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="f982b-105">Windows 10</span></span>
-   <span data-ttu-id="f982b-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="f982b-106">Windows Server 2016</span></span>

<span data-ttu-id="f982b-107">Egyéb Windows-ügyfelein, lásd: [automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépeit az Azure AD Windows régebbi ügyfelek](active-directory-device-registration-troubleshoot-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="f982b-107">For other Windows clients, see [Troubleshooting auto-registration of domain joined computers to Azure AD for Windows down-level clients](active-directory-device-registration-troubleshoot-windows-legacy.md).</span></span>

<span data-ttu-id="f982b-108">Ez a témakör feltételezi, hogy konfigurálta a tartományhoz csatlakoztatott eszközök automatikus regisztráció megfelelően ismertetett [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-device-registration-get-started.md) támogatásához a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="f982b-108">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md) to support the following scenarios:</span></span>

- [<span data-ttu-id="f982b-109">Eszközalapú feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="f982b-109">Device-based conditional access</span></span>](active-directory-conditional-access-automatic-device-registration-setup.md)

- [<span data-ttu-id="f982b-110">Vállalati központi beállítások</span><span class="sxs-lookup"><span data-stu-id="f982b-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="f982b-111">Vállalati Windows Hello</span><span class="sxs-lookup"><span data-stu-id="f982b-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="f982b-112">Ez a dokumentum a lehetséges problémák megoldásához nyújt hibaelhárítási útmutatót.</span><span class="sxs-lookup"><span data-stu-id="f982b-112">This document provides troubleshooting guidance on how to resolve potential issues.</span></span> 

<span data-ttu-id="f982b-113">A regisztrációt a Windows támogatott 2015. November 10. frissítés vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="f982b-113">The registration is supported in the Windows 10 November 2015 Update and above.</span></span>  
<span data-ttu-id="f982b-114">A évforduló frissítés engedélyezéséhez a fenti helyzetekben használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="f982b-114">We recommend using the Anniversary Update for enabling the scenarios above.</span></span>

## <a name="step-1-retrieve-the-registration-status"></a><span data-ttu-id="f982b-115">1. lépés: A regisztráció állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="f982b-115">Step 1: Retrieve the registration status</span></span> 

<span data-ttu-id="f982b-116">**A regisztrációs állapot beolvasása:**</span><span class="sxs-lookup"><span data-stu-id="f982b-116">**To retrieve the registration status:**</span></span>

1. <span data-ttu-id="f982b-117">Nyissa meg a parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f982b-117">Open the command prompt as an administrator.</span></span>

2. <span data-ttu-id="f982b-118">Típus **dsregcmd/status**</span><span class="sxs-lookup"><span data-stu-id="f982b-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="f982b-119">+----------------------------------------------------------------------+
   | Az eszköz állapotát |}+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="f982b-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined : YES
     <span data-ttu-id="f982b-120">EnterpriseJoined: Nincs DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 ujjlenyomat: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform titkosításszolgáltató TpmProtected: Igen KeySignTest:: emelt szintű tesztelése kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="f982b-120">EnterpriseJoined : NO DeviceId : 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint : B753A6679CE720451921302CA873794D94C6204A KeyContainerId : bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider : Microsoft Platform Crypto Provider TpmProtected : YES KeySignTest: : MUST Run elevated to test.</span></span>
                  <span data-ttu-id="f982b-121">IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-Beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0-ás JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0-ás KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Igen tartománynév: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="f982b-121">Idp : login.windows.net TenantId : 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName : Contoso AuthCodeUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl : eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion : 1.0 JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion : 1.0 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId : urn:ms-drs:enterpriseregistration.windows.net DomainJoined : YES DomainName : CONTOSO</span></span>
    
    <span data-ttu-id="f982b-122">+----------------------------------------------------------------------+
   | A felhasználói állapot |}+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="f982b-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    <span data-ttu-id="f982b-123">WamDefaultAuthority: szervezetek WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Igen</span><span class="sxs-lookup"><span data-stu-id="f982b-123">WamDefaultAuthority : organizations         WamDefaultId : https://login.microsoft.com       WamDefaultGUID : {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt : YES</span></span>



## <a name="step-2-evaluate-the-registration-status"></a><span data-ttu-id="f982b-124">2. lépés: A regisztráció állapotának kiértékelésére.</span><span class="sxs-lookup"><span data-stu-id="f982b-124">Step 2: Evaluate the registration status</span></span> 

<span data-ttu-id="f982b-125">Tekintse át a következő mezőket, és győződjön meg arról, hogy rendelkezik-e a várt értékek:</span><span class="sxs-lookup"><span data-stu-id="f982b-125">Review the following fields and make sure that they have the expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="f982b-126">AzureAdJoined: Igen</span><span class="sxs-lookup"><span data-stu-id="f982b-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="f982b-127">Ez a mező jeleníti meg, hogy az eszköz regisztrálva van-e az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="f982b-127">This field shows whether the device is registered with Azure AD.</span></span> <span data-ttu-id="f982b-128">Amennyiben az értéke, a "Nem", regisztrációs nem fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="f982b-128">If the value shows as ‘NO’, registration has not completed.</span></span> 

<span data-ttu-id="f982b-129">**Lehetséges okok:**</span><span class="sxs-lookup"><span data-stu-id="f982b-129">**Possible causes:**</span></span>

- <span data-ttu-id="f982b-130">A számítógép regisztrációjához hitelesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="f982b-130">Authentication of the computer for registration failed.</span></span>

- <span data-ttu-id="f982b-131">Nincs olyan szervezet, amely a számítógép nem észlelhetők egy HTTP-proxy</span><span class="sxs-lookup"><span data-stu-id="f982b-131">There is an HTTP proxy in the organization that cannot be discovered by the computer</span></span>

- <span data-ttu-id="f982b-132">A számítógép nem érhető el, az Azure AD hitelesítésében vagy Azure DRS a regisztrációhoz</span><span class="sxs-lookup"><span data-stu-id="f982b-132">The computer cannot reach Azure AD for authentication or Azure DRS for registration</span></span>

- <span data-ttu-id="f982b-133">A számítógép nem lehet a szervezet belső hálózaton vagy a VPN a közvetlen sor a láthatáron egy a helyszíni AD-tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="f982b-133">The computer may not be on the organization’s internal network or on VPN with direct line of sight to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="f982b-134">Ha a számítógép TPM-mel, valószínűleg rossz állapotban.</span><span class="sxs-lookup"><span data-stu-id="f982b-134">If the computer has a TPM, it may be in a bad state.</span></span>

- <span data-ttu-id="f982b-135">Előfordulhat, a szolgáltatások egy helytelen konfiguráció című dokumentumban korábban feljegyzett, hogy ellenőrizze újra kell.</span><span class="sxs-lookup"><span data-stu-id="f982b-135">There may be a misconfiguration in services noted in the document earlier that you will need to verify again.</span></span> <span data-ttu-id="f982b-136">Gyakori példák:</span><span class="sxs-lookup"><span data-stu-id="f982b-136">Common examples are:</span></span>

    - <span data-ttu-id="f982b-137">Az összevonási kiszolgálón nincs engedélyezve a WS-Trust végpontok</span><span class="sxs-lookup"><span data-stu-id="f982b-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="f982b-138">Az összevonási kiszolgáló integrált Windows-hitelesítés segítségével a hálózat nem teszi lehetővé bejövő hitelesítés számítógépekről.</span><span class="sxs-lookup"><span data-stu-id="f982b-138">Your federation server may not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="f982b-139">Nincs szolgáltatáskapcsolódási pont objektum mutat, a ellenőrzött tartomány nevét az Azure ad-ben az Active Directory-erdőben, ahol a számítógép tartozik</span><span class="sxs-lookup"><span data-stu-id="f982b-139">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="f982b-140">DomainJoined: Igen</span><span class="sxs-lookup"><span data-stu-id="f982b-140">DomainJoined : YES</span></span>  

<span data-ttu-id="f982b-141">Ez a mező jeleníti meg, hogy az eszköz csatlakozott a helyi Active Directory-e.</span><span class="sxs-lookup"><span data-stu-id="f982b-141">This field shows whether the device is joined to an on-premises Active Directory or not.</span></span> <span data-ttu-id="f982b-142">Amennyiben az értéke **nem**, az eszköz nem automatikus regisztrációját az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="f982b-142">If the value shows as **NO**, the device cannot auto-register with Azure AD.</span></span> <span data-ttu-id="f982b-143">Először ellenőrizze a, hogy az eszköz csatlakozik a helyszíni Active Directory előtt is regisztrálja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="f982b-143">Check first that the device joins to the on-premises Active Directory before it can register with Azure AD.</span></span> <span data-ttu-id="f982b-144">Ha a számítógép csatlakoztatása az Azure AD közvetlenül keres, lépjen további információ az Azure Active Directory csatlakozási képességeivel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="f982b-144">If you are looking for joining the computer to Azure AD directly, please go to Learn about capabilities of Azure Active Directory Join.</span></span>

---

### <a name="workplacejoined--no"></a><span data-ttu-id="f982b-145">WorkplaceJoined: nincs</span><span class="sxs-lookup"><span data-stu-id="f982b-145">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="f982b-146">Ez a mező jeleníti meg, hogy az Azure ad-vel, de (munkahelyhez csatlakoztatott jelölésű) személyes eszközként regisztrálja az eszközt.</span><span class="sxs-lookup"><span data-stu-id="f982b-146">This field shows whether the device is registered with Azure AD but as a personal device (marked as ‘Workplace Joined’).</span></span> <span data-ttu-id="f982b-147">Ez az érték "Nem", a tartományhoz csatlakozó számítógépen az Azure ad-vel regisztrált jelenjen meg, azonban ha igen, mert mutat azt jelenti, hogy a munkahelyi vagy iskolai fiókkal lett hozzáadva a számítógép regisztráció befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="f982b-147">If this value should show as ‘NO’ for a domain joined computer registered with Azure AD, however if it shows as YES it means that a work or school account was added prior to the computer completing registration.</span></span> <span data-ttu-id="f982b-148">Ebben az esetben a fiók lesz figyelembe véve, ha a Windows 10 (Ha a "Futtatás" vagy a parancssori ablakban futó a WinVer parancs 1607) évforduló frissítés verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="f982b-148">In this case the account will be ignored if using the Anniversary Update version of Windows 10 (1607 when running the WinVer command in the ‘Run’ window or a command prompt window).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="f982b-149">WamDefaultSet: Igen és AzureADPrt: Igen</span><span class="sxs-lookup"><span data-stu-id="f982b-149">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="f982b-150">Ezek a mezők megjelenítése, hogy az Azure AD-követően az eszközre való bejelentkezéskor rendelkezik sikeresen hitelesíteni a felhasználót.</span><span class="sxs-lookup"><span data-stu-id="f982b-150">These fields show that the user has successfully authenticated to Azure AD upon signing in to the device.</span></span> <span data-ttu-id="f982b-151">Ha ezek megjelenítése "Nem", a következők okozhatják:</span><span class="sxs-lookup"><span data-stu-id="f982b-151">If they show ‘NO’ the following are possible causes:</span></span>

- <span data-ttu-id="f982b-152">Hibás tárolási kulcs (STK) az eszköz regisztrálásakor (Ellenőrizze az emelt szintű futtatása közben KeySignTest) társított TPM.</span><span class="sxs-lookup"><span data-stu-id="f982b-152">Bad storage key (STK) in TPM associated with the device upon registration (check the KeySignTest while running elevated).</span></span>

- <span data-ttu-id="f982b-153">Másodlagos bejelentkezési Azonosítóval</span><span class="sxs-lookup"><span data-stu-id="f982b-153">Alternate Login ID</span></span>

- <span data-ttu-id="f982b-154">HTTP-Proxy nem található.</span><span class="sxs-lookup"><span data-stu-id="f982b-154">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="f982b-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f982b-155">Next steps</span></span>

<span data-ttu-id="f982b-156">További információkért lásd: a [automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="f982b-156">For more information, see the [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 