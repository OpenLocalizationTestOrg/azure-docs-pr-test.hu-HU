---
title: "Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott Windows 10 és Windows Server 2016-os eszközök |} Microsoft Docs"
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
ms.openlocfilehash: 51962c14a3c32bbfa9a613fa203cc48cfea50c0b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a><span data-ttu-id="60a0f-103">Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott Windows 10 és Windows Server 2016-os eszközök</span><span class="sxs-lookup"><span data-stu-id="60a0f-103">Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices</span></span> 

<span data-ttu-id="60a0f-104">Ez a témakör a következő alkalmazható a következő ügyfelekre:</span><span class="sxs-lookup"><span data-stu-id="60a0f-104">This topic is applicable to the following clients:</span></span>

-   <span data-ttu-id="60a0f-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="60a0f-105">Windows 10</span></span>
-   <span data-ttu-id="60a0f-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="60a0f-106">Windows Server 2016</span></span>

<span data-ttu-id="60a0f-107">Egyéb Windows-ügyfelein, lásd: [hibaelhárítás hibrid Azure Active Directoryhoz csatlakoztatott régebbi eszközök](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="60a0f-107">For other Windows clients, see [Troubleshooting hybrid Azure Active Directory joined down-level devices](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span></span>

<span data-ttu-id="60a0f-108">Ez a témakör azt feltételezi, hogy [konfigurált hibrid Azure Active Directoryhoz csatlakoztatott eszközök](device-management-hybrid-azuread-joined-devices-setup.md) a következő forgatókönyvek támogatása céljából:</span><span class="sxs-lookup"><span data-stu-id="60a0f-108">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) to support the following scenarios:</span></span>

- <span data-ttu-id="60a0f-109">Eszközalapú feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="60a0f-109">Device-based conditional access</span></span>

- [<span data-ttu-id="60a0f-110">Vállalati központi beállítások</span><span class="sxs-lookup"><span data-stu-id="60a0f-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="60a0f-111">Vállalati Windows Hello</span><span class="sxs-lookup"><span data-stu-id="60a0f-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="60a0f-112">Ez a dokumentum a lehetséges problémák megoldásához nyújt hibaelhárítási útmutatót.</span><span class="sxs-lookup"><span data-stu-id="60a0f-112">This document provides troubleshooting guidance on how to resolve potential issues.</span></span> 


<span data-ttu-id="60a0f-113">Windows 10 és Windows Server 2016, az Azure Active Directory join hibrid támogatja, a Windows 2015. November 10. frissítés vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="60a0f-113">For Windows 10 and Windows Server 2016, hybrid Azure Active Directory join supports the Windows 10 November 2015 Update and above.</span></span> <span data-ttu-id="60a0f-114">A évforduló frissítés használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="60a0f-114">We recommend using the Anniversary update.</span></span>

## <a name="step-1-retrieve-the-join-status"></a><span data-ttu-id="60a0f-115">1. lépés: Az illesztés állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="60a0f-115">Step 1: Retrieve the join status</span></span> 

<span data-ttu-id="60a0f-116">**Az illesztési állapotának lekérése:**</span><span class="sxs-lookup"><span data-stu-id="60a0f-116">**To retrieve the join status:**</span></span>

1. <span data-ttu-id="60a0f-117">Nyissa meg a parancssort rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="60a0f-117">Open the command prompt as an administrator</span></span>

2. <span data-ttu-id="60a0f-118">Típus **dsregcmd/status**</span><span class="sxs-lookup"><span data-stu-id="60a0f-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="60a0f-119">+----------------------------------------------------------------------+
   | Az eszköz állapotát |}+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="60a0f-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined: YES
     <span data-ttu-id="60a0f-120">EnterpriseJoined: Nincs DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 ujjlenyomat: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform kriptográfiai szolgáltató TpmProtected: Igen KeySignTest:: futtatásához emelt szintű teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="60a0f-120">EnterpriseJoined: NO DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform Crypto Provider TpmProtected: YES KeySignTest: : MUST Run elevated to test.</span></span>
                  <span data-ttu-id="60a0f-121">IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0-ás JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0-ás KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Igen tartománynév: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="60a0f-121">Idp: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn:ms-drs:enterpriseregistration.windows.net DomainJoined: YES DomainName: CONTOSO</span></span>
    
    <span data-ttu-id="60a0f-122">+----------------------------------------------------------------------+
   | A felhasználói állapot |}+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="60a0f-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    <span data-ttu-id="60a0f-123">WamDefaultAuthority: szervezetek WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Igen</span><span class="sxs-lookup"><span data-stu-id="60a0f-123">WamDefaultAuthority: organizations         WamDefaultId: https://login.microsoft.com       WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt: YES</span></span>



## <a name="step-2-evaluate-the-join-status"></a><span data-ttu-id="60a0f-124">2. lépés: Csatlakozás állapotának kiértékelésére.</span><span class="sxs-lookup"><span data-stu-id="60a0f-124">Step 2: Evaluate the join status</span></span> 

<span data-ttu-id="60a0f-125">Tekintse át a következő mezőket, és győződjön meg arról, hogy rendelkezik-e a várt értékek:</span><span class="sxs-lookup"><span data-stu-id="60a0f-125">Review the following fields and make sure that they have the expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="60a0f-126">AzureAdJoined: Igen</span><span class="sxs-lookup"><span data-stu-id="60a0f-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="60a0f-127">Ez a mező jelzi, hogy az eszköz csatlakozott-e az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="60a0f-127">This field indicates whether the device is joined with Azure AD.</span></span> <span data-ttu-id="60a0f-128">Ha az érték **nem**, az Azure AD join még nem fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="60a0f-128">If the value is **NO**, the join to Azure AD has not completed yet.</span></span> 

<span data-ttu-id="60a0f-129">**Lehetséges okok:**</span><span class="sxs-lookup"><span data-stu-id="60a0f-129">**Possible causes:**</span></span>

- <span data-ttu-id="60a0f-130">A számítógép a csatlakozzon a hitelesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="60a0f-130">Authentication of the computer for a join failed.</span></span>

- <span data-ttu-id="60a0f-131">Nincs olyan szervezet, amely a számítógép nem észlelhetők egy HTTP-proxy</span><span class="sxs-lookup"><span data-stu-id="60a0f-131">There is an HTTP proxy in the organization that cannot be discovered by the computer</span></span>

- <span data-ttu-id="60a0f-132">A számítógép nem érhető el az Azure AD-hitelesítés vagy Azure DRS a regisztrációhoz</span><span class="sxs-lookup"><span data-stu-id="60a0f-132">The computer cannot reach Azure AD to authenticate or Azure DRS for registration</span></span>

- <span data-ttu-id="60a0f-133">A számítógép nem lehet a szervezet belső hálózaton vagy a VPN a közvetlen sor a láthatáron egy a helyszíni AD-tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="60a0f-133">The computer may not be on the organization’s internal network or on VPN with direct line of sight to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="60a0f-134">Ha a számítógép TPM-mel, az állapota lehet.</span><span class="sxs-lookup"><span data-stu-id="60a0f-134">If the computer has a TPM, it can be in a bad state.</span></span>

- <span data-ttu-id="60a0f-135">Előfordulhat, a szolgáltatások helytelen beállítása a dokumentum korábban feljegyzett, hogy újra ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="60a0f-135">There might be a misconfiguration in the services noted in the document earlier that you will need to verify again.</span></span> <span data-ttu-id="60a0f-136">Gyakori példák:</span><span class="sxs-lookup"><span data-stu-id="60a0f-136">Common examples are:</span></span>

    - <span data-ttu-id="60a0f-137">Az összevonási kiszolgálón nincs engedélyezve a WS-Trust végpontok</span><span class="sxs-lookup"><span data-stu-id="60a0f-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="60a0f-138">Az összevonási kiszolgálón nem engedélyezi a bejövő hitelesítést számítógépekről integrált Windows-hitelesítés segítségével a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="60a0f-138">Your federation server does not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="60a0f-139">Nincs szolgáltatáskapcsolódási pont objektum mutat, a ellenőrzött tartomány nevét az Azure ad-ben az Active Directory-erdőben, ahol a számítógép tartozik</span><span class="sxs-lookup"><span data-stu-id="60a0f-139">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="60a0f-140">DomainJoined: Igen</span><span class="sxs-lookup"><span data-stu-id="60a0f-140">DomainJoined : YES</span></span>  

<span data-ttu-id="60a0f-141">Ez a mező jelzi, hogy az eszköz csatlakozott a helyi Active Directory-e.</span><span class="sxs-lookup"><span data-stu-id="60a0f-141">This field indicates whether the device is joined to an on-premises Active Directory or not.</span></span> <span data-ttu-id="60a0f-142">Ha az érték **nem**, az eszköz nem tudja végrehajtani a Azure AD hibrid csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="60a0f-142">If the value is **NO**, the device cannot perform a hybrid Azure AD join.</span></span>  

---

### <a name="workplacejoined--no"></a><span data-ttu-id="60a0f-143">WorkplaceJoined: nincs</span><span class="sxs-lookup"><span data-stu-id="60a0f-143">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="60a0f-144">Ez a mező jelzi, hogy az eszköz regisztrálva van-e az Azure AD egy személyes eszközként (jelölésű *munkahelyhez csatlakoztatott*).</span><span class="sxs-lookup"><span data-stu-id="60a0f-144">This field indicates whether the device is registered with Azure AD as a personal device (marked as *Workplace Joined*).</span></span> <span data-ttu-id="60a0f-145">Ez az érték legyen **nem** egy tartományhoz csatlakozó számítógép, amely egyúttal az Azure AD hibrid csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="60a0f-145">This value should be **NO** for a domain-joined computer that is also hybrid Azure AD joined.</span></span> <span data-ttu-id="60a0f-146">Ha az érték **Igen**, a munkahelyi vagy iskolai fiók hozzá lett adva, az Azure AD hibrid csatlakozási befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="60a0f-146">If the value is **YES**, a work or school account was added prior to the completion of the hybrid Azure AD join.</span></span> <span data-ttu-id="60a0f-147">Ebben az esetben a fiók rendszer figyelmen kívül hagyja a Windows 10 (1607) évforduló frissítés verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="60a0f-147">In this case, the account is ignored when using the Anniversary Update version of Windows 10 (1607).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="60a0f-148">WamDefaultSet: Igen és AzureADPrt: Igen</span><span class="sxs-lookup"><span data-stu-id="60a0f-148">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="60a0f-149">Ezek a mezők azt jelzi, hogy az Azure AD rendelkezik sikeresen hitelesíteni a felhasználót az eszköz történő bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="60a0f-149">These fields indicate whether the user has successfully authenticated to Azure AD when signing in to the device.</span></span> <span data-ttu-id="60a0f-150">Ha az értékek **nem**, annak oka az lehet esedékes:</span><span class="sxs-lookup"><span data-stu-id="60a0f-150">If the values are **NO**, it could be due:</span></span>

- <span data-ttu-id="60a0f-151">Hibás tárolási kulcs (STK) az eszköz regisztrálásakor (Ellenőrizze az emelt szintű futtatása közben KeySignTest) társított TPM.</span><span class="sxs-lookup"><span data-stu-id="60a0f-151">Bad storage key (STK) in TPM associated with the device upon registration (check the KeySignTest while running elevated).</span></span>

- <span data-ttu-id="60a0f-152">Másodlagos bejelentkezési Azonosítóval</span><span class="sxs-lookup"><span data-stu-id="60a0f-152">Alternate Login ID</span></span>

- <span data-ttu-id="60a0f-153">HTTP-Proxy nem található.</span><span class="sxs-lookup"><span data-stu-id="60a0f-153">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="60a0f-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60a0f-154">Next steps</span></span>

<span data-ttu-id="60a0f-155">Kérdéseit, tekintse meg a [eszköz felügyeleti kapcsolatos gyakori kérdések](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="60a0f-155">For questions, see the [device management FAQ](device-management-faq.md)</span></span> 