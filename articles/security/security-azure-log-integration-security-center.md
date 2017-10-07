---
title: "a security Center napló integrációs aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooget Azure Security center riasztások napló integrációs használata"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 03/22/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: bcc208d071ec03738215f2aee3b71c7b10927904
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-your-security-center-alerts-in-azure-log-integration"></a><span data-ttu-id="d3ed0-103">Hogyan tooget a Security Center riasztást küld az Azure naplóelemzés integráció</span><span class="sxs-lookup"><span data-stu-id="d3ed0-103">How tooget your Security Center alerts in Azure log integration</span></span>
<span data-ttu-id="d3ed0-104">Ez a cikk hello lépéseket szükséges tooenable hello Azure napló integrációs szolgáltatás toopull az Azure Security Center által generált biztonsági riasztások információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-104">This article provides hello steps required tooenable hello Azure Log Integration service toopull Security Alert information generated by Azure Security Center.</span></span> <span data-ttu-id="d3ed0-105">Sikeresen végrehajtotta a hello hello lépéseket [Ismerkedés](security-azure-log-integration-get-started.md) cikket ebben a cikkben hello lépések végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-105">You must have successfully completed hello steps in hello  [Get started](security-azure-log-integration-get-started.md) article before performing hello steps in this article.</span></span>

## <a name="detailed-steps"></a><span data-ttu-id="d3ed0-106">Részletes lépések</span><span class="sxs-lookup"><span data-stu-id="d3ed0-106">Detailed steps</span></span>
<span data-ttu-id="d3ed0-107">hello lépések hoz létre hello szükség Azure Active Directory szolgáltatás egyszerű és hozzárendelése hello szolgáltatás elv olvasási engedéllyel toohello előfizetés:</span><span class="sxs-lookup"><span data-stu-id="d3ed0-107">hello following steps will create hello required Azure Active Directory Service Principal and assign hello Service Principle read permissions toohello subscription:</span></span>
1. <span data-ttu-id="d3ed0-108">Nyissa meg a hello parancssort, és keresse meg a túl**c:\Program Files\Microsoft Azure napló integráció**</span><span class="sxs-lookup"><span data-stu-id="d3ed0-108">Open hello command prompt and navigate too**c:\Program Files\Microsoft Azure Log Integration**</span></span>
2. <span data-ttu-id="d3ed0-109">Hello parancs futtatása``azlog createazureid``</span><span class="sxs-lookup"><span data-stu-id="d3ed0-109">Run hello command ``azlog createazureid``</span></span>

    <span data-ttu-id="d3ed0-110">Ez a parancs kéri az Azure bejelentkezési.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-110">This command prompts you for your Azure login.</span></span> <span data-ttu-id="d3ed0-111">hello parancs létrehoz egy [Azure Active Directory szolgáltatás egyszerű](../active-directory/develop/active-directory-application-objects.md) hello az Azure AD-bérlő üzemeltető hello Azure-előfizetések a bejelentkezett felhasználó mely hello esetében a rendszergazda, a Társadminisztrátorának vagy a tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-111">hello command then creates an [Azure Active Directory Service Principal](../active-directory/develop/active-directory-application-objects.md) in hello Azure AD Tenants that host hello Azure subscriptions in which hello logged in user is an Administrator, a Co-Administrator, or an Owner.</span></span> <span data-ttu-id="d3ed0-112">hello parancs sikertelen lesz, ha a bejelentkezett felhasználó hello csak a Vendég felhasználó hello Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-112">hello command will fail if hello logged in user is only a Guest user in hello Azure AD Tenant.</span></span> <span data-ttu-id="d3ed0-113">Hitelesítési tooAzure keresztül Azure Active Directory (AD) történik.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-113">Authentication tooAzure is done through Azure Active Directory (AD).</span></span> <span data-ttu-id="d3ed0-114">Hello kapott hozzáférési tooread az Azure-előfizetések az Azure AD identity Azlog integrációs egyszerű szolgáltatás létrehozása hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-114">Creating a service principal for Azlog Integration creates hello Azure AD identity that will be given access tooread from Azure subscriptions.</span></span>

2. <span data-ttu-id="d3ed0-115">Ezután a hello előfizetés toohello egyszerű szolgáltatásnév a 2-olvasó hozzáférési jogosultságot rendel parancsot kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-115">Next you will run a command that assigns reader access on hello subscription toohello service principal created in step 2.</span></span> <span data-ttu-id="d3ed0-116">Ha nem ad meg egy előfizetés-azonosító, hello parancs tooassign hello szolgáltatás egyszerű olvasó szerepkört tooall előfizetések toowhich bármely hozzáférhetnek megpróbál.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-116">If you don’t specify a SubscriptionID, then hello command will attempt tooassign hello service principal reader role tooall subscriptions toowhich you have any access.</span></span> </br></br>
``azlog authorize <SubscriptionID>`` </br> <span data-ttu-id="d3ed0-117">például</span><span class="sxs-lookup"><span data-stu-id="d3ed0-117">for example</span></span> </br>
``azlog authorize 0ee55555-0bc4-4a32-a4e8-c29980000000``

    >[!NOTE]
    <span data-ttu-id="d3ed0-118">Ha futtatja a hello figyelmeztetések jelenhetnek engedélyezik a parancs azonnal hello createazureid parancs után.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-118">You may see warnings if you run hello authorize command immediately after hello createazureid command.</span></span> <span data-ttu-id="d3ed0-119">Néhány hello Azure AD-fiók létrehozásakor, és ha hello fiók használható közötti késés van.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-119">There is some latency between when hello Azure AD account is created and when hello account is available for use.</span></span> <span data-ttu-id="d3ed0-120">Ha vár 60 másodperc hello createazureid parancs toorun hello futtatása után engedélyezze a parancsot, majd figyelmeztetések nem szabadna megjelennie.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-120">If you wait about 60 seconds after running hello createazureid command toorun hello authorize command, then you should not see these warnings.</span></span>

4. <span data-ttu-id="d3ed0-121">Ellenőrizze, hogy a következő mappák tooconfirm, amely a naplófájlok JSON hello hello vannak:</span><span class="sxs-lookup"><span data-stu-id="d3ed0-121">Check hello following folders tooconfirm that hello Audit log JSON files are there:</span></span>
 * <span data-ttu-id="d3ed0-122">**c:\Users\azlog\AzureResourceManagerJson**</span><span class="sxs-lookup"><span data-stu-id="d3ed0-122">**c:\Users\azlog\AzureResourceManagerJson**</span></span>
 * <span data-ttu-id="d3ed0-123">**c:\Users\azlog\AzureResourceManagerJsonLD**</span><span class="sxs-lookup"><span data-stu-id="d3ed0-123">**c:\Users\azlog\AzureResourceManagerJsonLD**</span></span> </br></br>
5. <span data-ttu-id="d3ed0-124">Ellenőrizze a következő mappák tooconfirm, amely a Security Center riasztásait szerepel azokat hello:</span><span class="sxs-lookup"><span data-stu-id="d3ed0-124">Check hello following folders tooconfirm that Security Center alerts exist in them:</span></span></br></br>
 * <span data-ttu-id="d3ed0-125">**c:\Users\azlog\AzureSecurityCenterJson**</span><span class="sxs-lookup"><span data-stu-id="d3ed0-125">**c:\Users\azlog\AzureSecurityCenterJson**</span></span>
 * <span data-ttu-id="d3ed0-126">**c:\Users\azlog\AzureSecurityCenterJsonLD**</span><span class="sxs-lookup"><span data-stu-id="d3ed0-126">**c:\Users\azlog\AzureSecurityCenterJsonLD**</span></span> </br></br>

<span data-ttu-id="d3ed0-127">Ha probléma merül fel hello telepítés és konfigurálás során, nyisson meg egy [támogatási kérelem](/azure-supportability/how-to-create-azure-support-request.md), jelölje be **napló integrációs** hello szolgáltatást, amelynek támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-127">If you run into any issues during hello installation and configuration, please open a [support request](/azure-supportability/how-to-create-azure-support-request.md), select **Log Integration** as hello service for which you are requesting support.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3ed0-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3ed0-128">Next steps</span></span>
<span data-ttu-id="d3ed0-129">toolearn Azure napló-nal kapcsolatos további információkért tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="d3ed0-129">toolearn more about Azure Log Integration, see hello following documents:</span></span>

* <span data-ttu-id="d3ed0-130">[A Microsoft Azure napló integráció az Azure-naplók](https://www.microsoft.com/download/details.aspx?id=53324) – letöltőközpontból részletes rendszerkövetelményeket, és telepítése Azure naplóelemzés integrációs utasításokat.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-130">[Microsoft Azure Log Integration for Azure logs](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center for details, system requirements, and install instructions on Azure log integration.</span></span>
* <span data-ttu-id="d3ed0-131">[Bevezetés tooAzure napló integrációs](security-azure-log-integration-overview.md) – Ez a dokumentum bemutatja a tooAzure napló integrációs, annak főbb funkcióit és annak működéséről.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-131">[Introduction tooAzure log integration](security-azure-log-integration-overview.md) – This document introduces you tooAzure log integration, its key capabilities, and how it works.</span></span>
* <span data-ttu-id="d3ed0-132">[Partner konfigurációs lépések](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – ebben a blogbejegyzésben bemutatja, hogyan tooconfigure Azure naplófájl-integráció toowork Splunk, HP ArcSight és az IBM QRadar partneri megoldások.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-132">[Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – This blog post shows you how tooconfigure Azure log integration toowork with partner solutions Splunk, HP ArcSight, and IBM QRadar.</span></span>
* <span data-ttu-id="d3ed0-133">[Gyakori kérdések (GYIK) integrációs Azure naplóelemzés](security-azure-log-integration-faq.md) -Ez gyakran ismételt kérdések Azure naplóelemzés integrációs kapcsolatos kérdésekre ad választ.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-133">[Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) - This FAQ answers questions about Azure log integration.</span></span>
* <span data-ttu-id="d3ed0-134">[A Security Center integrálása az Azure-ral riasztások jelentkezzen integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md) – Ez a dokumentum bemutatja, hogyan toosync Security Center riasztást küld, és a virtuális gép biztonsági eseményeket a naplóelemzési az Azure Diagnostics és az Azure-beli Auditnaplók által gyűjtött vagy SIEM-megoldás.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-134">[Integrating Security Center alerts with Azure log Integration](../security-center/security-center-integrating-alerts-with-log-integration.md) – This document shows you how toosync Security Center alerts, along with virtual machine security events collected by Azure Diagnostics and Azure Audit Logs, with your log analytics or SIEM solution.</span></span>
* <span data-ttu-id="d3ed0-135">[Az Azure diagnostics és az Azure-beli Auditnaplók újdonságai](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – ebben a blogbejegyzésben bemutatja tooAzure naplókat, és egyéb szolgáltatásokat, amelyek segítenek betekintést nyerhet az Azure-erőforrások hello működésére.</span><span class="sxs-lookup"><span data-stu-id="d3ed0-135">[New features for Azure diagnostics and Azure Audit Logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – This blog post introduces you tooAzure Audit Logs and other features that help you gain insights into hello operations of your Azure resources.</span></span>