---
title: "aaaConfigure fájl feltöltése tooIoT Azure CLI-vel (az.py) Hub |} Microsoft Docs"
description: "Hogyan tooconfigure fileuploads tooAzure IoT-központ használatával hello platformfüggetlen Azure CLI 2.0 (az.py)."
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
ms.openlocfilehash: 390113df2d96df9833b6aa383ed66805528614a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a><span data-ttu-id="75325-103">Konfigurálja az IoT-központ fájlfeltöltések Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="75325-103">Configure IoT Hub file uploads using Azure CLI</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="75325-104">toouse hello [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure Storage-fiókot az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="75325-104">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your IoT hub.</span></span> <span data-ttu-id="75325-105">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="75325-105">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="75325-106">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="75325-106">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="75325-107">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="75325-107">An active Azure account.</span></span> <span data-ttu-id="75325-108">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="75325-108">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="75325-109">[Az Azure CLI 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="75325-109">[Azure CLI 2.0][lnk-CLI-install].</span></span>
* <span data-ttu-id="75325-110">Az Azure IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="75325-110">An Azure IoT hub.</span></span> <span data-ttu-id="75325-111">Ha nem rendelkezik az IoT-központ, használhatja a hello `az iot hub create` [parancs] [ lnk-cli-create-iothub] láthatóak, illetve egy toocreate hello portal túl [létrehoz egy IoT-központot] [lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="75325-111">If you don't have an IoT hub, you can use hello `az iot hub create` [command][lnk-cli-create-iothub] toocreate one or use hello portal too[Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="75325-112">Egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="75325-112">An Azure Storage account.</span></span> <span data-ttu-id="75325-113">Ha egy Azure Storage-fiók nem rendelkezik, használhatja a hello [Azure CLI 2.0 - storage-fiókok kezelése] [ lnk-manage-storage] láthatóak, illetve egy toocreate hello portal túl[hozzon létre egy tárfiókot] [lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="75325-113">If you don't have an Azure Storage account, you can use hello [Azure CLI 2.0 - Manage storage accounts][lnk-manage-storage] toocreate one or use hello portal too[Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="75325-114">Jelentkezzen be, és állítsa be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="75325-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="75325-115">Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="75325-115">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="75325-116">Hello parancssorban futtassa a hello [bejelentkezési parancs][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="75325-116">At hello command prompt, run hello [login command][lnk-login-command]:</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="75325-117">Hajtsa végre a hello utasításokat tooauthenticate hello kód használatával, és jelentkezzen be Azure-fiók egy webböngészőn keresztül tooyour.</span><span class="sxs-lookup"><span data-stu-id="75325-117">Follow hello instructions tooauthenticate using hello code and sign in tooyour Azure account through a web browser.</span></span>

1. <span data-ttu-id="75325-118">Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello a hitelesítő adatok társított Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="75325-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure accounts associated with your credentials.</span></span> <span data-ttu-id="75325-119">Hello következő [parancs toolist hello Azure-fiókra] [ lnk-az-account-command] meg toouse érhető el:</span><span class="sxs-lookup"><span data-stu-id="75325-119">Use hello following [command toolist hello Azure accounts][lnk-az-account-command] available for you toouse:</span></span>

    ```azurecli
    az account list
    ```

    <span data-ttu-id="75325-120">A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toocreate az IoT hub hello használata.</span><span class="sxs-lookup"><span data-stu-id="75325-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="75325-121">Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:</span><span class="sxs-lookup"><span data-stu-id="75325-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="75325-122">A tárfiókadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="75325-122">Retrieve your storage account details</span></span>

<span data-ttu-id="75325-123">hello következő lépések feltételezik, hogy a storage-fiókra hello **erőforrás-kezelő** üzembe helyezési modellt, és nem hello **klasszikus** üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="75325-123">hello following steps assume that you created your storage account using hello **Resource Manager** deployment model, and not hello **Classic** deployment model.</span></span>

<span data-ttu-id="75325-124">tooconfigure fájlfeltöltések az eszközökről, hello kapcsolati karakterlánc egy Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="75325-124">tooconfigure file uploads from your devices, you need hello connection string for an Azure storage account.</span></span> <span data-ttu-id="75325-125">hello tárfiók hello kell lennie az IoT hub tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="75325-125">hello storage account must be in hello same subscription as your IoT hub.</span></span> <span data-ttu-id="75325-126">Meg kell hello tárfiók a blob-tároló hello nevét is.</span><span class="sxs-lookup"><span data-stu-id="75325-126">You also need hello name of a blob container in hello storage account.</span></span> <span data-ttu-id="75325-127">A következő parancs tooretrieve hello használja a tárfiók kulcsait:</span><span class="sxs-lookup"><span data-stu-id="75325-127">Use hello following command tooretrieve your storage account keys:</span></span>

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

<span data-ttu-id="75325-128">Jegyezze fel a hello **connectionString** érték.</span><span class="sxs-lookup"><span data-stu-id="75325-128">Make a note of hello **connectionString** value.</span></span> <span data-ttu-id="75325-129">Az alábbi lépésekkel hello van szükség.</span><span class="sxs-lookup"><span data-stu-id="75325-129">You need it in hello following steps.</span></span>

<span data-ttu-id="75325-130">Meglévő blob tároló használata a fájl feltöltéshez, vagy hozzon létre újat:</span><span class="sxs-lookup"><span data-stu-id="75325-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="75325-131">toolist hello meglévő blob tárolók tárfiókba, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="75325-131">toolist hello existing blob containers in your storage account, use hello following command:</span></span>

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* <span data-ttu-id="75325-132">a blob-tároló tárfiókba, a következő parancs használata hello toocreate:</span><span class="sxs-lookup"><span data-stu-id="75325-132">toocreate a blob container in your storage account, use hello following command:</span></span>

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a><span data-ttu-id="75325-133">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="75325-133">File upload</span></span>

<span data-ttu-id="75325-134">Mostantól konfigurálhatja az IoT hub tooenable [fájl feltöltése funkció] [ lnk-upload] a tárfiókadatok használatával.</span><span class="sxs-lookup"><span data-stu-id="75325-134">You can now configure your IoT hub tooenable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="75325-135">hello konfigurációjában szerepelnie kell a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="75325-135">hello configuration requires hello following values:</span></span>

<span data-ttu-id="75325-136">**A tároló**: egy blob tároló, az a jelenlegi Azure-előfizetés tooassociate az IoT hubbal egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="75325-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription tooassociate with your IoT hub.</span></span> <span data-ttu-id="75325-137">Hello szükséges tárfiók adatait az előző szakaszban hello lekért.</span><span class="sxs-lookup"><span data-stu-id="75325-137">You retrieved hello necessary storage account information in hello preceding section.</span></span> <span data-ttu-id="75325-138">Ezek a fájlok feltöltése az IoT-központ automatikusan létrehozza SAS URI-azonosítók az írási engedélyek toothis blob tároló eszközök toouse.</span><span class="sxs-lookup"><span data-stu-id="75325-138">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

<span data-ttu-id="75325-139">**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések.</span><span class="sxs-lookup"><span data-stu-id="75325-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="75325-140">**SAS-élettartam**: Ez a beállítás akkor idő élettartamát a SAS URI-azonosítók toohello eszköz vissza az IoT-központ hello hello.</span><span class="sxs-lookup"><span data-stu-id="75325-140">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="75325-141">Alapértelmezés szerint tooone óra beállítva.</span><span class="sxs-lookup"><span data-stu-id="75325-141">Set tooone hour by default.</span></span>

<span data-ttu-id="75325-142">**Az értesítési beállítások alapértelmezett élettartam**: hello idő TTL fájl feltöltése értesítés előtt lejárt.</span><span class="sxs-lookup"><span data-stu-id="75325-142">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="75325-143">Alapértelmezés szerint tooone nap meg.</span><span class="sxs-lookup"><span data-stu-id="75325-143">Set tooone day by default.</span></span>

<span data-ttu-id="75325-144">**Értesítési maximális száma fájl**: hello számú alkalommal fordult elő az IoT-központ kísérletek toodeliver fájl feltöltése értesítést hello.</span><span class="sxs-lookup"><span data-stu-id="75325-144">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="75325-145">Alapértelmezés szerint too10 beállítva.</span><span class="sxs-lookup"><span data-stu-id="75325-145">Set too10 by default.</span></span>

<span data-ttu-id="75325-146">Az Azure parancssori felület parancsok tooconfigure hello fájl feltöltési beállításokat követően az IoT hub hello használata:</span><span class="sxs-lookup"><span data-stu-id="75325-146">Use hello following Azure CLI commands tooconfigure hello file upload settings on your IoT hub:</span></span>

<span data-ttu-id="75325-147">A bash rendszerhéjat használja:</span><span class="sxs-lookup"><span data-stu-id="75325-147">In a bash shell use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="75325-148">A Windows-parancssort használja:</span><span class="sxs-lookup"><span data-stu-id="75325-148">At a Windows command prompt use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="75325-149">Hello fájlok feltöltése konfigurálása a az IoT hub hello a következő parancs használatával tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="75325-149">You can review hello file upload configuration on your IoT hub using hello following command:</span></span>

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a><span data-ttu-id="75325-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="75325-150">Next steps</span></span>

<span data-ttu-id="75325-151">Az IoT-központ hello fájl feltöltése képességeivel kapcsolatos további információkért lásd: [egy eszközről tölt fel][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="75325-151">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="75325-152">Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:</span><span class="sxs-lookup"><span data-stu-id="75325-152">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="75325-153">[Tömeges az IoT-eszközök kezelése][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="75325-153">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="75325-154">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="75325-154">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="75325-155">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="75325-155">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="75325-156">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="75325-156">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="75325-157">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="75325-157">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="75325-158">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="75325-158">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="75325-159">[Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="75325-159">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md


[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-manage-storage]:../storage/common/storage-azure-cli.md#manage-storage-accounts
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md
[lnk-cli-create-iothub]: https://docs.microsoft.com/cli/azure/iot/hub#create