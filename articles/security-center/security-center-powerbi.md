---
title: az Azure Security Center adataiba a Power BI aaaGet insights |} Microsoft Docs
description: "hello Azure Security Center Power BI-tartalomcsomag segítségével egyszerűen toofind biztonsági riasztásokat, a javaslatokat, megtámadott erőforrásokat és trendeket, a jelentéskészítéshez létrehozott adatkészlet alapján."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 0ded6bc7-52e8-43b4-8940-0bee137526e3
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: yurid
ms.openlocfilehash: af68aef626034fe03d793c36b515a3f26619e5f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Mélyebb betekintés az Azure Security Center adataiba a Power BI segítségével
Hello [Power BI-irányítópulton](http://aka.ms/azure-security-center-power-bi) az Azure Security Center lehetővé teszi a toovisualize, elemzését és szűrését javaslatait és biztonsági riasztásait bárhonnan, akár a mobileszközökről. Hello Power BI irányítópult tooreveal trendek használja, és támadási mintákat - erőforrás vagy IP-forráscím biztonsági riasztások megtekintése és szerint biztonsági kockázatokat pedig erőforrás vagy kor szerint.

A Security Center javaslatainak és a biztonsági riasztásoknak más adatokkal történő összefűzésének is számos érdekes megoldása létezik, így például az [Azure vizsgálati naplók](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) és az [Azure SQL Database Auditing](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/) adatainak együttes felhasználása. A Power BI-irányítópult nyújtanak, és exportálhatja is az adatok tooExcel vonatkozó a felhőalapú erőforrások biztonsági állapotát hello jelentések egyszerű elkészítése.

## <a name="using-azure-security-center-dashboard-tooaccess-power-bi"></a>Az Azure Security Center irányítópultján tooaccess Power BI használatával
Hello Azure Security Center irányítópultján tooaccess Power BI-jelentéseket is használható. Ez a feladat hajtsa végre a hello lépéseket tooperform:

1. A hello **az Azure Security Center** irányítópultján kattintson **Power BI** gombra.

    ![Csatlakozás tooAzure Security Center Power BI használatával](./media/security-center-powerbi/security-center-powerbi-fig1-1-newUI-2017.png)
2. Hello **Power BI** panel megnyitása hello jobb oldalán látható módon a következő képernyő hello:

    ![Csatlakozás tooAzure Security Center Power BI használatával](./media/security-center-powerbi/security-center-powerbi-fig1-new11-2017.png)
3. Létrehozásakor hello Power BI-irányítópultot hello először, dönthet úgy hello alábbi lehetőségei a hello **Power BI böngészés** panel:

   * **Biztonság irányítópult**: válassza ezt a beállítást, ha azt szeretné, hogy toocreate egy irányítópultot, amely biztonsági állapotát, a szálakat és az észleléseket tartalmazza. E beállítás használata a DevOps szerepkör esetében gyakoribb, amely a védelmi állapot és az észlelt riasztások előfizetések közötti elemzését végzi.
   * **Házirend-kezelési irányítópult**: válassza ezt a beállítást, ha azt szeretné, hogy tooexplore kezelési és kényszerítési házirendet.  E beállítás az irányítás-központúbb központi informatikai alkalmazások esetében gyakoribb. Használhatják az irányítópult toogain láthatóságának és elemzések a biztonsági házirend betartása a szervezet.
   * Ha már rendelkezik Power BI irányítópulttal, kattintson a **Ugrás tooyour aktuális Power BI-irányítópultot**.
4. E példában kattintson a **Security insights irányítópult** lehetőségre. Ha hello első alkalommal, létrehozásakor egy Power BI-irányítópult a Security Center Ön tooinstall hello tartalomcsomag kéri. Kattintson a **beolvasása** hello gombjára **csomagokat a Power BI-tartalmat** ablakban látható módon a következő képernyő hello:

    ![Azure Security Center Security Insights irányítópult](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)
5. Hello **tooAzure Security Center biztonság fokozható csatlakozás** ablak jelenik meg. Győződjön meg arról, hogy hello **hitelesítési** módszer **oAuth2** alább látható módon, és kattintson a **bejelentkezés** gombra.

    ![Authentication](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)
6. Kérheti tooauthenticate újra a Azure hitelesítő adataival. A hitelesítést követően létrejön az irányítópult. Hello irányítópult létrehozása után jelenik meg hello hasonló struktúrájú jelentést a következő képernyő hello ismertetett módon:

    ![Power BI-irányítópult](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)

> [!NOTE]
> Hello jelentés frissítését a naponta ütemezett tootake-hely. Abban az esetben, ha a frissítésben hibát tapasztal, olvassa el a [frissítésével kapcsolatos potenciális problémák hello Azure Security Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), további információt a tootroubleshoot.
>
>

Itt láthatja a biztonsági riasztások és javaslatok hello száma, valamint a virtuális gépek, az Azure SQL-adatbázisok és a hálózati erőforrások az Azure Security Center által figyelt hello száma.

A hivatkozás tooAzure Security Center toohello Azure-portálra irányítja át. hello diagramok révén könnyen toovisualize információ a biztonsági javaslatokra és riasztásokra, beleértve:

* Erőforrás biztonsági állapota
* Függőben lévő javaslatok
* Virtuális gépekre vonatkozó javaslatok
* Riasztások adott idő alatt
* Megtámadott erőforrások
* Megtámadott IP-címek

Minden egyes diagram mögött további elemzések találhatók. Válassza ki a csempe toosee további információt. Például hello **erőforrás biztonsági állapota** csempe kiválasztásával további részleteket vonatkozó függőben lévő javaslatokról erőforrásonként látható módon a következő képernyő hello:

![Javaslatok](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Ha ez a diagram minden sorában kattint, hello mások fogják toogray, és összpontosíthat csak egy kijelölt hello a. tooreturn toohello irányítópultján kattintson **az Azure Security Center** hello alatt **irányítópultok** lehetőség hello bal oldali ablaktáblában a ezen a lapon.

> [!NOTE]
> Ha azt szeretné, hogy toocustomize a jelentések további mezők hozzáadásával vagy a meglévő látványelemek módosításával, szerkesztheti a hello jelentést. További információkért olvassa el a [Kommunikáció egy jelentéssel a Power BI szerkesztés nézetében](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) részt.
>
>

Hello **riasztások adott idő alatt, megtámadott erőforrások** és **támadó IP-címek** csempék hello hasonló kimenetet fog rendelkezni, minden egyes egyik gombjára kattintva. Ez akkor fordul elő, mert hello jelentés összesíti a három változóra vonatkozó információkat, és meghívja az **támadás alatt lévő erőforrások** látható módon a következő képernyő hello:

![Támadás alatt lévő erőforrások](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Ezen a ponton is ez a jelentés másolatának mentése, nyomtassa ki vagy hello elérhető hello-beállítások segítségével közzéteheti hello webes **fájl** menü.

![Fájl menü](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Az Azure Security Center adatainak felfedezése Power BI segítségével
Csatlakozás toohello [Power BI tartalomcsomag szolgáltatásokhoz](https://msit.powerbi.com/groups/me/getdata/services) Power BI-ban, majd hajtsa végre a lépéseket követve hello:

1. A hello **Power BI tartalomcsomag** ablak parancsot érhet el két alább látható módon.

    ![Power BI tartalomcsomag](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

   > [!NOTE]
   > Ez a cikk első része hello már végre csak egy lehetőség, amely Azure Security Center házirendkezelés látják.
   >
   >
2. Ebben a példában a hello célból, kattintson a **beolvasása** hello a **Azure Security Center házirendkezelés** csempére.
3. A hello **csatlakozzon a Security Center házirendkezelés tooAzure** ablakban, győződjön meg arról, hogy tooselect **oAuth2** alatt **hitelesítési módszer** legördülő menüben, a lent látható módon, és kattintson **Bejelentkezés** gombra.

    ![Házirendkezelés ablak](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)
4. Átirányított tooan hitelesítési lapot, ahol meg kell adnia, hogy azok be tooconnect tooAzure Security Center hello hitelesítő adatok lesznek. Hello hitelesítési folyamat befejezése után, Power BI elindítja toobuild adatok importálása a jelentéseket. Ebben az időszakban a következő üzenet a böngésző jobb sarkában hello hello jelenhetnek meg:

    ![Csatlakozás tooAzure Security Center Power BI használatával](./media/security-center-powerbi/security-center-powerbi-fig4.png)

   > [!NOTE]
   > Amikor hello irányítópult készül hello első alkalommal is igénybe vehet a szokásosnál hosszabb ideig, különösen ha több előfizetése esetében.
   >
   >
5. Hello folyamat befejeződése után az Azure Security Center Power BI irányítópultja betölti hello **házirendkezelés** jelentést az alábbihoz hasonló toohello:

    ![A Házirendkezelés irányítópultja](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban, megtudta, hogyan toouse az Azure Security Center Power bi-ban. További információ az Azure Security Center toolearn hello következő lásd:

* [Azure Security Center tervezéséhez és az üzemeltetési útmutatóban](security-center-planning-and-operations-guide.md) – további hogyan tooplan Azure Security Center bevezetését.
* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – további hogyan tooconfigure biztonsági beállításait az Azure Security Centerben
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztások
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
