---
title: "aaaPrevent váratlan költségek kezelése – Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooavoid az Azure számlázásának váratlan költségek. Költség-nyomon követését és a felügyeleti szolgáltatások használata a Microsoft Azure-előfizetés."
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
ms.openlocfilehash: d380f27861531351ac8e570469c59a84b9ca99e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Azure számlázás és költség felügyeleti váratlan díjak elkerülése végett

Amikor regisztrál az Azure-ba, számos módon teheti tooget segítenek meghatározni, hogy a ráfordítás. Hello [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/) költségek becslést ad egy Azure-erőforrás létrehozása előtt. Hello [Azure-portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) biztosít hello aktuális költség lebontása és előrejelzés az előfizetéséhez. Ha szeretné, hogy toogroup, és különböző projektekhez és csoportok költségek ismertetése, tekintse meg [erőforrás címkézés](../azure-resource-manager/resource-group-using-tags.md). Ha a szervezete jelentési rendszer, hogy szeretné-e toouse, tekintse meg hello [API-k számlázási](billing-usage-rate-card-overview.md). 

Emellett [töltse le a korábbi számlákat és fájlok részletességi](billing-download-azure-invoice-daily-usage-date.md) toomake meg arról, hogy helyesen alkalmazott. A napi használat és a számla összehasonlításával kapcsolatos további információkért lásd: [a számlázási megérteni a Microsoft Azure](billing-understand-your-bill.md).

Ha az előfizetés egy nagyvállalati szerződés (EA), a Cloud Solution Provider (CSP) vagy az Azure szponzorálás keresztül, számos szolgáltatást ebben a cikkben tooyou nem vonatkozik. Ehelyett azt rendelkeznek különböző eszközöket tartalmazza, amelyek költségek kezelésére használhatja. Lásd: [további erőforrásokat a EA CSP és szponzorálás](#other-offers).

Ha az előfizetés egy ingyenes próbaverzióra [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), nyissa meg Nyomtatókategóriák vagy BizSpark, az Azure az előfizetés automatikusan letiltja a kreditek használatakor. További tudnivalók [költségkeretek](#spending-limit) tooavoid, hogy az előfizetés unexpectantly le van tiltva. 

## <a name="get-estimated-costs-before-adding-azure-services"></a>Becsült költség beolvasása az Azure-szolgáltatás hozzáadása előtt

### <a name="estimate-cost-online-using-hello-pricing-calculator"></a>Becsült költség online hello árképzési Számológép használata

Tekintse meg a hello [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/) tooget kíváncsiak vagyunk hello szolgáltatás becsült havi költségét. Bármely első fél Azure-erőforrás tooget becsült költség adhat hozzá.

![Képernyőfelvétel a hello Számológép menü díjszabása](./media/billing-getting-started/pricing-calc.png)

Például egy A1 Windows virtuális gép (VM) a számítási órák $66.96 USD/hónap ha hagyja hello teljes futásidő becsült toocost:

![Képernyőfelvétel a hello árképzési Számológép, amely mutatja, hogy az A1 Windows virtuális gép becsült toocost $66.96 USD / hónap](./media/billing-getting-started/pricing-calcVM.png)

Az árakkal kapcsolatos további információkért tekintse meg a [gyakran ismételt kérdések](https://azure.microsoft.com/pricing/faq/). Vagy ha azt szeretné, hogy tootalk tooan Azure értékesítői, forduljon a 1-800-867-1389.

### <a name="review-hello-estimated-cost-in-hello-azure-portal"></a>Felülvizsgálati hello becsült költség hello Azure-portálon

Általában amikor hello Azure-portálon ad hozzá egy szolgáltatás, a rendszer havonta egy hasonló becsült költségét jeleníti meg a nézet. Például ha úgy dönt, hogy a Windows virtuális gép hello méretét, megjelenik az hello becsült havi költségét hello számítási órák:

![Példa: egy A1 Windows virtuális Gépre becsült toocost $66.96 USD / hónap](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="set-up-billing-alerts"></a>Számlázási értesítések beállítása

Beállítani számlázási riasztások tooget e-maileket a használati költségek haladhatja meg a megadott összeget. Ha a havi kreditek, beállítása fel a megadott méretű használatakor. További információkért lásd: [beállítása a Microsoft Azure-előfizetések riasztások számlázási](billing-set-up-alerts.md).

![A számlázási riasztás e-mail képernyőképe](./media/billing-getting-started/billing-alert.png)

> [!NOTE]
> A funkció jelenleg még előzetes, jelölje be a használati rendszeresen.

Érdemes lehet toouse hello költség becsült a hello díjkalkulátor az eszközöket az első riasztás.

### <a name="spending-limit"></a>Ellenőrizheti, hogy a költségkeret maximumát

Ha egy előfizetési kreditek használó, majd költségkeret hello van kapcsolva, alapértelmezés szerint. Ezzel a módszerrel töltött a jóváírások, amikor a hitelkártya nem get számítjuk fel. Lásd: hello [teljes listáját az Azure-ajánlatok és hello rendelkezésre állását költségkeret](https://azure.microsoft.com/support/legal/offer-details/).

Azonban ha elérte a költségkeret maximumát, a szolgáltatások beolvasása le van tiltva. Ez azt jelenti, hogy a virtuális gép fel van szabadítva. tooavoid állásideje, ki kell kapcsolnia költségkeret hello. Bármely túlhasználati a lekérdezi a hitelkártya fájlon alakzatot számítjuk fel. 

Nyissa meg, hogy a kapott a következő költségkeret, ha toosee toohello [előfizetések megtekintéséhez hello Account Center](https://account.windowsazure.com/Subscriptions). Egy szalagcím akkor jelenik meg, ha a költségkeret maximumát:

![Képernyőkép a kiadásokat korlát éppen a hello Account Center van kapcsolatos figyelmeztetés](./media/billing-getting-started/spending-limit-banner.PNG)

Kattintson a hello szalagcím, és kövesse az utasításokat tooremove hello költségkeret. Ha nem adta meg hitelkártyaadatokat regisztráció során, meg kell adnia azt költségkeret tooremove hello. További információkért lásd: [Azure költségeik korlátozása – hogyan működik, és hogyan tooenable vagy távolítsa el](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="ways-toomonitor-your-costs-when-using-azure-services"></a>Többféleképpen toomonitor a költségek, Azure-szolgáltatások használatakor

### <a name="tags"></a>Adja hozzá a címkék tooyour erőforrások toogroup a számlázási adatokat

Támogatott szolgáltatások címkék toogroup elszámolási adatok is használhatja. Például ha a különböző csapatok több virtuális gépeken futtatja, majd használhatja címkék toocategorize költségek költségközpont (HR, marketing, pénzügyi) vagy a környezet (, éles üzem előtti tesztelése). 

![Képernyőkép a címkék beállítása hello portálon](./media/billing-getting-started/tags.PNG)

teljes reporting nézetek különböző költség hello címkék jelennek meg. Például, hogy látható a [elemző nézet költség](#costs) azonnal és [használati .csv részletességi](#invoice-and-usage) az első számlázási időszak után.

További információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a>Rendszeresen költség lebontása hello portálon, és Írás gyakorisága

Miután beszerezte a futó szolgáltatásokat, rendszeresen ellenőrzi azokat, amelyek mennyi költségszámítás még meg. Megtekintheti a hello aktuális töltött és sebessége írása Azure-portálon. 

1. A Microsoft hello [előfizetések panel az Azure-portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) , és válasszon egy előfizetést.

2. Hello költség részletes információkat lásd: kell, és sebessége írása hello előugró panelen. Azt nem támogatja az előfizetéshez (figyelmeztetés fog megjelenni hello felső mellett).

    ![Képernyőkép a írási sebesség és a részletes információkat a hello Azure-portálon](./media/billing-getting-started/burn-rate.PNG)

3. Kattintson a **elemzés** hello lista toohello bal oldali toosee hello költség lebontásban erőforrás. Várjon 24 órát, a szolgáltatás hello adatok toopopulate hozzáadása után.

    ![Képernyőfelvétel a hello költség elemző nézet Azure-portálon](./media/billing-getting-started/cost-analysis.PNG)

4. Például különböző tulajdonságok szerint szűrheti [címkék](#tags), erőforráscsoport és timespan. Kattintson a **alkalmaz** tooconfirm hello szűrők és **letöltése** Ha azt szeretné, hogy tooexport hello nézet tooa Comma-Separated értékeket tartalmazó (.csv) fájlt.

5. Emellett kattinthat erőforrás toosee fordítják az előzmények és mennyi hello erőforrás költségek minden nap.

    ![Képernyőfelvétel a hello fordítják az előzmények megtekintését Azure-portálon](./media/billing-getting-started/costhistory.PNG)

Azt javasoljuk, hogy ellenőrizze a hello költségek hello becsléseket ad látott hello szolgáltatások kiválasztásakor megjelenik. Ha hello költségek menet eltér a becslések, ellenőrizze a tarifacsomagot (A1 vs VM A0 csomag, például), amely a kijelölt erőforrások hello. 

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Érdemes lehet engedélyezni az költség-kivágáshoz szolgáltatások, mint az automatikus rendszerleállítást virtuális gépekhez

A forgatókönyvtől függően tudta konfigurálni a virtuális gépek az Azure-portálon hello automatikus rendszerleállítást. További információkért lásd: [Azure Resource Manager használatával virtuális gépek automatikus leállítási](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Képernyőfelvétel a hello portálon automatikus leállítási lehetőséget](./media/billing-getting-started/auto-shutdown.PNG)

Automatikus leállítási nem hello azonos a belül leállításakor hello VM az Energiagazdálkodási lehetőségek. Automatikus leállítási leáll, és felszabadítja a virtuális gépek toostop további használati díjak. További információkért tekintse meg a gyakran ismételt kérdések a díjszabás [Linux virtuális gépek](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) és [Windows virtuális gépek](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) kapcsolatos virtuális gép állapota.

A fejlesztési és tesztkörnyezetek további költség-kivágáshoz funkciói, tekintse meg [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Kapcsolja be, és tekintse meg az Azure javaslatokat biztosít

[Az Azure Advisor](../advisor/advisor-overview.md) egy előzetes verziójú funkciók, amelynek segítségével csökkentheti a költségeket, alacsony használat rendelkező erőforrások azonosításával. Kapcsolja be a hello Azure-portálon:

![Képernyőfelvétel az Azure Advisor gomb az Azure portálon](./media/billing-getting-started/advisor-button.PNG)

Ezt követően kaphat hello végrehajthatóként javaslatok **költség** hello Advisor irányítópult lapján:

![Képernyőkép az Advisor költség javaslat – példa](./media/billing-getting-started/advisor-action.PNG)

További információkért lásd: [Advisor költség javaslatok](../advisor/advisor-cost-recommendations.md).

### <a name="billing-api"></a>Számlázási API

A számlázási API tooprogrammatically get használati adatok felhasználásával. Hello RateCard API és hello használati API együtt tooget a számlázott használati használja. További információkért lásd: [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a>További források és bizonyos esetekben

### <a name="ea-csp-and-sponsorship-customers"></a>EA, CSP és szponzorálás használó ügyfelek számára
Konzultáljon tooyour ügyfélfelelőshöz vagy a Azure partner tooget elindult.

| Ajánlat | Erőforrások |
|-------------------------------|-----------------------------------------------------------------------------------|
| Nagyvállalati Szerződés (EA) | [EA portal](https://ea.azure.com/), [docs súgó](https://ea.azure.com/helpdocs), és [Power BI-jelentés](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (Felhőszolgáltató) | Előadás tooyour szolgáltató |
| Azure-szponzorálás | [Szponzorálás portál](https://www.microsoftazuresponsorships.com/) |

Ha az Ön által felügyelt informatikai olyan nagy szervezethez, azt javasoljuk, olvasási [Azure enterprise scaffold](../azure-resource-manager/resource-manager-subscription-governance.md) és hello [vállalati informatikai tanulmány](http://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (.pdf letöltés, csak angol nyelvű).

### <a name="check-your-subscription-and-access"></a>Ellenőrizze az előfizetés és a hozzáférés

Megtekintés költségek szükséges [előfizetések szintű hozzáférés toobilling információk](billing-manage-access.md), azonban csak a hello fiókadminisztrátor férhet hozzá a hello [Account Center](https://account.windowsazure.com/Home/Index), módosítsa a számlázási információkat és -előfizetések kezelése. Üdvözöljük a rendszergazdákat fiók az hello személy, aki végrehajtás hello regisztrációs folyamat során. További információkért lásd: [kezelése hello előfizetés vagy szolgáltatások hozzáadása vagy módosítása Azure-rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md).

Ha fiókadminisztrátor, hogy hello Ugrás toohello toosee [előfizetések panel az Azure-portálon hello](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) rendelkezik hozzáféréssel előfizetések listájából hello tekintse meg. Keresse meg a **a szerepkör**. Ha a felirat látható *fiókadminisztrátor*, akkor Ön ok. Ha például a más felirat látható *tulajdonos*, akkor nem rendelkezik teljes körű jogosultságokkal.

![Képernyőfelvétel a hello hello Azure-portálon előfizetések nézet szerepkör](./media/billing-getting-started/sub-blade-view.PNG)

Ha Ön most nem hello fiókadminisztrátor, akkor valaki valószínűleg Önnek megadó részleges hozzáférésének [Azure Active Directory szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md) (RBAC). toomanage előfizetések és számlázási adatok, módosítás [hello fiókadminisztrátor található](billing-subscription-transfer.md#whoisaa) , és kérjen tooperform hello feladatok vagy [hello előfizetés tooyou átviteli](billing-subscription-transfer.md).

Ha a fiókadminisztrátor már nem a szervezet és toomanage számlázási kell [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 
## <a name="need-help-contact-support"></a>Segítség Kapcsolatfelvétel a támogatási szolgáltatással

Ha segítségre van szüksége, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.
