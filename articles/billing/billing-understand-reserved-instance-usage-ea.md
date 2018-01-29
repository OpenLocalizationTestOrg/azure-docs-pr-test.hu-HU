---
title: "A vállalati Azure fenntartott példány használatának megértéséhez |} Microsoft Docs"
description: "Ismerje meg, hogyan olvashatja az alkalmazás foglalt példány a nagyvállalati beléptetés megértéséhez használatának."
services: billing
documentationcenter: 
author: manish-shukla01
manager: manshuk
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: manshuk
ms.openlocfilehash: 7ef601033b36ee968cb766d40a0a6b05afa9a1a4
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/16/2017
---
# <a name="understand--reserved-instance-usage-for-your-enterprise-enrollment"></a>A nagyvállalati beléptetés használata fenntartott példány ismertetése
Fenntartott példány kihasználtsági értelmezése a ReservationId segítségével [foglalási lap](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade ) és a használati fájlt [EA portálon.](https://ea.azure.com) Azt is láthatja a használati összefoglaló szakaszában a Foglalás használati [EA portálon.](https://ea.azure.com)

>[!NOTE]
>Ha beszerezte a foglalás, használatalapú számlázási környezetben, tekintse meg [fenntartott példány megértéséhez használati a használatalapú fizetéses előfizetésre.](billing-understand-reserved-instance-usage.md)

A következő szakasz azt feltételezik, hogy futtatja a US régió keleti és a következő táblázat a foglalási információkat tűnik Windows Standard D1 v2 virtuális gép:

| Mező | Érték |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Mennyiség |1|
|SKU | Standard_D1|
|Régió | eastus |

## <a name="reservation-application"></a>Foglalási alkalmazás

A virtuális Gépet, a hardver része van tartalmazza, mivel a központilag telepített virtuális gép megfelel a Foglalás attribútumok. Milyen Windows szoftverek nem fedi le a fenntartott példány megtekintéséhez nyissa meg az Azure fenntartott Virtuálisgép-példányok származó szoftverek költségeit, írja be a [Azure tartalék virtuális gép példányok Windows szoftverek költségeit.](billing-reserved-instance-windows-software-costs.md)


### <a name="reservation-usage-in-csv"></a>Fürt megosztott kötetei szolgáltatás foglalás használata
Letöltheti a EA Használati csv EA portálról. További információ a szűrje a letöltött csv-fájlban, és írja be a foglalási azonosítóját. Az alábbi képernyőfelvételen látható a Foglalás kapcsolódó mezők:

![Fenntartott példány EA csv](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. További információ mezőben ReservationId jelenti. a Foglalás juttatás alkalmazása a virtuális gép használt.
2. ConsumptionMeter a MeterId a virtuális gép.
3. Ez az $0 költség ReservationMeter, mivel a virtuális gép futtatásának már fizeti a foglalás. 
4. Standard szintű, D1 egy vCPU virtuális gép és a virtuális gép központi telepítése Azure hibrid juttatás nélkül. Ezért a mérési hozzá van rendelve a felesleges ingyenesen elérhető Windows szoftver. Lásd: [Azure tartalék virtuális gép példányok Windows szoftverek költségeit.](billing-reserved-instance-windows-software-costs.md) a mérő megfelelő D sorozat 1 core virtuális gép található. Azure hibrid juttatás használata esetén ez külön díj nem alkalmazható.

### <a name="reservation-usage-in-usage-summary-page-in-ea-portal"></a>Használat összegzése lapjára EA foglalás használata

Használati is megjelennek használati összefoglaló adatai EA portál példány fenntartva: ![EA használatának összegzése](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Meg nem felszámított hardverösszetevő a virtuális gép fenntartott példány módosított. 
2. Van szó, a szoftver a Windows Azure hibrid juttatás nem használt. 

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.

Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.