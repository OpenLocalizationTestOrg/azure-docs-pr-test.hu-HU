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
# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben
Tartományhoz való csatlakozást, a hagyományos módon szervezetek számára az elmúlt 15 éves és egyéb eszközök csatlakozott. Jelentkezzen be az eszközeiket a Windows Server Active Directory (az Active Directory) munkahelyi vagy iskolai fiókok felhasználók engedélyezve van és engedélyezett informatikai részleg teljes körű felügyeletéhez az ezeket az eszközöket. A szervezetek általában támaszkodhat a lemezkép-készítési módszerek eszközök beállításához a felhasználók számára, és általában használja a System Center Configuration Manager (SCCM) vagy a Csoportházirend kezelése.


Tartományhoz való csatlakozást, a Windows 10-es a következő előnyt biztosít az Azure Active Directory (Azure AD-) eszközök csatlakoztatása után:

* Egyszeri bejelentkezés (SSO) az Azure AD-erőforrások bárhonnan
* Hozzáférés a vállalati Windows áruház munkahelyi vagy iskolai fiókokkal (nincs Microsoft-fiók szükséges)
* Vállalati-kompatibilis központi felhasználói beállítások az eszközön a munkahelyi vagy iskolai fiókját (nincs Microsoft-fiók szükséges)
* Erős hitelesítés és kényelmes jelentkezzen be munkahelyi vagy iskolai fiókok esetén a vállalati Windows Hello és a Windows Hello
* Képes eszközök, amelyek megfelelnek a szervezeti eszköz csoportházirend-beállítások csak való hozzáférés korlátozása

## <a name="prerequisites"></a>Előfeltételek
Tartományhoz való csatlakozást továbbra is hasznos lehet. Azonban az beszerzése az egyszeri bejelentkezés az Azure AD előnyei, központi a munkahelyi vagy iskolai fiókok és hozzáférési beállítások a Windows áruház munkahelyi vagy iskolai fiókkal rendelkező, szüksége lesz a következők:

* Azure AD-előfizetés
* Az Azure AD Connect kibővítheti a helyszíni címtár az Azure AD
* Házirend beállítva, hogy a tartományhoz csatlakozó eszközök csatlakoztatása az Azure AD
* Windows 10-buildekhez (build 10551 vagy újabb) eszközökhöz

Engedélyezéséhez a Windows Hello üzleti és a Windows Hello is szüksége lesz a következő:

- **Nyilvános kulcsokra épülő infrastruktúra (PKI)** felhasználói tanúsítványok kibocsátásához.

- **A System Center Configuration Manager aktuális ágának** -1606 vagy jobb verzióját telepíteni kell.  
További információkért lásd: 
    - [Dokumentáció a System Center Configuration Managerhez](https://technet.microsoft.com/library/mt346023.aspx)
    - [A System Center Configuration Manager Csapatblog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [A Windows Hello-üzleti beállításait a System Center Configuration Managerben](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

A PKI üzembe helyezése: követelményeknek alternatívájaként a következőket teheti:

* A Windows Server 2016 Active Directory tartományi szolgáltatások néhány tartományvezérlővel rendelkezik.

Feltételes hozzáférés engedélyezése, a csoportházirend-beállítások nincsenek további központi telepítéseket a tartományhoz csatlakoztatott eszközökhöz való hozzáférést engedélyező is létrehozhat. Az eszköz megfelelőségét alapuló hozzáférés-vezérlés kezelése, a következőkre lesz szüksége:

* A System Center Configuration Manager aktuális ágának (1606 vagy újabb) a Windows Hello-üzleti forgatókönyvek

## <a name="deployment-instructions"></a>Üzembe helyezési utasítások

Telepítéséhez kövesse a felsorolt lépéseket [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="next-step"></a>Következő lépés
* [Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata](active-directory-azureadjoin-windows10-devices-overview.md)
* [A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül](active-directory-azureadjoin-user-upgrade.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

