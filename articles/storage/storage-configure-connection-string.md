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
# <a name="configure-azure-storage-connection-strings"></a>Configure Azure Storage connection strings (Az Azure Storage kapcsolati karakterláncok konfigurálása)

Egy kapcsolati karakterláncot tartalmaz, az alkalmazásadatok tooaccess a futási időben Azure Storage-fiók szükséges hello hitelesítési adatokat. Konfigurálhatja a kapcsolati karakterláncokkal:

* Csatlakozás az Azure storage emulator toohello.
* Az Azure-ban a tárfiók eléréséhez.
* A közös hozzáférésű jogosultságkód (SAS) keresztül az Azure-ban megadott erőforrások eléréséhez.

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>A kapcsolati karakterlánc tárolásának
Az alkalmazás tooaccess hello kapcsolati karakterláncot, futásidejű tooauthenticate kérelmek tooAzure tárolási kell. A kapcsolati karakterlánc tárolására több lehetőség közül választhat:

* Az alkalmazást futtató hello asztalon vagy egy eszközön tárolhatja hello kapcsolati karakterlánc egy **app.config** vagy **web.config** fájlt. Adja hozzá a hello kapcsolati karakterlánc toohello **AppSettings** szakasz ezeket a fájlokat.
* Egy Azure-felhőszolgáltatásban futó alkalmazáshoz hello kapcsolati karakterlánc tárolhat hello [Azure szolgáltatás konfigurációs (.cscfg) fájl](https://msdn.microsoft.com/library/ee758710.aspx). Adja hozzá a hello kapcsolati karakterlánc toohello **ConfigurationSettings** hello szolgáltatás konfigurációs fájl részét.
* A kapcsolati karakterláncot a kódban közvetlenül is használhatja. Azt javasoljuk azonban, hogy a legtöbb esetben egy konfigurációs fájlban tárolhatja a kapcsolati karakterlánc.

A kapcsolati karakterlánc egy konfigurációs fájlban tárolja teszi könnyen tooupdate hello kapcsolati karakterlánc tooswitch hello storage emulator és az Azure-tárfiók közötti hello felhőben. Csak akkor kell tooedit hello kapcsolati karakterlánc toopoint tooyour célkörnyezet.

Használhatja a hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess a kapcsolati karakterlánc, függetlenül attól, ahol az alkalmazás fut-e futásidőben.

## <a name="create-a-connection-string-for-hello-storage-emulator"></a>Hello storage Emulator kapcsolati karakterlánc létrehozása
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Hello storage emulator kapcsolatos további információkért lásd: [hello Azure storage Emulatorral a fejlesztéshez és teszteléshez](storage-use-emulator.md).

## <a name="create-a-connection-string-for-an-azure-storage-account"></a>Hozzon létre egy Azure-tárfiók kapcsolati karakterlánca
toocreate az Azure-tárfiók kapcsolati karakterláncot, használja hello formátuma. Azt jelzi, hogy tooconnect toohello tárfiók keresztül (ajánlott) HTTPS vagy HTTP, cserélje le `myAccountName` hello nevet, a tárfiók, és cserélje ki `myAccountKey` rendelkező a hívóbetűre:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Például a kapcsolati karakterlánc hasonlóan néznének ki:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

Bár az Azure Storage támogatja a HTTP és HTTPS kapcsolat-karakterláncban *HTTPS erősen ajánlott*.

> [!TIP]
> A tárfiók kapcsolati karakterláncok hello található [Azure-portálon](https://portal.azure.com). Keresse meg a túl**beállítások** > **hívóbetűk** a tárfiók menü panel toosee kapcsolati karakterláncaiban mindkét elsődleges és másodlagos kulcsok.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>A kapcsolati karakterlánc használatával a közös hozzáférésű jogosultságkód létrehozása
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Explicit tárolási a végpont-kapcsolati karakterlánc létrehozása
Explicit Szolgáltatásvégpontok adhat meg a kapcsolati karakterlánc alapértelmezett végpontok hello használata helyett. toocreate egy kapcsolati karakterláncot, amely meghatározza egy explicit végpont hello teljes szolgáltatási végpont minden egyes szolgáltatás hello protokoll specifikációja (HTTPS (ajánlott) vagy HTTP), beleértve hello a következő formátumban kell megadni:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Egy olyan forgatókönyv, ahol előfordulhat, hogy kívánja toospecify explicit végpont esetén a Blob storage endpoint tooa átnevezte [egyéni tartomány](storage-custom-domain-name.md). Ebben az esetben a kapcsolódási karakterláncban a Blob Storage-egyéni végpontot is megadhat. Opcionálisan megadhat hello alapértelmezett végpontok a hello más szolgáltatások Ha az alkalmazás azokat.

Íme egy példa egy kapcsolati karakterláncot, amely meghatározza egy explicit végpontra hello Blob szolgáltatás:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

Ebben a példában az összes szolgáltatáshoz, például egy egyéni tartományt a Blob szolgáltatás hello explicit végpontok határozza meg:

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

hello végpont értékek kapcsolat-karakterláncban használt tooconstruct hello kérelem URI-azonosítók toohello tárolási szolgáltatások, és bármely URI-azonosítók tooyour kód visszaadott hello formája előírják.

Ha átnevezte tárolási végpont tooa egyéni tartományt, és hagyja ki ezt a végpont egy kapcsolati karakterláncból, akkor nem fogja tudni toouse az, hogy a kapcsolati karakterlánc tooaccess adatokat, hogy a felhasználói kódból szolgáltatásban.

> [!IMPORTANT]
> Lehet, hogy a szolgáltatási végpont értékeit a kapcsolati karakterláncok megfelelően formázott URI-k, beleértve a `https://` (ajánlott) vagy `http://`. Azure Storage egyelőre nem támogatják a HTTPS az egyéni tartományok, mert Ön *kell* meg `http://` a tetszőleges végpontot URI, amely az egyéni tartomány tooa mutat.
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Hozzon létre egy kapcsolati karakterláncot egy végpont utótag
toocreate egy kapcsolati karakterlánc egy társzolgáltatás régiókban, vagy másik végpont utótagok példányok többek között az Azure China vagy az Azure Governmentnek a következő kapcsolati karakterlánc-formátum használata hello. Azt jelzi, hogy tooconnect toohello tárfiók keresztül (ajánlott) HTTPS vagy HTTP, cserélje le `myAccountName` cserélje a tárfiókja hello nevű `myAccountKey` a hívóbetűre, és cserélje ki a `mySuffix` a hello URI utótag:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Íme egy példa kapcsolati karakterláncot az Azure Kínában tárolási szolgáltatások:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>A kapcsolódási karakterlánc elemzésekor
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Következő lépések
* [Hello Azure storage emulatorral a fejlesztéshez és teszteléshez](storage-use-emulator.md)
* [Az Azure Storage-kezelők](storage-explorers.md)
* [Közös hozzáférésű Jogosultságkód (SAS) használatával](storage-dotnet-shared-access-signature-part-1.md)

