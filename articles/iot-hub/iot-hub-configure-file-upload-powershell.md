---
title: "Fájl feltöltése konfigurálása az Azure PowerShell használatával |} Microsoft Docs"
description: "Az IoT hub fájl engedélyezéséhez konfigurálja az Azure PowerShell-parancsmagok segítségével feltölti a csatlakoztatott eszközökről. A cél Azure storage-fiók konfigurálásával kapcsolatos információkat tartalmazza."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: a72bda794b2da3e044c46249559610d06b1f1843
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a><span data-ttu-id="586b0-104">Konfigurálja az IoT-központ fájlfeltöltések a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="586b0-104">Configure IoT Hub file uploads using PowerShell</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="586b0-105">Használatához a [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure storage-fiókot az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="586b0-105">To use the [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure storage account with your IoT hub.</span></span> <span data-ttu-id="586b0-106">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="586b0-106">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="586b0-107">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="586b0-107">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="586b0-108">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="586b0-108">An active Azure account.</span></span> <span data-ttu-id="586b0-109">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="586b0-109">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="586b0-110">[Az Azure PowerShell-parancsmagok][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="586b0-110">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>
* <span data-ttu-id="586b0-111">Az Azure IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="586b0-111">An Azure IoT hub.</span></span> <span data-ttu-id="586b0-112">Ha még nem rendelkezik az IoT-központ, használhatja a [New-AzureRmIoTHub parancsmag] [ lnk-powershell-iothub] hozzon létre egyet, vagy használja a portál [létrehoz egy IoT-központot][lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="586b0-112">If you don't have an IoT hub, you can use the [New-AzureRmIoTHub cmdlet][lnk-powershell-iothub] to create one or use the portal to [Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="586b0-113">Egy Azure-tárfiók.</span><span class="sxs-lookup"><span data-stu-id="586b0-113">An Azure storage account.</span></span> <span data-ttu-id="586b0-114">Ha egy Azure storage-fiók nem rendelkezik, használhatja a [Azure Storage PowerShell parancsmagjainak] [ lnk-powershell-storage] hozzon létre egyet, vagy használja a portál [hozzon létre egy tárfiókot][lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="586b0-114">If you don't have an Azure storage account, you can use the [Azure Storage PowerShell cmdlets][lnk-powershell-storage] to create one or use the portal to [Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="586b0-115">Jelentkezzen be, és állítsa be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="586b0-115">Sign in and set your Azure account</span></span>

<span data-ttu-id="586b0-116">Jelentkezzen be az Azure-fiókjával, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="586b0-116">Sign in to your Azure account and select your subscription.</span></span>

1. <span data-ttu-id="586b0-117">A PowerShell parancssorból futtassa a **Login-AzureRmAccount** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="586b0-117">At the PowerShell prompt, run the **Login-AzureRmAccount** cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="586b0-118">Ha több Azure-előfizetéssel rendelkezik, a jelentkezik be az Azure ad hozzáférést az összes Azure-előfizetést a hitelesítő adatok társított.</span><span class="sxs-lookup"><span data-stu-id="586b0-118">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="586b0-119">Használja a következő parancsot a rendelkezésre álló használata Azure-előfizetések listázásához:</span><span class="sxs-lookup"><span data-stu-id="586b0-119">Use the following command to list the Azure subscriptions available for you to use:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="586b0-120">A következő parancs segítségével válassza ki, hogy az IoT hub kezelésére szolgáló parancsok futtatásához használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="586b0-120">Use the following command to select subscription that you want to use to run the commands to manage your IoT hub.</span></span> <span data-ttu-id="586b0-121">Az előfizetés neve vagy azonosítója is használhatja, ha az előző parancs kimenetében:</span><span class="sxs-lookup"><span data-stu-id="586b0-121">You can use either the subscription name or ID from the output of the previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="586b0-122">A tárfiókadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="586b0-122">Retrieve your storage account details</span></span>

<span data-ttu-id="586b0-123">A következő lépések feltételezik, hogy a tárolási fiók használata a **erőforrás-kezelő** telepítési modell, és nem a **klasszikus** üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="586b0-123">The following steps assume that you created your storage account using the **Resource Manager** deployment model, and not the **Classic** deployment model.</span></span>

<span data-ttu-id="586b0-124">Fájlfeltöltéseket adott területen működő eszközök konfigurálásához szükséges a kapcsolati karakterlánc egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="586b0-124">To configure file uploads from your devices, you need the connection string for an Azure storage account.</span></span> <span data-ttu-id="586b0-125">A tárfiók ugyanahhoz az előfizetéshez, mint az IoT hub kell lennie.</span><span class="sxs-lookup"><span data-stu-id="586b0-125">The storage account must be in the same subscription as your IoT hub.</span></span> <span data-ttu-id="586b0-126">A tárfiók a blob-tároló neve is kell.</span><span class="sxs-lookup"><span data-stu-id="586b0-126">You also need the name of a blob container in the storage account.</span></span> <span data-ttu-id="586b0-127">A következő paranccsal lekérni a tárfiók kulcsait:</span><span class="sxs-lookup"><span data-stu-id="586b0-127">Use the following command to retrieve your storage account keys:</span></span>

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

<span data-ttu-id="586b0-128">Jegyezze fel a **key1** tárolási fiók kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="586b0-128">Make a note of the **key1** storage account key value.</span></span> <span data-ttu-id="586b0-129">A következő lépésekben van szükség.</span><span class="sxs-lookup"><span data-stu-id="586b0-129">You need it in the following steps.</span></span>

<span data-ttu-id="586b0-130">Meglévő blob tároló használata a fájl feltöltéshez, vagy hozzon létre újat:</span><span class="sxs-lookup"><span data-stu-id="586b0-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="586b0-131">A meglévő blob tárolók a tárfiókban lévő listájában, használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="586b0-131">To list the existing blob containers in your storage account, use the following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* <span data-ttu-id="586b0-132">A tárfiók a blob-tároló létrehozásához használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="586b0-132">To create a blob container in your storage account, use the following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a><span data-ttu-id="586b0-133">Az IoT hub konfigurálása</span><span class="sxs-lookup"><span data-stu-id="586b0-133">Configure your IoT hub</span></span>

<span data-ttu-id="586b0-134">Mostantól konfigurálhatja az IoT hub engedélyezése [fájl feltöltése funkció] [ lnk-upload] a tárfiókadatok használatával.</span><span class="sxs-lookup"><span data-stu-id="586b0-134">You can now configure your IoT hub to enable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="586b0-135">A konfiguráció szükséges a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="586b0-135">The configuration requires the following values:</span></span>

<span data-ttu-id="586b0-136">**A tároló**: egy blob tároló, az Azure-tárfiók az IoT hub társítja a jelenlegi Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="586b0-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription to associate with your IoT hub.</span></span> <span data-ttu-id="586b0-137">A szükséges tárfiók adatait az előző szakaszban leírt lekért.</span><span class="sxs-lookup"><span data-stu-id="586b0-137">You retrieved the necessary storage account information in the preceding section.</span></span> <span data-ttu-id="586b0-138">Az IoT-központ automatikusan létrehozza a SAS URI-azonosítók eszközöket használja, ha ezek a fájlok feltöltése a blob tároló írási engedéllyel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="586b0-138">IoT Hub automatically generates SAS URIs with write permissions to this blob container for devices to use when they upload files.</span></span>

<span data-ttu-id="586b0-139">**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések.</span><span class="sxs-lookup"><span data-stu-id="586b0-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="586b0-140">**SAS-élettartam**: Ez a beállítás akkor a idő élettartamát az az eszközt az IoT-központ által visszaadott SAS URI-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="586b0-140">**SAS TTL**: This setting is the time-to-live of the SAS URIs returned to the device by IoT Hub.</span></span> <span data-ttu-id="586b0-141">Alapértelmezés szerint egyórás beállítva.</span><span class="sxs-lookup"><span data-stu-id="586b0-141">Set to one hour by default.</span></span>

<span data-ttu-id="586b0-142">**Az értesítési beállítások alapértelmezett élettartam**: az idő TTL-fájl feltöltése értesítési, előtt lejárt.</span><span class="sxs-lookup"><span data-stu-id="586b0-142">**File notification settings default TTL**: The time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="586b0-143">Alapértelmezés szerint egy nap beállítva.</span><span class="sxs-lookup"><span data-stu-id="586b0-143">Set to one day by default.</span></span>

<span data-ttu-id="586b0-144">**Értesítési maximális száma fájl**: A szám, ahányszor az IoT Hub megpróbál egy fájl feltöltése értesítést.</span><span class="sxs-lookup"><span data-stu-id="586b0-144">**File notification maximum delivery count**: The number of times the IoT Hub attempts to deliver a file upload notification.</span></span> <span data-ttu-id="586b0-145">Alapértelmezés szerint 10-re állítva.</span><span class="sxs-lookup"><span data-stu-id="586b0-145">Set to 10 by default.</span></span>

<span data-ttu-id="586b0-146">Használja a következő PowerShell-parancsmag segítségével konfigurálhatja a fájl feltöltése az IoT hub beállításainak:</span><span class="sxs-lookup"><span data-stu-id="586b0-146">Use the following PowerShell cmdlet to configure the file upload settings on your IoT hub:</span></span>

```powershell
Set-AzureRmIotHub `
    -ResourceGroupName "{your iot hub resource group}" `
    -Name "{your iot hub name}" `
    -FileUploadNotificationTtl "01:00:00" `
    -FileUploadSasUriTtl "01:00:00" `
    -EnableFileUploadNotifications $true `
    -FileUploadStorageConnectionString "DefaultEndpointsProtocol=https;AccountName={your storage account name};AccountKey={your storage account key};EndpointSuffix=core.windows.net" `
    -FileUploadContainerName "{your blob container name}" `
    -FileUploadNotificationMaxDeliveryCount 10
```

## <a name="next-steps"></a><span data-ttu-id="586b0-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="586b0-147">Next steps</span></span>

<span data-ttu-id="586b0-148">Az IoT-központ a fájl feltöltése képességeivel kapcsolatos további információk: [egy eszközről tölt fel][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="586b0-148">For more information about the file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="586b0-149">Az alábbi hivatkozásokból tudhat meg többet az Azure IoT Hub kezelése:</span><span class="sxs-lookup"><span data-stu-id="586b0-149">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="586b0-150">[Tömeges az IoT-eszközök kezelése][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="586b0-150">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="586b0-151">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="586b0-151">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="586b0-152">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="586b0-152">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="586b0-153">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="586b0-153">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="586b0-154">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="586b0-154">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="586b0-155">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="586b0-155">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="586b0-156">[Az IoT-megoldásból az alapoktól biztonságos mentése][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="586b0-156">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-powershell-storage]: https://docs.microsoft.com/powershell/module/azurerm.storage/
[lnk-powershell-iothub]: https://docs.microsoft.com/powershell/module/azurerm.iothub/new-azurermiothub
[lnk-portal-hub]: iot-hub-create-through-portal.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md