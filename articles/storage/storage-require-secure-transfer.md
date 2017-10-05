---
title: "Az Azure Storage biztonságos átvitelét igénylő |} Microsoft Docs"
description: "További tudnivalók az Azure Storage és az engedélyezéshez \"Biztonságos átvitelét igénylő\" szolgáltatás."
services: storage
documentationcenter: na
author: fhryo-msft
manager: Jason.Hogg
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/20/2017
ms.author: fryu
ms.openlocfilehash: bc5b7fc79869c632db96958f17aaf953a5fd3b19
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="require-secure-transfer"></a>Biztonságos átvitel megkövetelése

A "Biztonságos szükséges átviteli" beállítás növeli a a storage-fiókjának biztonságát azzal, hogy csak kérésének engedélyezése a tárolási fiók biztonságos kapcsolatoktól. Például ha előhívja a tárfiók eléréséhez a REST API-k, csatlakoznia kell HTTPS-kapcsolaton keresztül. A HTTP-n keresztül kérelmek azért lettek elutasítva, ha engedélyezve van a "Biztonságos szükséges átviteli".

Az Azure Fileshoz szolgáltatást használja, bármilyen titkosítás nélküli kapcsolatot sikertelen lesz, ha engedélyezve van a "Biztonságos szükséges átviteli". Ez magában foglalja az SMB 2.1, az SMB 3.0-titkosítás nélkül és az egyes változatban is elkészíti a Linux SMB-ügyfél a forgatókönyveket. 

Alapértelmezés szerint a "Biztonságos szükséges átviteli" beállítás le van tiltva.

> [!NOTE]
> Azure Storage az egyéni tartománynevek nem támogatja a HTTPS PROTOKOLLT, mert ez a beállítás nem lesz alkalmazva, egy egyéni tartománynevet használatakor.

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a>Engedélyezze a "Biztonságos átviteli szükséges" az Azure-portálon

A "biztonságos továbbítás szükséges" beállítást is, amikor létrehoz egy tárfiókot, a engedélyezheti a [Azure-portálon](https://portal.azure.com), és a meglévő tárfiókok.

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a>Biztonságos átvitele kérése, amikor a storage-fiók létrehozása

1. Nyissa meg a **storage-fiók létrehozása** panel az Azure portálon.
1. A **szükséges átviteli biztonságos**, jelölje be **engedélyezve**.

  ![Képernyőfelvétel](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Meglévő tárfiók biztonságos átvitele kérése

1. Válassza ki a meglévő tárfiókot az Azure portálon.
1. Válassza ki **konfigurációs** alatt **beállítások** a storage-fiók menü panelen.
1. A **szükséges átviteli biztonságos**, jelölje be **engedélyezve**.

  ![Képernyőfelvétel](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>Engedélyezze a "Biztonságos szükséges átviteli" programozott módon

A beállítás neve _supportsHttpsTrafficOnly_ a tárfiók tulajdonságai. Engedélyezheti a "Biztonságos továbbítás szükséges" beállítás a REST API-t, az eszközök vagy a szalagtárak:

* **REST API** (verzió: 2016-12-01): [csomag kiadása](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)
* **PowerShell** (verzió: 4.1.0): [csomag kiadása](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)
* **Parancssori felület** (verzió: 2.0.11): [csomag kiadása](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)
* **NodeJS** (verzió: 1.1.0-ás): [csomag kiadása](https://www.npmjs.com/package/azure-arm-storage/)
* **.NET SDK** (verzió: 6.3.0): [csomag kiadása](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)
* **Python SDK** (verzió: 1.1.0-ás): [csomag kiadása](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)
* **Ruby SDK** (verzió: 0.11.0): [csomag kiadása](https://rubygems.org/gems/azure_mgmt_storage)

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a>Engedélyezze a "Biztonságos továbbítás szükséges" beállítás a REST API-n

Egyszerűbbé teheti a tesztelést ezzel a REST API-t, használhatja a [ArmClient](https://github.com/projectkudu/ARMClient) a parancssorból hívható.

 Alább parancssor segítségével jelölje be a beállítást, a REST API-hoz:

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

A válaszban található _supportsHttpsTrafficOnly_ beállítást. Minta:

```Json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}",
  "kind": "Storage",
  ...
  "properties": {
    ...
    "supportsHttpsTrafficOnly": false
  },
  "type": "Microsoft.Storage/storageAccounts"
}
```

Alább parancssor segítségével engedélyezi a beállítást, a REST API-hoz:

```
# Login Azure and proceed with your credentials
> armclient login

> armclient PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01 < Input.json
```
A minta Input.json:
```Json
{
  "location": "westus",
  "properties": {
    "supportsHttpsTrafficOnly": true
  }
}
```

## <a name="next-steps"></a>Következő lépések
Az Azure Storage biztonsági képességeket, amelyek együtt lehetővé teszik a fejlesztők számára a biztonságos alkalmazások széles választékát nyújtja. További részletekért látogasson el a [tárolási biztonsági útmutatója](storage-security-guide.md).
