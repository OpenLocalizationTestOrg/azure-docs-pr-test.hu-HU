---
title: "Azure AD Connect szinkronizálása: véletlen törlések megakadályozása |} Microsoft Docs"
description: "Ez a témakör ismerteti a hello Azure AD Connect szolgáltatása (véletlen törlések megakadályozása) véletlen törlések megakadályozása."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 159597f8354806fcaea1430e0ff84956338592a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Az Azure AD Connect szinkronizálása: véletlen törlések megakadályozása
Ez a témakör ismerteti a hello Azure AD Connect szolgáltatása (véletlen törlések megakadályozása) véletlen törlések megakadályozása.

Ha az Azure AD Connect telepítése megakadályozása véletlen törlések alapértelmezés szerint engedélyezve van, és konfigurált toonot engedélyezése az export-legfeljebb 500 törlést. A szolgáltatás nem tervezett tooprotect megtilthatja véletlen konfiguráció módosításai és tooyour helyszíni címtár, amely hatással lenne a sok felhasználóval és egyéb objektumok változik.

## <a name="what-is-prevent-accidental-deletes"></a>Mi az véletlen törlések megakadályozása
Gyakori helyzetek, amikor megjelenik a sok törlések tartalmazza:

* Módosítja túl[szűrés](active-directory-aadconnectsync-configure-filtering.md) Amennyiben egy teljes [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) vagy [tartomány](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) nincs bejelölve.
* Egy szervezeti egység összes objektum törlődnek.
* Szervezeti egység összes objektumaira végzett szinkronizálás toobe minősülnek, új neve.

hello alapértelmezett érték 500 objektumok módosítható a PowerShell használatával `Enable-ADSyncExportDeletionThreshold`. Az érték toofit hello mérete a szervezet úgy kell konfigurálni. Hello szinkronizálásütemezési 30 percenként fut., mivel hello értéke 30 percen belül látható törlések hello száma.

Ha túl sok előkészített törlések toobe tooAzure AD, exportálni, majd hello exportálási leáll, és az e-maileket kapni ehhez hasonló:

![E-mailek véletlen törlések megakadályozása](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hello (technikai kapcsolattartó). (Idő): hello identitás szinkronizálási szolgáltatás észlelte, hogy törlések hello száma meghaladta a hello konfigurált törlési küszöbét az (szervezet neve). A (szám) teljes objektumok küldtek az identitások szinkronizálására, futtassa a törlésre. Ez a webhely elérte vagy túllépte a beállított hello törlés küszöbértéket (szám) objektumok. El kell tooprovide megerősítő, amelyeket a törlés feldolgozása a rendszer a folytatás előtt. Tekintse meg az e-mailben felsorolt hello hibával kapcsolatos további tudnivalókért véletlen törlések megakadályozása hello.*
>
> 

Azt is láthatja, hello állapot `stopped-deletion-threshold-exceeded` mikor célszerű figyelni a hello **Synchronization Service Managert** hello exportált profil felhasználói felülete.
![Szinkronizálás a Service Manager felhasználói felületén véletlen törlések megakadályozása](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Ha ez nem várt, vizsgálja meg, és tegye intézkedéseket. objektumok kapcsolatos toobe törölni, amelyek toosee hello a következő:

1. Start **szinkronizálási szolgáltatás** a Start menü hello.
2. Nyissa meg túl**összekötők**.
3. Válassza ki a Connector típusú hello **Azure Active Directory**.
4. A **műveletek** toohello jobbra, jelölje be **Összekötőtér keresési**.
5. Az előugró alatt hello **hatókör**, jelölje be **leválasztása óta** és a hello múltbeli időpont kiválasztása. Kattintson a **keresési**. Ezen a lapon az összes objektum törölt toobe vonatkozó nézetét jeleníti meg. Minden cikk kattintva kaphat a hello objektum további információt. Is **oszlop beállítás** tooadd további attribútumok toobe hello rácsban látható.

![Összekötőtér keresése](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Minden hello törlések igény szerint majd hello a következő:

1. tooretrieve hello aktuális törlési küszöbét, hello PowerShell-parancsmag futtatásához `Get-ADSyncExportDeletionThreshold`. Adja meg az Azure AD globális rendszergazdai fiókot és jelszót. hello alapértelmezett érték 500.
2. tootemporarily tiltsa le a védelmet, és segítségével azokat törli keresztül halad, hello PowerShell-parancsmaggal ellenőrizheti: `Disable-ADSyncExportDeletionThreshold`. Adja meg az Azure AD globális rendszergazdai fiókot és jelszót.
   ![Hitelesítő adatok](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
3. Az Azure Active Directory-összekötő továbbra is a kiválasztott hello, válassza ki a hello műveletet **futtatása** válassza **exportálása**.
4. toore-védelem engedélyezése a hello, hello PowerShell-parancsmaggal ellenőrizheti: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`. Cserélje le 500 hello érték első fellépése hello aktuális törlési küszöbét beolvasása közben. Adja meg az Azure AD globális rendszergazdai fiókot és jelszót.

## <a name="next-steps"></a>Következő lépések
**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
