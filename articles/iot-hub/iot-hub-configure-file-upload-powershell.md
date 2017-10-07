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
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>Konfigurálja az IoT-központ fájlfeltöltések a PowerShell használatával

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

toouse hello [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure storage-fiókot az IoT hub. Meglévő tárfiók használata, vagy hozzon létre egy újat.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* [Az Azure PowerShell-parancsmagok][lnk-powershell-install].
* Az Azure IoT-központ. Ha nem rendelkezik az IoT-központ, használhatja a hello [New-AzureRmIoTHub parancsmag] [ lnk-powershell-iothub] láthatóak, illetve egy toocreate hello portal túl[létrehoz egy IoT-központot] [ lnk-portal-hub].
* Egy Azure-tárfiók. Ha egy Azure storage-fiók nem rendelkezik, használhatja a hello [Azure Storage PowerShell parancsmagjainak] [ lnk-powershell-storage] láthatóak, illetve egy toocreate hello portal túl[hozzon létre egy tárfiókot] [ lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Jelentkezzen be, és állítsa be az Azure-fiókjával

Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését.

1. Hello PowerShell parancssorból futtassa a hello **Login-AzureRmAccount** parancsmagot:

    ```powershell
    Login-AzureRmAccount
    ```

1. Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello Azure-előfizetéssel társított hitelesítő adatait. A következő parancs toolist hello Azure-előfizetések elérhető az Ön toouse hello használata:

    ```powershell
    Get-AzureRMSubscription
    ```

    A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toomanage az IoT hub hello használata. Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>A tárfiókadatok beolvasása

hello következő lépések feltételezik, hogy a storage-fiókra hello **erőforrás-kezelő** üzembe helyezési modellt, és nem hello **klasszikus** üzembe helyezési modellben.

tooconfigure fájlfeltöltések az eszközökről, hello kapcsolati karakterlánc egy Azure storage-fiók szükséges. hello tárfiók hello kell lennie az IoT hub tárolóként ugyanazt az előfizetést. Meg kell hello tárfiók a blob-tároló hello nevét is. A következő parancs tooretrieve hello használja a tárfiók kulcsait:

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

Jegyezze fel a hello **key1** tárolási fiók kulcs értékét. Az alábbi lépésekkel hello van szükség.

Meglévő blob tároló használata a fájl feltöltéshez, vagy hozzon létre újat:

* toolist hello meglévő blob tárolók tárfiókba, a következő parancsok hello használata:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* a blob-tároló tárfiókba, a következő parancsok használata hello toocreate:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>Az IoT hub konfigurálása

Mostantól konfigurálhatja az IoT hub tooenable [fájl feltöltése funkció] [ lnk-upload] a tárfiókadatok használatával.

hello konfigurációjában szerepelnie kell a következő értékek hello:

**A tároló**: egy blob tároló, az a jelenlegi Azure-előfizetés tooassociate az IoT hubbal egy Azure storage-fiókot. Hello szükséges tárfiók adatait az előző szakaszban hello lekért. Ezek a fájlok feltöltése az IoT-központ automatikusan létrehozza SAS URI-azonosítók az írási engedélyek toothis blob tároló eszközök toouse.

**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések.

**SAS-élettartam**: Ez a beállítás akkor idő élettartamát a SAS URI-azonosítók toohello eszköz vissza az IoT-központ hello hello. Alapértelmezés szerint tooone óra beállítva.

**Az értesítési beállítások alapértelmezett élettartam**: hello idő TTL fájl feltöltése értesítés előtt lejárt. Alapértelmezés szerint tooone nap meg.

**Értesítési maximális száma fájl**: hello számú alkalommal fordult elő az IoT-központ kísérletek toodeliver fájl feltöltése értesítést hello. Alapértelmezés szerint too10 beállítva.

Az IoT hub PowerShell parancsmag tooconfigure hello fájl feltöltési beállításokat követően hello használata:

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