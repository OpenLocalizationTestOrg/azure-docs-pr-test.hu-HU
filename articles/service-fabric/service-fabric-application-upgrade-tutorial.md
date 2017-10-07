---
title: "aaaService háló alkalmazás frissítési oktatóanyag |} Microsoft Docs"
description: "Ez a cikk végigvezeti a Service Fabric-alkalmazás telepítése, hello kód megváltoztatása és működés közbeni frissítés ki a Visual Studio használatával hello élménye."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a3181a7a-9ab1-4216-b07a-05b79bd826a4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: d069ff0b291018dbac846e65cddff1e9d73d156c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>A Service Fabric frissítés oktatóanyag a Visual Studio használatával
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Az Azure Service Fabric egyszerűbben hello a felhőalapú alkalmazások fejlesztésével győződjön meg arról, hogy csak a módosított szolgáltatások frissíti, és, hogy az alkalmazás állapotának hello frissítési folyamat során a rendszer figyeli. Azt is automatikusan visszaállítja a hello toohello előző verziója problémák észlelése esetén. A Service Fabric-alkalmazás frissítéseket *nulla állásidő*, mert az hello alkalmazás frissítése állásidő nélkül. Ez az oktatóanyag ismerteti hogyan toocomplete a működés közbeni frissítés a Visual Studio eszközből.

## <a name="step-1-build-and-publish-hello-visual-objects-sample"></a>1. lépés: Létrehozása és közzététele hello Visual objektumok minta
Első lépésként töltse le a hello [Visual objektumok](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Actors/VisualObjects) alkalmazás a Githubról. Majd létre, és tegye közzé a hello alkalmazást kattintson a jobb gombbal a projekt hello, **VisualObjects**, és kiválasztja a hello **közzététel** hello Service Fabric menüpont parancsot.

![A Service Fabric-alkalmazás a helyi menü][image1]

Kiválasztása **közzététel** megjelenik egy előugró ablak, és beállíthatja a hello **céloz profil** túl**PublishProfiles\Local.xml**. hello ablak hasonlóan kell kinéznie hello, végül kattintson a következő **közzététel**.

![A Service Fabric-alkalmazás közzététele][image2]

Most kattintson **közzététel** hello párbeszédpanelen. Használhat [Service Fabric Explorer tooview hello fürt és hello alkalmazás](service-fabric-visualizing-your-cluster.md). hello Visual objektumok alkalmazás rendelkezik egy webszolgáltatás-bővítmény, hogy lépjen tooby beírásával [http://localhost:8081/visualobjects/](http://localhost:8081/visualobjects/) hello címsorába a böngésző.  Meg kell jelennie az üdvözlő képernyőt Navigálás 10 lebegőpontos látható objektumokat.

**Megjegyzés:** Ha túl üzembe`Cloud.xml` profil (Azure Service Fabric), hello alkalmazás érhető el kell **http://{ServiceFabricName}. { Region}.cloudapp.Azure.com:8081/visualobjects/**. Győződjön meg arról, hogy `8081/TCP` hello terheléselosztó konfigurált (hello terheléselosztó található hello ugyanabban az erőforráscsoportban hello Service Fabric-példányként).

## <a name="step-2-update-hello-visual-objects-sample"></a>2. lépés: A frissítés hello Visual objektumok minta
Bizonyára észrevette, hogy, hogy az 1. lépésben telepített hello verziójával, hello visual objektumok nem elforgatása. Most frissítse az alkalmazás tooone, ahol hello látható objektumokat is elforgatása.

Válassza ki a hello VisualObjects.ActorService projekt hello VisualObjects megoldás belül, és nyissa meg a hello **VisualObjectActor.cs** fájlt. A fájlba, lépjen a toohello metódus `MoveObject`, megjegyzéssé `visualObject.Move(false)`, és állítsa vissza `visualObject.Move(true)`. A kód módosítása hello objektumok elforgatja, hello szolgáltatás frissítése után.  **Most hozhat létre (nem rebuild) megoldás hello**, projektek hello hoz létre, amely módosította. Ha *építse újra az összes*, az összes hello projektek tooupdate hello verziójával rendelkezik.

Azt is kell tooversion az alkalmazás. toomake hello verzió módosítása után a jobb gombbal a hello **VisualObjects** projekt, használhatja a Visual Studio hello **Manifest verziók szerkesztése** lehetőséget. E beállítás megadásával megjeleníti hello párbeszédpanelt edition verziók az alábbiak szerint:

![Versioning párbeszédpanel][image3]

Frissítési hello verziók hello módosítani a projektek és a kód csomagok hello alkalmazás tooversion 2.0.0 együtt. Hello módosítása után, hello jegyzékfájl hello hasonlóan kell kinéznie (félkövér részei megjelenítése hello módosítások):

![Verziók frissítése][image4]

hello Visual Studio eszközök teheti verziók kiválasztása után az automatikus frissítések **automatikus frissítése az alkalmazás és szolgáltatás verziók**. Ha [SemVer](http://www.semver.org), tooupdate hello kódot kell és/vagy a konfigurációs csomag verziója önmagában, ha a beállítást.

Hello módosítások mentéséhez, és most ellenőrizzük hello **frissítési hello alkalmazás** mezőbe.

## <a name="step-3--upgrade-your-application"></a>3. lépés: Az alkalmazás frissítése
Ismerkedjen meg hello [alkalmazás frissítési paraméterei](service-fabric-application-upgrade-parameters.md) és hello [frissítési folyamat](service-fabric-application-upgrade.md) tooget különböző paraméterek időtúllépéseket és állapotfigyelő feltételt, amely képes frissíteni hello beható ismerete alkalmazható. Ennél a bemutatónál hello szolgáltatás állapotának kiértékelési feltételnek toohello alapértelmezett (a nem figyelt mód) van beállítva. Ezeket a beállításokat konfigurálhatja kiválasztásával **beállítások konfigurálása** hello paraméterek tetszés szerint módosításával.

Most a folyamatban, minden set toostart hello alkalmazás frissítési kiválasztásával **közzététel**. Ezt a lehetőséget az alkalmazás tooversion 2.0.0, amelyben hello objektumok elforgatása frissíti. A Service Fabric frissíti a frissítési tartományok (néhány objektum frissíti először, mások követ) egyszerre, és hello frissítés során hello szolgáltatás elérhető marad. Toohello szolgáltatás az ügyfél (böngésző) keresztül ellenőrizhetők.  

Most, mivel hello alkalmazás frissítési folytatódik, keresztül figyelheti a Service Fabric Explorerrel hello segítségével **folyamatban lévő frissítések** lapon a hello alkalmazások.

Néhány perc múlva minden frissítés tartományok (elvégezve) kell frissíteni, és hello Visual Studio kimeneti ablakában is tartalmaznia kell, hogy hello frissítés befejeződött. És keresse meg, amely *összes* hello vizuális objektum a böngészőablakban most vannak elforgatása!

Előfordulhat, hogy a tootry hello verziók módosítása, és tér át verzió 2.0.0 tooversion 3.0.0 gyakorlat szerint szeretné, vagy még verziójáról 2.0.0 biztonsági tooversion 1.0.0. Lejátszásához időtúllépések és állapotfigyelő házirendek toomake saját kezűleg ismeri a őket. Amikor tooa helyi fürt üzembe helyezése Azure fürtöt tooan ellenezte, használt hello paraméterek toodiffer is rendelkezhet. Azt javasoljuk, hogy hello időtúllépéseket konzervatív módon.

## <a name="next-steps"></a>Következő lépések
[Az alkalmazás használatával PowerShell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.

Szabályozza, hogyan az alkalmazás frissítés használatával [frissítési paraméterek](service-fabric-application-upgrade-parameters.md).

Az alkalmazásfrissítéseket Learning kompatibilissé hogyan toouse [adatszerializálás](service-fabric-application-upgrade-data-serialization.md).

Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[témakörök speciális](service-fabric-application-upgrade-advanced.md).

Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [alkalmazásfrissítések hibaelhárítási](service-fabric-application-upgrade-troubleshooting.md).

[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
