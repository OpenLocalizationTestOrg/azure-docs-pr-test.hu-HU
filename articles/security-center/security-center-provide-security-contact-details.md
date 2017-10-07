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
# <a name="provide-security-contact-details-in-azure-security-center"></a>Adja meg az Azure Security Centerben a biztonsági kapcsolattartási adatait
Azure Security Center javasolni fogja adni biztonsági kapcsolattartási adatait az Azure-előfizetéssel, ha még nem tette meg. Ezt az információt a Microsoft toocontact által használható, ha a Microsoft biztonsági válasz Center (MSRC) hello észleli, hogy az az ügyféladatok egy törvénybe ütköző vagy jogosulatlan félnek elérhető-e. MSRC hajt végre, válassza ki a biztonság ellenőrzése hello Azure-hálózatot és az infrastruktúra, és fenyegetést eszközintelligencia és visszaélés panaszokat kapott harmadik felek számára.

A hello első napi előfordulásakor riasztást, és csak magas súlyosságú riasztások küldött e-mailben értesítést. Az e-mail-beállítások kizárólag előfizetési szabályok esetében konfigurálhatóak. Erőforráscsoportok előfizetésen belül öröklik ezeket a beállításokat.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **adja meg biztonsági kapcsolattartási adatait**.
   ![Adja meg a biztonsági ügyfél][1]
2. Ekkor megnyílik hello panel **adja meg biztonsági kapcsolattartási adatait**. Válassza ki a hello Azure-előfizetés tooprovide kapcsolattartási adatokat a.
   ![Biztonsági kapcsolattartói adatok megadása][2]
3. Egy második **adja meg biztonsági kapcsolattartási adatait** panel nyílik meg.

   * Adja meg a hello biztonsági kapcsolattartási e-mail címet vagy címeket vesszővel válassza el egymástól elválasztva. Nincs e-mail címeket, amelyeket megadhat számának korlátozása toohello.
   * Adjon meg egy biztonsági nemzetközi telefonszámát.
   * magas súlyosságú riasztások kapcsolatos tooreceive e-mailek hello bekapcsolását **küldése e-mailt küld kapcsolatos riasztások**.
   * A jövőbeli hello hello beállítás toosend e-mail értesítések toosubscription tulajdonosok fog rendelkezni. Ez a beállítás jelenleg szürkén jelenik meg.
   * Válassza ki **OK** tooapply hello biztonsági információk tooyour előfizetés forduljon.

## <a name="see-also"></a>Lásd még:
További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
