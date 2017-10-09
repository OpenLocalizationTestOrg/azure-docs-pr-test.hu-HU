---
title: "aaaAzure felhő rendszerhéj (előzetes verzió) – áttekintés |} Microsoft Docs"
description: "Hello Azure Cloud rendszerhéj áttekintése."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: 45c6c85b167a90947a333f44a9186e2c01b4fa7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a>Azure-felhőbe rendszerhéj (előzetes verzió) áttekintése
Azure Cloud rendszerhéjjal egy interaktív, a böngésző által elérhető rendszerhéj Azure-erőforrások kezeléséhez.

![](media/overview-pic.png)

## <a name="features"></a>Szolgáltatások
### <a name="browser-based-shell-experience"></a>Böngészőalapú felület élmény
A felhő rendszerhéj lehetővé teszi, hogy hozzáférési tooa böngészőalapú parancssori felületén beépített szem előtt az Azure felügyeleti feladatokkal. Éljen az olyan felhő rendszerhéj toowork untethered oly módon, csak hello felhő biztosíthat a helyi gépről.

### <a name="pre-configured-azure-workstation"></a>Előre konfigurált Azure munkaállomás
Felhő rendszerhéj előre előre telepített népszerű parancssori eszközökkel, és a nyelv támogatja, így gyorsabban működnek.

[Hello teljes tooling listájának megtekintése Azure Cloud rendszerhéj itt.](features.md#tools)

### <a name="automatic-authentication"></a>Automatikus hitelesítéshez
Felhő rendszerhéj biztonságosan hitelesíti automatikusan minden munkameneten keresztül hello Azure CLI 2.0 azonnali hozzáférési tooyour erőforrások.

### <a name="connect-your-azure-file-storage"></a>Csatlakozás az Azure File storage
Felhő rendszerhéj gépek ideiglenes, és emiatt igényel az egy fájl Azure fájlmegosztás toobe csatlakoztatva, mint a `clouddrive` toopersist $Home címtárat.
Első indítsa el a felhő rendszerhéj kérni fogja a toocreate egy erőforráscsoport, a tárfiók, és fájlmegosztás az Ön nevében. Ez egy egyszeri lépés, és lesz automatikusan hozzárendelve minden munkamenetben. 

#### <a name="create-new-storage"></a>Új tároló létrehozása
![](media/basic-storage.png)

A helyileg redundáns tárolás (LRS) fiók az Azure fájlmegosztások alapértelmezett 5 GB-os lemezképet tartalmazó az Ön nevében hozhatja létre. hello fájlmegosztás csatlakoztatja `clouddrive` fájl megosztása hello lemezképet való együttműködéshez használt toosync alatt, és a $Home címtárban maradnak. Rendszeres tárolási költségek vonatkoznak.

Három erőforrások hozza létre az Ön nevében:
1. Az erőforráscsoport neve:`cloud-shell-storage-<region>`
2. A Tárfiók neve:`cs<uniqueGuid>`
3. A fájlmegosztás neve:`cs-<user>-<domain>-com-uniqueGuid`

> [!Note]
> Például az SSH-kulcsok a $Home könyvtárban található összes fájl megmaradnak, az a felhasználó lemezképet, a csatlakoztatott fájl megosztáson található. Alkalmazza az ajánlott eljárásokat, ha a fájlok mentése a $Home címtár és a csatlakoztatott fájlmegosztás.

#### <a name="use-existing-resources"></a>Létező erőforrásokból
![](media/advanced-storage.png)

Speciális beállítás így Ön tooassociate meglévő erőforrások tooCloud rendszerhéj is biztosítja. Hello tárolási telepítő kérdéshez megjelenésekor kattintson a "Speciális megjelenítése beállítások" tooselect további beállításokat. A hozzárendelt felhő rendszerhéj pedig a helyi/globálisan-redundancia tárfiókok legördülő listák Megnyílásának szűrve.

[További információk a felhő rendszerhéj tároló fájlmegosztások frissítése, és a feltöltése/letöltése fájlokat.] (megőrzése-rendszerhéj-storage.md)

## <a name="concepts"></a>Alapelvek
* Felhő rendszerhéj egy munkamenetenként, egyes felhasználók számára a megadott ideiglenes gépen fut
* Interaktív tevékenység nélkül 20 perc elteltével időtúllépést felhő rendszerhéj
* Felhő rendszerhéj csak és csatolt fájlmegosztás érhető el
* Felhő rendszerhéj hozzá van rendelve egy gép felhasználónként
* Linux felhasználói engedélyek beállítása

[További információ az összes felhő Shell szolgáltatás.](features.md)

## <a name="examples"></a>Példák
* Hozzon létre vagy parancsfájlok tooautomate Azure felügyeleti szerkesztése
* Egyidejűleg az Azure-portál és az Azure CLI 2.0-erőforrások kezelése
* Az Azure CLI 2.0 test-Drive

[Próbálja ki hello felhő rendszerhéj gyors üzembe helyezés, a példákat.](quickstart.md)

## <a name="pricing"></a>Díjszabás
Felhő rendszerhéj futtató hello gépen szabad, olyan előfeltételt egy csatlakoztatott Azure fájl a könyvtárban van toopersist a $Home. Rendszeres tárolási költségek vonatkoznak.

## <a name="supported-browsers"></a>Támogatott böngészők
Felhő rendszerhéj Chrome, biztonsági és Safari ajánlott. Felhő rendszerhéj Chrome, Firefox, Safari, Internet Explorer vagy Edge esetén támogatott, amíg a felhő rendszerhéj tulajdonos toospecific böngésző beállításait.

## <a name="troubleshooting"></a>Hibaelhárítás
1. Egy Azure Active Directory-előfizetés használata esetén nem lehet létrehozni a tárolási esedékes tooError: 400 DisallowedOperation. tooresolve, használja a képes a tárolási erőforrások létrehozása Azure-előfizetéssel. AD-előfizetés nem képes toocreate Azure-erőforrások vannak.

Adott ismert korlátozásai, látogasson el [felhő rendszerhéj korlátai](limitations.md).
