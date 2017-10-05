---
title: "Az Azure-előfizetést |} Microsoft Docs"
description: "Ismerteti, hogyan lehet Azure előfizetés, például az ingyenes próba-előfizetést"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: genli
ms.openlocfilehash: c415fada30aa0b0bd9b9d1e416bc37ef30653f68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="cancel-your-subscription-for-azure"></a>Az Azure-előfizetés

Az Azure-előfizetéssel, visszavonhatja a [Fiókadminisztrátort](billing-subscription-transfer.md#whoisaa). Az előfizetés megszüntetése után megszűnik az Azure-szolgáltatásokhoz és erőforrásokhoz való hozzáférését.

Mielőtt az előfizetés megszüntetése:

* Készítsen biztonsági másolatot az adatairól. Például ha az Azure storage vagy SQL adatokat tárolja, töltse le. Ha egy virtuális gépet, mentse a lemezképet, helyileg.
* A szolgáltatások leállítása. Lépjen a [erőforrások lapon a kezelési portál](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), és **leállítása** minden futó virtuális gépek, alkalmazások vagy más szolgáltatásokkal.
* Vegye figyelembe az adatok áttelepítését. Lásd: [erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe](../azure-resource-manager/resource-group-move-resources.md).

Ha megszakítja a fizetett [Azure támogatási csomag](https://azure.microsoft.com/support/plans/), akkor rendszer továbbra is a számlázás havonta a többi a 6 hónapos időszakra.

## <a name="cancel-subscription-using-the-azure-portal"></a>Az Azure portál használatával előfizetés megszüntetése

1. Válassza ki az előfizetést a [előfizetések oldalán](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Válassza ki az előfizetést, szakítsa meg, és kattintson a kívánt **előfizetés megszüntetése**.

    ![A Mégse gomb képernyőkép](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)

1. Kövesse az utasításokat, és fejezze be a megszakítását.

## <a name="cancel-subscription-using-the-azure-account-center"></a>Az Azure Account Center használata előfizetés megszüntetése

1. Jelentkezzen be a [Azure Account Center](https://account.windowsazure.com/subscriptions) fiók rendszergazdaként.

1. A **kattintson egy előfizetésre, a részletek és a használati**, válassza ki az előfizetést, amely megszakítja.

    ![Egy példa kijelölt előfizetésben képernyőkép](./media/billing-how-to-cancel-azure-subscription/Selectsub.png)

1. A lap jobb oldalán válassza ki a **előfizetés törlése**.

    ![Az előfizetés elutasítógombja képernyőkép](./media/billing-how-to-cancel-azure-subscription/cancelsub.png)

1. Válassza ki **Igen, az előfizetés megszüntetése**.

    ![A párbeszédpanel bezárása képernyőkép](./media/billing-how-to-cancel-azure-subscription/cancelbox.png)

1. Kattintson a következőre: ![Ellenőrizze a szimbólum gomb](./media/billing-how-to-cancel-azure-subscription/checkbutton.png) a párbeszédpanel bezárásához, és az előfizetési laphoz való visszatéréshez.

## <a name="what-happens-after-i-cancel-my-subscription"></a>Mi történik az I-előfizetés megszüntetése után?

Ha megszakítja, számlázási azonnal leáll. A portál azonban is igénybe vehet a megszakítási műsor akár 10 percet.

Ezt követően a szolgáltatások le vannak tiltva. Ez azt jelenti, hogy a virtuális gépek fel van szabadítva, ideiglenes IP-címek felszabadítását és tárolási csak olvasható.

Kivéve, ha használ, egy ingyenes próbaverziót, vagy elérhető kreditek rendelkezik, akkor bármely függőben lévő használati díjak, az utolsó számlázási ciklus és a megszakítási dátum közötti díjon számlázzuk. Az elszámolási időszak végén az utolsó számlázási kap.

Az előfizetés megszüntetése után 90 nappal korábbinak véglegesen törli az adatokat, abban az esetben kell-e férni vagy megváltoztatja döntését várunk. Jelenleg nem díjat számítanak fel az adatok megőrzésével. További tudnivalókért lásd: [Microsoft Trust Center – hogyan azt kezeli az adatokat](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Előfizetés újraaktiválása

Ha véletlenül a használatalapú előfizetés megszüntetése, akkor [aktiválja a fiókok Center](billing-subscription-become-disable.md).

Ha az előfizetés nincs használatalapú fizetés, forduljon a támogatási szolgálathoz, az előfizetés újraaktiválásához törlését követő 90 napon belül.

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.

Ha további kérdései, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.
