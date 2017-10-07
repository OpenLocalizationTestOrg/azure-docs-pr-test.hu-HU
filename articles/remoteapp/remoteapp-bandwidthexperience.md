---
title: "RemoteApp - aaaAzure hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt? | Microsoft Docs"
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
ms.openlocfilehash: 62b0caadf63359eceb63d27fae6ad289b682ff63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Az Azure RemoteApp - hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Ha éppen megtekintett hello [teljes hálózati sávszélességet](remoteapp-bandwidth.md) szükséges Azure RemoteApp, tartsa szem előtt tartva hello tényezőket a következő – ezek, hogy hatások hello általános felhasználói élmény dinamikus rendszer összes részét képezik. 

* **Rendelkezésre álló hálózati sávszélességet és az aktuális hálózati körülmények** -paraméterek (veszteségeit, késéseit, jitter) az adott időpontban azonos hálózati hello kedvezőtlen hatással lehet az adatfolyam-kezelés, azaz egy süllyesztett általános felhasználói felület hello alkalmazás. hello sávszélesség álljon rendelkezésre a hálózaton lévő feladata torlódás, a véletlen adatvesztést, a késés, mivel ezek a paraméterek hatással hello torlódás vezérlési mechanizmus, amely kapcsolja vezérlők hello átviteli sebesség tooavoid ütközések.  Például veszteséges hálózati vagy a nagy késleltetésű hálózati bizonyosodhatunk hello felhasználói felülettel rossz még akkor is, a hálózati sávszélességgel 1000 MB. hello veszteséget és késéseket függően változhat a felhasználók, akik a hello hello száma ugyanazon a hálózaton, és ezek a felhasználók tevékenységeit (például videók, letöltése vagy nagy fájlok nyomtatása feltöltése figyeli).
* **Használati eset** -hello élmény, attól függ, a felhasználók milyen hello tűnik, hogy minden egyéni felhasználók számára és egy csoport hello azonos hálózati. Például egy diák olvasása igényel csak egyetlen keret toobe frissítése; Ha egy szöveges dokumentum hello tartalomban görget hello felhasználói skims, másodpercenként frissített keretek toobe nagyobb számú szükségük. hello kommunikációs vissza, és oda-toohello server ebben a forgatókönyvben végül igényel, nagyobb hálózati sávszélességet. Is érdemes lehet egy rendkívüli példa: több felhasználó is figyeli a nagy felbontású videók (például a 4 KB-os megoldás), HD konferenciahívások okozó, 3D videó játékok vagy CAD-rendszereken működik. Mindegyik használhatatlan lehet akár egy valóban nagy sávszélességű hálózati gyakorlatilag.
* **Képernyő-feloldási és hello számát** -nagyobb hálózati sávszélesség szükséges toofull frissítés képernyők nagyobb, mint kisebb képernyőkön. hello alapul szolgáló technológiát does egy kódolást és helyre küldi hello képernyők frissített csak hello régiói meglehetősen jó feladatot, de a egyszer a egy while, hello teljes képernyőt toobe frissítése. Hello felhasználó rendelkezik-e egy magasabb feloldási képernyő (például a 4 KB-os megoldás), ez a frissítés megköveteli a képernyő alsó felbontású (például 1024x768px)-nál nagyobb hálózati sávszélességet. Ugyanez a logika vonatkozik, az átirányítás egynél több képernyő használatakor. Sávszélesség képernyők hello számú tooincrease kell.
* **Vágólap és eszköz-átirányítás** - Ez egy nagyon nyilvánvaló probléma, de sok esetben ha egy felhasználó által tárolt adatok toohello vágólap, nagy adattömb tart, amíg az adott információk tootransfer hello távoli asztali ügyfél bit toohello kiszolgáló. hello alárendelt élmény is negatív hatással lehet hello élménye hello vágólapra tartalom küldése előtt. hello Ugyanez vonatkozik az eszköz átirányítás - Ha képolvasó vagy webkamera hozza létre a nagy mennyiségű adat, amelyet a küldött toobe toohello felsőbb rétegbeli kiszolgáló, nyomtató kell tooreceive nagy dokumentum vagy helyi tárolási igényei toobe elérhető tooan alkalmazás fusson az hello felhő toocopy egy nagy méretű fájlok felhasználók észrevette, hogy az eldobott keretek, vagy átmenetileg "zárolva" videó mert hello eszközátirányítás szükséges hello adatok egyre hello hálózati sávszélesség szükséges. 

Amikor a hálózati sávszélesség szükségleteknek értékeli, győződjön meg arról, hogy tooconsider minden e tényezők működik-e a rendszer.

Most, lépjen vissza toohello [fő hálózati sávszélesség cikk](remoteapp-bandwidth.md), vagy helyezze át az tootesting a [hálózati sávszélességet](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Részletek
* [Azure RemoteApp a sávszélesség-használat becslése](remoteapp-bandwidth.md)
* [Az Azure RemoteApp – olyan gyakori forgatókönyveket tartalmaz, a hálózati sávszélesség-használat tesztelése](remoteapp-bandwidthtests.md)
* [Az Azure RemoteApp hálózati sávszélesség - általános irányelveket (Ha nem teszteli a saját)](remoteapp-bandwidthguidelines.md)

