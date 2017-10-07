---
title: "a Service Fabric-alkalmazás központi telepítésének aaaAzure |} Microsoft Docs"
description: "Hello FabricClient API-k toodeploy használja, és távolítsa el az alkalmazások a Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/07/2017
ms.author: ryanwi
ms.openlocfilehash: b2986b71c461f3e785ba16ec1b827fe47ad852fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a>Központi telepítése és eltávolítása a FabricClient használó alkalmazások
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient API-k](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Egyszer egy [alkalmazástípus csomagolás][10], az Azure Service Fabric-fürt szolgáltatássablonjaikat használatra kész. Központi telepítés hello a következő három lépést foglal magában:

1. Hello alkalmazás csomag toohello lemezképtárolóhoz feltöltése
2. Hello alkalmazástípus regisztrálása
3. Hello alkalmazáspéldány létrehozása

Miután egy alkalmazás lett telepítve, és a példány hello fürtben fut, törölheti a hello alkalmazáspéldány és az alkalmazás típusa. toocompletely eltávolítása egy alkalmazáskészletet hello fürtből hello lépéseket foglalja magában:

1. Alkalmazás-példányt futtató hello eltávolítása (vagy törlése)
2. Ha már nincs szüksége a hello alkalmazástípus regisztrációjának törlése
3. Hello lemezképtárolóhoz hello alkalmazáscsomag eltávolítása

Ha [központi telepítéséhez és alkalmazások hibakeresése a Visual Studio](service-fabric-publish-app-remote-cluster.md) a helyi fejlesztési fürtöt, a fenti lépéseket minden hello egy PowerShell-parancsfájl segítségével automatikusan kezeli.  Ez a parancsfájl található hello *parancsfájlok* hello projekt mappájából. Ez a cikk ismerteti a háttérben, hogy a parancsfájl tevékenységétől, hogy végezheti el a Visual Studio kívül ugyanazokat a műveleteket hello. 
 
## <a name="connect-toohello-cluster"></a>Csatlakoztassa toohello fürtöt
Csatlakoztassa a toohello fürtöt hozzon létre egy [FabricClient](/dotnet/api/system.fabric.fabricclient) példány futtatása előtt minden hello kódpéldák ebben a cikkben. Példák a kapcsolódó tooa helyi fejlesztési fürtök vagy egy távoli fürtöt vagy az Azure Active Directoryval, X509 védett fürt tanúsítványokat, vagy a Windows Active Directory [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis). tooconnect toohello helyi fejlesztési fürtöt, futtassa a következő hello:

```csharp
// Connect toohello local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-hello-application-package"></a>Hello alkalmazáscsomag feltöltése
Tegyük fel, hogy most felépíti és nevű csomag *MyApplication* a Visual Studióban. Alapértelmezés szerint a felsorolt hello ApplicationManifest.xml hello alkalmazástípus neve "MyApplicationType".  hello alkalmazáscsomagot, amely tartalmazza a hello szükséges alkalmazás jegyzékfájlja, a szolgáltatás jegyzékfájlban és a kód/config/adatok csomagok, található *C:\Users\&lt; felhasználónév&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.

Hello alkalmazáscsomag feltöltése hello belső Service Fabric összetevői által elérhető helyen helyezi. A Service Fabric hello alkalmazáscsomag hello alkalmazáscsomag hello regisztrálás során ellenőrzi. Azonban ha tooverify hello alkalmazáscsomagot helyileg (azaz feltöltés előtt), használható hello [teszt-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) parancsmag.

Hello [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API feltölt hello alkalmazás csomag toohello fürt lemezképtárolóhoz. 

Ha hello alkalmazáscsomag nagy és/vagy sok fájl van, akkor [a tömörítés](service-fabric-package-apps.md#compress-a-package) és a PowerShell használatával toohello lemezképtárolóhoz másolásához. hello tömörítés csökkenti a hello mérete és a fájlok hello száma.

Lásd: [hello lemezkép tárolási kapcsolati karakterlánc megértéséhez](service-fabric-image-store-connection-string.md) kiegészítő információkat megtudni hello image store és lemezkép tárolásához kapcsolati karakterláncot.

## <a name="register-hello-application-package"></a>Hello alkalmazáscsomag regisztrálása
regisztrálva van-e hello alkalmazáscsomag válnak használható hello alkalmazás jegyzékfájlban deklarált hello alkalmazástípus és -verzió. hello rendszer beolvassa az előző lépésben hello feltöltött hello csomag, hello csomag ellenőrzi, hello csomag tartalmának folyamatokat, és másolja át a feldolgozott hello csomagok tooan belső rendszer helyét.  

Hello [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API regiszterekben hello alkalmazástípus hello fürt, és tegye elérhetővé a központi telepítéshez.

Hello [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API sikeresen regisztrált alkalmazástípusokat ismerteti. Az API toodetermine is használhatja, amikor hello regisztrációs hajtja végre.

## <a name="create-an-application-instance"></a>Hozzon létre egy alkalmazáspéldányt
Az alkalmazás regisztrálása sikeresen befejeződött hello segítségével alkalmazás típussal a példányosítható [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API. minden alkalmazás hello neve kezdődhet hello *"fabric:"* sémáját, és minden alkalmazáspéldányhoz (fürtözött) egyedinek kell lennie. Hello célalkalmazás típusa hello alkalmazás jegyzékében definiált alapértelmezett szolgáltatások is jönnek létre.

Több alkalmazáspéldányt is létrehozható egy regisztrált alkalmazástípus bármely adott verzióját. Minden egyes alkalmazáspéldány feladata elkülönítve, a saját munkakönyvtár és a folyamatok.

toosee, amely nevű alkalmazások és szolgáltatások futnak hello fürthöz, futtassa a hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) és [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API-k.

## <a name="create-a-service-instance"></a>A szolgáltatáspéldány létrehozása
A szolgáltatás használatával hello szolgáltatás típusból példányosítható [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.  Ha hello szolgáltatás alapértelmezett szolgáltatásként hello alkalmazásjegyzékben van deklarálva, hello szolgáltatás létrejön, amikor hello alkalmazás létrejön.  Hívása hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API-t a már létrehozott egy szolgáltatást fog visszaadni a FabricErrorCode.ServiceAlreadyExists értékű hiba kódot tartalmazó FabricException típusú kivételt.

## <a name="remove-a-service-instance"></a>A szolgáltatás példányának eltávolítása
Amikor egy szolgáltatáspéldány már nem szükséges, eltávolíthatja azt hívó hello alkalmazáspéldány futó hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.  

> [!WARNING]
> Ez a művelet nem vonható vissza, és a szolgáltatás állapota nem állítható helyre.

## <a name="remove-an-application-instance"></a>Az alkalmazáspéldány eltávolítása
Az alkalmazáspéldány nincs szükség, ha véglegesen eltávolíthatja azt hello használatával név szerint [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API. [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automatikusan eltávolítja az is, végleges eltávolítása az összes szolgáltatás állapotának toohello alkalmazáshoz tartozó összes szolgáltatás.

> [!WARNING]
> Ez a művelet nem vonható vissza, és az alkalmazás állapota nem állítható helyre.

## <a name="unregister-an-application-type"></a>Az alkalmazástípus regisztrációjának törlése
Amikor egy alkalmazás típus adott verziójának már nem szükséges, hello alkalmazástípus hello segítségével, hogy adott verziójának kell regisztrációjának törlése [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API. Alkalmazás típusú kiadásokban tárolóhely hello lemezképtárolóhoz által használt regisztrációjának megszüntetése nem használt verziók. Egy alkalmazástípus verziója mindaddig, amíg nincs alkalmazások létrehozásának ellen, hogy hello alkalmazástípus verziója, és nincs függőben lévő alkalmazásfrissítések hivatkoznak, hogy hello alkalmazástípus verziója lehet regisztrációjának törlése.

## <a name="remove-an-application-package-from-hello-image-store"></a>Az alkalmazáscsomag eltávolítása hello lemezképtárolóhoz
Már nem szükséges egy alkalmazáscsomagot, törölheti azt hello lemezképet tároló toofree rendszer erőforrásait hello segítségével a [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Másolás-ServiceFabricApplicationPackage kér egy előtaggal
hello Service Fabric SDK környezet már rendelkezik hello megfelelő alapértelmezett beállítása. De szükség esetén – hello előtaggal összes parancs meg kell felelnie hello értéket, hogy a Service Fabric fürt által használt hello. Hello előtaggal hello fürtjegyzékben található beolvasott hello segítségével [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) és Get-ImageStoreConnectionStringFromClusterManifest parancsokat:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

Hello **Get-ImageStoreConnectionStringFromClusterManifest** hello Service Fabric SDK PowerShell modul részét képező parancsmag értéke használt tooget hello image kapcsolati karakterláncot tárolni.  tooimport hello SDK modul, futtassa:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


hello előtaggal megtalálható a fürtjegyzékben hello:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Lásd: [hello lemezkép tárolási kapcsolati karakterlánc megértéséhez](service-fabric-image-store-connection-string.md) kiegészítő információkat megtudni hello image store és lemezkép tárolásához kapcsolati karakterláncot.

### <a name="deploy-large-application-package"></a>Nagy alkalmazáscsomag telepítése
Probléma: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) a nagy alkalmazáscsomagok (GB-os sorrendben) időtúllépése API.
Próbálja meg:
- Adjon meg egy nagyobb időkorlátjának [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) metódus, a `timeout` paraméter. Alapértelmezés szerint hello időtúllépés értéke 30 perc.
- Ellenőrizze a hello hálózati kapcsolat a forrásgép és fürt között. Ha hello kapcsolat lassú, érdemes lehet a gépek nagyobb hálózati kapcsolattal rendelkező.
Ha hello ügyfélszámítógép mint hello fürt egy másik régióban van, érdemes lehet egy ügyfélszámítógépre egy szorosabb vagy ugyanabban a régióban hello fürtként.
- Ellenőrizze, hogy ha hogy elérte-e külső szabályozás. Például ha hello lemezképtárolóhoz konfigurált toouse azure tárolási, feltöltés előfordulhat, hogy szabályozva.

Probléma: Feltöltés csomag sikeresen befejeződött, de [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API túllépi az időkorlátot. Próbálja meg:
- [Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolása előtt.
hello tömörítés csökkenti hello és hello fájlok száma, ami viszont hello forgalom mennyiségét csökkenti, és működnek, hogy a Service Fabric kell elvégezni. lehet, hogy hello feltöltési művelet lassabb (különösen ha hello tömörítési idő), de regisztrálása és regisztrációjának hello alkalmazástípus gyorsabb.
- Adjon meg egy nagyobb időkorlátjának [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API-t `timeout` paraméter.

### <a name="deploy-application-package-with-many-files"></a>Sok fájlokkal alkalmazáscsomag telepítése
Probléma: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) sok fájlt (akár több ezer sorrendben) az alkalmazás csomagot időtúllépése.
Próbálja meg:
- [Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolása előtt. hello tömörítés csökkenti a fájlok hello száma.
- Adjon meg egy nagyobb időkorlátjának [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) a `timeout` paraméter.

## <a name="code-example"></a>Mintakód
hello következő például egy alkalmazás csomag toohello lemezképtárolóhoz másolja hello alkalmazástípus kiépítését, létrehoz egy alkalmazáspéldányt, hoz létre a szolgáltatáspéldány, eltávolítja hello alkalmazáspéldányt, un-rendelkezések hello alkalmazástípust, és hello alkalmazáscsomag törlése hello lemezképtárolóhoz.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2017\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect toohello cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy hello application package tooa location in hello image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied too{0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy tooImage Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision hello application.  "MyApplicationV1" is hello folder in hello image store where hello application package is located. 
    // hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) 
    // is now registered in hello cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create hello application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create hello stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create hello service instance.  If hello service is declared as a default service in hello ApplicationManifest.xml,
    // hello service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from hello application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision hello application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete hello application package from a location in hello image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a>Következő lépések
[A Service Fabric-alkalmazás frissítése](service-fabric-application-upgrade.md)

[A Service Fabric állapotának bemutatása](service-fabric-health-introduction.md)

[Diagnosztizálása és megoldása a Service Fabric-szolgáltatás](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[A Service Fabric alkalmazás minta](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
