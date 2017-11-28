---
title: "aaaTroubleshooting hello automatikus regisztráció az Azure AD-tartomány csatlakoztatott számítógépek Windows régebbi ügyfelekhez |} Microsoft Docs"
description: "Windows-kezelés régebbi ügyfelek hello automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit."
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
ms.openlocfilehash: 84fe666576f13de09d1eaa5692517d45a4dbeebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad-for-windows-down-level-clients"></a><span data-ttu-id="14068-103">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD Windows régebbi ügyfelek</span><span class="sxs-lookup"><span data-stu-id="14068-103">Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients</span></span> 

<span data-ttu-id="14068-104">Ez a témakör a következő ügyfelek alkalmazható csak toohello:</span><span class="sxs-lookup"><span data-stu-id="14068-104">This topic is applicable only toohello following clients:</span></span> 

- <span data-ttu-id="14068-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="14068-105">Windows 7</span></span> 
- <span data-ttu-id="14068-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="14068-106">Windows 8.1</span></span> 
- <span data-ttu-id="14068-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="14068-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="14068-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="14068-108">Windows Server 2012</span></span> 
- <span data-ttu-id="14068-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="14068-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="14068-110">Windows 10 és Windows Server 2016: [automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépek tooAzure AD – Windows 10 és Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span><span class="sxs-lookup"><span data-stu-id="14068-110">For Windows 10 or Windows Server 2016, see [Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span></span>

<span data-ttu-id="14068-111">Ez a témakör feltételezi, hogy konfigurálta a tartományhoz csatlakoztatott eszközök automatikus regisztráció megfelelően ismertetett [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-device-registration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="14068-111">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md).</span></span>
 
<span data-ttu-id="14068-112">Ez a témakör útmutatást hogyan tooresolve lehetőségeket kínál a problémák hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="14068-112">This topic provides you with troubleshooting guidance on how tooresolve potential issues.</span></span>  
<span data-ttu-id="14068-113">Néhány dolgot toonote a sikeres eredményekkel:</span><span class="sxs-lookup"><span data-stu-id="14068-113">Some things toonote for successful outcomes:</span></span> 

- <span data-ttu-id="14068-114">Az ügyfelek az Azure AD-regisztráció van felhasználó/eszköz.</span><span class="sxs-lookup"><span data-stu-id="14068-114">Registration of these clients on Azure AD is per user/device.</span></span> <span data-ttu-id="14068-115">Példa: Ha jdoe és jharnett bejelentkezéskor toothis eszköz, a különböző regisztrációs (DeviceID) hoz létre minden egyes ezen felhasználók hello felhasználó adatai lap.</span><span class="sxs-lookup"><span data-stu-id="14068-115">As an example: If jdoe and jharnett log in toothis device, a separate registration (DeviceID) is created for each of these users in hello USER info tab.</span></span>  

- <span data-ttu-id="14068-116">Ezen ügyfelek hello kezdő verzióról regisztrációs bejelentkezéskor vagy zárolt vagy feloldott konfigurált tootry, és lehet, hogy ez akkor váltódik ki, a Feladatütemező feladat 5 perces késleltetés.</span><span class="sxs-lookup"><span data-stu-id="14068-116">Registration of these clients out of hello box is configured tootry at either logon or lock/unlock and there could be 5-minute delay that this is triggered using a Task Scheduler task.</span></span> 

- <span data-ttu-id="14068-117">Egy telepítse újra a hello operációs rendszer vagy egy manuális regisztrációját és regisztrálja újra az lehet, hogy hozzon létre egy új regisztrációs Azure ad-val, és hatására a több bejegyzés hello felhasználó adatai lap hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="14068-117">A re-install of hello operating system or a manual un-register and re-register may create a new registration on Azure AD and will result in multiple entries under hello USER info tab in hello Azure portal.</span></span> 


## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="14068-118">1. lépés: Hello regisztrációs állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="14068-118">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="14068-119">**tooverify hello regisztrációs állapotát:**</span><span class="sxs-lookup"><span data-stu-id="14068-119">**tooverify hello registration status:**</span></span>  

1. <span data-ttu-id="14068-120">Nyissa meg hello parancssort rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="14068-120">Open hello command prompt as an administrator</span></span> 

2. <span data-ttu-id="14068-121">Típusa`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="14068-121">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="14068-122">A parancs egy párbeszédpanelt, amely biztosít hello illesztési állapotával kapcsolatos további adatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="14068-122">This command displays a dialog box that provides you with more details about hello join status.</span></span>

![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-registration-status"></a><span data-ttu-id="14068-124">2. lépés: Hello regisztrációs állapotának kiértékelésére.</span><span class="sxs-lookup"><span data-stu-id="14068-124">Step 2: Evaluate hello registration status</span></span> 

<span data-ttu-id="14068-125">Ha hello csatlakoztatás sikertelen volt, hello párbeszédpanel biztosít hello probléma történt részleteit.</span><span class="sxs-lookup"><span data-stu-id="14068-125">If hello join was not successful, hello dialog box provides you with details about hello issue that has occured.</span></span>

<span data-ttu-id="14068-126">**hello kapcsolatos leggyakoribb hibák a következők:**</span><span class="sxs-lookup"><span data-stu-id="14068-126">**hello most common issues are:**</span></span>

- <span data-ttu-id="14068-127">Egy helytelenül konfigurált AD FS vagy az Azure AD</span><span class="sxs-lookup"><span data-stu-id="14068-127">A misconfigured AD FS or Azure AD</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="14068-129">Nincs bejelentkezve tartományi felhasználóként</span><span class="sxs-lookup"><span data-stu-id="14068-129">You are not signed on as a domain user</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="14068-131">A kvóta elérve</span><span class="sxs-lookup"><span data-stu-id="14068-131">A quota has been reached</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="14068-133">hello szolgáltatás nem válaszol</span><span class="sxs-lookup"><span data-stu-id="14068-133">hello service is not responding</span></span> 

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="14068-135">Hello állapotadatokat is található eseménynaplóban hello **alkalmazások és szolgáltatások Log\Microsoft-munkahelyi csatlakoztatás**.</span><span class="sxs-lookup"><span data-stu-id="14068-135">You can also find hello status information in hello event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="14068-136">**Sikertelen regisztráció a hello leggyakoribb okok a következők:**</span><span class="sxs-lookup"><span data-stu-id="14068-136">**hello most common causes for a failed registration are:**</span></span> 

- <span data-ttu-id="14068-137">A számítógép be kapcsolva nem hello vállalati belső hálózaton vagy egy virtuális Magánhálózati kapcsolat tooan nélkül a helyszíni AD-tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="14068-137">Your computer is not on hello organization’s internal network or a VPN without connection tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="14068-138">Tooyour számítógép helyi fiókkal van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="14068-138">You are logged on tooyour computer with a local computer account.</span></span> 

- <span data-ttu-id="14068-139">Szolgáltatás konfigurációs problémák:</span><span class="sxs-lookup"><span data-stu-id="14068-139">Service configuration issues:</span></span> 

  - <span data-ttu-id="14068-140">hello összevonási kiszolgáló lett konfigurált toosupport **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="14068-140">hello federation server has been configured toosupport **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="14068-141">Nincs tooyour ellenőrzött tartomány nevét az Azure AD oldalra mutat, ahol hello a számítógép tartozik hello AD-erdő szolgáltatáskapcsolódási pont objektum.</span><span class="sxs-lookup"><span data-stu-id="14068-141">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to.</span></span>

  - <span data-ttu-id="14068-142">A felhasználó elérte eszközök hello korlátot.</span><span class="sxs-lookup"><span data-stu-id="14068-142">A user has reached hello limit of devices.</span></span> <span data-ttu-id="14068-143">Lásd: Ismerkedés az Azure Active Directory Eszközregisztráció.</span><span class="sxs-lookup"><span data-stu-id="14068-143">Please see Get Started with Azure Active Directory Device Registration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14068-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14068-144">Next steps</span></span>

<span data-ttu-id="14068-145">További információkért lásd: hello [automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="14068-145">For more information, see hello [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
