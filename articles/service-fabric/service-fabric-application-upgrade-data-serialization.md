---
title: "Az alkalmazásfrissítés: adatszerializálás |} Microsoft Docs"
description: "Ajánlott eljárások az adatok szerializálása és közbeni alkalmazás hatása."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: a5f36366-a2ab-4ae3-bb08-bc2f9533bc5a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 1b65dfd3813423550631490640a81953864f58e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Hogyan befolyásolja a adatszerializálás az alkalmazás frissítés
Az egy [működés közbeni frissítés alkalmazás](service-fabric-application-upgrade.md), hello frissítés alkalmazott tooa részhalmazát csomópontok, egyszerre több frissítési tartományt. A folyamat során néhány frissítési tartományok hello az alkalmazás újabb verzióját, és néhány frissítési tartományok hello az alkalmazás régebbi verzióján. Hello bevezetés alatt hello új az alkalmazás verziójúnak kell lennie képes tooread hello régi adatait, és hello régi az alkalmazás verziójúnak kell lennie képes tooread hello új adatait. Ha hello adatok formátuma nem kompatibilis, hello frissítés előre és hátra sikertelen vagy rosszabb, adatok előfordulhat, hogy elvész vagy sérült. Ez a cikk ismerteti, mi a adatformátum számít, és annak biztosítása, hogy az adatok előre és hátra ajánlott eljárásai kínál kompatibilis.

## <a name="what-makes-up-your-data-format"></a>Mi az adatformátum alkotó?
Az Azure Service Fabric megőrzött és a replikált hello adatok a C# osztályok származik. Használó alkalmazások esetén [megbízható gyűjtemények](service-fabric-reliable-services-reliable-collections.md), hogy adatokat hello objektumok hello megbízható szótárakat és a várólisták-e. Használó alkalmazások esetén [Reliable Actors](service-fabric-reliable-actors-introduction.md), vagyis hello szereplő állapota biztonsági hello. Ezeket az osztályokat C# szerializálhatónak kell lennie toobe tárolása és replikálása. Ezért hello adatformátum induló hello mezőit és tulajdonságait, amelyek szerializálva vannak, valamint hogy szerializálva vannak. Például egy `IReliableDictionary<int, MyClass>` hello adatokat egy szerializált `int` és egy szerializált `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Kód változik, hogy akár egy adatok formátumának módosítása
Hello adatformátum C# osztályok határozza meg, mert a módosítások toohello osztályok egy adatok formátumának módosítása okozhatja. Ügyelni kell a működés közbeni frissítés kezelhető tooensure hello adatok formázása módosítása. Példák, amelyek okozhatnak adatok formázása módosításokat:

* Hozzáadásával vagy eltávolításával a mezők és tulajdonságok
* Mezők és tulajdonságok átnevezése
* Hello típusú mezők és tulajdonságok módosítása
* Hello osztály nevet vagy névteret módosítása

### <a name="data-contract-as-hello-default-serializer"></a>Adategyezmény, alapértelmezett szerializáló hello
hello szerializáló felelős általában hello adatainak olvasása és deszerializálása azt hello aktuális verziójában, még akkor is, ha hello adatok korábbi vagy *újabb* verziója. hello alapértelmezett szerializáló hello [adategyezmény-szerializáló](https://msdn.microsoft.com/library/ms733127.aspx), amely jól meghatározott versioning szabályokkal rendelkeznek. Gyűjtemények engedélyezése hello szerializáló toobe felül, de a Reliable Actors jelenleg nem megbízható. hello adatok szerializáló a működés közbeni frissítés fontos szerepet játszik. hello adategyezmény-szerializáló, amely a Service Fabric-alkalmazásokhoz ajánlott hello szerializáló.

## <a name="how-hello-data-format-affects-a-rolling-upgrade"></a>Hogyan befolyásolja az hello adatformátum a működés közbeni frissítés
A működés közbeni frissítés során nincsenek kétféle módon történhet, ahol hello szerializáló régebbi észlelhetnek vagy *újabb* az adatok verziója:

1. Miután egy csomópont frissítve van, és biztonsági mentése elindul, hello új szerializáló hello régi verziója megőrzött toodisk hello adat tölti be.
2. Működés közbeni frissítés hello, során hello fürt hello régi és új verziókat a kód vegyesen fogja tartalmazni. Mivel replikák különböző frissítési tartományok kerülnek, és a replikák küldése adatok tooeach más, hello új és/vagy a régi verziója, az adatok a szerializáló az új és/vagy a régi verziója hello fordulhatnak elő.

> [!NOTE]
> "új" és "régi verziója" Hello itt tekintse meg a kódot futtató toohello verzióját. "új szerializáló" Hello, amely végrehajtja az az alkalmazás új verziójának hello toohello szerializáló kód hivatkozik. hello "új adatok" az alkalmazás új verziójának hello szerializált toohello C# osztály hivatkozik.
> 
> 

hello kód két verziója és az adatformátum előre és hátra kell lennie. kompatibilis. Ha nem kompatibilis, működés közbeni frissítés hello sikertelenek lehetnek, vagy adatvesztés történhet. működés közbeni frissítés hello sikertelen lehet, mert hello kód vagy a szerializáló generálhat kivételek vagy hiba esetén hello más verziót. Adatok elveszhetnek, ha például egy új tulajdonság hozzá lett adva, de hello régi szerializáló elveti deszerializálása során.

Adategyezmény ajánlott megoldás annak biztosítása, hogy az adatok kompatibilis hello. Rendelkezik, jól meghatározott versioning szabályok hozzáadása, eltávolítása és mezők módosítása Azt is támogatja az ismeretlen mezők foglalkoznak, hello szerializálása és deszerializálása folyamatba csatlakoztatás és osztályöröklődést foglalkoznak. További információkért lásd: [adategyezmény használatával](https://msdn.microsoft.com/library/ms733127.aspx).

## <a name="next-steps"></a>Következő lépések
[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.

[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.

Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).

Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[Speciális témakörök](service-fabric-application-upgrade-advanced.md).

Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [Alkalmazásfrissítések hibaelhárítási ](service-fabric-application-upgrade-troubleshooting.md).

