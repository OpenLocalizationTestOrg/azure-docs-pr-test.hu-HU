---
title: "Azure Cloud Services alkalmazások toomicroservices aaaConvert |} Microsoft Docs"
description: "Ez az útmutató összehasonlítja a Cloud Services Web, és feldolgozói szerepkörök és a Service Fabric állapotmentes szolgáltatások toohelp Felhőszolgáltatások tooService háló át."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: c43b11623b2ba7f6069782a8b7e030c82572a6e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guide-tooconverting-web-and-worker-roles-tooservice-fabric-stateless-services"></a>Útmutató tooconverting webes és feldolgozói szerepkörök tooService háló állapotmentes szolgáltatások
Ez a cikk ismerteti, hogyan toomigrate a felhőalapú szolgáltatások webes és feldolgozói szerepkörök tooService háló állapotmentes szolgáltatásokhoz. Ez az hello legegyszerűbb áttelepítési út a Felhőszolgáltatások tooService háló, az alkalmazások, amelyek általános architektúrája nagyjából érintetlen toostay hello azonos.

## <a name="cloud-service-project-tooservice-fabric-application-project"></a>Cloud Service projekt tooService háló projekt
 Egy Felhőszolgáltatás-projektet és a Service Fabric-alkalmazás projekt rendelkezik egy hasonló szerkezet és mindkét jelentik hello telepítési egység az alkalmazás - Ez azt jelenti, hogy ezek határozzák meg, amely telepített toorun hello-teljes csomag az alkalmazás. Egy Felhőszolgáltatás-projekt tartalmazza a legalább egy webes vagy feldolgozói szerepköröket. Hasonlóképpen a Service Fabric-alkalmazás projekt tartalmaz egy vagy több szolgáltatás. 

hello különbség az, hogy a Felhőszolgáltatás-projekt párokat hello hello alkalmazás központi telepítése egy virtuális gép üzembe helyezéséhez, és így tartalmazza a virtuális gép konfigurációs beállításai, mivel a Service Fabric-alkalmazás projekt hello csak határozza meg a központilag telepített alkalmazás a Service Fabric-fürt a meglévő virtuális gépek tooa készlete. hello Service Fabric-fürt maga csak van telepítve, amennyiben a Resource Manager-sablon vagy hello Azure-portálon, és az alkalmazások lehetnek több Service Fabric telepített tooit.

![A Service Fabric és a Felhőszolgáltatások projekt összehasonlítása][3]

## <a name="worker-role-toostateless-service"></a>Feldolgozói szerepkör toostateless szolgáltatás
Elméleti szinten a feldolgozói szerepkör jelöli egy állapot nélküli alkalmazások és szolgáltatások, azaz hello munkaterhelés minden példánya azonos és a kérelmeket a irányított tooany példány bármikor lehet. Minden példány várt tooremember hello előző kérést nem. Állapot: hello munkaterhelés működik, a külső állapottárolóhoz, például az Azure Table Storage vagy Azure Document DB rendszerbe kezeli. A Service Fabric alkalmazások és szolgáltatások az ilyen típusú állapotmentes szolgáltatások helyőrző jelzi. hello legegyszerűbb módszer toomigrating a feldolgozói szerepkör tooService háló feldolgozói szerepkör kódját tooa állapotmentes szolgáltatások átalakításával végezhető.

![Feldolgozói szerepkör tooStateless szolgáltatás][4]

## <a name="web-role-toostateless-service"></a>Webes szerepkör toostateless szolgáltatás
Hasonló tooWorker szerepkör, a webes szerepkör is képviseli egy állapot nélküli alkalmazások és szolgáltatások, és így fogalmilag túl lehet csatlakoztatott tooa Service Fabric állapotmentes szolgáltatások. Azonban eltérően webes szerepkörök a Service Fabric nem támogatja az IIS. toomigrate egy webes szerepkör tooa állapotmentes szolgáltatások a webalkalmazás első áthelyezése tooa webes keretrendszer, amely önállóan üzemel, és nem függ az IIS vagy System.Web, például az ASP.NET Core 1 igényel.

| **Alkalmazás** | **Támogatott** | **Áttelepítési útvonal** |
| --- | --- | --- |
| Az ASP.NET Web Forms keretrendszerre |Nem |Átalakítás tooASP.NET alapvető 1 MVC |
| ASP.NET MVC |Az áttelepítés |Frissítési tooASP.NET alapvető 1 MVC |
| ASP.NET Web API |Az áttelepítés |Üzemeltetett önálló kiszolgáló vagy az ASP.NET Core 1 |
| Az ASP.NET Core 1 |Igen |N/A |

## <a name="entry-point-api-and-lifecycle"></a>Belépési pont API és életciklusa
Feldolgozói szerepkör és a Service Fabric szolgáltatás az API-k ajánlat hasonló belépési pontok: 

| **A belépési pont** | **Feldolgozói szerepkör** | **Service Fabric-szolgáltatás** |
| --- | --- | --- |
| Feldolgozása |`Run()` |`RunAsync()` |
| Virtuális gép indítása |`OnStart()` |N/A |
| Virtuális gép leállítása |`OnStop()` |N/A |
| Nyissa meg figyelő az ügyféli kérelmek részére |N/A |<ul><li> `CreateServiceInstanceListener()`az állapot nélküli</li><li>`CreateServiceReplicaListener()`az állapotalapú alkalmazások és szolgáltatások</li></ul> |

### <a name="worker-role"></a>Feldolgozói szerepkör
```C#

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a>Service Fabric állapotmentes szolgáltatások
```C#

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

A mely toobegin feldolgozása egy elsődleges "Futtatás" felülbírálása egyaránt rendelkezik. A Service Fabric szolgáltatások egyesítése `Run`, `Start`, és `Stop` egy-egy belépési pont, a `RunAsync`. A szolgáltatás működik, ha használatba `RunAsync` elindul, és le kell állnia, ha hello használata `RunAsync` metódus CancellationToken leállítási jelzést kapott. 

Hello életciklusának és feldolgozói szerepkörök és a Service Fabric szolgáltatás élettartama között számos fontos különbség van:

* **Életciklus:** hello legnagyobb különbség az, hogy a feldolgozói szerepkör egy virtuális Gépet és annak életciklusa kapcsolt toohello virtuális gép, amely amikor hello VM indítása és leállítása eseményeit tartalmazza. A Service Fabric-szolgáltatás egy életciklussal, amely nem hello VM életciklusát, ezért nem tartalmazza a eseményeket amikor hello gazdagép, VM vagy a gép elindulása és a Leállítás, mert nem áll(nak) kapcsolatban van.
* **Élettartam:** A feldolgozói szerepkör példánya indul, ha hello `Run` metódus kilép. Hello `RunAsync` metódus a Service Fabric-szolgáltatás azonban futtatható toocompletion és hello szolgáltatáspéldány fel maradnak. 

A Service Fabric egy választható kommunikációs telepítő belépési pontot nyújt az ügyfélkérelmek szolgáltatások. A hello RunAsync, és a kommunikációs belépési pont is választható felülbírálások szolgáltatásban a Service Fabric - a szolgáltatás előfordulhat, hogy válasszon tooonly figyelési tooclient kérelmeket, vagy csak futtassa egy feldolgozási ciklus, vagy mindkettőt - ezért hello RunAsync metódusában engedélyezett tooexit nélkül újraindítása hello szolgáltatáspéldány, mert az ügyféli kérelmek részére toolisten továbbra is.

## <a name="application-api-and-environment"></a>Alkalmazás API és a környezet
hello Felhőszolgáltatások környezet API információkat és a hello aktuális Virtuálisgép-példány és a többi VM szerepkörpéldányokat kapcsolatos információkat biztosít a. A Service Fabric tooits futásidejű vonatkozó információkat, és jelenleg fut egy szolgáltatás hello csomópont kapcsolatos információkat biztosít. 

| **Környezet feladat** | **Felhőszolgáltatások** | **Service Fabric** |
| --- | --- | --- |
| A konfigurációs beállítások és módosítási értesítés |`RoleEnvironment` |`CodePackageActivationContext` |
| Helyi tároló |`RoleEnvironment` |`CodePackageActivationContext` |
| Végpont |`RoleInstance` <ul><li>Jelenlegi példány:`RoleEnvironment.CurrentRoleInstance`</li><li>Egyéb szerepköröket és példány:`RoleEnvironment.Roles`</li> |<ul><li>`NodeContext`az aktuális csomópont-címe</li><li>`FabricClient`és `ServicePartitionResolver` a szolgáltatási végpont felderítése</li> |
| Az emuláció környezet |`RoleEnvironment.IsEmulated` |N/A |
| Egyidejű esemény |`RoleEnvironment` |N/A |

## <a name="configuration-settings"></a>Konfigurációs beállítások
Konfigurációs beállításai a Felhőszolgáltatások Virtuálisgép-szerepkör állítja be, és alkalmazni az adott virtuális gép szerepkör tooall példányok. Ezek a beállítások olyan ServiceConfiguration.*.cscfg fájlok beállított kulcs-érték párok, és segítségével érhető el közvetlenül roleenvironment-et. A Service Fabric beállítások érvényesek külön-külön tooeach szolgáltatás és alkalmazás tooeach ahelyett, hogy tooa virtuális gép, mert egy virtuális Gépet, rendelkezhet több szolgáltatásokat és alkalmazásokat. A szolgáltatás három csomagok áll:

* **Kód:** hello szolgáltatás végrehajtható fájlok, bináris fájljai, dll-EK és egyéb fájlokat tartalmaz egy szolgáltatásnak kell toorun.
* **A Config:** összes konfigurációs fájlokat és a szolgáltatás beállításait.
* **Adatok:** hello szolgáltatás társított statikus adatok fájlt.

Ezeket a csomagokat mindegyikének lehet függetlenül rendszerverzióval ellátott és frissített. Hasonló tooCloud szolgáltatások, a konfigurációs csomag programozott módon is elérhetők az API-n keresztül, és események elérhető toonotify hello szolgáltatást a konfigurációs csomag megváltoztatása. A Settings.xml fájlban kulcs-érték mind programozott hozzáférés hasonló toohello alkalmazás beállításai szakaszában az App.config fájl használható. Azonban eltérően Felhőszolgáltatásokat, a Service Fabric config csomag bármely konfigurációs fájlokat tartalmazza tetszőleges méretű XML, JSON, YAM vagy egyéni bináris formátumot. 

### <a name="accessing-configuration"></a>Konfigurációs elérése
#### <a name="cloud-services"></a>Cloud Services
Konfigurációs beállítások ServiceConfiguration.*.cscfg keresztül elérhető `RoleEnvironment`. Ezek a beállítások esetén világszerte elérhető tooall szerepkörpéldányt hello azonos Cloud Service-környezetben.

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Service Fabric
Minden szolgáltatás van a saját egyéni konfigurációs csomagot. Nincs olyan beépített mechanizmus globális konfigurációs beállítások érhető el a fürt valamennyi alkalmazás. A Service Fabric különleges Settings.xml konfigurációs fájl belül a konfigurációs csomag használata esetén a Settings.xml értékek írhatja felül hello alkalmazás szinten, amely lehetővé teszi az alkalmazás-konfigurációs beállítások.

Konfigurációs beállítások állnak belül minden szolgáltatáspéldány hello szolgáltatáson keresztül fér hozzá `CodePackageActivationContext`.

```C#

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a>A Configuration (update) események
#### <a name="cloud-services"></a>Cloud Services
Hello `RoleEnvironment.Changed` esemény használt toonotify összes szerepkörpéldányt, amikor változás következik be, hello környezetben, például a konfiguráció módosítása. Ez a használt tooconsume konfigurációfrissítések szerepkörpéldányokat újrahasznosítási, vagy indítsa újra a munkavégző folyamat nélkül.

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get hello list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a>Service Fabric
A csomag háromféle hello szolgáltatásban - kódot, konfiguráció és adatok - rendelkező események, amelyek tájékoztatást adnak a szolgáltatáspéldány, a csomag frissítése, hozzáadásakor vagy eltávolításakor. Egy szolgáltatás minden típusú több csomagot tartalmazhat. Előfordulhat például, hogy szolgáltatás több konfigurációs csomagot, minden egyes külön-külön rendszerverzióval ellátott és bővíthető. 

Ezek az események módosulnak elérhető tooconsume service csomagokban hello szolgáltatáspéldány újraindítása nélkül.

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Indítási feladatok
Indítási feladatokat is az alkalmazás indítása előtt végzett műveleteket. Egy indítási tevékenységhez általánosan használt toorun telepítési parancsfájlokat emelt szintű jogosultságokkal. Cloud Services és a Service Fabric támogatja a kezdeti feladatok. hello fő különbség az, hogy a Felhőszolgáltatás, indítási feladat a feltételekhez tooa virtuális gép, mert része egy szerepkörpéldányt, mivel a Service Fabric egy indítási feladat a feltételekhez tooa szolgáltatás, amely nem a feltételekhez tooany adott virtuális gép.

| Cloud Services | Service Fabric |
| --- | --- | --- |
| Konfiguráció helye |ServiceDefinition.csdef |
| Jogosultságok |"korlátozott" vagy "emelt szintű" |
| Alkalmazás-előkészítés |"egyszerű", "Háttér", "előtér" |

### <a name="cloud-services"></a>Cloud Services
Cloud Services egy indítási belépési pont úgy van konfigurálva, a ServiceDefinition.csdef szerepkör /. 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a>Service Fabric
A Service Fabric egy indítási belépési pont úgy van konfigurálva, a ServiceManifest.xml-szolgáltatás esetében:

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a>Fejlesztői környezet Megjegyzés
Cloud Services és a Service Fabric amelyek integrálhatók a Visual Studio projektsablonjai, és támogatja a hibakeresés, konfigurálása és telepítése mindkét helyileg és tooAzure. Cloud Services és a Service Fabric is adja meg a helyi futtatási környezet. hello különbség, hogy amíg hello fejlesztői futtatókörnyezet emulálja a felhőalapú szolgáltatás hello Azure környezetben az azt futtató, a Service Fabric nem használja az emulátor – hello teljes Service Fabric-futtatókörnyezet fogja használni. hello Service Fabric-környezet futtatja a helyi fejlesztési számítógépén hello ugyanabban a környezetben, amely a termelésben futtatja.

## <a name="next-steps"></a>Következő lépések
További információk a Service Fabric Reliable Services és a felhőalapú szolgáltatások és a Service Fabric application architektúra toounderstand hogyan tootake előnyeit teljes hello beállítása a Service Fabric funkciók alapvető különbségei hello.

* [Bevezetés a Service Fabric Reliable Services használatába](service-fabric-reliable-services-quick-start.md)
* [Cloud Services és a Service Fabric közötti fogalmi útmutató toohello különbségek](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
