---
title: "aaaUse hello Azure PowerShell tooconfigure fájlfeltöltés |} Microsoft Docs"
description: "Hogyan toouse hello Azure PowerShell parancsmagok tooconfigure a IoT hub tooenable fájlfeltöltések csatlakoztatott eszközökről. Hello cél Azure storage-fiók konfigurálásával kapcsolatos információkat tartalmazza."
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
ms.openlocfilehash: 9dcdc41693c09cece411921b30c91d7b3db47395
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a><span data-ttu-id="61681-104">Konfigurálja az IoT-központ fájlfeltöltések a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="61681-104">Configure IoT Hub file uploads using PowerShell</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="61681-105">toouse hello [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure storage-fiókot az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="61681-105">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure storage account with your IoT hub.</span></span> <span data-ttu-id="61681-106">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="61681-106">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="61681-107">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="61681-107">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="61681-108">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="61681-108">An active Azure account.</span></span> <span data-ttu-id="61681-109">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="61681-109">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="61681-110">[Az Azure PowerShell-parancsmagok][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="61681-110">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>
* <span data-ttu-id="61681-111">Az Azure IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="61681-111">An Azure IoT hub.</span></span> <span data-ttu-id="61681-112">Ha nem rendelkezik az IoT-központ, használhatja a hello [New-AzureRmIoTHub parancsmag] [ lnk-powershell-iothub] láthatóak, illetve egy toocreate hello portal túl[létrehoz egy IoT-központot] [ lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="61681-112">If you don't have an IoT hub, you can use hello [New-AzureRmIoTHub cmdlet][lnk-powershell-iothub] toocreate one or use hello portal too[Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="61681-113">Egy Azure-tárfiók.</span><span class="sxs-lookup"><span data-stu-id="61681-113">An Azure storage account.</span></span> <span data-ttu-id="61681-114">Ha egy Azure storage-fiók nem rendelkezik, használhatja a hello [Azure Storage PowerShell parancsmagjainak] [ lnk-powershell-storage] láthatóak, illetve egy toocreate hello portal túl[hozzon létre egy tárfiókot] [ lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="61681-114">If you don't have an Azure storage account, you can use hello [Azure Storage PowerShell cmdlets][lnk-powershell-storage] toocreate one or use hello portal too[Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="61681-115">Jelentkezzen be, és állítsa be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="61681-115">Sign in and set your Azure account</span></span>

<span data-ttu-id="61681-116">Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="61681-116">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="61681-117">Hello PowerShell parancssorból futtassa a hello **Login-AzureRmAccount** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="61681-117">At hello PowerShell prompt, run hello **Login-AzureRmAccount** cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="61681-118">Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello Azure-előfizetéssel társított hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="61681-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="61681-119">A következő parancs toolist hello Azure-előfizetések elérhető az Ön toouse hello használata:</span><span class="sxs-lookup"><span data-stu-id="61681-119">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="61681-120">A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toomanage az IoT hub hello használata.</span><span class="sxs-lookup"><span data-stu-id="61681-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toomanage your IoT hub.</span></span> <span data-ttu-id="61681-121">Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:</span><span class="sxs-lookup"><span data-stu-id="61681-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="61681-122">A tárfiókadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="61681-122">Retrieve your storage account details</span></span>

<span data-ttu-id="61681-123">hello következő lépések feltételezik, hogy a storage-fiókra hello **erőforrás-kezelő** üzembe helyezési modellt, és nem hello **klasszikus** üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="61681-123">hello following steps assume that you created your storage account using hello **Resource Manager** deployment model, and not hello **Classic** deployment model.</span></span>

<span data-ttu-id="61681-124">tooconfigure fájlfeltöltések az eszközökről, hello kapcsolati karakterlánc egy Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="61681-124">tooconfigure file uploads from your devices, you need hello connection string for an Azure storage account.</span></span> <span data-ttu-id="61681-125">hello tárfiók hello kell lennie az IoT hub tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="61681-125">hello storage account must be in hello same subscription as your IoT hub.</span></span> <span data-ttu-id="61681-126">Meg kell hello tárfiók a blob-tároló hello nevét is.</span><span class="sxs-lookup"><span data-stu-id="61681-126">You also need hello name of a blob container in hello storage account.</span></span> <span data-ttu-id="61681-127">A következő parancs tooretrieve hello használja a tárfiók kulcsait:</span><span class="sxs-lookup"><span data-stu-id="61681-127">Use hello following command tooretrieve your storage account keys:</span></span>

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

<span data-ttu-id="61681-128">Jegyezze fel a hello **key1** tárolási fiók kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="61681-128">Make a note of hello **key1** storage account key value.</span></span> <span data-ttu-id="61681-129">Az alábbi lépésekkel hello van szükség.</span><span class="sxs-lookup"><span data-stu-id="61681-129">You need it in hello following steps.</span></span>

<span data-ttu-id="61681-130">Meglévő blob tároló használata a fájl feltöltéshez, vagy hozzon létre újat:</span><span class="sxs-lookup"><span data-stu-id="61681-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="61681-131">toolist hello meglévő blob tárolók tárfiókba, a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="61681-131">toolist hello existing blob containers in your storage account, use hello following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* <span data-ttu-id="61681-132">a blob-tároló tárfiókba, a következő parancsok használata hello toocreate:</span><span class="sxs-lookup"><span data-stu-id="61681-132">toocreate a blob container in your storage account, use hello following commands:</span></span>

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a><span data-ttu-id="61681-133">Az IoT hub konfigurálása</span><span class="sxs-lookup"><span data-stu-id="61681-133">Configure your IoT hub</span></span>

<span data-ttu-id="61681-134">Mostantól konfigurálhatja az IoT hub tooenable [fájl feltöltése funkció] [ lnk-upload] a tárfiókadatok használatával.</span><span class="sxs-lookup"><span data-stu-id="61681-134">You can now configure your IoT hub tooenable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="61681-135">hello konfigurációjában szerepelnie kell a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="61681-135">hello configuration requires hello following values:</span></span>

<span data-ttu-id="61681-136">**A tároló**: egy blob tároló, az a jelenlegi Azure-előfizetés tooassociate az IoT hubbal egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="61681-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription tooassociate with your IoT hub.</span></span> <span data-ttu-id="61681-137">Hello szükséges tárfiók adatait az előző szakaszban hello lekért.</span><span class="sxs-lookup"><span data-stu-id="61681-137">You retrieved hello necessary storage account information in hello preceding section.</span></span> <span data-ttu-id="61681-138">Ezek a fájlok feltöltése az IoT-központ automatikusan létrehozza SAS URI-azonosítók az írási engedélyek toothis blob tároló eszközök toouse.</span><span class="sxs-lookup"><span data-stu-id="61681-138">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

<span data-ttu-id="61681-139">**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések.</span><span class="sxs-lookup"><span data-stu-id="61681-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="61681-140">**SAS-élettartam**: Ez a beállítás akkor idő élettartamát a SAS URI-azonosítók toohello eszköz vissza az IoT-központ hello hello.</span><span class="sxs-lookup"><span data-stu-id="61681-140">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="61681-141">Alapértelmezés szerint tooone óra beállítva.</span><span class="sxs-lookup"><span data-stu-id="61681-141">Set tooone hour by default.</span></span>

<span data-ttu-id="61681-142">**Az értesítési beállítások alapértelmezett élettartam**: hello idő TTL fájl feltöltése értesítés előtt lejárt.</span><span class="sxs-lookup"><span data-stu-id="61681-142">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="61681-143">Alapértelmezés szerint tooone nap meg.</span><span class="sxs-lookup"><span data-stu-id="61681-143">Set tooone day by default.</span></span>

<span data-ttu-id="61681-144">**Értesítési maximális száma fájl**: hello számú alkalommal fordult elő az IoT-központ kísérletek toodeliver fájl feltöltése értesítést hello.</span><span class="sxs-lookup"><span data-stu-id="61681-144">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="61681-145">Alapértelmezés szerint too10 beállítva.</span><span class="sxs-lookup"><span data-stu-id="61681-145">Set too10 by default.</span></span>

<span data-ttu-id="61681-146">Az IoT hub PowerShell parancsmag tooconfigure hello fájl feltöltési beállításokat követően hello használata:</span><span class="sxs-lookup"><span data-stu-id="61681-146">Use hello following PowerShell cmdlet tooconfigure hello file upload settings on your IoT hub:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="61681-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61681-147">Next steps</span></span>

<span data-ttu-id="61681-148">Az IoT-központ hello fájl feltöltése képességeivel kapcsolatos további információkért lásd: [egy eszközről tölt fel][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="61681-148">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="61681-149">Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:</span><span class="sxs-lookup"><span data-stu-id="61681-149">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="61681-150">[Tömeges az IoT-eszközök kezelése][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="61681-150">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="61681-151">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="61681-151">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="61681-152">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="61681-152">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="61681-153">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="61681-153">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="61681-154">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="61681-154">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="61681-155">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="61681-155">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="61681-156">[Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="61681-156">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

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