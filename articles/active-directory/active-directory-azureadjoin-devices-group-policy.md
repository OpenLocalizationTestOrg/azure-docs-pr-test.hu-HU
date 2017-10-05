---
title: "Tartományhoz csatlakozó eszközök csatlakoztatása az Azure AD, a Windows 10 észlel |} Microsoft Docs"
description: "Ismerteti, hogyan rendszergazdák konfigurálhatják a csoportházirend segítségével engedélyezni kell a tartományhoz a vállalati hálózathoz."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2ff29f3e-5325-4f43-9baa-6ae8d6bad3e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 9c91579d20bb84701f6d0b97d944728c84044adf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a><span data-ttu-id="cf722-103">Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="cf722-103">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>
<span data-ttu-id="cf722-104">Tartományhoz való csatlakozást, a hagyományos módon szervezetek számára az elmúlt 15 éves és egyéb eszközök csatlakozott.</span><span class="sxs-lookup"><span data-stu-id="cf722-104">Domain join is the traditional way organizations have connected devices for work for the last 15 years and more.</span></span> <span data-ttu-id="cf722-105">Jelentkezzen be az eszközeiket a Windows Server Active Directory (az Active Directory) munkahelyi vagy iskolai fiókok felhasználók engedélyezve van és engedélyezett informatikai részleg teljes körű felügyeletéhez az ezeket az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="cf722-105">It has enabled users to sign in to their devices by using their Windows Server Active Directory (Active Directory) work or school accounts and allowed IT to fully manage these devices.</span></span> <span data-ttu-id="cf722-106">A szervezetek általában támaszkodhat a lemezkép-készítési módszerek eszközök beállításához a felhasználók számára, és általában használja a System Center Configuration Manager (SCCM) vagy a Csoportházirend kezelése.</span><span class="sxs-lookup"><span data-stu-id="cf722-106">Organizations typically rely on imaging methods to provision devices to users and generally use System Center Configuration Manager (SCCM) or Group Policy to manage them.</span></span>


<span data-ttu-id="cf722-107">Tartományhoz való csatlakozást, a Windows 10-es a következő előnyt biztosít az Azure Active Directory (Azure AD-) eszközök csatlakoztatása után:</span><span class="sxs-lookup"><span data-stu-id="cf722-107">Domain join in Windows 10 provides you with the following benefits after you connect devices to Azure Active Directory (Azure AD):</span></span>

* <span data-ttu-id="cf722-108">Egyszeri bejelentkezés (SSO) az Azure AD-erőforrások bárhonnan</span><span class="sxs-lookup"><span data-stu-id="cf722-108">Single sign-on (SSO) to Azure AD resources from anywhere</span></span>
* <span data-ttu-id="cf722-109">Hozzáférés a vállalati Windows áruház munkahelyi vagy iskolai fiókokkal (nincs Microsoft-fiók szükséges)</span><span class="sxs-lookup"><span data-stu-id="cf722-109">Access to the enterprise Windows Store by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="cf722-110">Vállalati-kompatibilis központi felhasználói beállítások az eszközön a munkahelyi vagy iskolai fiókját (nincs Microsoft-fiók szükséges)</span><span class="sxs-lookup"><span data-stu-id="cf722-110">Enterprise-compliant roaming of user settings across devices by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="cf722-111">Erős hitelesítés és kényelmes jelentkezzen be munkahelyi vagy iskolai fiókok esetén a vállalati Windows Hello és a Windows Hello</span><span class="sxs-lookup"><span data-stu-id="cf722-111">Strong authentication and convenient sign-in for work or school accounts with Windows Hello for Business and Windows Hello</span></span>
* <span data-ttu-id="cf722-112">Képes eszközök, amelyek megfelelnek a szervezeti eszköz csoportházirend-beállítások csak való hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="cf722-112">Ability to restrict access only to devices that comply with organizational device Group Policy settings</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf722-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cf722-113">Prerequisites</span></span>
<span data-ttu-id="cf722-114">Tartományhoz való csatlakozást továbbra is hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="cf722-114">Domain join continues to be useful.</span></span> <span data-ttu-id="cf722-115">Azonban az beszerzése az egyszeri bejelentkezés az Azure AD előnyei, központi a munkahelyi vagy iskolai fiókok és hozzáférési beállítások a Windows áruház munkahelyi vagy iskolai fiókkal rendelkező, szüksége lesz a következők:</span><span class="sxs-lookup"><span data-stu-id="cf722-115">However, to get the Azure AD benefits of SSO, roaming of settings with work or school accounts, and access to Windows Store with work or school accounts, you will need the following:</span></span>

* <span data-ttu-id="cf722-116">Azure AD-előfizetés</span><span class="sxs-lookup"><span data-stu-id="cf722-116">Azure AD subscription</span></span>
* <span data-ttu-id="cf722-117">Az Azure AD Connect kibővítheti a helyszíni címtár az Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf722-117">Azure AD Connect to extend the on-premises directory to Azure AD</span></span>
* <span data-ttu-id="cf722-118">Házirend beállítva, hogy a tartományhoz csatlakozó eszközök csatlakoztatása az Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf722-118">Policy that's set to connect domain-joined devices to Azure AD</span></span>
* <span data-ttu-id="cf722-119">Windows 10-buildekhez (build 10551 vagy újabb) eszközökhöz</span><span class="sxs-lookup"><span data-stu-id="cf722-119">Windows 10 build (build 10551 or newer) for devices</span></span>

<span data-ttu-id="cf722-120">Engedélyezéséhez a Windows Hello üzleti és a Windows Hello is szüksége lesz a következő:</span><span class="sxs-lookup"><span data-stu-id="cf722-120">To enable Windows Hello for Business and Windows Hello, you will also need the following:</span></span>

- <span data-ttu-id="cf722-121">**Nyilvános kulcsokra épülő infrastruktúra (PKI)** felhasználói tanúsítványok kibocsátásához.</span><span class="sxs-lookup"><span data-stu-id="cf722-121">**Public key infrastructure (PKI)** for user certificates issuance.</span></span>

- <span data-ttu-id="cf722-122">**A System Center Configuration Manager aktuális ágának** -1606 vagy jobb verzióját telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="cf722-122">**System Center Configuration Manager Current Branch** - You need to install version 1606 or better.</span></span>  
<span data-ttu-id="cf722-123">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="cf722-123">For more information, see:</span></span> 
    - [<span data-ttu-id="cf722-124">Dokumentáció a System Center Configuration Managerhez</span><span class="sxs-lookup"><span data-stu-id="cf722-124">Documentation for System Center Configuration Manager</span></span>](https://technet.microsoft.com/library/mt346023.aspx)
    - [<span data-ttu-id="cf722-125">A System Center Configuration Manager Csapatblog</span><span class="sxs-lookup"><span data-stu-id="cf722-125">System Center Configuration Manager Team Blog</span></span>](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [<span data-ttu-id="cf722-126">A Windows Hello-üzleti beállításait a System Center Configuration Managerben</span><span class="sxs-lookup"><span data-stu-id="cf722-126">Windows Hello for Business settings in System Center Configuration Manager</span></span>](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

<span data-ttu-id="cf722-127">A PKI üzembe helyezése: követelményeknek alternatívájaként a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="cf722-127">As an alternative to the PKI deployment requirement, you can do the following:</span></span>

* <span data-ttu-id="cf722-128">A Windows Server 2016 Active Directory tartományi szolgáltatások néhány tartományvezérlővel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cf722-128">Have a few domain controllers with Windows Server 2016 Active Directory Domain Services.</span></span>

<span data-ttu-id="cf722-129">Feltételes hozzáférés engedélyezése, a csoportházirend-beállítások nincsenek további központi telepítéseket a tartományhoz csatlakoztatott eszközökhöz való hozzáférést engedélyező is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="cf722-129">To enable conditional access, you can create Group Policy settings that allow access to domain-joined devices with no additional deployments.</span></span> <span data-ttu-id="cf722-130">Az eszköz megfelelőségét alapuló hozzáférés-vezérlés kezelése, a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="cf722-130">To manage access control based on compliance of the device, you will need the following:</span></span>

* <span data-ttu-id="cf722-131">A System Center Configuration Manager aktuális ágának (1606 vagy újabb) a Windows Hello-üzleti forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="cf722-131">System Center Configuration Manager Current Branch (1606 or later) for Windows Hello for Business scenarios</span></span>

## <a name="deployment-instructions"></a><span data-ttu-id="cf722-132">Üzembe helyezési utasítások</span><span class="sxs-lookup"><span data-stu-id="cf722-132">Deployment instructions</span></span>

<span data-ttu-id="cf722-133">Telepítéséhez kövesse a felsorolt lépéseket [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="cf722-133">To deploy, follow the steps listed in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

## <a name="next-step"></a><span data-ttu-id="cf722-134">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="cf722-134">Next step</span></span>
* [<span data-ttu-id="cf722-135">Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata</span><span class="sxs-lookup"><span data-stu-id="cf722-135">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="cf722-136">A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül</span><span class="sxs-lookup"><span data-stu-id="cf722-136">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="cf722-137">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="cf722-137">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="cf722-138">Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="cf722-138">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="cf722-139">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="cf722-139">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

