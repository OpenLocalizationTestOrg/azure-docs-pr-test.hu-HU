---
title: "aaaTroubleshooting hello automatikus regisztráció az Azure AD-tartományhoz csatlakoztatott számítógépekre Windows 10 és Windows Server 2016 |} Microsoft Docs"
description: "A Windows 10 és Windows Server 2016 hello automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit."
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
ms.openlocfilehash: 3795323ce9392368b412b3e1208868431e59a74b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad--windows-10-and-windows-server-2016"></a><span data-ttu-id="78cd6-103">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD – Windows 10 és Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="78cd6-103">Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016</span></span>

<span data-ttu-id="78cd6-104">Ez a témakör a következő ügyfelek alkalmazható toohello:</span><span class="sxs-lookup"><span data-stu-id="78cd6-104">This topic is applicable toohello following clients:</span></span>

-   <span data-ttu-id="78cd6-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="78cd6-105">Windows 10</span></span>
-   <span data-ttu-id="78cd6-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="78cd6-106">Windows Server 2016</span></span>

<span data-ttu-id="78cd6-107">Egyéb Windows-ügyfelein, lásd: [automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépek tooAzure AD a Windows régebbi verziójú kliensek](active-directory-device-registration-troubleshoot-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="78cd6-107">For other Windows clients, see [Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients](active-directory-device-registration-troubleshoot-windows-legacy.md).</span></span>

<span data-ttu-id="78cd6-108">Ez a témakör feltételezi, hogy konfigurálta a tartományhoz csatlakoztatott eszközök automatikus regisztráció megfelelően ismertetett [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-device-registration-get-started.md) a következő forgatókönyvek toosupport hello:</span><span class="sxs-lookup"><span data-stu-id="78cd6-108">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md) toosupport hello following scenarios:</span></span>

- [<span data-ttu-id="78cd6-109">Eszközalapú feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="78cd6-109">Device-based conditional access</span></span>](active-directory-conditional-access-automatic-device-registration-setup.md)

- [<span data-ttu-id="78cd6-110">Vállalati központi beállítások</span><span class="sxs-lookup"><span data-stu-id="78cd6-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="78cd6-111">Vállalati Windows Hello</span><span class="sxs-lookup"><span data-stu-id="78cd6-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="78cd6-112">Ez a dokumentum hogyan tooresolve potenciális problémák nyújt hibaelhárítási útmutatót.</span><span class="sxs-lookup"><span data-stu-id="78cd6-112">This document provides troubleshooting guidance on how tooresolve potential issues.</span></span> 

<span data-ttu-id="78cd6-113">hello regisztrációs támogatott a hello Windows 2015. November 10. frissítés vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="78cd6-113">hello registration is supported in hello Windows 10 November 2015 Update and above.</span></span>  
<span data-ttu-id="78cd6-114">Hello évforduló frissítés engedélyezéséhez a fenti helyzetekben hello használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="78cd6-114">We recommend using hello Anniversary Update for enabling hello scenarios above.</span></span>

## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="78cd6-115">1. lépés: Hello regisztrációs állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="78cd6-115">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="78cd6-116">**tooretrieve hello regisztrációs állapotát:**</span><span class="sxs-lookup"><span data-stu-id="78cd6-116">**tooretrieve hello registration status:**</span></span>

1. <span data-ttu-id="78cd6-117">Nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="78cd6-117">Open hello command prompt as an administrator.</span></span>

2. <span data-ttu-id="78cd6-118">Típus **dsregcmd/status**</span><span class="sxs-lookup"><span data-stu-id="78cd6-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="78cd6-119">+----------------------------------------------------------------------+
   | Az eszköz állapotát |}+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="78cd6-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined : YES
     <span data-ttu-id="78cd6-120">EnterpriseJoined: Nincs DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 ujjlenyomat: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform titkosításszolgáltató TpmProtected: Igen KeySignTest:: futtassa emelt szintű kell tootest.</span><span class="sxs-lookup"><span data-stu-id="78cd6-120">EnterpriseJoined : NO DeviceId : 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint : B753A6679CE720451921302CA873794D94C6204A KeyContainerId : bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider : Microsoft Platform Crypto Provider TpmProtected : YES KeySignTest: : MUST Run elevated tootest.</span></span>
                  <span data-ttu-id="78cd6-121">IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-Beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0-ás JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0-ás KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Igen tartománynév: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="78cd6-121">Idp : login.windows.net TenantId : 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName : Contoso AuthCodeUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl : eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion : 1.0 JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion : 1.0 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId : urn:ms-drs:enterpriseregistration.windows.net DomainJoined : YES DomainName : CONTOSO</span></span>
    
    <span data-ttu-id="78cd6-122">+----------------------------------------------------------------------+
   | A felhasználói állapot |}+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="78cd6-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    <span data-ttu-id="78cd6-123">WamDefaultAuthority: szervezetek WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Igen</span><span class="sxs-lookup"><span data-stu-id="78cd6-123">WamDefaultAuthority : organizations         WamDefaultId : https://login.microsoft.com       WamDefaultGUID : {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt : YES</span></span>



## <a name="step-2-evaluate-hello-registration-status"></a><span data-ttu-id="78cd6-124">2. lépés: Hello regisztrációs állapotának kiértékelésére.</span><span class="sxs-lookup"><span data-stu-id="78cd6-124">Step 2: Evaluate hello registration status</span></span> 

<span data-ttu-id="78cd6-125">Tekintse át a következő mezők hello, és győződjön meg arról, hogy rendelkezik-e hello várt értékek:</span><span class="sxs-lookup"><span data-stu-id="78cd6-125">Review hello following fields and make sure that they have hello expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="78cd6-126">AzureAdJoined: Igen</span><span class="sxs-lookup"><span data-stu-id="78cd6-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="78cd6-127">Ez a mező jeleníti meg, hogy hello eszköz regisztrálva van-e az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="78cd6-127">This field shows whether hello device is registered with Azure AD.</span></span> <span data-ttu-id="78cd6-128">Hello értéket jeleníti meg, mint a "Nem", ha regisztrációs nem fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="78cd6-128">If hello value shows as ‘NO’, registration has not completed.</span></span> 

<span data-ttu-id="78cd6-129">**Lehetséges okok:**</span><span class="sxs-lookup"><span data-stu-id="78cd6-129">**Possible causes:**</span></span>

- <span data-ttu-id="78cd6-130">Hello számítógép regisztrálása nem sikerült a hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="78cd6-130">Authentication of hello computer for registration failed.</span></span>

- <span data-ttu-id="78cd6-131">HTTP-proxy van hello olyan szervezet, amely nem észlelhetők hello számítógép</span><span class="sxs-lookup"><span data-stu-id="78cd6-131">There is an HTTP proxy in hello organization that cannot be discovered by hello computer</span></span>

- <span data-ttu-id="78cd6-132">hello számítógép nem érhető el, az Azure AD hitelesítésében vagy Azure DRS a regisztrációhoz</span><span class="sxs-lookup"><span data-stu-id="78cd6-132">hello computer cannot reach Azure AD for authentication or Azure DRS for registration</span></span>

- <span data-ttu-id="78cd6-133">hello számítógép nem lehet hello szervezet belső hálózaton vagy a VPN közvetlen tooan a helyszíni AD-tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="78cd6-133">hello computer may not be on hello organization’s internal network or on VPN with direct line of sight tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="78cd6-134">Ha hello számítógép TPM-mel, valószínűleg rossz állapotban.</span><span class="sxs-lookup"><span data-stu-id="78cd6-134">If hello computer has a TPM, it may be in a bad state.</span></span>

- <span data-ttu-id="78cd6-135">Nem lehet egy helytelen konfiguráció, a szolgáltatások hello dokumentumban korábban feljegyzett, akkor kell tooverify újra.</span><span class="sxs-lookup"><span data-stu-id="78cd6-135">There may be a misconfiguration in services noted in hello document earlier that you will need tooverify again.</span></span> <span data-ttu-id="78cd6-136">Gyakori példák:</span><span class="sxs-lookup"><span data-stu-id="78cd6-136">Common examples are:</span></span>

    - <span data-ttu-id="78cd6-137">Az összevonási kiszolgálón nincs engedélyezve a WS-Trust végpontok</span><span class="sxs-lookup"><span data-stu-id="78cd6-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="78cd6-138">Az összevonási kiszolgáló integrált Windows-hitelesítés segítségével a hálózat nem teszi lehetővé bejövő hitelesítés számítógépekről.</span><span class="sxs-lookup"><span data-stu-id="78cd6-138">Your federation server may not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="78cd6-139">Nincs tooyour ellenőrzött tartomány nevét az Azure AD oldalra mutat, ahol hello a számítógép tartozik hello AD-erdő szolgáltatáskapcsolódási pont objektum</span><span class="sxs-lookup"><span data-stu-id="78cd6-139">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="78cd6-140">DomainJoined: Igen</span><span class="sxs-lookup"><span data-stu-id="78cd6-140">DomainJoined : YES</span></span>  

<span data-ttu-id="78cd6-141">Ez a mező látható, hogy hello eszköz illesztett tooan a helyszíni Active Directory-e.</span><span class="sxs-lookup"><span data-stu-id="78cd6-141">This field shows whether hello device is joined tooan on-premises Active Directory or not.</span></span> <span data-ttu-id="78cd6-142">Amennyiben másként hello értéke **nem**, hello eszköz nem automatikus regisztrációját az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="78cd6-142">If hello value shows as **NO**, hello device cannot auto-register with Azure AD.</span></span> <span data-ttu-id="78cd6-143">Először ellenőrizze, hogy hello eszköz illesztések toohello a helyszíni Active Directory előtt is regisztrálja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="78cd6-143">Check first that hello device joins toohello on-premises Active Directory before it can register with Azure AD.</span></span> <span data-ttu-id="78cd6-144">Ha hello számítógép tooAzure AD közvetlenül csatlakoztatná keres, nyissa meg az Azure Active Directory csatlakozási képességekre vonatkozó tooLearn.</span><span class="sxs-lookup"><span data-stu-id="78cd6-144">If you are looking for joining hello computer tooAzure AD directly, please go tooLearn about capabilities of Azure Active Directory Join.</span></span>

---

### <a name="workplacejoined--no"></a><span data-ttu-id="78cd6-145">WorkplaceJoined: nincs</span><span class="sxs-lookup"><span data-stu-id="78cd6-145">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="78cd6-146">Ez a mező jeleníti meg, hogy hello eszköz regisztrálva van-e, és az Azure AD, de (munkahelyhez csatlakoztatott jelölésű) személyes eszközként.</span><span class="sxs-lookup"><span data-stu-id="78cd6-146">This field shows whether hello device is registered with Azure AD but as a personal device (marked as ‘Workplace Joined’).</span></span> <span data-ttu-id="78cd6-147">Ezt az értéket, a "Nem" jelenjen meg a tartományhoz csatlakoztatott számítógép regisztrálva az Azure AD, azonban azt mutatja, az azt jelenti, hogy Igen, mert egy munkahelyi vagy iskolai fiókot a hozzáadott előzetes toohello számítógép épp regisztrációs volt.</span><span class="sxs-lookup"><span data-stu-id="78cd6-147">If this value should show as ‘NO’ for a domain joined computer registered with Azure AD, however if it shows as YES it means that a work or school account was added prior toohello computer completing registration.</span></span> <span data-ttu-id="78cd6-148">Ebben az esetben hello fiókot figyelmen kívül ha Windows 10 (ha hello WinVer parancs futtatása hello a "Futtatás" vagy egy parancssori ablakot 1607) hello évforduló frissítés verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="78cd6-148">In this case hello account will be ignored if using hello Anniversary Update version of Windows 10 (1607 when running hello WinVer command in hello ‘Run’ window or a command prompt window).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="78cd6-149">WamDefaultSet: Igen és AzureADPrt: Igen</span><span class="sxs-lookup"><span data-stu-id="78cd6-149">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="78cd6-150">Ezek a mezők megjelenítése hello felhasználó sikeresen hitelesítette tooAzure AD toohello eszköz aláírásakor.</span><span class="sxs-lookup"><span data-stu-id="78cd6-150">These fields show that hello user has successfully authenticated tooAzure AD upon signing in toohello device.</span></span> <span data-ttu-id="78cd6-151">Ha azok megjelenítése "Nem" hello a következők okozhatják:</span><span class="sxs-lookup"><span data-stu-id="78cd6-151">If they show ‘NO’ hello following are possible causes:</span></span>

- <span data-ttu-id="78cd6-152">Hibás tárolási (STK) TPM hello eszköz regisztrálásakor (ellenőrzés hello KeySignTest emelt szintű futtatása közben) társított kulcs.</span><span class="sxs-lookup"><span data-stu-id="78cd6-152">Bad storage key (STK) in TPM associated with hello device upon registration (check hello KeySignTest while running elevated).</span></span>

- <span data-ttu-id="78cd6-153">Másodlagos bejelentkezési Azonosítóval</span><span class="sxs-lookup"><span data-stu-id="78cd6-153">Alternate Login ID</span></span>

- <span data-ttu-id="78cd6-154">HTTP-Proxy nem található.</span><span class="sxs-lookup"><span data-stu-id="78cd6-154">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="78cd6-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78cd6-155">Next steps</span></span>

<span data-ttu-id="78cd6-156">További információkért lásd: hello [automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="78cd6-156">For more information, see hello [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
