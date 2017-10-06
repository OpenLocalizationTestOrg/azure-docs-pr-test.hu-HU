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
# <a name="connect-domain-joined-devices-tooazure-ad-for-windows-10-experiences"></a>Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben
Tartományhoz való csatlakozást hello hagyományos módon szervezetek csatlakoztatott eszközök hello utolsó 15 éves és egyéb tevékenységeket. Lehetővé tette felhasználók toosign tootheir az eszközöknek a Windows Server Active Directory (az Active Directory) munkahelyi vagy iskolai fiókok és engedélyezett informatikai toofully kezelheti ezeket az eszközöket. A szervezetek általában támaszkodhat módszerek tooprovision eszközök toousers imaging, és általában a System Center Configuration Manager (SCCM) vagy a csoportházirend toomanage használja őket.


Tartományhoz való csatlakozást, a Windows 10-es hello Active Directory (Azure AD-) eszközök tooAzure csatlakoztatása után a következő előnyöket biztosítja:

* Egyszeri bejelentkezés (SSO) tooAzure AD erőforrások bárhonnan
* Toohello vállalati Windows áruház hozzáférhet a munkahelyi vagy iskolai fiókok (nincs Microsoft-fiók szükséges)
* Vállalati-kompatibilis központi felhasználói beállítások az eszközön a munkahelyi vagy iskolai fiókját (nincs Microsoft-fiók szükséges)
* Erős hitelesítés és kényelmes jelentkezzen be munkahelyi vagy iskolai fiókok esetén a vállalati Windows Hello és a Windows Hello
* Képes toorestrict hozzáférés csak olyan toodevices, amelyek megfelelnek a szervezeti eszköz csoportházirend-beállítások

## <a name="prerequisites"></a>Előfeltételek
Tartományhoz való csatlakozást továbbra is hasznos toobe. Azonban SSO tooget hello Azure AD előnyeit, a beállítások központi munkahelyi vagy iskolai fiókok, és tooWindows áruház eléréséhez a munkahelyi vagy iskolai fiókok, szüksége lesz a következő hello:

* Azure AD-előfizetés
* Az Azure AD Connect tooextend hello a helyszíni Active directory tooAzure
* Házirend tooconnect tartományhoz csatlakozó eszközök tooAzure AD beállítva
* Windows 10-buildekhez (build 10551 vagy újabb) eszközökhöz

a Windows Hello üzleti és a Windows Hello tooenable, konfigurálnia kell hello következő:

- **Nyilvános kulcsokra épülő infrastruktúra (PKI)** felhasználói tanúsítványok kibocsátásához.

- **A System Center Configuration Manager aktuális ágának** -1606 vagy jobb tooinstall verziója szükséges.  
További információkért lásd: 
    - [Dokumentáció a System Center Configuration Managerhez](https://technet.microsoft.com/library/mt346023.aspx)
    - [A System Center Configuration Manager Csapatblog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [A Windows Hello-üzleti beállításait a System Center Configuration Managerben](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

Egy alternatív toohello PKI üzembe helyezése: követelményeknek, mint teheti hello következő:

* A Windows Server 2016 Active Directory tartományi szolgáltatások néhány tartományvezérlővel rendelkezik.

feltételes hozzáférés tooenable, csoportházirend-beállítások, amelyek lehetővé teszik a hozzáférést toodomain csatlakoztatott eszközök nincsenek további központi telepítéseket is létrehozhat. toomanage hozzáférés-vezérlés hello eszköz megfelelőségét alapján, szüksége lesz a következő hello:

* A System Center Configuration Manager aktuális ágának (1606 vagy újabb) a Windows Hello-üzleti forgatókönyvek

## <a name="deployment-instructions"></a>Üzembe helyezési utasítások

toodeploy, szerepel a következő hello lépéseket kell [hogyan tooconfigure az automatikus regisztráció Windows tartományhoz csatlakoztatott eszközökre az Azure Active Directoryval](active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="next-step"></a>Következő lépés
* [Windows 10 Enterprise hello: módon toouse eszközök munkára](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-user-upgrade.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

