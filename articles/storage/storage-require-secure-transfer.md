---
title: "aaaRequire biztonságos átvitele az Azure Storage |} Microsoft Docs"
description: "További tudnivalók hello \"Biztonságos átvitelét igénylő\" funkció az Azure Storage, és hogyan tooenable azt."
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
ms.openlocfilehash: 27f745c5e771b50213c1dbb39dee081947be1f39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="require-secure-transfer"></a>Biztonságos átvitel megkövetelése

hello "biztonságos továbbítás szükséges" beállítást a tárfiókja hello biztonsági tételével csak kérelmek toohello tárfiók biztonságos kapcsolatoktól javítja. Például ha előhívja a REST API-k tooaccess a tárfiók, csatlakoznia kell HTTPS-kapcsolaton keresztül. A HTTP-n keresztül kérelmek azért lettek elutasítva, ha engedélyezve van a "Biztonságos szükséges átviteli".

Hello Azure fájlok szolgáltatást használja, a titkosítás nélküli kapcsolat sikertelen lesz, ha engedélyezve van a "Biztonságos szükséges átviteli". Ez magában foglalja a forgatókönyveket SMB 2.1, SMB 3.0-titkosítás nélkül, és néhány változatban is elkészíti a hello Linux SMB-ügyfél használatával. 

Alapértelmezés szerint le van tiltva hello "biztonságos továbbítás szükséges" beállítást.

> [!NOTE]
> Azure Storage az egyéni tartománynevek nem támogatja a HTTPS PROTOKOLLT, mert ez a beállítás nem lesz alkalmazva, egy egyéni tartománynevet használatakor.

## <a name="enable-secure-transfer-required-in-hello-azure-portal"></a>Engedélyezi a "Biztonságos továbbítás szükséges" hello Azure-portálon

Engedélyezheti a hello "biztonságos továbbítás szükséges" beállítást is hello a storage-fiók létrehozásakor [Azure-portálon](https://portal.azure.com), és a meglévő tárfiókok.

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a>Biztonságos átvitele kérése, amikor a storage-fiók létrehozása

1. Nyissa meg hello **storage-fiók létrehozása** panel az Azure-portálon hello.
1. A **szükséges átviteli biztonságos**, jelölje be **engedélyezve**.

  ![Képernyőfelvétel](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Meglévő tárfiók biztonságos átvitele kérése

1. Jelölje ki a meglévő tárfiók hello Azure-portálon.
1. Válassza ki **konfigurációs** alatt **beállítások** hello tárolási fiók menü panelen.
1. A **szükséges átviteli biztonságos**, jelölje be **engedélyezve**.

  ![Képernyőfelvétel](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>Engedélyezze a "Biztonságos szükséges átviteli" programozott módon

beállítás neve hello _supportsHttpsTrafficOnly_ a tárfiók tulajdonságai. Engedélyezheti a "Biztonságos továbbítás szükséges" beállítás a REST API-t, az eszközök vagy a szalagtárak:

* **REST API** (verzió: 2016-12-01): [csomag kiadása](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)
* **PowerShell** (verzió: 4.1.0): [csomag kiadása](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)
* **Parancssori felület** (verzió: 2.0.11): [csomag kiadása](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)
* **NodeJS** (verzió: 1.1.0-ás): [csomag kiadása](https://www.npmjs.com/package/azure-arm-storage/)
* **.NET SDK** (verzió: 6.3.0): [csomag kiadása](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)
* **Python SDK** (verzió: 1.1.0-ás): [csomag kiadása](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)
* **Ruby SDK** (verzió: 0.11.0): [csomag kiadása](https://rubygems.org/gems/azure_mgmt_storage)

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a>Engedélyezze a "Biztonságos továbbítás szükséges" beállítás a REST API-n

REST API-val tesztelés toosimplify használható [ArmClient](https://github.com/projectkudu/ARMClient) toocall a parancssorból.

 Alább parancssori toocheck hello setting hello REST API-t használhatja:

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

Hello válaszul található _supportsHttpsTrafficOnly_ beállítást. Minta:

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

Alább parancssori tooenable hello setting hello REST API-t használhatja:

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
Az Azure Storage a biztonságos alkalmazások toobuild biztonsági képességeket, amelyek együtt lehetővé teszik a fejlesztők széles választékát nyújtja. További részletekért látogasson el a hello [tárolási biztonsági útmutatója](storage-security-guide.md).
