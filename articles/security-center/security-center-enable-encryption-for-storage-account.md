---
title: "Engedélyezze a titkosítást a storage-fiókot az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan valósítja meg az Azure Security Center javaslatait ** engedélyezheti a titkosítást az Azure Storage fiók **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: b7b2e8a12cbab68da9c8fcc348e8e3c543607007
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a><span data-ttu-id="2acbf-103">Engedélyezze a titkosítást az Azure storage-fiókot az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="2acbf-103">Enable encryption for Azure storage account in Azure Security Center</span></span>
<span data-ttu-id="2acbf-104">Az Azure Security Center javasolhatja Azure Storage szolgáltatás titkosítási engedélyeznie az inaktív adatok.</span><span class="sxs-lookup"><span data-stu-id="2acbf-104">Azure Security Center may recommend that you enable Azure Storage Service Encryption for data at rest.</span></span>

<span data-ttu-id="2acbf-105">Storage Service Encryption (SSE) működik, az adatok titkosítása az Azure storage írásakor, és az adatok beolvasása előtt visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="2acbf-105">Storage Service Encryption (SSE) works by encrypting the data when it is written to Azure storage and decrypting the data before retrieval.</span></span>  <span data-ttu-id="2acbf-106">SSE jelenleg csak az Azure Blob szolgáltatáshoz érhető el, és nem használható blokkblobokat, lapblobokat, és hozzáfűző blobokat.</span><span class="sxs-lookup"><span data-stu-id="2acbf-106">SSE is currently available only for the Azure Blob service and can be used for block blobs, page blobs, and append blobs.</span></span>  <span data-ttu-id="2acbf-107">További tudnivalókért lásd: [Storage szolgáltatás titkosítási az inaktív adatok](../storage/common/storage-service-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="2acbf-107">To learn more, see [Storage Service Encryption for data at rest](../storage/common/storage-service-encryption.md).</span></span>


> [!Note]
> <span data-ttu-id="2acbf-108">Miután engedélyezte a titkosítás, csak az új adatok titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="2acbf-108">After enabling encryption, only new data is encrypted.</span></span> <span data-ttu-id="2acbf-109">Minden létező blobot, amely a tárfiók nem titkosított marad.</span><span class="sxs-lookup"><span data-stu-id="2acbf-109">Any existing blobs in your storage account remain unencrypted.</span></span> <span data-ttu-id="2acbf-110">Meglévő blobok titkosítása, tekintse meg a [Storage szolgáltatás titkosítási GYIK](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span><span class="sxs-lookup"><span data-stu-id="2acbf-110">To encrypt existing blobs, see the [Storage Service Encryption FAQ](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span></span>
>
>

<span data-ttu-id="2acbf-111">Storage szolgáltatás titkosítási csak erőforrás-kezelő tárfiókok esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="2acbf-111">Storage Service Encryption is only supported on Resource Manager storage accounts.</span></span> <span data-ttu-id="2acbf-112">Klasszikus tárfiókokba jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="2acbf-112">Classic storage accounts are not currently supported.</span></span> <span data-ttu-id="2acbf-113">A klasszikus és Resource Manager üzembe helyezési modell ismertetése: [Azure üzembe helyezési modellel](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="2acbf-113">To understand the classic and Resource Manager deployment models, see [Azure deployment models](../azure-classic-rm.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2acbf-114">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2acbf-114">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="2acbf-115">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="2acbf-115">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="2acbf-116">A javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="2acbf-116">Implement the recommendation</span></span>
1. <span data-ttu-id="2acbf-117">Az a **javaslatok** panelen válassza **engedélyezheti a titkosítást az Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="2acbf-117">In the **Recommendations** blade, select **Enable encryption for Azure Storage Account**.</span></span>
   <span data-ttu-id="2acbf-118">![Titkosítás engedélyezése tárfiókokon][1]</span><span class="sxs-lookup"><span data-stu-id="2acbf-118">![Enable encryption for storage account][1]</span></span>
2. <span data-ttu-id="2acbf-119">A **storage-titkosítás engedélyezéséhez** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2acbf-119">The **Enable storage encryption** blade opens.</span></span> <span data-ttu-id="2acbf-120">Ezen a panelen sorolja fel, ahol a tárolás titkosítása le van tiltva az Azure storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="2acbf-120">This blade lists the Azure storage accounts where storage encryption is disabled.</span></span> <span data-ttu-id="2acbf-121">Ebben a példában most válasszon **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="2acbf-121">In this example, let's select **storageacct1**.</span></span>
   <span data-ttu-id="2acbf-122">![Engedélyezze a tárolás titkosítása][2]</span><span class="sxs-lookup"><span data-stu-id="2acbf-122">![Enable storage encryption][2]</span></span>
3. <span data-ttu-id="2acbf-123">A **titkosítási** paneljén **storageacct1** nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2acbf-123">The **Encryption** blade for **storageacct1** opens.</span></span> <span data-ttu-id="2acbf-124">Válassza ki **engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="2acbf-124">Select **Enabled**.</span></span>
   <span data-ttu-id="2acbf-125">![Titkosítási panel][3]</span><span class="sxs-lookup"><span data-stu-id="2acbf-125">![Encryption blade][3]</span></span>
4. <span data-ttu-id="2acbf-126">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2acbf-126">Select **Save**.</span></span>

<span data-ttu-id="2acbf-127">Most már engedélyezte a tárolás titkosítása **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="2acbf-127">You have now enabled storage encryption for **storageacct1**.</span></span>


## <a name="see-also"></a><span data-ttu-id="2acbf-128">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2acbf-128">See also</span></span>
<span data-ttu-id="2acbf-129">Ez a dokumentum bemutatta megvalósításához a Security Center ajánlás "Titkosítási Azure Storage-fiókhoz tartozó Engedélyezés."</span><span class="sxs-lookup"><span data-stu-id="2acbf-129">This document showed you how to implement the Security Center recommendation "Enable encryption for Azure Storage Account."</span></span> <span data-ttu-id="2acbf-130">Azure Storage szolgáltatás titkosítási kapcsolatos további tudnivalókért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="2acbf-130">To learn more about Azure Storage Service Encryption, see the following:</span></span>

* [<span data-ttu-id="2acbf-131">Az Azure Storage szolgáltatás titkosítási az inaktív adatok</span><span class="sxs-lookup"><span data-stu-id="2acbf-131">Azure Storage Service Encryption for Data at Rest</span></span>](../storage/common/storage-service-encryption.md)

<span data-ttu-id="2acbf-132">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="2acbf-132">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="2acbf-133">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) -Útmutató: az Azure-előfizetések és -erőforráscsoportok biztonsági szabályzatainak konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="2acbf-133">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="2acbf-134">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) -Útmutató: az Azure-erőforrások állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="2acbf-134">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="2acbf-135">[Kezelése és válaszadás a biztonsági riasztásokra az Azure Security Center](security-center-managing-and-responding-alerts.md) -útmutató kezelése és válaszadás a biztonsági riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="2acbf-135">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="2acbf-136">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) -megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="2acbf-136">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="2acbf-137">[Azure Security Center: GYIK](security-center-faq.md) -gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2acbf-137">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="2acbf-138">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="2acbf-138">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
