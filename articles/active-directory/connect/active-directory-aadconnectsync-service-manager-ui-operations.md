---
title: "Az Azure AD Connect Synchronization Service Managert műveletek |} Microsoft Docs"
description: "Ismerje meg, az Azure AD Connect hello műveletek lapján hello Synchronization Service Managert."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: decbc53613d456a71cd116c40c5e1fd761efd4af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-sync-service-manager-operations-tab"></a>Hello szinkronizálása a Service Manager-műveletek lap használatával

![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

hello műveletek lapon hello legutóbbi műveletek hello eredményei láthatók. Ezen a lapon kulcs toounderstand és elhárítását.

## <a name="understand-hello-information-visible-in-hello-operations-tab"></a>Hello információk hello műveletek lapján látható ismertetése
hello felső félig minden futtatásakor időrendben jeleníti meg. Alapértelmezés szerint hello műveleti napló tartja hello információ elmúlt hét napban, de ez a beállítás használatával módosítható hello [Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md). Bármely futtatása, amelyek nem szerepelnek egy sikeres állapotnak toolook használni szeretne. Hello rendezés hello fejlécek kattintva módosíthatja.

Hello **állapot** oszlop hello legfontosabb adatokat, és látható hello futtató legsúlyosabb károkat okozó problémát. Hello leggyakoribb állapotokról tooinvestigate prioritási sorrendben gyors összegzése (ahol * több lehetséges hiba karakterláncok jelzi).

| status | Megjegyzés |
| --- | --- |
| leállított-* |hello futtatása nem sikerült befejezni. Például ha hello távoli rendszer le, és nem érhető el. |
| leállt hiba-korlát |Több mint 5000 hibák vannak. Futtatás hello automatikusan toohello hibák nagy száma miatt le lett állítva. |
| Befejezett -\*-hibák |hello futtatása befejeződött, de hibák (kevesebb mint 5000) meg kell vizsgálni. |
| Befejezett -\*-figyelmeztetések |hello futtatása befejeződött, de bizonyos adatok nem várt hello állapotban van. Ha hibákat, majd ezt az üzenetet az általában csak az egyik oka. Mindaddig, amíg meg kell oldani hibák, figyelmeztetések nem ki kell vizsgálni. |
| sikeres |Nincs probléma. |

Amikor kiválaszt egy sorra, akkor hello alsó tooshow hello részleteit futtató frissíti. toohello sokkal bal hello alsó részén, előfordulhat, hogy egy lista bármelyiket **lépés #**. Ez a lista csak akkor jelenik meg, ha több tartomány az erdőben, ahol minden egyes tartományhoz egy lépés képviseli. hello címszó alatt található hello tartománynév **partíció**. A **szinkronizálási statisztika**, található további információ a feldolgozott változtatások hello száma. Kattinthat hello hivatkozások tooget hello megváltozott objektumok listája. Ha hibákkal objektummal rendelkezik, azokat a hibákat az Eseménynaplón **szinkronizálási hibák**.

További információkért lásd: [olyan objektum, amely nem szinkronizál hibaelhárítása](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
