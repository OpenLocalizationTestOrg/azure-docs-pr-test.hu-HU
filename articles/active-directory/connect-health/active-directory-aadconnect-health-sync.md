---
title: "az Azure AD Connect Health szinkronizálási aaaUsing |} Microsoft Docs"
description: "Ez a hello Azure AD Connect Health lap, amely bemutatja, hogyan lehet hogyan toomonitor az Azure AD Connect szinkronizálása."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 1dfbeaba-bda2-4f68-ac89-1dbfaf5b4015
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56f0582be30e664026cedf15350bc23501998bfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-ad-connect-sync-with-azure-ad-connect-health"></a>Az Azure AD Connect-szinkronizálás megfigyelése az Azure AD Connect Health szolgáltatással
a következő dokumentáció hello adott toomonitoring az Azure AD Connect (Sync) az Azure AD Connect Health.  Az AD FS az Azure AD Connect Health használatával történő megfigyelésére vonatkozó információkat lásd: [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md). Az Active Directory tartományi szolgáltatások az Azure AD Connect Health használatával történő megfigyelésével kapcsolatos információkat pedig a [Using Azure AD Connect Health with AD DS](active-directory-aadconnect-health-adds.md) (Az Azure AD Connect Health használata az AD DS szolgáltatással) című témakörben találja.

![Azure AD Connect Health szinkronizálási szolgáltatás](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Az Azure AD Connect Health szinkronizálási szolgáltatás riasztásai
hello Azure AD Connect Health-riasztások szinkronizálása szakasz biztosít, akkor hello aktív riasztások listáját. Minden egyes riasztás vonatkozó információkat, a megoldási lépések és a hivatkozások toorelated dokumentációja tartalmazza. Egy aktív vagy megoldott riasztás kiválasztása jelenik meg egy új panel további információkat, valamint a lépéseket, amelyek tooresolve hello riasztást, és hivatkozások tooadditional dokumentációját. Az elmúlt hello megoldott riasztások esetében az előzményadatokat is megtekintheti.

További információkat, valamint a lépéseket a megadott egy riasztás kiválasztása és az ellenük tooresolve hello riasztás hivatkozások tooadditional dokumentációját.

![Azure AD Connect szinkronizálási szolgáltatás – hiba](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Riasztások korlátozott kiértékelése
Ha az Azure AD Connect nem használ (például ha az Attribútumszűrés változtatják hello alapértelmezett tooa egyéni konfiguráció) hello alapértelmezett konfigurációt, majd hello Azure AD Connect Health-ügynök nem tölti fel hello hiba események kapcsolódó tooAzure AD Connect.

Ezzel a riasztások kiértékelése hello hello szolgáltatás korlátozza. Ilyenkor megjelenik egy szalagcím, amely a szolgáltatás alatt futó Azure-portál hello ezt az állapotot jelzi.

![Azure AD Connect Health szinkronizálási szolgáltatás](./media/active-directory-aadconnect-health-sync/banner.png)

Módosíthatja a "Beállítások" gombra kattintva, és lehetővé teszi az Azure AD Connect Health agent tooupload összes hibanaplókat.

![Azure AD Connect Health szinkronizálási szolgáltatás](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Szinkronizálási elemzés
Rendszergazdák gyakran érdemes tooknow kapcsolatos hello időt toosync módosítások tooAzure AD és a változások hello mennyisége. Ez a szolgáltatás biztosít egy egyszerűen toovisualize ez alatt diagramjait hello használata:   

* Szinkronizálási műveletek késleltetése
* Objektummódosítási trend

### <a name="sync-latency"></a>Szinkronizálási késések
Ez a funkció egy tendenciagrafikont hoz létre az hello szinkronizálási műveletek (importálás, exportálás stb.) késéseiről biztosítja.  Ez biztosítja, hogy egy gyors és egyszerűen toounderstand hello késés, amely további vizsgálatot igénylő nem csak a műveleteket (ha változásokat számos nagyobb), hanem egy módon toodetect rendellenességeket késés hello.

![Szinkronizálási késések](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Alapértelmezés szerint csak hello hello hello Azure AD-összekötő "Exportálás" műveletének késései jelenik meg.  toosee hello összekötő további műveleteinek és más összekötők tooview műveleteinek kattintson a jobb gombbal a hello diagramon, válassza a diagram szerkesztése lehetőséget vagy hello "Késés módosítása" gombra, és válassza ki a hello adott műveletet és összekötőt.

### <a name="sync-object-changes"></a>Szinkronizálási objektumok módosításai
Ez a funkció egy tendenciagrafikonon a hello kiértékelése megtörténik, és tooAzure AD exportált módosítások számát.  Ma, miközben a rendszer toogather hello szinkronizálási naplók ezt az információt is nehézkes.  hello diagram nemcsak nemcsak leegyszerűsíti a környezetében bekövetkező változások hello száma, de is megjeleníti hello előforduló hibákat figyelő.

![Szinkronizálási késések](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Objektum szintű szinkronizálási hibajelentés (Előzetes verzió)
Ez a funkció olyan szinkronizálási hibákról készít jelentést, amelyek a Windows Server AD és az Azure AD közötti, Azure AD Connect használatával történő identitásszinkronizálás közben jelentkeznek.

* hello a jelentés által lefedett hello sync-ügyfél által rögzített hibákat (az Azure AD Connect 1.1.281.0 verzió vagy újabb)
* Utolsó szinkronizálási művelet hello hello szinkronizálási motoron hello előforduló tartalmazza. ("Export" hello Azure AD-összekötő a.)
* Az Azure AD Connect Health szinkronizálási ügynöke toohello szükséges végpontok kimenő kapcsolódás hello jelentés tooinclude hello legújabb adatok kell rendelkeznie.
* hello jelentés **után 30 percenként frissített** az Azure AD Connect Health szinkronizálási ügynöke által feltöltött hello adatokkal. A következő főbb funkciók hello biztosít

  * Hibák kategorizálása
  * Objektumlista kategóriánként összesített hibákkal
  * Egy adott helyen hello hibák adatait az összes hello
  * Egymás mellett tekintheti át objektumokat a következő hiba miatt tooa ütközés
  * Töltse le a hello hibajelentést, egy csak egyet (hamarosan elérhető)

### <a name="categorization-of-errors"></a>Hibák kategorizálása
hello jelentés kategorizálja a következő kategóriák hello hello meglévő objektumszinkronizálási hibák:

| Kategória | Leírás |
| --- | --- |
| Duplikált attribútum |Olyan hibák, amelyek akkor jelentkeznek, amikor az Azure AD Connect megpróbál olyan objektumokat létrehozni vagy frissíteni az Azure AD-ben, amelyek egy vagy több duplikált attribútummal rendelkeznek, noha az attribútumnak (például proxyAddresses vagy UserPrincipalName) egyedinek kell lennie a bérlőben. |
| Eltérés az adatokban |Hibák a hello soft-egyeztetés sikertelen toomatch objektumok szinkronizálási hibákat eredményez. |
| Sikertelen adatérvényesítés |Hibák miatt tooinvalid adatok, például a UserPrincipalName, például kritikus fontosságú attribútumok nem támogatott karaktereket formázza a hibákat, az Azure AD írása előtt érvényesítése sikertelen. |
| Túl nagy attribútum |Hibák, ha egy vagy több attribútum nagyobb, mint hello engedélyezett méretét, hossza vagy count. |
| Egyéb |Minden egyéb hibák, amelyek nem egyeznek a hello kategóriák felett. A visszajelzések függvényében ez a kategória további alkategóriákra lesz bontva. |

![Szinkronizálási hibajelentés – összefoglalás](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Szinkronizálási hiba – kategóriák](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Objektumlista kategóriánként összesített hibákkal
Állapotkategóriák vizsgálatát az egyes kategóriák kategória hello hiba rendelkező objektumok hello listáját adja meg.
![Szinkronizálási hibajelentés – lista](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>A hiba adatai
A következő adatok érhető el hello részletes nézet minden egyes hibához

* Hello azonosítóinak *AD-objektum* érintett
* Hello azonosítóinak *Azure AD-objektum* érintett (a megfelelő)
* Hiba leírása, hogyan toofix
* Kapcsolódó cikkek

![Szinkronizálási hibajelentés – részletes nézet](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-hello-error-report-as-csv"></a>Fürt megosztott kötetei hello esetleges hibajelentésben való megjelenítéshez letöltése
Hello kiválasztásával "Exportálás" gomb letöltheti CSV-fájl összes hello hibákkal kapcsolatos összes hello adatokkal.

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Szinkronizálási hibák elhárítása](../connect/active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Duplikált attribútummal kapcsolatos rugalmasság](../connect/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Az Azure AD Connect Health-ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Az Azure AD Connect Health műveletei](active-directory-aadconnect-health-operations.md)
* [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md)
* [Az Azure AD Connect Health használata az AD DS szolgáltatással](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Az Azure AD Connect Health verzióelőzményei](active-directory-aadconnect-health-version-history.md)