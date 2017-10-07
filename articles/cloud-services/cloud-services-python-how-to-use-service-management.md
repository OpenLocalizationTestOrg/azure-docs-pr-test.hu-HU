---
title: "aaaHow toouse hello service management API (Python) - funkcióismertető"
description: "Ismerje meg, hogyan tooprogrammatically közös szolgáltatás felügyeleti feladatok végrehajtása a Python."
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
ms.openlocfilehash: b59622203470e1586484cec4033515edb39ca4d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-management-from-python"></a><span data-ttu-id="3fc1f-103">Hogyan toouse-kezelés a Python</span><span class="sxs-lookup"><span data-stu-id="3fc1f-103">How toouse Service Management from Python</span></span>
<span data-ttu-id="3fc1f-104">Ez az útmutató bemutatja, hogyan tooprogrammatically közös szolgáltatás felügyeleti feladatok végrehajtása a Python.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-104">This guide shows you how tooprogrammatically perform common service management tasks from Python.</span></span> <span data-ttu-id="3fc1f-105">Hello **ServiceManagementService** hello osztály [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) támogatja a programozott hozzáférést toomuch hello szolgáltatás-felügyelettel kapcsolatos funkció elérhető hello [a klasszikus azure portálon] [ management-portal] (például **létrehozása, frissítése és törlése a felhőszolgáltatások, központi telepítések, adatok szolgáltatások és virtuális gépek**).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-105">hello **ServiceManagementService** class in hello [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) supports programmatic access toomuch of hello service management-related functionality that is available in hello [Azure classic portal][management-portal] (such as **creating, updating, and deleting cloud services, deployments, data management services, and virtual machines**).</span></span> <span data-ttu-id="3fc1f-106">Ez a funkció tooservice felügyeleti programozott hozzáférést igénylő alkalmazások fejlesztése során hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-106">This functionality can be useful in building applications that need programmatic access tooservice management.</span></span>

## <span data-ttu-id="3fc1f-107"><a name="WhatIs"></a>Kezelő újdonságai</span><span class="sxs-lookup"><span data-stu-id="3fc1f-107"><a name="WhatIs"> </a>What is Service Management</span></span>
<span data-ttu-id="3fc1f-108">hello szolgáltatásfelügyeleti API biztosít a hello szolgáltatás felügyeleti funkcióinak hello keresztül elérhető programozott hozzáférés toomuch [a klasszikus Azure portálon][management-portal].</span><span class="sxs-lookup"><span data-stu-id="3fc1f-108">hello Service Management API provides programmatic access toomuch of hello service management functionality available through hello [Azure classic portal][management-portal].</span></span> <span data-ttu-id="3fc1f-109">hello Azure SDK for Python toomanage lehetővé teszi a felhőalapú szolgáltatások és a storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-109">hello Azure SDK for Python allows you toomanage your cloud services and storage accounts.</span></span>

<span data-ttu-id="3fc1f-110">toouse hello Service Management API-t kell túl[az Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-110">toouse hello Service Management API, you need too[create an Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <span data-ttu-id="3fc1f-111"><a name="Concepts"></a>Fogalmak</span><span class="sxs-lookup"><span data-stu-id="3fc1f-111"><a name="Concepts"> </a>Concepts</span></span>
<span data-ttu-id="3fc1f-112">hello Azure SDK for Python becsomagolja hello [Azure szolgáltatásfelügyeleti API][svc-mgmt-rest-api], vagyis a REST API-t.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-112">hello Azure SDK for Python wraps hello [Azure Service Management API][svc-mgmt-rest-api], which is a REST API.</span></span> <span data-ttu-id="3fc1f-113">Az API műveletei SSL-kapcsolaton keresztül mennek végbe, és X.509 v3 tanúsítványok használatával kölcsönösen hitelesítettek.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-113">All API operations are performed over SSL and mutually authenticated using X.509 v3 certificates.</span></span> <span data-ttu-id="3fc1f-114">hello szolgáltatás előfordulhat, hogy érhetők el az Azure-ban, vagy hello interneten keresztül közvetlenül futó bármely alkalmazásból, amely egy HTTPS-kéréseket küldhet és fogadhat egy HTTPS-válasz szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-114">hello management service may be accessed from within a service running in Azure, or directly over hello Internet from any application that can send an HTTPS request and receive an HTTPS response.</span></span>

## <span data-ttu-id="3fc1f-115"><a name="Installation"></a>Telepítése</span><span class="sxs-lookup"><span data-stu-id="3fc1f-115"><a name="Installation"> </a>Installation</span></span>
<span data-ttu-id="3fc1f-116">Ebben a cikkben leírt összes hello funkciók érhetők el hello `azure-servicemanagement-legacy` csomagot, amely a pip használatával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-116">All hello features described in this article are available in hello `azure-servicemanagement-legacy` package, which you can install using pip.</span></span> <span data-ttu-id="3fc1f-117">(Például, ha új tooPython) telepítésével kapcsolatos további információkért lásd: Ez a cikk: [telepítése Python és hello Azure SDK](../python-how-to-install.md)</span><span class="sxs-lookup"><span data-stu-id="3fc1f-117">For more information about installation (for example, if you are new tooPython), see this article: [Installing Python and hello Azure SDK](../python-how-to-install.md)</span></span>

## <span data-ttu-id="3fc1f-118"><a name="Connect"></a>Hogyan: csatlakozás tooservice felügyeleti</span><span class="sxs-lookup"><span data-stu-id="3fc1f-118"><a name="Connect"> </a>How to: Connect tooservice management</span></span>
<span data-ttu-id="3fc1f-119">Azure-előfizetése Azonosítóját és egy érvényes felügyeleti tanúsítvány szüksége tooconnect toohello szolgáltatásfelügyelet végpont.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-119">tooconnect toohello Service Management endpoint, you need your Azure subscription ID and a valid management certificate.</span></span> <span data-ttu-id="3fc1f-120">Ezt úgy szerezheti be az előfizetés-Azonosítóval keresztül hello [a klasszikus Azure portálon][management-portal].</span><span class="sxs-lookup"><span data-stu-id="3fc1f-120">You can obtain your subscription ID through hello [Azure classic portal][management-portal].</span></span>

> [!NOTE]
> <span data-ttu-id="3fc1f-121">Az OpenSSL jön létre, amikor a Windows rendszeren futó lehetséges toouse tanúsítványok már.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-121">It is now possible toouse certificates created with OpenSSL when running on Windows.</span></span>  <span data-ttu-id="3fc1f-122">Python 2.7.4 igényel vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-122">It requires Python 2.7.4 or later.</span></span> <span data-ttu-id="3fc1f-123">.Pfx-tanúsítványok valószínűleg törlődni fog a jövőben hello támogatása óta ajánlott felhasználók toouse OpenSSL .pfx, helyett.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-123">We recommend users toouse OpenSSL instead of .pfx, since support for .pfx certificates will likely be removed in hello future.</span></span>
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a><span data-ttu-id="3fc1f-124">Felügyeleti tanúsítványok Windows vagy Mac/Linux (OpenSSL)</span><span class="sxs-lookup"><span data-stu-id="3fc1f-124">Management certificates on Windows/Mac/Linux (OpenSSL)</span></span>
<span data-ttu-id="3fc1f-125">Használhat [OpenSSL](http://www.openssl.org/) toocreate saját felügyeleti tanúsítványát.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-125">You can use [OpenSSL](http://www.openssl.org/) toocreate your management certificate.</span></span>  <span data-ttu-id="3fc1f-126">Toocreate két tanúsítványt, egy a hello server ténylegesen szüksége (egy `.cer` fájl), a másik hello ügyfél (egy `.pem` fájlt).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-126">You actually need toocreate two certificates, one for hello server (a `.cer` file) and one for hello client (a `.pem` file).</span></span> <span data-ttu-id="3fc1f-127">toocreate hello `.pem` fájlt, hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-127">toocreate hello `.pem` file, execute:</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="3fc1f-128">toocreate hello `.cer` tanúsítvány, hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-128">toocreate hello `.cer` certificate, execute:</span></span>

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

<span data-ttu-id="3fc1f-129">Az Azure-tanúsítványokkal kapcsolatos további információkért lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-129">For more information about Azure certificates, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="3fc1f-130">OpenSSL-paraméter teljes leírását lásd: hello dokumentációját a [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-130">For a complete description of OpenSSL parameters, see hello documentation at [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span></span>

<span data-ttu-id="3fc1f-131">Miután létrehozta ezeket a fájlokat, meg kell-e tooupload hello `.cer` keresztül hello lapja hello "Beállítások" hello "Feltöltés" művelettel tooAzure fájl [a klasszikus Azure portálon][management-portal], és toomake jegyezze fel kell hello menteni `.pem` fájlt.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-131">After you have created these files, you need tooupload hello `.cer` file tooAzure via hello "Upload" action of hello "Settings" tab of hello [Azure classic portal][management-portal], and you need toomake note of where you saved hello `.pem` file.</span></span>

<span data-ttu-id="3fc1f-132">Ha beszerezte az előfizetés-Azonosítóval, létrehozott egy tanúsítványt, és fel kell tölteni a hello `.cer` fájl tooAzure csatlakozáskor toohello Azure felügyeleti végpont úgy, hogy a hello előfizetése azonosítóját és hello elérési toohello `.pem` fájl túl**ServiceManagementService**:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-132">After you have obtained your subscription ID, created a certificate, and uploaded hello `.cer` file tooAzure, you can connect toohello Azure management endpoint by passing hello subscription id and hello path toohello `.pem` file too**ServiceManagementService**:</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="3fc1f-133">Az előző példában hello `sms` van egy **ServiceManagementService** objektum.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-133">In hello preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="3fc1f-134">Hello **ServiceManagementService** osztály hello használt elsődleges osztály toomanage van Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-134">hello **ServiceManagementService** class is hello primary class used toomanage Azure services.</span></span>

### <a name="management-certificates-on-windows-makecert"></a><span data-ttu-id="3fc1f-135">A Windows (MakeCert) felügyeleti tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="3fc1f-135">Management certificates on Windows (MakeCert)</span></span>
<span data-ttu-id="3fc1f-136">Létrehozhat egy önaláírt felügyeleti tanúsítvány a gép használt `makecert.exe`.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-136">You can create a self-signed management certificate on your machine using `makecert.exe`.</span></span>  <span data-ttu-id="3fc1f-137">Nyissa meg a **Visual Studio parancssorból** , egy **rendszergazda** , és használja a következő parancsot, hogy hello *AzureCertificate* szeretné hello tanúsítványnévvel rendelkező toouse.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-137">Open a **Visual Studio command prompt** as an **administrator** and use hello following command, replacing *AzureCertificate* with hello certificate name you would like toouse.</span></span>

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

<span data-ttu-id="3fc1f-138">hello parancs létrehoz hello `.cer` fájlt, és telepíti azt a hello **személyes** tanúsítványtárolójába.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-138">hello command creates hello `.cer` file, and installs it in hello **Personal** certificate store.</span></span> <span data-ttu-id="3fc1f-139">További információkért lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-139">For more information, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span>

<span data-ttu-id="3fc1f-140">Hello tanúsítvány létrehozását követően tooupload hello kell `.cer` keresztül hello lapja hello "Beállítások" hello "Feltöltés" művelettel tooAzure fájl [a klasszikus Azure portálon][management-portal].</span><span class="sxs-lookup"><span data-stu-id="3fc1f-140">After you have created hello certificate, you need tooupload hello `.cer` file tooAzure via hello "Upload" action of hello "Settings" tab of hello [Azure classic portal][management-portal].</span></span>

<span data-ttu-id="3fc1f-141">Ha beszerezte az előfizetés-Azonosítóval, létrehozott egy tanúsítványt, és fel kell tölteni a hello `.cer` fájl tooAzure csatlakozáskor toohello Azure felügyeleti végpont átadásával hello előfizetése azonosítóját és hello tanúsítvány hello helyét a **Személyes** tanúsítványtárolót túl**ServiceManagementService** (ebben az esetben cserélje le *AzureCertificate* hello nevet, a tanúsítvány):</span><span class="sxs-lookup"><span data-stu-id="3fc1f-141">After you have obtained your subscription ID, created a certificate, and uploaded hello `.cer` file tooAzure, you can connect toohello Azure management endpoint by passing hello subscription id and hello location of hello certificate in your **Personal** certificate store too**ServiceManagementService** (again, replace *AzureCertificate* with hello name of your certificate):</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="3fc1f-142">Az előző példában hello `sms` van egy **ServiceManagementService** objektum.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-142">In hello preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="3fc1f-143">Hello **ServiceManagementService** osztály hello használt elsődleges osztály toomanage van Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-143">hello **ServiceManagementService** class is hello primary class used toomanage Azure services.</span></span>

## <span data-ttu-id="3fc1f-144"><a name="ListAvailableLocations"></a>Hogyan: elérhető helyről felsorolása</span><span class="sxs-lookup"><span data-stu-id="3fc1f-144"><a name="ListAvailableLocations"> </a>How to: List available locations</span></span>
<span data-ttu-id="3fc1f-145">toolist hello helyeken elérhető szolgáltatásait üzemeltető, használja a hello **lista\_helyek** módszert:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-145">toolist hello locations that are available for hosting services, use hello **list\_locations** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

<span data-ttu-id="3fc1f-146">Amikor létrehoz egy felhőalapú szolgáltatás, vagy a társzolgáltatás tooprovide egy érvényes helyet kell.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-146">When you create a cloud service or storage service you need tooprovide a valid location.</span></span> <span data-ttu-id="3fc1f-147">Hello **lista\_helyek** metódus mindig hello jelenleg elérhető helyről naprakész listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-147">hello **list\_locations** method always returns an up-to-date list of hello currently available locations.</span></span> <span data-ttu-id="3fc1f-148">Től írásának pillanatában hello elérhető helyről a következők:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-148">As of this writing, hello available locations are:</span></span>

* <span data-ttu-id="3fc1f-149">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="3fc1f-149">West Europe</span></span>
* <span data-ttu-id="3fc1f-150">Észak-Európa</span><span class="sxs-lookup"><span data-stu-id="3fc1f-150">North Europe</span></span>
* <span data-ttu-id="3fc1f-151">Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="3fc1f-151">Southeast Asia</span></span>
* <span data-ttu-id="3fc1f-152">Kelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="3fc1f-152">East Asia</span></span>
* <span data-ttu-id="3fc1f-153">USA középső régiója</span><span class="sxs-lookup"><span data-stu-id="3fc1f-153">Central US</span></span>
* <span data-ttu-id="3fc1f-154">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="3fc1f-154">North Central US</span></span>
* <span data-ttu-id="3fc1f-155">USA déli középső régiója</span><span class="sxs-lookup"><span data-stu-id="3fc1f-155">South Central US</span></span>
* <span data-ttu-id="3fc1f-156">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="3fc1f-156">West US</span></span>
* <span data-ttu-id="3fc1f-157">USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="3fc1f-157">East US</span></span>
* <span data-ttu-id="3fc1f-158">Kelet-Japán</span><span class="sxs-lookup"><span data-stu-id="3fc1f-158">Japan East</span></span>
* <span data-ttu-id="3fc1f-159">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="3fc1f-159">Japan West</span></span>
* <span data-ttu-id="3fc1f-160">Dél-Brazília</span><span class="sxs-lookup"><span data-stu-id="3fc1f-160">Brazil South</span></span>
* <span data-ttu-id="3fc1f-161">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="3fc1f-161">Australia East</span></span>
* <span data-ttu-id="3fc1f-162">Délkelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="3fc1f-162">Australia Southeast</span></span>

## <span data-ttu-id="3fc1f-163"><a name="CreateCloudService"></a>Hogyan: felhőalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fc1f-163"><a name="CreateCloudService"> </a>How to: Create a cloud service</span></span>
<span data-ttu-id="3fc1f-164">Amikor létrehoz egy alkalmazást, és futtassa az Azure-ban, hello kódjával és a konfigurációjával együtt nevezzük az Azure [felhőalapú szolgáltatás] [ cloud service] (néven egy *üzemeltetett szolgáltatás* a korábbi Azure ad).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-164">When you create an application and run it in Azure, hello code and configuration together are called an Azure [cloud service][cloud service] (known as a *hosted service* in earlier Azure releases).</span></span> <span data-ttu-id="3fc1f-165">Hello **létrehozása\_üzemeltetett\_szolgáltatás** módszer lehetővé teszi egy új üzemeltetett szolgáltatás toocreate azáltal, hogy egy futtatott szolgáltatás nevét (amelyen az Azure-ban egyedinek kell lennie), a címkék (automatikusan kódolt toobase64), egy Leírás, és egy helyet.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-165">hello **create\_hosted\_service** method allows you toocreate a new hosted service by providing a hosted service name (which must be unique in Azure), a label (automatically encoded toobase64), a description, and a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

<span data-ttu-id="3fc1f-166">Hello előfizetés az összes üzemeltetett hello szolgáltatást is listázhatja **lista\_üzemeltetett\_szolgáltatások** módszert:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-166">You can list all hello hosted services for your subscription with hello **list\_hosted\_services** method:</span></span>

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

<span data-ttu-id="3fc1f-167">Ha azt szeretné, hogy egy adott tárolt szolgáltatás adatainak tooget, azt is megteheti úgy, hogy hello üzemeltetett szolgáltatás neve toohello **beolvasása\_üzemeltetett\_szolgáltatás\_tulajdonságok** módszert:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-167">If you want tooget information about a particular hosted service, you can do so by passing hello hosted service name toohello **get\_hosted\_service\_properties** method:</span></span>

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

<span data-ttu-id="3fc1f-168">Miután létrehozott egy felhőalapú szolgáltatás, a kód toohello hello szolgáltatást telepítheti **létrehozása\_telepítési** metódust.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-168">After you have created a cloud service, you can deploy your code toohello service with hello **create\_deployment** method.</span></span>

## <span data-ttu-id="3fc1f-169"><a name="DeleteCloudService"></a>Hogyan: egy felhőalapú szolgáltatás törlése</span><span class="sxs-lookup"><span data-stu-id="3fc1f-169"><a name="DeleteCloudService"> </a>How to: Delete a cloud service</span></span>
<span data-ttu-id="3fc1f-170">Egy felhőalapú szolgáltatás törölheti hello szolgáltatás neve toohello átadásával **törlése\_üzemeltetett\_szolgáltatás** módszert:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-170">You can delete a cloud service by passing hello service name toohello **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service('myhostedservice')

<span data-ttu-id="3fc1f-171">A szolgáltatás törlése előtt hello szolgáltatás központi telepítéseinek először törölni kell.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-171">Before you can delete a service, all deployments for hello service must first be deleted.</span></span> <span data-ttu-id="3fc1f-172">(Lásd: [hogyan: törölje a központi telepítést](#DeleteDeployment) részletes.)</span><span class="sxs-lookup"><span data-stu-id="3fc1f-172">(See [How to: Delete a deployment](#DeleteDeployment) for details.)</span></span>

## <span data-ttu-id="3fc1f-173"><a name="DeleteDeployment"></a>Útmutató: a központi telepítés törlése</span><span class="sxs-lookup"><span data-stu-id="3fc1f-173"><a name="DeleteDeployment"> </a>How to: Delete a deployment</span></span>
<span data-ttu-id="3fc1f-174">a központi telepítés toodelete hello használata **törlése\_telepítési** metódust.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-174">toodelete a deployment, use hello **delete\_deployment** method.</span></span> <span data-ttu-id="3fc1f-175">hello következő példa bemutatja, hogyan toodelete a központi telepítés nevű `v1`.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-175">hello following example shows how toodelete a deployment named `v1`.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <span data-ttu-id="3fc1f-176"><a name="CreateStorageService"></a>Hogyan: storage szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fc1f-176"><a name="CreateStorageService"> </a>How to: Create a storage service</span></span>
<span data-ttu-id="3fc1f-177">A [társzolgáltatás](../storage/common/storage-create-storage-account.md) hozzáférést tud biztosítani, hogy tooAzure [Blobok](../storage/blobs/storage-python-how-to-use-blob-storage.md), [táblák](../cosmos-db/table-storage-how-to-use-python.md), és [várólisták](../storage/queues/storage-python-how-to-use-queue-storage.md).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-177">A [storage service](../storage/common/storage-create-storage-account.md) gives you access tooAzure [Blobs](../storage/blobs/storage-python-how-to-use-blob-storage.md), [Tables](../cosmos-db/table-storage-how-to-use-python.md), and [Queues](../storage/queues/storage-python-how-to-use-queue-storage.md).</span></span> <span data-ttu-id="3fc1f-178">toocreate tárolási szolgáltatásként, van szüksége (kisbetűket 3 és 24 közötti és egyedi az Azure) hello szolgáltatás nevét, leírását, (felfelé too100 karakterek, automatikusan kódolt toobase64) címke és egy helyet.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-178">toocreate a storage service, you need a name for hello service (between 3 and 24 lowercase characters and unique within Azure), a description, a label (up too100 characters, automatically encoded toobase64), and a location.</span></span> <span data-ttu-id="3fc1f-179">hello a következő példa bemutatja, hogyan toocreate tárolási funkciókat biztosító szolgáltatást, a hely megadásával.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-179">hello following example shows how toocreate a storage service by specifying a location.</span></span>

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

<span data-ttu-id="3fc1f-180">Vegye figyelembe, hogy a fenti példa hello hello hello állapotának **létrehozása\_tárolási\_fiók** művelet által visszaadott eredmény hello átadásával kérhető **létrehozása\_tároló \_fiók** toohello **beolvasása\_művelet\_állapot** metódust.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-180">Note in hello preceding example that hello status of hello **create\_storage\_account** operation can be retrieved by passing hello result returned by **create\_storage\_account** toohello **get\_operation\_status** method.</span></span>  

<span data-ttu-id="3fc1f-181">A storage-fiókok és a hello tulajdonságaik listázhatja **lista\_tárolási\_fiókok** módszer:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-181">You can list your storage accounts and their properties with hello **list\_storage\_accounts** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <span data-ttu-id="3fc1f-182"><a name="DeleteStorageService"></a>Útmutató: a társzolgáltatás törlése</span><span class="sxs-lookup"><span data-stu-id="3fc1f-182"><a name="DeleteStorageService"> </a>How to: Delete a storage service</span></span>
<span data-ttu-id="3fc1f-183">Törölheti a társzolgáltatás úgy, hogy hello tárolási szolgáltatás neve toohello **törlése\_tárolási\_fiók** metódust.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-183">You can delete a storage service by passing hello storage service name toohello **delete\_storage\_account** method.</span></span> <span data-ttu-id="3fc1f-184">A storage szolgáltatás törlése (blobot, táblát és üzenetsort) hello szolgáltatásban tárolt összes adat törlése.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-184">Deleting a storage service deletes all data stored in hello service (blobs, tables, and queues).</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <span data-ttu-id="3fc1f-185"><a name="ListOperatingSystems"></a>Hogyan: listából a rendelkezésre álló operációs rendszeren</span><span class="sxs-lookup"><span data-stu-id="3fc1f-185"><a name="ListOperatingSystems"> </a>How to: List available operating systems</span></span>
<span data-ttu-id="3fc1f-186">toolist hello operációs rendszerek szolgáltatásait üzemeltető, rendelkezésre álló hello használata **lista\_működő\_rendszerek** módszert:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-186">toolist hello operating systems that are available for hosting services, use hello **list\_operating\_systems** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

<span data-ttu-id="3fc1f-187">Másik lehetőségként használhatja a hello **lista\_működő\_rendszer\_családok** metódus, amely csoportosítja hello operációs rendszerek termékcsalád:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-187">Alternatively, you can use hello **list\_operating\_system\_families** method, which groups hello operating systems by family:</span></span>

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <span data-ttu-id="3fc1f-188"><a name="CreateVMImage"></a>Hogyan: hozzon létre egy operációsrendszer-lemezképet</span><span class="sxs-lookup"><span data-stu-id="3fc1f-188"><a name="CreateVMImage"> </a>How to: Create an operating system image</span></span>
<span data-ttu-id="3fc1f-189">az operációs rendszer lemezképének toohello lemezképtárba tooadd hello használata **hozzáadása\_os\_kép** metódus:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-189">tooadd an operating system image toohello image repository, use hello **add\_os\_image** method:</span></span>

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

<span data-ttu-id="3fc1f-190">toolist hello operációsrendszer-lemezképek a rendelkezésre álló hello használata **lista\_os\_képek** metódust.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-190">toolist hello operating system images that are available, use hello **list\_os\_images** method.</span></span> <span data-ttu-id="3fc1f-191">Összes platform képek és felhasználói képeket tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-191">It includes all platform images and user images:</span></span>

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

## <span data-ttu-id="3fc1f-192"><a name="DeleteVMImage"></a>Útmutató: az operációsrendszer-lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="3fc1f-192"><a name="DeleteVMImage"> </a>How to: Delete an operating system image</span></span>
<span data-ttu-id="3fc1f-193">egy felhasználói lemezkép toodelete hello használata **törlése\_os\_kép** módszer:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-193">toodelete a user image, use hello **delete\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <span data-ttu-id="3fc1f-194"><a name="CreateVM"></a>Útmutató: virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fc1f-194"><a name="CreateVM"> </a>How to: Create a virtual machine</span></span>
<span data-ttu-id="3fc1f-195">a virtuális gépek toocreate, először toocreate egy [felhőalapú szolgáltatás](#CreateCloudService).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-195">toocreate a virtual machine, you first need toocreate a [cloud service](#CreateCloudService).</span></span>  <span data-ttu-id="3fc1f-196">Ezután hozzon létre hello virtuálisgép-példány használatával hello **létrehozása\_virtuális\_gép\_telepítési** metódus:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-196">Then create hello virtual machine deployment using hello **create\_virtual\_machine\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set hello location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where hello VM disk
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

## <span data-ttu-id="3fc1f-197"><a name="DeleteVM"></a>Útmutató: virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="3fc1f-197"><a name="DeleteVM"> </a>How to: Delete a virtual machine</span></span>
<span data-ttu-id="3fc1f-198">a virtuális gépek toodelete, előbb törölnie hello állításra hello **törlése\_telepítési** módszer:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-198">toodelete a virtual machine, you first delete hello deployment using hello **delete\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

<span data-ttu-id="3fc1f-199">hello felhőszolgáltatás majd törölheti hello segítségével **törlése\_üzemeltetett\_szolgáltatás** módszert:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-199">hello cloud service can then be deleted using hello **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a><span data-ttu-id="3fc1f-200">A virtuális gép létrehozása a rögzített virtuálisgép-lemezkép</span><span class="sxs-lookup"><span data-stu-id="3fc1f-200">How To: Create a Virtual Machine from a Captured Virtual Machine Image</span></span>
<span data-ttu-id="3fc1f-201">egy Virtuálisgép-lemezkép toocapture, először meghívja a hello **rögzítése\_vm\_kép** módszer:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-201">toocapture a VM image, you first call hello **capture\_vm\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace hello below three parameters with actual values
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

<span data-ttu-id="3fc1f-202">Ezt követően toomake arról, hogy hello lemezkép rögzítése sikerült, használja a hello **lista\_vm\_képek** API-t, és győződjön meg arról, hogy a lemezkép hello eredmény jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="3fc1f-202">Next, toomake sure that you have successfully captured hello image, use hello **list\_vm\_images** api, and make sure your image is displayed in hello results:</span></span>

    images = sms.list_vm_images()

<span data-ttu-id="3fc1f-203">toofinally hello rögzített lemezképpel hello virtuális gép létrehozása, használata hello **létrehozása\_virtuális\_gép\_telepítési** előtt, de ezúttal adjon át hello vm_image_name helyette módszer</span><span class="sxs-lookup"><span data-stu-id="3fc1f-203">toofinally create hello virtual machine using hello captured image, use hello **create\_virtual\_machine\_deployment** method as before, but this time pass in hello vm_image_name instead</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set hello location
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

<span data-ttu-id="3fc1f-204">További részletek toolearn, hogyan toocapture egy Linux virtuális gép: [hogyan tooCapture Linux virtuális gépek.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="3fc1f-204">toolearn more about how toocapture a Linux Virtual Machine, see [How tooCapture a Linux Virtual Machine.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="3fc1f-205">További részletek toolearn, hogyan toocapture egy Windows rendszerű virtuális gép: [hogyan tooCapture Windows virtuális gépként.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="3fc1f-205">toolearn more about how toocapture a Windows Virtual Machine, see [How tooCapture a Windows Virtual Machine.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="3fc1f-206"><a name="What's Next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3fc1f-206"><a name="What's Next"> </a>Next Steps</span></span>
<span data-ttu-id="3fc1f-207">Most, hogy megismerte a service management hello alapjait, hogy elérhető-hello [teljes API referenciadokumentációt tartalmaz hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) , és végezze el összetett feladatokat könnyen toomanage a python alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3fc1f-207">Now that you've learned hello basics of service management, you can access hello [Complete API reference documentation for hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) and perform complex tasks easily toomanage your python application.</span></span>

<span data-ttu-id="3fc1f-208">További információkért lásd: hello [Python fejlesztői központ](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="3fc1f-208">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect tooservice management]: #Connect
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
