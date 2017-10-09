---
title: az Operations Management Suite (OMS) aaaIntegrating |} Microsoft Docs
description: "Ezenkívül toousing hello OMS szabványos szolgáltatásait, integrálható az egyéb felügyeleti alkalmazások és szolgáltatások tooprovide egy hibrid felügyeleti környezet, tooprovide egyéni felügyeleti forgatókönyvek egyedi tooyour környezet vagy egy egyéni tooprovide az ügyfelek felügyeletét.  Ez a cikk áttekintést OMS integrálása a különböző lehetőségek közül, és hivatkozásokat tartalmaz tooarticles részletes technikai információkat biztosít."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fc5f3a8a-77f7-4103-bd7e-744c15ffcca7
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: dce752dcdc6c725bbafd49db4a5055750487ecf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-with-operations-management-suite-oms"></a>Az Operations Management Suite (OMS) integrálása
Az Operations Management Suite a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra.  Ezenkívül toousing hello OMS szabványos szolgáltatásait, integrálható az egyéb felügyeleti alkalmazások és szolgáltatások tooprovide egy hibrid felügyeleti környezet, tooprovide egyéni felügyeleti forgatókönyvek egyedi tooyour környezet vagy egy egyéni tooprovide az ügyfelek felügyeletét.  Ez a cikk ismerteti a különböző lehetőségek közül OMS integrálása áttekintése szolgáltatását, és hivatkozásokat tartalmaz részletes technikai információkat biztosító tooarticles. 

## <a name="log-analytics"></a>Log Analytics
A tárházban lévő Azure tárolódjanak a webhelykezelési adatok Naplóelemzési által gyűjtött.  Hello tárházban tárolt összes adat érhető el, amelyek rendkívül nagy adatmennyiségek gyors elemzés biztosít napló keresi.  Az integráció követelményeit és az elérhetővé válik az elemzést követően az új adatok és hello tárház tooprovide új képi megjelenítés adatainak tooextract toopopulate hello tárház vagy egy másik felügyeleti eszközzel toointegrate lehet.

Minden adat hello tárházban bejegyzésként tárolja.  Hello tárház tölthető fel, ha a felhasználók hello rekordtípus a megoldást használó és a tulajdonságok leírása kell megadnia.  Visszaállíthatja az adatokat, ha tájékoztatásra van szüksége a dolgozunk hello adatokról.

![Hello OMS-tárház feltöltése](media/operations-management-suite-integration/repository.png)

### <a name="populate-hello-log-analytics-repository"></a>Hello Naplóelemzési tárház feltöltése
Több módon a hello OMS-tárház feltöltéséhez.  hello használt módszertől függ tényezőket, mint ahol hello forrásadatok, a hello formátumú hello adatokat, és amely ügyfelek toosupport van szüksége.  Miután hello tárház adatokat tárolja, nincs különbség, hogy gyűjtötte teszi.

hello következő részek a hello különböző beállítások a hello OMS-tárház feltöltéséhez.

#### <a name="connected-sources-and-data-sources"></a>Csatlakoztatott adatforrások és adatforrások
Csatlakoztatott adatforrások olyan hello helyek, ahol lehet adatokat beolvasni a hello OMS-tárházat.  Adatforrások és a megoldások a csatlakoztatott források futtatásához, és állítsa be az összegyűjtött hello adatokat.  Ha az alkalmazás írja az adatokat tooone ezeknek az adatforrásoknak, majd gyűjtheti az hello adatforrás konfigurálásával.  Például, ha az alkalmazás hozza létre a Syslog-események, majd azok gyűjthetők hello Syslog adatforrás egy Linux-ügynök.

* [A Naplóelemzési adatforrások](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Megoldások
Megoldások OMS hello funkcióinak kiterjesztése.  Megoldás lehet, hogy adatokat gyűjteni hello csatlakoztatott adatforrás, vagy az elemzés hajthat végre a hello tárházban már gyűjtött rögzíti.  Egyes Microsoft által biztosított megoldások hello részletesen hello adatokon, amely összegyűjti az egyes cikk tartalmaz.

* [A Naplóelemzési megoldások](../log-analytics/log-analytics-add-solutions.md)

#### <a name="http-data-collector-api"></a>HTTP adatgyűjtő API
hello napló Analytics HTTP adatokat gyűjtő API a REST API-t, amely lehetővé teszi tooadd JSON toohello Naplóelemzési adattárház.  Ez az API használhatja, ha egy alkalmazás, amely nem biztosítja a hello egyikével adatokat, más adatforrások vagy megoldások.  Lehet, hogy a használt toopopulate hello tárház bármely ügyfél hello API meghívása, és nem szükséges minden adatforrás vagy megoldás hello adatgyűjtési ütemezést.

* [Elemzés HTTP adatok naplógyűjtő API](../log-analytics/log-analytics-data-collector-api.md)

### <a name="retrieve-data-from-hello-log-analytics-repository"></a>Hello Naplóelemzési tárház adatainak lekérése
Több módon adattárból hello OMS adatok lekéréséhez.  Előfordulhat, hogy felhasználók tooretrieve adatok hello OMS-konzollal, és adja meg különböző képi megjelenítések és elemzése.  Hello adatokat is egy másik felügyeleti megoldás például külső folyamat kérhetnek le.

#### <a name="log-searches"></a>Napló-keresések
A hello OMS-tárházban tárolt összes adat napló keresések keresztül érhető el.  Felhasználók előfordulhat, hogy a saját alkalmi elemzést hello OMS-konzolon, vagy hozzon létre egy irányítópultot a képi megjelenítés, adott napló keresés.  Megoldások tartalmazhatnak egyéni nézetek alapján előre definiált keresések megjelenítésekkel.  A külső alkalmazás- vagy felügyeleti eszköz a hello OMS-tárházban hello napló keresése API tooaccess adatokat is használhatja.  

* [A Naplóelemzési napló-keresések](../log-analytics/log-analytics-log-searches.md)
* [A Naplóelemzési jelentkezzen search REST API-n](../log-analytics/log-analytics-log-search-api.md)
* [Napló Analytics parancsmagok](https://msdn.microsoft.com/library/mt188224.aspx)

#### <a name="custom-views"></a>Egyéni nézetek
hello adatforrásnézet-tervezőből toocreate egyéni nézetek hello OMS, hogy módszereket biztosítsanak a felhasználóknak a képi megjelenítés és a megoldásban hello adatok elemzése lehetővé teszi.  Minden egyes nézetében egy csempe hello hello konzol és az Ön által meghatározott napló keresések alapuló képi megjelenítés részek tetszőleges számú fő lapján látható.

* [Napló Analytics adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md)

#### <a name="power-bi"></a>Power BI
A Naplóelemzési is automatikusan adatok exportálása adattárból hello OMS Power BI-ba így kihasználhatja a képi megjelenítések és elemzésére szolgáló eszközöket.  Elvégez ehhez az exporthoz ütemezés szerint, hello adatok mentése toodate maradnak. 

* [A Naplóelemzési adatok tooPower BI exportálása](../log-analytics/log-analytics-powerbi.md)

## <a name="automation"></a>Automatizálás
OMS automatizálhatja folyamatok tooreact toocollected adatok vagy tooperform más felügyeleti funkciókat.  Azt előfordulhat, hogy az alkalmazás adatokat gyűjteni, és helyezze be hello OMS-tárház, vagy előfordulhat, hogy automatizálja hello válasz toodata hello tárházban található egy ismert probléma javítása. 

![Automatizálás](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbookok
Azure Automation Runbookjai PowerShell-parancsfájlok és munkafolyamatok futtatása hello Azure felhőben.  Használhatja őket toomanage erőforrások az Azure-ban vagy bármely más hello felhőből elérhető erőforrások.  Runbookok használata a hibrid forgatókönyv-feldolgozó helyi adatközpontban is futtatható.  Runbook-indítási hello Azure-portál vagy külső folyamatok, számos módszer, például a PowerShell használatával, vagy hello Automation API.

* [Runbook elindítása az Azure Automationben](../automation/automation-starting-a-runbook.md)
* [Azure Automation parancsmagokkal](https://msdn.microsoft.com/library/dn690262.aspx)
* [Automatizálási REST API-n](https://msdn.microsoft.com/library/mt662285.aspx)
* [Automatizálási .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Riasztások
A riasztási szabályok automatikusan napló keresések tooa ütemezés szerint futtatni.  Ha hello eredmények megfelelő adott feltételek hello eredményül kapott riasztás elindít egy forgatókönyvet az Azure Automation, vagy hívja a webhook, amely elindíthatja egy külső folyamatban.  Mindkét ezeket a válaszokat lehetnek többek között szereplő hello napló keresése hello adatok hello riasztás részletes adatait.

* [A Naplóelemzési riasztások](../log-analytics/log-analytics-alerts.md)
* [Napló Analytics riasztási API](../log-analytics/log-analytics-api-alerts.md)

## <a name="backup-and-site-recovery"></a>Biztonsági mentés és helyreállítás
Az Azure biztonsági mentés és helyreállítás a vállalati adatok védelme és a kiszolgálók és alkalmazások hello rendelkezésre állásának biztosítása a szolgáltatásokhoz.  Kihasználhatja a szolgáltatások tooperform ilyen forgatókönyvek például az alkalmazás biztonsági mentési szolgáltatások vagy a virtuális gépek a feladatátvétel kezdeményezése.

* [Az Azure biztonsági mentést készítő parancsmagok](https://msdn.microsoft.com/library/mt619253.aspx)
* [Azure Site Recovery REST API-n](https://msdn.microsoft.com/library/azure/mt750497.aspx)
* [Az Azure Site Recovery-parancsmagok](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Egyéni megoldások
Integrációs programot is foglalják magukban azokat egy egyéni megoldás toorun, a munkaterületén, vagy az ügyfél munkaterületen.  A megoldás tartalmazhat bármilyen hello integrációs módszerek hozzáadása tooother erőforrások tooprovide a cikkben egy teljes körű forgatókönyv.  hello megoldásban hello erőforrások vannak csomagolva, úgy, hogy hello megoldás eltávolítása után létrehozott hello erőforrásokat el lesznek távolítva hello OMS-munkaterület és az Azure-előfizetés.

Például a megoldás sikerült automatizálási runbook toogather és a folyamat adatokat, és töltse fel tagokkal hello Naplóelemzési tárházat hello HTTP adatait gyűjtője API használatával.  Egyéni nézet, amely megadja, és elemzi az összegyűjtött hello adatokat is tartalmazhatnak.  

* Egyéni megoldások (hamarosan elérhető)    

## <a name="next-steps"></a>Következő lépések
* Hivatkozás hello [OMS SDK](operations-management-suite-sdk.md) technikai információkat az OMS-szolgáltatások automatizálása.  

