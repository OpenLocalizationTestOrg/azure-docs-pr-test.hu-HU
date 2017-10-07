---
title: "aaaService háló és a tárolók üzembe helyezése |} Microsoft Docs"
description: "A Service Fabric és hello tárolók toodeploy mikroszolgáltatási alkalmazások használatát. Ez a cikk ismerteti, amely a Service Fabric-tárolókban, és hogyan toodeploy egy Windows-tároló kép fürtbe hello képességek."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 799cc9ad-32fd-486e-a6b6-efff6b13622d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: 8b6540579641474f21b8712b56049c7d177bec26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-windows-container-tooservice-fabric"></a>A Windows-tároló tooService háló telepítése
> [!div class="op_single_selector"]
> * [Windows-tároló üzembe](service-fabric-deploy-container.md)
> * [Docker-tároló telepítése](service-fabric-deploy-container-linux.md)
> 
> 

Ez a cikk bemutatja, hogyan indexelése szolgáltatások a Windows-tárolókban lévő hello folyamatán.

A Service Fabric különböző képességeket, amelyek segítenek a tárolók belül futó mikroszolgáltatások épülnek alkalmazások építéséhez rendelkezik. 

hello képességei a következők:

* Tároló lemezkép-telepítés és az aktiválás
* Erőforrás-irányítás
* Tárház-hitelesítés
* Tároló port-állomás porthozzárendelés
* Tároló-tároló felderítése és kommunikáció
* Képes tooconfigure és környezeti változók megadása

Egyes funkciók működése során egy indexelése szolgáltatás toobe bele az alkalmazásba most csomagolására vizsgáljuk meg.

## <a name="package-a-windows-container"></a>A csomag egy Windows-tároló
Ha csomag tárolója, választhat toouse vagy a Visual Studio-projektsablont vagy [hozzon létre manuálisan hello alkalmazáscsomag](#manually).  Visual Studio, alkalmazás-csomag szerkezete hello használata, miután fájlok, a rendszer létrehozza hello új projektsablon által.

> [!TIP]
> hello legegyszerűbb módja toopackage szolgáltatás meglévő tárolót a képet a Visual Studio toouse.

## <a name="use-visual-studio-toopackage-an-existing-container-image"></a>Visual Studio toopackage meglévő tároló lemezkép használata
A Visual Studio biztosít a Service Fabric szolgáltatás sablon toohelp tároló tooa Service Fabric-fürt központi telepítése.

1. Válasszon **fájl** > **új projekt**, és a Service Fabric-alkalmazás létrehozása.
2. Válasszon **Vendég tároló** hello szolgáltatássablonként.
3. Válasszon **Lemezképnév** , és adja meg a tároló-tárház hello elérési toohello lemezképet. Például `myrepo/myimage:v1` a https://hub.docker.com
4. Nevezze el a szolgáltatást, és kattintson az **OK** gombra.
5. Ha a tárolóalapú szolgáltatásnak kell a végpont-kommunikációhoz, hozzáadhat hello protokoll, port és típus toohello ServiceManifest.xml fájlt. Példa: 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    Hello megadásával `UriScheme`, a Service Fabric automatikusan regisztrálja hello tároló végpont hello felderíthetőség a Naming service. hello port kell (a fenti példa hello) rögzített vagy dinamikusan lefoglalt. Ha nem adja meg a portot, azt dinamikusan lefoglalt az alkalmazás porttartományát hello (ahogyan bármely szolgáltatással).
    Szükség tooconfigure hello tároló toohost porthozzárendelés megadásával egy `PortBinding` hello alkalmazásjegyzékben házirend. További információkért lásd: [tároló toohost porthozzárendelés konfigurálása](#Portsection).
6. Ha a tároló erőforrás irányítás kell, majd adja hozzá a `ResourceGovernancePolicy`.
8. Ha a tároló tooauthenticate a személyes tára, majd adja hozzá `RepositoryCredentials`.
7. Futtatja egy Windows Server 2016-gépnek tároló támogatás engedélyezett, ha hello csomagot használja, és művelet toodeploy tooyour helyi fürt közzététele. 
8. Ha készen áll, hello alkalmazás tooa távoli fürt közzététele, vagy ellenőrizze hello megoldás toosource vezérlőben. 

Például egy kivételt hello [Service Fabric tároló mintakódjainak megtekintése a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="creating-a-windows-server-2016-cluster"></a>A Windows Server 2016-os-fürt létrehozása
toodeploy toocreate tároló-támogatással rendelkező Windows Server 2016 rendszerű fürtre engedélyezve van szüksége a tárolóalapú alkalmazás. A fürt helyben fut, vagy telepítés keresztül Azure Resource Manager az Azure-ban. 

Azure Resource Manager, a fürt toodeploy válasszon hello **Windows Server 2016 tárolók** rendszerképet az Azure-ban a beállítás. Hello cikke [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md). Győződjön meg arról, hogy hello Azure Resource Manager-beállítások a következő:

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
Is használhatja a hello [öt csomópont Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate egy fürt. Másik lehetőségként olvassa el a közösségi [blogbejegyzés](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) használatával a Service Fabric és a Windows-tárolókon.

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

Megadhatja a választható parancsok toorun hello tárolóhoz tartozó hello elindításakor `Commands` elemet. A több parancsok vesszővel-korlátozza őket. 

## <a name="understand-resource-governance"></a>Erőforrás-irányítás ismertetése
Erőforrás-irányítás, mert egy olyan képességet, amely korlátozza a tároló hello hello erőforrások hello tároló hello gazdagépen használhatják. Hello `ResourceGovernancePolicy`, amely hello alkalmazásjegyzékben megadott használt toodeclare erőforrás korlátai a szolgáltatáscsomagot a kódot. Erőforrás-korlátozások állíthat be a következő erőforrások hello:

* Memory (Memória)
* MemorySwap
* CpuShares (CPU relatív súly)
* MemoryReservationInMB  
* BlkioWeight (BlockIO relatív súly).

> [!NOTE]
> Támogatja a megadott blokk IO korlátok például iops-érték megadására, olvasási/írási bit/másodperc, megint mások tervbe van véve egy későbbi kiadásban.
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

## <a name ="Portsection"></a>Tároló toohost porthozzárendelés konfigurálása
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
Használhatja a hello `PortBinding` elem toomap a tárolóportot tooan végpont hello szolgáltatás jegyzékben. A következő példa hello, hello végpont `Endpoint1` 8905 egy rögzített portot határozza meg. Azt is megadhatja, nincs port minden, ebben az esetben az alkalmazás porttartományát hello fürt véletlenszerű port meg van kiválasztva.


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
Ha megad egy végpontot, használatával hello `Endpoint` címkét hello szolgáltatás jegyzékfájlban Vendég-tároló, a Service Fabric automatikusan közzéteheti a végpont toohello Naming service. Hello fürtben futó egyéb szolgáltatások így felderíthetők az ebben a tárolóban hello REST lekérdezésekkel kapcsolatos.

A Naming service hello regisztrálásával hajthat végre tároló-tároló kommunikációs a tárolóban hello segítségével [fordított proxy](service-fabric-reverseproxy.md). Kommunikációs hello fordított proxy http figyelőportja és hello szolgáltatások kiszolgálóként használni kívánt toocommunicate a környezeti változók neve hello megadásával történik. További információkért lásd a hello következő szakaszt. 

## <a name="configure-and-set-environment-variables"></a>Környezeti változók konfigurálása és beállítása
Környezeti változók minden kódcsomag hello szolgáltatás jegyzékben adható meg. Ez a funkció az összes szolgáltatáshoz elérhető attól függetlenül, hogy tárolókként, folyamatokként vagy vendég futtatható fájlokként vannak-e üzembe helyezve. Ha szeretné felülbírálni az környezeti változó értékek hello alkalmazás jegyzékfájlja, vagy adja meg azokat az alkalmazás paraméterekként üzembe helyezése során.

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

## <a name="configure-isolation-mode"></a>Az elkülönítési mód konfigurálása

A Windows tárolók - folyamat és a Hyper-V két elkülönítési módot támogat.  A hello folyamatainak elkülönítési módjának futó összes hello tárolók hello ugyanazon gazdagép gép megosztás hello kernel hello gazdagéphez. A Hyper-V hello elkülönítési üzemmódját hello kernelek elkülönítik minden Hyper-V tároló és a tároló-gazdagépen hello között. hello elkülönítési üzemmódját megadott hello `ContainerHostPolicies` hello Alkalmazásjegyzék-fájl címkét.  hello elkülönítési módok megadható `process`, `hyperv`, és `default`. Hello `default` elkülönítési üzemmódját alapértelmezés szerint használt érték túl`process` Windows Server rendszeren futtatja, és alapértelmezés szerint használt érték túl`hyperv` Windows 10-gazdagépeken.  hello alábbi kódrészletben láthatja, hogyan hello elkülönítési üzemmódját hello Alkalmazásjegyzék-fájl megadott.

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


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
* Egy vonatkozó példáért kivételt [Service Fabric tároló mintakódjainak megtekintése a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
