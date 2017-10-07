---
title: "az Azure számlázási számla és a napi használati adatok aaaDownload |} Microsoft Docs"
description: "Ismerteti, hogyan toodownload vagy a nézet a Azure számlázás számla és a napi használati adatok."
keywords: "számlázási számla, számla letöltési, azure számla, azure kihasználtsága"
services: 
documentationcenter: 
author: genlin
manager: tonguyen
editor: 
tags: billing
ms.assetid: 6d568d1d-3bd6-4348-97d0-1098b5fe0661
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2826df10f39914fcaeb9985271dadde550c68dfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Az Azure számlázási számla és a napi használati adatok megtekintése és letöltése
A számla letölthető hello [Azure-portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) vagy annak az e-mailben küldött. toodownload a napi használat, nyissa meg toohello [Azure Account Center](https://account.windowsazure.com). Csak egyes szerepkörök a számla és használati adatok, például a fiók rendszergazdájához hello számlázási engedély tooget rendelkezik. További információ az első toobilling információinak toolearn lásd: [Manage access tooAzure számlázási szerepkörök használatával](billing-manage-access.md).

## <a name="get-your-invoice-in-email-pdf"></a>A számla beolvasni e-mailben (.pdf)
Részt, és konfigurálja az Azure számla további címzetteket tooreceive e-mailben. Ez a szolgáltatás nem az egyes előfizetések például támogatási ajánlatokat, kötött nagyvállalati szerződése vagy Azure in Open licencprogram érhetők el.

1. Jelölje ki az előfizetését a hello [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Részvétel az egyes előfizetésekhez Ön a tulajdonosa. Kattintson a **számlák** majd **E-mail-a számla**. 

    ![Képernyőkép a hello részt folyamata](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Kattintson a **részt vevő** és fogadnia hello.

    ![Képernyőkép a hello részt folyamata](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Hello megállapodás elfogadása után további címzetteket is konfigurálhat.

    ![Képernyőkép a hello részt folyamata](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Ha hello lépések végrehajtása után nem kap egy e-mailt, ellenőrizze az e-mail címet a hello [kapcsolattartási beállítások a profil](https://account.windowsazure.com/profile).

## <a name="download-invoice-from-azure-portal-pdf"></a>Töltse le a számla az Azure portál (.pdf)

1. Jelölje ki az előfizetését a hello [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) , Azure-portálon [egy felhasználó hozzáférési tooinvoices](billing-manage-access.md).

2. Válassza ki **számlák**. 

    ![Képernyőkép a hello számlázási és használati beállítás](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Kattintson a **töltse le a számla** tooview PDF számláját másolatát. Ha a felirat látható **nem érhető el**, lásd: [miért nem látom hello utolsó számlázási időszakban a számla?](#noinvoice)

    ![Képernyőkép a számlázási, hello letöltése beállítás, és a teljes költségek az egyes számlázási időszakban](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. A napi használat hello számlázási időszakban kattintva is megtekintheti. 

A számla kapcsolatos további információkért lásd: [a számlázási megérteni a Microsoft Azure](billing-understand-your-bill.md). Költségek kezelésére, lásd: [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](billing-getting-started.md).

## <a name="download-usage-from-hello-account-center-csv"></a>Használati letöltését hello Account Center (.csv)

1. Jelentkezzen be a hello [Azure Account Center](https://account.windowsazure.com/subscriptions) , hello fiók rendszergazdájához.

2. Válassza ki a kívánt hello számla és használati adatok hello előfizetést.

3. Válassza ki **számlázási előzmények**. 

    ![Képernyőkép a Számlázási előzmények beállítás](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. A kimutatások hello utolsó hat elszámolási időszakok és a jelenlegi időszak teljesüléséig hello tekintheti meg. 

    ![Képernyőkép a számlázási időszak, a beállítások toodownload számla és a napi használat és a teljes költségek az egyes számlázási időszakban](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Válassza ki **aktuális nyilatkozat megtekintése** toosee: hello idő hello becsült a díjak becsült jött létre. Ezek az adatok naponta csak frissül, és nem tartalmazhatják a használati. A havi számla Ez a becslés eltérhet.

    ![Képernyőkép a hello aktuális nyilatkozat megtekintése beállítás](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Képernyőkép a jelenlegi díjak hello becslése](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Válassza ki **letöltése használati** toodownload hello napi használati adatok CSV-fájlként. Ha elérhető két verzióját látja, töltse le a 2-es verzióját.

    ![Képernyőkép a hello használati letöltése beállítás](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Csak a Fiókadminisztrátor hello hello Azure Account Center érheti el. Más számlázási adminisztrátorok, például egy olyan tulajdonost, be használati adatokat hello segítségével [számlázási API-k](billing-usage-rate-card-overview.md).

A napi használatával kapcsolatos további információkért lásd: [a számlázási megérteni a Microsoft Azure](billing-understand-your-bill.md). Költségek kezelésére, lásd: [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](billing-getting-started.md).

## <a name="noinvoice"></a>Miért nem látom hello utolsó számlázási időszakban a számla?

Számla nem látható több oka lehet:

- Ingyenes próbaverzió vagy havi összege van, amely nem túllépi az előfizetéshez. Számla csak akkor keletkezik, amikor pénz akikkel.

- Az előfizetett tooAzure hello nap 30 napon belül.

- hello számla még a nem jönnek létre. Várjon, amíg hello hello számlázási időszak vége.

- Ha még nem hello fiók rendszergazdájához, korábbi számlák nem lehet avaialbe tooyou.

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további kérdései további, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.

