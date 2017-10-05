---
title: "A régebbi Windows-ügyfelek automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit |} Microsoft Docs"
description: "A régebbi Windows-ügyfelek automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit."
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
ms.openlocfilehash: a7c8ef4c59c53c21258f0c61963d8f994a3946ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad-for-windows-down-level-clients"></a><span data-ttu-id="1e44e-103">Automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépeit az Azure AD Windows régebbi ügyfelek</span><span class="sxs-lookup"><span data-stu-id="1e44e-103">Troubleshooting auto-registration of domain joined computers to Azure AD for Windows down-level clients</span></span> 

<span data-ttu-id="1e44e-104">Ez a témakör a tulajdonság csak a következő ügyfelek vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="1e44e-104">This topic is applicable only to the following clients:</span></span> 

- <span data-ttu-id="1e44e-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="1e44e-105">Windows 7</span></span> 
- <span data-ttu-id="1e44e-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="1e44e-106">Windows 8.1</span></span> 
- <span data-ttu-id="1e44e-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="1e44e-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="1e44e-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="1e44e-108">Windows Server 2012</span></span> 
- <span data-ttu-id="1e44e-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="1e44e-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="1e44e-110">Windows 10 és Windows Server 2016: [automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépeit az Azure AD – a Windows 10 és Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span><span class="sxs-lookup"><span data-stu-id="1e44e-110">For Windows 10 or Windows Server 2016, see [Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span></span>

<span data-ttu-id="1e44e-111">Ez a témakör feltételezi, hogy konfigurálta a tartományhoz csatlakoztatott eszközök automatikus regisztráció megfelelően ismertetett [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-device-registration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1e44e-111">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md).</span></span>
 
<span data-ttu-id="1e44e-112">Ez a témakör nyújt hibaelhárítási útmutatót a lehetséges problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="1e44e-112">This topic provides you with troubleshooting guidance on how to resolve potential issues.</span></span>  
<span data-ttu-id="1e44e-113">A sikeres eredményekkel ügyeljen a következőkre:</span><span class="sxs-lookup"><span data-stu-id="1e44e-113">Some things to note for successful outcomes:</span></span> 

- <span data-ttu-id="1e44e-114">Az ügyfelek az Azure AD-regisztráció van felhasználó/eszköz.</span><span class="sxs-lookup"><span data-stu-id="1e44e-114">Registration of these clients on Azure AD is per user/device.</span></span> <span data-ttu-id="1e44e-115">Példa: jdoe és jharnett jelentkezzen be az eszközt, ha ezeket a felhasználókat, a felhasználó adatai lap egy különálló regisztrációs (DeviceID) van-e létre.</span><span class="sxs-lookup"><span data-stu-id="1e44e-115">As an example: If jdoe and jharnett log in to this device, a separate registration (DeviceID) is created for each of these users in the USER info tab.</span></span>  

- <span data-ttu-id="1e44e-116">Ezek az ügyfelek a beépített nyilvántartási bejelentkezéskor vagy zárolt vagy feloldott próbálja van konfigurálva, és lehet, hogy ez akkor váltódik ki, a Feladatütemező feladat 5 perces késleltetés.</span><span class="sxs-lookup"><span data-stu-id="1e44e-116">Registration of these clients out of the box is configured to try at either logon or lock/unlock and there could be 5-minute delay that this is triggered using a Task Scheduler task.</span></span> 

- <span data-ttu-id="1e44e-117">Egy telepítse újra az operációs rendszer vagy egy manuális regisztrációját és regisztrálja újra az lehet, hogy hozzon létre egy új regisztrációs Azure ad-val, és hatására a több bejegyzést a felhasználó adatai lap az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1e44e-117">A re-install of the operating system or a manual un-register and re-register may create a new registration on Azure AD and will result in multiple entries under the USER info tab in the Azure portal.</span></span> 


## <a name="step-1-retrieve-the-registration-status"></a><span data-ttu-id="1e44e-118">1. lépés: A regisztráció állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="1e44e-118">Step 1: Retrieve the registration status</span></span> 

<span data-ttu-id="1e44e-119">**A regisztráció állapotának ellenőrzése:**</span><span class="sxs-lookup"><span data-stu-id="1e44e-119">**To verify the registration status:**</span></span>  

1. <span data-ttu-id="1e44e-120">Nyissa meg a parancssort rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="1e44e-120">Open the command prompt as an administrator</span></span> 

2. <span data-ttu-id="1e44e-121">Típusa`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="1e44e-121">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="1e44e-122">A parancs egy párbeszédpanelt, amely lehetővé teszi az illesztési állapotával kapcsolatos további adatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1e44e-122">This command displays a dialog box that provides you with more details about the join status.</span></span>

![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-registration-status"></a><span data-ttu-id="1e44e-124">2. lépés: A regisztráció állapotának kiértékelésére.</span><span class="sxs-lookup"><span data-stu-id="1e44e-124">Step 2: Evaluate the registration status</span></span> 

<span data-ttu-id="1e44e-125">Ha a csatlakozás sikertelen volt, a párbeszédpanel biztosít információkhoz juthat a problémáról történt.</span><span class="sxs-lookup"><span data-stu-id="1e44e-125">If the join was not successful, the dialog box provides you with details about the issue that has occured.</span></span>

<span data-ttu-id="1e44e-126">**A leggyakoribb problémák vannak:**</span><span class="sxs-lookup"><span data-stu-id="1e44e-126">**The most common issues are:**</span></span>

- <span data-ttu-id="1e44e-127">Egy helytelenül konfigurált AD FS vagy az Azure AD</span><span class="sxs-lookup"><span data-stu-id="1e44e-127">A misconfigured AD FS or Azure AD</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="1e44e-129">Nincs bejelentkezve tartományi felhasználóként</span><span class="sxs-lookup"><span data-stu-id="1e44e-129">You are not signed on as a domain user</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="1e44e-131">A kvóta elérve</span><span class="sxs-lookup"><span data-stu-id="1e44e-131">A quota has been reached</span></span>

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="1e44e-133">A szolgáltatás nem válaszol.</span><span class="sxs-lookup"><span data-stu-id="1e44e-133">The service is not responding</span></span> 

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="1e44e-135">Az állapot információt az eseménynaplóban a is talál **alkalmazások és szolgáltatások Log\Microsoft-munkahelyi csatlakoztatás**.</span><span class="sxs-lookup"><span data-stu-id="1e44e-135">You can also find the status information in the event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="1e44e-136">**Sikertelen regisztráció leggyakoribb okai a következők:**</span><span class="sxs-lookup"><span data-stu-id="1e44e-136">**The most common causes for a failed registration are:**</span></span> 

- <span data-ttu-id="1e44e-137">A számítógép nincs a szervezet belső hálózaton vagy egy helyszíni kapcsolat nélkül VPN AD-tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="1e44e-137">Your computer is not on the organization’s internal network or a VPN without connection to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="1e44e-138">A számítógép helyi fiókkal jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="1e44e-138">You are logged on to your computer with a local computer account.</span></span> 

- <span data-ttu-id="1e44e-139">Szolgáltatás konfigurációs problémák:</span><span class="sxs-lookup"><span data-stu-id="1e44e-139">Service configuration issues:</span></span> 

  - <span data-ttu-id="1e44e-140">Az összevonási kiszolgáló konfigurációja támogatja **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="1e44e-140">The federation server has been configured to support **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="1e44e-141">Nincs mutat, a ellenőrzött tartomány nevét az Azure ad-ben az Active Directory-erdőben, ahol a számítógép tartozik szolgáltatáskapcsolódási pont objektum.</span><span class="sxs-lookup"><span data-stu-id="1e44e-141">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to.</span></span>

  - <span data-ttu-id="1e44e-142">A felhasználó elérte a határértéket, az eszközök.</span><span class="sxs-lookup"><span data-stu-id="1e44e-142">A user has reached the limit of devices.</span></span> <span data-ttu-id="1e44e-143">Lásd: Ismerkedés az Azure Active Directory Eszközregisztráció.</span><span class="sxs-lookup"><span data-stu-id="1e44e-143">Please see Get Started with Azure Active Directory Device Registration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e44e-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e44e-144">Next steps</span></span>

<span data-ttu-id="1e44e-145">További információkért lásd: a [automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="1e44e-145">For more information, see the [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
