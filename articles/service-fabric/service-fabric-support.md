---
title: "Azure Service Fabric támogatási lehetőségekkel kapcsolatos aaaLearn |} Microsoft Docs"
description: "Az Azure Service Fabric-fürt verzióit támogatja, és hivatkozásokat tartalmaz toofile támogatási jegyek"
services: service-fabric
documentationcenter: .net
author: pkc
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: pkc
ms.openlocfilehash: 42394e2cd9dad2040d37d3a2ff3600ee040d8720
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-support-options"></a>Az Azure Service Fabric támogatási lehetőségek

toodeliver hello megfelelő támogatás a Service Fabric-fürtök, hogy futnak-e az alkalmazás a munkahelyi betölt, azt különböző lehetőségek beállítását. Attól függően, hogy hello szintű támogatást szükséges, és hello probléma hello súlyossága, kapott toopick hello megfelelő beállításokat. 


<a id="getlivesitesupportonazure"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-azure"></a>Éles vagy élő hely problémákat, vagy kérje meg az Azure-fizetős támogatási

Élő hely problémák jelentések a telepített Azure Service Fabric-fürt, nyissa meg a jegyet a professzionális támogatás [Azure-portál](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) vagy [a Microsoft támogatási portal](http://support.microsoft.com/oas/default.aspx?prid=16146).

További információ:
 
- [A Microsoft Azure-beli professzionális támogatás](https://azure.microsoft.com/en-us/support/plans/?b=16.44).
- [Microsoft Premier szintű támogatás](https://support.microsoft.com/en-us/premier).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Éles vagy élő hely problémákat, vagy kérje meg a Service Fabric-fürtök különálló fizetős támogatási

A helyszíni élő hely problémák jelentés készítését a Service Fabric-fürt telepítése vagy a többi felhőből, nyissa meg a jegyet a professzionális támogatás a [a Microsoft támogatási portal](http://support.microsoft.com/oas/default.aspx?prid=16146).

További információ:

- [A helyszíni Microsoft professzionális támogatás](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Microsoft Premier szintű támogatás](https://support.microsoft.com/en-us/premier).


<a id="getsupportonissues"></a>
## <a name="report-azure-service-fabric-issues"></a>A jelentés Azure Service Fabric-problémák 
Jelenleg a Service Fabric problémák jelentési egy GitHub-tárház beállítása.  Azt is aktívan figyeli a következő fórumok hello.

### <a name="github-repo"></a>GitHub-tárház 
Azure Service Fabric problémák jelentése a [szolgáltatás-háló-problémák git-tárház](https://github.com/Azure/service-fabric-issues). A tárház jelentéskészítési és nyomkövetési problémákat, az Azure Service Fabric, adja meg a megfelelő kis funkciókra vonatkozó kérések számára készült. **Ne használja a tooreport live-hely problémák**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow és MSDN-fórumok

Hello [StackOverflow a Service Fabric címke] [ stackoverflow] és hello [MSDN fórum Service Fabric] [ msdn-forum] biztosan legjobb kapcsolatos kérdéseket feltenni használt hello platform működése, és hogyan lehet vele bizonyos feladatok elvégzéséhez.

### <a name="azure-feedback-forum"></a>Az Azure visszajelzési fórumon

Hello [Azure visszajelzési fórumon a Service Fabric] [ uservoice-forum] hello a legjobb hely a nagy szolgáltatás ötleteket hello termék, a legnépszerűbb kérelmek hello tanulmányozzák rendelkezik a közepes toolong kifejezés részei küldésére a tervezési. Javasoljuk, a javaslatok hello Közösségben toorally támogatása.


<a id="releasesuport"></a>
## <a name="supported-service-fabric-versions"></a>A Service Fabric-verziója támogatott.

Győződjön meg arról, hogy a fürt mindig fut a Service Fabric támogatott verziója. Amennyiben, és amikor azt hello kiadása a Service Fabric verziójának bejelentése, hello korábbi verziója legalább attól az időponttól 60 nap után végéhez van megjelölve. hello új kiadásokat történik bejelentés [hello Service Fabric csapat blogjában](https://blogs.msdn.microsoft.com/azureservicefabric/).

Tekintse meg a dokumentumok részletes információt a következő toohello tookeep a Service Fabric támogatott verzióját futtató fürtre.

- [Egy Azure fürt frissítési Service Fabric-verzió](service-fabric-cluster-upgrade.md)
- [Service Fabric verziója a különálló windows server-fürt frissítése](service-fabric-cluster-upgrade-windows-server.md)
 
Az alábbiakban hello hello Service Fabric által támogatott verziók listáját és azok támogatás záró dátumát.

| **Service Fabric-futtatókörnyezet fürt** | **Kompatibilis SDK / NuGet csomag verziója** | **Támogatási dátum vége** |
| --- | --- | --- |
| A korábbi verziók too5.3.121 fürt összes |Kisebb vagy egyenlő tooversion 2.3 |2017. január 20. |
| 5.3.* |Kisebb vagy egyenlő tooversion 2.3 |2017. február 24. |
| 5.4.* |Kisebb vagy egyenlő tooversion 2.4 |Előfordulhat, hogy 10,2017     |
| 5.5.* |Kisebb vagy egyenlő tooversion 2,5 |Augusztus 10,2017    |
| 5.6.* |Kisebb vagy egyenlő tooversion 2.6 |Október 13,2017    |
| 5.7.* |Kisebb vagy egyenlő tooversion 2.7 |Aktuális verzióra, és ezért nincs befejezési dátum

<a id="previewversion"></a>
## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric előzetes verzió – üzemi használatra nem támogatott.
Az idő tootime azt verzióira, amelyek azt szeretnénk, ha visszajelzést, amelyeket mintaként jelentős funkciót. Ezen előzetes verziói csak tesztelési célra használható. Az éles fürt mindig futnia kell egy támogatott, stabil, a Service Fabric-verzió. Előzetes verzióval mindig a fő- és alverzió verziószámát 255 kezdődik. Például, ha a Service Fabric verzió 255.255.5703.949 című kiadás a verzió nem csak toobe tesztfürtökön szerepel, és jelenleg előzetes verzióban érhető. Ezen előzetes kiadások is történik bejelentés hello [Service Fabric csapat blogja](https://blogs.msdn.microsoft.com/azureservicefabric) és részletek rendelkezzen hello szolgáltatásait.

Nincs a preview kiadásokban fizetős támogatási lehetőség. Hello lehetőség alatt felsorolva [jelentés Azure Service Fabric problémák](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) tooask kérdésekre, vagy visszajelzést.

## <a name="next-steps"></a>Következő lépések

- [Service fabric-verzió egy Azure fürt frissítése](service-fabric-cluster-upgrade.md)
- [Service Fabric verziója a különálló windows server-fürt frissítése](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples
