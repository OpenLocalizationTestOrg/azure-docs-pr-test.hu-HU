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
# <a name="create-your-first-service-fabric-container-application-on-linux"></a><span data-ttu-id="2a310-104">Az első Service Fabric-tárolóalkalmazás létrehozása Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2a310-104">Create your first Service Fabric container application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2a310-105">Windows</span><span class="sxs-lookup"><span data-stu-id="2a310-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="2a310-106">Linux</span><span class="sxs-lookup"><span data-stu-id="2a310-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="2a310-107">Egy meglévő alkalmazást futtat egy Linux-tárolóban a Service Fabric-fürt nem igényel módosításokat tooyour alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="2a310-107">Running an existing application in a Linux container on a Service Fabric cluster doesn't require any changes tooyour application.</span></span> <span data-ttu-id="2a310-108">Ez a cikk végigvezeti a Python tartalmazó Docker lemezkép létrehozása [Flask](http://flask.pocoo.org/) webes alkalmazás és a Service Fabric-fürt tooa telepítését.</span><span class="sxs-lookup"><span data-stu-id="2a310-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it tooa Service Fabric cluster.</span></span>  <span data-ttu-id="2a310-109">Emellett meg is fogja osztani a tárolóalapú alkalmazást az [Azure Container Registry](/azure/container-registry/) használatával.</span><span class="sxs-lookup"><span data-stu-id="2a310-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="2a310-110">A cikk feltételezi, hogy rendelkezik a Docker használatára vonatkozó alapvető ismeretekkel.</span><span class="sxs-lookup"><span data-stu-id="2a310-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="2a310-111">Megismerkedhet a Docker által olvasása hello [Docker áttekintése](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="2a310-111">You can learn about Docker by reading hello [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a310-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2a310-112">Prerequisites</span></span>
* <span data-ttu-id="2a310-113">Egy fejlesztői számítógép, amelyen a következők futnak:</span><span class="sxs-lookup"><span data-stu-id="2a310-113">A development computer running:</span></span>
  * <span data-ttu-id="2a310-114">[Service Fabric SDK és -eszközök](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2a310-114">[Service Fabric SDK and tools](service-fabric-get-started-linux.md).</span></span>
  * <span data-ttu-id="2a310-115">[Linuxhoz készült Docker CE](https://docs.docker.com/engine/installation/#prior-releases).</span><span class="sxs-lookup"><span data-stu-id="2a310-115">[Docker CE for Linux](https://docs.docker.com/engine/installation/#prior-releases).</span></span> 
  * [<span data-ttu-id="2a310-116">Service Fabric parancssori felület</span><span class="sxs-lookup"><span data-stu-id="2a310-116">Service Fabric CLI</span></span>](service-fabric-cli.md)

* <span data-ttu-id="2a310-117">Egy Azure Container Registry-beállításjegyzék – ehhez [hozzon létre egy tároló-beállításjegyzéket](../container-registry/container-registry-get-started-portal.md) Azure-előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="2a310-117">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span> 

## <a name="define-hello-docker-container"></a><span data-ttu-id="2a310-118">Hello Docker-tároló megadása</span><span class="sxs-lookup"><span data-stu-id="2a310-118">Define hello Docker container</span></span>
<span data-ttu-id="2a310-119">Build hello alapuló rendszerképet [Python kép](https://hub.docker.com/_/python/) Docker hub található.</span><span class="sxs-lookup"><span data-stu-id="2a310-119">Build an image based on hello [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span> 

<span data-ttu-id="2a310-120">Definiálja a Docker-tárolót egy Docker-fájlban.</span><span class="sxs-lookup"><span data-stu-id="2a310-120">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="2a310-121">hello Dockerfile belül a tároló hello környezet létrehozása, azt szeretné, hogy toorun hello alkalmazásokat és portok leképezési kapcsolatos utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2a310-121">hello Dockerfile contains instructions for setting up hello environment inside your container, loading hello application you want toorun, and mapping ports.</span></span> <span data-ttu-id="2a310-122">hello Dockerfile hello bemeneti toohello `docker build` parancsot, amely hello lemezképet.</span><span class="sxs-lookup"><span data-stu-id="2a310-122">hello Dockerfile is hello input toohello `docker build` command, which creates hello image.</span></span> 

<span data-ttu-id="2a310-123">Hozzon létre egy üres könyvtárat, és hozzon létre hello fájl *Dockerfile* (kiterjesztésű nem).</span><span class="sxs-lookup"><span data-stu-id="2a310-123">Create an empty directory and create hello file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="2a310-124">Adja hozzá a hello túl a következő*Dockerfile* és mentse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="2a310-124">Add hello following too*Dockerfile* and save your changes:</span></span>

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

<span data-ttu-id="2a310-125">Olvasási hello [Dockerfile hivatkozás](https://docs.docker.com/engine/reference/builder/) további információt.</span><span class="sxs-lookup"><span data-stu-id="2a310-125">Read hello [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="2a310-126">Egyszerű webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2a310-126">Create a simple web application</span></span>
<span data-ttu-id="2a310-127">Hozzon létre egy olyan Flask-webalkalmazást, amely a 80-as portot figyeli, és a „Hello World!” szöveget adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2a310-127">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="2a310-128">A hello ugyanabban a könyvtárban, és hozzon létre hello fájl *requirements.txt*.</span><span class="sxs-lookup"><span data-stu-id="2a310-128">In hello same directory, create hello file *requirements.txt*.</span></span>  <span data-ttu-id="2a310-129">Adja hozzá a következő hello, és mentse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="2a310-129">Add hello following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="2a310-130">Hello is létrehozhat *app.py* fájlt, és adja hozzá a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a310-130">Also create hello *app.py* file and add hello following:</span></span>

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-hello-image"></a><span data-ttu-id="2a310-131">Hello lemezkép</span><span class="sxs-lookup"><span data-stu-id="2a310-131">Build hello image</span></span>
<span data-ttu-id="2a310-132">Futtassa a hello `docker build` , amelyen a webalkalmazás toocreate hello képe.</span><span class="sxs-lookup"><span data-stu-id="2a310-132">Run hello `docker build` command toocreate hello image that runs your web application.</span></span> <span data-ttu-id="2a310-133">Nyisson meg egy PowerShell-ablakot, és keresse meg a túl*c:\temp\helloworldapp*.</span><span class="sxs-lookup"><span data-stu-id="2a310-133">Open a PowerShell window and navigate too*c:\temp\helloworldapp*.</span></span> <span data-ttu-id="2a310-134">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2a310-134">Run hello following command:</span></span>

```bash
docker build -t helloworldapp .
```

<span data-ttu-id="2a310-135">Ez a parancs buildek hello hello utasítások használatát a Dockerfile új lemezkép elnevezési (-t címkézés) hello kép "helloworldapp".</span><span class="sxs-lookup"><span data-stu-id="2a310-135">This command builds hello new image using hello instructions in your Dockerfile, naming (-t tagging) hello image "helloworldapp".</span></span> <span data-ttu-id="2a310-136">Lemezkép létrehozása hello alapjául szolgáló lemezképhez kérjen az Docker Hub, és létrehoz egy új lemezképet, amely az alkalmazás hello alapjául szolgáló lemezképhez felett.</span><span class="sxs-lookup"><span data-stu-id="2a310-136">Building an image pulls hello base image down from Docker Hub and creates a new image that adds your application on top of hello base image.</span></span>  

<span data-ttu-id="2a310-137">Miután hello build parancs végrehajtása után futtassa az hello `docker images` hello új lemezkép toosee vonatkozó parancsot:</span><span class="sxs-lookup"><span data-stu-id="2a310-137">Once hello build command completes, run hello `docker images` command toosee information on hello new image:</span></span>

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-hello-application-locally"></a><span data-ttu-id="2a310-138">Hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="2a310-138">Run hello application locally</span></span>
<span data-ttu-id="2a310-139">Győződjön meg arról, hogy a tárolóalapú alkalmazás helyileg előtt azt hello tároló beállításjegyzék fut-e.</span><span class="sxs-lookup"><span data-stu-id="2a310-139">Verify that your containerized application runs locally before pushing it hello container registry.</span></span>  

<span data-ttu-id="2a310-140">Hello alkalmazás futtatásához, a számítógép 4000 portra toohello tároló leképezési kitett 80-as port:</span><span class="sxs-lookup"><span data-stu-id="2a310-140">Run hello application, mapping your computer's port 4000 toohello container's exposed port 80:</span></span>

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

<span data-ttu-id="2a310-141">*név* biztosít a tárolóhoz (és nem hello tárolóhely-azonosító) futtató neve toohello.</span><span class="sxs-lookup"><span data-stu-id="2a310-141">*name* gives a name toohello running container (instead of hello container ID).</span></span>

<span data-ttu-id="2a310-142">Csatlakoztassa a tárolóban futó toohello.</span><span class="sxs-lookup"><span data-stu-id="2a310-142">Connect toohello running container.</span></span>  <span data-ttu-id="2a310-143">Nyisson meg egy webböngészőt, toohello IP-címet adja vissza a port 4000, például "http://localhost:4000" mutat.</span><span class="sxs-lookup"><span data-stu-id="2a310-143">Open a web browser pointing toohello IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="2a310-144">Láthatja a "Hello World!" fejléc hello</span><span class="sxs-lookup"><span data-stu-id="2a310-144">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="2a310-145">hello böngészőben megjelenő.</span><span class="sxs-lookup"><span data-stu-id="2a310-145">display in hello browser.</span></span>

![Hello World!][hello-world]

<span data-ttu-id="2a310-147">toostop a tárolóhoz, futtassa:</span><span class="sxs-lookup"><span data-stu-id="2a310-147">toostop your container, run:</span></span>

```bash
docker stop my-web-site
```

<span data-ttu-id="2a310-148">Hello tároló törlése a fejlesztési számítógépén:</span><span class="sxs-lookup"><span data-stu-id="2a310-148">Delete hello container from your development machine:</span></span>

```bash
docker rm my-web-site
```

## <a name="push-hello-image-toohello-container-registry"></a><span data-ttu-id="2a310-149">Leküldéses hello kép toohello tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="2a310-149">Push hello image toohello container registry</span></span>
<span data-ttu-id="2a310-150">Miután ellenőrizte, hogy hello alkalmazás a Docker fut-e, leküldéses hello kép tooyour beállításjegyzék Azure tároló beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="2a310-150">After you verify that hello application runs in Docker, push hello image tooyour registry in Azure Container Registry.</span></span>

<span data-ttu-id="2a310-151">Futtatás `docker login` tooyour tároló beállításjegyzék rendelkező toolog a [beállításjegyzék hitelesítő adatok](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="2a310-151">Run `docker login` toolog in tooyour container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="2a310-152">hello alábbi példa továbbítja hello Azonosítót és jelszót egy Azure Active Directory [egyszerű](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="2a310-152">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="2a310-153">Például előfordulhat, hogy rendelt hozzá egy szolgáltatás egyszerű tooyour beállításjegyzék az automation-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="2a310-153">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span>  <span data-ttu-id="2a310-154">Vagy bejelentkezhet a beállításjegyzékhez tartozó felhasználónevével és jelszavával.</span><span class="sxs-lookup"><span data-stu-id="2a310-154">Or, you could login using your registry username and password.</span></span>

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="2a310-155">hello következő parancs létrehoz egy címkét, vagy egy aliast hello kép, a teljes elérési útja tooyour beállításjegyzékbeli.</span><span class="sxs-lookup"><span data-stu-id="2a310-155">hello following command creates a tag, or alias, of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="2a310-156">Ez a Példa helyek hello hello lemezképet `samples` névtér tooavoid zsúfoltságát hello beállításjegyzék hello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="2a310-156">This example places hello image in hello `samples` namespace tooavoid clutter in hello root of hello registry.</span></span>

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="2a310-157">Leküldéses hello kép tooyour tároló beállításjegyzék:</span><span class="sxs-lookup"><span data-stu-id="2a310-157">Push hello image tooyour container registry:</span></span>

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-hello-docker-image-with-yeoman"></a><span data-ttu-id="2a310-158">Csomag hello Docker rendszerképének Yeoman</span><span class="sxs-lookup"><span data-stu-id="2a310-158">Package hello Docker image with Yeoman</span></span>
<span data-ttu-id="2a310-159">Service Fabric SDK Linux hello tartalmaz egy [Yeoman](http://yeoman.io/) , így könnyen toocreate generátor az alkalmazás, és adja hozzá a tároló-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="2a310-159">hello Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy toocreate your application and add a container image.</span></span> <span data-ttu-id="2a310-160">Most használja az alkalmazás egy Docker-tároló nevű Yeoman toocreate *SimpleContainerApp*.</span><span class="sxs-lookup"><span data-stu-id="2a310-160">Let's use Yeoman toocreate an application with a single Docker container called *SimpleContainerApp*.</span></span>

<span data-ttu-id="2a310-161">a Service Fabric-tároló alkalmazás toocreate nyisson meg egy terminálablakot, és futtassa `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="2a310-161">toocreate a Service Fabric container application, open a terminal window and run `yo azuresfcontainer`.</span></span>  

<span data-ttu-id="2a310-162">Adjon nevet az alkalmazásnak (például „mycontainer”).</span><span class="sxs-lookup"><span data-stu-id="2a310-162">Name your application (for example, "mycontainer").</span></span> 

<span data-ttu-id="2a310-163">Adja meg hello tároló lemezképének egy tároló beállításjegyzék hello URL-címe (például "").</span><span class="sxs-lookup"><span data-stu-id="2a310-163">Provide hello URL for hello container image in a container registry (for example, "").</span></span> 

<span data-ttu-id="2a310-164">Ez a rendszerkép rendelkezik egy munkaterhelés belépési pont definiálva, ezért kell tooexplicitly adja meg az bemeneti parancsot (a parancsok futhat hello tároló, amely közli a hello tároló indítás után fut).</span><span class="sxs-lookup"><span data-stu-id="2a310-164">This image has a workload entry-point defined, so need tooexplicitly specify input commands (commands run inside hello container, which will keep hello container running after startup).</span></span> 

<span data-ttu-id="2a310-165">Adja meg az „1” példányszámát.</span><span class="sxs-lookup"><span data-stu-id="2a310-165">Specify an instance count of "1".</span></span>

![Tárolókhoz készült Service Fabric Yeoman-generátor][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a><span data-ttu-id="2a310-167">A porthozzárendelés és a tárolóadattár hitelesítésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2a310-167">Configure port mapping and container repository authentication</span></span>
<span data-ttu-id="2a310-168">A tárolóalapú szolgáltatáshoz szükség van egy kommunikációs végpontra.</span><span class="sxs-lookup"><span data-stu-id="2a310-168">Your containerized service needs an endpoint for communication.</span></span>  <span data-ttu-id="2a310-169">Ezután adja hozzá a hello protokoll, port és típus tooan `Endpoint` hello ServiceManifest.xml fájlban.</span><span class="sxs-lookup"><span data-stu-id="2a310-169">Now add hello protocol, port, and type tooan `Endpoint` in hello ServiceManifest.xml file.</span></span> <span data-ttu-id="2a310-170">Ez a cikk indexelése hello szolgáltatást figyel 4000 portra:</span><span class="sxs-lookup"><span data-stu-id="2a310-170">For this article, hello containerized service listens on port 4000:</span></span> 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
<span data-ttu-id="2a310-171">Hello biztosító `UriScheme` automatikusan regiszterekben hello-tároló végpont hello Service Fabric-szolgáltatás felderítése a.</span><span class="sxs-lookup"><span data-stu-id="2a310-171">Providing hello `UriScheme` automatically registers hello container endpoint with hello Service Fabric Naming service for discoverability.</span></span> <span data-ttu-id="2a310-172">Ez a cikk végén hello ServiceManifest.xml teljes példa fájl valósul meg.</span><span class="sxs-lookup"><span data-stu-id="2a310-172">A full ServiceManifest.xml example file is provided at hello end of this article.</span></span> 

<span data-ttu-id="2a310-173">Konfigurálja az hello tárolóportot állomásport leképezés meghatározásával egy `PortBinding` házirend `ContainerHostPolicies` hello ApplicationManifest.xml fájl.</span><span class="sxs-lookup"><span data-stu-id="2a310-173">Configure hello container port-to-host port mapping by specifying a `PortBinding` policy in `ContainerHostPolicies` of hello ApplicationManifest.xml file.</span></span>  <span data-ttu-id="2a310-174">Ebben a cikkben `ContainerPort` 80 (hello tároló mutatja, 80-as port megadott hello Dockerfile) és `EndpointRef` "myserviceTypeEndpoint" (hello szolgáltatás jegyzékben meghatározott hello végpont) van.</span><span class="sxs-lookup"><span data-stu-id="2a310-174">For this article, `ContainerPort` is 80 (hello container exposes port 80, as specified in hello Dockerfile) and `EndpointRef` is "myserviceTypeEndpoint" (hello endpoint defined in hello service manifest).</span></span>  <span data-ttu-id="2a310-175">4000 portra toohello szolgáltatás bejövő kérelmek tooport hello tárolóra 80 képezi le.</span><span class="sxs-lookup"><span data-stu-id="2a310-175">Incoming requests toohello service on port 4000 are mapped tooport 80 on hello container.</span></span>  <span data-ttu-id="2a310-176">Ha a tároló tooauthenticate a személyes tára, majd adja hozzá `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="2a310-176">If your container needs tooauthenticate with a private repository, then add `RepositoryCredentials`.</span></span>  <span data-ttu-id="2a310-177">Ez a cikk vegye fel az hello fióknevet és jelszót hello myregistry.azurecr.io tároló rendszerleíró adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="2a310-177">For this article, add hello account name and password for hello myregistry.azurecr.io container registry.</span></span> 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-hello-service-fabric-application"></a><span data-ttu-id="2a310-178">Buildelés és a csomag hello Service Fabric-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2a310-178">Build and package hello Service Fabric application</span></span>
<span data-ttu-id="2a310-179">hello Service Fabric Yeoman-sablonjai tartalmazzák a build parancsfájl [Gradle](https://gradle.org/), mely terminál hello toobuild hello alkalmazást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2a310-179">hello Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use toobuild hello application from hello terminal.</span></span> <span data-ttu-id="2a310-180">toobuild és a csomag hello alkalmazás, futtassa a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a310-180">toobuild and package hello application, run hello following:</span></span>

```bash
cd mycontainer
gradle
```

## <a name="deploy-hello-application"></a><span data-ttu-id="2a310-181">Hello alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="2a310-181">Deploy hello application</span></span>
<span data-ttu-id="2a310-182">Miután hello alkalmazás épül, ezután telepítheti azt toohello helyi fürt hello Service Fabric parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="2a310-182">Once hello application is built, you can deploy it toohello local cluster using hello Service Fabric CLI.</span></span>

<span data-ttu-id="2a310-183">Csatlakozás helyi Service Fabric-fürt toohello.</span><span class="sxs-lookup"><span data-stu-id="2a310-183">Connect toohello local Service Fabric cluster.</span></span>

```bash
sfctl cluster select --endpoint http://localhost:19080
```

<span data-ttu-id="2a310-184">Használjon hello telepítési parancsfájlját hello sablon toocopy hello alkalmazás csomag toohello fürt lemezképtárolóhoz, hello alkalmazástípus regisztrálása, és a hello alkalmazás példányt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2a310-184">Use hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

```bash
./install.sh
```

<span data-ttu-id="2a310-185">Nyisson meg egy böngészőt, és keresse meg a Fabric Explorer tooService: 19080/Explorer (a név felülírandó localhost a hello privát IP-címe hello VM Vagrant használatakor a Mac OS x).</span><span class="sxs-lookup"><span data-stu-id="2a310-185">Open a browser and navigate tooService Fabric Explorer at http://localhost:19080/Explorer (replace localhost with hello private IP of hello VM if using Vagrant on Mac OS X).</span></span> <span data-ttu-id="2a310-186">Hello alkalmazások csomópontot, és vegye figyelembe, hogy most egy bejegyzést, az alkalmazás típusának és egy másikat a hello adott típus első példányát.</span><span class="sxs-lookup"><span data-stu-id="2a310-186">Expand hello Applications node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

<span data-ttu-id="2a310-187">Csatlakoztassa a tárolóban futó toohello.</span><span class="sxs-lookup"><span data-stu-id="2a310-187">Connect toohello running container.</span></span>  <span data-ttu-id="2a310-188">Nyisson meg egy webböngészőt, toohello IP-címet adja vissza a port 4000, például "http://localhost:4000" mutat.</span><span class="sxs-lookup"><span data-stu-id="2a310-188">Open a web browser pointing toohello IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="2a310-189">Láthatja a "Hello World!" fejléc hello</span><span class="sxs-lookup"><span data-stu-id="2a310-189">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="2a310-190">hello böngészőben megjelenő.</span><span class="sxs-lookup"><span data-stu-id="2a310-190">display in hello browser.</span></span>

![Hello World!][hello-world]

## <a name="clean-up"></a><span data-ttu-id="2a310-192">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="2a310-192">Clean up</span></span>
<span data-ttu-id="2a310-193">Hello eltávolítás parancsfájl használata hello sablon toodelete hello alkalmazáspéldány hello helyi fejlesztési fürtök és a hello alkalmazástípus regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="2a310-193">Use hello uninstall script provided in hello template toodelete hello application instance from hello local development cluster and unregister hello application type.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="2a310-194">Miután hello kép toohello tároló beállításjegyzék leküldéses hello kép: helyi törölheti a fejlesztési számítógépen:</span><span class="sxs-lookup"><span data-stu-id="2a310-194">After you push hello image toohello container registry you can delete hello local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="2a310-195">Példa teljes Service Fabric-alkalmazásra és szolgáltatásjegyzékre</span><span class="sxs-lookup"><span data-stu-id="2a310-195">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="2a310-196">Az alábbiakban hello teljes szolgáltatási és az alkalmazásjegyzékeknek a cikk ezt használja.</span><span class="sxs-lookup"><span data-stu-id="2a310-196">Here are hello complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="2a310-197">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="2a310-197">ServiceManifest.xml</span></span>
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
### <a name="applicationmanifestxml"></a><span data-ttu-id="2a310-198">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="2a310-198">ApplicationManifest.xml</span></span>
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
## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="2a310-199">További szolgáltatások tooan meglévő alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a310-199">Adding more services tooan existing application</span></span>

<span data-ttu-id="2a310-200">tooadd egy másik tároló szolgáltatás tooan yeoman, már létrehozott alkalmazás végrehajtani hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2a310-200">tooadd another container service tooan application already created using yeoman, perform hello following steps:</span></span>

1. <span data-ttu-id="2a310-201">Hello meglévő alkalmazás gyökérkönyvtárának toohello módosítása.</span><span class="sxs-lookup"><span data-stu-id="2a310-201">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="2a310-202">Például `cd ~/YeomanSamples/MyApplication`, ha `MyApplication` Yeoman által létrehozott hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2a310-202">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="2a310-203">Futtassa a `yo azuresfcontainer:AddService` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2a310-203">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>


## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="2a310-204">A tároló kényszerített leállítását megelőző időköz beállítása</span><span class="sxs-lookup"><span data-stu-id="2a310-204">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="2a310-205">Hello futásidejű toowait időintervallum hello tároló eltávolítása hello szolgáltatás törlése (vagy áthelyezés tooanother csomópont) megkezdése után konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="2a310-205">You can configure a time interval for hello runtime toowait before hello container is removed after hello service deletion (or a move tooanother node) has started.</span></span> <span data-ttu-id="2a310-206">Konfigurálás hello alatt az időtartam alatt küldi hello `docker stop <time in seconds>` parancs toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="2a310-206">Configuring hello time interval sends hello `docker stop <time in seconds>` command toohello container.</span></span>   <span data-ttu-id="2a310-207">További információ: [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="2a310-207">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="2a310-208">hello idő időköz toowait hello szakaszban megadott `Hosting` szakasz.</span><span class="sxs-lookup"><span data-stu-id="2a310-208">hello time interval toowait is specified under hello `Hosting` section.</span></span> <span data-ttu-id="2a310-209">a következő fürt jegyzékének részlet hello bemutatja, hogyan tooset hello várakozás időtartama:</span><span class="sxs-lookup"><span data-stu-id="2a310-209">hello following cluster manifest snippet shows how tooset hello wait interval:</span></span>

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
<span data-ttu-id="2a310-210">hello alapértelmezett időtartam értéke too10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="2a310-210">hello default time interval is set too10 seconds.</span></span> <span data-ttu-id="2a310-211">Mivel ez a konfiguráció a dinamikus, a konfiguráció csak hello fürt frissítések hello időtúllépés frissíti.</span><span class="sxs-lookup"><span data-stu-id="2a310-211">Since this configuration is dynamic, a config only upgrade on hello cluster updates hello timeout.</span></span> 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a><span data-ttu-id="2a310-212">Hello futásidejű tooremove konfigurálása nem használt tároló lemezképek</span><span class="sxs-lookup"><span data-stu-id="2a310-212">Configure hello runtime tooremove unused container images</span></span>

<span data-ttu-id="2a310-213">Konfigurálhatja az Service Fabric-fürt tooremove hello nem használt tároló képek hello csomópontból.</span><span class="sxs-lookup"><span data-stu-id="2a310-213">You can configure hello Service Fabric cluster tooremove unused container images from hello node.</span></span> <span data-ttu-id="2a310-214">Ez a konfiguráció lehetővé teszi, hogy a szabad terület toobe újrarögzítése, ha túl sok tároló képek hello csomóponton találhatók.</span><span class="sxs-lookup"><span data-stu-id="2a310-214">This configuration allows disk space toobe recaptured if too many container images are present on hello node.</span></span>  <span data-ttu-id="2a310-215">tooenable ezt a funkciót, a frissítés hello `Hosting` szakasz hello a fürtjegyzékben, ahogy az alábbi részlet hello:</span><span class="sxs-lookup"><span data-stu-id="2a310-215">tooenable this feature, update hello `Hosting` section in hello cluster manifest as shown in hello following snippet:</span></span> 


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

<span data-ttu-id="2a310-216">Nem törölhetők, képek, megadhatja azokat a hello `ContainerImagesToSkip` paraméter.</span><span class="sxs-lookup"><span data-stu-id="2a310-216">For images that should not be deleted, you can specify them under hello `ContainerImagesToSkip` parameter.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="2a310-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a310-217">Next steps</span></span>
* <span data-ttu-id="2a310-218">További információk a [tárolók futtatásáról a Service Fabricban](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2a310-218">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="2a310-219">Olvasási hello [tároló egy .NET-alkalmazás központi telepítése](service-fabric-host-app-in-a-container.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="2a310-219">Read hello [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="2a310-220">További tudnivalók a Service Fabric hello [alkalmazás-életciklus](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="2a310-220">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="2a310-221">Kivételt hello [Service Fabric tároló mintakódok](https://github.com/Azure-Samples/service-fabric-dotnet-containers) a Githubon.</span><span class="sxs-lookup"><span data-stu-id="2a310-221">Checkout hello [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
