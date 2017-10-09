---
title: "a Service Fabric-alkalmazás modell aaaAzure |} Microsoft Docs"
description: "Hogyan toomodel és az alkalmazások és szolgáltatások a Service Fabric ismertetik."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: 54c4d026e7d556be5f697d4a6f2ee886687e1c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="model-an-application-in-service-fabric"></a>A Service Fabric alkalmazás minta
Ez a cikk áttekintése hello Azure Service Fabric-alkalmazás modell, és hogyan toodefine egy alkalmazás és szolgáltatás keresztül manifest fájlt.

## <a name="understand-hello-application-model"></a>Hello alkalmazásmodell ismertetése
Egy alkalmazás egy bizonyos funkciója vagy funkciói végző alkotó szolgáltatások gyűjteménye. Egy szolgáltatás teljes és a különálló funkciót hajt végre és elindíthatja és egyéb szolgáltatások függetlenül futtatni.  A szolgáltatás kódot, a konfiguráció és az adatok tevődik össze. Minden egyes szolgáltatás kód áll hello végrehajtható bináris fájlok konfigurálása a futási időben betölthető Szolgáltatásbeállítások áll és hello szolgáltatás által használt tetszőleges statikus adatok toobe tartalmaz. A hierarchikus alkalmazás modellben minden egyes összetevő csak rendszerverzióval ellátott és frissített egymástól függetlenül.

![hello Service Fabric alkalmazásmodellt.][appmodel-diagram]

Alkalmazástípust egy kategorizálási kérelem és a csomag egyik gyermekszoftver található szolgáltatástípusok áll. A szolgáltatás típus egy kategorizálási szolgáltatás. hello kategorizálási különböző beállítások és konfigurációk rendelkezhet, de hello core funkció marad hello azonos. hello szolgáltatás példányai hello különböző konfigurációs eltéréseket hello az azonos szolgáltatás típusa.  

Osztályok (vagy "típusú") az alkalmazások és szolgáltatások leírása XML-fájlok (alkalmazás és szolgáltatás jegyzékfájlokban) keresztül.  hello jegyzékfájlokat hello sablonok, amely alkalmazások példányosítható hello fürt kép áruházból. hello hello ServiceManifest.xml és ApplicationManifest.xml fájl sémadefiníciót hello Service Fabric SDK telepítve van és eszközöket túl*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

a Futtatás másként különálló folyamat akkor is, ha a futó hello azonos a Service Fabric-csomópont különböző alkalmazáspéldányok hello kódját. Továbbá minden alkalmazáspéldány hello életciklusát kezelhető (például frissítése) egymástól függetlenül. hello alábbi ábra bemutatja hogyan alkalmazástípusok szolgáltatástípusok, viszont álló kódot, a konfiguráció és adatok csomagok állnak. toosimplify hello diagram, csak hello kódot/config/adatok csomagjai `ServiceType4` láthatók, bár egyes szolgáltatástípusokról tartalmazhat néhány vagy minden csomagtípus.

![A Service Fabric alkalmazás és szolgáltatás típusainak][cluster-imagestore-apptypes]

Két különböző fájlok használt toodescribe alkalmazások és szolgáltatások: hello szolgáltatás jegyzékfájl és az alkalmazásjegyzék. A következő részekben hello részletes leírása az jegyzékfájljai.

Egy vagy több példányát egy típusú szolgáltatás lehet hello fürt aktív. Például állapot-nyilvántartó szolgáltatáspéldány, vagy a replikák elérése magas megbízhatóság állapot replikálja a replikák található hello fürt csomópontjai között. Replikációs lényegében biztosít redundanciát hello szolgáltatás toobe érhető el a, még akkor is, ha a fürt egyik csomópontjáról sikertelen. A [szolgáltatás particionálva](service-fabric-concepts-partitioning.md) további felosztása a állapota (és a hozzáférési minták toothat állapot) hello fürt csomópontjai között.

hello alábbi ábrán látható alkalmazások és a szolgáltatáspéldány, a partíciók és a replikák hello kapcsolatát.

![Partíciók és replikáit a szolgáltatáson belül][cluster-application-instances]

> [!TIP]
> Alkalmazások hello elrendezésének megtekintheti a http:// hello Service Fabric Explorer eszközt érhető el a fürt&lt;yourclusteraddress&gt;: 19080/Explorer. További információkért lásd: [a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>A szolgáltatás leírása
hello szolgáltatás deklarációval jegyzékfájl hello szolgáltatás típusától és verziójától. Azt adja meg a szolgáltatás metaadatokat, például szolgáltatás típusa, a rendszerállapot-tulajdonságai, a terheléselosztás metrikák, a bináris fájljait és a konfigurációs fájlok.  Másképp fogalmazva, ismerteti, hogyan hello kódot, a konfiguráció és az adatok csomagok a service-csomag toosupport alkotó egy vagy több szolgáltatás típust. Íme egy egyszerű példa service jegyzékfájl:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

**Verzió** attribútumok strukturálatlan karakterláncok és hello rendszer nem elemzi. Verzió attribútumok vannak használt tooversion minden összetevő frissítésre.

**ServiceTypes** kijelenti, hogy milyen típusú szolgáltatásokat által támogatott **CodePackages** a jegyzékfájlban. Amikor egy szolgáltatás létrejön az említett szolgáltatás ellen, a jegyzékfájlban deklarált összes kódot csomag aktívak a belépési pontok futtatásával. hello eredményül kapott folyamatok várt tooregister hello támogatott szolgáltatástípusok futási időben. Szolgáltatástípusok hello jegyzék szint és a nem az hello kód csomag szintjén deklarált. Ezért ha több kód csomagot, az összes aktiválás amikor hello rendszer keresi szolgáltatástípusok deklarált hello egyikét sem.

**SetupEntryPoint** egy jogosultsági szintű belépési pontja, amelynek ugyanaz, mint a Service Fabric hitelesítő adatok hello (általában hello *LocalSystem* fiók) más belépési pont előtt. hello végrehajtható fájl megadott **EntryPoint** általában hello hosszan futó szolgáltatás gazdagép legyen. egy külön telepítő belépési pont hello jelenléte elkerülhető toorun hello szolgáltatásgazda magas jogosultsággal rendelkező huzamosabb ideig. hello végrehajtható fájl megadott **EntryPoint** futtatása **SetupEntryPoint** sikeresen kilép. Ha bármikor hello folyamat leáll, vagy összeomlik, hello eredményül kapott folyamat ellenőrzött és újraindítása (újra kezdve **SetupEntryPoint**).  

A jellemző forgatókönyvek **SetupEntryPoint** Biztosan futtatni egy végrehajtható fájlt hello szolgáltatás indítása előtt, vagy egy emelt szintű jogosultságokkal műveletét úgy végezheti el. Példa:

* Beállítását, valamint inicializálása környezeti változókat, amelyek hello végrehajtható van szüksége. Ez nem korlátozott tooonly végrehajtható fájlok keresztül hello Service Fabric programozási modellek írása. Például npm.exe kell néhány környezetiblokk-változót, egy node.js-alkalmazás telepítéséhez konfigurált.
* Hozzáférés-vezérlés beállítása biztonsági tanúsítványok telepítésével.

További részletes információt a tooconfigure hello **SetupEntryPoint** lásd: [egy szolgáltatás telepítője a belépési pont hello házirend konfigurálása](service-fabric-application-runas-security.md)

**Változók** környezeti változókat, amelyek be vannak állítva a kódcsomag listáját tartalmazza. Environmentment változók felülbírálhatók hello `ApplicationManifest.xml` különböző szolgáltatáspéldány tooprovide eltérő értékeket. 

**DataPackage** deklarál egy által hello nevű mappát **neve** futási időben hello folyamat által használt tetszőleges statikus adatok toobe tartalmazó attribútum.

**ConfigPackage** deklarál egy által hello nevű mappát **neve** attribútum, amely tartalmaz egy *Settings.xml* fájlt. hello beállításfájl a felhasználó által definiált, a kulcs-érték pár beállítások hello folyamat futási időben vissza olvasó részeket tartalmazza. Ha csak frissítés során hello **ConfigPackage** **verzió** módosult, akkor hello folyamat fut nem indítják újra. Ehelyett egy visszahívási értesíti hello folyamat, amely a konfigurációs beállításai módosultak, így azok dinamikusan kell tölteni. Példa *Settings.xml* fájlt:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> A szolgáltatás jegyzék több kódot, konfiguráció és adatok csomagok tartalmazhatnak. Egyes lehetnek rendszerverzióval ellátott egymástól függetlenül.
> 
> 

<!--
For more information about other features supported by service manifests, refer toohello following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Az alkalmazás leírása
hello alkalmazásjegyzék deklarációval ismerteti hello alkalmazástípus és -verzió. Azt adja meg a szolgáltatás összeállítás metaadatai köztük, példányok száma vagy replikációs tényező, biztonsági/elkülönítési házirenddel, placement Constraints korlátozásokat, konfigurációs felülbírálások, és a bennük foglalt szolgáltatástípusok stabil nevét. hello terheléselosztás tartományok amelybe hello alkalmazás el van helyezve is ismerteti.

Ebből kifolyólag alkalmazásjegyzéket hello alkalmazás szinten elemeket ismerteti és hivatkozik egy vagy több szolgáltatás jelentkezik toocompose alkalmazástípust. Íme egy egyszerű példa alkalmazásjegyzék:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

Szolgáltatás jegyzékfájlban, például **verzió** attribútumok strukturálatlan karakterláncok, és nem elemzésének hello rendszer. Verzió attribútumok minden összetevő frissítésre használt tooversion is vannak.

**ServiceManifestImport** hivatkozások tooservice jegyzékfájlokban, ez az alkalmazástípus alkotó tartalmazza. Importált service jegyzékfájlokban meghatározására, hogy milyen típusú szolgáltatásokat érvényes ez az alkalmazástípus belül. Belül hello ServiceManifestImport bírálja felül konfigurációs ServiceManifest.xml fájlok Settings.xml és környezeti változók értékeit. 


**DefaultServices** szolgáltatáspéldány, amikor egy alkalmazás létrejön az alkalmazás típusa alapján automatikusan létrehozott deklarál. Alapértelmezett szolgáltatások csupán a könnyebb elérhetőség érdekében, és minden tekintetben normál szolgáltatások viselkednek, azok létrehozását követően. Azok a hello alkalmazáspéldány bármely más szolgáltatásokkal együtt verzióra frissíti, és is távolíthatók el.

> [!NOTE]
> Az alkalmazás jegyzékében több szolgáltatás nyilvánvaló imports és alapértelmezett szolgáltatások tartalmazhat. Minden egyes importált jegyzékből szolgáltatás egymástól függetlenül lehet rendszerverzióval ellátott.
> 
> 

Hogyan toomaintain különböző alkalmazás és szolgáltatás paraméterei egyes környezetekben: toolearn [alkalmazás paramétereinek több környezet kezelése](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer toohello following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Következő lépések
[Egy alkalmazás becsomagolása](service-fabric-package-apps.md) , és teszi toodeploy kész.

[Központi telepítése és távolíthat el alkalmazásokat] [ 10] ismerteti, hogyan toouse PowerShell toomanage alkalmazáspéldányok.

[Alkalmazás paramétereinek több környezet kezelése] [ 11] ismerteti, hogyan tooconfigure paraméterek és a környezeti változók különböző alkalmazás-példányok.

[Az alkalmazás biztonsági szabályzatainak konfigurálásához] [ 12] ismerteti, hogyan toorun a biztonsági házirendek toorestrict hozzáférés szolgáltatásokat.

[Modellek futtató alkalmazás] [ 13] írják le a replikák (vagy példányok) telepített szolgáltatások és folyamata közötti kapcsolat.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
