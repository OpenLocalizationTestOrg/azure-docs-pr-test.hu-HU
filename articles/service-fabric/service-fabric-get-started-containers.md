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
# <a name="create-your-first-service-fabric-container-application-on-windows"></a><span data-ttu-id="1c4e6-104">Az első Service Fabric-tárolóalkalmazás létrehozása Windows rendszeren</span><span class="sxs-lookup"><span data-stu-id="1c4e6-104">Create your first Service Fabric container application on Windows</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1c4e6-105">Windows</span><span class="sxs-lookup"><span data-stu-id="1c4e6-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="1c4e6-106">Linux</span><span class="sxs-lookup"><span data-stu-id="1c4e6-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="1c4e6-107">Egy meglévő alkalmazást futtat egy Windows-tárolóban a Service Fabric-fürt nem igényel módosításokat tooyour alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-107">Running an existing application in a Windows container on a Service Fabric cluster doesn't require any changes tooyour application.</span></span> <span data-ttu-id="1c4e6-108">Ez a cikk végigvezeti a Python tartalmazó Docker lemezkép létrehozása [Flask](http://flask.pocoo.org/) webes alkalmazás és a Service Fabric-fürt tooa telepítését.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it tooa Service Fabric cluster.</span></span>  <span data-ttu-id="1c4e6-109">Emellett meg is fogja osztani a tárolóalapú alkalmazást az [Azure Container Registry](/azure/container-registry/) használatával.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="1c4e6-110">A cikk feltételezi, hogy rendelkezik a Docker használatára vonatkozó alapvető ismeretekkel.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="1c4e6-111">Megismerkedhet a Docker által olvasása hello [Docker áttekintése](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-111">You can learn about Docker by reading hello [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c4e6-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1c4e6-112">Prerequisites</span></span>
<span data-ttu-id="1c4e6-113">Egy fejlesztői számítógép, amelyen a következők futnak:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-113">A development computer running:</span></span>
* <span data-ttu-id="1c4e6-114">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="1c4e6-115">[Service Fabric SDK és -eszközök](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-115">[Service Fabric SDK and tools](service-fabric-get-started.md).</span></span>
*  <span data-ttu-id="1c4e6-116">Windows rendszerhez készült Docker.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-116">Docker for Windows.</span></span>  <span data-ttu-id="1c4e6-117">[A Docker CE for Windows (stable) letöltése](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-117">[Get Docker CE for Windows (stable)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span></span> <span data-ttu-id="1c4e6-118">Miután telepíti és indítja a Docker, kattintson a jobb gombbal a hello tálcai ikonja, és válassza ki **tooWindows tárolók kapcsoló**.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-118">After installing and starting Docker, right-click on hello tray icon and select **Switch tooWindows containers**.</span></span> <span data-ttu-id="1c4e6-119">Ez a Windows-alapú szükséges toorun Docker-lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-119">This is required toorun Docker images based on Windows.</span></span>

<span data-ttu-id="1c4e6-120">Egy Windows-fürt legalább három, Windows Server 2016 rendszerű, a Containerst futtató csomóponttal. Ehhez [hozzon létre egy fürtöt](service-fabric-cluster-creation-via-portal.md) vagy [próbálja ki ingyen a Service Fabricot](https://aka.ms/tryservicefabric).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-120">A Windows cluster with three or more nodes running on Windows Server 2016 with Containers- [Create a cluster](service-fabric-cluster-creation-via-portal.md) or [try Service Fabric for free](https://aka.ms/tryservicefabric).</span></span>

<span data-ttu-id="1c4e6-121">Egy Azure Container Registry-beállításjegyzék – ehhez [hozzon létre egy tároló-beállításjegyzéket](../container-registry/container-registry-get-started-portal.md) Azure-előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-121">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span>

## <a name="define-hello-docker-container"></a><span data-ttu-id="1c4e6-122">Hello Docker-tároló megadása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-122">Define hello Docker container</span></span>
<span data-ttu-id="1c4e6-123">Build hello alapuló rendszerképet [Python kép](https://hub.docker.com/_/python/) Docker hub található.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-123">Build an image based on hello [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span>

<span data-ttu-id="1c4e6-124">Definiálja a Docker-tárolót egy Docker-fájlban.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-124">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="1c4e6-125">hello Dockerfile belül a tároló hello környezet létrehozása, azt szeretné, hogy toorun hello alkalmazásokat és portok leképezési kapcsolatos utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-125">hello Dockerfile contains instructions for setting up hello environment inside your container, loading hello application you want toorun, and mapping ports.</span></span> <span data-ttu-id="1c4e6-126">hello Dockerfile hello bemeneti toohello `docker build` parancsot, amely hello lemezképet.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-126">hello Dockerfile is hello input toohello `docker build` command, which creates hello image.</span></span>

<span data-ttu-id="1c4e6-127">Hozzon létre egy üres könyvtárat, és hozzon létre hello fájl *Dockerfile* (kiterjesztésű nem).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-127">Create an empty directory and create hello file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="1c4e6-128">Adja hozzá a hello túl a következő*Dockerfile* és mentse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-128">Add hello following too*Dockerfile* and save your changes:</span></span>

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

<span data-ttu-id="1c4e6-129">Olvasási hello [Dockerfile hivatkozás](https://docs.docker.com/engine/reference/builder/) további információt.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-129">Read hello [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="1c4e6-130">Egyszerű webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-130">Create a simple web application</span></span>
<span data-ttu-id="1c4e6-131">Hozzon létre egy olyan Flask-webalkalmazást, amely a 80-as portot figyeli, és a „Hello World!” szöveget adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-131">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="1c4e6-132">A hello ugyanabban a könyvtárban, és hozzon létre hello fájl *requirements.txt*.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-132">In hello same directory, create hello file *requirements.txt*.</span></span>  <span data-ttu-id="1c4e6-133">Adja hozzá a következő hello, és mentse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-133">Add hello following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="1c4e6-134">Hello is létrehozhat *app.py* fájlt, és adja hozzá a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-134">Also create hello *app.py* file and add hello following:</span></span>

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
## <a name="build-hello-image"></a><span data-ttu-id="1c4e6-135">Hello lemezkép</span><span class="sxs-lookup"><span data-stu-id="1c4e6-135">Build hello image</span></span>
<span data-ttu-id="1c4e6-136">Futtassa a hello `docker build` , amelyen a webalkalmazás toocreate hello képe.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-136">Run hello `docker build` command toocreate hello image that runs your web application.</span></span> <span data-ttu-id="1c4e6-137">Nyisson meg egy PowerShell-ablakot, és keresse meg a hello Dockerfile tartalmazó toohello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-137">Open a PowerShell window and navigate toohello directory containing hello Dockerfile.</span></span> <span data-ttu-id="1c4e6-138">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-138">Run hello following command:</span></span>

```
docker build -t helloworldapp .
```

<span data-ttu-id="1c4e6-139">Ez a parancs buildek hello hello utasítások használatát a Dockerfile új lemezkép elnevezési (-t címkézés) hello kép "helloworldapp".</span><span class="sxs-lookup"><span data-stu-id="1c4e6-139">This command builds hello new image using hello instructions in your Dockerfile, naming (-t tagging) hello image "helloworldapp".</span></span> <span data-ttu-id="1c4e6-140">Lemezkép létrehozása hello alapjául szolgáló lemezképhez kérjen az Docker Hub, és létrehoz egy új lemezképet, amely az alkalmazás hello alapjául szolgáló lemezképhez felett.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-140">Building an image pulls hello base image down from Docker Hub and creates a new image that adds your application on top of hello base image.</span></span>  

<span data-ttu-id="1c4e6-141">Miután hello build parancs végrehajtása után futtassa az hello `docker images` hello új lemezkép toosee vonatkozó parancsot:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-141">Once hello build command completes, run hello `docker images` command toosee information on hello new image:</span></span>

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-hello-application-locally"></a><span data-ttu-id="1c4e6-142">Hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-142">Run hello application locally</span></span>
<span data-ttu-id="1c4e6-143">Ellenőrizze a lemezkép helyileg előtt azt hello tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-143">Verify your image locally before pushing it hello container registry.</span></span>  

<span data-ttu-id="1c4e6-144">Hello alkalmazás futtatásához:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-144">Run hello application:</span></span>

```
docker run -d --name my-web-site helloworldapp
```

<span data-ttu-id="1c4e6-145">*név* biztosít a tárolóhoz (és nem hello tárolóhely-azonosító) futtató neve toohello.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-145">*name* gives a name toohello running container (instead of hello container ID).</span></span>

<span data-ttu-id="1c4e6-146">Hello tároló elindul, ha az IP-címének, hogy a tárolóban futó böngészővel tooyour csatlakozáskor:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-146">Once hello container starts, find its IP address so that you can connect tooyour running container from a browser:</span></span>
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

<span data-ttu-id="1c4e6-147">Csatlakoztassa a tárolóban futó toohello.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-147">Connect toohello running container.</span></span>  <span data-ttu-id="1c4e6-148">Nyisson meg egy webböngészőt, toohello IP-címet ad vissza, például "http://172.31.194.61" mutat.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-148">Open a web browser pointing toohello IP address returned, for example "http://172.31.194.61".</span></span> <span data-ttu-id="1c4e6-149">Láthatja a "Hello World!" fejléc hello</span><span class="sxs-lookup"><span data-stu-id="1c4e6-149">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="1c4e6-150">hello böngészőben megjelenő.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-150">display in hello browser.</span></span>

<span data-ttu-id="1c4e6-151">toostop a tárolóhoz, futtassa:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-151">toostop your container, run:</span></span>

```
docker stop my-web-site
```

<span data-ttu-id="1c4e6-152">Hello tároló törlése a fejlesztési számítógépén:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-152">Delete hello container from your development machine:</span></span>

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-hello-image-toohello-container-registry"></a><span data-ttu-id="1c4e6-153">Leküldéses hello kép toohello tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="1c4e6-153">Push hello image toohello container registry</span></span>
<span data-ttu-id="1c4e6-154">Miután meggyőződött a fejlesztési számítógépén fut hello tároló, leküldéses hello kép tooyour beállításjegyzék Azure tároló beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-154">After you verify that hello container runs on your development machine, push hello image tooyour registry in Azure Container Registry.</span></span>

<span data-ttu-id="1c4e6-155">Futtatás ``docker login`` tooyour tároló beállításjegyzék rendelkező toolog a [beállításjegyzék hitelesítő adatok](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-155">Run ``docker login`` toolog in tooyour container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="1c4e6-156">hello alábbi példa továbbítja hello Azonosítót és jelszót egy Azure Active Directory [egyszerű](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-156">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="1c4e6-157">Például előfordulhat, hogy rendelt hozzá egy szolgáltatás egyszerű tooyour beállításjegyzék az automation-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-157">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span> <span data-ttu-id="1c4e6-158">Vagy bejelentkezhet a beállításjegyzékhez tartozó felhasználónevével és jelszavával.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-158">Or, you could login using your registry username and password.</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="1c4e6-159">hello következő parancs létrehoz egy címkét, vagy egy aliast hello kép, a teljes elérési útja tooyour beállításjegyzékbeli.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-159">hello following command creates a tag, or alias, of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="1c4e6-160">Ez a Példa helyek hello hello lemezképet ```samples``` névtér tooavoid zsúfoltságát hello beállításjegyzék hello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-160">This example places hello image in hello ```samples``` namespace tooavoid clutter in hello root of hello registry.</span></span>

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="1c4e6-161">Leküldéses hello kép tooyour tároló beállításjegyzék:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-161">Push hello image tooyour container registry:</span></span>

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-hello-containerized-service-in-visual-studio"></a><span data-ttu-id="1c4e6-162">A Visual Studio indexelése hello szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-162">Create hello containerized service in Visual Studio</span></span>
<span data-ttu-id="1c4e6-163">hello Service Fabric SDK és az eszközök adja meg a szolgáltatási sablon toohelp indexelése-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-163">hello Service Fabric SDK and tools provide a service template toohelp you create a containerized application.</span></span>

1. <span data-ttu-id="1c4e6-164">Indítsa el a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-164">Start Visual Studio.</span></span>  <span data-ttu-id="1c4e6-165">Válassza a **File** (Fájl) > **New** (Új) > **Project** (Projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-165">Select **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="1c4e6-166">Válassza a **Service Fabric application** (Service Fabric-alkalmazás) lehetőséget, nevezze el „MyFirstContainer” néven, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-166">Select **Service Fabric application**, name it "MyFirstContainer", and click **OK**.</span></span>
3. <span data-ttu-id="1c4e6-167">Válassza ki **Vendég tároló** hello listája **szolgáltatássablonok**.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-167">Select **Guest Container** from hello list of **service templates**.</span></span>
4. <span data-ttu-id="1c4e6-168">A **Lemezképnév** adja meg a "myregistry.azurecr.io/samples/helloworldapp", akkor leküldött tooyour tároló tárház hello kép.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-168">In **Image Name** enter "myregistry.azurecr.io/samples/helloworldapp", hello image you pushed tooyour container repository.</span></span>
5. <span data-ttu-id="1c4e6-169">Nevezze el a szolgáltatást, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-169">Give your service a name, and click **OK**.</span></span>

## <a name="configure-communication"></a><span data-ttu-id="1c4e6-170">A kommunikáció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-170">Configure communication</span></span>
<span data-ttu-id="1c4e6-171">indexelése hello szolgáltatást kell a végpont-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-171">hello containerized service needs an endpoint for communication.</span></span> <span data-ttu-id="1c4e6-172">Adja hozzá egy `Endpoint` elem hello protokoll, port és típus toohello ServiceManifest.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-172">Add an `Endpoint` element with hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="1c4e6-173">Ez a cikk indexelése hello szolgáltatás 8081 porton figyel.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-173">For this article, hello containerized service listens on port 8081.</span></span>  <span data-ttu-id="1c4e6-174">Ez a példa egy rögzített 8081-es portot használ erre a célra.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-174">In this example, a fixed port 8081 is used.</span></span>  <span data-ttu-id="1c4e6-175">Ha nincs port meg van adva, az alkalmazás porttartományát hello véletlenszerű port van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-175">If no port is specified, a random port from hello application port range is chosen.</span></span>  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="1c4e6-176">Meghatározhat egy olyan végpont, a Service Fabric hello végpont toohello Naming service tesz közzé.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-176">By defining an endpoint, Service Fabric publishes hello endpoint toohello Naming service.</span></span>  <span data-ttu-id="1c4e6-177">Más hello fürtben futó szolgáltatások azt feloldva a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-177">Other services running in hello cluster can resolve this container.</span></span>  <span data-ttu-id="1c4e6-178">Tároló-tároló kommunikációs hello használatával is elvégezheti [fordított proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-178">You can also perform container-to-container communication using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span>  <span data-ttu-id="1c4e6-179">Kommunikációs hello fordított proxy HTTP-figyelő portja és hello szolgáltatások kiszolgálóként használni kívánt toocommunicate a környezeti változók neve hello megadásával történik.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-179">Communication is performed by providing hello reverse proxy HTTP listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="1c4e6-180">Környezeti változók konfigurálása és beállítása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-180">Configure and set environment variables</span></span>
<span data-ttu-id="1c4e6-181">Környezeti változók minden kódcsomag hello szolgáltatás jegyzékben adható meg.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-181">Environment variables can be specified for each code package in hello service manifest.</span></span> <span data-ttu-id="1c4e6-182">Ez a funkció az összes szolgáltatáshoz elérhető attól függetlenül, hogy tárolókként, folyamatokként vagy vendég futtatható fájlokként vannak-e üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-182">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="1c4e6-183">Ha szeretné felülbírálni az környezeti változó értékek hello alkalmazás jegyzékfájlja, vagy adja meg azokat az alkalmazás paraméterekként üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-183">You can override environment variable values in hello application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="1c4e6-184">hello service manifest következő XML-részletet szemlélteti, hogyan toospecify környezeti változók kód csomag:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-184">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

<span data-ttu-id="1c4e6-185">Ezek a környezeti változók hello alkalmazásjegyzékben bírálható felül:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-185">These environment variables can be overridden in hello application manifest:</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a><span data-ttu-id="1c4e6-186">Tárolóport–gazdagépport hozzárendelés és tároló–tároló felderítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-186">Configure container port-to-host port mapping and container-to-container discovery</span></span>
<span data-ttu-id="1c4e6-187">Konfigurálja a gazdagépen használt port toocommunicate hello tároló.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-187">Configure a host port used toocommunicate  with hello container.</span></span> <span data-ttu-id="1c4e6-188">hello port kötés leképezi a hello port mely hello szolgáltatást figyel tooa tárolóportot hello hello gazdagépen belül.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-188">hello port binding maps hello port on which hello service is listening inside hello container tooa port on hello host.</span></span> <span data-ttu-id="1c4e6-189">Adja hozzá a `PortBinding` elemében `ContainerHostPolicies` hello ApplicationManifest.xml fájl eleme.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-189">Add a `PortBinding` element in `ContainerHostPolicies` element of hello ApplicationManifest.xml file.</span></span>  <span data-ttu-id="1c4e6-190">Ebben a cikkben `ContainerPort` 80 (hello tároló mutatja, 80-as port megadott hello Dockerfile) és `EndpointRef` "Guest1TypeEndpoint" van (a korábban a hello szolgáltatás jegyzékben megadott végpont hello).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-190">For this article, `ContainerPort` is 80 (hello container exposes port 80, as specified in hello Dockerfile) and `EndpointRef` is "Guest1TypeEndpoint" (hello endpoint previously defined in hello service manifest).</span></span>  <span data-ttu-id="1c4e6-191">A porton 8081 toohello szolgáltatás bejövő kérelmek tooport hello tárolóra 80 képezi le.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-191">Incoming requests toohello service on port 8081 are mapped tooport 80 on hello container.</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a><span data-ttu-id="1c4e6-192">Tárolóregisztrációs adatbázis hitelesítésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-192">Configure container registry authentication</span></span>
<span data-ttu-id="1c4e6-193">Tároló beállításjegyzék-hitelesítés konfigurálása hozzáadásával `RepositoryCredentials` túl`ContainerHostPolicies` hello ApplicationManifest.xml fájl.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-193">Configure container registry authentication by adding `RepositoryCredentials` too`ContainerHostPolicies` of hello ApplicationManifest.xml file.</span></span> <span data-ttu-id="1c4e6-194">Adja hozzá a hello fiókkal és jelszóval hello myregistry.azurecr.io tároló beállításjegyzék, amely lehetővé teszi a hello szolgáltatás toodownload hello tároló kép adattárból hello.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-194">Add hello account and password for hello myregistry.azurecr.io container registry, which allows hello service toodownload hello container image from hello repository.</span></span>

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

<span data-ttu-id="1c4e6-195">Azt javasoljuk, hogy hello tárház jelszó rejtjelezése tanúsítványban tooall hello fürt csomópontjaira telepített segítségével a titkosítást.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-195">We recommend that you encrypt hello repository password by using an encipherment certificate that's deployed tooall nodes of hello cluster.</span></span> <span data-ttu-id="1c4e6-196">A Service Fabric hello szolgáltatás csomag toohello fürt telepíti, hello rejtjelezése tanúsítvány esetén használt toodecrypt hello titkosított szöveg.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-196">When Service Fabric deploys hello service package toohello cluster, hello encipherment certificate is used toodecrypt hello cipher text.</span></span>  <span data-ttu-id="1c4e6-197">hello Invoke-ServiceFabricEncryptText parancsmag az használt toocreate hello titkosított szöveg hello jelszót, toohello ApplicationManifest.xml fájl kerül.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-197">hello Invoke-ServiceFabricEncryptText cmdlet is used toocreate hello cipher text for hello password, which is added toohello ApplicationManifest.xml file.</span></span>

<span data-ttu-id="1c4e6-198">hello következő parancsfájl egy új önaláírt tanúsítványt hoz létre, és exportálja azt tooa PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-198">hello following script creates a new self-signed certificate and exports it tooa PFX file.</span></span>  <span data-ttu-id="1c4e6-199">hello tanúsítvány egy meglévő kulcstároló importálja, és majd a Service Fabric-fürt toohello telepít.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-199">hello certificate is imported into an existing key vault and then deployed toohello Service Fabric cluster.</span></span>

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
<span data-ttu-id="1c4e6-200">Hello jelszó használatával hello titkosítása [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-200">Encrypt hello password using hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet.</span></span>

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

<span data-ttu-id="1c4e6-201">Hello jelszó cseréje hello titkosítási szövegre hello által visszaadott [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) parancsmag és `PasswordEncrypted` túl "true".</span><span class="sxs-lookup"><span data-stu-id="1c4e6-201">Replace hello password with hello cipher text returned by hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet and set `PasswordEncrypted` too"true".</span></span>

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

## <a name="configure-isolation-mode"></a><span data-ttu-id="1c4e6-202">Az elkülönítési mód konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-202">Configure isolation mode</span></span>
<span data-ttu-id="1c4e6-203">A Windows a tárolók két elkülönítési módját támogatja: a folyamatalapú és a Hyper-V módot.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-203">Windows supports two isolation modes for containers: process and Hyper-V.</span></span> <span data-ttu-id="1c4e6-204">A hello folyamatainak elkülönítési módjának futó összes hello tárolók hello ugyanazon gazdagép gép megosztás hello kernel hello gazdagéphez.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-204">With hello process isolation mode, all hello containers running on hello same host machine share hello kernel with hello host.</span></span> <span data-ttu-id="1c4e6-205">A Hyper-V hello elkülönítési üzemmódját hello kernelek elkülönítik minden Hyper-V tároló és a tároló-gazdagépen hello között.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-205">With hello Hyper-V isolation mode, hello kernels are isolated between each Hyper-V container and hello container host.</span></span> <span data-ttu-id="1c4e6-206">hello elkülönítési üzemmódját megadott hello `ContainerHostPolicies` hello Alkalmazásjegyzék-fájl elemében.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-206">hello isolation mode is specified in hello `ContainerHostPolicies` element in hello application manifest file.</span></span> <span data-ttu-id="1c4e6-207">hello elkülönítési módok megadható `process`, `hyperv`, és `default`.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-207">hello isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="1c4e6-208">hello alapértelmezett elkülönítési üzemmódját alapértelmezés szerint használt érték túl`process` Windows Server rendszeren futtatja, és alapértelmezés szerint használt érték túl`hyperv` Windows 10-állomáson.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-208">hello default isolation mode defaults too`process` on Windows Server hosts, and defaults too`hyperv` on Windows 10 hosts.</span></span> <span data-ttu-id="1c4e6-209">hello alábbi kódrészletben láthatja, hogyan hello elkülönítési üzemmódját hello Alkalmazásjegyzék-fájl megadott.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-209">hello following snippet shows how hello isolation mode is specified in hello application manifest file.</span></span>

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a><span data-ttu-id="1c4e6-210">Az erőforrás-szabályozás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-210">Configure resource governance</span></span>
<span data-ttu-id="1c4e6-211">[Erőforrás-irányítás](service-fabric-resource-governance.md) erőforrásokat, amelyek tároló hello használható hello gazdagépen hello korlátozza.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-211">[Resource governance](service-fabric-resource-governance.md) restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="1c4e6-212">Hello `ResourceGovernancePolicy` , hello alkalmazásjegyzékben megadott eleme használt toodeclare erőforrás korlátai a szolgáltatáscsomagot a kódot.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-212">hello `ResourceGovernancePolicy` element, which is specified in hello application manifest, is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="1c4e6-213">Erőforrás-korlátozások állíthat be a következő erőforrások hello: memória, MemorySwap, CpuShares (CPU relatív súly), MemoryReservationInMB, BlkioWeight (BlockIO relatív súly).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-213">Resource limits can be set for hello following resources: Memory, MemorySwap, CpuShares (CPU relative weight), MemoryReservationInMB, BlkioWeight (BlockIO relative weight).</span></span>  <span data-ttu-id="1c4e6-214">Ebben a példában a service-csomag Guest1Pkg egy alapvető lekérdezi a hello fürtcsomóponton, ahol el van helyezve.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-214">In this example, service package Guest1Pkg gets one core on hello cluster nodes where it is placed.</span></span>  <span data-ttu-id="1c4e6-215">Memóriakorlátokat abszolút, így hello kódcsomag korlátozott too1024 MB memória (és a soft-garancia lefoglalása hello ugyanaz).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-215">Memory limits are absolute, so hello code package is limited too1024 MB of memory (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="1c4e6-216">Kód csomagok (tárolók és folyamatok) olyan nem tud tooallocate toodo kísérlet, és ezt a határt több memóriával, kevés a memória kivétel eredményez.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-216">Code packages (containers or processes) are not able tooallocate more memory than this limit, and attempting toodo so results in an out-of-memory exception.</span></span> <span data-ttu-id="1c4e6-217">A service-csomag összes kódot csomagok erőforrás korlátját kényszerítési toowork, a megadott memóriakorlátokat kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-217">For resource limit enforcement toowork, all code packages within a service package should have memory limits specified.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-hello-container-application"></a><span data-ttu-id="1c4e6-218">Hello tároló alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="1c4e6-218">Deploy hello container application</span></span>
<span data-ttu-id="1c4e6-219">A módosítások mentéséhez és hello alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-219">Save all your changes and build hello application.</span></span> <span data-ttu-id="1c4e6-220">toopublish az alkalmazás, kattintson a jobb gombbal a **MyFirstContainer** megoldáskezelő, és válasszon **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-220">toopublish your application, right-click on **MyFirstContainer** in Solution Explorer and select **Publish**.</span></span>

<span data-ttu-id="1c4e6-221">A **csatlakozási végpont**, adja meg a hello felügyeleti végpont hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-221">In **Connection Endpoint**, enter hello management endpoint for hello cluster.</span></span>  <span data-ttu-id="1c4e6-222">például a „containercluster.westus2.cloudapp.azure.com:19000” végpontot.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-222">For example, "containercluster.westus2.cloudapp.azure.com:19000".</span></span> <span data-ttu-id="1c4e6-223">Hello ügyfélkapcsolat található végpont hello áttekintése panelen a fürt a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-223">You can find hello client connection endpoint in hello Overview blade for your cluster in hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="1c4e6-224">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-224">Click **Publish**.</span></span>

<span data-ttu-id="1c4e6-225">A [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) egy webalapú eszköz az alkalmazások és csomópontok vizsgálatához és kezeléséhez a Service Fabric-fürtökben.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a web-based tool for inspecting and managing applications and nodes in a Service Fabric cluster.</span></span> <span data-ttu-id="1c4e6-226">Nyisson meg egy böngészőt, és lépjen toohttp://containercluster.westus2.cloudapp.azure.com:19080/Explorer /, és kövesse az alkalmazástelepítés hello.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-226">Open a browser and navigate toohttp://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ and follow hello application deployment.</span></span>  <span data-ttu-id="1c4e6-227">hello alkalmazás központi telepítését, de hiba állapotban van, amíg hello lemezkép letöltődik fürtcsomópontokon hello (ami eltarthat egy ideig, attól függően, hogy a kép mérete hello): ![hiba][1]</span><span class="sxs-lookup"><span data-stu-id="1c4e6-227">hello application deploys but is in an error state until hello image is downloaded on hello cluster nodes (which can take some time, depending on hello image size): ![Error][1]</span></span>

<span data-ttu-id="1c4e6-228">hello alkalmazás készen áll, ha a ```Ready``` állapota: ![kész][2]</span><span class="sxs-lookup"><span data-stu-id="1c4e6-228">hello application is ready when it's in ```Ready``` state: ![Ready][2]</span></span>

<span data-ttu-id="1c4e6-229">Nyisson meg egy böngészőt, és keresse meg a toohttp://containercluster.westus2.cloudapp.azure.com:8081.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-229">Open a browser and navigate toohttp://containercluster.westus2.cloudapp.azure.com:8081.</span></span> <span data-ttu-id="1c4e6-230">Láthatja a "Hello World!" fejléc hello</span><span class="sxs-lookup"><span data-stu-id="1c4e6-230">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="1c4e6-231">hello böngészőben megjelenő.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-231">display in hello browser.</span></span>

## <a name="clean-up"></a><span data-ttu-id="1c4e6-232">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-232">Clean up</span></span>
<span data-ttu-id="1c4e6-233">Tooincur díjak folytatni, amíg hello fürt fut, érdemes lehet [a fürt törlése](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-233">You continue tooincur charges while hello cluster is running, consider [deleting your cluster](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span></span>  <span data-ttu-id="1c4e6-234">A [nyilvános fürtök](http://tryazureservicefabric.westus.cloudapp.azure.com/) néhány óra múlva automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-234">[Party clusters](http://tryazureservicefabric.westus.cloudapp.azure.com/) are automatically deleted after a few hours.</span></span>

<span data-ttu-id="1c4e6-235">Miután hello kép toohello tároló beállításjegyzék leküldéses hello kép: helyi törölheti a fejlesztési számítógépen:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-235">After you push hello image toohello container registry you can delete hello local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="1c4e6-236">Példa teljes Service Fabric-alkalmazásra és szolgáltatásjegyzékre</span><span class="sxs-lookup"><span data-stu-id="1c4e6-236">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="1c4e6-237">Az alábbiakban hello teljes szolgáltatási és az alkalmazásjegyzékeknek a cikk ezt használja.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-237">Here are hello complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="1c4e6-238">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="1c4e6-238">ServiceManifest.xml</span></span>
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
### <a name="applicationmanifestxml"></a><span data-ttu-id="1c4e6-239">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="1c4e6-239">ApplicationManifest.xml</span></span>
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

## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="1c4e6-240">A tároló kényszerített leállítását megelőző időköz beállítása</span><span class="sxs-lookup"><span data-stu-id="1c4e6-240">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="1c4e6-241">Hello futásidejű toowait időintervallum hello tároló eltávolítása hello szolgáltatás törlése (vagy áthelyezés tooanother csomópont) megkezdése után konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-241">You can configure a time interval for hello runtime toowait before hello container is removed after hello service deletion (or a move tooanother node) has started.</span></span> <span data-ttu-id="1c4e6-242">Konfigurálás hello alatt az időtartam alatt küldi hello `docker stop <time in seconds>` parancs toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-242">Configuring hello time interval sends hello `docker stop <time in seconds>` command toohello container.</span></span>   <span data-ttu-id="1c4e6-243">További információ: [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-243">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="1c4e6-244">hello idő időköz toowait hello szakaszban megadott `Hosting` szakasz.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-244">hello time interval toowait is specified under hello `Hosting` section.</span></span> <span data-ttu-id="1c4e6-245">a következő fürt jegyzékének részlet hello bemutatja, hogyan tooset hello várakozás időtartama:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-245">hello following cluster manifest snippet shows how tooset hello wait interval:</span></span>

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
<span data-ttu-id="1c4e6-246">hello alapértelmezett időtartam értéke too10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-246">hello default time interval is set too10 seconds.</span></span> <span data-ttu-id="1c4e6-247">Mivel ez a konfiguráció a dinamikus, a konfiguráció csak hello fürt frissítések hello időtúllépés frissíti.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-247">Since this configuration is dynamic, a config only upgrade on hello cluster updates hello timeout.</span></span> 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a><span data-ttu-id="1c4e6-248">Hello futásidejű tooremove konfigurálása nem használt tároló lemezképek</span><span class="sxs-lookup"><span data-stu-id="1c4e6-248">Configure hello runtime tooremove unused container images</span></span>

<span data-ttu-id="1c4e6-249">Konfigurálhatja az Service Fabric-fürt tooremove hello nem használt tároló képek hello csomópontból.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-249">You can configure hello Service Fabric cluster tooremove unused container images from hello node.</span></span> <span data-ttu-id="1c4e6-250">Ez a konfiguráció lehetővé teszi, hogy a szabad terület toobe újrarögzítése, ha túl sok tároló képek hello csomóponton találhatók.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-250">This configuration allows disk space toobe recaptured if too many container images are present on hello node.</span></span>  <span data-ttu-id="1c4e6-251">tooenable ezt a funkciót, a frissítés hello `Hosting` szakasz hello a fürtjegyzékben, ahogy az alábbi részlet hello:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-251">tooenable this feature, update hello `Hosting` section in hello cluster manifest as shown in hello following snippet:</span></span> 


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

<span data-ttu-id="1c4e6-252">Nem törölhetők, képek, megadhatja azokat a hello `ContainerImagesToSkip` paraméter.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-252">For images that should not be deleted, you can specify them under hello `ContainerImagesToSkip` parameter.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="1c4e6-253">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1c4e6-253">Next steps</span></span>
* <span data-ttu-id="1c4e6-254">További információk a [tárolók futtatásáról a Service Fabricban](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-254">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="1c4e6-255">Olvasási hello [tároló egy .NET-alkalmazás központi telepítése](service-fabric-host-app-in-a-container.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-255">Read hello [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="1c4e6-256">További tudnivalók a Service Fabric hello [alkalmazás-életciklus](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="1c4e6-256">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="1c4e6-257">Kivételt hello [Service Fabric tároló mintakódok](https://github.com/Azure-Samples/service-fabric-dotnet-containers) a Githubon.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-257">Checkout hello [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
