---
title: "aaaUpgrade tooAzure AD alkalmazásproxy |} Microsoft Docs"
description: "Válassza ki, melyik proxy megoldás esetén ajánlott, ha frissít, a Microsoft Forefront vagy egységes Access-átjárón."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7dc2633140b384e25792470dadbb7f3fa7992a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="compare-remote-access-solutions"></a>A távelérési megoldások összehasonlítása

Az Azure Active Directory Alkalmazásproxyjával egyike a két távelérési megoldás, amely a Microsoft biztosít. más hello a webalkalmazás-Proxy hello helyszíni verzióira. Ezek a megoldások két cserélje le a korábbi termékek, amelyet a Microsoft kínál: Microsoft Forefront Threat Management Gateway (TMG) és az egységes Access-átjáró (UAG). Ez a cikk toounderstand használja hogyan ezek a megoldások összehasonlítása más tooeach. Azok az Ön továbbra is hello és elavult TMG vagy UAG megoldások, ez a cikk toohelp csomag az áttelepítési tooone az alkalmazásproxy hello használata. 


## <a name="feature-comparison"></a>Funkciók összehasonlítása

Használja a tábla toounderstand hogyan Threat Management Gateway (TMG), a Unified Access-átjáró (UAG), a webalkalmazás-proxykiszolgálóként (WAP) és az Azure AD Application Proxy (AP) összehasonlítása más tooeach.

| Szolgáltatás | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Tanúsítványhitelesítés | Igen | Igen | - | - |
| Szelektív közzététel böngészőben megjelenő alkalmazásokba | Igen | Igen | Igen | Igen |
| Előhitelesítés és egyszeri bejelentkezés | Igen | Igen | Igen | Igen | 
| 2/3 tűzfal réteg | Igen | Igen | - | - |
| Előre proxyképességekkel | Igen | - | - | - |
| VPN-képességek | Igen | Igen | - | - |
| Gazdag protokoll támogatása | - | Igen | Igen, ha HTTP Protokollon keresztül fut | Igen, ha HTTP Protokollon keresztül vagy a távoli asztali átjárón keresztül futtatja |
| Az AD FS-proxy kiszolgálóként működik | - | Igen | Igen | - |
| Egy alkalmazás-hozzáférés-portál | - | Igen | - | Igen |
| Válasz törzsében hivatkozás fordítás | Igen | Igen | - | Igen | 
| A fejlécek hitelesítés | - | Igen | - | Igen, de PingAccess | 
| A felhőméretű biztonsági | - | - | - | Igen | 
| Feltételes hozzáférés | - | Igen | - | Igen |
| Nincsenek összetevők hello demilitarizált zónában (DMZ) | - | - | - | Igen |
| Bejövő kapcsolatok nem | - | - | - | Igen |

A legtöbb esetben ajánlott hello modern megoldás az Azure AD-alkalmazást. Webalkalmazás-Proxy proxykiszolgáló szükséges az AD FS Szolgáltatásokhoz csak az előnyben részesített forgatókönyveket, és nem használhat egyéni tartományok az Azure Active Directoryban. 

Az Azure AD-alkalmazásproxy kínál egyedi előnyökkel jár a mikor toosimilar termékek, köztük képest:

- Az Azure AD tooon helyszíni erőforrások kiterjesztése
   - A felhőméretű biztonság és védelem
   - Például a feltételes hozzáférés és a multi-factor Authentication szolgáltatások is könnyen tooenable
- Nincs componenet hello demilitarizált zónában
- Bejövő kapcsolatok nem szükséges
- Egy hozzáférési panel, hogy a felhasználók folytathatja toofor minden az alkalmazások, beleértve az Office 365, az Azure AD integrált Szolgáltatottszoftver-alkalmazásoknál, és a helyszíni webalkalmazások. 


## <a name="next-steps"></a>Következő lépések

- [Az Azure AD-alkalmazást tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások használata](active-directory-application-proxy-get-started.md)
- [Forefront TMG és UAG tooApplication proxyval való áttérés](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).
