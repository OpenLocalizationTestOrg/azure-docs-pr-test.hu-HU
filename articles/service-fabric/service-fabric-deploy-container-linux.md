---
title: "aaaService háló és a Linux tárolók üzembe helyezése |} Microsoft Docs"
description: "A Service Fabric és hello használata Linux tárolók toodeploy mikroszolgáltatási alkalmazások. Ez a cikk ismerteti, amely a Service Fabric-tárolókban, és hogyan toodeploy egy Linux-tároló kép fürtbe hello képességek"
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
ms.openlocfilehash: e28f99a145b0594d871b0ec0566233a7ad235ce8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-linux-container-tooservice-fabric"></a>A Linux-tároló tooService háló telepítése
> [!div class="op_single_selector"]
> * [Windows-tároló üzembe](service-fabric-deploy-container.md)
> * [Linux-tároló üzembe](service-fabric-deploy-container-linux.md)
>
>

Ez a cikk bemutatja, hogyan indexelése szolgáltatások Docker-tárolókban lévő Linux építve.

A Service Fabric különböző tároló képességeket, amelyek segítenek a mikroszolgáltatások létrehozására, amelyek indexelése épülnek alkalmazások építéséhez rendelkezik. Ezek a szolgáltatások indexelése szolgáltatások nevezzük.

hello lehetőségek tartalmazzák;

* Tároló lemezkép-telepítés és az aktiválás
* Erőforrás-irányítás
* Tárház-hitelesítés
* Tároló-porthozzárendelést, toohost port
* Tároló-tároló felderítése és kommunikáció
* Képes tooconfigure és környezeti változók megadása

## <a name="packaging-a-docker-container-with-yeoman"></a>Egy docker-tároló csomagolására rendelkező yeoman
Egy tároló Linux csomagolás, beállíthatja vagy toouse yeoman sablon vagy [hozzon létre manuálisan hello alkalmazáscsomag](#manually).

A Service Fabric-alkalmazás tartalmazhat egy vagy több tárolóban, az egy adott szerepkör a postai hello alkalmazás működését. Service Fabric SDK Linux hello tartalmaz egy [Yeoman](http://yeoman.io/) , így könnyen toocreate generátor az alkalmazás, és adja hozzá a tároló-lemezkép. Most használja az alkalmazás egy Docker-tároló nevű Yeoman toocreate *SimpleContainerApp*. Hozzáadhat további szolgáltatások később hello létrehozott fájlok szerkesztésével.

## <a name="install-docker-on-your-development-box"></a>A fejlesztési mezőben Docker telepítése

Futtatási hello következő parancsokat a Linux-fejlesztési be tooinstall docker (hello vagrant lemezkép használatakor az os x docker már telepítve van):

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása
1. Írja be a terminálba a következőt: `yo azuresfcontainer`.
2. Az alkalmazás - például mycontainerap neve
3. Adja meg hello tároló lemezképének egy DockerHub tárházból hello URL-CÍMÉT. hello kép paraméter vesz hello űrlap [tárház] / [lemezkép neve]
4. Ha hello kép nem rendelkezik egy munkaterhelés belépési pont definiálva, akkor meg kell tooexplicitly adja meg a bemeneti parancsokat parancsok toorun belül hello tároló, amely közli a hello tároló indítás után fut vesszővel tagolt állítja be.

![Tárolókhoz készült Service Fabric Yeoman-generátor][sf-yeoman]

## <a name="deploy-hello-application"></a>Hello alkalmazás központi telepítése

### <a name="using-xplat-cli"></a>Az XPlat CLI használatával
Miután hello alkalmazás épül, ezután telepítheti azt toohello helyi fürt hello Azure parancssori felület használatával.

1. Csatlakozás helyi Service Fabric-fürt toohello.

    ```bash
    azure servicefabric cluster connect
    ```

2. Használjon hello telepítési parancsfájlját hello sablon toocopy hello alkalmazás csomag toohello fürt lemezképtárolóhoz, hello alkalmazástípus regisztrálása, és a hello alkalmazás példányt létrehozni.

    ```bash
    ./install.sh
    ```

3. Nyisson meg egy böngészőt, és keresse meg a Fabric Explorer tooService: 19080/Explorer (a név felülírandó localhost a hello privát IP-címe hello VM Vagrant használatakor a Mac OS x).
4. Hello alkalmazások csomópontot, és vegye figyelembe, hogy most egy bejegyzést, az alkalmazás típusának és egy másikat a hello adott típus első példányát.
5. Hello eltávolítás parancsfájl használata hello sablon toodelete hello alkalmazáspéldány és a hello alkalmazástípus regisztrációjának törlése.

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a>Az Azure CLI 2.0 használatával

Lásd: hello hivatkozás doc kezeléséről egy [életciklus használó hello Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).

A mintaalkalmazás [kivételt hello Service Fabric tároló kódja a Githubon található – minták](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="adding-more-services-tooan-existing-application"></a>További szolgáltatások tooan meglévő alkalmazás hozzáadása

tooadd egy másik tárolóban szolgáltatásalkalmazás tooan már létrehozott `yo`, hajtsa végre az alábbi lépésekkel hello:

1. Hello meglévő alkalmazás gyökérkönyvtárának toohello módosítása.  Például `cd ~/YeomanSamples/MyApplication`, ha `MyApplication` Yeoman által létrehozott hello alkalmazás.
2. Futtassa a `yo azuresfcontainer:AddService` parancsot.

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a>Manuálisan csomag és a tároló-lemezkép központi telepítése
hello folyamat manuálisan csomagolási indexelése szolgáltatás lépések hello alapul:

1. Hello tárolók tooyour tárház közzététele.
2. Hello csomag könyvtárstruktúrát létrehozása.
3. Hello service manifest-fájl szerkesztése.
4. Hello Alkalmazásjegyzék-fájl szerkesztése.

## <a name="deploy-and-activate-a-container-image"></a>Központi telepítése és a tároló lemezkép aktiválása
A Service Fabric hello [alkalmazásmodell](service-fabric-application-model.md), a tároló jelöli egy alkalmazásgazda, mely több szolgáltatásban replikák kerülnek. toodeploy, és aktiválja a tároló, hello tároló lemezkép a put hello nevét egy `ContainerHost` elem hello szolgáltatás jegyzékben.

Hello szolgáltatás jegyzékben, vegye fel a `ContainerHost` hello belépési ponthoz. Majd a készlet hello `ImageName` hello tároló tárház és lemezkép toobe hello nevét. hello következő részleges jegyzékfájl szemlélteti, hogyan toodeploy hello tároló nevű `myimage:v1` a tárházból nevű `myrepo`:

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

Megadhat bemeneti parancsok választható hello megadásával `Commands` parancsok toorun belül hello tároló vesszővel tagolt számú elemet.

> [!NOTE]
> Ha hello kép nem rendelkezik egy munkaterhelés belépési pont definiálva, akkor meg kell tooexplicitly adja meg a bemeneti parancsok belül `Commands` parancsok toorun belül hello tároló, amely közli a hello tároló futtatása után vesszővel tagolt számú elem indítási.

## <a name="understand-resource-governance"></a>Erőforrás-irányítás ismertetése
Erőforrás-irányítás, mert egy olyan képességet, amely korlátozza a tároló hello hello erőforrások hello tároló hello gazdagépen használhatják. Hello `ResourceGovernancePolicy`, amely hello alkalmazásjegyzékben megadott használt toodeclare erőforrás korlátai a szolgáltatáscsomagot a kódot. Erőforrás-korlátozások állíthat be a következő erőforrások hello:

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
egy tároló toodownload, lehetséges, hogy tooprovide bejelentkezési hitelesítő adatok toohello tároló tárházba. hello bejelentkezési, hello alkalmazásjegyzékben, a megadott hitelesítő adatok használt toospecify hello bejelentkezési adatait, vagy SSH-kulcsban, a hello tároló lemezkép letöltése hello lemezképtárból. hello alábbi példa bemutatja nevű *tesztfelhasználó néven* hello a jelszó nyílt szövegként együtt (*nem* ajánlott):

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

Azt javasoljuk, hogy titkosítsa a hello jelszó toohello gépen központilag telepített tanúsítvánnyal.

hello alábbi példa bemutatja nevű *tesztfelhasználó néven*, ahol hello jelszó nevű tanúsítvánnyal titkosított *MyCert*. Használhatja a hello `Invoke-ServiceFabricEncryptText` PowerShell toocreate hello titkos titkosítási parancsszövege hello jelszót. További információkért lásd: hello cikk [Service Fabric-alkalmazások a titkos kulcsok kezelése](service-fabric-application-secret-management.md).

hello az tanúsítvány titkos kulcsa hello toodecrypt hello jelszó által használt helyi számítógép telepített toohello sávon kívüli metódusban kell lennie. (Az Azure-ban Ez a módszer az Azure Resource Manager.) Majd amikor a Service Fabric hello szolgáltatás csomag toohello számítógépe telepíti, azt vissza tudja fejteni hello titkos kulcsot. Hello titkos hello fióknév együtt használva, majd hitelesítheti a hello tároló tárház.

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
Konfigurálhatja a gazdagépen használt port toocommunicate hello tárolóval megadásával egy `PortBinding` hello alkalmazásjegyzékben. hello port kötés maps hello port toowhich hello szolgáltatást figyel tooa tárolóportot hello hello gazdagépen belül.

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
Hello segítségével `PortBinding` házirend, leképezheti a tárolóportot tooan `Endpoint` hello szolgáltatás jegyzékben. végpont hello `Endpoint1` adhat meg egy rögzített port (például a 80-as port). Azt is megadhatja, nincs port minden, ebben az esetben az alkalmazás porttartományát hello fürt véletlenszerű port meg van kiválasztva.

Ha megad egy végpontot, használatával hello `Endpoint` címkét hello szolgáltatás jegyzékfájlban Vendég-tároló, a Service Fabric automatikusan közzéteheti a végpont toohello Naming service. Hello fürtben futó egyéb szolgáltatások így felderíthetők az ebben a tárolóban hello REST lekérdezésekkel kapcsolatos.

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

Ha regisztrálja a Naming service hello, egyszerűen elvégezhető tároló-tároló kommunikációs hello kódban a tárolóban hello segítségével [fordított proxy](service-fabric-reverseproxy.md). Kommunikációs hello fordított proxy http figyelőportja és hello szolgáltatások kiszolgálóként használni kívánt toocommunicate a környezeti változók neve hello megadásával történik. További információkért lásd a hello következő szakaszt.

## <a name="configure-and-set-environment-variables"></a>Környezeti változók konfigurálása és beállítása
Környezeti változók minden hello szolgáltatás jegyzékben, mind a tárolók üzembe helyezett szolgáltatások vagy szolgáltatások, folyamatok/Vendég végrehajtható fájlok telepített kódcsomag adható meg. A környezeti változók értékeinek kifejezetten hello alkalmazásjegyzék felülbírálva, vagy alkalmazás paraméterekként telepítés során megadott.

hello service manifest következő XML-részletet szemlélteti, hogyan toospecify környezeti változók kód csomag:

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

Ezek a környezeti változók hello alkalmazás jegyzékének szintjén felülbírálható lesz:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

Hello előző példában a Microsoft hello explicit érték megadott `HttpGateway` környezeti változó (19000), miközben hivatott hello értéke `BackendServiceName` keresztül hello paraméter `[BackendSvc]` alkalmazás paraméter. Ezek a beállítások lehetővé teszik toospecify hello értéke `BackendServiceName`értéke, ha hello alkalmazás központi telepítése, és nem rendelkezik rögzített érték hello jegyzékben.

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

A következő egy példa service jegyzékfájl (alkalmazásjegyzék megelőző hello megadott):

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
Most, hogy indexelése szolgáltatásként telepített, akkor megtudhatja, hogyan toomanage olvasásával életciklus [Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md).

* [A Service Fabric és a tárolók áttekintése](service-fabric-containers-overview.md)
* [Service Fabric-fürtök hello Azure parancssori felület használatával való interakció](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a>Kapcsolódó cikkek

* [A Service Fabric első lépései az Azure CLI 2.0 használatával](service-fabric-azure-cli-2-0.md)
* [Első lépések a Service Fabric XPlat CLI használatával](service-fabric-azure-cli.md)
