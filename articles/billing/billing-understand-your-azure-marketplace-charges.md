---
title: "aaaUnderstand az Azure külső szolgáltatási díjak |} Microsoft Docs"
description: "További információk a számlázási külső szolgáltatások, korábbi nevén a piactéren, az Azure-ban díjakat."
services: 
documentationcenter: 
author: adpick
manager: tonguyen
editor: 
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: adpick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d2cb28319e2ab4eff753177220993cbf94c96ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a>Az Azure számlázás külső szolgáltatás költségek ismertetése
Külső szolgáltatások használt Azure piactér nevű toobe. Általában által közzétett harmadik felek érhető el az Azure-szolgáltatásokat, de teljesen integrált Azure-ban. Például ClearDB és SendGrid a külső szolgáltatások, Azure-ban vásárolhatnak, de nem a Microsoft által közzétett.

Ha egy új külső szolgáltatás vagy az erőforrás, figyelmeztetés jelenik meg:

![Piactér-beszerzési figyelmeztetés](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> Külső vállalatok, amelyek nem Microsoft által közzétett szolgáltatások, de más Microsoft-termékek is kategóriába sorolni külső szolgáltatások.
> 
> 

## <a name="how-external-services-are-billed"></a>Hogyan külső szolgáltatások számlázása
- Külső szolgáltatások külön vannak számlázva. Az Azure-előfizetéshez belül egyedi rendelések tekintendők. hello számlázási időszakban az egyes szolgáltatások beállítása során vásárol hello szolgáltatást. Nem toobe összetéveszthető hello számlázási időszakban hello előfizetés, amelynek keretében megvásárolta azt. Külön váltók is kap, és a hitelkártya felszámítása külön történik.
- Egyes külső szolgáltatásnak van egy másik számlázási modellt. Egyes szolgáltatások számlázása a használatalapú módon történt mások havi alapján fizetési modell használatára. Hitelkártya Azure külső-szolgáltatások van szüksége, külső szolgáltatások számla fizetéssel nem vásárolhat.
- Ingyenes havi krediteket külső szolgáltatások nem használható. Amely tartalmazza az Azure-előfizetés használata [kreditek szabad](https://azure.microsoft.com/pricing/spending-limits/), akkor nem lehet alkalmazott tooexternal szolgáltatás váltók. A hitelkártya toopurchase külső szolgáltatások használata.


## <a name="view-external-service-spending-and-history-in-hello-azure-portal"></a>Nézet külső szolgáltatás költségeik és előzmények hello Azure-portálon
Megtekintheti az egyes előfizetések belül hello hello külső szolgáltatások [Azure-portálon](https://portal.azure.com/): 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) hello fiókadminisztrátora.
2. Hello menüben válassza ki **előfizetések**.
   
    ![Válassza ki az előfizetések hello központ menü](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. A hello **előfizetések** panelen, jelölje be hello előfizetés tooview szeretne, és válassza **külső szolgáltatások**.
   
    ![Válasszon egy előfizetést hello számlázási panelen](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. A külső szolgáltatás rendeléseket hello közzétevő neve, vásárolt szolgáltatási rétegben, hello erőforrás és hello rendelés állapota megadott névnek mindegyikének meg kell jelennie. toosee korábbi számlák, válasszon ki egy külső szolgáltatást.
   
    ![Válasszon ki egy külső szolgáltatást](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. Itt megtekintheti, hello adó lebontása például számlázási összegek részhez.
   
    ![Külső szolgáltatások számlázási előzmények megtekintése](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a>Nagyvállalati Szerződés (EA) ügyfelek esetén ebből külső tartalom megtekintése
Nagyvállalati ügyfelek Lásd: a külső szolgáltatás kiadásokat, és töltse le a jelentések hello EA portálon. Lásd: [Azure piactér a Nagyvállalati ügyfelek](https://ea.azure.com/helpdocs/azureMarketplace) tooget elindult.

## <a name="manage-payment-methods-for-external-service-orders"></a>Külső szolgáltatás rendelések fizetési módok kezelése
Frissíti a fizetési módok, a külső szolgáltatás rendelést hello [Account Center](https://account.windowsazure.com/).

> [!NOTE]
> Ha a munkahelyi vagy iskolai fiókkal vásárolt [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toomake változik tooyour fizetési módot.
> 
> 

1. Jelentkezzen be toohello [Account Center](https://account.windowsazure.com/) és [keresse meg a toohello **piactér** lap](https://account.windowsazure.com/Store)
   
    ![Válassza ki a piactér hello fiók központban](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Válassza ki a kívánt toomanage hello külső szolgáltatás
   
    ![Válassza ki a kívánt toomanage hello külső szolgáltatás](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. Kattintson a **fizetési mód megváltoztatása** hello jobb oldalán található hello. Ez a hivatkozás számos lehetőséget kínál, tooa különböző portál toomanage a fizetési módot.
   
    ![Rendelés összegzése](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Kattintson a **adatainak szerkesztése** , és kövesse az utasításokat tooupdate a fizetési adatok.
   
    ![Válassza ki a Szerkesztés adatai](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a>Egy külső szolgáltatás rendelés visszavonása
Ha szeretné toocancel külső szolgáltatás rendelés, a hello hello erőforrás törlése [Azure-portálon](https://portal.azure.com).

![Erőforrás törlése](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további kérdései, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.

