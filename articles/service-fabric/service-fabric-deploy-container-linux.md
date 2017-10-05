---
title: "Service Fabric és központi telepítése Linux a tárolók |} Microsoft Docs"
description: "A Service Fabric és a Linux-tárolók használatát mikroszolgáltatási alkalmazások központi telepítése. Ez a cikk ismerteti, amely tárolók biztosít a Service Fabric képességeit és a Linux-tároló lemezkép központi telepítése a fürtbe"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4ba99103-6064-429d-ba17-82861b6ddb11
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: msfussell
ms.openlocfilehash: 9dcec753e5f999a1bac07276373c0c25f89ec58d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-linux-container-to-service-fabric"></a>A Service Fabric központi telepítése egy Linux-tároló
> [!div class="op_single_selector"]
> * [Windows-tároló üzembe](service-fabric-deploy-container.md)
> * [Linux-tároló üzembe](service-fabric-deploy-container-linux.md)
>
>

Ez a cikk bemutatja, hogyan indexelése szolgáltatások Docker-tárolókban lévő Linux építve.

A Service Fabric különböző tároló képességeket, amelyek segítenek a mikroszolgáltatások létrehozására, amelyek indexelése épülnek alkalmazások építéséhez rendelkezik. Ezek a szolgáltatások indexelése szolgáltatások nevezzük.

A lehetőségek tartalmazzák;

* Tároló lemezkép-telepítés és az aktiválás
* Erőforrás-irányítás
* Tárház-hitelesítés
* A gazdagép porthozzárendelés tárolóportot
* Tároló-tároló felderítése és kommunikáció
* Konfigurálása, és állítsa be a környezeti változók

## <a name="packaging-a-docker-container-with-yeoman"></a>Egy docker-tároló csomagolására rendelkező yeoman
Egy tároló Linux-csomagban, amikor Ön választhatja yeoman sablon használata vagy [hozzon létre manuálisan az alkalmazáscsomag](#manually).

A Service Fabric-alkalmazás tartalmazhat egy vagy több tárolóban, az egy adott szerepkör a postai az alkalmazás működését. A Linux Service Fabric SDK tartalmaz egy [Yeoman](http://yeoman.io/)-generátort, amely megkönnyíti az alkalmazás létrehozását és egy tárolórendszerkép hozzáadását. Hozzunk létre egy alkalmazást, amely egyetlen, *SimpleContainerApp* nevű Docker-tárolóval rendelkezik. Hozzáadhat további szolgáltatások később a létrehozott szerkesztésével manifest fájlt.

## <a name="install-docker-on-your-development-box"></a>A fejlesztési mezőben Docker telepítése

Docker telepítését a Linux-fejlesztési mezőben a következő parancsokat (a vagrant lemezkép használatakor az os x docker már telepítve van):

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-the-application"></a>Az alkalmazás létrehozása
1. Írja be a terminálba a következőt: `yo azuresfcontainer`.
2. Az alkalmazás - például mycontainerap neve
3. Adja meg az URL-címet a tároló lemezképének egy DockerHub tárházból. A kép paraméter [tárház] formájában történik [kép neve]
4. Ha a kép nem rendelkezik a munkaterhelés belépési ponttal definiált, explicit módon adja meg a parancsok futtatásához a tároló, amely akkor is megtartja a tároló indítás után fut belül vesszővel tagolt számú bemeneti parancsokat kell használnia.

![Tárolókhoz készült Service Fabric Yeoman-generátor][sf-yeoman]

## <a name="deploy-the-application"></a>Az alkalmazás központi telepítése

### <a name="using-xplat-cli"></a>Az XPlat CLI használatával
Az alkalmazást a létrehozása után az Azure parancssori felülettel telepítheti a helyi fürtben.

1. Csatlakozzon a helyi Service Fabric-fürthöz.

    ```bash
    azure servicefabric cluster connect
    ```

2. Használja a sablonban megadott telepítési szkriptet az alkalmazáscsomag a fürt lemezképtárolójába való másolásához, regisztrálja az alkalmazás típusát, és hozza létre az alkalmazás egy példányát.

    ```bash
    ./install.sh
    ```

3. Nyisson meg egy böngészőt, és keresse fel a Service Fabric Explorert a következő címen: http://localhost:19080/Explorer (a Vagrant Mac OS X rendszeren való használata esetében a localhost helyett használja a virtuális gép magánhálózati IP-címét).
4. Bontsa ki az Alkalmazások csomópontot, és figyelje meg, hogy most már megjelenik benne egy bejegyzés az alkalmazása típusához, és egy másik a típus első példányához.
5. A sablonban megadott eltávolítása parancsfájl segítségével törölje az alkalmazáspéldány, és az alkalmazástípus regisztrációjának törlése.

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a>Az Azure CLI 2.0 használatával

Lásd: a hivatkozási doc kezeléséről egy [alkalmazás életciklusa az Azure CLI 2.0 használatával](service-fabric-application-lifecycle-azure-cli-2-0.md).

A mintaalkalmazás [kivételt a Service Fabric-tároló kódja a Githubon található – minták](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="adding-more-services-to-an-existing-application"></a>További szolgáltatások hozzáadása meglévő alkalmazáshoz

Egy másik tárolószolgáltatás hozzáadása egy már létrehozott alkalmazás `yo`, hajtsa végre a következő lépéseket:

1. Lépjen a meglevő alkalmazás gyökérkönyvtárába.  Például `cd ~/YeomanSamples/MyApplication`, ha a `MyApplication` a Yeoman által létrehozott alkalmazás.
2. Futtassa a `yo azuresfcontainer:AddService` parancsot.

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a>Manuálisan csomag és a tároló-lemezkép központi telepítése
A folyamat manuálisan csomagolási indexelése szolgáltatás alapul az alábbi lépéseket:

1. A tárolók közzététele a tárházhoz.
2. Hozza létre a csomag könyvtárstruktúrát.
3. A service manifest fájl szerkesztése.
4. Az Alkalmazásjegyzék-fájl szerkesztése.

## <a name="deploy-and-activate-a-container-image"></a>Központi telepítése és a tároló lemezkép aktiválása
A Service Fabric a [alkalmazásmodell](service-fabric-application-model.md), a tároló jelöli egy alkalmazásgazda, mely több szolgáltatásban replikák kerülnek. Üzembe helyezését, és aktiválja a tároló, helyezze be a tároló lemezkép nevét egy `ContainerHost` elem a szolgáltatás jegyzékben.

A szolgáltatás jegyzékfájlban hozzáadása egy `ContainerHost` a belépési pont számára. Utána állítsa be a `ImageName` kell lennie a tároló tárházhoz és a lemezkép nevét. A következő részleges jegyzékfájl látható példa központi telepítése a tároló neve `myimage:v1` a tárházból nevű `myrepo`:

```xml
    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimage:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>
```

Bemeneti parancsok megadhatja a választható megadásával `Commands` parancsok futtatásához a tároló belső vesszővel tagolt számú elemet.

> [!NOTE]
> Ha a kép nem rendelkezik egy munkaterhelés belépési pont definiálva, akkor explicit módon adja meg a bemeneti parancsokat belül kell `Commands` elem futhat a tároló, amely akkor is megtartja a tároló indítás után fut, parancsokat tartalmazó, vesszővel elválasztott készletével.

## <a name="understand-resource-governance"></a>Erőforrás-irányítás ismertetése
Erőforrás-irányítás egy olyan képességet, a tároló, amely korlátozza az erőforrásokat, amelyek a tároló a gazdagépen. A `ResourceGovernancePolicy`, amely az alkalmazás jegyzékében meghatározott szolgál a szolgáltatáscsomagot a kód erőforrás korlátairól deklarálható. Erőforrás-korlátozások állíthat be a következőket:

* Memory (Memória)
* MemorySwap
* CpuShares (CPU relatív súly)
* MemoryReservationInMB  
* BlkioWeight (BlockIO relatív súly).

> [!NOTE]
> Egy későbbi kiadásban támogatja a megadott blokk IO korlátok iops-érték, például megadása olvasási/írási bit/másodperc, és másokkal is fog szerepelni.
>
>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500"
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="authenticate-a-repository"></a>A tárház hitelesítéséhez
Egy tároló letöltéséhez, lehetséges, hogy bejelentkezési hitelesítő adatok a tároló tárházba. Adja meg a bejelentkezési adatait, vagy az SSH-kulcsot a tároló lemezkép letöltése a lemezképtárból a bejelentkezéshez megadott hitelesítő adatokat, az alkalmazás jegyzékében szolgálnak. A következő példa bemutatja nevű *tesztfelhasználó néven* és a jelszó nyílt szövegként (*nem* ajánlott):

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

Azt javasoljuk, hogy a gépre telepített tanúsítvány használatával titkosítja a jelszót.

A következő példa bemutatja nevű *tesztfelhasználó néven*, ahol a jelszó nevű tanúsítvánnyal titkosított *MyCert*. Használhatja a `Invoke-ServiceFabricEncryptText` PowerShell-parancsot a jelszót a titkos titkosítási szöveg létrehozásához. További információkért lásd: a cikk [Service Fabric-alkalmazások a titkos kulcsok kezelése](service-fabric-application-secret-management.md).

A tanúsítvány használatával fejthetők vissza a jelszót a titkos kulcsot egy sávon kívüli módszert a helyi gépre kell telepíteni. (Az Azure-ban Ez a módszer az Azure Resource Manager.) Majd amikor a Service Fabric telepíti a szolgáltatáscsomagnak a gép, azt vissza tudja fejteni a titkos kulcsot. A fiók nevét és a titkos kulcs használatával, majd hitelesítheti a tároló tárházban.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping"></a>Tároló port-állomás porthozzárendelés konfigurálása
Egy megadásával a tárolóhoz való kommunikációhoz használt állomás port is beállítható egy `PortBinding` az alkalmazásjegyzékben. A port kötés leképezi a portot, amelyre a szolgáltatás figyeli-e a tárolót, hogy a portot a gazdagépen belül.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-to-container-discovery-and-communication"></a>Tároló-tároló felderítése és a kommunikáció konfigurálása
Használatával a `PortBinding` házirend, a tároló port hozzárendelhető egy `Endpoint` a szolgáltatás jegyzékben. A végpont `Endpoint1` adhat meg egy rögzített port (például a 80-as port). Azt is megadhatja, nincs port minden, ebben az esetben a fürt alkalmazás porttartományát a véletlenszerű port meg van kiválasztva.

Ha megad egy végpontot, használja a `Endpoint` címkét a szolgáltatás jegyzékfájlban Vendég-tároló, a Service Fabric automatikusan közzéteheti a végpont a Naming service. A fürtben futó egyéb szolgáltatások így képes felderíteni a tárolót a REST-lekérdezésekkel kapcsolatos.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

Ha regisztrálja a Naming Service, egyszerűen elvégezhető tároló-tároló kommunikációs a kódban a tárolóban használatával a [fordított proxy](service-fabric-reverseproxy.md). Kommunikáció a fordított proxy figyelő http-port és a szolgáltatások környezeti változóként folytatott kommunikációhoz használni kívánt név megadásával történik. További információkért tekintse meg a következő szakaszban.

## <a name="configure-and-set-environment-variables"></a>Környezeti változók konfigurálása és beállítása
Környezeti változók minden szolgáltatás jegyzékben, mind a tárolók üzembe helyezett szolgáltatások vagy szolgáltatások, folyamatok/Vendég végrehajtható fájlok telepített kódcsomag adható meg. A környezeti változók értékeinek kifejezetten az alkalmazásjegyzékben szereplő felül, vagy alkalmazás paraméterekként telepítés során megadott.

A következő szolgáltatásjegyzékbeli XML-kódrészlet arra mutat be egy példát, hogyan adhat meg környezeti változókat egy kódcsomaghoz:

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>
```

Ezek a környezeti változók az alkalmazás jegyzékének szintjén felülbírálható lesz:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

Az előző példában azt meg explicit érték a `HttpGateway` környezeti változó (19000), miközben hivatott értéke `BackendServiceName` keresztül paraméter a `[BackendSvc]` alkalmazás paraméter. Ezek a beállítások lehetővé teszik az értéket adja meg `BackendServiceName`értéke, ha az alkalmazás központi telepítése, és nem rendelkezik rögzített érték a jegyzékfájlban.

## <a name="complete-examples-for-application-and-service-manifest"></a>Fejezze be az alkalmazás és szolgáltatás jegyzékfájl példák

A következő példa alkalmazásjegyzéket:

```xml
    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>
```

A következő egy példa service jegyzékfájl (az előző alkalmazásjegyzékben megadott):

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless front end in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Endpoints>
                <Endpoint Name="Endpoint1" UriScheme="http" Port="80" Protocol="http"/>
            </Endpoints>
        </Resources>
    </ServiceManifest>
```

## <a name="next-steps"></a>Következő lépések
Most, hogy indexelése szolgáltatásként telepített, akkor útmutató elolvasásával életciklus kezeléséhez [Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md).

* [A Service Fabric és a tárolók áttekintése](service-fabric-containers-overview.md)
* [Service Fabric-fürtökkel folytatott interakció az Azure parancssori felületének használatával](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a>Kapcsolódó cikkek

* [A Service Fabric első lépései az Azure CLI 2.0 használatával](service-fabric-azure-cli-2-0.md)
* [Első lépések a Service Fabric XPlat CLI használatával](service-fabric-azure-cli.md)
