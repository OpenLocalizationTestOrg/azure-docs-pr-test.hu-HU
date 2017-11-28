---
title: "aaaTroubleshooting hibrid Azure Active Directoryhoz csatlakoztatott Windows 10 és Windows Server 2016-os eszközök |} Microsoft Docs"
description: "Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott Windows 10 és Windows Server 2016-os eszközök."
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
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: cc252d1d0684d6632694afc8a367327794228c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a><span data-ttu-id="2323d-103">Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott Windows 10 és Windows Server 2016-os eszközök</span><span class="sxs-lookup"><span data-stu-id="2323d-103">Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices</span></span> 

<span data-ttu-id="2323d-104">Ez a témakör a következő ügyfelek alkalmazható toohello:</span><span class="sxs-lookup"><span data-stu-id="2323d-104">This topic is applicable toohello following clients:</span></span>

-   <span data-ttu-id="2323d-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="2323d-105">Windows 10</span></span>
-   <span data-ttu-id="2323d-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="2323d-106">Windows Server 2016</span></span>

<span data-ttu-id="2323d-107">Egyéb Windows-ügyfelein, lásd: [hibaelhárítás hibrid Azure Active Directoryhoz csatlakoztatott régebbi eszközök](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="2323d-107">For other Windows clients, see [Troubleshooting hybrid Azure Active Directory joined down-level devices](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span></span>

<span data-ttu-id="2323d-108">Ez a témakör azt feltételezi, hogy [konfigurált hibrid Azure Active Directoryhoz csatlakoztatott eszközök](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="2323d-108">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello following scenarios:</span></span>

- <span data-ttu-id="2323d-109">Eszközalapú feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="2323d-109">Device-based conditional access</span></span>

- [<span data-ttu-id="2323d-110">Vállalati központi beállítások</span><span class="sxs-lookup"><span data-stu-id="2323d-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="2323d-111">Vállalati Windows Hello</span><span class="sxs-lookup"><span data-stu-id="2323d-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="2323d-112">Ez a dokumentum hogyan tooresolve potenciális problémák nyújt hibaelhárítási útmutatót.</span><span class="sxs-lookup"><span data-stu-id="2323d-112">This document provides troubleshooting guidance on how tooresolve potential issues.</span></span> 


<span data-ttu-id="2323d-113">Windows 10 és Windows Server 2016, az Azure Active Directory join támogatja hello Windows hibrid 2015. November 10. Frissítse vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="2323d-113">For Windows 10 and Windows Server 2016, hybrid Azure Active Directory join supports hello Windows 10 November 2015 Update and above.</span></span> <span data-ttu-id="2323d-114">Hello évforduló frissítés használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="2323d-114">We recommend using hello Anniversary update.</span></span>

## <a name="step-1-retrieve-hello-join-status"></a><span data-ttu-id="2323d-115">1. lépés: Hello illesztési állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="2323d-115">Step 1: Retrieve hello join status</span></span> 

<span data-ttu-id="2323d-116">**tooretrieve hello illesztési állapota:**</span><span class="sxs-lookup"><span data-stu-id="2323d-116">**tooretrieve hello join status:**</span></span>

1. <span data-ttu-id="2323d-117">Nyissa meg hello parancssort rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="2323d-117">Open hello command prompt as an administrator</span></span>

2. <span data-ttu-id="2323d-118">Típus **dsregcmd/status**</span><span class="sxs-lookup"><span data-stu-id="2323d-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="2323d-119">+----------------------------------------------------------------------+
   | Az eszköz állapotát |}+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="2323d-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined: YES
     <span data-ttu-id="2323d-120">EnterpriseJoined: Nincs DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 ujjlenyomat: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform titkosításszolgáltató TpmProtected: Igen KeySignTest:: futtassa emelt szintű kell tootest.</span><span class="sxs-lookup"><span data-stu-id="2323d-120">EnterpriseJoined: NO DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform Crypto Provider TpmProtected: YES KeySignTest: : MUST Run elevated tootest.</span></span>
                  <span data-ttu-id="2323d-121">IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0-ás JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0-ás KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Igen tartománynév: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="2323d-121">Idp: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn:ms-drs:enterpriseregistration.windows.net DomainJoined: YES DomainName: CONTOSO</span></span>
    
    <span data-ttu-id="2323d-122">+----------------------------------------------------------------------+
   | A felhasználói állapot |}+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="2323d-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    <span data-ttu-id="2323d-123">WamDefaultAuthority: szervezetek WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Igen</span><span class="sxs-lookup"><span data-stu-id="2323d-123">WamDefaultAuthority: organizations         WamDefaultId: https://login.microsoft.com       WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt: YES</span></span>



## <a name="step-2-evaluate-hello-join-status"></a><span data-ttu-id="2323d-124">2. lépés: Hello illesztési állapotának kiértékelésére.</span><span class="sxs-lookup"><span data-stu-id="2323d-124">Step 2: Evaluate hello join status</span></span> 

<span data-ttu-id="2323d-125">Tekintse át a következő mezők hello, és győződjön meg arról, hogy rendelkezik-e hello várt értékek:</span><span class="sxs-lookup"><span data-stu-id="2323d-125">Review hello following fields and make sure that they have hello expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="2323d-126">AzureAdJoined: Igen</span><span class="sxs-lookup"><span data-stu-id="2323d-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="2323d-127">Ez a mező jelzi, hogy hello eszköz csatlakozott-e az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="2323d-127">This field indicates whether hello device is joined with Azure AD.</span></span> <span data-ttu-id="2323d-128">Ha hello érték **nem**, hello illesztési tooAzure AD még nem fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="2323d-128">If hello value is **NO**, hello join tooAzure AD has not completed yet.</span></span> 

<span data-ttu-id="2323d-129">**Lehetséges okok:**</span><span class="sxs-lookup"><span data-stu-id="2323d-129">**Possible causes:**</span></span>

- <span data-ttu-id="2323d-130">Az illesztés hello számítógép hitelesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2323d-130">Authentication of hello computer for a join failed.</span></span>

- <span data-ttu-id="2323d-131">HTTP-proxy van hello olyan szervezet, amely nem észlelhetők hello számítógép</span><span class="sxs-lookup"><span data-stu-id="2323d-131">There is an HTTP proxy in hello organization that cannot be discovered by hello computer</span></span>

- <span data-ttu-id="2323d-132">hello számítógép nem érhető el az Azure AD tooauthenticate vagy Azure DRS a regisztrációhoz</span><span class="sxs-lookup"><span data-stu-id="2323d-132">hello computer cannot reach Azure AD tooauthenticate or Azure DRS for registration</span></span>

- <span data-ttu-id="2323d-133">hello számítógép nem lehet hello szervezet belső hálózaton vagy a VPN közvetlen tooan a helyszíni AD-tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="2323d-133">hello computer may not be on hello organization’s internal network or on VPN with direct line of sight tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="2323d-134">Ha hello számítógép TPM-mel, az állapota lehet.</span><span class="sxs-lookup"><span data-stu-id="2323d-134">If hello computer has a TPM, it can be in a bad state.</span></span>

- <span data-ttu-id="2323d-135">Valószínűleg a helytelen konfiguráció hello szolgáltatásban hello dokumentumban korábban feljegyzett, akkor kell tooverify újra.</span><span class="sxs-lookup"><span data-stu-id="2323d-135">There might be a misconfiguration in hello services noted in hello document earlier that you will need tooverify again.</span></span> <span data-ttu-id="2323d-136">Gyakori példák:</span><span class="sxs-lookup"><span data-stu-id="2323d-136">Common examples are:</span></span>

    - <span data-ttu-id="2323d-137">Az összevonási kiszolgálón nincs engedélyezve a WS-Trust végpontok</span><span class="sxs-lookup"><span data-stu-id="2323d-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="2323d-138">Az összevonási kiszolgálón nem engedélyezi a bejövő hitelesítést számítógépekről integrált Windows-hitelesítés segítségével a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="2323d-138">Your federation server does not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="2323d-139">Nincs tooyour ellenőrzött tartomány nevét az Azure AD oldalra mutat, ahol hello a számítógép tartozik hello AD-erdő szolgáltatáskapcsolódási pont objektum</span><span class="sxs-lookup"><span data-stu-id="2323d-139">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="2323d-140">DomainJoined: Igen</span><span class="sxs-lookup"><span data-stu-id="2323d-140">DomainJoined : YES</span></span>  

<span data-ttu-id="2323d-141">Ez a mező azt jelzi, hogy hello eszköz tooan a helyszíni Active Directory-e.</span><span class="sxs-lookup"><span data-stu-id="2323d-141">This field indicates whether hello device is joined tooan on-premises Active Directory or not.</span></span> <span data-ttu-id="2323d-142">Ha hello érték **nem**, hello eszköz nem tudja végrehajtani a Azure AD hibrid csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="2323d-142">If hello value is **NO**, hello device cannot perform a hybrid Azure AD join.</span></span>  

---

### <a name="workplacejoined--no"></a><span data-ttu-id="2323d-143">WorkplaceJoined: nincs</span><span class="sxs-lookup"><span data-stu-id="2323d-143">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="2323d-144">Ez a mező jelzi, hogy hello eszköz regisztrálva van-e az Azure AD egy személyes eszközként (jelölésű *munkahelyhez csatlakoztatott*).</span><span class="sxs-lookup"><span data-stu-id="2323d-144">This field indicates whether hello device is registered with Azure AD as a personal device (marked as *Workplace Joined*).</span></span> <span data-ttu-id="2323d-145">Ez az érték legyen **nem** egy tartományhoz csatlakozó számítógép, amely egyúttal az Azure AD hibrid csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="2323d-145">This value should be **NO** for a domain-joined computer that is also hybrid Azure AD joined.</span></span> <span data-ttu-id="2323d-146">Ha hello érték **Igen**, a munkahelyi vagy iskolai fiók hozzá lett adva hello hibrid az Azure AD join előzetes toohello befejezését.</span><span class="sxs-lookup"><span data-stu-id="2323d-146">If hello value is **YES**, a work or school account was added prior toohello completion of hello hybrid Azure AD join.</span></span> <span data-ttu-id="2323d-147">Ebben az esetben hello fiók rendszer figyelmen kívül hagyja a Windows 10 (1607) hello évforduló frissítés verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="2323d-147">In this case, hello account is ignored when using hello Anniversary Update version of Windows 10 (1607).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="2323d-148">WamDefaultSet: Igen és AzureADPrt: Igen</span><span class="sxs-lookup"><span data-stu-id="2323d-148">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="2323d-149">Ezek a mezők azt jelzi, hogy hello felhasználó sikeresen hitelesített tooAzure AD toohello eszköz bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="2323d-149">These fields indicate whether hello user has successfully authenticated tooAzure AD when signing in toohello device.</span></span> <span data-ttu-id="2323d-150">Ha hello értékek **nem**, annak oka az lehet esedékes:</span><span class="sxs-lookup"><span data-stu-id="2323d-150">If hello values are **NO**, it could be due:</span></span>

- <span data-ttu-id="2323d-151">Hibás tárolási (STK) TPM hello eszköz regisztrálásakor (ellenőrzés hello KeySignTest emelt szintű futtatása közben) társított kulcs.</span><span class="sxs-lookup"><span data-stu-id="2323d-151">Bad storage key (STK) in TPM associated with hello device upon registration (check hello KeySignTest while running elevated).</span></span>

- <span data-ttu-id="2323d-152">Másodlagos bejelentkezési Azonosítóval</span><span class="sxs-lookup"><span data-stu-id="2323d-152">Alternate Login ID</span></span>

- <span data-ttu-id="2323d-153">HTTP-Proxy nem található.</span><span class="sxs-lookup"><span data-stu-id="2323d-153">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="2323d-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2323d-154">Next steps</span></span>

<span data-ttu-id="2323d-155">A kérdésekhez lásd: hello [eszköz felügyeleti kapcsolatos gyakori kérdések](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="2323d-155">For questions, see hello [device management FAQ](device-management-faq.md)</span></span> 
