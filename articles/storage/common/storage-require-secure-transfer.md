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
# <a name="require-secure-transfer"></a><span data-ttu-id="ecac1-103">Biztonságos átvitel megkövetelése</span><span class="sxs-lookup"><span data-stu-id="ecac1-103">Require secure transfer</span></span>

<span data-ttu-id="ecac1-104">A "Biztonságos szükséges átviteli" beállítás növeli a a storage-fiókjának biztonságát azzal, hogy csak kérésének engedélyezése a tárolási fiók biztonságos kapcsolatoktól.</span><span class="sxs-lookup"><span data-stu-id="ecac1-104">The "Secure transfer required" option enhances the security of your storage account by only allowing requests to the storage account from secure connections.</span></span> <span data-ttu-id="ecac1-105">Például ha előhívja a tárfiók eléréséhez a REST API-k, csatlakoznia kell HTTPS-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="ecac1-105">For example, when calling REST APIs to access your storage account, you must connect using HTTPS.</span></span> <span data-ttu-id="ecac1-106">A HTTP-n keresztül kérelmek azért lettek elutasítva, ha engedélyezve van a "Biztonságos szükséges átviteli".</span><span class="sxs-lookup"><span data-stu-id="ecac1-106">Any requests using HTTP are rejected when "Secure transfer required" is enabled.</span></span>

<span data-ttu-id="ecac1-107">Az Azure Fileshoz szolgáltatást használja, bármilyen titkosítás nélküli kapcsolatot sikertelen lesz, ha engedélyezve van a "Biztonságos szükséges átviteli".</span><span class="sxs-lookup"><span data-stu-id="ecac1-107">When you are using the Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="ecac1-108">Ez magában foglalja az SMB 2.1, az SMB 3.0-titkosítás nélkül és az egyes változatban is elkészíti a Linux SMB-ügyfél a forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="ecac1-108">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of the Linux SMB client.</span></span> 

<span data-ttu-id="ecac1-109">Alapértelmezés szerint a "Biztonságos szükséges átviteli" beállítás le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="ecac1-109">By default, the "Secure transfer required" option is disabled.</span></span>

> [!NOTE]
> <span data-ttu-id="ecac1-110">Azure Storage az egyéni tartománynevek nem támogatja a HTTPS PROTOKOLLT, mert ez a beállítás nem lesz alkalmazva, egy egyéni tartománynevet használatakor.</span><span class="sxs-lookup"><span data-stu-id="ecac1-110">Because Azure Storage doesn't support HTTPS for custom domain names, this option is not applied when using a custom domain name.</span></span>

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a><span data-ttu-id="ecac1-111">Engedélyezze a "Biztonságos átviteli szükséges" az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ecac1-111">Enable "Secure transfer required" in the Azure portal</span></span>

<span data-ttu-id="ecac1-112">A "biztonságos továbbítás szükséges" beállítást is, amikor létrehoz egy tárfiókot, a engedélyezheti a [Azure-portálon](https://portal.azure.com), és a meglévő tárfiókok.</span><span class="sxs-lookup"><span data-stu-id="ecac1-112">You can enable the "Secure transfer required" setting both when you create a storage account in the [Azure portal](https://portal.azure.com), and for existing storage accounts.</span></span>

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a><span data-ttu-id="ecac1-113">Biztonságos átvitele kérése, amikor a storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecac1-113">Require secure transfer when you create a storage account</span></span>

1. <span data-ttu-id="ecac1-114">Nyissa meg a **storage-fiók létrehozása** panel az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="ecac1-114">Open the **Create storage account** blade in the Azure portal.</span></span>
1. <span data-ttu-id="ecac1-115">A **szükséges átviteli biztonságos**, jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="ecac1-115">Under **Secure transfer required**, select **Enabled**.</span></span>

  ![Képernyőfelvétel](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a><span data-ttu-id="ecac1-117">Meglévő tárfiók biztonságos átvitele kérése</span><span class="sxs-lookup"><span data-stu-id="ecac1-117">Require secure transfer for an existing storage account</span></span>

1. <span data-ttu-id="ecac1-118">Válassza ki a meglévő tárfiókot az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="ecac1-118">Select an existing storage account in the Azure portal.</span></span>
1. <span data-ttu-id="ecac1-119">Válassza ki **konfigurációs** alatt **beállítások** a storage-fiók menü panelen.</span><span class="sxs-lookup"><span data-stu-id="ecac1-119">Select **Configuration** under **SETTINGS** in the storage account menu blade.</span></span>
1. <span data-ttu-id="ecac1-120">A **szükséges átviteli biztonságos**, jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="ecac1-120">Under **Secure transfer required**, select **Enabled**.</span></span>

  ![Képernyőfelvétel](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a><span data-ttu-id="ecac1-122">Engedélyezze a "Biztonságos szükséges átviteli" programozott módon</span><span class="sxs-lookup"><span data-stu-id="ecac1-122">Enable "Secure transfer required" programmatically</span></span>

<span data-ttu-id="ecac1-123">A beállítás neve _supportsHttpsTrafficOnly_ a tárfiók tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="ecac1-123">The setting name is _supportsHttpsTrafficOnly_ in storage account properties.</span></span> <span data-ttu-id="ecac1-124">Engedélyezheti a "Biztonságos továbbítás szükséges" beállítás a REST API-t, az eszközök vagy a szalagtárak:</span><span class="sxs-lookup"><span data-stu-id="ecac1-124">You can enable "Secure transfer required" setting with REST API, tools or libraries:</span></span>

* <span data-ttu-id="ecac1-125">**REST API** (verzió: 2016-12-01): [csomag kiadása](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)</span><span class="sxs-lookup"><span data-stu-id="ecac1-125">**REST API** (Version: 2016-12-01): [release package](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)</span></span>
* <span data-ttu-id="ecac1-126">**PowerShell** (verzió: 4.1.0): [csomag kiadása](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)</span><span class="sxs-lookup"><span data-stu-id="ecac1-126">**PowerShell** (Version: 4.1.0): [release package](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)</span></span>
* <span data-ttu-id="ecac1-127">**Parancssori felület** (verzió: 2.0.11): [csomag kiadása](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)</span><span class="sxs-lookup"><span data-stu-id="ecac1-127">**CLI** (Version: 2.0.11): [release package](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)</span></span>
* <span data-ttu-id="ecac1-128">**NodeJS** (verzió: 1.1.0-ás): [csomag kiadása](https://www.npmjs.com/package/azure-arm-storage/)</span><span class="sxs-lookup"><span data-stu-id="ecac1-128">**NodeJS** (Version: 1.1.0): [release package](https://www.npmjs.com/package/azure-arm-storage/)</span></span>
* <span data-ttu-id="ecac1-129">**.NET SDK** (verzió: 6.3.0): [csomag kiadása](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)</span><span class="sxs-lookup"><span data-stu-id="ecac1-129">**.NET SDK** (Version: 6.3.0): [release package](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)</span></span>
* <span data-ttu-id="ecac1-130">**Python SDK** (verzió: 1.1.0-ás): [csomag kiadása](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)</span><span class="sxs-lookup"><span data-stu-id="ecac1-130">**Python SDK** (Version: 1.1.0): [release package](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)</span></span>
* <span data-ttu-id="ecac1-131">**Ruby SDK** (verzió: 0.11.0): [csomag kiadása](https://rubygems.org/gems/azure_mgmt_storage)</span><span class="sxs-lookup"><span data-stu-id="ecac1-131">**Ruby SDK** (Version: 0.11.0): [release package](https://rubygems.org/gems/azure_mgmt_storage)</span></span>

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a><span data-ttu-id="ecac1-132">Engedélyezze a "Biztonságos továbbítás szükséges" beállítás a REST API-n</span><span class="sxs-lookup"><span data-stu-id="ecac1-132">Enable "Secure transfer required" setting with REST API</span></span>

<span data-ttu-id="ecac1-133">Egyszerűbbé teheti a tesztelést ezzel a REST API-t, használhatja a [ArmClient](https://github.com/projectkudu/ARMClient) a parancssorból hívható.</span><span class="sxs-lookup"><span data-stu-id="ecac1-133">To simplify testing with REST API, you can use [ArmClient](https://github.com/projectkudu/ARMClient) to call from command line.</span></span>

 <span data-ttu-id="ecac1-134">Alább parancssor segítségével jelölje be a beállítást, a REST API-hoz:</span><span class="sxs-lookup"><span data-stu-id="ecac1-134">You can use below command line to check the setting with the REST API:</span></span>

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

<span data-ttu-id="ecac1-135">A válaszban található _supportsHttpsTrafficOnly_ beállítást.</span><span class="sxs-lookup"><span data-stu-id="ecac1-135">In the response, you can find _supportsHttpsTrafficOnly_ setting.</span></span> <span data-ttu-id="ecac1-136">Minta:</span><span class="sxs-lookup"><span data-stu-id="ecac1-136">Sample:</span></span>

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

<span data-ttu-id="ecac1-137">Alább parancssor segítségével engedélyezi a beállítást, a REST API-hoz:</span><span class="sxs-lookup"><span data-stu-id="ecac1-137">You can use below command line to enable the setting with the REST API:</span></span>

```
# Login Azure and proceed with your credentials
> armclient login

> armclient PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01 < Input.json
```
<span data-ttu-id="ecac1-138">A minta Input.json:</span><span class="sxs-lookup"><span data-stu-id="ecac1-138">Sample of Input.json:</span></span>
```Json
{
  "location": "westus",
  "properties": {
    "supportsHttpsTrafficOnly": true
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="ecac1-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecac1-139">Next steps</span></span>
<span data-ttu-id="ecac1-140">Az Azure Storage biztonsági képességeket, amelyek együtt lehetővé teszik a fejlesztők számára a biztonságos alkalmazások széles választékát nyújtja.</span><span class="sxs-lookup"><span data-stu-id="ecac1-140">Azure Storage provides a comprehensive set of security capabilities, which together enable developers to build secure applications.</span></span> <span data-ttu-id="ecac1-141">További részletekért látogasson el a [tárolási biztonsági útmutatója](storage-security-guide.md).</span><span class="sxs-lookup"><span data-stu-id="ecac1-141">For more details, visit the [Storage Security Guide](storage-security-guide.md).</span></span>
