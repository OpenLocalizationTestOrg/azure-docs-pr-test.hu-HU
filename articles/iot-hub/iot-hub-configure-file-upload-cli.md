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
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>Konfigurálja az IoT-központ fájlfeltöltések Azure parancssori felület használatával

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

toouse hello [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure Storage-fiókot az IoT hub. Meglévő tárfiók használata, vagy hozzon létre egy újat.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* [Az Azure CLI 2.0][lnk-CLI-install].
* Az Azure IoT-központ. Ha nem rendelkezik az IoT-központ, használhatja a hello `az iot hub create` [parancs] [ lnk-cli-create-iothub] láthatóak, illetve egy toocreate hello portal túl [létrehoz egy IoT-központot] [lnk-portal-hub].
* Egy Azure Storage-fiókot. Ha egy Azure Storage-fiók nem rendelkezik, használhatja a hello [Azure CLI 2.0 - storage-fiókok kezelése] [ lnk-manage-storage] láthatóak, illetve egy toocreate hello portal túl[hozzon létre egy tárfiókot] [lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Jelentkezzen be, és állítsa be az Azure-fiókjával

Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését.

1. Hello parancssorban futtassa a hello [bejelentkezési parancs][lnk-login-command]:

    ```azurecli
    az login
    ```

    Hajtsa végre a hello utasításokat tooauthenticate hello kód használatával, és jelentkezzen be Azure-fiók egy webböngészőn keresztül tooyour.

1. Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello a hitelesítő adatok társított Azure-fiókra. Hello következő [parancs toolist hello Azure-fiókra] [ lnk-az-account-command] meg toouse érhető el:

    ```azurecli
    az account list
    ```

    A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toocreate az IoT hub hello használata. Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>A tárfiókadatok beolvasása

hello következő lépések feltételezik, hogy a storage-fiókra hello **erőforrás-kezelő** üzembe helyezési modellt, és nem hello **klasszikus** üzembe helyezési modellben.

tooconfigure fájlfeltöltések az eszközökről, hello kapcsolati karakterlánc egy Azure storage-fiók szükséges. hello tárfiók hello kell lennie az IoT hub tárolóként ugyanazt az előfizetést. Meg kell hello tárfiók a blob-tároló hello nevét is. A következő parancs tooretrieve hello használja a tárfiók kulcsait:

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

Jegyezze fel a hello **connectionString** érték. Az alábbi lépésekkel hello van szükség.

Meglévő blob tároló használata a fájl feltöltéshez, vagy hozzon létre újat:

* toolist hello meglévő blob tárolók tárfiókba, a következő parancs hello használata:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* a blob-tároló tárfiókba, a következő parancs használata hello toocreate:

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Fájl feltöltése

Mostantól konfigurálhatja az IoT hub tooenable [fájl feltöltése funkció] [ lnk-upload] a tárfiókadatok használatával.

hello konfigurációjában szerepelnie kell a következő értékek hello:

**A tároló**: egy blob tároló, az a jelenlegi Azure-előfizetés tooassociate az IoT hubbal egy Azure storage-fiókot. Hello szükséges tárfiók adatait az előző szakaszban hello lekért. Ezek a fájlok feltöltése az IoT-központ automatikusan létrehozza SAS URI-azonosítók az írási engedélyek toothis blob tároló eszközök toouse.

**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések.

**SAS-élettartam**: Ez a beállítás akkor idő élettartamát a SAS URI-azonosítók toohello eszköz vissza az IoT-központ hello hello. Alapértelmezés szerint tooone óra beállítva.

**Az értesítési beállítások alapértelmezett élettartam**: hello idő TTL fájl feltöltése értesítés előtt lejárt. Alapértelmezés szerint tooone nap meg.

**Értesítési maximális száma fájl**: hello számú alkalommal fordult elő az IoT-központ kísérletek toodeliver fájl feltöltése értesítést hello. Alapértelmezés szerint too10 beállítva.

Az Azure parancssori felület parancsok tooconfigure hello fájl feltöltési beállításokat követően az IoT hub hello használata:

A bash rendszerhéjat használja:

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

A Windows-parancssort használja:

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Hello fájlok feltöltése konfigurálása a az IoT hub hello a következő parancs használatával tekintheti meg:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Következő lépések

Az IoT-központ hello fájl feltöltése képességeivel kapcsolatos további információkért lásd: [egy eszközről tölt fel][lnk-upload].

Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:

* [Tömeges az IoT-eszközök kezelése][lnk-bulk]
* [Az IoT-központ metrikák][lnk-metrics]
* [Figyelési műveletek][lnk-monitor]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva IoT oldala][lnk-iotedge]
* [Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]

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