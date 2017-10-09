---
title: "az Azure-előfizetések számlázással vagy jóváírás figyelmeztetéseket aaaSet |} Microsoft Docs"
description: "Ismerteti, hogyan állíthat be riasztásokat a az Azure számlázásának számlázási meglepetések számát elkerülése érdekében."
keywords: "jóváírás riasztást, számlázási riasztás"
services: 
documentationcenter: 
author: vikdesai
manager: tonguyen
editor: 
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: vikdesai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 711b9c72c59874792b0e229cdc5ec0fa517c24c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a>A Microsoft Azure-előfizetések a számlázással vagy jóváírás riasztások beállítása
Ha Ön hello Fiókadminisztrátor az Azure-előfizetéssel, használhatja a hello Azure számlázási riasztás szolgáltatás testre szabott toocreate számlázási riasztásokat, amelyek segítik a figyelheti és kezelheti a számlázási tevékenységet az Azure-fiókra.

Ez a szolgáltatás ennyi az egész Preview, ezért meg kell tooenable hello előzetes verziójú funkciók a lapon először.

## <a name="set-hello-alert-threshold-and-email-recipients"></a>Hello riasztási küszöbérték és e-mailek címzettjeinek beállítása
1. Látogasson el [hello előzetes verziójú funkciók lap](https://account.windowsazure.com/PreviewFeatures) , és engedélyezze **értesítési szolgáltatás számlázási**.

1. Miután megkapta hello e-mailes megerősítés hello számlázási szolgáltatás be van kapcsolva, az előfizetés, látogasson el [hello előfizetések oldalán](https://account.windowsazure.com/Subscriptions) hello fiókportálon. Szeretné, hogy toomonitor, és kattintson az előfizetés hello **riasztások**.

    ![Képernyőfelvétel a hello előfizetések nézet az Azure Account center, a riasztások kiemelt][Image1]

2. Ezután kattintson **riasztási hozzáadása** toocreate az elsőt. Is beállítása öt számlázási riasztásokat, amelyek különböző küszöbértéke előfizetésenként összesen és az egyes riasztások tootwo e-mailek címzettjeinek fel.

    ![Képernyőfelvétel a hello riasztások nézet, ahol felveheti a riasztás][Image2]

3. Amikor egy riasztást, akkor adjon neki egy egyedi nevet, válassza ki a költségkeret küszöbérték, és válassza hello e-mail címeket, ha elküldi a riasztásokat. Hello küszöb beállításához, vagy választhat egy **teljes számlázási** vagy egy **dolláros kreditet** a hello **riasztási a** listája. Számlázási összesen riasztást küldi, amikor hello küszöbértéket meghaladó előfizetés kiadásokat. A pénzügyi kreditet riasztást küldi, amikor a pénzügyi kreditek eshessen hello korlátot. Pénzügyi kreditek általában tooFree próbaverzió és a Visual Studio előfizetések alkalmazni.

    ![Képernyőfelvétel a hello riasztási hozzáadása nézet, ahol konfigurálhatja a címzetteknek][Image3]

Azure támogatja akármilyen e-mail címet, de nem győződjön meg arról, hogy működik-e hello e-mail címét, így gondosan ellenőrizze a elírás eredménye.

## <a name="check-on-your-alerts"></a>A riasztások ellenőrzése
Miután beállította a riasztások, hello Account Center sorolja fel azokat, és bemutatja, hogy hány több állíthat be. Az egyes riasztások látható a hello dátum és a küldési időpontja, hogy egy riasztás teljes számlázási vagy pénzügyi kreditet-e, és a hello korlátot állít be. hello dátum és idő formátuma 24 órás világidő koordinálják (UTC), és hello dátum, éééé-hh-nn formátumban. Kattintson a hello és írja alá a hello lista tooedit riasztás vagy hello Kuka toodelete azt.

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a>A nagyvállalati szerződés (EA) ügyfelek számlázási riasztások
Nagyvállalati ügyfelek a beléptetési kvóták költségeik beállítás a minden részleg számára is megkapja az értesítéseket. Lásd: [részleg költségeik kvóták](https://ea.azure.com/helpdocs/departmentSpendingQuotas) a hello EA portál tooget elindult.

## <a name="learn-more-about-azure-cost-management"></a>További információ az Azure költség kezeléséről
- Becsült költségeit hello segítségével [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/), [összköltsége tulajdonjoga Számológép](https://aka.ms/azure-tco-calculator), és egy szolgáltatás hozzáadásakor.
- [Tekintse át a használati és költségek az Azure portálon rendszeresen](billing-getting-started.md#costs).
- Kapcsolja be a [Azure Advisor költség javaslatok](../advisor/advisor-cost-recommendations.md).

több, lásd: toolearn [Azure költség felügyeleti útmutató](billing-getting-started.md).

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
