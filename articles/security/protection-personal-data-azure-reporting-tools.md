---
title: "az Azure-ral Jelentéskészítő eszközök személyes adatok védelme aaaDocument |} Microsoft Docs"
description: "Hogyan toouse Azure jelentéskészítési szolgáltatásaival és technológiáival toohelp megvédeni a személyes adatok."
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 3230d26ed308a8a0e72421c001793be06334a7c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="document-protection-of-personal-data-with-azure-reporting-tools"></a>Dokumentálja a személyes adatok védelme az Azure-ral Jelentéskészítő eszközök

Ez a cikk bemutatja, hogyan lehet hogyan toouse Azure reporting services és technológiák toohelp személyes adatok védelme.

## <a name="scenario"></a>Forgatókönyv

A nagy körutazás vállalati telephelyének hello az Amerikai Egyesült Államokban, a műveletek toooffer útvonalak hello mediterrán, Adriai, és Balti tengerek, valamint hello Brit-szigetekre növekszik. toohelp ilyen erőfeszítések szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit

hello vállalati használja a Microsoft Azure feldolgozási és a vállalati adatok tárolására. Ez magában foglalja a személyes azonosításra alkalmas adatokat, például neveket, címeket, telefonszámokat, és a globális felhasználói bázis hitelkártya adatait. Minden helyen hagyományos emberi erőforrások adatokat, például a címet, telefonszámot, azonosító számokat és a vállalat alkalmazottai orvosi információt is tartalmaz. hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok, amely tartalmazza az aktuális és korábbi ügyfelek tootrack kapcsolatokkal személyes adatokat.

Vállalati alkalmazottak hozzáférési hello hálózati hello vállalati távoli iroda és utazás ügynökök található hello világ rendelkezik toosome vállalati erőforrások eléréséhez.

## <a name="problem-statement"></a>Probléma leírása

hello vállalati kell védelmében hello alkalmazottak és a felhasználók a személyes adatok keresztül egy többrétegű biztonsági stratégia, amely az Azure felügyeleti és biztonsági szolgáltatások tooimpose szigorú a személyes adatok hozzáférési tooand feldolgozása, kell lennie képes toodemonstrate a védelmi intézkedések toointernal és külső auditorok.

## <a name="company-goal"></a>Vállalati cél

A védelmi jellegű biztonsági stratégia részeként ez egy vállalati cél tootrack személyes adatok összes hozzáférési tooand feldolgozása, és gondoskodjon arról, hogy a megfelelő adatvédelmi személyes adatok védelmét dokumentációban létezik és működik.

## <a name="solutions"></a>Megoldások

A Microsoft Azure átfogó figyelési, naplózási és diagnosztikai eszközök toohelp nyomon követése és rögzíteni a tevékenységeket és elérése és a személyes adatok, földrajzi folyamata, és a külső hozzáférés toopersonal adatok feldolgozása kapcsolódó eseményeket. Hello felhőben személyes adatok biztonsága érdekében feladata egy megosztott, mert a Microsoft rendelkező ügyfelek emellett biztosítja:

- Részletes információkat a saját feldolgozási ügyfelek adatok

- A Microsoft által felügyelt biztonsági intézkedések

- Hol és hogyan ügyfelek adatokat küld

- Microsoft saját adatvédelmi értékelést folyamat részletes adatait

### <a name="azure-active-directory"></a>Azure Active Directory

[Az Azure Active Directory](https://azure.microsoft.com/services/active-directory/) a Microsoft felhőalapú, több-bérlős directory és az identity management szolgáltatás. szolgáltatás-Bejelentkezés hello és naplózási jelentéskészítési lehetőségeket biztosít részletes be- és alkalmazás használati tevékenység információk toohelp figyelése, és ellenőrizze a megfelelő hozzáférési toocustomers és alkalmazottak a személyes adatok.

A tevékenység jelentések két típusa van:

- Hello [tevékenység jelentések/naplók](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) adja meg a rendszer tevékenységek/feladatok részletes rekord

- Hello [bejelentkezések jelentés/tevékenységnapló](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) bemutatja, akik minden tevékenység hello ellenőrzési jelentésben felsorolt hajtott végre

Hello két együtt használja, nyomon követheti az hello előzmények végrehajtott minden tevékenység és minden egyes végző felhasználók listáját. Mindkét típusú jelentésekre testre szabhatók, és szűrhetők.

#### <a name="how-do-i-access-hello-audit-and-security-logs"></a>Hogyan érhetem el hello naplózási és biztonsági naplókat?

hello naplózási és biztonsági naplókat elérhető hello Active Directory portálon három különböző módon: hello keresztül **tevékenység** szakasz (válassza **naplók** vagy **bejelentkezések**), vagy a **felhasználók és csoportok** vagy **vállalati alkalmazások** alatt **kezelése** az Active Directoryban. Jelentés is elérhető keresztül hello Azure Active Directory reporting API-val. 

1. Hello Azure-portálon, válassza ki **Azure Active Directoryban.**

2. A hello **tevékenység** szakaszban jelölje be **a naplók.**

    ![](media/protection-personal-data-azure-reporting-tools/image001.png)

3. Testre szabhatja a hello listanézet kattintva **oszlopok** hello eszköztáron.

4.  Jelöljön ki egy elemet hello lista nézet toosee összes rendelkezésre álló részleteit.

    ![](media/protection-personal-data-azure-reporting-tools/image003.png)

Az Azure Active Directory jelentéskészítési is magában foglalja a kétféle típusú biztonsági jelentések **felhasználók meg van jelölve, a kockázat** és **kockázatos bejelentkezések**, amely is segítenek az esetleges kockázatokat az Azure környezetben.

További információ a reporting service hello: [Azure Active Directory reportingban](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)

Látogasson el [Azure Active Directory Tevékenységjelentések](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal#activity-reports) a érhető el az Azure Active Directoryban hello jelentésekkel kapcsolatos további részletekért. Ez a hely kapcsolatos további részleteket tartalmaz tooaccess és [auditnaplókat Tevékenységjelentések](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) és [bejelentkezési Tevékenységjelentések](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) hello portálon. Emellett tájékoztatást kapcsolatos [felhasználók meg van jelölve, a kockázat](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-user-at-risk) és [kockázatos bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-risky-sign-ins) jelentéseket.

A Microsoft hello [Azure Active Directory naplózási API-referencia](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference) további információt a hely tooconnect tooAzure Directory reporting programozott módon.

### <a name="log-analytics"></a>Log Analytics

[Naplófájl Analytics](https://azure.microsoft.com/services/log-analytics/) is [adatokat gyűjteni Azure figyelő](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage) toocorrelate azt más adatokkal, és adja meg a további elemzés. Azure figyelő összegyűjti és elemzi az Azure környezetben vonatkozó figyelési adatokat. 

Például a napló keresések, nézetek és megoldásokat a Naplóelemzési elemzésére szolgáló eszközöket gyűjtött adatokat, így a teljes környezet központi elemzés dolgozhat. Naplóelemzési összesíteni, és Windows-eseménynaplók, IIS-naplókba és rendszerbejegyzések, így könnyen észleli a potenciális személyes adatszivárgáshoz teheti ki a személyes adatok toounauthorized felhasználókat.

#### <a name="how-do-i-use-log-analytics"></a>Log Analytics használata?

A Naplóelemzési hello OMS-portálon vagy hello bármely webböngészővel Azure-portálon keresztül érheti el. A Naplóelemzési tartalmaz egy lekérdezési nyelv tooquickly olvashatók be, és összevonni az adatokat a hello tárházban. Létrehozhat és menthet napló keresések toodirectly adatelemzés hello portálon.

a Naplóelemzési munkaterület hello Azure-portálon a toocreate hello a következő:

1. Válassza ki **Naplóelemzési** hello hello piactér szolgáltatások közül.

2. Válassza ki **létrehozása,** majd adja meg az OMS-munkaterület hello nevét, válassza ki az előfizetés, az erőforráscsoport, a hely és IP-címek.

3. Kattintson a **OK** toodisplay a munkaterületek listáját.

4. Válassza ki a munkaterület toosee hozzá tartozó részletek.

    ![](media/protection-personal-data-azure-reporting-tools/image004.png)

A Microsoft hello [Log Analytics-dokumentáció](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) hello szolgáltatással kapcsolatos további toolearn.

Látogasson el a hello [Ismerkedjen meg a Naplóelemzési munkaterület](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started) oktatóanyag toocreate egy értékelési munkaterület, és megtudhatja, hogyan toouse hello szolgáltatás hello alapjait.

A fent leírt Microsoft hello pontosabb információt a weblapok meg, hogy miként naplózza az tooconnect toouse Naplóelemzési hello a következő:

[A Windows Eseménynapló adatforrások Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)

[A Naplóelemzési naplózza az IIS](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs)

[A Naplóelemzési Syslog adatforrások](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-syslog)

### <a name="azure-monitorazure-activity-log"></a>Az Azure figyelő/Azure tevékenységnapló 

[Az Azure figyelő](https://azure.microsoft.com/services/monitor/) alapszintű infrastruktúra metrikák és a naplók biztosít a legtöbb szolgáltatás a Microsoft Azure-ban.
Figyelés segítségével toogain mélyebben elemezheti az Azure alkalmazásokkal kapcsolatos. Az Azure figyelő hello Azure diagnostics bővítmény (Windows vagy Linux) a legtöbb alkalmazás szintű metrikák és naplók összegyűjtésére támaszkodik. [hello Azure tevékenységnapló](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) tekintheti meg az Azure-figyelő hello erőforrások egyike. Minden API-hívás nyomon követi, és számos olyan az előforduló tevékenységek információt biztosít [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
Az Azure-infrastruktúra hello szerinti erőforrásokra vonatkozó kereshet hello tevékenységnapló (korábbi nevén az operatív vagy a vizsgálati naplók) kapcsolatos információt. 

Bár hello információkat rögzíti a hello tevékenység napló számítógépfiókokhoz tooperformance és a szolgáltatás állapotának-is adatok kapcsolódó tooprotection információkat. Az hello tevékenységnapló is meghatározható hello "mi, ki, és mikor" vonatkozó bármelyik írási műveleteket (PUT, POST, Törlés) végzett hello erőforrásokat az Azure-előfizetéshez.

Például biztosít egy olyan rekordot rendszergazda törli a hálózati biztonsági csoport, amely jelentős hatással lehet a személyes adatok hello védelméről. Tevékenység naplóbejegyzések Azure figyelő 90 napig tárolja.

#### <a name="how-do-i-use-hello-data-collected-by-azure-monitor"></a>Miként használható az Azure-figyelő által gyűjtött hello adatokat?

Számos módon toouse hello adatok hello műveletnaplóban és más Azure-figyelő erőforrások vannak.

- Hello adatok tooother helye valós sorban is adatfolyam.

- Adattárolás hello hosszabb időtartamokat hello alapértelmezett beállításokat, mint a használatával egy [Azure storage-fiók](https://docs.microsoft.com/azure/storage/common/storage-introduction) és egy adatmegőrzési beállítást.

- Használatával a grafikus és diagramokat, visual hello adatok hello [Azure-portálon](https://azure.microsoft.com/features/azure-portal/), [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), [Microsoft PowerBI](https://powerbi.microsoft.com/), vagy külső képi megjelenítés eszközök.

- Hello adatok hello Azure figyelő REST API-t parancssori felület parancsai használatával kérdezheti [PowerShell](https://docs.microsoft.com/powershell/) parancsmagok vagy a .NET SDK hello.

tooget figyelőjének Azure, válassza ki **több szolgáltatások** a hello Azure-portálon.

1. Görgessen lefelé, túl**figyelő** a hello **figyeléséhez és felügyeletéhez** szakasz.

    ![](media/protection-personal-data-azure-reporting-tools/image005.png)

2.  Figyelő nyílik meg hello **tevékenységnapló** nézet.

    ![](media/protection-personal-data-azure-reporting-tools/image007.png)

Hozhat létre és mentsen lekérdezéseket, közös szűrők, majd a PIN-kód hello legfontosabb lekérdezések tooa portál Irányítópultjára, hogy minden esetben ha a megadott feltételeket teljesítő eseményekről.

1. Erőforráscsoport timespan és eseménykategória hello nézettel jeleníthetők meg.

    ![](media/protection-personal-data-azure-reporting-tools/image008.png)

2. Majd rögzíthető lekérdezések tooa portál Irányítópultjára hello kattintva **PIN-kód** gombra. Ez segít a működési adatok egyetlen forrásként a szolgáltatásoknak. hello lekérdezés neve és a találatok száma hello irányítópulton megjelenik.

Hello figyelő tooview metrikákat használ az összes Azure-erőforrások, konfigurálja a diagnosztikai beállítások és a riasztások, és hello napló keresése. További információ a hogyan toouse hello Azure figyelő és tevékenységnapló, lásd: [Ismerkedés az Azure-figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

### <a name="azure-diagnostics"></a>Azure Diagnostics 

az Azure-ban hello diagnosztika funkció lehetővé teszi, hogy több forrásból származó adatok gyűjtésére. lehet, hogy hello Windows eseménynaplók, többek között hello biztonsági naplóba, különösen akkor hasznos, nyomon követése és dokumentálásához személyes adatok védelmét. hello biztonsági naplóba nyomon követi a bejelentkezés sikeres és sikertelen események, valamint engedélyek módosítását, bizonyos típusú támadások, biztonsági házirendek módosításai, biztonsági csoporttagsági változások és még sok más jelző mintázatok észlelését.

Esemény azonosítója 4695 például akkor próbált toohello unprotection naplózható védett adatok riasztást küld. Ez vonatkozik toohello Data Protection API (DPAPI), ami segít a tooprotect adatok, például a titkos kulcsok, tárolt hitelesítő adatok és más bizalmas adatokat.

#### <a name="how-do-i-enable-hello-diagnostics-extension-for-windows-vms"></a>Hogyan engedélyezhető hello diagnosztika bővítmény Windows virtuális gépek?

Használja PowerShell tooenable hello diagnosztika bővítményét a Windows virtuális gépek, így toocollect naplóadatait. hello lépéseket úgy határozza meg, milyen központi telepítési modell (erőforrás-kezelő vagy klasszikus) használja. tooenable hello diagnosztika bővítmény egy meglévő virtuális gépen keresztül hello Resource Manager üzembe helyezési modellben létrehozott, hello használható [Set-AzureRMVMDiagnosticsExtension PowerShell-parancsmag](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension?view=azurermps-4.3.1).

   ![](media/protection-personal-data-azure-reporting-tools/image009.png)

*\$diagnosticsconfig_path* hello elérési toohello tartalmazó fájl hello diagnosztika konfigurációs XML-kódban. A virtuális gép Azure Diagnostics engedélyezésével kapcsolatos további: [használja a Powershellt tooenable Azure Diagnostics Windows rendszerű virtuális gépként.](https://docs.microsoft.com/azure/virtual-machines/windows/ps-extensions-diagnostics)

hello Azure diagnostics bővítmény átviteli hello gyűjtött adatok tooan Azure storage-fiók, vagy küldje el, például az Application Insights tooservices. Használhatja a hello adatok naplózását.

#### <a name="how-do-i-store-and-view-diagnostic-data"></a>Hogyan tárolja és diagnosztikai adatainak megtekintéséhez?

Fontos tooremember, hogy diagnosztikai adatok nem véglegesen tárolja kivéve, ha azt át toohello a Microsoft Azure storage emulator vagy tooAzure storage. az Azure Storage toostore és nézet diagnosztikai adatok kövesse az alábbi lépéseket:

1. Adjon meg egy tárfiókot hello ServiceConfiguration.cscfg fájlban. Az Azure Diagnostics hello Blob szolgáltatás vagy hello Table szolgáltatás, attól függően, hogy hello típusú adatok is használhatja. Windows-eseménynaplók tábla formában tároljuk.

2. Hello adatátvitelt. Hello konfigurációs fájl segítségével tootransfer hello diagnosztikai adatokat kérhet. Az SDK 2.4 és az előző programozott módon is tehet hello kérelem.

3. Hello adatok megjelenítése, használatával [Azure Tártallózó](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer), [Server Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) a Visual Studio vagy [Azure Diagnostics Manager](https://www.cerebrata.com/products/azure-diagnostics-manager) az Azure Management Studio.

További információt a tooperform egyes lépések, tekintse meg [tároló és a nézet diagnosztikai adatokat az Azure Storage.](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)

### <a name="azure-storage-analytics"></a>Azure Storage Analytics 

Tárolási analitika sikeres és sikertelen kérelmek tooa storage szolgáltatással kapcsolatos részletes információkat naplózza. Ez az információ használt toomonitor egyes kérelmeket, amelyek segítik a hello szolgáltatásban tárolt hozzáférés toopersonal adatok dokumentálásáért lehet. Azonban tárolási analitika naplózás nem alapértelmezés szerint engedélyezve van a tárfiók. Az Azure-portálon hello engedélyezheti azt.

#### <a name="how-do-i-configure-monitoring-for-a-storage-account"></a>Hogyan konfigurálhatók a tárfiók figyelését?

tooconfigure figyelését a tárfiók hello a következő:

1. Válassza ki **tárfiókok** a hello Azure-portálon, majd válassza ki hello hello fióknevet, amelyet a toomonitor.

    ![](media/protection-personal-data-azure-reporting-tools/image011.png)

2. A hello **figyelés** szakaszban jelölje be **diagnosztika.**

3.  Jelölje be hello **típus** metrikai adatok kívánt toomonitor az egyes szolgáltatásokhoz (Blob, Table, fájl). tooinstruct Azure Storage toosave diagnosztikai naplókat a rekordhoz olvasási, írási és törlési kérelmek hello blob, table és queue szolgáltatások, válasszon ki **naplók Blob, Table naplók** és **naplók várólistára.**

    ![](media/protection-personal-data-azure-reporting-tools/image013.png)

4. Hello csúszkát hello lap alján, állítsa be hello **megőrzési** házirend napban (1 – 365 érték). Hét nap hello alapértelmezett beállítás.

5. Válassza ki **mentése** tooapply hello konfigurációs beállításokat.

Tárolási naplózási naplóbejegyzések hello következő egyes kérelmekkel kapcsolatos információ tartalmazza:

- Például a kezdési idő, a végpontok közötti késés és a kiszolgáló késleltetés információk.

- Például hello művelet szerint hello művelet részletek hello hello tárolási objektum hello ügyfél kulcsa elérése sikeres vagy sikertelen és hello toohello ügyfél visszaadott HTTP-állapotkód.

- Például a hitelesítési hello ügyfél használt hello típusú hitelesítési adatokat.

- Feldolgozási adatok például hello ETag érték és az utolsó módosítás időbélyegző.

- a kérelem-válasz köszönőüzenetei hello méretét.

Részletes útmutatást tooenable tárolási analitika naplózás, lásd: [a tárfiók hello Azure-portálon a figyelheti.](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)

### <a name="azure-security-center"></a>Azure Security Center 

[Az Azure Security Center](https://azure.microsoft.com/services/security-center/) figyelők rendelés tooprevent az Azure-erőforrások biztonsági állapotát hello észleli a veszélyeket és ajánlások válaszol. Számos módon toohelp dokumentum biztosít a biztonsági intézkedéseket hello adatvédelmi személyes adatok védelmét.

Biztonsági állapotfigyelés segít a biztonsági házirendek betartását. Proaktív stratégiát jelent, amely a vállalati szabványoknak vagy ajánlott eljárásoknak eleget nem tevő erőforrások tooidentify rendszerek naplózás biztonsági figyelés. A következő erőforrások hello hello biztonsági állapota figyelhető meg:

- Számítási (virtuális gépek és felhőszolgáltatások)

- Hálózatkezelés (virtuális hálózatok)

- Tárolás és adatokat (a kiszolgáló és az adatbázis naplózásának és a fenyegetések észlelésére, TDE, tárolás titkosítása)

- Alkalmazások (potenciális biztonsági problémákat)

E kategóriák közül egyik sem biztonsági problémák jelenthetnek a fenyegetés toohello adatvédelmi személyes adatok.

#### <a name="how-do-i-view-hello-security-state-of-my-azure-resources"></a>Hogyan tekinthetem meg az Azure-erőforrások biztonsági állapotának hello?

A Security Center rendszeresen elemzi az Azure-erőforrások biztonsági állapotának hello. Megtekintheti a potenciális biztonsági hiányosságok azonosítja a hello **megelőzési** hello irányítópult szakasza.

   ![](media/protection-personal-data-azure-reporting-tools/image014.png)

1. A hello **megelőzési** szakaszban, jelölje be hello **számítási** csempére. Itt láthatja az **áttekintése,** együtt hello **virtuális gépek** listája az összes virtuális gépek és azok biztonsági állapota, és hello **a felhőalapú szolgáltatások** webes és feldolgozói szerepkörök listája a Security Center által figyelt.

2. A hello **áttekintése** lapon, a második egy javaslat tooview további információt.

3. A hello **virtuális gépek** lapra, válassza ki a virtuális gép tooview további részleteket.

Ha adatok gyűjtése az Azure Security Center engedélyezve van, automatikusan létrehozza a Microsoft Monitoring Agent hello rendszer összes meglévő, és új támogatott telepített virtuális gépek. Ez az ügynök gyűjtött adatokat tárolja vagy egy meglévő [Naplóelemzési](https://azure.microsoft.com/services/log-analytics/) az előfizetés vagy új munkaterület társított munkaterület.

[A fenyegetés Eszközintelligencia-jelentések](https://docs.microsoft.com/azure/security-center/security-center-threat-report) Security Center által biztosított. Ezek segítenek toohelp megfejteni a behatolók identitás, a célok, a jelenlegi és korábbi támadás kampányokat és taktikai, eszközök és a használt eljárásokat hello támadó hasznos információkat. Megoldás és a szervizelési információk is megtalálható.

hello elsődleges fenyegetés jelentésekkel célja toohelp meg toorespond hatékonyan toohello azonnali fenyegetés és súgó hajtsa végre a megfelelő intézkedéseket ezt követően toomitigate hello probléma. hello jelentésekben hello információkat is nagyban, a dokumentum az incidensekre adott reakciók, a jelentéskészítési és naplózási célokra.

hello fenyegetés Eszközintelligencia-jelentések jelenjenek meg. PDF formátumú hello tartalmaz egy hivatkozást keresztül elért **jelentések** hello mezőjében **gyanús folyamat végrehajtása** az egyes az Azure Security Center biztonsági riasztások panel.

Hogyan tooview és -felhasználási hello fenyegetés az Eszközintelligencia-jelentés további információkért lásd: [Azure Security Center fenyegetés Eszközintelligencia jelentés.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

## <a name="next-steps"></a>További lépések:

[Ismerkedés az Azure Active Directory reporting API-val hello](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

[Mi az a Log Analytics?](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

[A Microsoft Azure Figyelés szolgáltatásának áttekintése](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

[Bevezetés toohello Azure tevékenységnapló (videó)](https://azure.microsoft.com/resources/videos/intro-activity-log/)
