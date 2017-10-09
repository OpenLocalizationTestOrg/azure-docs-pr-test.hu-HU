---
title: "BizTalk szolgáltatások tooAzure Logic Apps alkalmazások aaaMove |} Microsoft Docs"
description: "Helyezze át vagy Azure BizTalk szolgáltatások MABS tooLogic alkalmazások áttelepítése"
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: ladocs; jonfan; mandia
ms.openlocfilehash: b3b065b90a37002f72305b0fc866c24231fb5f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-from-biztalk-services-toologic-apps"></a>BizTalk szolgáltatások tooLogic alkalmazások mozgatása

A Microsoft Azure BizTalk szolgáltatások (MABS) kivonás alatt áll. Ez a témakör toomove MABS integrációs megoldásokat tooAzure Logic Apps használja. 

## <a name="overview"></a>Áttekintés

BizTalk szolgáltatások két alárendelt szolgáltatásokból állnak:

1.  Microsoft BizTalk szolgáltatások hibrid kapcsolatok
2.  EAI- és EDI híd-alapú integrációs

Ha toomove hibrid kapcsolatok, majd [Azure App Service hibrid kapcsolatok](../app-service/app-service-hybrid-connections.md) hello módosításokat, és ez a szolgáltatás funkcióit ismerteti. Az Azure hibrid kapcsolatok BizTalk szolgáltatások hibrid kapcsolatok váltja fel. Az Azure hibrid kapcsolatok érhető el az Azure App Service, és szereplő hello Azure-portálon. Azure hibrid kapcsolatok is biztosít a meglévő BizTalk szolgáltatások hibrid kapcsolatok új Hybrid Connection Manager toomanage, és létrehoz az új hibrid kapcsolatok hello portál. Az Azure App Service hibrid kapcsolatok általánosan elérhető (GA).

EAI- és EDI híd-alapú integrációs a Logic Apps szolgáltatás hello helyettesíti. A Logic Apps biztosít minden hello BizTalk szolgáltatások, és több mint ugyanazokat a képességeket. A Logic Apps felhőméretű fogyasztás alapján munkafolyamat és a vezénylési olyan funkciókat biztosít, amelyek lehetővé teszik a tooquickly és könnyen felépítése az összetett integrációs megoldásokat böngésző használatával, vagy a Visual Studio eszközök segítségével.

a következő táblázat hello biztosít BizTalk szolgáltatások képességek leképezéseket tooLogic alkalmazásokat.

| BizTalk Services   | Logic Apps            | Cél                  |
| ------------------ | --------------------- | ---------------------------- |
| összekötő          | összekötő             | Adatok küldésére és fogadására   |
| Híd             | Logikai alkalmazás             | Feldolgozási sor processzor           |
| Szakasz ellenőrzése     | XML-érvényesítés művelet      | Az XML-dokumentum, a séma érvényesítése             |
| Szakasz kiegészítése       | Adatok jogkivonatok      | Tulajdonságok előléptetni üzenetbe vagy a útválasztási döntések             |
| Átalakítás szakasz    | Átalakítási művelet      | XML-üzenetek konvertálása egy formátummal tooanother             |
| Dekódolás szakasz       | Egybesimított fájl dekódolási művelet      | Egybesimított fájl tooXML konvertálása             |
| Szakasz kódolása       |  Egybesimított fájl kódolása művelet      | XML-tooflat fájl konvertálása             |
| Üzenet Inspector       |  Az Azure Functions vagy API-alkalmazások      | Futtassa a egyéni kódot a integrációja             |
| Útvonal-művelet      |  Az állapot vagy a kapcsoló      | Útvonal üzenetek tooone hello a megadott összekötők             |

Számos különböző típusú BizTalk Services összetevő.

## <a name="connectors"></a>Összekötők
BizTalk szolgáltatások összekötők hidak toosend engedélyezése és fogadhat adatokat, többek között az engedélyezett HTTP-alapú kérelem/válasz kapcsolati kétirányú hidak. A Logic Apps, azonos terminológia hello. Összekötők logic Apps alkalmazások kiszolgálására hello azonos célját, és több mint 140, csatlakoztatható technológiák és szolgáltatások széles körű tömbje tooa is tartalmaznak, mind a helyszíni használatával hello helyszíni Data Gateway (cseréje hello BizTalk szolgáltatás BizTalk szolgáltatás által használt), és a Szolgáltatottszoftver- és PaaS felhőszolgáltatásokhoz, mint például a onedrive-on, Office365, Dynamics CRM és sok más.

BizTalk szolgáltatások forrásai korlátozott tooFTP, SFTP, és a Service Bus-üzenetsorba vagy Üzenettémakör-előfizetésben.

![](media/logic-apps-move-from-mabs/sources.png)

Minden egyes híd HTTP-végponttal rendelkezik alapértelmezés szerint hello futásidejű cím és hello híd tulajdonságairól hello relatív címet kell konfigurálni. tooachieve hello ugyanaz a Logic Apps, használjon hello [kérelem-válasz](../connectors/connectors-native-reqres.md) műveletek.

## <a name="xml-processing-and-bridges"></a>XML-feldolgozás és a hidak
BizTalk szolgáltatások hidat hasonló tooa folyamat. Hidat is igénybe vehet egy összekötő érkező adatokat, és néhány hello adatokkal dolgozni, és küldje el tooanother rendszer. A Logic Apps azonos hello hello azonos csővezeték-alapú kommunikáció BizTalk szolgáltatásként mintákra és az egyéb integrációs minták számos támogatásával. Hello [XML-kérelem-válasz híd](https://msdn.microsoft.com/library/azure/hh689781.aspx) a BizTalk szolgáltatások nevezik, amelyek szakaszból álló VETER folyamat:

* (V) érvényesítése
* E kiegészítése
* (T) átalakítás
* E kiegészítése
* (R) útvonal

Látható kép a következő hello, hello feldolgozási helykérelemmel és válasszal elosztva, és lehetővé teszi, hogy a hello kérelem és hello válasz elérési utak külön-külön (például az egyes különböző megtekintését):

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Emellett egyirányú híd XML ad hozzá dekódolási és Encode szakaszában hello elején és végén lévő feldolgozási, és hello áteresztő híd tartalmaz egyetlen Enrich szintre.

### <a name="message-processing-and-decodingencoding"></a>Üzenet feldolgozásához, és dekódolási/kódolás
BizTalk szolgáltatások a különböző XML-üzeneteket fogadni, és határozza meg a megfelelő sémáját hello hello üzenet érkezett. Ez történik, hello **üzenettípusok** hello szakasza kap feldolgozásához. Majd hello dekódolási szakasz ezt használja a észlelt hello üzenet típusa toodecode megadott hello sémáját használja. Ha hello séma flatfile a séma, hello bejövő flatfile tooXML alakítja át. 

A Logic Apps hasonló funkciókat biztosít. Egy flatfile különböző protokollok hello különböző összekötő eseményindítókat (File System, FTP, HTTP és így tovább) segítségével számos protokollal fogadjanak, és használja a hello [Egybesimított fájl dekódolása](../logic-apps/logic-apps-enterprise-integration-flatfile.md) művelet tooconvert hello bejövő adatok tooXML. Áthelyezheti a létező fájlszintű sémák közvetlen toologic alkalmazások anélkül, hogy bármelyik megváltozik, és majd feltölteni a sémák tooyour integrációs fiók.

### <a name="validation"></a>Ellenőrzés
Miután hello bejövő adatok konvertált tooXML (vagy ha XML-kódja hello üzenetformátum érkezett), az érvényesítési fut, ha üdvözlőüzenetére megfelelő tooyour XSD-séma toodetermine. toodo ezt a Logic Apps, használjon hello [XML-érvényesítés](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) művelet. Ebben az esetben használhatja a BizTalk szolgáltatások módosítások nélkül azonos sémák hello.

### <a name="transform-messages"></a>Átalakítás üzenetek
A BizTalk szolgáltatások hello átalakítási szakasz egy XML-alapú üzenet formátuma tooanother alakítja át. Ez a térkép hello TRFM alapú leképező használatával alkalmazásával történik. A Logic Apps hello folyamat hasonlít. hello átalakítási műveletet hajtja végre a térkép integrációs fiókjából. hello fő különbség, hogy a Logic Apps maps XSLT-formátumban vannak-e. XSLT hello képességét tooreuse XSLT már, beleértve a maps létre BizTalk Server functoids tartalmazó meglévő tartalmazza. 

### <a name="routing-rules"></a>Útválasztási szabályokat
BizTalk szolgáltatások döntés teszi végpont/összekötő toosend bejövő üzenetek/adatokról. hello képességét tooselect előre konfigurált végpontok alternatíva a következő lehetséges hello útválasztási szűrő beállítás használatával:

![](media/logic-apps-move-from-mabs/route-filter.png)

A Logic Apps bonyolultabb logikát képességeket biztosít a [feltétel](../logic-apps/logic-apps-use-logic-app-features.md) és [kapcsoló](../logic-apps/logic-apps-switch-case.md), speciális folyamatábrán engedélyezése és útválasztást. BizTalk szolgáltatások útválasztási szűrők konvertálása a legkönnyebben használatával egy **feltétel** *Ha* csak két lehetőség. Ha több mint két, majd használja a **kapcsoló**.

### <a name="enrich"></a>Kiegészítése
hello feldolgozása BizTalk szolgáltatások Enrich szakasza hello fogadott adatok társított tulajdonságok toohello üzenet környezetének hozzáadja. Például előléptetni egy tulajdonság toouse útválasztás (alább) az adatbázis-lekérdezés, illetve XPath kifejezés segítségével érték beolvasása. A Logic Apps biztosít hozzáférést tooall környezetfüggő adatok kimenetek a fenti műveletek, így egyszerű tooreplicate hello kívánt viselkedést eredményező beállítást. Hello használata esetén például `Get Row` SQL kapcsolat művelet, akkor adja vissza egy SQL Server-adatbázis adatait, és használjon hello adatokat irányításához döntési művelettel. Hasonlóképpen, a bejövő Service Buson tulajdonságok várólistára helyezett üzenetek egy eseményindító által megcímezhető, valamint XPath hello xpath munkafolyamat definition language kifejezés használatával.

### <a name="use-custom-code"></a>Egyéni kód használata
BizTalk szolgáltatások túl hello lehetőséget nyújt[egyéni kódra](https://msdn.microsoft.com/library/azure/dn232389.aspx) saját szerelvényekben feltöltve. Ez hello megvalósítja [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx) felületet. Minden szakaszhoz hello Bridge két tulajdonságok (adja meg Inspector a és a kilépési Inspector) adja meg a hello .net típusát hozott létre, amely megvalósítja ezt a felületet tartalmazza. Egyéni kód lehetővé teszi a tooperform összetettebb feldolgozás esetén a hello adatok, valamint a meglévő kód újbóli általános üzleti logika végző szerelvényeket. 

A Logic Apps két elsődleges lehetőséget biztosít az egyéni kód tooexecute: az Azure Functions és API-alkalmazások. Az Azure Functions hozható létre, és a logic Apps alkalmazásokból nevezik. Lásd: [hozzáadása és az Azure Functions használatával logic Apps-alkalmazások futtatása egyéni kód](../logic-apps/logic-apps-azure-functions.md). Az API appst, Azure App Service-ben toocreate része a saját eseményindítók és műveletek. További információ [egy egyéni API toouse létrehozása a Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Ha egyéni kód, amely meghívja a BizTalk szolgáltatásokból assmeblies van, vagy áthelyezheti a code tooAzure funkciók, vagy hozzon létre egyéni API-kat API-alkalmazások; attól függően, hogy mi meg megvalósításához. Például ha kódot, amely egy másik szolgáltatást, hogy a Logic Apps nem rendelkezik egy összekötő becsomagolja, majd API-alkalmazás létrehozása, és az API-alkalmazás biztosít a logikai alkalmazásban hello műveleteket használni. Ha súgófunkciókat vagy könyvtárak, az Azure Functions valószínűleg hello legmegfelelőbb.

### <a name="edi-processing-and-trading-partner-management"></a>EDI dolgoz fel, és kereskedelmipartner-kezelés
BizTalk szolgáltatások részét képező EDI és B2B feldolgozási AS2-támogatással rendelkező (alkalmazhatósági utasítás 2), X12 és EDIFACT. Ezáltal a Logic Apps. BizTalk szolgáltatások a EDI hidak létrehozása és létrehozása vagy kezelése üzleti partnerek és megállapodások hello dedikált nyomon követését és a felügyeleti portálon.

A Logic Apps, ez a funkció megtalálható hello [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md). Ez több hello integrációs fiók és a B2B műveletek EDI és B2B feldolgozásra. Hello [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) használt toocreate és kezelése [kereskedelmi partnerek](../logic-apps/logic-apps-enterprise-integration-partners.md) és [megállapodások](../logic-apps/logic-apps-enterprise-integration-agreements.md). Miután létrehozott egy integrációs fiókkal, egy vagy több logic apps toohello fiók lehet társítani. Miután hozzárendelt, hello B2B műveletek tooaccess kereskedelmi partneradatok a logikai alkalmazásban is használhatja. a következő műveletek hello itt találhatók:

* AS2-kódolása
* AS2 dekódolása
* X12 kódolása
* X12 dekódolása
* EDIFACT kódolása
* EDIFACT dekódolása

BizTalk szolgáltatások eltérően ezeket a műveleteket vannak különválik hello átviteli protokollt. Ezért a logic apps létrehozásakor több beleszólása van a mely összekötőket toosend használ, és adatok fogadására. Például ez lehetséges tooreceive X12 fájlok e-mail mellékletként, és ezeket a fájlokat, a logikai alkalmazás majd feldolgozni. 

## <a name="manage-and-monitor"></a>Kezelése és figyelése
Kereskedelmipartner-kezelés, valamint hello dedikált portal BizTalk szolgáltatások biztosított képességek toomonitor nyomon követése és a problémák megoldását. 

A Logic Apps nyújt részletesebb nyomon követése és figyelési képességek a hello [Azure-portálon](../logic-apps/logic-apps-monitor-your-logic-apps.md), és a hello [Operations Management Suite B2B megoldás](../logic-apps/logic-apps-monitor-b2b-message.md); beleértve egy mobilalkalmazást követheti a dolgok tartása Ha a hello helyezze át.

## <a name="high-availability"></a>Magas rendelkezésre állás
tooachieve magas rendelkezésre ÁLLÁS a BizTalk szolgáltatások, egynél több példányát használja egy adott régió tooshare hello adatfeldolgozás miatti terhelést a. A logic apps-régió magas rendelkezésre ÁLLÁSÚ beépített, tesztelhet, és minden további költség nélkül. Ki a régiót vész-helyreállítási BizTalk szolgáltatások B2B feldolgozásához a biztonsági mentési és visszaállítási folyamat szükség. A Logic Apps, a kereszt-régió aktív/passzív [vész-Helyreállítási képességet](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) biztosított; a B2B hello szinkronizálását lehetővé teszi az üzletmenet folytonossága érdekében különböző régiókban integrációs fiókok között.

## <a name="next"></a>Következő lépés
* [Mi az a Logic Apps?](logic-apps-what-are-logic-apps.md)
* [Az első logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md), vagy a használat gyors megkezdése [előre elkészített sablonokkal](logic-apps-use-logic-app-templates.md)  
* [A nézet az összes rendelkezésre álló összekötők hello](../connectors/apis-list.md) is használhatja a logikai alkalmazás
