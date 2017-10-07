---
title: "aaaManage több környezeteknek a Service Fabric |} Microsoft Docs"
description: "Service Fabric-alkalmazások futtatható fürtökben, amelyek mérete a egy gép toothousands gépek között. Bizonyos esetekben érdemes tooconfigure eltérően az alkalmazását ezen változatos környezetekben. Ez a cikk ismerteti hogyan toodefine különböző alkalmazás paraméterei / környezetben."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 2b3327e0e1a3bbd35a50835e720619f308b1b501
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a>Alkalmazás paramétereinek több környezet kezelése
Azure Service Fabric-fürtök használatával bárhol egy toomany több ezer gépek hozhat létre. Bináris alkalmazásfájlokat módosítás nélkül futtathatja a széles skáláját környezetek között, miközben gyakran szeretné tooconfigure hello alkalmazás másképp, attól függően, hogy a gépek való telepítése esetén hello száma.

Egy egyszerű példa, fontolja meg a `InstanceCount` az állapotmentes szolgáltatások. Amikor alkalmazásokat futtat az Azure-ban, általában kívánt tooset a toohello különleges paraméterérték a "-"1. Ez a konfiguráció biztosítja, hogy fut-e a szolgáltatás minden egyes csomópontjára hello fürt (vagy minden csomópont hello csomóponttípus, ha egy elhelyezési korlátozás van beállítva). Azonban ez a konfiguráció nem alkalmas egyszámítógépes fürt hello figyel azonos több folyamat nem tartozik végpont egy gépen. Ehelyett általában beállított `InstanceCount` túl "1".

## <a name="specifying-environment-specific-parameters"></a>Környezetfüggő paramétereinek megadása
hello megoldás toothis konfigurációs probléma a paraméteres alapértelmezett szolgáltatások és alkalmazásparaméter-fájlokat az adott értékei az adott környezetben. Alapértelmezett szolgáltatások és alkalmazás paraméterei hello alkalmazásban és a szolgáltatás a jegyzékfájlban. hello sémadefiníciót hello ServiceManifest.xml és ApplicationManifest.xml fájlok hello Service Fabric SDK telepítve van és eszközöket túl*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Alapértelmezett szolgáltatások
Service Fabric-alkalmazások szolgáltatáspéldány gyűjteménye épülnek fel. Toocreate üres alkalmazás lehet, és hozzon létre minden szolgáltatáspéldány dinamikusan, a legtöbb alkalmazás rendelkezik alapvető szolgáltatások mindig jöjjenek létre, amikor hello alkalmazás létrejön. Ezek a hivatkozott tooas "alapértelmezett szolgáltatások". Hello alkalmazásjegyzék szögletes zárójelek között szerepel környezeti konfiguráció helyőrzőkkel vannak megadva:

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

Egyes paraméterek nevű hello hello paraméterek elem hello az alkalmazás jegyzékének belül kell megadni:

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

hello DefaultValue attribútumot hello érték toobe egy adott környezetben használt hello hiányában egy több-specifikus paraméter határozza meg.

> [!NOTE]
> Nem minden szolgáltatás példány paraméterei megfelelő környezet konfigurációhoz. Hello a fenti példában a hello LowKey és hello szolgáltatás particionálási sémát HighKey értékeinek explicit módon definiálva hello szolgáltatás minden példányának mivel hello partíciótartomány hello adatok tartomány nem hello környezet függvényében.
> 
> 

### <a name="per-environment-service-configuration-settings"></a>Környezet szolgáltatás konfigurációs beállításai
Hello [Service Fabric-alkalmazás modell](service-fabric-application-model.md) lehetővé teszi, hogy szolgáltatások futási időben olvasható egyéni kulcs-érték párokat tartalmazó tooinclude konfigurációs csomagokat. Ezek a beállítások értékeit hello is is lehet szerint megkülönböztetett környezet megadásával egy `ConfigOverride` hello alkalmazásjegyzékben.

Tegyük fel, hogy rendelkezik-e a következő fájlban hello Config\Settings.xml hello beállítás hello `Stateful1` szolgáltatás:

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
toooverride ezt az értéket egy adott alkalmazás-környezet párhoz, hozzon létre egy `ConfigOverride` hello szolgáltatás jegyzékfájl hello alkalmazásjegyzékben importálásakor.

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
Ennek a paraméternek, majd konfigurálhatja környezet a fentiek szerint. Ehhez deklaráló azt hello paraméterek szakaszban hello az alkalmazás jegyzékének és környezetfüggő értékek megadása hello alkalmazásparaméter-fájlokat.

> [!NOTE]
> A szolgáltatás konfigurációs beállításainak esetet hello, nincsenek három helyen, ahol a kulcs értékének hello állítható be: hello szolgáltatás konfigurációs csomagot, hello alkalmazásjegyzék és hello alkalmazás paraméterfájl. A Service Fabric fog mindig válasszon hello alkalmazás paraméterfájl először (ha az meg van adva), majd az alkalmazásjegyzék hello, és végül a konfigurációs csomag hello.
> 
> 

### <a name="setting-and-using-environment-variables"></a>És környezeti változók használatához 
Adja meg és állítsa be a környezeti változók hello ServiceManifest.xml fájlban, és ezt felülbírálhatja hello ApplicationManifest.xml fájlba / példány alapon.
hello az alábbi példában két környezeti változókat, egy-egy értéket állítsa be, és más hello felülbírálja. Használhatja az alkalmazás paraméterei tooset környezeti változók értékei hello azonos módon, hogy azokat config felülbírálások használták.

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
toooverride hello környezeti változók hello ApplicationManifest.xml, hivatkozás hello kódcsomag hello ServiceManifest a hello `EnvironmentOverrides` elemet.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 Hello nevű szolgáltatáspéldány létrehozása után a hello környezeti változók kódból végezheti el. például a C# teheti hello következő

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a>A Service Fabric környezeti változók
A Service Fabric környezeti változók beállítása minden szolgáltatáspéldány létrehozta. hello környezeti változók teljes listáját van az alábbiakban, ahol hello azokat, félkövérrel szedett is hello azokon, szüksége lesz a szolgáltatás hello más alatt Service Fabric-futtatókörnyezet által használt. 

* Fabric_ApplicationHostId
* Fabric_ApplicationHostType
* Fabric_ApplicationId
* **Fabric_ApplicationName**
* Fabric_CodePackageInstanceId
* **Fabric_CodePackageName**
* **[YourServiceName] Fabric_Endpoint_ TypeEndpoint**
* **Fabric_Folder_App_Log**
* **Fabric_Folder_App_Temp**
* **Fabric_Folder_App_Work**
* **Fabric_Folder_Application**
* Fabric_NodeId
* **Fabric_NodeIPOrFQDN**
* **Fabric_NodeName**
* Fabric_RuntimeConnectionAddress
* Fabric_ServicePackageInstanceId
* Fabric_ServicePackageName
* Fabric_ServicePackageVersionInstance
* FabricPackageFileName

hello kód belows bemutatja, hogyan toolist hello Service Fabric környezeti változók
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
hello következő példákban a környezeti változók nevű alkalmazás típusú `GuestExe.Application` egy szolgáltatás típusának neve `FrontEndService` futása közben a helyi fejlesztési számítógépén.

* **Fabric_ApplicationName = fabric:/GuestExe.Application**
* **Fabric_CodePackageName kód =**
* **Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**
* **Fabric_NodeIPOrFQDN = localhost**
* **Fabric_NodeName = _Node_2**

### <a name="application-parameter-files"></a>Alkalmazásparaméter-fájlokat
a Service Fabric-alkalmazás projekt hello tartalmazhat egy vagy több alkalmazásparaméter-fájlokat. Azok meghatározza, hogy hello adott hello alkalmazás jegyzékében definiált hello paraméterek értékeit:

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
Új alkalmazás alapértelmezés szerint három alkalmazásparaméter-fájlokat, Local.1Node.xml, Local.5Node.xml és Cloud.xml nevű foglalja magában:

![A Solution Explorer alkalmazásparaméter-fájlokat][app-parameters-solution-explorer]

a paraméterfájl toocreate egyszerűen másolja és illessze be egy meglévő, és adjon neki egy új nevet.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Azonosító környezetfüggő paraméterek központi telepítése során
A központi telepítéskor a szükséges toochoose hello megfelelő paraméter fájl tooapply az alkalmazást. Ehhez a Visual Studio hello közzététel párbeszédpaneléről vagy a Powershellen keresztül.

### <a name="deploy-from-visual-studio"></a>Üzembe helyezés a Visual Studióból
Az alkalmazást a Visual Studio közzétételekor elérhető paraméter fájlok hello listája közül választhatnak.

![Válassza ki a paraméterfájl hello közzététele párbeszédpanelen][publishdialog]

### <a name="deploy-from-powershell"></a>A PowerShell telepítése
Hello `Deploy-FabricApplication.ps1` hello alkalmazás projektsablon szereplő PowerShell-parancsfájl fogad paraméterként közzétételi profilt, és hello PublishProfile toohello alkalmazás referencia paraméterek-fájlt tartalmaz.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Következő lépések
több azzal kapcsolatban, ebben a témakörben tárgyalt hello alapfogalmak toolearn lásd: hello [Service Fabric a műszaki áttekintés](service-fabric-technical-overview.md). Más elérhető a Visual Studio alkalmazás-felügyeleti képességekkel kapcsolatos információkért lásd: [kezelése a Service Fabric-alkalmazások, a Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
