---
title: "Azure Service Fabric-tárolóalkalmazás létrehozása Linuxon | Microsoft Docs"
description: "Hozza létre első saját, Linux-alapú tárolóalkalmazását az Azure Service Fabricban.  Az alkalmazással elkészíthet egy Docker-rendszerképet, amelyet leküldéssel továbbíthat egy tárolóregisztrációs adatbázisba, majd összeállíthat és üzembe helyezhet egy Service Fabric-tárolóalkalmazást."
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
ms.openlocfilehash: 8355478cb2fff3a63bc4a9b359ec8e2b132c80f6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-service-fabric-container-application-on-linux"></a><span data-ttu-id="1f6bd-104">Az első Service Fabric-tárolóalkalmazás létrehozása Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="1f6bd-104">Create your first Service Fabric container application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1f6bd-105">Windows</span><span class="sxs-lookup"><span data-stu-id="1f6bd-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="1f6bd-106">Linux</span><span class="sxs-lookup"><span data-stu-id="1f6bd-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="1f6bd-107">A meglévő alkalmazások Service Fabric-fürtökön lévő Linux-tárolókban való futtatásához nem szükséges módosítania az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-107">Running an existing application in a Linux container on a Service Fabric cluster doesn't require any changes to your application.</span></span> <span data-ttu-id="1f6bd-108">Ez a cikk ismerteti a Python [Flask](http://flask.pocoo.org/)-webalkalmazást tartalmazó Docker-rendszerképek létrehozását, illetve egy Service Fabric-fürtön való üzembe helyezését.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it to a Service Fabric cluster.</span></span>  <span data-ttu-id="1f6bd-109">Emellett meg is fogja osztani a tárolóalapú alkalmazást az [Azure Container Registry](/azure/container-registry/) használatával.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="1f6bd-110">A cikk feltételezi, hogy rendelkezik a Docker használatára vonatkozó alapvető ismeretekkel.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="1f6bd-111">A Docker megismeréséhez olvassa el a [Docker áttekintő ismertetését](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-111">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f6bd-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1f6bd-112">Prerequisites</span></span>
* <span data-ttu-id="1f6bd-113">Egy fejlesztői számítógép, amelyen a következők futnak:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-113">A development computer running:</span></span>
  * <span data-ttu-id="1f6bd-114">[Service Fabric SDK és -eszközök](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-114">[Service Fabric SDK and tools](service-fabric-get-started-linux.md).</span></span>
  * <span data-ttu-id="1f6bd-115">[Linuxhoz készült Docker CE](https://docs.docker.com/engine/installation/#prior-releases).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-115">[Docker CE for Linux](https://docs.docker.com/engine/installation/#prior-releases).</span></span> 
  * [<span data-ttu-id="1f6bd-116">Service Fabric parancssori felület</span><span class="sxs-lookup"><span data-stu-id="1f6bd-116">Service Fabric CLI</span></span>](service-fabric-cli.md)

* <span data-ttu-id="1f6bd-117">Egy Azure Container Registry-beállításjegyzék – ehhez [hozzon létre egy tároló-beállításjegyzéket](../container-registry/container-registry-get-started-portal.md) Azure-előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-117">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span> 

## <a name="define-the-docker-container"></a><span data-ttu-id="1f6bd-118">A Docker-tároló definiálása</span><span class="sxs-lookup"><span data-stu-id="1f6bd-118">Define the Docker container</span></span>
<span data-ttu-id="1f6bd-119">Állítson össze egy rendszerképet a Docker Hubban található [Python-rendszerkép](https://hub.docker.com/_/python/) alapján.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-119">Build an image based on the [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span> 

<span data-ttu-id="1f6bd-120">Definiálja a Docker-tárolót egy Docker-fájlban.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-120">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="1f6bd-121">A Docker-fájl tartalmazza a környezet tárolón belüli beállítására, a futtatni kívánt alkalmazás betöltésére és a portok hozzárendelésére vonatkozó utasításokat.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-121">The Dockerfile contains instructions for setting up the environment inside your container, loading the application you want to run, and mapping ports.</span></span> <span data-ttu-id="1f6bd-122">A Docker-fájl a `docker build` parancs bemenete, amely a rendszerképet létrehozza.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-122">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span> 

<span data-ttu-id="1f6bd-123">Hozzon létre egy üres könyvtárat és a *Docker-fájlt* (fájlkiterjesztés nélkül).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-123">Create an empty directory and create the file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="1f6bd-124">Adja hozzá a következőket a *Docker-fájlhoz*, és mentse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-124">Add the following to *Dockerfile* and save your changes:</span></span>

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

<span data-ttu-id="1f6bd-125">További információkért olvassa el a [Docker-fájlra vonatkozó referenciákat](https://docs.docker.com/engine/reference/builder/).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-125">Read the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="1f6bd-126">Egyszerű webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f6bd-126">Create a simple web application</span></span>
<span data-ttu-id="1f6bd-127">Hozzon létre egy olyan Flask-webalkalmazást, amely a 80-as portot figyeli, és a „Hello World!” szöveget adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-127">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="1f6bd-128">Ugyanebben a könyvtárban hozza létre a *requirements.txt* fájlt.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-128">In the same directory, create the file *requirements.txt*.</span></span>  <span data-ttu-id="1f6bd-129">Adja hozzá a következőket, és mentse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-129">Add the following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="1f6bd-130">Hozza létre az *app.py* fájlt, és adja hozzá a következőket:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-130">Also create the *app.py* file and add the following:</span></span>

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-the-image"></a><span data-ttu-id="1f6bd-131">Rendszerkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f6bd-131">Build the image</span></span>
<span data-ttu-id="1f6bd-132">Futtassa a(z) `docker build` parancsot a webalkalmazást futtató rendszerkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-132">Run the `docker build` command to create the image that runs your web application.</span></span> <span data-ttu-id="1f6bd-133">Nyisson meg egy PowerShell-ablakot, és lépjen a *c:\temp\helloworldapp* mappára.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-133">Open a PowerShell window and navigate to *c:\temp\helloworldapp*.</span></span> <span data-ttu-id="1f6bd-134">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-134">Run the following command:</span></span>

```bash
docker build -t helloworldapp .
```

<span data-ttu-id="1f6bd-135">Ez a parancs létrehozza az új rendszerképet a Docker-fájlban foglalt utasítások alapján, és elnevezi (-t címkézés) a rendszerképet „helloworldapp”-nak.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-135">This command builds the new image using the instructions in your Dockerfile, naming (-t tagging) the image "helloworldapp".</span></span> <span data-ttu-id="1f6bd-136">A rendszerképek készítése során a rendszer lekéri az alaprendszerképet a Docker Hubból, és létrehoz egy olyan új rendszerképet, amelyben az alkalmazás hozzá van adva az alaprendszerképhez.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-136">Building an image pulls the base image down from Docker Hub and creates a new image that adds your application on top of the base image.</span></span>  

<span data-ttu-id="1f6bd-137">Miután az összeállító parancs lefutott, futtassa a `docker images` parancsot az új rendszerkép információinak megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-137">Once the build command completes, run the `docker images` command to see information on the new image:</span></span>

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-the-application-locally"></a><span data-ttu-id="1f6bd-138">Az alkalmazás helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="1f6bd-138">Run the application locally</span></span>
<span data-ttu-id="1f6bd-139">Ellenőrizze a tárolóalapú alkalmazás helyi működését, mielőtt leküldené a tárolóregisztrációs adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-139">Verify that your containerized application runs locally before pushing it the container registry.</span></span>  

<span data-ttu-id="1f6bd-140">Futtassa az alkalmazást, amivel számítógép 4000-es portját a tároló elérhető 80-as portjához rendeli hozzá:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-140">Run the application, mapping your computer's port 4000 to the container's exposed port 80:</span></span>

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

<span data-ttu-id="1f6bd-141">A *name* nevet ad a futtató tárolónak (a tárolóazonosító helyett).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-141">*name* gives a name to the running container (instead of the container ID).</span></span>

<span data-ttu-id="1f6bd-142">Csatlakozzon a futó tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-142">Connect to the running container.</span></span>  <span data-ttu-id="1f6bd-143">Nyisson meg egy webböngészőt, majd a 4000-es porton visszaadott IP-címet, például http://localhost:4000 .</span><span class="sxs-lookup"><span data-stu-id="1f6bd-143">Open a web browser pointing to the IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="1f6bd-144">A „Hello World!” címsornak kell</span><span class="sxs-lookup"><span data-stu-id="1f6bd-144">You should see the heading "Hello World!"</span></span> <span data-ttu-id="1f6bd-145">megjelennie a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-145">display in the browser.</span></span>

![Hello World!][hello-world]

<span data-ttu-id="1f6bd-147">A tároló leállításához futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-147">To stop your container, run:</span></span>

```bash
docker stop my-web-site
```

<span data-ttu-id="1f6bd-148">Törölje a tárolót a fejlesztői gépről:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-148">Delete the container from your development machine:</span></span>

```bash
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a><span data-ttu-id="1f6bd-149">A rendszerkép leküldése a tároló-beállításjegyzékbe</span><span class="sxs-lookup"><span data-stu-id="1f6bd-149">Push the image to the container registry</span></span>
<span data-ttu-id="1f6bd-150">Miután ellenőrizte, hogy az alkalmazás fut-e a Dockerben, küldje le a rendszerképet a regisztrációs adatbázisba az Azure Container Registryben.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-150">After you verify that the application runs in Docker, push the image to your registry in Azure Container Registry.</span></span>

<span data-ttu-id="1f6bd-151">Futtassa a(z) `docker login` parancsot a tároló-beállításjegyzékbe való bejelentkezéshez a [beállításjegyzékhez tartozó hitelesítő adataival](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-151">Run `docker login` to log in to your container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="1f6bd-152">Az alábbi példában a rendszer egy Azure Active Directory [egyszerű szolgáltatás](../active-directory/active-directory-application-objects.md) azonosítóját és jelszavát adja át.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-152">The following example passes the ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="1f6bd-153">Például lehet, hogy hozzárendelt egy egyszerű szolgáltatást a beállításjegyzékhez egy automatizálási forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-153">For example, you might have assigned a service principal to your registry for an automation scenario.</span></span>  <span data-ttu-id="1f6bd-154">Vagy bejelentkezhet a beállításjegyzékhez tartozó felhasználónevével és jelszavával.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-154">Or, you could login using your registry username and password.</span></span>

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="1f6bd-155">A következő parancs létrehoz egy címkét vagy aliast a rendszerképről a beállításjegyzékre mutató teljes elérési úttal.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-155">The following command creates a tag, or alias, of the image, with a fully qualified path to your registry.</span></span> <span data-ttu-id="1f6bd-156">Az alábbi példa a rendszerképet a `samples` névtérben helyezi el, hogy ne legyen zsúfolt a beállításjegyzék gyökere.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-156">This example places the image in the `samples` namespace to avoid clutter in the root of the registry.</span></span>

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="1f6bd-157">Küldje le a rendszerképet tároló-beállításjegyzékbe:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-157">Push the image to your container registry:</span></span>

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-the-docker-image-with-yeoman"></a><span data-ttu-id="1f6bd-158">A Docker-rendszerkép becsomagolása a Yeomannal</span><span class="sxs-lookup"><span data-stu-id="1f6bd-158">Package the Docker image with Yeoman</span></span>
<span data-ttu-id="1f6bd-159">A Linux Service Fabric SDK tartalmaz egy [Yeoman](http://yeoman.io/)-generátort, amely megkönnyíti az alkalmazás létrehozását és egy tárolórendszerkép hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-159">The Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy to create your application and add a container image.</span></span> <span data-ttu-id="1f6bd-160">Hozzunk létre egy alkalmazást, amely egyetlen, *SimpleContainerApp* nevű Docker-tárolóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-160">Let's use Yeoman to create an application with a single Docker container called *SimpleContainerApp*.</span></span>

<span data-ttu-id="1f6bd-161">Service Fabric-tárolóalkalmazás létrehozásához nyisson meg egy terminálablakot, és futtassa a következőt: `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-161">To create a Service Fabric container application, open a terminal window and run `yo azuresfcontainer`.</span></span>  

<span data-ttu-id="1f6bd-162">Adjon nevet az alkalmazásnak (például „mycontainer”).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-162">Name your application (for example, "mycontainer").</span></span> 

<span data-ttu-id="1f6bd-163">Adja meg a tároló rendszerképéhez tartozó URL-címet egy tárolóregisztrációs adatbázisban (például „”).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-163">Provide the URL for the container image in a container registry (for example, "").</span></span> 

<span data-ttu-id="1f6bd-164">Ez a rendszerkép meghatározott számításifeladat-belépési ponttal rendelkezik, így a bemeneti parancsokat (tárolón belül futó parancsok, amelyek az indítás után biztosítják a tároló futtatását) explicit módon kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-164">This image has a workload entry-point defined, so need to explicitly specify input commands (commands run inside the container, which will keep the container running after startup).</span></span> 

<span data-ttu-id="1f6bd-165">Adja meg az „1” példányszámát.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-165">Specify an instance count of "1".</span></span>

![Tárolókhoz készült Service Fabric Yeoman-generátor][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a><span data-ttu-id="1f6bd-167">A porthozzárendelés és a tárolóadattár hitelesítésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1f6bd-167">Configure port mapping and container repository authentication</span></span>
<span data-ttu-id="1f6bd-168">A tárolóalapú szolgáltatáshoz szükség van egy kommunikációs végpontra.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-168">Your containerized service needs an endpoint for communication.</span></span>  <span data-ttu-id="1f6bd-169">Most hozzáadhatja a protokoll, a port és a típus adatait egy `Endpoint` objektumhoz a servicemanifest.xml fájlban.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-169">Now add the protocol, port, and type to an `Endpoint` in the ServiceManifest.xml file.</span></span> <span data-ttu-id="1f6bd-170">Ebben a cikkben a tárolóalapú szolgáltatás a 4000-es portot figyeli:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-170">For this article, the containerized service listens on port 4000:</span></span> 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
<span data-ttu-id="1f6bd-171">Az `UriScheme` megadásával a tároló végpontja automatikusan regisztrálva lesz a Service Fabric elnevezési szolgáltatásban, így felderíthető lesz.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-171">Providing the `UriScheme` automatically registers the container endpoint with the Service Fabric Naming service for discoverability.</span></span> <span data-ttu-id="1f6bd-172">A cikk végén talál egy például szolgáló teljes ServiceManifest.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-172">A full ServiceManifest.xml example file is provided at the end of this article.</span></span> 

<span data-ttu-id="1f6bd-173">A tárolóport-gazdagépport leképezés konfigurálásához adjon meg egy `PortBinding` szabályzatot az ApplicationManifest.xml fájl `ContainerHostPolicies` elemében.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-173">Configure the container port-to-host port mapping by specifying a `PortBinding` policy in `ContainerHostPolicies` of the ApplicationManifest.xml file.</span></span>  <span data-ttu-id="1f6bd-174">Ebben a cikkben a(z) `ContainerPort` értéke 80 (a tároló a 80-as portot használja a Docker-fájlban foglalt beállítások szerint), az `EndpointRef` pedig „myserviceTypeEndpoint” (a szolgáltatásjegyzékben definiált végpont).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-174">For this article, `ContainerPort` is 80 (the container exposes port 80, as specified in the Dockerfile) and `EndpointRef` is "myserviceTypeEndpoint" (the endpoint defined in the service manifest).</span></span>  <span data-ttu-id="1f6bd-175">A szolgáltatáshoz a 4000-es porton beérkező kérések a tárolón a 80-as portra vannak leképezve.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-175">Incoming requests to the service on port 4000 are mapped to port 80 on the container.</span></span>  <span data-ttu-id="1f6bd-176">Ha a tárolót hitelesíteni kell egy magántárolóval, adja hozzá a `RepositoryCredentials` elemet.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-176">If your container needs to authenticate with a private repository, then add `RepositoryCredentials`.</span></span>  <span data-ttu-id="1f6bd-177">Ebben a cikkben a myregistry.azurecr.io tárolóregisztrációs adatbázis fióknevét és jelszavát adja meg.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-177">For this article, add the account name and password for the myregistry.azurecr.io container registry.</span></span> 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a><span data-ttu-id="1f6bd-178">Service Fabric-alkalmazás felépítése és becsomagolása</span><span class="sxs-lookup"><span data-stu-id="1f6bd-178">Build and package the Service Fabric application</span></span>
<span data-ttu-id="1f6bd-179">A Service Fabric Yeoman-sablonok tartalmaznak egy [Gradle](https://gradle.org/) felépítési szkriptet, amelyet felhasználhat az alkalmazás terminálból történő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-179">The Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use to build the application from the terminal.</span></span> <span data-ttu-id="1f6bd-180">Az alkalmazás felépítéséhez és becsomagolásához futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-180">To build and package the application, run the following:</span></span>

```bash
cd mycontainer
gradle
```

## <a name="deploy-the-application"></a><span data-ttu-id="1f6bd-181">Az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="1f6bd-181">Deploy the application</span></span>
<span data-ttu-id="1f6bd-182">Az alkalmazást a létrehozása után a Service Fabric parancssori felülettel telepítheti a helyi fürtben.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-182">Once the application is built, you can deploy it to the local cluster using the Service Fabric CLI.</span></span>

<span data-ttu-id="1f6bd-183">Csatlakozzon a helyi Service Fabric-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-183">Connect to the local Service Fabric cluster.</span></span>

```bash
sfctl cluster select --endpoint http://localhost:19080
```

<span data-ttu-id="1f6bd-184">Használja a sablonban megadott telepítési szkriptet az alkalmazáscsomag a fürt lemezképtárolójába való másolásához, regisztrálja az alkalmazás típusát, és hozza létre az alkalmazás egy példányát.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-184">Use the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

```bash
./install.sh
```

<span data-ttu-id="1f6bd-185">Nyisson meg egy böngészőt, és keresse fel a Service Fabric Explorert a következő címen: http://localhost:19080/Explorer (a Vagrant Mac OS X rendszeren való használata esetében a localhost helyett használja a virtuális gép magánhálózati IP-címét).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-185">Open a browser and navigate to Service Fabric Explorer at http://localhost:19080/Explorer (replace localhost with the private IP of the VM if using Vagrant on Mac OS X).</span></span> <span data-ttu-id="1f6bd-186">Bontsa ki az Alkalmazások csomópontot, és figyelje meg, hogy most már megjelenik benne egy bejegyzés az alkalmazása típusához, és egy másik a típus első példányához.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-186">Expand the Applications node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>

<span data-ttu-id="1f6bd-187">Csatlakozzon a futó tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-187">Connect to the running container.</span></span>  <span data-ttu-id="1f6bd-188">Nyisson meg egy webböngészőt, majd a 4000-es porton visszaadott IP-címet, például http://localhost:4000 .</span><span class="sxs-lookup"><span data-stu-id="1f6bd-188">Open a web browser pointing to the IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="1f6bd-189">A „Hello World!” címsornak kell</span><span class="sxs-lookup"><span data-stu-id="1f6bd-189">You should see the heading "Hello World!"</span></span> <span data-ttu-id="1f6bd-190">megjelennie a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-190">display in the browser.</span></span>

![Hello World!][hello-world]

## <a name="clean-up"></a><span data-ttu-id="1f6bd-192">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1f6bd-192">Clean up</span></span>
<span data-ttu-id="1f6bd-193">Használja a sablonban megadott eltávolítási szkriptet az alkalmazáspéldány helyi fejlesztési fürtről történő törléséhez, és törölje az alkalmazástípus regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-193">Use the uninstall script provided in the template to delete the application instance from the local development cluster and unregister the application type.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="1f6bd-194">Miután leküldte a rendszerképet a tároló-beállításjegyzékbe, törölheti a helyi rendszerképet a fejlesztői számítógépről:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-194">After you push the image to the container registry you can delete the local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="1f6bd-195">Példa teljes Service Fabric-alkalmazásra és szolgáltatásjegyzékre</span><span class="sxs-lookup"><span data-stu-id="1f6bd-195">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="1f6bd-196">Itt találja a jelen cikkben használt teljes szolgáltatás- és alkalmazásjegyzéket.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-196">Here are the complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="1f6bd-197">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="1f6bd-197">ServiceManifest.xml</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers 
      to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a><span data-ttu-id="1f6bd-198">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="1f6bd-198">ApplicationManifest.xml</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="mycontainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
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
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
      cluster, set InstanceCount to -1 for the container service to run on every node in 
      the cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="adding-more-services-to-an-existing-application"></a><span data-ttu-id="1f6bd-199">További szolgáltatások hozzáadása meglévő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="1f6bd-199">Adding more services to an existing application</span></span>

<span data-ttu-id="1f6bd-200">Ha egy másik tárolószolgáltatást szeretne hozzáadni a Yeoman használatával már létrehozott alkalmazáshoz, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-200">To add another container service to an application already created using yeoman, perform the following steps:</span></span>

1. <span data-ttu-id="1f6bd-201">Lépjen a meglevő alkalmazás gyökérkönyvtárába.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-201">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="1f6bd-202">Például `cd ~/YeomanSamples/MyApplication`, ha a `MyApplication` a Yeoman által létrehozott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-202">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="1f6bd-203">Futtassa a `yo azuresfcontainer:AddService` parancsot.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-203">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>


## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="1f6bd-204">A tároló kényszerített leállítását megelőző időköz beállítása</span><span class="sxs-lookup"><span data-stu-id="1f6bd-204">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="1f6bd-205">Konfigurálhat egy időintervallumot a futtatókörnyezet számára, ezzel megadva, hogy az mennyit várjon a tároló eltávolítása előtt, miután megkezdődött a szolgáltatás törlése (vagy másik csomópontba áthelyezése).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-205">You can configure a time interval for the runtime to wait before the container is removed after the service deletion (or a move to another node) has started.</span></span> <span data-ttu-id="1f6bd-206">Az időintervallum konfigurálásával a `docker stop <time in seconds>` parancsot küldi a tárolónak.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-206">Configuring the time interval sends the `docker stop <time in seconds>` command to the container.</span></span>   <span data-ttu-id="1f6bd-207">További információ: [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-207">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="1f6bd-208">A várakozási időköz a `Hosting` szakaszban van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-208">The time interval to wait is specified under the `Hosting` section.</span></span> <span data-ttu-id="1f6bd-209">Az alábbi fürtjegyzék kódrészlete azt mutatja be, hogyan adható meg a várakozási időköz:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-209">The following cluster manifest snippet shows how to set the wait interval:</span></span>

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
<span data-ttu-id="1f6bd-210">Az alapértelmezett időintervallum 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-210">The default time interval is set to 10 seconds.</span></span> <span data-ttu-id="1f6bd-211">Mivel ez egy dinamikus konfiguráció, a csak konfigurációs frissítés a fürtön frissíti az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-211">Since this configuration is dynamic, a config only upgrade on the cluster updates the timeout.</span></span> 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a><span data-ttu-id="1f6bd-212">Futtatókörnyezet konfigurálása a nem használt tárolórendszerképek eltávolításához</span><span class="sxs-lookup"><span data-stu-id="1f6bd-212">Configure the runtime to remove unused container images</span></span>

<span data-ttu-id="1f6bd-213">A Service Fabric-fürtöt úgy is konfigurálhatja, hogy eltávolítsa a nem használt tárolórendszerképeket a csomópontról.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-213">You can configure the Service Fabric cluster to remove unused container images from the node.</span></span> <span data-ttu-id="1f6bd-214">Ez a konfiguráció lehetővé teszi a lemezterület visszanyerését, ha túl sok tárolórendszerkép található a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-214">This configuration allows disk space to be recaptured if too many container images are present on the node.</span></span>  <span data-ttu-id="1f6bd-215">A funkció engedélyezéséhez frissítse a fürtjegyzék `Hosting` szakaszát az alábbi kódrészletben látható módon:</span><span class="sxs-lookup"><span data-stu-id="1f6bd-215">To enable this feature, update the `Hosting` section in the cluster manifest as shown in the following snippet:</span></span> 


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

<span data-ttu-id="1f6bd-216">A `ContainerImagesToSkip` paraméternél megadhatja azokat a rendszerképeket, amelyeket nem szabad törölni.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-216">For images that should not be deleted, you can specify them under the `ContainerImagesToSkip` parameter.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="1f6bd-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f6bd-217">Next steps</span></span>
* <span data-ttu-id="1f6bd-218">További információk a [tárolók futtatásáról a Service Fabricban](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-218">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="1f6bd-219">Tekintse meg a [.NET-alkalmazás üzembe helyezését](service-fabric-host-app-in-a-container.md) ismertető oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-219">Read the [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="1f6bd-220">További információk a Service Fabric [alkalmazásainak élettartamáról](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="1f6bd-220">Learn about the Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="1f6bd-221">Tekintse meg [a Service Fabric-tárolók mintakódjait](https://github.com/Azure-Samples/service-fabric-dotnet-containers) a GitHubon.</span><span class="sxs-lookup"><span data-stu-id="1f6bd-221">Checkout the [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
