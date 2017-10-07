---
title: "ellenőrzési lépés aaaTwo és az AD FS - Azure MFA |} Microsoft Docs"
description: "Ez a hello Azure többtényezős hitelesítés lap, amely leírja, hogyan tooget lépések az Azure MFA és az AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: 7c1c925039d3cb753ba60e286168e5869faeae4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Bevezetés az Azure Multi-Factor Authentication és az Active Directory összevonási szolgáltatások használatába
<center>![Felhő](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Ha a szervezete az AD FS használatával vonta össze a helyszíni Active Directoryt az Azure Active Directoryval, az Azure Multi-Factor Authentication két módon használható .

* A felhőerőforrások védelme az Azure Multi-Factor Authentication vagy az Active Directory összevonási szolgáltatások használatával
* A felhő és a helyszíni erőforrások védelme az Azure Multi-Factor Authentication-kiszolgáló használatával

a következő táblázat hello hello ellenőrzési élmény biztonságossá tétele az Azure többtényezős hitelesítés és az AD FS a források között foglalja össze.

| Ellenőrzés – Böngészőalapú alkalmazások | Ellenőrzés – Nem böngészőalapú alkalmazások |
|:--- |:--- |:--- |
| Az Azure AD-erőforrások védelme az Azure Multi-Factor Authentication használatával |<li>hello első ellenőrzési lépés történik a helyszíni Active Directory összevonási szolgáltatások használatával.</li> <li>hello második lépése a felhőalapú hitelesítés használatával végzett phone-alapú módszer.</li> |
| Az Azure AD-erőforrások védelme az Active Directory összevonási szolgáltatásokkal |<li>hello első ellenőrzési lépés történik a helyszíni Active Directory összevonási szolgáltatások használatával.</li><li>hello második lépésben szerint hello jogcím érvényesítenie a helyszínen történik.</li> |

Összevont felhasználók alkalmazásjelszavaival kapcsolatos figyelmeztetések:

* Az alkalmazásjelszavak ellenőrzése felhőalapú hitelesítéssel történik, így mellőzik az összevonásokat. Az összevonás csak alkalmazásjelszó beállításakor van aktív használatban.
* Az alkalmazásjelszavak nem tartják be a helyszíni ügyfél hozzáférés-vezérlési beállításait.
* Az alkalmazásjelszavak használata esetén nem érhető el a helyszíni hitelesítésnaplózás.
* Fiók letiltása/törlése címtár-Szinkronizáló letiltását/törlését a hello felhőalapú identitás alkalmazásjelszók késleltetése toothree órát igénybe vehet.

## <a name="next-steps"></a>Következő lépések
Azure multi-factor Authentication vagy hello Azure multi-factor Authentication kiszolgáló az AD FS beállításával kapcsolatos információkért lásd: a következő cikkek hello:

* [A felhőerőforrások védelme az Azure Multi-Factor Authentication és az AD FS használatával](multi-factor-authentication-get-started-adfs-cloud.md)
* [A felhő és a helyszíni erőforrások védelme az Azure Multi-Factor Authentication-kiszolgáló és a Windows Server 2012 R2 AD FS használatával](multi-factor-authentication-get-started-adfs-w2k12.md)
* [A felhő és a helyszíni erőforrások védelme az Azure Multi-Factor Authentication-kiszolgáló és az AD FS 2.0-s verziójának használatával](multi-factor-authentication-get-started-adfs-adfs2.md)
