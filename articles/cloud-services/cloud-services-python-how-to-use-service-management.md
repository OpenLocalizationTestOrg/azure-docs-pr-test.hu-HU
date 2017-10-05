---
title: "A szolgáltatáskezelő API (Python) - funkcióismertető használata"
description: "Megtudhatja, hogyan programozott módon hajtható végre az általános szolgáltatás-felügyeleti feladatokat a Python."
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: 13249ba9a4b317a3154776b411ce0bb1f316b3bb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-management-from-python"></a><span data-ttu-id="c9e56-103">Kezelés a Python használata</span><span class="sxs-lookup"><span data-stu-id="c9e56-103">How to use Service Management from Python</span></span>
<span data-ttu-id="c9e56-104">Ez az útmutató bemutatja, hogyan programozott módon hajtható végre az általános szolgáltatás-felügyeleti feladatokat a Python.</span><span class="sxs-lookup"><span data-stu-id="c9e56-104">This guide shows you how to programmatically perform common service management tasks from Python.</span></span> <span data-ttu-id="c9e56-105">A **ServiceManagementService** osztályt a [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) programozott hozzáférést a nagy részét a szolgáltatás-felügyelettel kapcsolatos funkció elérhető támogatja a [a klasszikus Azure portálon] [ management-portal] (például **létrehozása, frissítése és törlése a felhőszolgáltatások, központi telepítések, adatok szolgáltatások és virtuális gépek**).</span><span class="sxs-lookup"><span data-stu-id="c9e56-105">The **ServiceManagementService** class in the [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) supports programmatic access to much of the service management-related functionality that is available in the [Azure classic portal][management-portal] (such as **creating, updating, and deleting cloud services, deployments, data management services, and virtual machines**).</span></span> <span data-ttu-id="c9e56-106">Ez a funkció akkor lehet hasznos, szolgáltatás-felügyelet programozott hozzáférést igénylő alkalmazások fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="c9e56-106">This functionality can be useful in building applications that need programmatic access to service management.</span></span>

## <span data-ttu-id="c9e56-107"><a name="WhatIs"></a>Kezelő újdonságai</span><span class="sxs-lookup"><span data-stu-id="c9e56-107"><a name="WhatIs"> </a>What is Service Management</span></span>
<span data-ttu-id="c9e56-108">A Service Management API nagy részét a szolgáltatás felügyeleti funkció keresztül elérhető programozott hozzáférést biztosít a [a klasszikus Azure portálon][management-portal].</span><span class="sxs-lookup"><span data-stu-id="c9e56-108">The Service Management API provides programmatic access to much of the service management functionality available through the [Azure classic portal][management-portal].</span></span> <span data-ttu-id="c9e56-109">Az Azure SDK for Python teszi lehetővé a felhőszolgáltatások és a storage-fiókok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="c9e56-109">The Azure SDK for Python allows you to manage your cloud services and storage accounts.</span></span>

<span data-ttu-id="c9e56-110">A szolgáltatásfelügyeleti API használatával kell [az Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9e56-110">To use the Service Management API, you need to [create an Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <span data-ttu-id="c9e56-111"><a name="Concepts"></a>Fogalmak</span><span class="sxs-lookup"><span data-stu-id="c9e56-111"><a name="Concepts"> </a>Concepts</span></span>
<span data-ttu-id="c9e56-112">Az Azure SDK for Python becsomagolja a [Azure szolgáltatásfelügyeleti API][svc-mgmt-rest-api], vagyis a REST API-t.</span><span class="sxs-lookup"><span data-stu-id="c9e56-112">The Azure SDK for Python wraps the [Azure Service Management API][svc-mgmt-rest-api], which is a REST API.</span></span> <span data-ttu-id="c9e56-113">Az API műveletei SSL-kapcsolaton keresztül mennek végbe, és X.509 v3 tanúsítványok használatával kölcsönösen hitelesítettek.</span><span class="sxs-lookup"><span data-stu-id="c9e56-113">All API operations are performed over SSL and mutually authenticated using X.509 v3 certificates.</span></span> <span data-ttu-id="c9e56-114">A felügyeleti szolgáltatás elérhető egy Azure-ban futó szolgáltatáson belülről, illetve az interneten keresztül közvetlenül is bármely olyan alkalmazásból, amely képes HTTPS-kéréseket küldeni és HTTPS-válaszokat fogadni.</span><span class="sxs-lookup"><span data-stu-id="c9e56-114">The management service may be accessed from within a service running in Azure, or directly over the Internet from any application that can send an HTTPS request and receive an HTTPS response.</span></span>

## <span data-ttu-id="c9e56-115"><a name="Installation"></a>Telepítése</span><span class="sxs-lookup"><span data-stu-id="c9e56-115"><a name="Installation"> </a>Installation</span></span>
<span data-ttu-id="c9e56-116">Ebben a cikkben leírt összes funkcióját érhetők el a `azure-servicemanagement-legacy` csomagot, amely a pip használatával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="c9e56-116">All the features described in this article are available in the `azure-servicemanagement-legacy` package, which you can install using pip.</span></span> <span data-ttu-id="c9e56-117">(Például, ha még nem ismeri a Python) telepítésével kapcsolatos további információkért lásd: Ez a cikk: [Python telepítése és az Azure SDK](../python-how-to-install.md)</span><span class="sxs-lookup"><span data-stu-id="c9e56-117">For more information about installation (for example, if you are new to Python), see this article: [Installing Python and the Azure SDK](../python-how-to-install.md)</span></span>

## <span data-ttu-id="c9e56-118"><a name="Connect"></a>Hogyan: szolgáltatásfelügyelet kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="c9e56-118"><a name="Connect"> </a>How to: Connect to service management</span></span>
<span data-ttu-id="c9e56-119">A Service Management végponthoz kapcsolódni, Azure-előfizetése Azonosítóját és egy érvényes felügyeleti tanúsítványt kell.</span><span class="sxs-lookup"><span data-stu-id="c9e56-119">To connect to the Service Management endpoint, you need your Azure subscription ID and a valid management certificate.</span></span> <span data-ttu-id="c9e56-120">Ezt úgy szerezheti be az előfizetés-Azonosítóval keresztül a [a klasszikus Azure portálon][management-portal].</span><span class="sxs-lookup"><span data-stu-id="c9e56-120">You can obtain your subscription ID through the [Azure classic portal][management-portal].</span></span>

> [!NOTE]
> <span data-ttu-id="c9e56-121">Már lehetséges a Windows futtatásakor OpenSSL létre tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="c9e56-121">It is now possible to use certificates created with OpenSSL when running on Windows.</span></span>  <span data-ttu-id="c9e56-122">Python 2.7.4 igényel vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="c9e56-122">It requires Python 2.7.4 or later.</span></span> <span data-ttu-id="c9e56-123">Azt javasoljuk, hogy a felhasználók általi OpenSSL .pfx, helyett óta .pfx-tanúsítványok valószínűleg törlődni fog a későbbiekben támogatása.</span><span class="sxs-lookup"><span data-stu-id="c9e56-123">We recommend users to use OpenSSL instead of .pfx, since support for .pfx certificates will likely be removed in the future.</span></span>
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a><span data-ttu-id="c9e56-124">Felügyeleti tanúsítványok Windows vagy Mac/Linux (OpenSSL)</span><span class="sxs-lookup"><span data-stu-id="c9e56-124">Management certificates on Windows/Mac/Linux (OpenSSL)</span></span>
<span data-ttu-id="c9e56-125">Használhat [OpenSSL](http://www.openssl.org/) a felügyeleti tanúsítvány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c9e56-125">You can use [OpenSSL](http://www.openssl.org/) to create your management certificate.</span></span>  <span data-ttu-id="c9e56-126">Ténylegesen kell létrehoznia egy kiszolgáló két tanúsítványt (a `.cer` fájl) és egy, az ügyfél (egy `.pem` fájlt).</span><span class="sxs-lookup"><span data-stu-id="c9e56-126">You actually need to create two certificates, one for the server (a `.cer` file) and one for the client (a `.pem` file).</span></span> <span data-ttu-id="c9e56-127">Létrehozásához a `.pem` fájlt, hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="c9e56-127">To create the `.pem` file, execute:</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="c9e56-128">Létrehozásához a `.cer` tanúsítvány, hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="c9e56-128">To create the `.cer` certificate, execute:</span></span>

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

<span data-ttu-id="c9e56-129">Az Azure-tanúsítványokkal kapcsolatos további információkért lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="c9e56-129">For more information about Azure certificates, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="c9e56-130">OpenSSL-paraméter teljes leírását lásd: a dokumentációban a [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span><span class="sxs-lookup"><span data-stu-id="c9e56-130">For a complete description of OpenSSL parameters, see the documentation at [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span></span>

<span data-ttu-id="c9e56-131">A létrehozást követően ezeket a fájlokat, szeretné-e feltölteni a `.cer` fájl az Azure-ba, a "Feltöltés" művelet "Beállítások" lapján keresztül a [a klasszikus Azure portálon][management-portal], és jegyezze fel a mentési helyét kell a `.pem` fájl.</span><span class="sxs-lookup"><span data-stu-id="c9e56-131">After you have created these files, you need to upload the `.cer` file to Azure via the "Upload" action of the "Settings" tab of the [Azure classic portal][management-portal], and you need to make note of where you saved the `.pem` file.</span></span>

<span data-ttu-id="c9e56-132">Ha beszerezte az előfizetés-Azonosítóval, létrehozott egy tanúsítványt, és fel kell tölteni a `.cer` fájl az Azure-ba, csatlakozhat az Azure felügyeleti végpont úgy, hogy az előfizetés-azonosító és elérési útját a `.pem` fájl **ServiceManagementService**:</span><span class="sxs-lookup"><span data-stu-id="c9e56-132">After you have obtained your subscription ID, created a certificate, and uploaded the `.cer` file to Azure, you can connect to the Azure management endpoint by passing the subscription id and the path to the `.pem` file to **ServiceManagementService**:</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="c9e56-133">Az előző példában `sms` van egy **ServiceManagementService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c9e56-133">In the preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="c9e56-134">A **ServiceManagementService** osztály az Azure-szolgáltatások kezelésére szolgáló elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="c9e56-134">The **ServiceManagementService** class is the primary class used to manage Azure services.</span></span>

### <a name="management-certificates-on-windows-makecert"></a><span data-ttu-id="c9e56-135">A Windows (MakeCert) felügyeleti tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="c9e56-135">Management certificates on Windows (MakeCert)</span></span>
<span data-ttu-id="c9e56-136">Létrehozhat egy önaláírt felügyeleti tanúsítvány a gép használt `makecert.exe`.</span><span class="sxs-lookup"><span data-stu-id="c9e56-136">You can create a self-signed management certificate on your machine using `makecert.exe`.</span></span>  <span data-ttu-id="c9e56-137">Nyissa meg a **Visual Studio parancssorból** , egy **rendszergazda** és használja a következő parancsot, cseréje *AzureCertificate* használni szeretné a tanúsítványt nevű.</span><span class="sxs-lookup"><span data-stu-id="c9e56-137">Open a **Visual Studio command prompt** as an **administrator** and use the following command, replacing *AzureCertificate* with the certificate name you would like to use.</span></span>

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

<span data-ttu-id="c9e56-138">A parancs létrehozza a `.cer` fájlt, és telepíti azt a a **személyes** tanúsítványtárolójába.</span><span class="sxs-lookup"><span data-stu-id="c9e56-138">The command creates the `.cer` file, and installs it in the **Personal** certificate store.</span></span> <span data-ttu-id="c9e56-139">További információkért lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="c9e56-139">For more information, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span>

<span data-ttu-id="c9e56-140">Miután létrehozta a tanúsítványt, szeretné-e feltölteni a `.cer` fájl az Azure-ba, a "Feltöltés" művelet "Beállítások" lapján keresztül a [a klasszikus Azure portálon][management-portal].</span><span class="sxs-lookup"><span data-stu-id="c9e56-140">After you have created the certificate, you need to upload the `.cer` file to Azure via the "Upload" action of the "Settings" tab of the [Azure classic portal][management-portal].</span></span>

<span data-ttu-id="c9e56-141">Ha beszerezte az előfizetés-Azonosítóval, létrehozott egy tanúsítványt, és fel kell tölteni a `.cer` fájl az Azure-ba, csatlakozhat az Azure felügyeleti végpont úgy, hogy az előfizetés-azonosító és a tanúsítvány helye a **személyes** tanúsítványtárolójának **ServiceManagementService** (újra, cserélje le *AzureCertificate* a tanúsítvány neve):</span><span class="sxs-lookup"><span data-stu-id="c9e56-141">After you have obtained your subscription ID, created a certificate, and uploaded the `.cer` file to Azure, you can connect to the Azure management endpoint by passing the subscription id and the location of the certificate in your **Personal** certificate store to **ServiceManagementService** (again, replace *AzureCertificate* with the name of your certificate):</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="c9e56-142">Az előző példában `sms` van egy **ServiceManagementService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c9e56-142">In the preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="c9e56-143">A **ServiceManagementService** osztály az Azure-szolgáltatások kezelésére szolgáló elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="c9e56-143">The **ServiceManagementService** class is the primary class used to manage Azure services.</span></span>

## <span data-ttu-id="c9e56-144"><a name="ListAvailableLocations"></a>Hogyan: elérhető helyről felsorolása</span><span class="sxs-lookup"><span data-stu-id="c9e56-144"><a name="ListAvailableLocations"> </a>How to: List available locations</span></span>
<span data-ttu-id="c9e56-145">A helyek elérhető szolgáltatásait üzemeltető kilistázhatja a **lista\_helyek** metódus:</span><span class="sxs-lookup"><span data-stu-id="c9e56-145">To list the locations that are available for hosting services, use the **list\_locations** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

<span data-ttu-id="c9e56-146">Egy felhőalapú szolgáltatás vagy a storage szolgáltatás létrehozásakor meg kell adnia egy érvényes helyet.</span><span class="sxs-lookup"><span data-stu-id="c9e56-146">When you create a cloud service or storage service you need to provide a valid location.</span></span> <span data-ttu-id="c9e56-147">A **lista\_helyek** metódus mindig a jelenleg elérhető helyről naprakész listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c9e56-147">The **list\_locations** method always returns an up-to-date list of the currently available locations.</span></span> <span data-ttu-id="c9e56-148">Től írásának, a helyeket a következők:</span><span class="sxs-lookup"><span data-stu-id="c9e56-148">As of this writing, the available locations are:</span></span>

* <span data-ttu-id="c9e56-149">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="c9e56-149">West Europe</span></span>
* <span data-ttu-id="c9e56-150">Észak-Európa</span><span class="sxs-lookup"><span data-stu-id="c9e56-150">North Europe</span></span>
* <span data-ttu-id="c9e56-151">Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="c9e56-151">Southeast Asia</span></span>
* <span data-ttu-id="c9e56-152">Kelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="c9e56-152">East Asia</span></span>
* <span data-ttu-id="c9e56-153">USA középső régiója</span><span class="sxs-lookup"><span data-stu-id="c9e56-153">Central US</span></span>
* <span data-ttu-id="c9e56-154">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="c9e56-154">North Central US</span></span>
* <span data-ttu-id="c9e56-155">USA déli középső régiója</span><span class="sxs-lookup"><span data-stu-id="c9e56-155">South Central US</span></span>
* <span data-ttu-id="c9e56-156">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="c9e56-156">West US</span></span>
* <span data-ttu-id="c9e56-157">USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="c9e56-157">East US</span></span>
* <span data-ttu-id="c9e56-158">Kelet-Japán</span><span class="sxs-lookup"><span data-stu-id="c9e56-158">Japan East</span></span>
* <span data-ttu-id="c9e56-159">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="c9e56-159">Japan West</span></span>
* <span data-ttu-id="c9e56-160">Dél-Brazília</span><span class="sxs-lookup"><span data-stu-id="c9e56-160">Brazil South</span></span>
* <span data-ttu-id="c9e56-161">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="c9e56-161">Australia East</span></span>
* <span data-ttu-id="c9e56-162">Délkelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="c9e56-162">Australia Southeast</span></span>

## <span data-ttu-id="c9e56-163"><a name="CreateCloudService"></a>Hogyan: felhőalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9e56-163"><a name="CreateCloudService"> </a>How to: Create a cloud service</span></span>
<span data-ttu-id="c9e56-164">Amikor létrehoz egy alkalmazást, és futtassa az Azure-ban, a kódot és konfigurációs együtt nevezzük az Azure [felhőalapú szolgáltatás] [ cloud service] (néven egy *üzemeltetett szolgáltatás* korábbi Azure-ban kiadások).</span><span class="sxs-lookup"><span data-stu-id="c9e56-164">When you create an application and run it in Azure, the code and configuration together are called an Azure [cloud service][cloud service] (known as a *hosted service* in earlier Azure releases).</span></span> <span data-ttu-id="c9e56-165">A **létrehozása\_üzemeltetett\_szolgáltatás** metódus hozhat létre egy új kezelt szolgáltatást a futtatott szolgáltatás nevét (amelyen az Azure-ban egyedinek kell lennie), (automatikusan Base64 kódolású) címke, a leírás megadásával és helyét.</span><span class="sxs-lookup"><span data-stu-id="c9e56-165">The **create\_hosted\_service** method allows you to create a new hosted service by providing a hosted service name (which must be unique in Azure), a label (automatically encoded to base64), a description, and a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

<span data-ttu-id="c9e56-166">Előfizetés az összes üzemeltetett szolgáltatás listázhatja a **lista\_üzemeltetett\_szolgáltatások** módszert:</span><span class="sxs-lookup"><span data-stu-id="c9e56-166">You can list all the hosted services for your subscription with the **list\_hosted\_services** method:</span></span>

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

<span data-ttu-id="c9e56-167">Ha le szeretné kérdezni egy adott tárolt szolgáltatás adatainak, akkor megteheti úgy, hogy a futtatott szolgáltatás nevét, hogy a **beolvasása\_üzemeltetett\_szolgáltatás\_tulajdonságok** módszert:</span><span class="sxs-lookup"><span data-stu-id="c9e56-167">If you want to get information about a particular hosted service, you can do so by passing the hosted service name to the **get\_hosted\_service\_properties** method:</span></span>

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

<span data-ttu-id="c9e56-168">Miután létrehozott egy felhőalapú szolgáltatás, telepítheti a kódot a szolgáltatást, amely a **létrehozása\_telepítési** metódus.</span><span class="sxs-lookup"><span data-stu-id="c9e56-168">After you have created a cloud service, you can deploy your code to the service with the **create\_deployment** method.</span></span>

## <span data-ttu-id="c9e56-169"><a name="DeleteCloudService"></a>Hogyan: egy felhőalapú szolgáltatás törlése</span><span class="sxs-lookup"><span data-stu-id="c9e56-169"><a name="DeleteCloudService"> </a>How to: Delete a cloud service</span></span>
<span data-ttu-id="c9e56-170">Egy felhőalapú szolgáltatás törölheti úgy, hogy a szolgáltatás nevét a **törlése\_üzemeltetett\_szolgáltatás** módszert:</span><span class="sxs-lookup"><span data-stu-id="c9e56-170">You can delete a cloud service by passing the service name to the **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service('myhostedservice')

<span data-ttu-id="c9e56-171">A szolgáltatás törlése előtt a szolgáltatás központi telepítéseinek először törölni kell.</span><span class="sxs-lookup"><span data-stu-id="c9e56-171">Before you can delete a service, all deployments for the service must first be deleted.</span></span> <span data-ttu-id="c9e56-172">(Lásd: [hogyan: törölje a központi telepítést](#DeleteDeployment) részletes.)</span><span class="sxs-lookup"><span data-stu-id="c9e56-172">(See [How to: Delete a deployment](#DeleteDeployment) for details.)</span></span>

## <span data-ttu-id="c9e56-173"><a name="DeleteDeployment"></a>Útmutató: a központi telepítés törlése</span><span class="sxs-lookup"><span data-stu-id="c9e56-173"><a name="DeleteDeployment"> </a>How to: Delete a deployment</span></span>
<span data-ttu-id="c9e56-174">A központi telepítés törléséhez használja a **törlése\_telepítési** metódust.</span><span class="sxs-lookup"><span data-stu-id="c9e56-174">To delete a deployment, use the **delete\_deployment** method.</span></span> <span data-ttu-id="c9e56-175">A következő példa bemutatja, hogyan törölhető egy központi telepítést `v1`.</span><span class="sxs-lookup"><span data-stu-id="c9e56-175">The following example shows how to delete a deployment named `v1`.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <span data-ttu-id="c9e56-176"><a name="CreateStorageService"></a>Hogyan: storage szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9e56-176"><a name="CreateStorageService"> </a>How to: Create a storage service</span></span>
<span data-ttu-id="c9e56-177">A [társzolgáltatás](../storage/common/storage-create-storage-account.md) biztosít hozzáférést az Azure-bA [Blobok](../storage/blobs/storage-python-how-to-use-blob-storage.md), [táblák](../cosmos-db/table-storage-how-to-use-python.md), és [várólisták](../storage/queues/storage-python-how-to-use-queue-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c9e56-177">A [storage service](../storage/common/storage-create-storage-account.md) gives you access to Azure [Blobs](../storage/blobs/storage-python-how-to-use-blob-storage.md), [Tables](../cosmos-db/table-storage-how-to-use-python.md), and [Queues](../storage/queues/storage-python-how-to-use-queue-storage.md).</span></span> <span data-ttu-id="c9e56-178">Hozzon létre egy társzolgáltatás, kell a szolgáltatás (kisbetűket 3 és 24 közötti és egyedi Azure-ban), leírását, egy címke (legfeljebb 100 karakter, automatikusan a Base64 kódolású) és egy hely nevét.</span><span class="sxs-lookup"><span data-stu-id="c9e56-178">To create a storage service, you need a name for the service (between 3 and 24 lowercase characters and unique within Azure), a description, a label (up to 100 characters, automatically encoded to base64), and a location.</span></span> <span data-ttu-id="c9e56-179">A következő példa bemutatja, hogyan tárolási szolgáltatás létrehozása a hely megadásával.</span><span class="sxs-lookup"><span data-stu-id="c9e56-179">The following example shows how to create a storage service by specifying a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

<span data-ttu-id="c9e56-180">Megjegyzés: az előző példában, amely állapotának a **létrehozása\_tárolási\_fiók** művelet által visszaadott eredmény átadásával kérhető **létrehozása\_tárolási\_fiók** számára a **beolvasása\_művelet\_állapot** metódus.</span><span class="sxs-lookup"><span data-stu-id="c9e56-180">Note in the preceding example that the status of the **create\_storage\_account** operation can be retrieved by passing the result returned by **create\_storage\_account** to the **get\_operation\_status** method.</span></span>  

<span data-ttu-id="c9e56-181">A storage-fiókok és az azok tulajdonságait listázhatja a **lista\_tárolási\_fiókok** metódus:</span><span class="sxs-lookup"><span data-stu-id="c9e56-181">You can list your storage accounts and their properties with the **list\_storage\_accounts** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <span data-ttu-id="c9e56-182"><a name="DeleteStorageService"></a>Útmutató: a társzolgáltatás törlése</span><span class="sxs-lookup"><span data-stu-id="c9e56-182"><a name="DeleteStorageService"> </a>How to: Delete a storage service</span></span>
<span data-ttu-id="c9e56-183">Egy tároló szolgáltatást úgy, hogy a szolgáltatás nevét a törölheti a **törlése\_tárolási\_fiók** metódus.</span><span class="sxs-lookup"><span data-stu-id="c9e56-183">You can delete a storage service by passing the storage service name to the **delete\_storage\_account** method.</span></span> <span data-ttu-id="c9e56-184">A storage szolgáltatás törlése (blobot, táblát és üzenetsort) szolgáltatásban tárolt összes adat törlése.</span><span class="sxs-lookup"><span data-stu-id="c9e56-184">Deleting a storage service deletes all data stored in the service (blobs, tables, and queues).</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <span data-ttu-id="c9e56-185"><a name="ListOperatingSystems"></a>Hogyan: listából a rendelkezésre álló operációs rendszeren</span><span class="sxs-lookup"><span data-stu-id="c9e56-185"><a name="ListOperatingSystems"> </a>How to: List available operating systems</span></span>
<span data-ttu-id="c9e56-186">Üzemeltetési szolgáltatásokat rendelkezésre álló operációs rendszerek listáját, használja a **lista\_működő\_rendszerek** módszer:</span><span class="sxs-lookup"><span data-stu-id="c9e56-186">To list the operating systems that are available for hosting services, use the **list\_operating\_systems** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

<span data-ttu-id="c9e56-187">Másik lehetőségként használhatja a **lista\_működő\_rendszer\_családok** metódus, amely csoportosítja a termékcsalád operációs rendszerek:</span><span class="sxs-lookup"><span data-stu-id="c9e56-187">Alternatively, you can use the **list\_operating\_system\_families** method, which groups the operating systems by family:</span></span>

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <span data-ttu-id="c9e56-188"><a name="CreateVMImage"></a>Hogyan: hozzon létre egy operációsrendszer-lemezképet</span><span class="sxs-lookup"><span data-stu-id="c9e56-188"><a name="CreateVMImage"> </a>How to: Create an operating system image</span></span>
<span data-ttu-id="c9e56-189">A lemezképtár adhat hozzá az operációs rendszer lemezképét, a **hozzáadása\_os\_kép** módszer:</span><span class="sxs-lookup"><span data-stu-id="c9e56-189">To add an operating system image to the image repository, use the **add\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

<span data-ttu-id="c9e56-190">Kilistázhatja az elérhető rendszerképek az **lista\_os\_képek** metódus.</span><span class="sxs-lookup"><span data-stu-id="c9e56-190">To list the operating system images that are available, use the **list\_os\_images** method.</span></span> <span data-ttu-id="c9e56-191">Összes platform képek és felhasználói képeket tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="c9e56-191">It includes all platform images and user images:</span></span>

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <span data-ttu-id="c9e56-192"><a name="DeleteVMImage"></a>Útmutató: az operációsrendszer-lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="c9e56-192"><a name="DeleteVMImage"> </a>How to: Delete an operating system image</span></span>
<span data-ttu-id="c9e56-193">Egy felhasználói lemezkép törléséhez használja a **törlése\_os\_kép** módszert:</span><span class="sxs-lookup"><span data-stu-id="c9e56-193">To delete a user image, use the **delete\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <span data-ttu-id="c9e56-194"><a name="CreateVM"></a>Útmutató: virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9e56-194"><a name="CreateVM"> </a>How to: Create a virtual machine</span></span>
<span data-ttu-id="c9e56-195">A virtuális gép létrehozásához először hozzon létre egy [felhőalapú szolgáltatás](#CreateCloudService).</span><span class="sxs-lookup"><span data-stu-id="c9e56-195">To create a virtual machine, you first need to create a [cloud service](#CreateCloudService).</span></span>  <span data-ttu-id="c9e56-196">Majd létre szeretne hozni a virtuális gép telepítési használja a **létrehozása\_virtuális\_gép\_telepítési** módszer:</span><span class="sxs-lookup"><span data-stu-id="c9e56-196">Then create the virtual machine deployment using the **create\_virtual\_machine\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <span data-ttu-id="c9e56-197"><a name="DeleteVM"></a>Útmutató: virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="c9e56-197"><a name="DeleteVM"> </a>How to: Delete a virtual machine</span></span>
<span data-ttu-id="c9e56-198">A virtuális gép törlése, előbb törölnie a központi telepítés használatával a **törlése\_telepítési** módszer:</span><span class="sxs-lookup"><span data-stu-id="c9e56-198">To delete a virtual machine, you first delete the deployment using the **delete\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

<span data-ttu-id="c9e56-199">A felhőszolgáltatás használja majd törölhető a **törlése\_üzemeltetett\_szolgáltatás** módszert:</span><span class="sxs-lookup"><span data-stu-id="c9e56-199">The cloud service can then be deleted using the **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a><span data-ttu-id="c9e56-200">A virtuális gép létrehozása a rögzített virtuálisgép-lemezkép</span><span class="sxs-lookup"><span data-stu-id="c9e56-200">How To: Create a Virtual Machine from a Captured Virtual Machine Image</span></span>
<span data-ttu-id="c9e56-201">A virtuális gép lemezképének, akkor először hívja meg a **rögzítése\_vm\_kép** módszert:</span><span class="sxs-lookup"><span data-stu-id="c9e56-201">To capture a VM image, you first call the **capture\_vm\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

<span data-ttu-id="c9e56-202">Ezt követően győződjön meg arról, hogy sikeresen elkészült a kép, használja a **lista\_vm\_képek** API-t, és győződjön meg arról, hogy a kép megjelenik az eredmények között:</span><span class="sxs-lookup"><span data-stu-id="c9e56-202">Next, to make sure that you have successfully captured the image, use the **list\_vm\_images** api, and make sure your image is displayed in the results:</span></span>

    images = sms.list_vm_images()

<span data-ttu-id="c9e56-203">Végül hozhat létre a virtuális gépet a rögzített lemezkép használatával, a **létrehozása\_virtuális\_gép\_telepítési** előtt, de ezúttal adjon át a vm_image_name helyette módszer</span><span class="sxs-lookup"><span data-stu-id="c9e56-203">To finally create the virtual machine using the captured image, use the **create\_virtual\_machine\_deployment** method as before, but this time pass in the vm_image_name instead</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

<span data-ttu-id="c9e56-204">Linux virtuális gép rögzítése kapcsolatban további tudnivalókért lásd: [egy Linux virtuális gép rögzítése.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c9e56-204">To learn more about how to capture a Linux Virtual Machine, see [How to Capture a Linux Virtual Machine.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="c9e56-205">A Windows rendszerű virtuális gép rögzítése kapcsolatos további információkért lásd: [egy Windows virtuális gép rögzítése.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c9e56-205">To learn more about how to capture a Windows Virtual Machine, see [How to Capture a Windows Virtual Machine.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="c9e56-206"><a name="What's Next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9e56-206"><a name="What's Next"> </a>Next Steps</span></span>
<span data-ttu-id="c9e56-207">Most, hogy megismerte a service management alapjait, hogy elérhető a [teljes API referenciadokumentációt tartalmaz az Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) , és végezze el az összetett feladatok könnyedén kezelheti a python alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c9e56-207">Now that you've learned the basics of service management, you can access the [Complete API reference documentation for the Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) and perform complex tasks easily to manage your python application.</span></span>

<span data-ttu-id="c9e56-208">További információ: [Python fejlesztői központban](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="c9e56-208">For more information, see the [Python Developer Center](/develop/python/).</span></span>

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
