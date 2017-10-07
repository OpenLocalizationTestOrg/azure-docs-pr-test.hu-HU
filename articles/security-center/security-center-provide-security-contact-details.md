---
title: "aaaProvide biztonsági kapcsolattartási adatait az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan tooprovide biztonsági fel a kapcsolatot az Azure Security Centerben részleteit."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 6c378c9c84c6e4fce70b2541e4cc121018700269
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provide-security-contact-details-in-azure-security-center"></a><span data-ttu-id="f7ae3-103">Adja meg az Azure Security Centerben a biztonsági kapcsolattartási adatait</span><span class="sxs-lookup"><span data-stu-id="f7ae3-103">Provide security contact details in Azure Security Center</span></span>
<span data-ttu-id="f7ae3-104">Azure Security Center javasolni fogja adni biztonsági kapcsolattartási adatait az Azure-előfizetéssel, ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-104">Azure Security Center will recommend that you provide security contact details for your Azure subscription if you haven’t already.</span></span> <span data-ttu-id="f7ae3-105">Ezt az információt a Microsoft toocontact által használható, ha a Microsoft biztonsági válasz Center (MSRC) hello észleli, hogy az az ügyféladatok egy törvénybe ütköző vagy jogosulatlan félnek elérhető-e.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-105">This information will be used by Microsoft toocontact you if hello Microsoft Security Response Center (MSRC) discovers that your customer data has been accessed by an unlawful or unauthorized party.</span></span> <span data-ttu-id="f7ae3-106">MSRC hajt végre, válassza ki a biztonság ellenőrzése hello Azure-hálózatot és az infrastruktúra, és fenyegetést eszközintelligencia és visszaélés panaszokat kapott harmadik felek számára.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-106">MSRC performs select security monitoring of hello Azure network and infrastructure and receives threat intelligence and abuse complaints from third parties.</span></span>

<span data-ttu-id="f7ae3-107">A hello első napi előfordulásakor riasztást, és csak magas súlyosságú riasztások küldött e-mailben értesítést.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-107">An email notification is sent on hello first daily occurrence of an alert and only for high severity alerts.</span></span> <span data-ttu-id="f7ae3-108">Az e-mail-beállítások kizárólag előfizetési szabályok esetében konfigurálhatóak.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-108">Email preferences can only be configured for subscription policies.</span></span> <span data-ttu-id="f7ae3-109">Erőforráscsoportok előfizetésen belül öröklik ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-109">Resource groups within a subscription will inherit these settings.</span></span>

> [!NOTE]
> <span data-ttu-id="f7ae3-110">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="f7ae3-111">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="f7ae3-112">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="f7ae3-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="f7ae3-113">A hello **javaslatok** panelen válassza **adja meg biztonsági kapcsolattartási adatait**.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-113">In hello **Recommendations** blade, select **Provide security contact details**.</span></span>
   <span data-ttu-id="f7ae3-114">![Adja meg a biztonsági ügyfél][1]</span><span class="sxs-lookup"><span data-stu-id="f7ae3-114">![Provide security contact][1]</span></span>
2. <span data-ttu-id="f7ae3-115">Ekkor megnyílik hello panel **adja meg biztonsági kapcsolattartási adatait**.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-115">This opens hello blade **Provide security contact details**.</span></span> <span data-ttu-id="f7ae3-116">Válassza ki a hello Azure-előfizetés tooprovide kapcsolattartási adatokat a.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-116">Select hello Azure subscription tooprovide contact information on.</span></span>
   <span data-ttu-id="f7ae3-117">![Biztonsági kapcsolattartói adatok megadása][2]</span><span class="sxs-lookup"><span data-stu-id="f7ae3-117">![Provide security contact details][2]</span></span>
3. <span data-ttu-id="f7ae3-118">Egy második **adja meg biztonsági kapcsolattartási adatait** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-118">A second **Provide security contact details** blade opens.</span></span>

   * <span data-ttu-id="f7ae3-119">Adja meg a hello biztonsági kapcsolattartási e-mail címet vagy címeket vesszővel válassza el egymástól elválasztva.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-119">Enter hello security contact email address or addresses separated by commas.</span></span> <span data-ttu-id="f7ae3-120">Nincs e-mail címeket, amelyeket megadhat számának korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-120">There is not a limit toohello number of email addresses that you can enter.</span></span>
   * <span data-ttu-id="f7ae3-121">Adjon meg egy biztonsági nemzetközi telefonszámát.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-121">Enter one security contact international phone number.</span></span>
   * <span data-ttu-id="f7ae3-122">magas súlyosságú riasztások kapcsolatos tooreceive e-mailek hello bekapcsolását **küldése e-mailt küld kapcsolatos riasztások**.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-122">tooreceive emails about high severity alerts, turn on hello option **Send me emails about alerts**.</span></span>
   * <span data-ttu-id="f7ae3-123">A jövőbeli hello hello beállítás toosend e-mail értesítések toosubscription tulajdonosok fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-123">In hello future, you will have hello option toosend email notifications toosubscription owners.</span></span> <span data-ttu-id="f7ae3-124">Ez a beállítás jelenleg szürkén jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-124">This option is currently grayed out.</span></span>
   * <span data-ttu-id="f7ae3-125">Válassza ki **OK** tooapply hello biztonsági információk tooyour előfizetés forduljon.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-125">Select **OK** tooapply hello security contact information tooyour subscription.</span></span>

## <a name="see-also"></a><span data-ttu-id="f7ae3-126">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f7ae3-126">See also</span></span>
<span data-ttu-id="f7ae3-127">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="f7ae3-127">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="f7ae3-128">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="f7ae3-129">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="f7ae3-130">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="f7ae3-131">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-131">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="f7ae3-132">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="f7ae3-133">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="f7ae3-134">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="f7ae3-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
