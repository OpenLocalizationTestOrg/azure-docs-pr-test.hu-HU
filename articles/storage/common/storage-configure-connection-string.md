---
title: "Az Azure Storage kapcsolati karakterláncának konfigurálása |} Microsoft Docs"
description: "Az Azure-tárfiók kapcsolati karakterláncának konfigurálása. A kapcsolati karakterlánc hitelesíti a hozzáférést a tárfiókhoz futásidőben az alkalmazásból szükséges információkat tartalmazza."
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
ms.openlocfilehash: 4b21e75fde55f195362809ce486a2615954ff93c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-azure-storage-connection-strings"></a><span data-ttu-id="1f8fb-104">Configure Azure Storage connection strings (Az Azure Storage kapcsolati karakterláncok konfigurálása)</span><span class="sxs-lookup"><span data-stu-id="1f8fb-104">Configure Azure Storage connection strings</span></span>

<span data-ttu-id="1f8fb-105">A kapcsolati karakterláncok futásidőben Azure Storage-fiók hozzáférési adatainak az alkalmazáshoz szükséges hitelesítési adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-105">A connection string includes the authentication information required for your application to access data in an Azure Storage account at runtime.</span></span> <span data-ttu-id="1f8fb-106">Konfigurálhatja a kapcsolati karakterláncokkal:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-106">You can configure connection strings to:</span></span>

* <span data-ttu-id="1f8fb-107">Az Azure storage emulator csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-107">Connect to the Azure storage emulator.</span></span>
* <span data-ttu-id="1f8fb-108">Az Azure-ban a tárfiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-108">Access a storage account in Azure.</span></span>
* <span data-ttu-id="1f8fb-109">A közös hozzáférésű jogosultságkód (SAS) keresztül az Azure-ban megadott erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-109">Access specified resources in Azure via a shared access signature (SAS).</span></span>

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a><span data-ttu-id="1f8fb-110">A kapcsolati karakterlánc tárolásának</span><span class="sxs-lookup"><span data-stu-id="1f8fb-110">Storing your connection string</span></span>
<span data-ttu-id="1f8fb-111">Az alkalmazás hozzá kell férniük az Azure Storage intézett kérelmek hitelesítéséhez szükséges futásidőben a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-111">Your application needs to access the connection string at runtime to authenticate requests made to Azure Storage.</span></span> <span data-ttu-id="1f8fb-112">A kapcsolati karakterlánc tárolására több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-112">You have several options for storing your connection string:</span></span>

* <span data-ttu-id="1f8fb-113">Egy alkalmazás fut, az asztalon vagy egy eszközön tárolhatja a kapcsolati karakterlánc egy **app.config** vagy **web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-113">An application running on the desktop or on a device can store the connection string in an **app.config** or **web.config** file.</span></span> <span data-ttu-id="1f8fb-114">A kapcsolati karakterláncot adja hozzá a **AppSettings** szakasz ezeket a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-114">Add the connection string to the **AppSettings** section in these files.</span></span>
* <span data-ttu-id="1f8fb-115">Egy Azure-felhőszolgáltatásban futó valamelyik alkalmazás a kapcsolati karakterlánc tárolhatja a [Azure szolgáltatás konfigurációs (.cscfg) fájl](https://msdn.microsoft.com/library/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="1f8fb-115">An application running in an Azure cloud service can store the connection string in the [Azure service configuration schema (.cscfg) file](https://msdn.microsoft.com/library/ee758710.aspx).</span></span> <span data-ttu-id="1f8fb-116">A kapcsolati karakterláncot adja hozzá a **ConfigurationSettings** a szolgáltatás konfigurációs fájl részét.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-116">Add the connection string to the **ConfigurationSettings** section of the service configuration file.</span></span>
* <span data-ttu-id="1f8fb-117">A kapcsolati karakterláncot a kódban közvetlenül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-117">You can use your connection string directly in your code.</span></span> <span data-ttu-id="1f8fb-118">Azt javasoljuk azonban, hogy a legtöbb esetben egy konfigurációs fájlban tárolhatja a kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-118">However, we recommend that you store your connection string in a configuration file in most scenarios.</span></span>

<span data-ttu-id="1f8fb-119">A kapcsolati karakterlánc egy konfigurációs fájlban tárolja megkönnyíti, hogy a kapcsolati karakterláncot Váltás a storage emulator és a felhőben Azure storage-fiók frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-119">Storing your connection string in a configuration file makes it easy to update the connection string to switch between the storage emulator and an Azure storage account in the cloud.</span></span> <span data-ttu-id="1f8fb-120">Csak akkor módosíthatók, hogy a célkörnyezet mutasson a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-120">You only need to edit the connection string to point to your target environment.</span></span>

<span data-ttu-id="1f8fb-121">Használhatja a [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) a kapcsolati karakterláncot, függetlenül attól, amelyen fut az alkalmazás futásidőben eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-121">You can use the [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) to access your connection string at runtime regardless of where your application is running.</span></span>

## <a name="create-a-connection-string-for-the-storage-emulator"></a><span data-ttu-id="1f8fb-122">A storage Emulator kapcsolati karakterlánc létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f8fb-122">Create a connection string for the storage emulator</span></span>
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

<span data-ttu-id="1f8fb-123">A storage emulator kapcsolatos további információkért lásd: [fejlesztéshez és teszteléshez használja az Azure storage emulator](storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="1f8fb-123">For more information about the storage emulator, see [Use the Azure storage emulator for development and testing](storage-use-emulator.md).</span></span>

## <a name="create-a-connection-string-for-an-azure-storage-account"></a><span data-ttu-id="1f8fb-124">Hozzon létre egy Azure-tárfiók kapcsolati karakterlánca</span><span class="sxs-lookup"><span data-stu-id="1f8fb-124">Create a connection string for an Azure storage account</span></span>
<span data-ttu-id="1f8fb-125">Az Azure-tárfiók kapcsolati karakterláncot, használja a következő formátumban.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-125">To create a connection string for your Azure storage account, use the following format.</span></span> <span data-ttu-id="1f8fb-126">Azt jelzi, hogy csatlakozni a tárfiók keresztül (ajánlott) HTTPS vagy HTTP, cserélje le `myAccountName` nevű, a tárfiók, és cserélje ki `myAccountKey` rendelkező a hívóbetűre:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-126">Indicate whether you want to connect to the storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with the name of your storage account, and replace `myAccountKey` with your account access key:</span></span>

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

<span data-ttu-id="1f8fb-127">Például a kapcsolati karakterlánc hasonlóan néznének ki:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-127">For example, your connection string might look similar to:</span></span>

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

<span data-ttu-id="1f8fb-128">Bár az Azure Storage támogatja a HTTP és HTTPS kapcsolat-karakterláncban *HTTPS erősen ajánlott*.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-128">Although Azure Storage supports both HTTP and HTTPS in a connection string, *HTTPS is highly recommended*.</span></span>

> [!TIP]
> <span data-ttu-id="1f8fb-129">A tárfiók kapcsolati karakterláncokat megtalálhatja a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1f8fb-129">You can find your storage account's connection strings in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="1f8fb-130">Navigáljon a **beállítások** > **hívóbetűk** a tárfiók menü panelen kapcsolati karakterláncainak mindkét elsődleges és másodlagos hívóbetűk megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-130">Navigate to **SETTINGS** > **Access keys** in your storage account's menu blade to see connection strings for both primary and secondary access keys.</span></span>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a><span data-ttu-id="1f8fb-131">A kapcsolati karakterlánc használatával a közös hozzáférésű jogosultságkód létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f8fb-131">Create a connection string using a shared access signature</span></span>
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a><span data-ttu-id="1f8fb-132">Explicit tárolási a végpont-kapcsolati karakterlánc létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f8fb-132">Create a connection string for an explicit storage endpoint</span></span>
<span data-ttu-id="1f8fb-133">Explicit Szolgáltatásvégpontok adhat meg a kapcsolati karakterlánc alapértelmezett végpontok használata helyett.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-133">You can specify explicit service endpoints in your connection string instead of using the default endpoints.</span></span> <span data-ttu-id="1f8fb-134">Hozzon létre egy kapcsolati karakterláncot, amely meghatározza egy explicit végpontot, adja meg a teljes szolgáltatási végpont az egyes szolgáltatásokhoz, beleértve a protokollt (HTTPS (ajánlott) vagy HTTP), a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-134">To create a connection string that specifies an explicit endpoint, specify the complete service endpoint for each service, including the protocol specification (HTTPS (recommended) or HTTP), in the following format:</span></span>

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

<span data-ttu-id="1f8fb-135">A Blob storage végponthoz átnevezte egy forgatókönyvet, ahol előfordulhat, hogy szeretne megadni egy explicit végpont esetén egy [egyéni tartomány](../blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="1f8fb-135">One scenario where you might wish to specify an explicit endpoint is when you've mapped your Blob storage endpoint to a [custom domain](../blobs/storage-custom-domain-name.md).</span></span> <span data-ttu-id="1f8fb-136">Ebben az esetben a kapcsolódási karakterláncban a Blob Storage-egyéni végpontot is megadhat.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-136">In that case, you can specify your custom endpoint for Blob storage in your connection string.</span></span> <span data-ttu-id="1f8fb-137">Opcionálisan megadhat alapértelmezett végpontok a többi szolgáltatást, ha az alkalmazás azokat.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-137">You can optionally specify the default endpoints for the other services if your application uses them.</span></span>

<span data-ttu-id="1f8fb-138">Íme egy példa egy kapcsolati karakterláncot, amely meghatározza a Blob szolgáltatás explicit végpont:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-138">Here is an example of a connection string that specifies an explicit endpoint for the Blob service:</span></span>

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="1f8fb-139">Ebben a példában az összes szolgáltatáshoz, beleértve a Blob szolgáltatás az egyéni tartománynév explicit végpontok határozza meg:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-139">This example specifies explicit endpoints for all services, including a custom domain for the Blob service:</span></span>

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

<span data-ttu-id="1f8fb-140">A végpont értékek a kapcsolati karakterláncban a kérelem URI-azonosítók a tárolási szolgáltatásokhoz, és bármely URI, amely a kódot a rendszer visszairányítja formájában előírják szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-140">The endpoint values in a connection string are used to construct the request URIs to the storage services, and dictate the form of any URIs that are returned to your code.</span></span>

<span data-ttu-id="1f8fb-141">Ha egy tárolási végpont már leképezve az egyéni tartománynév, és hagyja ki ezt a kapcsolati karakterláncot az adott végpontra, majd csak akkor használhatják a kapcsolati karakterlánc az, hogy a szolgáltatás a hozzáférési adatok a felhasználói kódból.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-141">If you've mapped a storage endpoint to a custom domain and omit that endpoint from a connection string, then you will not be able to use that connection string to access data in that service from your code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1f8fb-142">Lehet, hogy a szolgáltatási végpont értékeit a kapcsolati karakterláncok megfelelően formázott URI-k, beleértve a `https://` (ajánlott) vagy `http://`.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-142">Service endpoint values in your connection strings must be well-formed URIs, including `https://` (recommended) or `http://`.</span></span> <span data-ttu-id="1f8fb-143">Azure Storage egyelőre nem támogatják a HTTPS az egyéni tartományok, mert Ön *kell* meg `http://` a tetszőleges végpontot URI mutató egyéni tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-143">Because Azure Storage does not yet support HTTPS for custom domains, you *must* specify `http://` for any endpoint URI that points to a custom domain.</span></span>
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a><span data-ttu-id="1f8fb-144">Hozzon létre egy kapcsolati karakterláncot egy végpont utótag</span><span class="sxs-lookup"><span data-stu-id="1f8fb-144">Create a connection string with an endpoint suffix</span></span>
<span data-ttu-id="1f8fb-145">A régiókban, vagy másik végpont utótagok példányok társzolgáltatás kapcsolati karakterláncot, mint az Azure China vagy az Azure Governmentnek használja a következő kapcsolati karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="1f8fb-145">To create a connection string for a storage service in regions or instances with different endpoint suffixes, such as for Azure China or Azure Government, use the following connection string format.</span></span> <span data-ttu-id="1f8fb-146">Azt jelzi, hogy csatlakozni a tárfiók keresztül (ajánlott) HTTPS vagy HTTP, cserélje le `myAccountName` cserélje le a tárfiók nevével, `myAccountKey` a hívóbetűre, és cserélje ki a `mySuffix` URI utótaggal rendelkező:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-146">Indicate whether you want to connect to the storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with the name of your storage account, replace `myAccountKey` with your account access key, and replace `mySuffix` with the URI suffix:</span></span>

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

<span data-ttu-id="1f8fb-147">Íme egy példa kapcsolati karakterláncot az Azure Kínában tárolási szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="1f8fb-147">Here's an example connection string for storage services in Azure China:</span></span>

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a><span data-ttu-id="1f8fb-148">A kapcsolódási karakterlánc elemzésekor</span><span class="sxs-lookup"><span data-stu-id="1f8fb-148">Parsing a connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a><span data-ttu-id="1f8fb-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f8fb-149">Next steps</span></span>
* [<span data-ttu-id="1f8fb-150">Az Azure storage emulator használata a fejlesztéshez és teszteléshez</span><span class="sxs-lookup"><span data-stu-id="1f8fb-150">Use the Azure storage emulator for development and testing</span></span>](storage-use-emulator.md)
* [<span data-ttu-id="1f8fb-151">Az Azure Storage-kezelők</span><span class="sxs-lookup"><span data-stu-id="1f8fb-151">Azure Storage explorers</span></span>](storage-explorers.md)
* [<span data-ttu-id="1f8fb-152">Közös hozzáférésű Jogosultságkód (SAS) használatával</span><span class="sxs-lookup"><span data-stu-id="1f8fb-152">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)

