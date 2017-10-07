---
title: "az Azure Service Fabric-tároló alkalmazás aaaCreate |} Microsoft Docs"
description: "Hozza létre első saját, Windows-alapú tárolóalkalmazását az Azure Service Fabricban.  A Python alkalmazásokkal rendelkező Docker-lemezkép elkészítése, hello kép tooa tároló beállításjegyzék leküldéses, hozza létre, és a Service Fabric-tároló alkalmazás központi telepítése."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/18/2017
ms.author: ryanwi
ms.openlocfilehash: b79d3a41eb2da5f7791266588fe9ea7becb0e58f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-container-application-on-windows"></a>Az első Service Fabric-tárolóalkalmazás létrehozása Windows rendszeren
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Egy meglévő alkalmazást futtat egy Windows-tárolóban a Service Fabric-fürt nem igényel módosításokat tooyour alkalmazásokat. Ez a cikk végigvezeti a Python tartalmazó Docker lemezkép létrehozása [Flask](http://flask.pocoo.org/) webes alkalmazás és a Service Fabric-fürt tooa telepítését.  Emellett meg is fogja osztani a tárolóalapú alkalmazást az [Azure Container Registry](/azure/container-registry/) használatával.  A cikk feltételezi, hogy rendelkezik a Docker használatára vonatkozó alapvető ismeretekkel. Megismerkedhet a Docker által olvasása hello [Docker áttekintése](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Előfeltételek
Egy fejlesztői számítógép, amelyen a következők futnak:
* Visual Studio 2015 vagy Visual Studio 2017.
* [Service Fabric SDK és -eszközök](service-fabric-get-started.md).
*  Windows rendszerhez készült Docker.  [A Docker CE for Windows (stable) letöltése](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Miután telepíti és indítja a Docker, kattintson a jobb gombbal a hello tálcai ikonja, és válassza ki **tooWindows tárolók kapcsoló**. Ez a Windows-alapú szükséges toorun Docker-lemezképeket.

Egy Windows-fürt legalább három, Windows Server 2016 rendszerű, a Containerst futtató csomóponttal. Ehhez [hozzon létre egy fürtöt](service-fabric-cluster-creation-via-portal.md) vagy [próbálja ki ingyen a Service Fabricot](https://aka.ms/tryservicefabric).

Egy Azure Container Registry-beállításjegyzék – ehhez [hozzon létre egy tároló-beállításjegyzéket](../container-registry/container-registry-get-started-portal.md) Azure-előfizetésében.

## <a name="define-hello-docker-container"></a>Hello Docker-tároló megadása
Build hello alapuló rendszerképet [Python kép](https://hub.docker.com/_/python/) Docker hub található.

Definiálja a Docker-tárolót egy Docker-fájlban. hello Dockerfile belül a tároló hello környezet létrehozása, azt szeretné, hogy toorun hello alkalmazásokat és portok leképezési kapcsolatos utasításokat tartalmazza. hello Dockerfile hello bemeneti toohello `docker build` parancsot, amely hello lemezképet.

Hozzon létre egy üres könyvtárat, és hozzon létre hello fájl *Dockerfile* (kiterjesztésű nem). Adja hozzá a hello túl a következő*Dockerfile* és mentse a módosításokat:

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set hello working directory too/app
WORKDIR /app

# Copy hello current directory contents into hello container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available toohello world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when hello container launches
CMD ["python", "app.py"]
```

Olvasási hello [Dockerfile hivatkozás](https://docs.docker.com/engine/reference/builder/) további információt.

## <a name="create-a-simple-web-application"></a>Egyszerű webalkalmazás létrehozása
Hozzon létre egy olyan Flask-webalkalmazást, amely a 80-as portot figyeli, és a „Hello World!” szöveget adja vissza.  A hello ugyanabban a könyvtárban, és hozzon létre hello fájl *requirements.txt*.  Adja hozzá a következő hello, és mentse a módosításokat:
```
Flask
```

Hello is létrehozhat *app.py* fájlt, és adja hozzá a következő hello:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():

    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

<a id="Build-Containers"></a>
## <a name="build-hello-image"></a>Hello lemezkép
Futtassa a hello `docker build` , amelyen a webalkalmazás toocreate hello képe. Nyisson meg egy PowerShell-ablakot, és keresse meg a hello Dockerfile tartalmazó toohello könyvtár. Futtassa a következő parancs hello:

```
docker build -t helloworldapp .
```

Ez a parancs buildek hello hello utasítások használatát a Dockerfile új lemezkép elnevezési (-t címkézés) hello kép "helloworldapp". Lemezkép létrehozása hello alapjául szolgáló lemezképhez kérjen az Docker Hub, és létrehoz egy új lemezképet, amely az alkalmazás hello alapjául szolgáló lemezképhez felett.  

Miután hello build parancs végrehajtása után futtassa az hello `docker images` hello új lemezkép toosee vonatkozó parancsot:

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-hello-application-locally"></a>Hello alkalmazás helyileg történő futtatása
Ellenőrizze a lemezkép helyileg előtt azt hello tároló beállításjegyzék.  

Hello alkalmazás futtatásához:

```
docker run -d --name my-web-site helloworldapp
```

*név* biztosít a tárolóhoz (és nem hello tárolóhely-azonosító) futtató neve toohello.

Hello tároló elindul, ha az IP-címének, hogy a tárolóban futó böngészővel tooyour csatlakozáskor:
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

Csatlakoztassa a tárolóban futó toohello.  Nyisson meg egy webböngészőt, toohello IP-címet ad vissza, például "http://172.31.194.61" mutat. Láthatja a "Hello World!" fejléc hello hello böngészőben megjelenő.

toostop a tárolóhoz, futtassa:

```
docker stop my-web-site
```

Hello tároló törlése a fejlesztési számítógépén:

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-hello-image-toohello-container-registry"></a>Leküldéses hello kép toohello tároló beállításjegyzék
Miután meggyőződött a fejlesztési számítógépén fut hello tároló, leküldéses hello kép tooyour beállításjegyzék Azure tároló beállításjegyzékben.

Futtatás ``docker login`` tooyour tároló beállításjegyzék rendelkező toolog a [beállításjegyzék hitelesítő adatok](../container-registry/container-registry-authentication.md).

hello alábbi példa továbbítja hello Azonosítót és jelszót egy Azure Active Directory [egyszerű](../active-directory/active-directory-application-objects.md). Például előfordulhat, hogy rendelt hozzá egy szolgáltatás egyszerű tooyour beállításjegyzék az automation-forgatókönyv. Vagy bejelentkezhet a beállításjegyzékhez tartozó felhasználónevével és jelszavával.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

hello következő parancs létrehoz egy címkét, vagy egy aliast hello kép, a teljes elérési útja tooyour beállításjegyzékbeli. Ez a Példa helyek hello hello lemezképet ```samples``` névtér tooavoid zsúfoltságát hello beállításjegyzék hello gyökérmappájában.

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Leküldéses hello kép tooyour tároló beállításjegyzék:

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-hello-containerized-service-in-visual-studio"></a>A Visual Studio indexelése hello szolgáltatás létrehozása
hello Service Fabric SDK és az eszközök adja meg a szolgáltatási sablon toohelp indexelése-alkalmazás létrehozása.

1. Indítsa el a Visual Studiót.  Válassza a **File** (Fájl) > **New** (Új) > **Project** (Projekt) lehetőséget.
2. Válassza a **Service Fabric application** (Service Fabric-alkalmazás) lehetőséget, nevezze el „MyFirstContainer” néven, és kattintson az **OK** gombra.
3. Válassza ki **Vendég tároló** hello listája **szolgáltatássablonok**.
4. A **Lemezképnév** adja meg a "myregistry.azurecr.io/samples/helloworldapp", akkor leküldött tooyour tároló tárház hello kép.
5. Nevezze el a szolgáltatást, és kattintson az **OK** gombra.

## <a name="configure-communication"></a>A kommunikáció konfigurálása
indexelése hello szolgáltatást kell a végpont-kommunikációhoz. Adja hozzá egy `Endpoint` elem hello protokoll, port és típus toohello ServiceManifest.xml fájlt. Ez a cikk indexelése hello szolgáltatás 8081 porton figyel.  Ez a példa egy rögzített 8081-es portot használ erre a célra.  Ha nincs port meg van adva, az alkalmazás porttartományát hello véletlenszerű port van kiválasztva.  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

Meghatározhat egy olyan végpont, a Service Fabric hello végpont toohello Naming service tesz közzé.  Más hello fürtben futó szolgáltatások azt feloldva a tárolóban.  Tároló-tároló kommunikációs hello használatával is elvégezheti [fordított proxy](service-fabric-reverseproxy.md).  Kommunikációs hello fordított proxy HTTP-figyelő portja és hello szolgáltatások kiszolgálóként használni kívánt toocommunicate a környezeti változók neve hello megadásával történik.

## <a name="configure-and-set-environment-variables"></a>Környezeti változók konfigurálása és beállítása
Környezeti változók minden kódcsomag hello szolgáltatás jegyzékben adható meg. Ez a funkció az összes szolgáltatáshoz elérhető attól függetlenül, hogy tárolókként, folyamatokként vagy vendég futtatható fájlokként vannak-e üzembe helyezve. Ha szeretné felülbírálni az környezeti változó értékek hello alkalmazás jegyzékfájlja, vagy adja meg azokat az alkalmazás paraméterekként üzembe helyezése során.

hello service manifest következő XML-részletet szemlélteti, hogyan toospecify környezeti változók kód csomag:
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

Ezek a környezeti változók hello alkalmazásjegyzékben bírálható felül:

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a>Tárolóport–gazdagépport hozzárendelés és tároló–tároló felderítés konfigurálása
Konfigurálja a gazdagépen használt port toocommunicate hello tároló. hello port kötés leképezi a hello port mely hello szolgáltatást figyel tooa tárolóportot hello hello gazdagépen belül. Adja hozzá a `PortBinding` elemében `ContainerHostPolicies` hello ApplicationManifest.xml fájl eleme.  Ebben a cikkben `ContainerPort` 80 (hello tároló mutatja, 80-as port megadott hello Dockerfile) és `EndpointRef` "Guest1TypeEndpoint" van (a korábban a hello szolgáltatás jegyzékben megadott végpont hello).  A porton 8081 toohello szolgáltatás bejövő kérelmek tooport hello tárolóra 80 képezi le.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a>Tárolóregisztrációs adatbázis hitelesítésének konfigurálása
Tároló beállításjegyzék-hitelesítés konfigurálása hozzáadásával `RepositoryCredentials` túl`ContainerHostPolicies` hello ApplicationManifest.xml fájl. Adja hozzá a hello fiókkal és jelszóval hello myregistry.azurecr.io tároló beállításjegyzék, amely lehetővé teszi a hello szolgáltatás toodownload hello tároló kép adattárból hello.

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

Azt javasoljuk, hogy hello tárház jelszó rejtjelezése tanúsítványban tooall hello fürt csomópontjaira telepített segítségével a titkosítást. A Service Fabric hello szolgáltatás csomag toohello fürt telepíti, hello rejtjelezése tanúsítvány esetén használt toodecrypt hello titkosított szöveg.  hello Invoke-ServiceFabricEncryptText parancsmag az használt toocreate hello titkosított szöveg hello jelszót, toohello ApplicationManifest.xml fájl kerül.

hello következő parancsfájl egy új önaláírt tanúsítványt hoz létre, és exportálja azt tooa PFX-fájlt.  hello tanúsítvány egy meglévő kulcstároló importálja, és majd a Service Fabric-fürt toohello telepít.

```powershell
# Variables.
$certpwd = ConvertTo-SecureString -String "Pa$$word321!" -Force -AsPlainText
$filepath = "C:\MyCertificates\dataenciphermentcert.pfx"
$subjectname = "dataencipherment"
$vaultname = "mykeyvault"
$certificateName = "dataenciphermentcert"
$groupname="myclustergroup"
$clustername = "mycluster"

$subscriptionId = "subscription ID"

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a self signed cert, export tooPFX file.
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject $subjectname -Provider 'Microsoft Enhanced Cryptographic Provider v1.0' `
| Export-PfxCertificate -FilePath $filepath -Password $certpwd

# Import hello certificate tooan existing key vault.  hello key vault must be enabled for deployment.
$cer = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certificateName -FilePath $filepath -Password $certpwd

Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $groupname -EnabledForDeployment

# Add hello certificate tooall hello VMs in hello cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $cer.SecretId
```
Hello jelszó használatával hello titkosítása [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) parancsmag.

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

Hello jelszó cseréje hello titkosítási szövegre hello által visszaadott [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) parancsmag és `PasswordEncrypted` túl "true".

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-isolation-mode"></a>Az elkülönítési mód konfigurálása
A Windows a tárolók két elkülönítési módját támogatja: a folyamatalapú és a Hyper-V módot. A hello folyamatainak elkülönítési módjának futó összes hello tárolók hello ugyanazon gazdagép gép megosztás hello kernel hello gazdagéphez. A Hyper-V hello elkülönítési üzemmódját hello kernelek elkülönítik minden Hyper-V tároló és a tároló-gazdagépen hello között. hello elkülönítési üzemmódját megadott hello `ContainerHostPolicies` hello Alkalmazásjegyzék-fájl elemében. hello elkülönítési módok megadható `process`, `hyperv`, és `default`. hello alapértelmezett elkülönítési üzemmódját alapértelmezés szerint használt érték túl`process` Windows Server rendszeren futtatja, és alapértelmezés szerint használt érték túl`hyperv` Windows 10-állomáson. hello alábbi kódrészletben láthatja, hogyan hello elkülönítési üzemmódját hello Alkalmazásjegyzék-fájl megadott.

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a>Az erőforrás-szabályozás konfigurálása
[Erőforrás-irányítás](service-fabric-resource-governance.md) erőforrásokat, amelyek tároló hello használható hello gazdagépen hello korlátozza. Hello `ResourceGovernancePolicy` , hello alkalmazásjegyzékben megadott eleme használt toodeclare erőforrás korlátai a szolgáltatáscsomagot a kódot. Erőforrás-korlátozások állíthat be a következő erőforrások hello: memória, MemorySwap, CpuShares (CPU relatív súly), MemoryReservationInMB, BlkioWeight (BlockIO relatív súly).  Ebben a példában a service-csomag Guest1Pkg egy alapvető lekérdezi a hello fürtcsomóponton, ahol el van helyezve.  Memóriakorlátokat abszolút, így hello kódcsomag korlátozott too1024 MB memória (és a soft-garancia lefoglalása hello ugyanaz). Kód csomagok (tárolók és folyamatok) olyan nem tud tooallocate toodo kísérlet, és ezt a határt több memóriával, kevés a memória kivétel eredményez. A service-csomag összes kódot csomagok erőforrás korlátját kényszerítési toowork, a megadott memóriakorlátokat kell rendelkeznie.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-hello-container-application"></a>Hello tároló alkalmazás központi telepítése
A módosítások mentéséhez és hello alkalmazás létrehozása. toopublish az alkalmazás, kattintson a jobb gombbal a **MyFirstContainer** megoldáskezelő, és válasszon **közzététel**.

A **csatlakozási végpont**, adja meg a hello felügyeleti végpont hello fürthöz.  például a „containercluster.westus2.cloudapp.azure.com:19000” végpontot. Hello ügyfélkapcsolat található végpont hello áttekintése panelen a fürt a hello [Azure-portálon](https://portal.azure.com).

Kattintson a **Publish** (Közzététel) gombra.

A [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) egy webalapú eszköz az alkalmazások és csomópontok vizsgálatához és kezeléséhez a Service Fabric-fürtökben. Nyisson meg egy böngészőt, és lépjen toohttp://containercluster.westus2.cloudapp.azure.com:19080/Explorer /, és kövesse az alkalmazástelepítés hello.  hello alkalmazás központi telepítését, de hiba állapotban van, amíg hello lemezkép letöltődik fürtcsomópontokon hello (ami eltarthat egy ideig, attól függően, hogy a kép mérete hello): ![hiba][1]

hello alkalmazás készen áll, ha a ```Ready``` állapota: ![kész][2]

Nyisson meg egy böngészőt, és keresse meg a toohttp://containercluster.westus2.cloudapp.azure.com:8081. Láthatja a "Hello World!" fejléc hello hello böngészőben megjelenő.

## <a name="clean-up"></a>A fölöslegessé vált elemek eltávolítása
Tooincur díjak folytatni, amíg hello fürt fut, érdemes lehet [a fürt törlése](service-fabric-get-started-azure-cluster.md#remove-the-cluster).  A [nyilvános fürtök](http://tryazureservicefabric.westus.cloudapp.azure.com/) néhány óra múlva automatikusan törlődnek.

Miután hello kép toohello tároló beállításjegyzék leküldéses hello kép: helyi törölheti a fejlesztési számítógépen:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Példa teljes Service Fabric-alkalmazásra és szolgáltatásjegyzékre
Az alábbiakban hello teljes szolgáltatási és az alkalmazásjegyzékeknek a cikk ezt használja.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         hello UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers tooService Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables tooyour container: -->    
    <EnvironmentVariables>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
    </EnvironmentVariables>

  </CodePackage>

  <!-- Config package is hello contents of hello Config directoy under PackageRoot that contains an
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which to
           listen. Please note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion
       should match hello Name and Version attributes of hello ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="FrontendService.Code">
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentOverrides>
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types, when an instance of this
         application type is created. You can also create one or more instances of service type using the
         ServiceFabric PowerShell module.

         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="configure-time-interval-before-container-is-force-terminated"></a>A tároló kényszerített leállítását megelőző időköz beállítása

Hello futásidejű toowait időintervallum hello tároló eltávolítása hello szolgáltatás törlése (vagy áthelyezés tooanother csomópont) megkezdése után konfigurálhatja. Konfigurálás hello alatt az időtartam alatt küldi hello `docker stop <time in seconds>` parancs toohello tároló.   További információ: [docker stop](https://docs.docker.com/engine/reference/commandline/stop/). hello idő időköz toowait hello szakaszban megadott `Hosting` szakasz. a következő fürt jegyzékének részlet hello bemutatja, hogyan tooset hello várakozás időtartama:

```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "ContainerDeactivationTimeout": "10",
          ...
          }
        ]
}
```
hello alapértelmezett időtartam értéke too10 másodperc. Mivel ez a konfiguráció a dinamikus, a konfiguráció csak hello fürt frissítések hello időtúllépés frissíti. 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a>Hello futásidejű tooremove konfigurálása nem használt tároló lemezképek

Konfigurálhatja az Service Fabric-fürt tooremove hello nem használt tároló képek hello csomópontból. Ez a konfiguráció lehetővé teszi, hogy a szabad terület toobe újrarögzítése, ha túl sok tároló képek hello csomóponton találhatók.  tooenable ezt a funkciót, a frissítés hello `Hosting` szakasz hello a fürtjegyzékben, ahogy az alábbi részlet hello: 


```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "PruneContainerImages": “True”,
            "ContainerImagesToSkip": "microsoft/windowsservercore|microsoft/nanoserver|…",
          ...
          }
        ]
} 
```

Nem törölhetők, képek, megadhatja azokat a hello `ContainerImagesToSkip` paraméter. 



## <a name="next-steps"></a>Következő lépések
* További információk a [tárolók futtatásáról a Service Fabricban](service-fabric-containers-overview.md).
* Olvasási hello [tároló egy .NET-alkalmazás központi telepítése](service-fabric-host-app-in-a-container.md) oktatóanyag.
* További tudnivalók a Service Fabric hello [alkalmazás-életciklus](service-fabric-application-lifecycle.md).
* Kivételt hello [Service Fabric tároló mintakódok](https://github.com/Azure-Samples/service-fabric-dotnet-containers) a Githubon.

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
