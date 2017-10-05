---
title: "Váratlan költségek megakadályozása, számlázási - Azure kezelése |} Microsoft Docs"
description: "Útmutató az Azure számlázásának váratlan költségek elkerülése érdekében. Költség-nyomon követését és a felügyeleti szolgáltatások használata a Microsoft Azure-előfizetés."
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2017
ms.author: tonguyen
ms.openlocfilehash: c1185063880fa58da9487bc98977c0acae8f403d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Azure számlázás és költség felügyeleti váratlan díjak elkerülése végett

Amikor regisztrál az Azure-ba, számos módon teheti a ráfordítás megvilágításához. A [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/) költségek becslést ad egy Azure-erőforrás létrehozása előtt. A [Azure-portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) tesz lehetővé az aktuális költség lebontása és előrejelzés az előfizetéséhez. Ha azt szeretné, csoport, és megismerheti a különböző projektekhez és csoportok költségek, tekintse meg [erőforrás címkézés](../azure-resource-manager/resource-group-using-tags.md). Ha a szervezete jelentési rendszer, amely szeretné használni, tekintse meg a [API-k számlázási](billing-usage-rate-card-overview.md). 

Emellett [töltse le a korábbi számlákat és fájlok részletességi](billing-download-azure-invoice-daily-usage-date.md) győződjön meg arról, hogy helyesen alkalmazott számára. A napi használat és a számla összehasonlításával kapcsolatos további információkért lásd: [a számlázási megérteni a Microsoft Azure](billing-understand-your-bill.md).

Ha az előfizetés egy nagyvállalati szerződés (EA), a Cloud Solution Provider (CSP) vagy az Azure szponzorálás, majd számos szolgáltatást ebben a cikkben nem vonatkozik Önre. Ehelyett azt rendelkeznek különböző eszközöket tartalmazza, amelyek költségek kezelésére használhatja. Lásd: [további erőforrásokat a EA CSP és szponzorálás](#other-offers).

Ha az előfizetés egy ingyenes próbaverzióra [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), nyissa meg Nyomtatókategóriák vagy BizSpark, az Azure az előfizetés automatikusan letiltja a kreditek használatakor. További tudnivalók [költségkeretek](#spending-limit) kívánja kerülni az előfizetés unexpectantly le van tiltva. 

## <a name="get-estimated-costs-before-adding-azure-services"></a>Becsült költség beolvasása az Azure-szolgáltatás hozzáadása előtt

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Becsült költség, a árképzési Számológép online használatával

Tekintse meg a [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/) beolvasni a szolgáltatás továbbra is érdekli becsült havi költségét. Első fél Azure-erőforrás egy becsült költség segítségével adhat hozzá.

![A díjszabási Számológép menü képernyőképe](./media/billing-getting-started/pricing-calc.png)

Például egy A1 Windows virtuális gép (VM) becsült költség $66.96 USD/hónap számítási órák, ha nem adja meg azt a teljes futásidő:

![A megjelenítő, hogy az A1 Windows virtuális gép átmásolása $66.96 USD havi költségeket árképzési Számológép képernyőképe](./media/billing-getting-started/pricing-calcVM.png)

Az árakkal kapcsolatos további információkért tekintse meg a [gyakran ismételt kérdések](https://azure.microsoft.com/pricing/faq/). Vagy ha azt szeretné, egy Azure értékesítői felvegye, lépjen kapcsolatba a 1-800-867-1389.

### <a name="review-the-estimated-cost-in-the-azure-portal"></a>Tekintse át a becsült költség az Azure-portálon

Általában amikor egy szolgáltatás az Azure portálon, nincs egy nézet, amellyel havonta egy hasonló becsült költségét jeleníti meg. Például ha úgy dönt, hogy a Windows virtuális gép méretét, megjelenik az becsült havi költségét a számítási órák:

![Példa: egy A1 Windows virtuális Gépre átmásolása $66.96 USD havi költségeket](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="set-up-billing-alerts"></a>Számlázási értesítések beállítása

Számlázási riasztás beállítása kapniuk, ha a használati költségek haladhatja meg a megadott összeget. Ha a havi kreditek, beállítása fel a megadott méretű használatakor. További információkért lásd: [beállítása a Microsoft Azure-előfizetések riasztások számlázási](billing-set-up-alerts.md).

![A számlázási riasztás e-mail képernyőképe](./media/billing-getting-started/billing-alert.png)

> [!NOTE]
> A funkció jelenleg még előzetes, jelölje be a használati rendszeresen.

Előfordulhat, hogy szeretné használni a árképzési Számológép a becsült költség az eszközöket, az első riasztás.

### <a name="spending-limit"></a>Ellenőrizheti, hogy a költségkeret maximumát

Ha egy előfizetési kreditek használó, majd a költségkeret maximumát van kapcsolva, alapértelmezés szerint. Ezzel a módszerrel töltött a jóváírások, amikor a hitelkártya nem get számítjuk fel. Tekintse meg a [teljes listáját az Azure-ajánlatok és a költségkeret](https://azure.microsoft.com/support/legal/offer-details/).

Azonban ha elérte a költségkeret maximumát, a szolgáltatások beolvasása le van tiltva. Ez azt jelenti, hogy a virtuális gép fel van szabadítva. Szolgáltatás állásidő elkerülése érdekében ki kell kapcsolnia a költségkeretét. Bármely túlhasználati a lekérdezi a hitelkártya fájlon alakzatot számítjuk fel. 

Ha Ön már készült költségkeret a megtekintéséhez keresse fel a [előfizetések tekintse meg az Account Center](https://account.windowsazure.com/Subscriptions). Egy szalagcím akkor jelenik meg, ha a költségkeret maximumát:

![Képernyőkép a kiadásokat korlátja alatt a fiók közepén kapcsolatos figyelmeztetés](./media/billing-getting-started/spending-limit-banner.PNG)

Kattintson a szalagcím, és távolítsa el a költségkeret maximumát utasításokat követve. Ha nem adta meg hitelkártyaadatokat regisztráció során, meg kell adnia, hogy távolítsa el a költségkeretét. További információkért lásd: [Azure költségeik korlátozása – hogyan működik, és hogyan lehet engedélyezni, vagy távolítsa el](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="ways-to-monitor-your-costs-when-using-azure-services"></a>A költségek figyelése az Azure-szolgáltatások használatakor módjai

### <a name="tags"></a>Címkék hozzáadása az erőforrások az elszámolási adatok

Támogatott szolgáltatások címkék számlázási adatainak csoportosítására is használhatja. Például ha a különböző csapatok több virtuális gépeken futtatja, majd meg címkék csoportosítására használhatók költségek költségközpont (HR, marketing, pénzügyi) vagy a környezet (, éles üzem előtti tesztelése). 

![Képernyőkép a címkék beállítása a portálon](./media/billing-getting-started/tags.PNG)

A címkék teljes reporting nézetek különböző költség jelenik meg. Például, hogy látható a [elemző nézet költség](#costs) azonnal és [használati .csv részletességi](#invoice-and-usage) az első számlázási időszak után.

További információkért lásd: [az Azure-erőforrások rendszerezése címkék használatával](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a>Rendszeresen költség bontásához a portálon, és Írás gyakorisága

Miután beszerezte a futó szolgáltatásokat, rendszeresen ellenőrzi azokat, amelyek mennyi költségszámítás még meg. Tekintse meg az aktuális ráfordítás, és Írás gyakorisága Azure-portálon. 

1. Látogasson el a [előfizetések panel az Azure-portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) , és válasszon egy előfizetést.

2. A költségek részletes információkat lásd: kell, és sebessége írni a helyi menü paneljén. Azt nem támogatja az előfizetéshez (figyelmeztetés fog megjelenni felső).

    ![Képernyőkép a írási sebesség és a részletes információkat az Azure-portálon](./media/billing-getting-started/burn-rate.PNG)

3. Kattintson a **elemzés** a bal oldalon erőforrás a költség lebontása látható a listában. Várjon 24 órát, miután hozzáadta az adatok feltöltése a szolgáltatás.

    ![Képernyőfelvétel a költség elemzés nézet Azure-portálon](./media/billing-getting-started/cost-analysis.PNG)

4. Például különböző tulajdonságok szerint szűrheti [címkék](#tags), erőforráscsoport és timespan. Kattintson a **alkalmaz** megerősítéséhez, hogy a szűrők és **letöltése** Ha azt szeretné, exportálja a nézet egy Comma-Separated értékeket tartalmazó (.csv) fájlt.

5. Emellett egy erőforrás megtekintéséhez kattintson előzményeit, és mekkora a erőforrás költségek minden nap.

    ![Azure-portálon ráfordítás előzménynézeteket képernyőképe](./media/billing-getting-started/costhistory.PNG)

Azt javasoljuk, hogy ellenőrizze a költségek, megjelenik a kiválasztott szolgáltatások látott becsléseket ad. Ha a költségek menet eltérnek a becslések, ellenőrizze a tarifacsomag (A1 vs VM A0 csomag, például), amely a kijelölt erőforrások. 

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Érdemes lehet engedélyezni az költség-kivágáshoz szolgáltatások, mint az automatikus rendszerleállítást virtuális gépekhez

A forgatókönyvtől függően automatikus leállítási tudta konfigurálni a virtuális gépek Azure-portálon. További információkért lásd: [Azure Resource Manager használatával virtuális gépek automatikus leállítási](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Képernyőkép az automatikus rendszerleállítást lehetőségek a portálon](./media/billing-getting-started/auto-shutdown.PNG)

Automatikus leállítási nem ugyanaz, mint amikor leállítja az Energiagazdálkodási lehetőségek a virtuális Gépen belül. Automatikus leállítási leáll, és felszabadítja a további használati díjak leállítja a virtuális gépek. További információkért tekintse meg a gyakran ismételt kérdések a díjszabás [Linux virtuális gépek](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) és [Windows virtuális gépek](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) kapcsolatos virtuális gép állapota.

A fejlesztési és tesztkörnyezetek további költség-kivágáshoz funkciói, tekintse meg [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Kapcsolja be, és tekintse meg az Azure javaslatokat biztosít

[Az Azure Advisor](../advisor/advisor-overview.md) egy előzetes verziójú funkciók, amelynek segítségével csökkentheti a költségeket, alacsony használat rendelkező erőforrások azonosításával. Kapcsolja be az Azure-portálon:

![Képernyőfelvétel az Azure Advisor gomb az Azure portálon](./media/billing-getting-started/advisor-button.PNG)

Ezt követően kaphat végrehajthatóként javaslatok a **költség** az Advisor irányítópult lapon:

![Képernyőkép az Advisor költség javaslat – példa](./media/billing-getting-started/advisor-action.PNG)

További információkért lásd: [Advisor költség javaslatok](../advisor/advisor-cost-recommendations.md).

### <a name="billing-api"></a>Számlázási API

A számlázási API használatával programozott módon a használati adatok beolvasása. A RateCard API és a használati API együtt történő beolvasásához használja a számlázott használat. További információkért lásd: [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a>További források és bizonyos esetekben

### <a name="ea-csp-and-sponsorship-customers"></a>EA, CSP és szponzorálás használó ügyfelek számára
Konzultáljon a ügyfélfelelőshöz vagy a Azure partner a kezdéshez.

| Ajánlat | Erőforrások |
|-------------------------------|-----------------------------------------------------------------------------------|
| Nagyvállalati Szerződés (EA) | [EA portal](https://ea.azure.com/), [docs súgó](https://ea.azure.com/helpdocs), és [Power BI-jelentés](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (Felhőszolgáltató) | Kérdezze meg a szolgáltató |
| Azure-szponzorálás | [Szponzorálás portál](https://www.microsoftazuresponsorships.com/) |

Ha az Ön által felügyelt informatikai olyan nagy szervezethez, azt javasoljuk, olvasási [Azure enterprise scaffold](../azure-resource-manager/resource-manager-subscription-governance.md) és a [vállalati informatikai tanulmány](http://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (.pdf letöltés, csak angol nyelvű).

### <a name="check-your-subscription-and-access"></a>Ellenőrizze az előfizetés és a hozzáférés

Megtekintés költségek szükséges [számlázási adatokat előfizetések szintű hozzáférést](billing-manage-access.md), csak a fiókadminisztrátor férhet hozzá, de a [Account Center](https://account.windowsazure.com/Home/Index), módosítsa a számlázási információkat és -előfizetések kezelése. A fiókadminisztrátor az a személy, aki merült fel a regisztrációs folyamatot. További információkért lásd: [hozzáadása vagy módosítása, hogy az előfizetés vagy a szolgáltatások kezelése az Azure rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md).

Ha Ön a fiókadminisztrátor megtekintéséhez keresse fel a [előfizetések panel az Azure portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) , és tekintse meg rendelkezik hozzáféréssel előfizetések listájából. Keresse meg a **a szerepkör**. Ha a felirat látható *fiókadminisztrátor*, akkor Ön ok. Ha például a más felirat látható *tulajdonos*, akkor nem rendelkezik teljes körű jogosultságokkal.

![Képernyőfelvétel a szerepkör az előfizetések nézetben az Azure-portálon](./media/billing-getting-started/sub-blade-view.PNG)

Ha még nem fiókadminisztrátor, akkor valaki valószínűleg Önnek megadó részleges hozzáférésének [Azure Active Directory szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md) (RBAC). Előfizetések és számlázási adatok, módosítás kezeléséhez [fiókadminisztrátor található](billing-subscription-transfer.md#whoisaa) és kérje meg a feladatok végrehajtásához vagy [az előfizetés átvitele](billing-subscription-transfer.md).

Ha a fiókadminisztrátor már nem a szervezet és számlázási kezeléséhez szükséges [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 
## <a name="need-help-contact-support"></a>Segítség Kapcsolatfelvétel a támogatási szolgáltatással

Ha segítségre van szüksége, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.
