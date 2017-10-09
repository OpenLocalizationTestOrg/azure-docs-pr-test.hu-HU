---
title: "Service Fabric üzemeltetési modell aaaAzure |} Microsoft Docs"
description: "Replikák (vagy példányok) egy telepített szolgáltatás Fabric-szolgáltatás és folyamata közötti kapcsolatot ismerteti."
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: 30e0ba19cd672a722f76477a311ecef7c213b055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-hosting-model"></a>A Service Fabric üzemeltetési modell
Ez a cikk a Service Fabric által biztosított modellek futtató alkalmazás áttekintést nyújt, és leírja a hello hello különbségei **megosztott folyamat** és **kizárólagos folyamat** modellek. Leírja, hogyan egy telepített alkalmazás néz ki egy Service Fabric-csomópont és a replikák (vagy példányok) hello szolgáltatás és hello szolgáltatás gazdafolyamat közötti kapcsolat.

A folytatás előtt, győződjön meg arról, hogy jártas [Service Fabric alkalmazásmodell] [ a1] és megismerheti a különféle fogalmakat és a közöttük lévő kapcsolat. 

> [!NOTE]
> Ebben a cikkben az egyszerűség kivéve, ha explicit módon említett:
>
> - Minden használ, a word *replika* tooboth egy replikát készít egy állapotalapú service vagy egy statless szolgáltatás egy példánya hivatkozik.
> - *CodePackage* megfelel a kezelt túl*ServiceHost* folyamat, amely regisztrálja a *ServiceType* és az adott szolgáltatások állomások replikák *ServiceType*.
>

toounderstand hello üzemeltetési modell, ossza meg velünk ismerteti egy példán keresztül. Ossza meg velünk tegyük fel például, van egy *ApplicationType* "MyAppType", amely rendelkezik egy *ServiceType* "MyServiceType" által biztosított *ServicePackage* "MyServicePackage", amely rendelkezik egy *CodePackage* "MyCodePackage", amely *ServiceType* "MyServiceType" futtatásakor.

Tegyük fel, 3 csomópontból álló fürt tudunk és létrehozhatunk egy *alkalmazás* **fabric: / App1** "MyAppType" típusú. Ez belül *alkalmazás* **fabric: / App1** létrehozhatunk egy szolgáltatás **háló: / App1/ServiceA** "MyServiceType", amelynek 2 partíció típusú (tegyük fel például **P1** & **P2**) és partíciónként 3 replikák. hello alábbi ábrán látható, mert az alkalmazás hello nézet említi telepített egy csomóponton.

<center>
![Telepített alkalmazás csomópont ábrázolása][node-view-one]
</center>

A Service Fabric aktiválása "MyServicePackage", "MyCodePackage", amely mindkét hello partíciókból replikák azaz üzemeltető indítottak **P1** & **P2**. Ne feledje, hogy hello fürt összes csomópontjának hello lesz azonos nézet óta hello fürt választottuk replikák száma egyenlő toonumber partíció csomópontok száma. Hozzon létre egy másik szolgáltatás **fabric: / App1/ServiceB** alkalmazásban **fabric: / App1** 1 partíciót tartalmaz (tegyük fel például **P3**) és 3 replikák partíciónként. Alábbi ábrán látható hello új nézet hello csomóponton:

<center>
![Telepített alkalmazás csomópont ábrázolása][node-view-two]
</center>

A Service Fabric elhelyezett hello új partíció replikája láthatja, **P3** szolgáltatás **fabric: / App1/ServiceB** a meglévő aktiválásának hello "MyServicePackage". Most segítségével létrehozhat egy másik *alkalmazás* **fabric: / App2** "MyAppType" típusú, és belül **fabric: / App2** szolgáltatás létrehozása **háló: / App2/ServiceA** 2-partíciókkal rendelkezik, amely (tegyük fel például **P4** & **P5**) és partíciónként 3 replikák. A következő ábrákon látható hello új csomópont megtekintése:

<center>
![Telepített alkalmazás csomópont ábrázolása][node-view-three]
</center>

Jelenleg a Service Fabric aktiválva van a "MyServicePackage", "MyCodePackage" és a replikák új szolgáltatás mindkét partíciókból elinduló másolatát **fabric: / App2/ServiceA** (azaz **P4** & **P5**) kerülnek, az új példány "MyCodePackage".

## <a name="shared-process-model"></a>Megosztott folyamatmodell
Mi látott fent hello alapértelmezett futtatási modell Service Fabric által biztosított, és említett tooas **megosztott folyamat** modell. Ebben a modellben az egy adott *alkalmazás*, csak egy példányát egy adott *ServicePackage* aktiválódik egy *csomópont* (amely elindítja az összes hello *CodePackages* benne tárolt) összes hello replikákat az összes olyan szolgáltatás, és egy adott *ServiceType* hello kerülnek *CodePackage* , amely regisztrálja, amelyek *ServiceType*. Ez azt jelenti, az összes hello replikákat az összes szolgáltatások egy adott *ServiceType* megosztása hello ugyanazt a folyamatot.

## <a name="exclusive-process-model"></a>Kizárólagos folyamatmodell
hello Service Fabric által biztosított egyéb üzemeltetési modell van **kizárólagos folyamat** modell. Ebben a modellben az egy adott *csomópont*, minden egyes replikának elhelyezéséhez, a Service Fabric egy új példányát aktiválja *ServicePackage* (amely elindítja az összes hello *CodePackages* benne tárolt ), és a replika az hello *CodePackage* , hogy regisztrált hello *ServiceType* hello szolgáltatás toowhich a replika tartozik. Ez azt jelenti minden egyes replikának él, a saját dedikált folyamat. 

Ez a modell verzióját a Service Fabric 5.6 verziójától kezdve alkalmazható. **Kizárólagos folyamat** modell választható ki hello időpontban hello szolgáltatás létrehozása (használatával [PowerShell][p1], [REST] [ r1]vagy [FabricClient][c1]) megadásával **ServicePackageActivationMode** mint "ExclusiveProcess".

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Ha egy alapértelmezett szolgáltatás a alkalmazásjegyzékben, választhat **kizárólagos folyamat** modellre megadásával **ServicePackageActivationMode** attribútumot a lent látható módon:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Folytatása a fenti példában, lehetővé teszi, hogy hozzon létre egy másik szolgáltatást **fabric: / App1/ServiceC** alkalmazásban **fabric: / App1** 2-partíciókkal rendelkezik, amely (tegyük fel például **P6**  &  **S7**) és a partíciónként 3 replikák **ServicePackageActivationMode** too'ExclusiveProcess' beállítása. Alábbi ábrán látható hello csomóponton új nézet:

<center>
![Telepített alkalmazás csomópont ábrázolása][node-view-four]
</center>

Ahogy látja, a Service Fabric aktiválása "MyServicePackage" két új példányát (egy partícióról minden egyes replikához **P6** & **S7**) és minden egyes replikának helyezett dedikált példányát *CodePackage*. Egy másik művelet toonote itt van, amikor **kizárólagos folyamat** modellt használja, az egy adott *alkalmazás*, először az egy adott *ServicePackage* lehet aktív egy  *Csomópont*. A fenti példában látható, hogy a "MyServicePackage" három másolatot aktívak a **fabric: / App1**. "MyServicePackage" aktív másolatait mindegyike rendelkezik egy **ServicePackageAtivationId** társítva belül példányt azonosítja *alkalmazás* **fabric: / App1**.

Ha csak **megosztott folyamat** modellje egy *alkalmazás*, például **fabric: / App2** a fenti példában esetében csak egy aktív példányát *ServicePackage* a egy *csomópont* és **ServicePackageAtivationId** a aktiválásához *ServicePackage* "üres karakterlánc".

> [!NOTE]
>- **Megosztott folyamat** futtatási modell megfelel-e túl**ServicePackageAtivationMode** egyenlő **SharedProcess**. Ez az hello alapértelmezett futtatási modell és **ServicePackageAtivationMode** hello hello szolgáltatás létrehozása során nem kell megadni.
>
>- **Kizárólagos folyamat** futtatási modell megfelel-e túl**ServicePackageAtivationMode** egyenlő **ExclusiveProcess** és toobe explicit módon megadott hello hello létrehozása során a szolgáltatás. 
>
>- A szolgáltatás üzemeltetési modell is ismert hello lekérdezésével [szolgáltatás leírását] [ p2] és megnézi a értékének **ServicePackageAtivationMode**.
>
>

## <a name="working-with-deployed-service-package"></a>A telepített service csomag használata
Egy aktív másolatot készít egy *ServicePackage* csomóponton kezeli [telepített service-csomag][p3]. Korábban már említettük, amikor **kizárólagos folyamat** modell használt szolgáltatások, hozzon létre egy adott *alkalmazás*, lehet ugyanaz a hello csomagok több telepített szolgáltatások  *ServicePackage*. Műveletek során meghatározott toodeployed szolgáltatáscsomagot, például [telepített szervizcsomag állapotának reporting] [ p4] vagy [kódcsomag egy telepített service-csomagújraindítása] [ p5] stb., **ServicePackageActivationId** igényeinek toobe megadott tooidentify egy adott központilag telepített service-csomag.

 **ServicePackageActivationId** a telepített szolgáltatások csomag hello listája lekérdezésével kapott [telepített szervizcsomagok] [ p3] csomóponton. Lekérdezésekor [szolgáltatástípusok telepített][p6], [replikák telepített] [ p7] és [kód csomagoktelepített] [ p8] a csomóponton hello lekérdezés eredménye is tartalmaz hello **ServicePackageActivationId** szülő telepített service-csomag.

> [!NOTE]
>- A **megosztott folyamat** futtatási modell, a egy adott *csomópont*, az egy adott *alkalmazás*, csak egy példányát egy *ServicePackage* aktiválva van. Rendelkezik **ServicePackageActivationId** túl egyenlő*üres karakterlánc* és nem szükséges megadni teljesítő telepített szervizcsomag kapcsolatos műveletek közben. 
>
> - A **kizárólagos folyamat** futtatási modell, az egy adott *csomópont*, az egy adott *alkalmazás*, egy vagy több példányát egy *ServicePackage* lehet aktív. Az egyes aktiválások rendelkezik egy *nem üres* **ServicePackageActivationId** toobe megadott teljesítő telepített szervizcsomag kapcsolatos műveletek közben, ezért. 
>
> - Ha **ServicePackageActivationId** ommited too'empty karakterlánc alapértelmezés szerint az ". Ha a telepített szolgáltatások csomag, amely alatt aktiválása **megosztott folyamat** modell jelen, akkor a művelet történik, ellenkező esetben hello művelet sikertelen lesz.
>
> - Nem ajánlott egyszer tooquery és a gyorsítótár **ServicePackageActivationId** dinamikusan generált, és számos okból módosíthatja. Igénylő művelet végrehajtása előtt **ServicePackageActivationId**, először meg kell lekérdezni hello listája [telepített szervizcsomagok] [ p3] egy csomópontot, majd használja a *ServicePackageActivationId** lekérdezés eredménye tooperform hello eredeti műveletből.
>
>

## <a name="guest-executable-and-container-applications"></a>Vendég végrehajtható parancs és a tároló alkalmazások
Kezeli a Service Fabric [Vendég végrehajtható] [ a2] és [tároló] [ a3] alkalmazások vannak statless szolgáltatásként nincs azaz nem a Service Fabric-futtatókörnyezet *ServiceHost* (a folyamatot vagy tároló). Mivel ezek a szolgáltatások önálló, a replikák másodpercenkénti száma *ServiceHost* esetén nem alkalmazható ezeket a szolgáltatásokat. hello ezekkel a szolgáltatásokkal használt leggyakoribb konfigurációja, egypartíciós rendelkező [InstanceCount] [ c2] egyenlő túl-1 (azaz egy másolatot hello szolgáltatást kód fürt mindegyik csomópontján fut). 

alapértelmezett hello **ServicePackageActivationMode** ezeket a szolgáltatásokat a **SharedProcess** ebben az esetben a Service Fabric csak akkor aktiválódik, egy példányát *ServicePackage* a egy *Csomópont* az egy adott *alkalmazás* ami azt jelenti, hogy a szolgáltatás kódot csak egy példányban fog futni egy *csomópont*. Ha szeretné a szolgáltatást kód toorun több példányát egy *csomópont* több szolgáltatás létrehozásakor (*Service1* túl*ServiceN*) a *ServiceType* (a megadott *ServiceManifest*), vagy ha a szolgáltatás több particionált, meg kell adnia **ServicePackageActivationMode** , **ExclusiveProcess**hello hello szolgáltatás létrehozása során.

## <a name="changing-hosting-model-of-an-existing-service"></a>Meglévő szolgáltatás üzemeltetési modelljének megváltoztatása
A meglévő szolgáltatás üzemeltetési modelljének megváltoztatása **megosztott folyamat** túl**kizárólagos folyamat** és viszont keresztül frissítése vagy mechanizmus frissítése (vagy az alapértelmezett szolgáltatás meghatározása az alkalmazás a jegyzékfájl) jelenleg nem támogatott. Ez a szolgáltatás támogatása későbbi verzióiban határozza meg.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>Választás az megosztott folyamat és kizárólagos folyamatmodell
Ezek a modellek üzemeltető rendelkezik az előnyei és hátrányai mind a felhasználó számára szükséges tooevaluate azt, amelyiket a követelményeinek legjobban megfelelő. **Megosztott folyamat** modell lehetővé teszi, hogy az operációs rendszer erőforrások jobb használatát, mert kevesebb folyamatok indított vannak, a több replika hello azonos folyamat megoszthatja a portokat, stb. Azonban hello replikák egyik találatok, ahol toobring hello szolgáltatásgazda le kell hiba, ha azt egyéb ugyanabban a folyamatban lévő összes replika hatással lesz.

 **Kizárólagos folyamat** modell és a saját folyamat minden replika jobb elkülönítést is biztosít, és egy átirányítóban replika nem befolyásolja a többi. Az ismét hasznos olyan esetekben, ahol port megosztása nem támogatja a hello kommunikációs protokollhoz. Elősegíti a hello képességét tooapply erőforrás irányítás replika szinten. Ugyanakkor, a hello **kizárólagos folyamat** fog több erőforrást az operációs rendszer, akkor minden egyes replikához hello csomóponton egy folyamatot indít.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Kizárólagos folyamatmodell és alkalmazás modell kapcsolatos szempontok
hello ajánlott módja toomodel az alkalmazás a Service Fabric rendszer egyik tookeep *ServiceType* / *ServicePackage* és a legtöbb hello alkalmazások esetén ez a modell működik. 

Azonban tooenable speciális forgatókönyvek Ha az egyik *ServiceType* / *ServicePackage* nem lehet hasznos, működését tekintve a Service Fabric lehetővé teszi, hogy toohave egynél több *ServiceType* / *ServicePackage* (és egy *CodePackage* regisztrálhatja az egynél több *ServiceType*). Az alábbiakban néhány hello forgatókönyv, ahol ezek a beállítások akkor lehet hasznos:

- Kívánt származtatását kevesebb folyamatokat, és amelyek magasabb replika sűrűsége folyamatonként toooptimize OS erőforrás-használat.
- A különböző ServiceTypes replikák szükség tooshare közös magas inicializálási vagy memória költsége.
- Szabad szolgáltatásajánlat rendelkezik, és a kívánt erőforrás-használat korlátozása tooput hello szolgáltatás összes replika tegyen ugyanazon folyamat során.

**Kizárólagos folyamat** futtatási modell nincs összefüggő több alkalmazás modellel *ServiceTypes* / *ServicePackage*. Ez azért, mert több *ServiceTypes* / *ServicePackage* tervezett tooachieve magasabb szintű erőforrás-megosztás replikák között, és lehetővé teszi, hogy magasabb replika sűrűség folyamatonként. Ez az ellentétes toowhat **kizárólagos folyamat** modell tervezett tooachieve.

Vegye figyelembe a hello eset több *ServiceTypes* / *ServicePackage* másik *CodePackage* regisztrálása egyes *ServiceType*. Tegyük fel, van egy *ServicePackage* "MultiTypeServicePackge", amely két *CodePackages*:

- "MyCodePackageA", amely *ServiceType* "MyServiceTypeA".
- "MyCodePackageB", amely *ServiceType* "MyServiceTypeB".

Mostantól lehetővé teszi, hogy tegyük fel például, létrehozhatunk egy *alkalmazás* **fabric: / SpecialApp** és a belső **fabric: / SpecialApp** tudjuk létrehozni a következő két szolgáltatást együtt **kizárólagos folyamat** modell:

- Szolgáltatás **fabric: / SpecialApp/ServiceA** "MyServiceTypeA" típusú, és két partíció (tegyük fel például **P1** és **P2**) és 3 replikák partíciónként.
- Szolgáltatás **fabric: / SpecialApp/ServiceB** "MyServiceTypeB" típusú, és két partíció (tegyük fel például **P3** és **P4**) és 3 replikák partíciónként.

Egy adott csomópont mindkét hello szolgáltatást két replika lesz. Mivel használtuk **kizárólagos folyamat** modell toocreate hello szolgáltatások, a Service Fabric aktiválódik egy új példányt a "MyServicePackge" minden egyes replikához. Az egyes aktiválások "MultiTypeServicePackge" a "MyCodePackageA" és "MyCodePackageB" másolatát indul el. Azonban csak az egyik "MyCodePackageA" vagy "MyCodePackageB" hello replika, amelynek "MultiTypeServicePackge" aktiválása tárolni fogja. Alábbi ábrán látható hello csomópont megtekintése:

<center>
![Telepített alkalmazás csomópont ábrázolása][node-view-five]
</center>

 Ahogyan azt láthatja, a partíció replikája "MultiTypeServicePackge" hello aktiválásának **P1** szolgáltatás **fabric: / SpecialApp/ServiceA**, hello replikát üzemeltető "MyCodePackageA" és " MyCodePackageB "ugyanúgy működik és elérhető. Hasonlóképpen, a partíció replikája számára "MultiTypeServicePackge" aktiválásának **P3** szolgáltatás **fabric: / SpecialApp/ServiceB**, hello replikát üzemeltető "MyCodePackageB" és "MyCodePackageA" ugyanúgy működik, és és így tovább. Emiatt több hello száma *CodePackages* (regisztrálása különböző *ServiceTypes*) / *ServicePackage*, magasabb lesz redundáns Erőforrás kihasználtsága. 
 
 A hello ugyanakkor, ha a szolgáltatások létrehozhatunk **fabric: / SpecialApp/ServiceA** és **fabric: / SpecialApp/ServiceB** a **megosztott folyamat** modell, a Service Fabric lesz aktiválja a "MultiTypeServicePackge" csak egy példányát a *alkalmazás* **fabric: / SpecialApp** (a korábban látott). "MyCodePackageA" összes replika szolgáltatás fogja tárolni **fabric: / SpecialApp/ServiceA** (vagy bármely szolgáltatás típusa "MyServiceTypeA" toobe pontosabb) és "MyCodePackageB" összes replika szolgáltatás fogja tárolni **fabric: / SpecialApp/ServiceB** (vagy bármely szolgáltatás "MyServiceTypeB" toobe pontosabb típusú). Következő diagramon láthatók a hello csomópont megtekintése az ezt a beállítást: 

<center>
![Telepített alkalmazás csomópont ábrázolása][node-view-six]
</center>

A fenti példa hello, ha "MyCodePackageA" is "MyServiceTypeA" és "MyServiceTypeB" regisztrálja, és nem MyCodePackageB van, akkor nem lesznek nem redundáns érdemes *CodePackage* futtatása. Helyes, azonban amint azt korábban említettük, az alkalmazás modell topológia nem igazodik **kizárólagos folyamat** futtatási modell. Ha, tooput minden egyes replikához saját dedikált regisztrálja, mindkét folyamat *ServiceTypes* azonos *CodePackage* nincs rá szükség, és minden egyes üzembe *ServiceType* a a saját *ServicePacakge* több természetes döntés.

## <a name="next-steps"></a>Következő lépések
[Egy alkalmazás becsomagolása] [ a4] vele, és készen áll a toodeploy.

[Központi telepítése és távolíthat el alkalmazásokat] [ a5] ismerteti, hogyan toouse PowerShell toomanage alkalmazáspéldányok.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-deploy-existing-app.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage