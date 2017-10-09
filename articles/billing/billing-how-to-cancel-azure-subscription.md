---
title: "aaaCancel az Azure-előfizetéshez |} Microsoft Docs"
description: "Ismerteti, hogyan toocancel az Azure-előfizetéshez, például a hello ingyenes próba-előfizetést"
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
ms.openlocfilehash: 198d6ab3de143c071a572db899037f8de85a8cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cancel-your-subscription-for-azure"></a>Az Azure-előfizetés

Az Azure-előfizetéshez hello, megszakíthatja [Fiókadminisztrátort](billing-subscription-transfer.md#whoisaa). Hello előfizetés megszüntetése után megszűnik a hozzáférés tooAzure szolgáltatásokat és erőforrásokat.

Mielőtt az előfizetés megszüntetése:

* Készítsen biztonsági másolatot az adatairól. Például ha az Azure storage vagy SQL adatokat tárolja, töltse le. Ha egy virtuális gépet, mentse a lemezképet, helyileg.
* A szolgáltatások leállítása. Toohello lépjen [erőforrások lapon hello kezelési portál](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), és **leállítása** minden futó virtuális gépek, alkalmazások vagy más szolgáltatásokkal.
* Vegye figyelembe az adatok áttelepítését. Lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md).

Ha megszakítja a fizetett [Azure támogatási csomag](https://azure.microsoft.com/support/plans/), amelyek továbbra is a számlázás havonta hello többi hello 6 hónapos időszakra.

## <a name="cancel-subscription-using-hello-azure-portal"></a>Hello Azure-portál használatával előfizetés megszüntetése

1. Jelölje ki az előfizetését a hello [előfizetések oldalán](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Válassza ki, hogy szeretné, hogy toocancel, és kattintson a hello előfizetést **előfizetés megszüntetése**.

    ![Képernyőkép a hello Mégse gomb](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)

1. Kövesse az utasításokat, és fejezze be a megszakítását.

## <a name="cancel-subscription-using-hello-azure-account-center"></a>Hello Azure Account Center használata előfizetés megszüntetése

1. Jelentkezzen be toohello [Azure Account Center](https://account.windowsazure.com/subscriptions) , hello fiók rendszergazdájához.

1. A **kattintson egy előfizetés tooview részletekért és használati**, válassza ki, hogy szeretné-e toocancel hello előfizetést.

    ![Egy példa kijelölt előfizetésben képernyőkép](./media/billing-how-to-cancel-azure-subscription/Selectsub.png)

1. Hello jobb oldalán található hello, válassza ki a **előfizetés törlése**.

    ![Képernyőkép a hello Mégse előfizetés gomb](./media/billing-how-to-cancel-azure-subscription/cancelsub.png)

1. Válassza ki **Igen, az előfizetés megszüntetése**.

    ![Képernyőkép a hello Mégse párbeszédpanel](./media/billing-how-to-cancel-azure-subscription/cancelbox.png)

1. Kattintson a következőre: ![Ellenőrizze a szimbólum gomb](./media/billing-how-to-cancel-azure-subscription/checkbutton.png) tooclose hello párbeszédpanel ablakot, és térjen vissza tooyour előfizetés lapján.

## <a name="what-happens-after-i-cancel-my-subscription"></a>Mi történik az I-előfizetés megszüntetése után?

Ha megszakítja, számlázási azonnal leáll. Azonban is eltarthat, hello cancellation megjelenítése hello portálon too10 percig.

Ezt követően a szolgáltatások le vannak tiltva. Ez azt jelenti, hogy a virtuális gépek fel van szabadítva, ideiglenes IP-címek felszabadítását és tárolási csak olvasható.

Kivéve, ha használ, egy ingyenes próbaverziót, vagy elérhető kreditek rendelkezik, akkor bármely függőben lévő használati díjak, az utolsó számlázási ciklus és hello cancellation dátum közötti díjon számlázzuk. Az utolsó számlázási ciklus számlázási hello hello végén kap.

Az előfizetés megszüntetése után 90 nappal korábbinak véglegesen törli az adatokat, amennyiben szükséges, vagy hogy meggondolja tooaccess várunk. Jelenleg nem díjat számítanak fel a hello adatok megőrzésével. toolearn több, lásd: [Microsoft Trust Center – hogyan azt kezeli az adatokat](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Előfizetés újraaktiválása

Ha véletlenül a használatalapú előfizetés megszüntetése, akkor [aktiválja a hello Accounts Center](billing-subscription-become-disable.md).

Ha az előfizetés nincs használatalapú fizetés, forduljon a támogatási szolgálathoz megszakításának tooreactivate számított 90 napon belül az előfizetéshez.

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.

Ha további kérdései, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.
