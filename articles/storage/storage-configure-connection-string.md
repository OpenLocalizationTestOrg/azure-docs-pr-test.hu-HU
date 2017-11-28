---
title: "az Azure Storage kapcsolati karakterláncot aaaConfigure |} Microsoft Docs"
description: "Az Azure-tárfiók kapcsolati karakterláncának konfigurálása. A kapcsolati karakterlánc szükséges tooauthenticate tooa tárfiók az alkalmazásból futásidőben hello információkat tartalmaz."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: marsma
ms.openlocfilehash: 80c38a6f8f0d4f06b99e7c487647b984e01d1772
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-storage-connection-strings"></a><span data-ttu-id="9ba6b-104">Configure Azure Storage connection strings (Az Azure Storage kapcsolati karakterláncok konfigurálása)</span><span class="sxs-lookup"><span data-stu-id="9ba6b-104">Configure Azure Storage connection strings</span></span>

<span data-ttu-id="9ba6b-105">Egy kapcsolati karakterláncot tartalmaz, az alkalmazásadatok tooaccess a futási időben Azure Storage-fiók szükséges hello hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-105">A connection string includes hello authentication information required for your application tooaccess data in an Azure Storage account at runtime.</span></span> <span data-ttu-id="9ba6b-106">Konfigurálhatja a kapcsolati karakterláncokkal:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-106">You can configure connection strings to:</span></span>

* <span data-ttu-id="9ba6b-107">Csatlakozás az Azure storage emulator toohello.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-107">Connect toohello Azure storage emulator.</span></span>
* <span data-ttu-id="9ba6b-108">Az Azure-ban a tárfiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-108">Access a storage account in Azure.</span></span>
* <span data-ttu-id="9ba6b-109">A közös hozzáférésű jogosultságkód (SAS) keresztül az Azure-ban megadott erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-109">Access specified resources in Azure via a shared access signature (SAS).</span></span>

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a><span data-ttu-id="9ba6b-110">A kapcsolati karakterlánc tárolásának</span><span class="sxs-lookup"><span data-stu-id="9ba6b-110">Storing your connection string</span></span>
<span data-ttu-id="9ba6b-111">Az alkalmazás tooaccess hello kapcsolati karakterláncot, futásidejű tooauthenticate kérelmek tooAzure tárolási kell.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-111">Your application needs tooaccess hello connection string at runtime tooauthenticate requests made tooAzure Storage.</span></span> <span data-ttu-id="9ba6b-112">A kapcsolati karakterlánc tárolására több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-112">You have several options for storing your connection string:</span></span>

* <span data-ttu-id="9ba6b-113">Az alkalmazást futtató hello asztalon vagy egy eszközön tárolhatja hello kapcsolati karakterlánc egy **app.config** vagy **web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-113">An application running on hello desktop or on a device can store hello connection string in an **app.config** or **web.config** file.</span></span> <span data-ttu-id="9ba6b-114">Adja hozzá a hello kapcsolati karakterlánc toohello **AppSettings** szakasz ezeket a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-114">Add hello connection string toohello **AppSettings** section in these files.</span></span>
* <span data-ttu-id="9ba6b-115">Egy Azure-felhőszolgáltatásban futó alkalmazáshoz hello kapcsolati karakterlánc tárolhat hello [Azure szolgáltatás konfigurációs (.cscfg) fájl](https://msdn.microsoft.com/library/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ba6b-115">An application running in an Azure cloud service can store hello connection string in hello [Azure service configuration schema (.cscfg) file](https://msdn.microsoft.com/library/ee758710.aspx).</span></span> <span data-ttu-id="9ba6b-116">Adja hozzá a hello kapcsolati karakterlánc toohello **ConfigurationSettings** hello szolgáltatás konfigurációs fájl részét.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-116">Add hello connection string toohello **ConfigurationSettings** section of hello service configuration file.</span></span>
* <span data-ttu-id="9ba6b-117">A kapcsolati karakterláncot a kódban közvetlenül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-117">You can use your connection string directly in your code.</span></span> <span data-ttu-id="9ba6b-118">Azt javasoljuk azonban, hogy a legtöbb esetben egy konfigurációs fájlban tárolhatja a kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-118">However, we recommend that you store your connection string in a configuration file in most scenarios.</span></span>

<span data-ttu-id="9ba6b-119">A kapcsolati karakterlánc egy konfigurációs fájlban tárolja teszi könnyen tooupdate hello kapcsolati karakterlánc tooswitch hello storage emulator és az Azure-tárfiók közötti hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-119">Storing your connection string in a configuration file makes it easy tooupdate hello connection string tooswitch between hello storage emulator and an Azure storage account in hello cloud.</span></span> <span data-ttu-id="9ba6b-120">Csak akkor kell tooedit hello kapcsolati karakterlánc toopoint tooyour célkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-120">You only need tooedit hello connection string toopoint tooyour target environment.</span></span>

<span data-ttu-id="9ba6b-121">Használhatja a hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess a kapcsolati karakterlánc, függetlenül attól, ahol az alkalmazás fut-e futásidőben.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-121">You can use hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess your connection string at runtime regardless of where your application is running.</span></span>

## <a name="create-a-connection-string-for-hello-storage-emulator"></a><span data-ttu-id="9ba6b-122">Hello storage Emulator kapcsolati karakterlánc létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ba6b-122">Create a connection string for hello storage emulator</span></span>
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

<span data-ttu-id="9ba6b-123">Hello storage emulator kapcsolatos további információkért lásd: [hello Azure storage Emulatorral a fejlesztéshez és teszteléshez](storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="9ba6b-123">For more information about hello storage emulator, see [Use hello Azure storage emulator for development and testing](storage-use-emulator.md).</span></span>

## <a name="create-a-connection-string-for-an-azure-storage-account"></a><span data-ttu-id="9ba6b-124">Hozzon létre egy Azure-tárfiók kapcsolati karakterlánca</span><span class="sxs-lookup"><span data-stu-id="9ba6b-124">Create a connection string for an Azure storage account</span></span>
<span data-ttu-id="9ba6b-125">toocreate az Azure-tárfiók kapcsolati karakterláncot, használja hello formátuma.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-125">toocreate a connection string for your Azure storage account, use hello following format.</span></span> <span data-ttu-id="9ba6b-126">Azt jelzi, hogy tooconnect toohello tárfiók keresztül (ajánlott) HTTPS vagy HTTP, cserélje le `myAccountName` hello nevet, a tárfiók, és cserélje ki `myAccountKey` rendelkező a hívóbetűre:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-126">Indicate whether you want tooconnect toohello storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with hello name of your storage account, and replace `myAccountKey` with your account access key:</span></span>

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

<span data-ttu-id="9ba6b-127">Például a kapcsolati karakterlánc hasonlóan néznének ki:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-127">For example, your connection string might look similar to:</span></span>

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

<span data-ttu-id="9ba6b-128">Bár az Azure Storage támogatja a HTTP és HTTPS kapcsolat-karakterláncban *HTTPS erősen ajánlott*.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-128">Although Azure Storage supports both HTTP and HTTPS in a connection string, *HTTPS is highly recommended*.</span></span>

> [!TIP]
> <span data-ttu-id="9ba6b-129">A tárfiók kapcsolati karakterláncok hello található [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9ba6b-129">You can find your storage account's connection strings in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9ba6b-130">Keresse meg a túl**beállítások** > **hívóbetűk** a tárfiók menü panel toosee kapcsolati karakterláncaiban mindkét elsődleges és másodlagos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-130">Navigate too**SETTINGS** > **Access keys** in your storage account's menu blade toosee connection strings for both primary and secondary access keys.</span></span>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a><span data-ttu-id="9ba6b-131">A kapcsolati karakterlánc használatával a közös hozzáférésű jogosultságkód létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ba6b-131">Create a connection string using a shared access signature</span></span>
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a><span data-ttu-id="9ba6b-132">Explicit tárolási a végpont-kapcsolati karakterlánc létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ba6b-132">Create a connection string for an explicit storage endpoint</span></span>
<span data-ttu-id="9ba6b-133">Explicit Szolgáltatásvégpontok adhat meg a kapcsolati karakterlánc alapértelmezett végpontok hello használata helyett.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-133">You can specify explicit service endpoints in your connection string instead of using hello default endpoints.</span></span> <span data-ttu-id="9ba6b-134">toocreate egy kapcsolati karakterláncot, amely meghatározza egy explicit végpont hello teljes szolgáltatási végpont minden egyes szolgáltatás hello protokoll specifikációja (HTTPS (ajánlott) vagy HTTP), beleértve hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-134">toocreate a connection string that specifies an explicit endpoint, specify hello complete service endpoint for each service, including hello protocol specification (HTTPS (recommended) or HTTP), in hello following format:</span></span>

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

<span data-ttu-id="9ba6b-135">Egy olyan forgatókönyv, ahol előfordulhat, hogy kívánja toospecify explicit végpont esetén a Blob storage endpoint tooa átnevezte [egyéni tartomány](storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="9ba6b-135">One scenario where you might wish toospecify an explicit endpoint is when you've mapped your Blob storage endpoint tooa [custom domain](storage-custom-domain-name.md).</span></span> <span data-ttu-id="9ba6b-136">Ebben az esetben a kapcsolódási karakterláncban a Blob Storage-egyéni végpontot is megadhat.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-136">In that case, you can specify your custom endpoint for Blob storage in your connection string.</span></span> <span data-ttu-id="9ba6b-137">Opcionálisan megadhat hello alapértelmezett végpontok a hello más szolgáltatások Ha az alkalmazás azokat.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-137">You can optionally specify hello default endpoints for hello other services if your application uses them.</span></span>

<span data-ttu-id="9ba6b-138">Íme egy példa egy kapcsolati karakterláncot, amely meghatározza egy explicit végpontra hello Blob szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-138">Here is an example of a connection string that specifies an explicit endpoint for hello Blob service:</span></span>

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="9ba6b-139">Ebben a példában az összes szolgáltatáshoz, például egy egyéni tartományt a Blob szolgáltatás hello explicit végpontok határozza meg:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-139">This example specifies explicit endpoints for all services, including a custom domain for hello Blob service:</span></span>

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="9ba6b-140">hello végpont értékek kapcsolat-karakterláncban használt tooconstruct hello kérelem URI-azonosítók toohello tárolási szolgáltatások, és bármely URI-azonosítók tooyour kód visszaadott hello formája előírják.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-140">hello endpoint values in a connection string are used tooconstruct hello request URIs toohello storage services, and dictate hello form of any URIs that are returned tooyour code.</span></span>

<span data-ttu-id="9ba6b-141">Ha átnevezte tárolási végpont tooa egyéni tartományt, és hagyja ki ezt a végpont egy kapcsolati karakterláncból, akkor nem fogja tudni toouse az, hogy a kapcsolati karakterlánc tooaccess adatokat, hogy a felhasználói kódból szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-141">If you've mapped a storage endpoint tooa custom domain and omit that endpoint from a connection string, then you will not be able toouse that connection string tooaccess data in that service from your code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9ba6b-142">Lehet, hogy a szolgáltatási végpont értékeit a kapcsolati karakterláncok megfelelően formázott URI-k, beleértve a `https://` (ajánlott) vagy `http://`.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-142">Service endpoint values in your connection strings must be well-formed URIs, including `https://` (recommended) or `http://`.</span></span> <span data-ttu-id="9ba6b-143">Azure Storage egyelőre nem támogatják a HTTPS az egyéni tartományok, mert Ön *kell* meg `http://` a tetszőleges végpontot URI, amely az egyéni tartomány tooa mutat.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-143">Because Azure Storage does not yet support HTTPS for custom domains, you *must* specify `http://` for any endpoint URI that points tooa custom domain.</span></span>
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a><span data-ttu-id="9ba6b-144">Hozzon létre egy kapcsolati karakterláncot egy végpont utótag</span><span class="sxs-lookup"><span data-stu-id="9ba6b-144">Create a connection string with an endpoint suffix</span></span>
<span data-ttu-id="9ba6b-145">toocreate egy kapcsolati karakterlánc egy társzolgáltatás régiókban, vagy másik végpont utótagok példányok többek között az Azure China vagy az Azure Governmentnek a következő kapcsolati karakterlánc-formátum használata hello.</span><span class="sxs-lookup"><span data-stu-id="9ba6b-145">toocreate a connection string for a storage service in regions or instances with different endpoint suffixes, such as for Azure China or Azure Government, use hello following connection string format.</span></span> <span data-ttu-id="9ba6b-146">Azt jelzi, hogy tooconnect toohello tárfiók keresztül (ajánlott) HTTPS vagy HTTP, cserélje le `myAccountName` cserélje a tárfiókja hello nevű `myAccountKey` a hívóbetűre, és cserélje ki a `mySuffix` a hello URI utótag:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-146">Indicate whether you want tooconnect toohello storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with hello name of your storage account, replace `myAccountKey` with your account access key, and replace `mySuffix` with hello URI suffix:</span></span>

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

<span data-ttu-id="9ba6b-147">Íme egy példa kapcsolati karakterláncot az Azure Kínában tárolási szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="9ba6b-147">Here's an example connection string for storage services in Azure China:</span></span>

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a><span data-ttu-id="9ba6b-148">A kapcsolódási karakterlánc elemzésekor</span><span class="sxs-lookup"><span data-stu-id="9ba6b-148">Parsing a connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a><span data-ttu-id="9ba6b-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ba6b-149">Next steps</span></span>
* [<span data-ttu-id="9ba6b-150">Hello Azure storage emulatorral a fejlesztéshez és teszteléshez</span><span class="sxs-lookup"><span data-stu-id="9ba6b-150">Use hello Azure storage emulator for development and testing</span></span>](storage-use-emulator.md)
* [<span data-ttu-id="9ba6b-151">Az Azure Storage-kezelők</span><span class="sxs-lookup"><span data-stu-id="9ba6b-151">Azure Storage explorers</span></span>](storage-explorers.md)
* [<span data-ttu-id="9ba6b-152">Közös hozzáférésű Jogosultságkód (SAS) használatával</span><span class="sxs-lookup"><span data-stu-id="9ba6b-152">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)

