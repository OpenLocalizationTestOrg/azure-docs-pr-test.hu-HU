---
title: "Az Azure RemoteApp - hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt? | Microsoft Docs"
description: "Ismerje meg, hogyan hálózati sávszélesség az Azure Remoteappban hatással lehet a felhasználó minőséget."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 74ebc1fb-5187-4056-b08c-0e03b5ecaca6
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 74116902e973fba440b3c662fdf76202d052b4c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Az Azure RemoteApp - hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Ha éppen megtekintett a [teljes hálózati sávszélességet](remoteapp-bandwidth.md) szükséges Azure RemoteApp, vegye figyelembe az alábbi tényezők – ezek hatással van a teljes felhasználói élmény dinamikus rendszer összes részét képezik. 

* **Rendelkezésre álló hálózati sávszélességet és az aktuális hálózati körülmények** -paraméterek (veszteségeit, késéseit, jitter) ugyanazon a hálózaton, egy adott időben kedvezőtlen hatással lehet az alkalmazás adatfolyam-kezelés, azaz egy süllyesztett általános felhasználói felület. A hálózaton elérhető sávszélességet feladata torlódás, a véletlen adatvesztést, a késés, mivel ezek a paraméterek hatással az torlódás-vezérlési mechanizmus, amely vezérlők az átviteli sebesség, így elkerülhetik az ütközéseket.  Például veszteséges hálózati vagy a nagy késleltetésű hálózati teszi a felhasználói felület rossz még akkor is, a hálózati sávszélességgel 1000 MB. A veszteséget és késéseket függ, amelyek ugyanazon a hálózaton, és ezek a felhasználók tevékenységeit (például videók, letöltése vagy nagy fájlok nyomtatása feltöltése figyeli) a felhasználók száma.
* **Használati eset** -a felhasználói élmény, attól függ, a felhasználók tevékenységeit egyéni felhasználók számára és a csoport ugyanazon a hálózaton. Például egy diák olvasása van szükség a csak egy keretet frissíthető; Ha a felhasználó skims, és a szöveges dokumentum tartalmának keresztül görget, szükségük keretek frissítenie kell a másodpercenkénti száma. A kommunikáció oda-vissza ebben a forgatókönyvben a kiszolgáló végül fog használni, nagyobb hálózati sávszélességet. Is érdemes lehet egy rendkívüli példa: több felhasználó is figyeli a nagy felbontású videók (például a 4 KB-os megoldás), HD konferenciahívások okozó, 3D videó játékok vagy CAD-rendszereken működik. Mindegyik használhatatlan lehet akár egy valóban nagy sávszélességű hálózati gyakorlatilag.
* **Képernyő-feloldási és számát** -további hálózati sávszélességre szükség, hogy teljes frissítés nagyobb képernyők, mint a kisebb képernyőkön. Az alapul szolgáló technológiát does kódolását, és csak a képernyőn megjelenő frissítve lett a régiók átvitele egy meglehetősen jó feladatot, de egyszer a egy while, a teljes képernyőt frissíteni kell. Amikor a felhasználó rendelkezik-e egy magasabb feloldási képernyő (például a 4 KB-os megoldás), a frissítéshez alacsonyabb felbontású (például 1024x768px) képernyő-nál nagyobb hálózati sávszélességet. Ugyanez a logika vonatkozik, az átirányítás egynél több képernyő használatakor. Sávszélesség kell képernyők számának növeléséhez.
* **Vágólap és eszköz-átirányítás** - Ez egy nagyon nyilvánvaló probléma, de sok esetben ha a felhasználó tárolja az adatokat a vágólapra a nagy adattömb tart, amíg az adatokat a kiszolgáló a távoli asztali ügyfél átvitele bit. Az alsóbb rétegbeli élmény is negatív hatással lehet a vágólapra tartalom küldése előtt élménye. Ugyanez vonatkozik a eszközátirányítás -, ha egy olvasó vagy webkamera hozza létre a nagy mennyiségű adat, amely kell küldeni a kiszolgálónak felsőbb rétegbeli, vagy egy nyomtató kell fogadni a nagy dokumentum, vagy helyi tárhelyet kell lennie a nagy fájlok másolása a felhőben futó alkalmazások számára elérhető, felhasználók észrevette, hogy az eldobott keretek, vagy átmenetileg "zárolva" videó, mert a eszközátirányítás szükséges adatokat a hálózati sávszélesség egyre kell. 

Amikor a hálózati sávszélesség szükségleteknek értékeli, győződjön meg arról, figyelembe kell venni az összes ezek a tényezők működik-e a rendszer.

Most, lépjen vissza a [fő hálózati sávszélesség cikk](remoteapp-bandwidth.md), vagy helyezze át tesztelés a [hálózati sávszélességet](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Részletek
* [Azure RemoteApp a sávszélesség-használat becslése](remoteapp-bandwidth.md)
* [Az Azure RemoteApp – olyan gyakori forgatókönyveket tartalmaz, a hálózati sávszélesség-használat tesztelése](remoteapp-bandwidthtests.md)
* [Az Azure RemoteApp hálózati sávszélesség - általános irányelveket (Ha nem teszteli a saját)](remoteapp-bandwidthguidelines.md)

