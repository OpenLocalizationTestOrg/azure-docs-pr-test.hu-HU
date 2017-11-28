---
title: "Fájl feltöltése az Azure CLI-vel (az.py) IoT-központ konfigurálása |} Microsoft Docs"
description: "Hogyan fileuploads az Azure IoT hubhoz a platformok közötti Azure CLI 2.0 (az.py) használatával konfigurálható."
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
ms.openlocfilehash: a9af26d7ebacf5513952786621aaa92f64be263b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a><span data-ttu-id="986d6-103">Konfigurálja az IoT-központ fájlfeltöltések Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="986d6-103">Configure IoT Hub file uploads using Azure CLI</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="986d6-104">Használatához a [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure Storage-fiókot az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="986d6-104">To use the [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your IoT hub.</span></span> <span data-ttu-id="986d6-105">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="986d6-105">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="986d6-106">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="986d6-106">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="986d6-107">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="986d6-107">An active Azure account.</span></span> <span data-ttu-id="986d6-108">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="986d6-108">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="986d6-109">[Az Azure CLI 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="986d6-109">[Azure CLI 2.0][lnk-CLI-install].</span></span>
* <span data-ttu-id="986d6-110">Az Azure IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="986d6-110">An Azure IoT hub.</span></span> <span data-ttu-id="986d6-111">Ha még nem rendelkezik az IoT-központ, használhatja a `az iot hub create` [parancs] [ lnk-cli-create-iothub] hozzon létre egyet, vagy a portál használatával [létrehoz egy IoT-központot] [lnk-portal-hub].</span><span class="sxs-lookup"><span data-stu-id="986d6-111">If you don't have an IoT hub, you can use the `az iot hub create` [command][lnk-cli-create-iothub] to create one or use the portal to [Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="986d6-112">Egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="986d6-112">An Azure Storage account.</span></span> <span data-ttu-id="986d6-113">Ha egy Azure Storage-fiók nem rendelkezik, használhatja a [Azure CLI 2.0 - storage-fiókok kezelése] [ lnk-manage-storage] hozzon létre egyet, vagy használja a portál [hozzon létre egy tárfiókot][lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="986d6-113">If you don't have an Azure Storage account, you can use the [Azure CLI 2.0 - Manage storage accounts][lnk-manage-storage] to create one or use the portal to [Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="986d6-114">Jelentkezzen be, és állítsa be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="986d6-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="986d6-115">Jelentkezzen be az Azure-fiókjával, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="986d6-115">Sign in to your Azure account and select your subscription.</span></span>

1. <span data-ttu-id="986d6-116">A parancssorban futtassa a [bejelentkezési parancs][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="986d6-116">At the command prompt, run the [login command][lnk-login-command]:</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="986d6-117">Kövesse az utasításokat a hitelesítést a kódot, és jelentkezzen be az Azure-fiókjával webböngészőn keresztül.</span><span class="sxs-lookup"><span data-stu-id="986d6-117">Follow the instructions to authenticate using the code and sign in to your Azure account through a web browser.</span></span>

1. <span data-ttu-id="986d6-118">Ha több Azure-előfizetéssel rendelkezik, az Azure-bA bejelentkezés engedélyezi a hozzáférést az Azure fiókokhoz tartozó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="986d6-118">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure accounts associated with your credentials.</span></span> <span data-ttu-id="986d6-119">Használja a következő [paranccsal listát készíthet az Azure-fiókra] [ lnk-az-account-command] elérhető lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="986d6-119">Use the following [command to list the Azure accounts][lnk-az-account-command] available for you to use:</span></span>

    ```azurecli
    az account list
    ```

    <span data-ttu-id="986d6-120">Az alábbi parancs segítségével válassza ki, hogy az IoT hub létrehozására szolgáló parancsok futtatásához használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="986d6-120">Use the following command to select subscription that you want to use to run the commands to create your IoT hub.</span></span> <span data-ttu-id="986d6-121">Az előfizetés neve vagy azonosítója is használhatja, ha az előző parancs kimenetében:</span><span class="sxs-lookup"><span data-stu-id="986d6-121">You can use either the subscription name or ID from the output of the previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="986d6-122">A tárfiókadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="986d6-122">Retrieve your storage account details</span></span>

<span data-ttu-id="986d6-123">A következő lépések feltételezik, hogy a tárolási fiók használata a **erőforrás-kezelő** telepítési modell, és nem a **klasszikus** üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="986d6-123">The following steps assume that you created your storage account using the **Resource Manager** deployment model, and not the **Classic** deployment model.</span></span>

<span data-ttu-id="986d6-124">Fájlfeltöltéseket adott területen működő eszközök konfigurálásához szükséges a kapcsolati karakterlánc egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="986d6-124">To configure file uploads from your devices, you need the connection string for an Azure storage account.</span></span> <span data-ttu-id="986d6-125">A tárfiók ugyanahhoz az előfizetéshez, mint az IoT hub kell lennie.</span><span class="sxs-lookup"><span data-stu-id="986d6-125">The storage account must be in the same subscription as your IoT hub.</span></span> <span data-ttu-id="986d6-126">A tárfiók a blob-tároló neve is kell.</span><span class="sxs-lookup"><span data-stu-id="986d6-126">You also need the name of a blob container in the storage account.</span></span> <span data-ttu-id="986d6-127">A következő paranccsal lekérni a tárfiók kulcsait:</span><span class="sxs-lookup"><span data-stu-id="986d6-127">Use the following command to retrieve your storage account keys:</span></span>

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

<span data-ttu-id="986d6-128">Jegyezze fel a **connectionString** érték.</span><span class="sxs-lookup"><span data-stu-id="986d6-128">Make a note of the **connectionString** value.</span></span> <span data-ttu-id="986d6-129">A következő lépésekben van szükség.</span><span class="sxs-lookup"><span data-stu-id="986d6-129">You need it in the following steps.</span></span>

<span data-ttu-id="986d6-130">Meglévő blob tároló használata a fájl feltöltéshez, vagy hozzon létre újat:</span><span class="sxs-lookup"><span data-stu-id="986d6-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="986d6-131">A meglévő blob tárolók a tárfiókban lévő listájában, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="986d6-131">To list the existing blob containers in your storage account, use the following command:</span></span>

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* <span data-ttu-id="986d6-132">A tárfiók a blob-tároló létrehozásához használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="986d6-132">To create a blob container in your storage account, use the following command:</span></span>

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a><span data-ttu-id="986d6-133">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="986d6-133">File upload</span></span>

<span data-ttu-id="986d6-134">Mostantól konfigurálhatja az IoT hub engedélyezése [fájl feltöltése funkció] [ lnk-upload] a tárfiókadatok használatával.</span><span class="sxs-lookup"><span data-stu-id="986d6-134">You can now configure your IoT hub to enable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="986d6-135">A konfiguráció szükséges a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="986d6-135">The configuration requires the following values:</span></span>

<span data-ttu-id="986d6-136">**A tároló**: egy blob tároló, az Azure-tárfiók az IoT hub társítja a jelenlegi Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="986d6-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription to associate with your IoT hub.</span></span> <span data-ttu-id="986d6-137">A szükséges tárfiók adatait az előző szakaszban leírt lekért.</span><span class="sxs-lookup"><span data-stu-id="986d6-137">You retrieved the necessary storage account information in the preceding section.</span></span> <span data-ttu-id="986d6-138">Az IoT-központ automatikusan létrehozza a SAS URI-azonosítók eszközöket használja, ha ezek a fájlok feltöltése a blob tároló írási engedéllyel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="986d6-138">IoT Hub automatically generates SAS URIs with write permissions to this blob container for devices to use when they upload files.</span></span>

<span data-ttu-id="986d6-139">**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések.</span><span class="sxs-lookup"><span data-stu-id="986d6-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="986d6-140">**SAS-élettartam**: Ez a beállítás akkor a idő élettartamát az az eszközt az IoT-központ által visszaadott SAS URI-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="986d6-140">**SAS TTL**: This setting is the time-to-live of the SAS URIs returned to the device by IoT Hub.</span></span> <span data-ttu-id="986d6-141">Alapértelmezés szerint egyórás beállítva.</span><span class="sxs-lookup"><span data-stu-id="986d6-141">Set to one hour by default.</span></span>

<span data-ttu-id="986d6-142">**Az értesítési beállítások alapértelmezett élettartam**: az idő TTL-fájl feltöltése értesítési, előtt lejárt.</span><span class="sxs-lookup"><span data-stu-id="986d6-142">**File notification settings default TTL**: The time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="986d6-143">Alapértelmezés szerint egy nap beállítva.</span><span class="sxs-lookup"><span data-stu-id="986d6-143">Set to one day by default.</span></span>

<span data-ttu-id="986d6-144">**Értesítési maximális száma fájl**: A szám, ahányszor az IoT Hub megpróbál egy fájl feltöltése értesítést.</span><span class="sxs-lookup"><span data-stu-id="986d6-144">**File notification maximum delivery count**: The number of times the IoT Hub attempts to deliver a file upload notification.</span></span> <span data-ttu-id="986d6-145">Alapértelmezés szerint 10-re állítva.</span><span class="sxs-lookup"><span data-stu-id="986d6-145">Set to 10 by default.</span></span>

<span data-ttu-id="986d6-146">A következő Azure CLI-parancsok segítségével beállításainak megadása a fájl feltöltése az IoT hub:</span><span class="sxs-lookup"><span data-stu-id="986d6-146">Use the following Azure CLI commands to configure the file upload settings on your IoT hub:</span></span>

<span data-ttu-id="986d6-147">A bash rendszerhéjat használja:</span><span class="sxs-lookup"><span data-stu-id="986d6-147">In a bash shell use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="986d6-148">A Windows-parancssort használja:</span><span class="sxs-lookup"><span data-stu-id="986d6-148">At a Windows command prompt use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="986d6-149">A fájl feltöltése konfigurációs az IoT hub, a következő parancsot a tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="986d6-149">You can review the file upload configuration on your IoT hub using the following command:</span></span>

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a><span data-ttu-id="986d6-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="986d6-150">Next steps</span></span>

<span data-ttu-id="986d6-151">Az IoT-központ a fájl feltöltése képességeivel kapcsolatos további információk: [egy eszközről tölt fel][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="986d6-151">For more information about the file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="986d6-152">Az alábbi hivatkozásokból tudhat meg többet az Azure IoT Hub kezelése:</span><span class="sxs-lookup"><span data-stu-id="986d6-152">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="986d6-153">[Tömeges az IoT-eszközök kezelése][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="986d6-153">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="986d6-154">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="986d6-154">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="986d6-155">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="986d6-155">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="986d6-156">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="986d6-156">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="986d6-157">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="986d6-157">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="986d6-158">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="986d6-158">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="986d6-159">[Az IoT-megoldásból az alapoktól biztonságos mentése][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="986d6-159">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

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