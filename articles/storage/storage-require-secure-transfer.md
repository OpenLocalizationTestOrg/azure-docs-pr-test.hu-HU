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
# <a name="require-secure-transfer"></a><span data-ttu-id="b8075-103">Biztonságos átvitel megkövetelése</span><span class="sxs-lookup"><span data-stu-id="b8075-103">Require secure transfer</span></span>

<span data-ttu-id="b8075-104">hello "biztonságos továbbítás szükséges" beállítást a tárfiókja hello biztonsági tételével csak kérelmek toohello tárfiók biztonságos kapcsolatoktól javítja.</span><span class="sxs-lookup"><span data-stu-id="b8075-104">hello "Secure transfer required" option enhances hello security of your storage account by only allowing requests toohello storage account from secure connections.</span></span> <span data-ttu-id="b8075-105">Például ha előhívja a REST API-k tooaccess a tárfiók, csatlakoznia kell HTTPS-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b8075-105">For example, when calling REST APIs tooaccess your storage account, you must connect using HTTPS.</span></span> <span data-ttu-id="b8075-106">A HTTP-n keresztül kérelmek azért lettek elutasítva, ha engedélyezve van a "Biztonságos szükséges átviteli".</span><span class="sxs-lookup"><span data-stu-id="b8075-106">Any requests using HTTP are rejected when "Secure transfer required" is enabled.</span></span>

<span data-ttu-id="b8075-107">Hello Azure fájlok szolgáltatást használja, a titkosítás nélküli kapcsolat sikertelen lesz, ha engedélyezve van a "Biztonságos szükséges átviteli".</span><span class="sxs-lookup"><span data-stu-id="b8075-107">When you are using hello Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="b8075-108">Ez magában foglalja a forgatókönyveket SMB 2.1, SMB 3.0-titkosítás nélkül, és néhány változatban is elkészíti a hello Linux SMB-ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="b8075-108">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of hello Linux SMB client.</span></span> 

<span data-ttu-id="b8075-109">Alapértelmezés szerint le van tiltva hello "biztonságos továbbítás szükséges" beállítást.</span><span class="sxs-lookup"><span data-stu-id="b8075-109">By default, hello "Secure transfer required" option is disabled.</span></span>

> [!NOTE]
> <span data-ttu-id="b8075-110">Azure Storage az egyéni tartománynevek nem támogatja a HTTPS PROTOKOLLT, mert ez a beállítás nem lesz alkalmazva, egy egyéni tartománynevet használatakor.</span><span class="sxs-lookup"><span data-stu-id="b8075-110">Because Azure Storage doesn't support HTTPS for custom domain names, this option is not applied when using a custom domain name.</span></span>

## <a name="enable-secure-transfer-required-in-hello-azure-portal"></a><span data-ttu-id="b8075-111">Engedélyezi a "Biztonságos továbbítás szükséges" hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b8075-111">Enable "Secure transfer required" in hello Azure portal</span></span>

<span data-ttu-id="b8075-112">Engedélyezheti a hello "biztonságos továbbítás szükséges" beállítást is hello a storage-fiók létrehozásakor [Azure-portálon](https://portal.azure.com), és a meglévő tárfiókok.</span><span class="sxs-lookup"><span data-stu-id="b8075-112">You can enable hello "Secure transfer required" setting both when you create a storage account in hello [Azure portal](https://portal.azure.com), and for existing storage accounts.</span></span>

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a><span data-ttu-id="b8075-113">Biztonságos átvitele kérése, amikor a storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b8075-113">Require secure transfer when you create a storage account</span></span>

1. <span data-ttu-id="b8075-114">Nyissa meg hello **storage-fiók létrehozása** panel az Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="b8075-114">Open hello **Create storage account** blade in hello Azure portal.</span></span>
1. <span data-ttu-id="b8075-115">A **szükséges átviteli biztonságos**, jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="b8075-115">Under **Secure transfer required**, select **Enabled**.</span></span>

  ![Képernyőfelvétel](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a><span data-ttu-id="b8075-117">Meglévő tárfiók biztonságos átvitele kérése</span><span class="sxs-lookup"><span data-stu-id="b8075-117">Require secure transfer for an existing storage account</span></span>

1. <span data-ttu-id="b8075-118">Jelölje ki a meglévő tárfiók hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b8075-118">Select an existing storage account in hello Azure portal.</span></span>
1. <span data-ttu-id="b8075-119">Válassza ki **konfigurációs** alatt **beállítások** hello tárolási fiók menü panelen.</span><span class="sxs-lookup"><span data-stu-id="b8075-119">Select **Configuration** under **SETTINGS** in hello storage account menu blade.</span></span>
1. <span data-ttu-id="b8075-120">A **szükséges átviteli biztonságos**, jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="b8075-120">Under **Secure transfer required**, select **Enabled**.</span></span>

  ![Képernyőfelvétel](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a><span data-ttu-id="b8075-122">Engedélyezze a "Biztonságos szükséges átviteli" programozott módon</span><span class="sxs-lookup"><span data-stu-id="b8075-122">Enable "Secure transfer required" programmatically</span></span>

<span data-ttu-id="b8075-123">beállítás neve hello _supportsHttpsTrafficOnly_ a tárfiók tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="b8075-123">hello setting name is _supportsHttpsTrafficOnly_ in storage account properties.</span></span> <span data-ttu-id="b8075-124">Engedélyezheti a "Biztonságos továbbítás szükséges" beállítás a REST API-t, az eszközök vagy a szalagtárak:</span><span class="sxs-lookup"><span data-stu-id="b8075-124">You can enable "Secure transfer required" setting with REST API, tools or libraries:</span></span>

* <span data-ttu-id="b8075-125">**REST API** (verzió: 2016-12-01): [csomag kiadása](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)</span><span class="sxs-lookup"><span data-stu-id="b8075-125">**REST API** (Version: 2016-12-01): [release package](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)</span></span>
* <span data-ttu-id="b8075-126">**PowerShell** (verzió: 4.1.0): [csomag kiadása](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)</span><span class="sxs-lookup"><span data-stu-id="b8075-126">**PowerShell** (Version: 4.1.0): [release package](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)</span></span>
* <span data-ttu-id="b8075-127">**Parancssori felület** (verzió: 2.0.11): [csomag kiadása](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)</span><span class="sxs-lookup"><span data-stu-id="b8075-127">**CLI** (Version: 2.0.11): [release package](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)</span></span>
* <span data-ttu-id="b8075-128">**NodeJS** (verzió: 1.1.0-ás): [csomag kiadása](https://www.npmjs.com/package/azure-arm-storage/)</span><span class="sxs-lookup"><span data-stu-id="b8075-128">**NodeJS** (Version: 1.1.0): [release package](https://www.npmjs.com/package/azure-arm-storage/)</span></span>
* <span data-ttu-id="b8075-129">**.NET SDK** (verzió: 6.3.0): [csomag kiadása](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)</span><span class="sxs-lookup"><span data-stu-id="b8075-129">**.NET SDK** (Version: 6.3.0): [release package](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)</span></span>
* <span data-ttu-id="b8075-130">**Python SDK** (verzió: 1.1.0-ás): [csomag kiadása](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)</span><span class="sxs-lookup"><span data-stu-id="b8075-130">**Python SDK** (Version: 1.1.0): [release package](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)</span></span>
* <span data-ttu-id="b8075-131">**Ruby SDK** (verzió: 0.11.0): [csomag kiadása](https://rubygems.org/gems/azure_mgmt_storage)</span><span class="sxs-lookup"><span data-stu-id="b8075-131">**Ruby SDK** (Version: 0.11.0): [release package](https://rubygems.org/gems/azure_mgmt_storage)</span></span>

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a><span data-ttu-id="b8075-132">Engedélyezze a "Biztonságos továbbítás szükséges" beállítás a REST API-n</span><span class="sxs-lookup"><span data-stu-id="b8075-132">Enable "Secure transfer required" setting with REST API</span></span>

<span data-ttu-id="b8075-133">REST API-val tesztelés toosimplify használható [ArmClient](https://github.com/projectkudu/ARMClient) toocall a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="b8075-133">toosimplify testing with REST API, you can use [ArmClient](https://github.com/projectkudu/ARMClient) toocall from command line.</span></span>

 <span data-ttu-id="b8075-134">Alább parancssori toocheck hello setting hello REST API-t használhatja:</span><span class="sxs-lookup"><span data-stu-id="b8075-134">You can use below command line toocheck hello setting with hello REST API:</span></span>

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

<span data-ttu-id="b8075-135">Hello válaszul található _supportsHttpsTrafficOnly_ beállítást.</span><span class="sxs-lookup"><span data-stu-id="b8075-135">In hello response, you can find _supportsHttpsTrafficOnly_ setting.</span></span> <span data-ttu-id="b8075-136">Minta:</span><span class="sxs-lookup"><span data-stu-id="b8075-136">Sample:</span></span>

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

<span data-ttu-id="b8075-137">Alább parancssori tooenable hello setting hello REST API-t használhatja:</span><span class="sxs-lookup"><span data-stu-id="b8075-137">You can use below command line tooenable hello setting with hello REST API:</span></span>

```
# Login Azure and proceed with your credentials
> armclient login

> armclient PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01 < Input.json
```
<span data-ttu-id="b8075-138">A minta Input.json:</span><span class="sxs-lookup"><span data-stu-id="b8075-138">Sample of Input.json:</span></span>
```Json
{
  "location": "westus",
  "properties": {
    "supportsHttpsTrafficOnly": true
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="b8075-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8075-139">Next steps</span></span>
<span data-ttu-id="b8075-140">Az Azure Storage a biztonságos alkalmazások toobuild biztonsági képességeket, amelyek együtt lehetővé teszik a fejlesztők széles választékát nyújtja.</span><span class="sxs-lookup"><span data-stu-id="b8075-140">Azure Storage provides a comprehensive set of security capabilities, which together enable developers toobuild secure applications.</span></span> <span data-ttu-id="b8075-141">További részletekért látogasson el a hello [tárolási biztonsági útmutatója](storage-security-guide.md).</span><span class="sxs-lookup"><span data-stu-id="b8075-141">For more details, visit hello [Storage Security Guide](storage-security-guide.md).</span></span>
