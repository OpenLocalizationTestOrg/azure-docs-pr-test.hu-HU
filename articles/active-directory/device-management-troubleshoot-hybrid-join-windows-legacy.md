---
title: "Azure Active Directory aaaTroubleshooting hibrid csatlakoztatott régebbi eszközök |} Microsoft Docs"
description: "Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott régebbi eszközök."
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
ms.openlocfilehash: edd56b89579fac6b427732902284ad9c568b87b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a><span data-ttu-id="335fc-103">Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott régebbi eszközök</span><span class="sxs-lookup"><span data-stu-id="335fc-103">Troubleshooting hybrid Azure Active Directory joined down-level devices</span></span> 

<span data-ttu-id="335fc-104">Ez a témakör a következő eszközök alkalmazható csak toohello:</span><span class="sxs-lookup"><span data-stu-id="335fc-104">This topic is applicable only toohello following devices:</span></span> 

- <span data-ttu-id="335fc-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="335fc-105">Windows 7</span></span> 
- <span data-ttu-id="335fc-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="335fc-106">Windows 8.1</span></span> 
- <span data-ttu-id="335fc-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="335fc-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="335fc-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="335fc-108">Windows Server 2012</span></span> 
- <span data-ttu-id="335fc-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="335fc-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="335fc-110">Windows 10 és Windows Server 2016: [hibaelhárítás hibrid Azure Active Directoryhoz csatlakoztatott Windows 10 és Windows Server 2016-os eszközök](device-management-troubleshoot-hybrid-join-windows-current.md).</span><span class="sxs-lookup"><span data-stu-id="335fc-110">For Windows 10 or Windows Server 2016, see [Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices](device-management-troubleshoot-hybrid-join-windows-current.md).</span></span>

<span data-ttu-id="335fc-111">Ez a témakör azt feltételezi, hogy [konfigurált hibrid Azure Active Directoryhoz csatlakoztatott eszközök](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="335fc-111">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello following scenarios:</span></span>

- <span data-ttu-id="335fc-112">Eszközalapú feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="335fc-112">Device-based conditional access</span></span>

- [<span data-ttu-id="335fc-113">Vállalati központi beállítások</span><span class="sxs-lookup"><span data-stu-id="335fc-113">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="335fc-114">Vállalati Windows Hello</span><span class="sxs-lookup"><span data-stu-id="335fc-114">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md) 





<span data-ttu-id="335fc-115">Ez a témakör útmutatást hogyan tooresolve lehetőségeket kínál a problémák hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="335fc-115">This topic provides you with troubleshooting guidance on how tooresolve potential issues.</span></span>  

<span data-ttu-id="335fc-116">**Tudnivalók:**</span><span class="sxs-lookup"><span data-stu-id="335fc-116">**What you should know:**</span></span> 

- <span data-ttu-id="335fc-117">hello maximális száma felhasználónként eszközközpontú.</span><span class="sxs-lookup"><span data-stu-id="335fc-117">hello maximum number of devices per user is device-centric.</span></span> <span data-ttu-id="335fc-118">Például ha *jdoe* és *jharnett* bejelentkezési tooa eszköz, a különböző regisztrációs (DeviceID) hoz létre minden egyes azokat hello **felhasználói** információ lapon.</span><span class="sxs-lookup"><span data-stu-id="335fc-118">For example, if *jdoe* and *jharnett* sign-in tooa device, a separate registration (DeviceID) is created for each of them in hello **USER** info tab.</span></span>  

- <span data-ttu-id="335fc-119">kezdeti regisztrációs hello / join az eszközök az beállított tooperform egy kísérlet bejelentkezési vagy a zárolás / zárolásának feloldásához.</span><span class="sxs-lookup"><span data-stu-id="335fc-119">hello initial registration / join of devices is configured tooperform an attempt at either logon or lock / unlock.</span></span> <span data-ttu-id="335fc-120">A Feladatütemező által indított 5 perces késleltetés lehet.</span><span class="sxs-lookup"><span data-stu-id="335fc-120">There could be 5-minute delay triggered by a task scheduler task.</span></span> 

- <span data-ttu-id="335fc-121">Egy hello operációs rendszer vagy a manuális újratelepítés unregister és regisztrálja újra lehet, hogy hozzon létre egy új regisztrációs Azure ad-val és hello felhasználó adatai lap több bejegyzést eredményez hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="335fc-121">A reinstall of hello operating system or a manual unregister and re-register may create a new registration on Azure AD and results in multiple entries under hello USER info tab in hello Azure portal.</span></span> 


## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="335fc-122">1. lépés: Hello regisztrációs állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="335fc-122">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="335fc-123">**tooverify hello regisztrációs állapotát:**</span><span class="sxs-lookup"><span data-stu-id="335fc-123">**tooverify hello registration status:**</span></span>  

1. <span data-ttu-id="335fc-124">Nyissa meg hello parancssort rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="335fc-124">Open hello command prompt as an administrator</span></span> 

2. <span data-ttu-id="335fc-125">Típusa`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="335fc-125">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="335fc-126">A parancs egy párbeszédpanelt, amely biztosít hello illesztési állapotával kapcsolatos további adatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="335fc-126">This command displays a dialog box that provides you with more details about hello join status.</span></span>

![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-hybrid-azure-ad-join-status"></a><span data-ttu-id="335fc-128">2. lépés: Hello hibrid az Azure AD join állapotának kiértékelésére.</span><span class="sxs-lookup"><span data-stu-id="335fc-128">Step 2: Evaluate hello hybrid Azure AD join status</span></span> 

<span data-ttu-id="335fc-129">Ha hello hibrid az Azure AD join nem volt sikeres, hello párbeszédpanel biztosít hello probléma történt részleteit.</span><span class="sxs-lookup"><span data-stu-id="335fc-129">If hello hybrid Azure AD join was not successful, hello dialog box provides you with details about hello issue that has occurred.</span></span>

<span data-ttu-id="335fc-130">**hello kapcsolatos leggyakoribb hibák a következők:**</span><span class="sxs-lookup"><span data-stu-id="335fc-130">**hello most common issues are:**</span></span>

- <span data-ttu-id="335fc-131">Egy helytelenül konfigurált AD FS vagy az Azure AD</span><span class="sxs-lookup"><span data-stu-id="335fc-131">A misconfigured AD FS or Azure AD</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="335fc-133">Nincs bejelentkezve tartományi felhasználóként</span><span class="sxs-lookup"><span data-stu-id="335fc-133">You are not signed on as a domain user</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="335fc-135">A kvóta elérve</span><span class="sxs-lookup"><span data-stu-id="335fc-135">A quota has been reached</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="335fc-137">hello szolgáltatás nem válaszol</span><span class="sxs-lookup"><span data-stu-id="335fc-137">hello service is not responding</span></span> 

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="335fc-139">Hello állapotadatokat is található eseménynaplóban hello **alkalmazások és szolgáltatások Log\Microsoft-munkahelyi csatlakoztatás**.</span><span class="sxs-lookup"><span data-stu-id="335fc-139">You can also find hello status information in hello event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="335fc-140">**egy sikertelen a hibrid az Azure AD join hello leggyakoribb okai a következők:**</span><span class="sxs-lookup"><span data-stu-id="335fc-140">**hello most common causes for a failed hybrid Azure AD join are:**</span></span> 

- <span data-ttu-id="335fc-141">A számítógép be kapcsolva nem hello vállalati belső hálózaton vagy egy virtuális Magánhálózati kapcsolat tooan nélkül a helyszíni AD-tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="335fc-141">Your computer is not on hello organization’s internal network or a VPN without connection tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="335fc-142">Tooyour számítógép helyi fiókkal van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="335fc-142">You are logged on tooyour computer with a local computer account.</span></span> 

- <span data-ttu-id="335fc-143">Szolgáltatás konfigurációs problémák:</span><span class="sxs-lookup"><span data-stu-id="335fc-143">Service configuration issues:</span></span> 

  - <span data-ttu-id="335fc-144">hello összevonási kiszolgáló lett konfigurált toosupport **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="335fc-144">hello federation server has been configured toosupport **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="335fc-145">Nincs tooyour ellenőrzött tartomány nevét az Azure AD oldalra mutat, ahol hello a számítógép tartozik hello AD-erdő szolgáltatáskapcsolódási pont objektum.</span><span class="sxs-lookup"><span data-stu-id="335fc-145">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to.</span></span>

  - <span data-ttu-id="335fc-146">A felhasználó elérte eszközök hello korlátot.</span><span class="sxs-lookup"><span data-stu-id="335fc-146">A user has reached hello limit of devices.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="335fc-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="335fc-147">Next steps</span></span>

<span data-ttu-id="335fc-148">A kérdésekhez lásd: hello [eszköz felügyeleti kapcsolatos gyakori kérdések](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="335fc-148">For questions, see hello [device management FAQ](device-management-faq.md)</span></span>  
