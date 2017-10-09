---
title: "az Azure Security Center biztonsági riasztások aaaHandling |} Microsoft Docs"
description: "Ez a dokumentum segít toouse az Azure Security Center képességek toohandle biztonsági eseményekre."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: edb911c298a2ce93cd0ea5b22ce002005040090f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a>Biztonsági incidensek kezelése az Azure Security Centerben
Triaging és biztonsági riasztások kivizsgálásának sok időt vesz igénybe a még akkor is, hello legtöbb gyakorlott biztonsági elemzőknek. lehet, és számos rögzített tooeven tudja, honnan toobegin. A [analytics](security-center-detection-capabilities.md) tooconnect hello információk között a különböző [biztonsági riasztások](security-center-managing-and-responding-alerts.md), a Security Center adja meg a egy támadás kampány egyetlen nézetben, és az összes hello kapcsolódó riasztások – is gyorsan ismerje meg, milyen műveleteket hello támadó tartott, és milyen erőforrásokat hatással volt.

Ez a dokumentum azt ismerteti, hogyan toouse biztonsági riasztásokat a Security Center tooassist funkció biztonsági incidensek kezelése.

## <a name="what-is-a-security-incident"></a>Mi az a biztonsági incidens?
A Security Centerben egy biztonsági incidens az adott erőforráshoz tartozó összes olyan riasztás együttese, amelyek egy [támadási folyamatba](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) illeszthetők. Incidensek megjelennek a hello [biztonsági riasztások](security-center-managing-and-responding-alerts.md) csempe és panelen. Incidens jelenítenek meg hello kapcsolódó riasztások listája, amely lehetővé teszi, hogy tooobtain további információ a minden egyes előfordulásakor.

## <a name="managing-security-incidents"></a>Biztonsági incidensek kezelése
Az aktuális biztonsági események megtekintésével hello biztonsági riasztások csempén tekintheti meg. Hello Azure portál eléréséhez, és kövesse az alábbi toosee hello lépéseket minden biztonsági esemény kapcsolatos további részletekért:

1. A Security Center irányítópultjának hello, látni fogja a hello **biztonsági riasztások** csempére.

    ![Biztonsági riasztások csempe a Security Centerben](./media/security-center-incident/security-center-incident-fig1.png)

2. Kattintson a a csempe tooexpand, és ha egy biztonsági incidens észlel, akkor jelenik meg a hello biztonsági riasztások graph alább látható módon:

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig2.png)

3. Figyelje meg, hogy rendelkezik-e más ikon hello biztonsági esemény leírása tooother riasztások képest. Kattintson rá tooview több ezt az incidenst vonatkozó részletek.

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig3.png)

4. A hello **incidens** panel jelenik meg több részleteivel kapcsolatban a biztonsági incidens, amely tartalmazza a teljes leírását, a súlyosság (amely ebben az esetben magas), a jelenlegi állapotában (ebben az esetben van még *aktív*, amiből hello felhasználó még nem vett egy művelet tooit – hello incidenst hello a jobb gombbal kattintva ehhez **biztonsági riasztások** panel), hello megtámadott erőforrások (ebben az esetben *VM1*), hello hello incidens javítási lépéseket, és hello alsó ablaktáblában hello riasztások ezt az incidenst foglalt rendelkezik. Tooobtain további információt az egyes riasztások, kattintson a, és egy újabb panel fog nyissa meg, alább látható módon:

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig4.png)

hello információkat a panel konfigurációtól függően toohello riasztás. Olvasási [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) további információt a toomanage ezeket a riasztásokat. Ezzel a képességgel kapcsolatos fontos szempontok:

* Új szűrő lehetővé teszi a nézet tooIncident csak, riasztásokat csak toocustomize, vagy mindkettőt.
* hello ugyanabból a riasztásból létezhet egy incidens (ha van ilyen), valamint a toobe látható, mint egy különálló riasztás részeként.

## <a name="see-also"></a>Lásd még:
Ebből a dokumentumból megtanulta, hogyan toouse hello Security Center biztonsági incidens képességét. További információ a Security Center toolearn hello következő lásd:

* [Kezelése és reagálás toosecurity értesítések az Azure Security Centerben](security-center-managing-and-responding-alerts.md)
* [Az Azure Security Center észlelési funkciói](security-center-detection-capabilities.md)
* [Útmutató az Azure Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md)
* [Kezelése és reagálás toosecurity értesítések az Azure Security Centerben](security-center-managing-and-responding-alerts.md)
* [Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
