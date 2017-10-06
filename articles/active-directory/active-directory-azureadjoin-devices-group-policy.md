---
title: "aaaConnect tartományhoz csatlakozó eszközök tooAzure AD a Windows 10 észlel |} Microsoft Docs"
description: "Ismerteti, hogyan rendszergazdák konfigurálhatják a csoportházirend tooenable eszközök toobe toohello tartományhoz csatlakoztatott vállalati hálózatra."
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
ms.openlocfilehash: 9766aa702352dea2ecad3a9a0bdf8d3286ee6d91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-domain-joined-devices-tooazure-ad-for-windows-10-experiences"></a><span data-ttu-id="50563-103">Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="50563-103">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>
<span data-ttu-id="50563-104">Tartományhoz való csatlakozást hello hagyományos módon szervezetek csatlakoztatott eszközök hello utolsó 15 éves és egyéb tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="50563-104">Domain join is hello traditional way organizations have connected devices for work for hello last 15 years and more.</span></span> <span data-ttu-id="50563-105">Lehetővé tette felhasználók toosign tootheir az eszközöknek a Windows Server Active Directory (az Active Directory) munkahelyi vagy iskolai fiókok és engedélyezett informatikai toofully kezelheti ezeket az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="50563-105">It has enabled users toosign in tootheir devices by using their Windows Server Active Directory (Active Directory) work or school accounts and allowed IT toofully manage these devices.</span></span> <span data-ttu-id="50563-106">A szervezetek általában támaszkodhat módszerek tooprovision eszközök toousers imaging, és általában a System Center Configuration Manager (SCCM) vagy a csoportházirend toomanage használja őket.</span><span class="sxs-lookup"><span data-stu-id="50563-106">Organizations typically rely on imaging methods tooprovision devices toousers and generally use System Center Configuration Manager (SCCM) or Group Policy toomanage them.</span></span>


<span data-ttu-id="50563-107">Tartományhoz való csatlakozást, a Windows 10-es hello Active Directory (Azure AD-) eszközök tooAzure csatlakoztatása után a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="50563-107">Domain join in Windows 10 provides you with hello following benefits after you connect devices tooAzure Active Directory (Azure AD):</span></span>

* <span data-ttu-id="50563-108">Egyszeri bejelentkezés (SSO) tooAzure AD erőforrások bárhonnan</span><span class="sxs-lookup"><span data-stu-id="50563-108">Single sign-on (SSO) tooAzure AD resources from anywhere</span></span>
* <span data-ttu-id="50563-109">Toohello vállalati Windows áruház hozzáférhet a munkahelyi vagy iskolai fiókok (nincs Microsoft-fiók szükséges)</span><span class="sxs-lookup"><span data-stu-id="50563-109">Access toohello enterprise Windows Store by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="50563-110">Vállalati-kompatibilis központi felhasználói beállítások az eszközön a munkahelyi vagy iskolai fiókját (nincs Microsoft-fiók szükséges)</span><span class="sxs-lookup"><span data-stu-id="50563-110">Enterprise-compliant roaming of user settings across devices by using work or school accounts (no Microsoft account required)</span></span>
* <span data-ttu-id="50563-111">Erős hitelesítés és kényelmes jelentkezzen be munkahelyi vagy iskolai fiókok esetén a vállalati Windows Hello és a Windows Hello</span><span class="sxs-lookup"><span data-stu-id="50563-111">Strong authentication and convenient sign-in for work or school accounts with Windows Hello for Business and Windows Hello</span></span>
* <span data-ttu-id="50563-112">Képes toorestrict hozzáférés csak olyan toodevices, amelyek megfelelnek a szervezeti eszköz csoportházirend-beállítások</span><span class="sxs-lookup"><span data-stu-id="50563-112">Ability toorestrict access only toodevices that comply with organizational device Group Policy settings</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50563-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="50563-113">Prerequisites</span></span>
<span data-ttu-id="50563-114">Tartományhoz való csatlakozást továbbra is hasznos toobe.</span><span class="sxs-lookup"><span data-stu-id="50563-114">Domain join continues toobe useful.</span></span> <span data-ttu-id="50563-115">Azonban SSO tooget hello Azure AD előnyeit, a beállítások központi munkahelyi vagy iskolai fiókok, és tooWindows áruház eléréséhez a munkahelyi vagy iskolai fiókok, szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="50563-115">However, tooget hello Azure AD benefits of SSO, roaming of settings with work or school accounts, and access tooWindows Store with work or school accounts, you will need hello following:</span></span>

* <span data-ttu-id="50563-116">Azure AD-előfizetés</span><span class="sxs-lookup"><span data-stu-id="50563-116">Azure AD subscription</span></span>
* <span data-ttu-id="50563-117">Az Azure AD Connect tooextend hello a helyszíni Active directory tooAzure</span><span class="sxs-lookup"><span data-stu-id="50563-117">Azure AD Connect tooextend hello on-premises directory tooAzure AD</span></span>
* <span data-ttu-id="50563-118">Házirend tooconnect tartományhoz csatlakozó eszközök tooAzure AD beállítva</span><span class="sxs-lookup"><span data-stu-id="50563-118">Policy that's set tooconnect domain-joined devices tooAzure AD</span></span>
* <span data-ttu-id="50563-119">Windows 10-buildekhez (build 10551 vagy újabb) eszközökhöz</span><span class="sxs-lookup"><span data-stu-id="50563-119">Windows 10 build (build 10551 or newer) for devices</span></span>

<span data-ttu-id="50563-120">a Windows Hello üzleti és a Windows Hello tooenable, konfigurálnia kell hello következő:</span><span class="sxs-lookup"><span data-stu-id="50563-120">tooenable Windows Hello for Business and Windows Hello, you will also need hello following:</span></span>

- <span data-ttu-id="50563-121">**Nyilvános kulcsokra épülő infrastruktúra (PKI)** felhasználói tanúsítványok kibocsátásához.</span><span class="sxs-lookup"><span data-stu-id="50563-121">**Public key infrastructure (PKI)** for user certificates issuance.</span></span>

- <span data-ttu-id="50563-122">**A System Center Configuration Manager aktuális ágának** -1606 vagy jobb tooinstall verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="50563-122">**System Center Configuration Manager Current Branch** - You need tooinstall version 1606 or better.</span></span>  
<span data-ttu-id="50563-123">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="50563-123">For more information, see:</span></span> 
    - [<span data-ttu-id="50563-124">Dokumentáció a System Center Configuration Managerhez</span><span class="sxs-lookup"><span data-stu-id="50563-124">Documentation for System Center Configuration Manager</span></span>](https://technet.microsoft.com/library/mt346023.aspx)
    - [<span data-ttu-id="50563-125">A System Center Configuration Manager Csapatblog</span><span class="sxs-lookup"><span data-stu-id="50563-125">System Center Configuration Manager Team Blog</span></span>](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [<span data-ttu-id="50563-126">A Windows Hello-üzleti beállításait a System Center Configuration Managerben</span><span class="sxs-lookup"><span data-stu-id="50563-126">Windows Hello for Business settings in System Center Configuration Manager</span></span>](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

<span data-ttu-id="50563-127">Egy alternatív toohello PKI üzembe helyezése: követelményeknek, mint teheti hello következő:</span><span class="sxs-lookup"><span data-stu-id="50563-127">As an alternative toohello PKI deployment requirement, you can do hello following:</span></span>

* <span data-ttu-id="50563-128">A Windows Server 2016 Active Directory tartományi szolgáltatások néhány tartományvezérlővel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="50563-128">Have a few domain controllers with Windows Server 2016 Active Directory Domain Services.</span></span>

<span data-ttu-id="50563-129">feltételes hozzáférés tooenable, csoportházirend-beállítások, amelyek lehetővé teszik a hozzáférést toodomain csatlakoztatott eszközök nincsenek további központi telepítéseket is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="50563-129">tooenable conditional access, you can create Group Policy settings that allow access toodomain-joined devices with no additional deployments.</span></span> <span data-ttu-id="50563-130">toomanage hozzáférés-vezérlés hello eszköz megfelelőségét alapján, szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="50563-130">toomanage access control based on compliance of hello device, you will need hello following:</span></span>

* <span data-ttu-id="50563-131">A System Center Configuration Manager aktuális ágának (1606 vagy újabb) a Windows Hello-üzleti forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="50563-131">System Center Configuration Manager Current Branch (1606 or later) for Windows Hello for Business scenarios</span></span>

## <a name="deployment-instructions"></a><span data-ttu-id="50563-132">Üzembe helyezési utasítások</span><span class="sxs-lookup"><span data-stu-id="50563-132">Deployment instructions</span></span>

<span data-ttu-id="50563-133">toodeploy, szerepel a következő hello lépéseket kell [hogyan tooconfigure az automatikus regisztráció Windows tartományhoz csatlakoztatott eszközökre az Azure Active Directoryval](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="50563-133">toodeploy, follow hello steps listed in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

## <a name="next-step"></a><span data-ttu-id="50563-134">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="50563-134">Next step</span></span>
* [<span data-ttu-id="50563-135">Windows 10 Enterprise hello: módon toouse eszközök munkára</span><span class="sxs-lookup"><span data-stu-id="50563-135">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="50563-136">Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="50563-136">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="50563-137">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="50563-137">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="50563-138">Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="50563-138">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="50563-139">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="50563-139">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

