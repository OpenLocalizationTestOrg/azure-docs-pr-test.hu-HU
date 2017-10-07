---
title: "aaaManage hozzáférés tooAzure számlázási szerepkörök használatával |} Microsoft Docs"
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: 5937fac5ffa5ca204eb03a1dcbc5e800b3d5eb74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-toobilling-information-for-azure-using-role-based-access-control"></a>Az Azure szerepköralapú hozzáférés-vezérlés használatával hozzáférést toobilling információk kezelése

Az Azure számlázási információkat toomembers a csoport hozzáférést biztosíthat a következő felhasználói szerepkörök tooyour előfizetés hello hozzárendelésével: Fiókadminisztrátor, szolgáltatás-rendszergazdát, társadminisztrátoraként, tulajdonos, közreműködő, olvasó és számlázási olvasó. Akkor toobilling adatok eléréséhez a hello [Azure-portálon](https://portal.azure.com/), és használhatják a hello [számlázási API-k](billing-usage-rate-card-overview.md) tooprogrammatically le a számlák (egyszer engedélyezte, bejövő) és a használat részleteiről. További információ szerepkörök adhatnak ki, és mely szerepköröket is mi, olvassa el [szerepkörök az Azure RBAC](../active-directory/role-based-access-built-in-roles.md).

## <a name="opt-in"></a>További felhasználók tooaccess számlák engedélyezése

hello Fiókadminisztrátort bírálhatja felül a hello használatával [Azure-portálon](https://portal.azure.com/) lehetővé teszik a hozzáférést tooinvoices más felhasználók számára, és API-n keresztül.

1. Hello fiók rendszergazdájához, akkor jelölje ki az előfizetés a hello [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure-portálon.

1. Válassza ki **számlák** , majd **tooinvoices hozzáférési**.

    ![Képernyőfelvételen látható, hogyan férhetnek hozzá a toodelegate tooinvoices](./media/billing-manage-access/AA-optin.png)

1. Kapcsolja be **a** hello hozzáférés követ hello változások, az előfizetés hatókörbe tartozó szerepkörök toodownload számlán tooallow felhasználók mentése.

    ![Képernyőfelvétel a ki-toodelegate hozzáférés tooinvoice](./media/billing-manage-access/AA-optinAllow.png)

Engedélyezés lehetővé teszi az szolgáltatás-rendszergazdát, társadminisztrátoraként, tulajdonos, közreműködő, olvasó és számlázási olvasó hello előfizetés toodownload PDF számlák hello Azure-portálon. Azonban régebbi, mint a 2016. December számlák egyelőre nem érhető el egyetlen toohello fiók rendszergazdájához.

hello Fiókadminisztrátort is konfigurálhatja, e-mailben elküldött toohave számlákat. toolearn több, lásd: [e-mailben a számla beolvasása](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-toohello-billing-reader-role"></a>Felhasználók toohello számlázási olvasó szerepkör hozzáadása

hello számlázási olvasó szerepkört a csak olvasási hozzáféréssel toosubscription számlázási adatokat az Azure portálon, és nincs hozzáférési tooservices, például a virtuális gépek és tárfiókok rendelkezik. Rendelje hozzá a hello számlázási olvasó szerepkört toosomeone kell toohello előfizetés számlázási adatok eléréséhez, de nem hello képességét toomanage Azure szolgáltatások. Ez a szerepkör azon egy szervezet számára, akik csak Azure-előfizetések végrehajtani a pénzügyi és költség-kezelés megfelelő.

1. Jelölje ki az előfizetését a hello [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure-portálon.

1. Válassza ki **hozzáférés-vezérlés (IAM)** majd **Hozzáadás**.

    ![Képernyőfelvétel IAM hello előfizetés panelen](./media/billing-manage-access/select-iam.PNG)

1. Válasszon **számlázási olvasó** a hello **Szerepkörválasztás** lap.

    ![Képernyőfelvétel a hello helyi menü Nézet számlázási olvasó](./media/billing-manage-access/select-roles.PNG)

1. Írja be az üdvözlő e-mail hello felhasználói tooinvite szeretné, majd kattintson a **OK** toosend hello meghívó.

    ![Képernyőkép a tooenter e-mail tooinvite valaki](./media/billing-manage-access/add-user.PNG)

1. Kövesse a témakör utasításait: hello a meghívás e-mail toolog a, a számlázási olvasó.

    ![Képernyőkép a számlázási olvasó hello mit láthatnak a Azure-portálon](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> hello számlázási olvasó a szolgáltatás jelenleg előzetes verzióban érhető és egyelőre nem támogatják a vállalati (EA) előfizetések vagy nem globális felhők.

## <a name="adding-users-tooother-roles"></a>Felhasználók tooother szerepkörök hozzáadása

Más szerepkörök, például a tulajdonos vagy közreműködő szerepkörrel, a felhasználók férhetnek hozzá csak számlázási adatokat, de az Azure-szolgáltatásokat is. toomanage ezeket a szerepköröket, lásd: [kezelése hello előfizetés vagy szolgáltatások hozzáadása vagy módosítása Azure-rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md).

## <a name="who-can-access-hello-account-centerhttpsaccountwindowsazurecom"></a>Hello hozzáférő felhasználók [Account Center](https://account.windowsazure.com)?

Csak a Fiókadminisztrátor hello toohello Account center bejelentkezhet. hello Fiókadminisztrátort hello előfizetés hello jogi tulajdonosa. Alapértelmezés szerint a hello személy, aki regisztrált a vagy vásárolt Azure-előfizetés hello hello fiók rendszergazdájához,, kivéve, ha hello [előfizetés tulajdonjogának áthelyezése történt](billing-subscription-transfer.md) toosomebody más. hello Fiókadminisztrátort előfizetések létrehozása, előfizetések megszakíthatja, módosítsa hello számlázási címét az előfizetéshez, és hello előfizetés hozzáférési házirendek kezelése.

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.

Ha további kérdései további, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.
