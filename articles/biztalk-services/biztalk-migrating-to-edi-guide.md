---
title: "aaaMigrating BizTalk Server EDI megoldások tooBizTalk szolgáltatások műszaki útmutatója |} Microsoft Docs"
description: "EDI tooMABS; áttelepítése A Microsoft Azure BizTalk szolgáltatások"
services: biztalk-services
documentationcenter: na
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 61c179fa-3f37-495b-8016-dee7474fd3a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 34cca3c939a6a7845a860ead6858287000d03ee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-biztalk-server-edi-solutions-toobiztalk-services-technical-guide"></a>BizTalk Server EDI megoldások tooBizTalk áttelepítése szolgáltatások: műszaki útmutatója

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Szerző: Tim Wieman és Nitin Mehrotra

A felülvizsgálók: Karthik Bharthy

Használatával készítettek: Microsoft Azure BizTalk szolgáltatások – 2014. február kiadási.

## <a name="introduction"></a>Bevezetés
Elektronikus adatcsere (EDI) az egyik hello legjellemzőbb eszköz, amellyel vállalatok exchange-adatok elektronikus úton, vállalatok vagy B2B tranzakcióként is hívják. BizTalk Server volt egy évtizedben hello kezdeti BizTalk Server kiadása óta keresztül támogatása EDI-e. BizTalk szolgáltatások Microsoft hello támogatása EDI-megoldások hello a Microsoft Azure platformon továbbra is. B2B tranzakciók többnyire külső tooan szervezet, és ha megtörtént a felhő platform ezért is könnyebben tooimplement. A Microsoft Azure lehetővé teszi a BizTalk szolgáltatások segítségével.

Egyes ügyfelek BizTalk szolgáltatások új EDI-megoldások "greenfield" platformként tekinti meg, míg számos ügyfél rendelkezik, toomigrate tooAzure felmerülhet aktuális BizTalk Server EDI-megoldások. Mivel BizTalk szolgáltatások EDI felépített alapján hello ugyanaz a kulcs entitásokat, a BizTalk Server EDI architektúra (kereskedelmi a partnerek, entitásokat, szerződések), célszerű a lehető toomigrate BizTalk Server EDI összetevők tooBizTalk szolgáltatások.

Ez a dokumentum ismerteti a BizTalk Server EDI-összetevők áttelepítése tooBizTalk szerepet játszó hello különbségek némelyike szolgáltatások. Ez a dokumentum a BizTalk Server EDI-feldolgozás és Kereskedelmipartner-egyezmények ismeretét feltételezi. BizTalk Server EDI további információkért lásd: [Kereskedelmipartner-kezelés használatával BizTalk partnerkiszolgáló](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-toobiztalk-services"></a>BizTalk Server EDI-összetevők verziójának lehet át tooBizTalk szolgáltatások?
hello BizTalk Server EDI modul jelentősen bővült a BizTalk Server 2010, amikor újra modellezett tooinclude partnerek, profilok és megállapodások volt. BizTalk szolgáltatások által használt hello ugyanazon modell tooorganize hello kereskedelmi partnerek, és azok kereskedelmi partnerek üzleti osztályai hello. Ennek eredményeképpen EDI áttelepítése a BizTalk Server 2010 és újabb verziók tooBizTalk lévő összetevők szolgáltatásokat, sokkal több egyszerű folyamat. toomigrate EDI-összetevők verziója előzetes tooBizTalk Server 2010, a társított először frissítenie kell tooBizTalk Server 2010, majd telepítse át a EDI-összetevők tooBizTalk szolgáltatások.

## <a name="scenariosmessage-flow"></a>Forgatókönyv/üzenet folyamata
Mint a BizTalk Server EDI-feldolgozás alatt álló BizTalk szolgáltatások épül kereskedelmi Partner (TPM) megoldás. hello TPM-megoldás kulcsfontosságú összetevők a következő hello rendelkezik:

* Kereskedelmi partnereknek, amelyek szervezetet képviseljék a B2B tranzakció.
* Profilok, amelyek megfelelnek a kereskedelmi partner osztályai.
* Kereskedelmipartner-egyezmények (vagy szerződések), amely képviseli hello üzleti szerződés között két partnerek/profil.

a következő ábra hello hello Hasonlóságok, valamint a BizTalk Server EDI-megoldás és a BizTalk szolgáltatások EDI-megoldás közötti különbségek ábrázol:

![][EDImessageflow]

hello fontosabb különbségeket, és egy EDI megoldás folyamata a BizTalk Server Hasonlóságok, és a BizTalk szolgáltatások a következő:

* BizTalk Server használ egy EDIReceive csővezeték tooreceive, EDI-üzenetet, és egy EDISend csővezeték toosend EDI-üzenetet, ahogy a BizTalk szolgáltatások egy híd tooreceive EDI fogadni és a híd toosend EDI küldése EDI-üzenetek használja. BizTalk Server hello adatcsatornákat társított szerződés küldési használatával, és portok fogadására. A BizTalk szolgáltatások hello megállapodás hello küldési jelöli, vagy híd fogadására.
* BizTalk Server hello EDIReceive csővezeték folyamatok hello EDI-üzenetet, miután hello üzenet dömpingelt tooa SQL Server-adatbázist. hello EdiSend csővezeték majd szerzi be üdvözlőüzenetére hello SQL Server-adatbázisból, folyamatokat engedélyez, és elküldi a toohello kereskedelmi partner.
  
    A BizTalk szolgáltatások után hello EDI üzenet híd folyamatok hello EDI, irányítja a hello üzenet tooan külső folyamat. a Microsoft Azure vagy a helyszíni hello külső folyamat sikerült futnak. hello külső folyamat hello üzenet toohello EDI küldése híd; kell továbbítani. hello küldési híd eredendően nem lekéréses üdvözlőüzenetére. Feldolgozási üdvözlőüzenetére, miután EDI küldése híd hello hello üzenet toohello kereskedelmi partner irányítja.

BizTalk szolgáltatások biztosít egy könnyen használható konfigurációs élményt tooquickly létrehozása és központi telepítése egy kereskedelmi partnerek nélkül konfigurálása a Microsoft Azure számítás-példányok (webes vagy feldolgozói szerepkörök), a Microsoft Azure SQL-adatbázisok vagy bármelyik B2B megállapodást A Microsoft Azure storage-fiókok. Összetettebb forgatókönyveket szükséges a munkafolyamatok vagy más szolgáltatás-feldolgozás Ön "körül hello széleit" egy Kereskedelmipartner-egyezmény, ez azt jelenti, hogy előtt vagy után kereskedelmi Partner megállapodás EDI híd feldolgozása. Részletes hello következő eseménysorozat elő egy BizTalk szolgáltatások EDI üzenetfeldolgozás során.

1. EDI-üzenetet kapott kereskedelmi partner, a Fabrikam.  BizTalk szolgáltatások támogatja például FTP SFTP, AS2 vagy HTTP/s átviteli protokollt EDI-üzenetek fogadása kereskedelmi partnereknek,
2. kereskedelmipartner-partner megállapodás fogadóoldali feldolgozás hello hello EDI üzenetformátum tooXML visszafejti.  Szét hello EDI (XML formátumban) üzenet tooService Bus-végpontokat például egy Service Bus Relay végpont, a Service Bus-témakörbe, a Service Bus-üzenetsorba vagy a BizTalk szolgáltatások hidat irányíthatja.
3. hello szétszerelt XML sikerült majd kell érkező üzenetek hello végpont további egyéni feldolgozásra.  Ezeket a végpontokat tudta azt feldolgozni egy helyszíni összetevő vagy egy Microsoft Azure számítási példány toofurther folyamat hello üzenet egy Windows-munkafolyamat (WF) vagy a Windows Communication Foundation (WCF) szolgáltatást, például.
4. hello "küldési-oldalon feldolgozása" hello kereskedelmipartner-egyezmény majd állítja össze a üdvözlőüzenetére XML EDI-formátumba, és elküldi tootrading partner, Contoso.  BizTalk szolgáltatások üzenetküldésre EDI tootrading partnerek, támogatja a azonos EDI-üzenetek fogadására használt protokollok hello.

Ez a dokumentum további útmutatást fogalmi áttelepítése néhány hello különböző BizTalk Server EDI összetevők tooBizTalk szolgáltatások.

## <a name="sendreceive-ports-tootrading-partners"></a>Küldés/fogadás portok tooTrading partnerek
BizTalk Server beállítása kap helyek és a fogadási portok tooreceive EDI/XML-üzenetek kereskedelmi partnereknek a, és küldési portok toosend EDI/XML üzenetek tootrading partner beállítása. Majd lefoglalják a portok tooa kereskedelmipartner-egyezmény hello BizTalk Server felügyeleti konzol használatával. A BizTalk szolgáltatások hello helyek, ahol megjelenik az üzenetek kereskedelmi partnereknek és ahol küldése a üzenetek tootrading partnerek hello kereskedelmipartner-egyezmény magát (az átviteli beállítások részeként) részeként van konfigurálva a hello BizTalk szolgáltatások portálja .  Ezért valóban nincs "küldési portok" és "fogadásához helyek" hello fogalmát önmagában, a BizTalk szolgáltatások. További információkért lásd: [létrehozása megállapodások](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Folyamatok (hidak)
BizTalk Server EDI, a folyamatok üzenet feldolgozása entitások egyedi feldolgozása képességeinek hello alkalmazás szükség szerint egyéni logika is használható. BizTalk szolgáltatások hello egyenértékű egy EDI híd lenne. Azonban a BizTalk szolgáltatások most hello EDI hidak "bezárul".  Ez azt jelenti, hogy a saját egyéni tevékenységek tooan EDI híd nem vehető fel. Minden egyéni feldolgozási előtt vagy után üdvözlőüzenetére kerül hello híd hello Kereskedelmipartner-egyezmény tagja hello EDI híd az alkalmazás kívülről kell végrehajtani. EAI-Összekötők hidak hello toodo egyéni feldolgozása rendelkezik. Ha azt szeretné, hogy egyéni feldolgozási, használhatja EAI-Összekötők hidak, előtt vagy után üdvözlőüzenetére hello EDI híd dolgoz fel. További információkért lásd: [hogyan tooInclude hidak kódot egyéni](https://msdn.microsoft.com/library/azure/dn232389.aspx).

A közzététel/előfizetés áramlását egyéni kód és/vagy a Service Bus üzenetkezelés üzenetsorok és témakörök előtt kereskedelmipartner-egyezmény hello hello üzenetet kap, és után hello megállapodás üdvözlőüzenetére feldolgozza, és továbbítja azt tooa Service Bus-végpont használatával is beszúrhat.

Lásd: **forgatókönyvek/üzenet folyamat** hello üzenet folyamata minta ebben a témakörben.

## <a name="agreements"></a>Szerződés
Ha ismeri a BizTalk Server 2010 Trading Partner megállapodások használt feldolgozása EDI hello, BizTalk szolgáltatások kereskedelmipartner-egyezmények keresés tisztában. A legtöbb beállítások vannak hello azonos hello szerződés és -felhasználási hello ugyanazokat a kifejezéseket. Bizonyos esetekben hello megállapodás beállítások vannak sokkal egyszerűbb összehasonlított toohello a BizTalk Server ugyanazokat a beállításokat. A Microsoft Azure BizTalk szolgáltatások által támogatott X12, EDIFACT és AS2 átviteli.

A Microsoft Azure BizTalk szolgáltatásokat is biztosít egy **TPM adatáttelepítés** toomigrate üzleti partnerek és a BizTalk Server kereskedelmi Partner modul tooBizTalk szolgáltatások portálja megállapodások eszköz. hello TPM adatáttelepítési eszköz érhető el az eszközök csomag részeként, amely letölthető hello [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). hello csomag is tartalmaz egy fontos, hogy hogyan toouse hello eszközt, és alapvető információkat hello eszközzel kapcsolatos utasításokat tartalmazza.

## <a name="schemas"></a>Sémák
BizTalk szolgáltatások EDI-sémák, amely használható a BizTalk szolgáltatások megoldások biztosít.  Ezenkívül BizTalk Server EDI-sémák is használható a BizTalk szolgáltatások mert hello gyökércsomópont hello EDI séma legyen, a BizTalk Serveren, valamint a BizTalk szolgáltatások között. Így fog kell tudni toodirectly hajtsa végre a megfelelő a BizTalk Server EDI-sémák, és használja őket hello EDI megoldások, amelyek a most kialakított BizTalk szolgáltatások használatával. Is letölthetők hello sémák hello [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>A Maps (átalakítások)
BizTalk Server Maps a BizTalk szolgáltatások átalakítások nevezik. BizTalk Server tooBizTalk szolgáltatások valamelyike lehet áttelepítése térképeit hello összetettebb feladatok tooachieve (attól függően, hogy térkép összetettsége). BizTalk szolgáltatások használható hello leképezési eszköz hello BizTalk leképező eltér. Annak ellenére, hogy hello leképező keres többnyire hello azonos, hello alapul szolgáló map formátum nem egyezik. hello functoids (nevű **térkép műveletek** a BizTalk szolgáltatások), valamint különböző elérhető toohello felhasználók.  Érvényben nem közvetlenül használható BizTalk térképre BizTalk szolgáltatások. Nem minden hello functoids BizTalk Server rendszerben elérhető is elérhető BizTalk szolgáltatások térkép műveletei.

### <a name="new-transform-operations"></a>Új átalakítási műveletek
Tűnhet, hogy igen különböző hello BizTalk Server leképező hello listája átalakítási térkép műveletek érhető el, amíg új módokat parancsokról ugyanazokhoz a feladatokhoz hello BizTalk szolgáltatások átalakítja rendelkezik. Például a BizTalk szolgáltatások átalakítja rendelkezik **listázási műveletei** érhető el. Ez nem volt elérhető hello BizTalk leképező.  Hello **listázási műveletei** lehetővé teszik a toocreate és működik a "List", ha a lista egy olyan elemek (más néven "sort"), és ha egyes elemek rendelkezhet több tagot (más néven "oszlop").  Rendezheti hello listájában válassza cikkek alapján egy feltétel, stb.

Egy másik példa a BizTalk szolgáltatások átalakítja az új funkciók hello **hurok műveletek**.  BizTalk Server leképező hello nehéz toocreate beágyazott hurkok.  BizTalk szolgáltatások átalakítja hello így hello hurok térkép műveletek hozzáadott.

Még egy másik hello: **Ha-akkor más** kifejezés térkép művelete.  Mert az if-majd más műveletet végez korábban az hello BizTalk leképező, de szükség több functoids tooaccomplish látszólag egyszerű feladat.

### <a name="migrating-biztalk-server-maps"></a>A Maps BizTalk-kiszolgáló áttelepítése
A Microsoft Azure BizTalk szolgáltatások biztosítja, hogy egy eszköz toomigrate BizTalk Server tooBizTalk szolgáltatások átalakítások képezi le. Hello **BTMMigrationTool** elérhető hello részeként **eszközök** hello megadott csomag [BizTalk szolgáltatások SDK letöltési](http://go.microsoft.com/fwlink/p/?LinkId=235057). Hello eszközzel kapcsolatos további információkért lásd: [alakítsa át a BizTalk térkép tooa BizTalk szolgáltatások átalakítási](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Is megtekintheti egy minta Sandro Pereira, BizTalk MVP, amelyet a hogyan túl[telepítse át a BizTalk Server maps tooBizTalk szolgáltatások átalakítások](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Álló üzenettípusok összehangolását
BizTalk Server vezénylési feldolgozási tooMicrosoft Azure toomigrate van szüksége, ha hello álló üzenettípusok összehangolását kellene írni, mert a Microsoft Azure nem támogatja a futó BizTalk Server álló üzenettípusok összehangolását toobe.  A Windows Workflow Foundation 4.0 (WF4) szolgáltatásának hello vezénylési funkcióiról sikerült újraírási.  Egy teljes átdolgozás lenne, mert jelenleg nincs a BizTalk Serveren álló üzenettípusok összehangolását tooWF4 való áttelepítést. Íme néhány forrás, a Windows-munkafolyamat:

* [*Hogyan toointegrate egy WCF-munkafolyamat szolgáltatást a Service Bus-üzenetsorok és témakörök* ](https://msdn.microsoft.com/library/azure/hh709041.aspx) Paolo Salvatori által. 
* [*A Windows Workflow Foundation és az Azure-alkalmazások építése* munkamenet](http://go.microsoft.com/fwlink/p/?LinkId=237314) a Build 2011 hello konferencia.
* [*A Windows folyamatkövető alaprendszer fejlesztői központja* ](http://go.microsoft.com/fwlink/p/?LinkId=237315) az MSDN Webhelyén.
* [*Windows munkafolyamat Foundation 4 (WF4) dokumentációjában* ](https://msdn.microsoft.com/library/dd489441.aspx) az MSDN Webhelyén.

## <a name="other-considerations"></a>Egyéb szempontok
Az alábbiakban néhány olyan szempontot, amelyek meg kell nyitnia a BizTalk szolgáltatás használata során.

### <a name="fallback-agreements"></a>Tartalék megállapodások
BizTalk Server EDI feldolgozási rendelkezik "Tartalék megállapodások" hello fogalmát.  BizTalk szolgáltatások does **nem** egy tartalék megállapodás fogalom, amennyiben rendelkezik.  BizTalk dokumentáció témakörök [megállapodások szerepkör EDI feldolgozásakor hello](http://go.microsoft.com/fwlink/p/?LinkId=237317) és [globális konfigurálása vagy tartalék megállapodás tulajdonságok](https://msdn.microsoft.com/library/bb245981.aspx) tartalék megállapodások használata a BizTalk információk A kiszolgáló.

### <a name="routing-toomultiple-destinations"></a>Útválasztási toomultiple célok
BizTalk szolgáltatások hidak, a jelenlegi állapotában nem támogatja a közzététel segítségével a útválasztási üzenetek toomultiple célok-modell előfizetés. Ehelyett sikerült továbbítani a BizTalk szolgáltatások híd tooa Service Bus-témakörbe, amely majd több előfizetések tooreceive üdvözlőüzenetére egynél több végpont üzeneteit.

## <a name="conclusion"></a>Összegzés
A Microsoft Azure BizTalk szolgáltatások frissül, rendszeres mérföldkövek tooadd további funkciók és képességek. Minden egyes frissítés számítunk nőtt toosupporting funkció toofacilitate végpontok közötti megoldások BizTalk szolgáltatások és egyéb Azure technológiák segítségével.

## <a name="see-also"></a>Lásd még:
[Vállalati alkalmazások Azure programmal](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
