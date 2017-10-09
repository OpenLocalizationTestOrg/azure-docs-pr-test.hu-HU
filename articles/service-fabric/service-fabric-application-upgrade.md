---
title: "Fabric-alkalmazás frissítés aaaService |} Microsoft Docs"
description: "Ez a cikk a Service Fabric-alkalmazás, beleértve a választhatja frissítési módok és teljesítő állapot-ellenőrzést egy bevezető tooupgrading tartalmazza."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 6f649ef4a5c0afab682522bcba7d2d66a4268ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade"></a>Service Fabric-alkalmazás frissítése
Az Azure Service Fabric-alkalmazás szolgáltatások gyűjteménye. A frissítés során a Service Fabric összehasonlítja új hello [alkalmazásjegyzék](service-fabric-application-model.md#describe-an-application) hello korábbi verziójával, illetve határozza meg, melyik szolgáltatás hello alkalmazásban frissítés szükséges. A Service Fabric hello szolgáltatásban számok akkor jelentkezik, a hello verziószámok hello korábbi verziójában hello verzió hasonlítja össze. Ha a szolgáltatás nem változott, hogy a szolgáltatás nincs frissítve.

## <a name="rolling-upgrades-overview"></a>Működés közbeni frissítés áttekintése
A működés közbeni alkalmazás frissítés hello frissítés végrehajtása fázisból áll. Egyes szakaszokban hello frissítés hello fürt, egy frissítési tartományi csomópontok alkalmazott tooa részhalmaza. Ennek eredményeképpen hello alkalmazás továbbra is elérhető hello frissítése során. Hello a frissítés során a hello fürt hello régi és új verzióit vegyesen tartalmazhatnak.

Ezért két hello verziószámmal kell előre és hátra kompatibilis. Ha azok nem kompatibilisek, hello alkalmazás-rendszergazda feladata egy több-fázis frissítési toomaintain rendelkezésre állási átmeneti. A több fázisú frissítés hello első lépéseként tooan köztes hello alkalmazás hello korábbi verziójával kompatibilis verziójának frissítését végzi. hello második lépésre tooupgrade hello végleges verziójú, amely megsérti hello frissítés előtti verziójával való kompatibilitás szempontjából, de hello köztes verziójával kompatibilis.

Frissítési tartományok megadott hello fürtjegyzékben hello fürt konfigurálásakor. Frissítési tartományok nem kaphat frissítéseket meghatározott sorrendben. Egy frissítési tartomány nem az alkalmazás központi telepítésének logikai tárolóegységet jelöl. Frissítési tartományok magas rendelkezésre álló hello szolgáltatások tooremain engedélyezése a frissítés során.

Nem működés közbeni frissítés is előfordulhatnak, ha hello frissítés alkalmazott tooall csomópontok hello fürt, amely hello eset esetén hello alkalmazás csak egy frissítési tartomány van. Ez a módszer nem ajánlott, mivel hello szolgáltatás leáll, és a frissítés hello időpontjában nem érhető el. Emellett Azure nem biztosít semmilyen garanciák egy fürt csak egy frissítési tartomány be van állítva.

## <a name="health-checks-during-upgrades"></a>Állapot-ellenőrzési eredményeire frissítéskor
A frissítéshez állapotházirendeket van beállítva toobe (vagy alapértelmezett értékek használható). Frissítés sikeres nevezik amikor megtörténik a frissítési tartományok belül hello időtúllépéseket meg, és minden frissítés tartományok kifogástalan minősülnek.  A megfelelő frissítési tartomány azt jelenti, hogy adott hello frissítési tartomány átadott hello állapotfigyelő házirendben meghatározott összes hello állapotellenőrzést. Például egy olyan házirendet is engedélyezhetik, hogy kell-e egy alkalmazás példányán belül minden szolgáltatás *kifogástalan*, a health Service Fabric által meghatározott.

Házirendek és a Service Fabric által a frissítés során ellenőrzi olyan szolgáltatások és alkalmazások független. Ez azt jelenti, hogy nem szolgáltatásspecifikus tesztek történik.  Például előfordulhat, hogy a szolgáltatás egy átviteli követelményt, de a Service Fabric nincs hello információk toocheck átviteli sebesség. Tekintse meg a toohello [egészségügyi cikkek](service-fabric-health-introduction.md) hello ellenőrzésére kerül sor. hello ellenőrzi, hogy a az e hello alkalmazáscsomag megfelelően, másolja egy frissítési include tesztek során fordulhat elő, hogy hello példány indult el, és így tovább.

hello alkalmazás állapotának összesítését hello gyermek entitások hello alkalmazás. A Service Fabric röviden hello állapotának hello alkalmazást hello állapotadatokat. melyeket hello alkalmazás jelentenek keresztül értékeli ki. Azt is kiértékeli minden hello szolgáltatás hello alkalmazás állapotának hello ily módon. A Service Fabric további hello alkalmazásszolgáltatások hello állapotának értékelje gyermekei, például a hello szolgáltatás replika hello állapotának összesítése. Után az alkalmazás állapotházirendje hello teljesül, hello frissítés végrehajtható. Ha a hello állapotházirend sérül, hello alkalmazás frissítése sikertelen.

## <a name="upgrade-modes"></a>Frissítési módok
hello az alkalmazásfrissítés általunk javasolt módja figyelt hello módban, amely általánosan használt hello mód. Figyelt mód hello frissítést végez egy frissítési tartományt, és ha ellenőrzi az összes (e hello házirend megadva), pass áthelyezése toohello következő frissítési tartományra automatikusan.  Ha állapotának ellenőrzése sikertelen, és/vagy időtúllépéseket elérésekor, hello frissítés vagy vissza lesz állítva hello frissítési tartomány, vagy hello mód módosított toounmonitored manuális. E két mód a sikertelen frissítések hello frissítési toochoose konfigurálhatja. 

A nem figyelt manuális mód kézi beavatkozás után minden frissítés frissítési tartományokon, tookick hello tovább frissítési tartomány hello frissítéséhez ki kell. Nem a Service Fabric állapotellenőrzést végez. a rendszergazda hello hello állapotfigyelő vagy állapotának vizsgálata hello tovább frissítési tartomány hello frissítés megkezdése előtt.

## <a name="upgrade-default-services"></a>Alapértelmezett szolgáltatások frissítésére
Service Fabric-alkalmazás alapértelmezett szolgáltatások frissítése az alkalmazás hello a frissítési folyamat során. Alapértelmezett szolgáltatások definiált hello [alkalmazásjegyzék](service-fabric-application-model.md#describe-an-application). az alapértelmezett szolgáltatások frissítése a hello szabványos szabályok a következők:

1. Az új hello szolgáltatások alapértelmezett [alkalmazásjegyzék](service-fabric-application-model.md#describe-an-application) , amely nem szerepel a hello fürt jönnek létre.
> [!TIP]
> [EnableDefaultServicesUpgrade](service-fabric-cluster-fabric-settings.md#fabric-settings-that-you-can-customize) igények toobe beállítása tootrue tooenable hello szabályainak. Ez a funkció a v5.5 támogatott.

2. Mindkét előző meglévő szolgáltatási alapértelmezett [alkalmazásjegyzék](service-fabric-application-model.md#describe-an-application) és új verziójára frissülnek. Szolgáltatások ismertetése hello új verziójában felülírná már a hello fürt. Az alkalmazásfrissítés alapértelmezett szolgáltatás hibája frissítése után automatikusan visszaállítási lenne.
3. Az előző hello szolgáltatások alapértelmezett [alkalmazásjegyzék](service-fabric-application-model.md#describe-an-application) , de nem az új verzió hello törli. **Vegye figyelembe, hogy a törlési alapértelmezett szolgáltatások nem beállításai állíthatók vissza.**

Abban az esetben, ha egy alkalmazás a frissítés az vissza lesz állítva, az alapértelmezett szolgáltatások visszatért toohello állapot frissítésének megkezdése előtt. De törölt szolgáltatások soha nem hozható létre.

## <a name="application-upgrade-flowchart"></a>Alkalmazás frissítési folyamatábra
hello folyamatábra alábbi azt segítenek megérteni a Service Fabric-alkalmazás hello frissítési folyamatát. Hello folyamatot ismerteti, hogyan hello időtúllépéseket, beleértve a *HealthCheckStableDuration*, *HealthCheckRetryTimeout*, és *UpgradeHealthCheckInterval*, szabályozni, amikor egy frissítési tartomány hello frissítés akkor tekinthető, sikeres, vagy a hiba.

![a Service Fabric-alkalmazás hello frissítési folyamata][image]

## <a name="next-steps"></a>Következő lépések
[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.

[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.

Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).

Az alkalmazásfrissítéseket Learning kompatibilissé hogyan toouse [Adatszerializálás](service-fabric-application-upgrade-data-serialization.md).

Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[Speciális témakörök](service-fabric-application-upgrade-advanced.md).

Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [Alkalmazásfrissítések hibaelhárítási](service-fabric-application-upgrade-troubleshooting.md).

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
