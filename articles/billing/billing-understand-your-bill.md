---
title: "aaaUnderstand a számlázási az Azure-bA |} Microsoft Docs"
description: "Megtudhatja, hogyan tooread és a használati és az Azure-előfizetéshez tartozó számlázási ismertetése"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: tonguyen
ms.openlocfilehash: a3195eb129b1576e8cb665aa6f88a1a2647edd78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-your-bill-for-microsoft-azure"></a>A Microsoft Azure-hoz kapcsolódó számlák magyarázata
toounderstand az Azure számlázás, hasonlítsa össze a számla hello részletes napi használati fájl és a költségeket, az Azure-portálon hello jelentések hello.

a számla PDF- és a részletes napi használati fájl CSV letöltés, másolatát tooobtain lásd [beolvasni a számlázási számla és a napi használati adatok Azure](billing-download-azure-invoice-daily-usage-date.md). 

Részletes feltételeket és a számla és a napi használat fájl részletes leírását lásd: [a Microsoft Azure számlán feltételek megismeréséhez](billing-understand-your-invoice.md) és [megértése feltételeket a Microsoft Azure részletes használati](billing-understand-your-usage.md). 

További részletek a felügyeleti jelentések költség hello: [Azure-portálon költségkezelésére](https://docs.microsoft.com/en-us/azure/billing/billing-getting-started).


## <a name="charges"></a>Hogyan tehetem meg arról, hogy helyesen-e a számlán hello díjak?
Ha a kívánt további részleteket a számlán járnak, többféle beállítások.

### <a name="option-1-review-your-invoice-and-compare-hello-usage-and-costs-with-hello-detailed-usage-csv-file"></a>1. lehetőség: Tekintse át a számla és hello használati és költségek összehasonlításra hello részletes használati CSV-fájl

hello részletes használati CSV-fájl tartalmazza a költségeket számlázási időszakban és napi használat. tooget a részletes használati CSV-fájlból, lásd: [beolvasni a számlázási számla és a napi használati adatok Azure](https://docs.microsoft.com/en-us/azure/billing/billing-download-azure-invoice-daily-usage-date).

A használati díjak hello mérő szinten jelenik meg. hello következő feltételeket: hello ugyanaz a mindkét hello számla és hello részletes használati fájl. Például a számlázási ciklus hello számlán hello egyenértékű toohello hello részletes használati fájl látható számlázási időszakban.

 | Számla (PDF) | A részletes használati (CSV)|
 | --- | --- |
|Billing Cycle (Számlázási ciklus) | Billing Period (Számlázási időszak) |
 |Név |Meter Category (Mérési kategória) |
 |Típus |A mérési alkategória |
 |Erőforrás |Meter Name (Mérés neve) |
 |Régió |Meter Region (Mérési régió) |
 |Consumed (Felhasznált mennyiség) |Consumed Quantity (Felhasznált mennyiség) |
 |Tartalmazza |Included Quantity (Bennefoglalt mennyiség) |
 |Billable (Számlázandó) |Overage Quantity (Kereten túli mennyiség) |

Hello **használati díjak** a számla szakasza értéke hello teljes minden mérő a a számlázási időszak alatt felhasznált. Például hello következő képernyőfelvétel a hello Azure Scheduler szolgáltatás használati járnak.

![Számla használati díjak](./media/billing-understand-your-bill/1.png)

Hello **utasítás** szakasz a fürt megosztott kötetei szolgáltatás részletes használati látható hello azonos kell fizetni. Mindkét hello *felhasznált* összeg és *érték* egyezés hello számla.

![Fürt megosztott kötetei szolgáltatás használati díjak](./media/billing-understand-your-bill/2.png)

Ez a díj naponta, nyissa meg toohello részletes információkat toosee **napi használat** hello CSV részét. "Feladatütemező" szerint szűrhet *mérő kategória* és láthatja, melyik napon hello mérő lett megadva, és mekkora felhasznált. Hello *erőforrás* és *erőforráscsoport* információk is láthatók az összehasonlításhoz. Hello *felhasznált* értékeket kell egyezzen toowhat tartozó hello számlán látható.

![A fürt megosztott kötetei szolgáltatás hello napi használati adatai](./media/billing-understand-your-bill/3.png)

napi tooget hello költség szorzási hello *felhasznált* hello összegeket *arány* hello értéket **utasítás** szakasz.

További információ az hello számla toolearn lásd [megismerése az Azure számla](billing-understand-your-invoice.md).

toolearn hello CSV, hello oszlopainak mindegyike kapcsolatban lásd: [az Azure részletes használatának megértéséhez](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-hello-usage-and-costs-in-hello-azure-portal"></a>2. lehetőség: Tekintse át a számla és összehasonlításra hello használati és költségek az hello Azure-portálon

hello Azure portál segítségével ellenőrizze a költségeket. hello Azure-portálon költség felügyeleti diagramok a használati és hello költségek a számla gyors áttekintést biztosít.

toocontinue hello a fenti példában, a Microsoft hello [előfizetések oldalán](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), jelölje ki az előfizetését, és válassza a **elemzés**. Ott hello-időtartam megadása, és tekintse meg a hello Azure Scheduler szolgáltatás használati költségek.

![Azure-portálon költség elemző nézet](./media/billing-understand-your-bill/4.png)

toosee hello napi költségek bontását **előzmények költség**, hello sorára kattintson.

![Azure-portálon költség előzményeinek megtekintése](./media/billing-understand-your-bill/5.png)

toolearn több, lásd: [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](billing-getting-started.md#costs).

## <a name="external"></a>Mi a helyzet a külső szolgáltatási díjak?
Külső szolgáltatások független szolgáltatás szerezhetők (más néven Azure piactér rendelések), és külön vannak számlázva. hello költségek az Azure számlán nem jelennek meg. több, lásd: toolearn [az Azure külső szolgáltatási díjak megismeréséhez](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Hogyan tehetem egy fizetési?

Ha beállított hitelkártyával vagy bankkártyával egy a fizetési módot, a hello fizetési hello számlázási időszak végén 10 napon belül automatikusan fel van töltve. A hitelkártya fényében hello sorelemet fel kellene **MSFT Azure**.

Ha Ön [kell fizetnie, amennyit által számlázás](billing-how-to-pay-by-invoice.md), küldése a számla hello alján megtalálja a fizetési toohello helyét. További segítségért [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-hello-status-of-a-payment-made-by-credit-card"></a>Hogyan ellenőrzi a hitelkártya által hello állapotát?

[Támogatási jegy létrehozásával](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooask a fizetési hello állapot. 

## <a name="tips-for-cost-management"></a>Költség felügyeleti tippek
- Becsült költségeit hello segítségével [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/) és [összköltsége tulajdonjoga Számológép](https://aka.ms/azure-tco-calculator), és hello [részletes díjszabási információk minden egyes szolgáltatás](https://azure.microsoft.com/en-us/pricing/).
- [Állítson be riasztásokat számlázási](billing-set-up-alerts.md).
- [Tekintse át a használati és rendszeresen hello Azure-portálon a költségek](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.

Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.
