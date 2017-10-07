---
title: "egy Azure Service Fabric-tároló alkalmazás Linux aaaCreate |} Microsoft Docs"
description: "Hozza létre első saját, Linux-alapú tárolóalkalmazását az Azure Service Fabricban.  Az alkalmazás a Docker-lemezkép elkészítése, hello kép tooa tároló beállításjegyzék leküldéses, hozza létre, és a Service Fabric-tároló alkalmazás központi telepítése."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 348dbcbaa1a534fb2808450e4c2d5f9acc7c7b35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-container-application-on-linux"></a>Az első Service Fabric-tárolóalkalmazás létrehozása Linux rendszeren
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Egy meglévő alkalmazást futtat egy Linux-tárolóban a Service Fabric-fürt nem igényel módosításokat tooyour alkalmazásokat. Ez a cikk végigvezeti a Python tartalmazó Docker lemezkép létrehozása [Flask](http://flask.pocoo.org/) webes alkalmazás és a Service Fabric-fürt tooa telepítését.  Emellett meg is fogja osztani a tárolóalapú alkalmazást az [Azure Container Registry](/azure/container-registry/) használatával.  A cikk feltételezi, hogy rendelkezik a Docker használatára vonatkozó alapvető ismeretekkel. Megismerkedhet a Docker által olvasása hello [Docker áttekintése](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Előfeltételek
* Egy fejlesztői számítógép, amelyen a következők futnak:
  * [Service Fabric SDK és -eszközök](service-fabric-get-started-linux.md).
  * [Linuxhoz készült Docker CE](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Service Fabric parancssori felület](service-fabric-cli.md)

* Egy Azure Container Registry-beállításjegyzék – ehhez [hozzon létre egy tároló-beállításjegyzéket](../container-registry/container-registry-get-started-portal.md) Azure-előfizetésében. 

## <a name="define-hello-docker-container"></a>Hello Docker-tároló megadása
Build hello alapuló rendszerképet [Python kép](https://hub.docker.com/_/python/) Docker hub található. 

Definiálja a Docker-tárolót egy Docker-fájlban. hello Dockerfile belül a tároló hello környezet létrehozása, azt szeretné, hogy toorun hello alkalmazásokat és portok leképezési kapcsolatos utasításokat tartalmazza. hello Dockerfile hello bemeneti toohello `docker build` parancsot, amely hello lemezképet. 

Hozzon létre egy üres könyvtárat, és hozzon létre hello fájl *Dockerfile* (kiterjesztésű nem). Adja hozzá a hello túl a következő*Dockerfile* és mentse a módosításokat:

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set hello working directory too/app
WORKDIR /app

# Copy hello current directory contents into hello container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
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

## <a name="build-hello-image"></a>Hello lemezkép
Futtassa a hello `docker build` , amelyen a webalkalmazás toocreate hello képe. Nyisson meg egy PowerShell-ablakot, és keresse meg a túl*c:\temp\helloworldapp*. Futtassa a következő parancs hello:

```bash
docker build -t helloworldapp .
```

Ez a parancs buildek hello hello utasítások használatát a Dockerfile új lemezkép elnevezési (-t címkézés) hello kép "helloworldapp". Lemezkép létrehozása hello alapjául szolgáló lemezképhez kérjen az Docker Hub, és létrehoz egy új lemezképet, amely az alkalmazás hello alapjául szolgáló lemezképhez felett.  

Miután hello build parancs végrehajtása után futtassa az hello `docker images` hello új lemezkép toosee vonatkozó parancsot:

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-hello-application-locally"></a>Hello alkalmazás helyileg történő futtatása
Győződjön meg arról, hogy a tárolóalapú alkalmazás helyileg előtt azt hello tároló beállításjegyzék fut-e.  

Hello alkalmazás futtatásához, a számítógép 4000 portra toohello tároló leképezési kitett 80-as port:

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

*név* biztosít a tárolóhoz (és nem hello tárolóhely-azonosító) futtató neve toohello.

Csatlakoztassa a tárolóban futó toohello.  Nyisson meg egy webböngészőt, toohello IP-címet adja vissza a port 4000, például "http://localhost:4000" mutat. Láthatja a "Hello World!" fejléc hello hello böngészőben megjelenő.

![Hello World!][hello-world]

toostop a tárolóhoz, futtassa:

```bash
docker stop my-web-site
```

Hello tároló törlése a fejlesztési számítógépén:

```bash
docker rm my-web-site
```

## <a name="push-hello-image-toohello-container-registry"></a>Leküldéses hello kép toohello tároló beállításjegyzék
Miután ellenőrizte, hogy hello alkalmazás a Docker fut-e, leküldéses hello kép tooyour beállításjegyzék Azure tároló beállításjegyzékben.

Futtatás `docker login` tooyour tároló beállításjegyzék rendelkező toolog a [beállításjegyzék hitelesítő adatok](../container-registry/container-registry-authentication.md).

hello alábbi példa továbbítja hello Azonosítót és jelszót egy Azure Active Directory [egyszerű](../active-directory/active-directory-application-objects.md). Például előfordulhat, hogy rendelt hozzá egy szolgáltatás egyszerű tooyour beállításjegyzék az automation-forgatókönyv.  Vagy bejelentkezhet a beállításjegyzékhez tartozó felhasználónevével és jelszavával.

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

hello következő parancs létrehoz egy címkét, vagy egy aliast hello kép, a teljes elérési útja tooyour beállításjegyzékbeli. Ez a Példa helyek hello hello lemezképet `samples` névtér tooavoid zsúfoltságát hello beállításjegyzék hello gyökérmappájában.

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Leküldéses hello kép tooyour tároló beállításjegyzék:

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-hello-docker-image-with-yeoman"></a>Csomag hello Docker rendszerképének Yeoman
Service Fabric SDK Linux hello tartalmaz egy [Yeoman](http://yeoman.io/) , így könnyen toocreate generátor az alkalmazás, és adja hozzá a tároló-lemezkép. Most használja az alkalmazás egy Docker-tároló nevű Yeoman toocreate *SimpleContainerApp*.

a Service Fabric-tároló alkalmazás toocreate nyisson meg egy terminálablakot, és futtassa `yo azuresfcontainer`.  

Adjon nevet az alkalmazásnak (például „mycontainer”). 

Adja meg hello tároló lemezképének egy tároló beállításjegyzék hello URL-címe (például ""). 

Ez a rendszerkép rendelkezik egy munkaterhelés belépési pont definiálva, ezért kell tooexplicitly adja meg az bemeneti parancsot (a parancsok futhat hello tároló, amely közli a hello tároló indítás után fut). 

Adja meg az „1” példányszámát.

![Tárolókhoz készült Service Fabric Yeoman-generátor][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a>A porthozzárendelés és a tárolóadattár hitelesítésének konfigurálása
A tárolóalapú szolgáltatáshoz szükség van egy kommunikációs végpontra.  Ezután adja hozzá a hello protokoll, port és típus tooan `Endpoint` hello ServiceManifest.xml fájlban. Ez a cikk indexelése hello szolgáltatást figyel 4000 portra: 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
Hello biztosító `UriScheme` automatikusan regiszterekben hello-tároló végpont hello Service Fabric-szolgáltatás felderítése a. Ez a cikk végén hello ServiceManifest.xml teljes példa fájl valósul meg. 

Konfigurálja az hello tárolóportot állomásport leképezés meghatározásával egy `PortBinding` házirend `ContainerHostPolicies` hello ApplicationManifest.xml fájl.  Ebben a cikkben `ContainerPort` 80 (hello tároló mutatja, 80-as port megadott hello Dockerfile) és `EndpointRef` "myserviceTypeEndpoint" (hello szolgáltatás jegyzékben meghatározott hello végpont) van.  4000 portra toohello szolgáltatás bejövő kérelmek tooport hello tárolóra 80 képezi le.  Ha a tároló tooauthenticate a személyes tára, majd adja hozzá `RepositoryCredentials`.  Ez a cikk vegye fel az hello fióknevet és jelszót hello myregistry.azurecr.io tároló rendszerleíró adatbázis számára. 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-hello-service-fabric-application"></a>Buildelés és a csomag hello Service Fabric-alkalmazás
hello Service Fabric Yeoman-sablonjai tartalmazzák a build parancsfájl [Gradle](https://gradle.org/), mely terminál hello toobuild hello alkalmazást is használhatja. toobuild és a csomag hello alkalmazás, futtassa a következő hello:

```bash
cd mycontainer
gradle
```

## <a name="deploy-hello-application"></a>Hello alkalmazás központi telepítése
Miután hello alkalmazás épül, ezután telepítheti azt toohello helyi fürt hello Service Fabric parancssori felület használatával.

Csatlakozás helyi Service Fabric-fürt toohello.

```bash
sfctl cluster select --endpoint http://localhost:19080
```

Használjon hello telepítési parancsfájlját hello sablon toocopy hello alkalmazás csomag toohello fürt lemezképtárolóhoz, hello alkalmazástípus regisztrálása, és a hello alkalmazás példányt létrehozni.

```bash
./install.sh
```

Nyisson meg egy böngészőt, és keresse meg a Fabric Explorer tooService: 19080/Explorer (a név felülírandó localhost a hello privát IP-címe hello VM Vagrant használatakor a Mac OS x). Hello alkalmazások csomópontot, és vegye figyelembe, hogy most egy bejegyzést, az alkalmazás típusának és egy másikat a hello adott típus első példányát.

Csatlakoztassa a tárolóban futó toohello.  Nyisson meg egy webböngészőt, toohello IP-címet adja vissza a port 4000, például "http://localhost:4000" mutat. Láthatja a "Hello World!" fejléc hello hello böngészőben megjelenő.

![Hello World!][hello-world]

## <a name="clean-up"></a>A fölöslegessé vált elemek eltávolítása
Hello eltávolítás parancsfájl használata hello sablon toodelete hello alkalmazáspéldány hello helyi fejlesztési fürtök és a hello alkalmazástípus regisztrációjának törlése.

```bash
./uninstall.sh
```

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
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         hello UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers 
      tooService Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables tooyour container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="mycontainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion 
       should match hello Name and Version attributes of hello ServiceManifest element defined in hello 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="myservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using hello 
         ServiceFabric PowerShell module.
         
         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount too1.  On a multi-node production 
      cluster, set InstanceCount too-1 for hello container service toorun on every node in 
      hello cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="adding-more-services-tooan-existing-application"></a>További szolgáltatások tooan meglévő alkalmazás hozzáadása

tooadd egy másik tároló szolgáltatás tooan yeoman, már létrehozott alkalmazás végrehajtani hello a következő lépéseket:

1. Hello meglévő alkalmazás gyökérkönyvtárának toohello módosítása.  Például `cd ~/YeomanSamples/MyApplication`, ha `MyApplication` Yeoman által létrehozott hello alkalmazás.
2. Futtassa a `yo azuresfcontainer:AddService` parancsot.

<a id="manually"></a>


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

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
