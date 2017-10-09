---
title: "aaaEnable titkosítási tárfiók az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslatait ** engedélyezheti a titkosítást az Azure Storage fiók **."
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
ms.openlocfilehash: c5cbafbf3a8be86f213dcf1c0c0ddcc0222b3d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a><span data-ttu-id="36837-103">Engedélyezze a titkosítást az Azure storage-fiókot az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="36837-103">Enable encryption for Azure storage account in Azure Security Center</span></span>
<span data-ttu-id="36837-104">Az Azure Security Center javasolhatja Azure Storage szolgáltatás titkosítási engedélyeznie az inaktív adatok.</span><span class="sxs-lookup"><span data-stu-id="36837-104">Azure Security Center may recommend that you enable Azure Storage Service Encryption for data at rest.</span></span>

<span data-ttu-id="36837-105">Storage Service Encryption (SSE) hello adatok beolvasása előtt titkosítása hello adatokat, amikor tooAzure tárolási írás és visszafejtése során.</span><span class="sxs-lookup"><span data-stu-id="36837-105">Storage Service Encryption (SSE) works by encrypting hello data when it is written tooAzure storage and decrypting hello data before retrieval.</span></span>  <span data-ttu-id="36837-106">SSE jelenleg csak hello Azure Blob szolgáltatáshoz érhető el, és nem használható blokkblobokat, lapblobokat, és hozzáfűző blobokat.</span><span class="sxs-lookup"><span data-stu-id="36837-106">SSE is currently available only for hello Azure Blob service and can be used for block blobs, page blobs, and append blobs.</span></span>  <span data-ttu-id="36837-107">több, lásd: toolearn [Storage szolgáltatás titkosítási az inaktív adatok](../storage/common/storage-service-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="36837-107">toolearn more, see [Storage Service Encryption for data at rest](../storage/common/storage-service-encryption.md).</span></span>


> [!Note]
> <span data-ttu-id="36837-108">Miután engedélyezte a titkosítás, csak az új adatok titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="36837-108">After enabling encryption, only new data is encrypted.</span></span> <span data-ttu-id="36837-109">Minden létező blobot, amely a tárfiók nem titkosított marad.</span><span class="sxs-lookup"><span data-stu-id="36837-109">Any existing blobs in your storage account remain unencrypted.</span></span> <span data-ttu-id="36837-110">tooencrypt létező blobot, lásd: hello [Storage szolgáltatás titkosítási GYIK](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span><span class="sxs-lookup"><span data-stu-id="36837-110">tooencrypt existing blobs, see hello [Storage Service Encryption FAQ](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span></span>
>
>

<span data-ttu-id="36837-111">Storage szolgáltatás titkosítási csak erőforrás-kezelő tárfiókok esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="36837-111">Storage Service Encryption is only supported on Resource Manager storage accounts.</span></span> <span data-ttu-id="36837-112">Klasszikus tárfiókokba jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="36837-112">Classic storage accounts are not currently supported.</span></span> <span data-ttu-id="36837-113">toounderstand hello klasszikus és Resource Manager üzembe helyezési modellel, lásd: [Azure üzembe helyezési modellel](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="36837-113">toounderstand hello classic and Resource Manager deployment models, see [Azure deployment models](../azure-classic-rm.md).</span></span>

> [!NOTE]
> <span data-ttu-id="36837-114">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="36837-114">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="36837-115">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="36837-115">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="36837-116">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="36837-116">Implement hello recommendation</span></span>
1. <span data-ttu-id="36837-117">A hello **javaslatok** panelen válassza **engedélyezheti a titkosítást az Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="36837-117">In hello **Recommendations** blade, select **Enable encryption for Azure Storage Account**.</span></span>
   <span data-ttu-id="36837-118">![Titkosítás engedélyezése tárfiókokon][1]</span><span class="sxs-lookup"><span data-stu-id="36837-118">![Enable encryption for storage account][1]</span></span>
2. <span data-ttu-id="36837-119">Hello **storage-titkosítás engedélyezéséhez** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="36837-119">hello **Enable storage encryption** blade opens.</span></span> <span data-ttu-id="36837-120">Ezen a panelen sorolja fel, ahol le a tárolás titkosítása hello Azure storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="36837-120">This blade lists hello Azure storage accounts where storage encryption is disabled.</span></span> <span data-ttu-id="36837-121">Ebben a példában most válasszon **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="36837-121">In this example, let's select **storageacct1**.</span></span>
   <span data-ttu-id="36837-122">![Engedélyezze a tárolás titkosítása][2]</span><span class="sxs-lookup"><span data-stu-id="36837-122">![Enable storage encryption][2]</span></span>
3. <span data-ttu-id="36837-123">Hello **titkosítási** paneljén **storageacct1** nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="36837-123">hello **Encryption** blade for **storageacct1** opens.</span></span> <span data-ttu-id="36837-124">Válassza ki **engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="36837-124">Select **Enabled**.</span></span>
   <span data-ttu-id="36837-125">![Titkosítási panel][3]</span><span class="sxs-lookup"><span data-stu-id="36837-125">![Encryption blade][3]</span></span>
4. <span data-ttu-id="36837-126">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="36837-126">Select **Save**.</span></span>

<span data-ttu-id="36837-127">Most már engedélyezte a tárolás titkosítása **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="36837-127">You have now enabled storage encryption for **storageacct1**.</span></span>


## <a name="see-also"></a><span data-ttu-id="36837-128">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="36837-128">See also</span></span>
<span data-ttu-id="36837-129">Ez a dokumentum bemutatta, hogyan tooimplement hello Security Center ajánlás "engedélyezése a titkosítását az Azure Storage-fiók."</span><span class="sxs-lookup"><span data-stu-id="36837-129">This document showed you how tooimplement hello Security Center recommendation "Enable encryption for Azure Storage Account."</span></span> <span data-ttu-id="36837-130">További információ az Azure Storage szolgáltatás titkosítási toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="36837-130">toolearn more about Azure Storage Service Encryption, see hello following:</span></span>

* [<span data-ttu-id="36837-131">Az Azure Storage szolgáltatás titkosítási az inaktív adatok</span><span class="sxs-lookup"><span data-stu-id="36837-131">Azure Storage Service Encryption for Data at Rest</span></span>](../storage/common/storage-service-encryption.md)

<span data-ttu-id="36837-132">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="36837-132">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="36837-133">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) -megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="36837-133">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="36837-134">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) -megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="36837-134">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="36837-135">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) -megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="36837-135">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="36837-136">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) -megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="36837-136">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="36837-137">[Azure Security Center: GYIK](security-center-faq.md) -gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="36837-137">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="36837-138">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="36837-138">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
