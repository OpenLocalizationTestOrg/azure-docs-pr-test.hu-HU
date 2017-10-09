---
title: "Azure BizTalk szolgáltatások aaaRelease megjegyzései |} Microsoft Docs"
description: "Hello Azure BizTalk szolgáltatások kapcsolatos ismert problémák listája"
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: f4906fdc-4cd9-4a57-a007-a88c2e51a18f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: ea53d6c40ed58badf4141453dc77d28dcfc6407f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-biztalk-services"></a>Kibocsátási megjegyzések a Azure BizTalk szolgáltatások

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

hello kibocsátási megjegyzések a hello Microsoft Azure BizTalk szolgáltatások ismert problémák ebben a kiadásban hello tartalmaz.

## <a name="whats-new-in-hello-november-update-of-biztalk-services"></a>What's new in hello November frissítés BizTalk szolgáltatások
* BizTalk szolgáltatások portálja hello titkosítását is engedélyezhető. Lásd: [engedélyezheti a titkosítást a BizTalk szolgáltatások portál aktívan](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Frissítési előzmények
### <a name="october-update"></a>. Októberi frissítés
* Munkahelyi és iskolai fiókok támogatottak:  
  * **A forgatókönyv**: regisztrálta a BizTalk szolgáltatás központi telepítése a Microsoft-fiókkal (például user@live.com). Ebben a forgatókönyvben csak a felhasználók kezelhetik a Microsoft Account hello BizTalk szolgáltatás hello BizTalk szolgáltatások portál használatával. Egy szervezeti fiók nem használható.  
  * **A forgatókönyv**: regisztrálta a BizTalk szolgáltatás központi telepítése egy Azure Active Directory szervezeti fiókkal (például user@fabrikam.com vagy user@contoso.com). Ebben a forgatókönyvben csak Azure Active Directory-felhasználók belül ugyanazon a szervezeten belül kezelhető hello hello BizTalk szolgáltatás hello BizTalk szolgáltatások portál használatával. A Microsoft-fiók nem használható.  
* BizTalk szolgáltatás létrehozása a klasszikus Azure portálon hello, ha vannak automatikusan regisztrált hello BizTalk Services portálra.
  * **A forgatókönyv**: hello klasszikus Azure-portál bA BizTalk szolgáltatás létrehozása, és válassza **kezelése** a hello első alkalommal. Hello BizTalk szolgáltatások portál megnyitásakor hello BizTalk szolgáltatás automatikusan regisztrálja, és készen áll a központi telepítések a.  
    Lásd: [regisztrálása és a BizTalk Szolgáltatástelepítésben frissítése hello BizTalk szolgáltatások portálja](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>Augusztus 14 frissítés
* BizTalk szolgáltatások portálja hello szerződés és híd leválasztásával – Kereskedelmipartner-egyezmények és hidak most le. Most hozza létre külön szerződések és a hidak, és futásidejű hidak, oldja meg a tooan megállapodás EDI üdvözlőüzenetére hello értékei alapján. Lásd: [megállapodások létrehozása az Azure BizTalk szolgáltatások](https://msdn.microsoft.com/library/azure/hh689908.aspx), [hozzon létre egy BizTalk szolgáltatások portálon EDI híd](https://msdn.microsoft.com/library/azure/dn793986.aspx), [hozzon létre egy BizTalk szolgáltatások portálon AS2 híd](https://msdn.microsoft.com/library/azure/dn793993.aspx), és [ Hogyan tegye megoldásához hidak futásidőben szerződéseket?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* hello beállítás toocreate sablonok megállapodások megszakad.  
* Hello küldési-oldalon szerződés megadhat különböző elválasztó mindegyik séma-készletet. Ez a konfiguráció a küldési oldal megállapodás protokoll beállításai szakaszban megadott. További információkért lásd: [egy X12 létrehozása az Azure BizTalk szolgáltatások megállapodás](https://msdn.microsoft.com/library/azure/hh689847.aspx) és [egy EDIFACT-egyezmény létrehozása az Azure BizTalk szolgáltatások](https://msdn.microsoft.com/library/azure/dn606267.aspx). Két új entitásokat is bekerül a hello TPM OM API toohello ugyanerre a célra. Lásd: [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) és [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standard XSD szerkezetek, beleértve a származtatott típusok támogatása. Lásd: [a a maps szerkezeteket használjon szokásos XSD](https://msdn.microsoft.com/library/azure/dn793987.aspx) és [használati származtatott típusok Leképezési forgatókönyvek és példák](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 új MIC üzenet az aláíráshoz és új titkosítási algoritmusok támogatja. Lásd: [az AS2-egyezmény létrehozása az Azure BizTalk szolgáltatások](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
  ## <a name="know-issues"></a>Ismert problémák

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>BizTalk szolgáltatások portál frissítése után csatlakozási problémák
  Ha rendelkezik hello BizTalk szolgáltatások portálja nyissa meg a BizTalk szolgáltatások frissítése közben a tooroll toohello szolgáltatás megváltozik, csatlakozási hibák léptek fel hello BizTalk szolgáltatások portálja előfordulhat, hogy szembesülhetnek.  
  Megoldás lehet, hogy indítsa újra a hello böngésző, törli hello gyorsítótárban, illetve hello portálon a saját módjában indul el.  

### <a name="visual-studio-ide-cannot-locate-hello-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE hello összetevő nem található, ha egy hiba vagy figyelmeztetés BizTalk szolgáltatások projektben kattintson
Telepítse a Visual Studio 2012 Update 3 RC 1 hello toofix hello probléma.  

### <a name="custom-binding-project-reference"></a>Egyéni kötésben projektbe egy hivatkozást a
Vegye figyelembe a következő helyzetekben a Visual Studio megoldás BizTalk szolgáltatások projekttel hello:  

* A hello azonos Visual Studio megoldás, a BizTalk szolgáltatások projekt és az egyéni kötésben projekt van. hello BizTalk szolgáltatás projekt egy hivatkozás toothis egyéni kötésben projektfájlt rendelkezik.
* hello BizTalk szolgáltatás projekt rendelkezik egy hivatkozás tooa egyéni kötési/viselkedését dll-Fájljában.

Sikeresen hello "Build", a Visual Studio megoldás. Ezt követően "Rebuild" vagy "Tiszta" hello megoldás. Ezután amikor újbóli létrehozásához vagy tiszta ebben az esetben a következő hello hiba akkor fordul elő:  
  Nem lehet toocopy fájl <Path tooDLL> too"bin\Debug\FileName.dll". hello folyamat nem éri a hello fájlt "bin\Debug\FileName.dll", mert egy másik folyamat használja.  

#### <a name="workaround"></a>Megkerülő megoldás
* Ha [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) van telepítve, a következő két lehetőség közül választhat hello rendelkezik:
  
  * Indítsa újra a Visual Studio vagy
  * Indítsa újra a hello megoldás. Build csak ezt követően végre hello megoldás.  
* Ha [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) van nincs telepítve, nyissa meg a Feladatkezelőt, hello folyamatok lapon, hello MSBuild.exe folyamat kattintson, majd hello folyamat leállítása gombra.  

### <a name="routing-toobasichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Útválasztás tooBasicHttpRelay végpontok nem támogatott a hidak és a BizTalk szolgáltatások portálja nem nyomtatható karakterek van előléptetve, mint a HTTP-fejlécek Ha
Nem nyomtatható karakterek Előléptetett tulajdonságok részeként, az üzenetek használatakor, az üzenetek hello BasicHttpRelay kötést használó célhelyre irányított toorelay nem lehet. Hello is, a tulajdonságok érhetők el, mert követési része a blobok URL-kódolású és megbízhatatlan kódolt célhoz előléptetve.  

### <a name="mdn-is-sent-asynchronously-even-if-hello-send-asynchronous-mdn-option-is-unchecked"></a>MDN aszinkron módon legyen elküldve, még akkor is, ha hello küldése aszinkron MDN beállítás nincs bejelölve
Fontolja meg ebben a forgatókönyvben – hello választásakor **aszinkron MDN küldése** jelölőnégyzetet, és adjon meg egy URL-cím toosend hello aszinkron MDN, és törölje a hello **aszinkron MDN küldése** jelölőnégyzet újra, hello MDN még mindig elküldött toohello megadott URL-CÍMÉT, annak ellenére, hogy hello beállítás toosend aszinkron MDNs nincs bejelölve.  
A probléma megoldásához törölje a jelölést a megadott URL-cím hello hello jelölésének törlése előtt **aszinkron MDN küldése** jelölőnégyzetet, majd telepítse a megállapodás hello AS2.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-toobe-sent-toohello-suspend-endpoint"></a>Egy érvényes interchange ok egy üres üzenetet küldött toobe toohello túl szóköz karakterből végpont felfüggesztése
Ha túl egy IEA szegmens szóközöket, hello kicsomagoló kezeli a jelenlegi interchange végét és hello következő beállítva szóközöket, a következő üzenet jelenik meg. Mivel ez nem érvényes interchange, azt láthatja, hogy egy sikeres üzenettel toohello útvonal cél, és egy üres üzenetet küld hello végpont felfüggesztése.  

### <a name="tracking-in-biztalk-services-portal"></a>BizTalk szolgáltatások portálon nyomon követése
Nyomkövetési események kerülnek rögzítésre toohello EDI-üzenet feldolgozása és bármely korrelációs. Ha egy üzenet hello protokoll szakasz kívül nem sikerül, nyomkövetési sikeres jeleníti meg. Ilyen esetben tekintse meg a napló szakasza toohello hello **részletek** oszlopa **követési** a hiba részletei.
hello X12 beállítások küldhet és fogadhat ([hozzon létre egy X12 Azure BizTalk Services szerződés](https://msdn.microsoft.com/library/azure/hh689847.aspx)) protokoll szakasz hello kapcsolatban nyújtanak információkat.  

### <a name="update-agreement"></a>Frissítés megállapodás
BizTalk szolgáltatások portálja hello lehetővé teszi toomodify hello identitás minősítő szerződés konfigurálásakor. Ez a eredményezhet inconsistence tulajdonságait. Például ZZ:1234567 és ZZ:7654321 hello minősítő megállapodás van. Hello BizTalk szolgáltatások portálja profil beállításaiban ZZ:1234567 toobe 01:ChangedValue módosítja. Nyissa meg a hello szerződés és 01:ChangedValue helyett ZZ:1234567 jelenik meg.
toomodify hello identitás, törlési hello megállapodás, minősítő frissítés **identitások** a partner profil hello, és hozza létre hello szerződést.  

> AZURE. Ez a viselkedés figyelmeztetés X12 és AS2 hatással van.  
> 
> 

### <a name="as2-attachments"></a>AS2-mellékletek
Üzenetek nem támogatottak az AS2-mellékletek elküldeni vagy fogadni. Pontosabban mellékletek csendes figyelmen kívül hagyja, és hello üzenettörzs rendszeres AS2-üzenetet dolgoz fel.  

### <a name="resources-remembering-path"></a>Erőforrások: Jelszóelőzmények elérési útja
Hozzáadásakor **erőforrások**, hello párbeszédpanelen előfordulhat, hogy ne feledje hello korábban használt elérési utat tooadd erőforrás. tooremember hello korábban használt elérési utat, próbálja meg felvenni túl hello BizTalk szolgáltatások portál webhely**megbízható helyek** az Internet Explorerben.  

### <a name="if-you-rename-hello-entity-name-of-a-bridge-and-close-hello-project-without-saving-changes-opening-hello-entity-again-results-in-an-error"></a>Módosítások mentése nélkül hello entitás nevét egy híd és Bezárás hello projekt átnevezése, hello entitás újra megnyitni a hibát eredményez
Vegye figyelembe a sorrend hello esetén:  

* Egy híd (például egy XML One-Way híd) tooa BizTalk szolgáltatás projekt hozzáadása  
* Nevezze át a hello híd hello egyednév tulajdonság értékének megadásával. Ez átnevezi hello társított .bridgeconfig fájlt hello megadott név.  
* Hello módosítások mentése nélkül zárja be a hello .bcs fájlt (a Visual Studio hello lapon bezárásával).  
* A Solution Explorer hello újra megnyitni a hello .bcs fájlt.  
  Megfigyelheti, hogy míg hello társított .bridgeconfig fájl hello megadott új néven, a hello a tervezési felülethez hello entitás neve még hello régi nevét. Ha tooopen hello híd konfigurációs hello híd összetevő duplán kattintva, kapott hello hiba a következő:  
  `‘<old name>’ Entity’s associated file ‘<old name>.bridgeconfig’ does not exist`Ebben az esetben rendszert futtató tooavoid ellenőrizze, hogy hello entitások BizTalk szolgáltatás projekt átnevezése után a módosítások mentése.  
  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk szolgáltatás projekt sikeresen hoz létre, akkor is, ha egy közbülső ki lett zárva a Visual Studio-projekt
Fontolja meg egy olyan forgatókönyvet, ahol egy összetevő (például egy XSD-fájlt) tooa BizTalk szolgáltatás projekt hozzáadása, adott összetevő foglalandó hello híd konfigurációs (például megadásával, egy kérelem üzenet típusa) és kizárása hello Visual Studio-projektet. Ebben az esetben hello projekt nem ad semmilyen hiba mindaddig, amíg törölt hello összetevő érhető el a hello hello lemezen a hol található a Visual Studio-projekt hello ugyanazon a helyen.
  
### <a name="hello-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-hello-bridges"></a>hello BizTalk szolgáltatás projekt nem ellenőrzi a séma rendelkezésre állási hello hidak konfigurálása közben
BizTalk szolgáltatás projektben toohello projekt hozzáadott séma importálhatja egy másik séma, ha hello BizTalk szolgáltatás projekt nem ellenőrzi, hogy szerepel-e az importált séma hello toohello projekt. A projekt toobuild kísérli meg, ha nem jelenik meg összeállítási hiba.
  
### <a name="hello-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>az UTF-8 charset mindig van hello válaszüzenetet egy XML-kérelem-válasz híd
Ebben a kiadásban hello charset hello válasz üzenet egy XML-kérelem-válasz híd értéke mindig tooUTF – 8.
  
### <a name="user-defined-datatypes"></a>Felhasználó által definiált adattípusok
hello BizTalk Adapter Pack adapterek belül hello BizTalk Adapter szolgáltatás nyújthatnak a felhasználó által definiált adattípusok adapter műveletekhez.
Felhasználó által definiált adattípusok használata esetén hello fájlok (.dll) toodrive:\Program Files\Microsoft BizTalk Adapter Service\BAServiceRuntime\bin\ vagy a globális szerelvény-gyorsítótárban (GAC) toohello hello hello BizTalk Adapter Service szolgáltatást futtató kiszolgálón másolja. Ellenkező esetben a következő hiba hello hello ügyfélen fordulhat elő:  
```
<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
<faultcode>s:Client</faultcode>
<faultstring xml:lang="en-US">hello UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing hello assembly containing hello UDT definition in hello Global Assembly Cache.</faultstring>
<detail>
  <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
  </AFConnectRuntimeFault>
</detail>
</s:Fault>
```  
  
> [!IMPORTANT]
> Ajánlott toouse GACUtil.exe tooinstall a globális szerelvény-gyorsítótárban hello fájl. GACUtil.exe dokumentumok hogyan toouse az eszköz és hello Visual Studio parancssori kapcsolókat.  
> 
> 

### <a name="restarting-hello-biztalk-adapter-service-web-site"></a>Hello BizTalk Adapter szolgáltatás webhely újraindítása
Telepítése hello **BizTalk Adapter szolgáltatás futásideje*** hoz létre hello **BizTalk szolgáltatás** webhely az IIS-ben, amely tartalmazza a hello **BAService** alkalmazás. **BAService** alkalmazás belső módon használja a továbbítási kötés tooextend hello reach helyszíni szolgáltatási végpont toohello felhő. Egy üzemeltetett szolgáltatást helyi hello megfelelő továbbítási végpont regisztrálva lesz a Service Bus hello csak akkor, ha hello a helyszíni szolgáltatás indításakor.  

Állítsa le és indítsa el a kérelmet, ha az alkalmazás automatikusan induló hello konfigurációja nem vehető figyelembe. Így ha **BAService** van leállítja, hogy mindig újra kell indítani hello **BizTalk szolgáltatás** webhely helyett. Nem elindulni, vagy állítsa le a hello **BAService** alkalmazás.

### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Különleges karakterek nem használható a cím és egyéb entitások nevekkel LOB-összetevők
Ne használjon speciális karaktereket a cím és egyéb entitások nevekkel LOB-összetevők. Ha így tesz, hello BizTalk szolgáltatás projekt telepítése során hiba lép fel. Bizonyos karaktereket, mint például a "%", hello BizTalk szolgáltatás webhely előfordulhat, hogy nyissa meg leállított állapotba, és hogy toomanually indítsa el.

### <a name="test-map-with-get-context-property"></a>A Context tulajdonság Get térkép tesztelése
Ha egy átalakítási tartalmaz egy **Context tulajdonság beolvasása** térkép művelet **teszt térkép** sikertelen lesz. Ideiglenes megoldásként, cserélje le a hello **Context tulajdonság Get** egy karakterlánc összefűzésére térkép művelet típusú adatokat tartalmazó térkép műveletet. A cél séma hello feltöltése és tesztelése más átalakítási-funkciók engedélyezése.

### <a name="test-map-property-does-not-display"></a>Teszt térkép tulajdonság nem jelenik meg
Hello **teszt térkép** tulajdonságok nem jelennek meg a Visual Studióban. Ez akkor fordulhat elő, ha hello **tulajdonságok** ablakot, és hello **Megoldáskezelőben** ablak egyidejűleg nincs rögzítve. tooresolve, a rögzítési hely hello **tulajdonságok** és hello **Megoldáskezelőben** windows.  

### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Dátum és idő formázza újra legördülő szürkén jelenik meg
Egy DateTime formázza újra térkép művelet hozzáadásakor toohello kialakítási felületi és konfigurálását követően hello Formátum legördülő lista lehet, hogy szürkén jelenik meg. Ez akkor fordulhat elő, ha hello számítógép megjelenítési **Közepes – 125 %** vagy **nagyobb – 150 %**. tooresolve, állítsa be a hello megjelenítési túl**Smaller – 100 %-os (alapértelmezett)** alábbi hello lépéseket használhatja:  

1. Nyissa meg hello **Vezérlőpult** kattintson **Megjelenés és személyre szabása**.
2. Kattintson a **megjelenített**.
3. Kattintson a **Smaller – 100 %-os (alapértelmezett)** kattintson **alkalmaz**.

Hello **formátum** legördülő lista most már a várt módon működik a kell.

### <a name="duplicate-agreements-in-hello-biztalk-services-portal"></a>BizTalk szolgáltatások portálja hello ismétlődő megállapodások
Vegye figyelembe a következő forgatókönyv hello:

1. Hozzon létre egy szerződést hello Trading Partner Management OM API használatával.
2. BizTalk szolgáltatások portálja hello két különböző lapon nyissa meg a hello szerződést.
3. Telepítse a hello megállapodás mindkét hello lapról.
4. Ennek eredményeképpen mindkét hello megállapodások telepítve hello BizTalk szolgáltatások portálja ismétlődő bejegyzést eredményez

**Megkerülő megoldás**. Nyissa meg a hello ismétlődő megállapodások hello BizTalk Services portálra, és eltávolítása.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-hello-artifact-store"></a>Hidak ne használja a frissített tanúsítvány még a tanúsítvány hello összetevő tárolójában frissítése után
Vegye figyelembe a következő forgatókönyvek hello:  

**1. példa: Tanúsítványok ujjlenyomat-alapú használata híd tooa szolgáltatásvégpont átvitelét üzenet biztonságossá tétele**  
Fontolja meg egy olyan forgatókönyvet, ahol a BizTalk szolgáltatás projekt ujjlenyomat-alapú tanúsítványokat használ. BizTalk szolgáltatások portálja hello hello tanúsítványt hello azonos nevet azonban egy másik ujjlenyomatot, de nem frissítik az hello BizTalk szolgáltatás projekt, ennek megfelelően frissíti. Ilyen esetben hello híd előfordulhat, hogy folytatni tooprocess köszönőüzenetei, mert hello régebbi Tanúsítványadatok hello csatorna gyorsítótár továbbra is lehet. Ezt követően üzenet feldolgozása sikertelen.  

**Megkerülő megoldás**: hello BizTalk szolgáltatás projekt hello tanúsítvány frissítése, és telepítse újra a hello projekt.  

**2. forgatókönyv: Név-alapú viselkedések tooidentify tanúsítványok használatával biztonságossá tételéhez híd tooa szolgáltatásvégpont üzenet átvitelét**

Fontolja meg egy olyan forgatókönyvet, ahol a BizTalk szolgáltatás projekt neve alapú viselkedések tooidentify tanúsítványokat használ. BizTalk szolgáltatások portálja hello hello tanúsítvány frissítése, de nem frissítik az hello BizTalk szolgáltatás projekt, ennek megfelelően. Ilyen esetben hello híd előfordulhat, hogy folytatni tooprocess köszönőüzenetei, mert hello régebbi Tanúsítványadatok hello csatorna gyorsítótár továbbra is lehet. Ezt követően üzenet feldolgozása sikertelen.  

**Megkerülő megoldás**: hello BizTalk szolgáltatás projekt hello tanúsítvány frissítése, és telepítse újra a hello projekt.  

### <a name="bridges-continue-tooprocess-messages-even-when-hello-sql-database-is-offline"></a>Hidak tooprocess üzenetek továbbra is, még akkor is, ha hello SQL-adatbázis offline állapotban.
BizTalk szolgáltatások hidak hello tooprocess üzenetek egy ideig továbbra is akkor is, ha a Microsoft Azure SQL Database (amely tárolja az információkat, például a telepített összetevők és a folyamatok futtató hello), hello offline állapotban. Ennek az az oka BizTalk szolgáltatások gyorsítótárazott hello összetevők és híd konfigurációs használ.
Ha nem szeretné, hogy hello hidak tooprocess üzeneteire Ha hello SQL-adatbázis offline állapotban, használja a BizTalk szolgáltatások PowerShell-parancsmagok toostop hello, vagy hello BizTalk szolgáltatás felfüggesztése. Lásd: [Azure BizTalk szolgáltatás felügyeleti minta](http://go.microsoft.com/fwlink/p/?LinkID=329019) hello Windows PowerShell parancsmagok toomanage műveletekhez.  

### <a name="reading-hello-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>XML üdvözlőüzenetére olvasási egy híd egyéni kód összetevőn belüli egy extra AJ karaktert tartalmaz.
Fontolja meg egy olyan forgatókönyvet, ahol azt szeretné, hogy tooread egy híd egyéni kód egy XML-üzenet. .NET API System.Text.Encoding.UTF8.GetString(bytes) hello használatakor egy extra AJ karakter hello kimeneti üdvözlőüzenetére hello elején szerepel. Ha nem szeretné, hogy hello kimeneti tooinclude hello extra AJ karaktert kell használnia ```System.IO.StreamReader().ReadToEnd()```.

### <a name="sending-messages-tooa-bridge-using-wcf-does-not-scale"></a>Üzenetek küldése tooa híd WCF használatával nem méretezhető
Üzenetek küldése WCF használatával tooa híd nem méretezhető. Ha azt szeretné, hogy méretezhető ügyfél HttpWebRequest helyette használjon.

### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-toogeneral-availability-ga"></a>FRISSÍTÉS: Jogkivonat-szolgáltató által jelzett hibát BizTalk szolgáltatás előzetes tooGeneral rendelkezésre állási (GA) verzióról
A program egy EDI vagy AS2 megállapodás aktív kötegek. Hello BizTalk szolgáltatás frissítésekor a Preview tooGA hello következő fordulhat elő:

* Hiba: hello jogkivonat-szolgáltató lett nem tooprovide egy biztonsági jogkivonatot. Küldött biztonságijogkivonat-szolgáltatót: hello távoli név nem sikerült feloldani.
* Kötegelt feladatok törölve lesznek.

**Megkerülő megoldás**: Miután hello BizTalk szolgáltatás frissített tooGeneral rendelkezésre állási (GA), telepítse újra a hello szerződést.  

### <a name="upgrade-toolbox-shows-hello-old-bridge-icons-after-upgrading-hello-biztalk-services-sdk"></a>FRISSÍTÉS: Eszközkészlet megjeleníti hello régi híd ikonok hello BizTalk SDK-t a frissítés után
BizTalk szolgáltatások SDK, melyből régi ikonjai hello hidak hello egy korábbi frissítés után hello eszközkészlet tooshow hello régi ikonok hello hidak továbbra is. Azonban ha hozzáad egy híd tooBizTalk szolgáltatás projekt Tervező felületére, hello felület hello új ikon látható.  

**Megkerülő megoldás**. A probléma alatt hello .tbd fájlok törlésével megkerüléséhez <system drive>: \Users\<felhasználó > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-tooga-might-show-an-error-indicating-that-hello-edi-capability-is-not-available"></a>FRISSÍTÉS: A BizTalk Portal update előzetes tooGA a azt jelzi, hogy adott hello EDI funkció nem érhető el hiba előfordulhat, hogy megjelenítése
BizTalk szolgáltatások portálja hello bejelentkezett telepítő hello BizTalk szolgáltatások az előzetes tooGA frissítését végzi, ha kaphat hello hello portál a következő hiba:  

Ez a funkció nem érhető el a Microsoft Azure BizTalk Services jelen kiadása részeként. toouse ezeket a képességeket tooan megfelelő edition váltani.  

**Megoldási**: hello portal, zárja be és nyissa meg hello böngészőben, majd jelentkezzen be a hello portálon található kimenő naplót.  

### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-tooga"></a>FRISSÍTÉS: Új követési adatok nem jelenik meg frissített tooGA végeztével a BizTalk szolgáltatások
Tegyük fel, a helyzetet, amelyben egy XML híd BizTalk szolgáltatás előzetes előfizetés telepítve vannak-e. Toohello híd küldött üzenetek és hello megfelelő nyomon követési adatok elérhető hello BizTalk Services portálra. Most, ha hello BizTalk Services portál és a BizTalk szolgáltatások futásidejű bits frissített tooGA, és küldünk egy azonos híd végpont telepített üzenetben toohello, nyomkövetési adatok hello nem jelenik meg a frissítés után küldött üzenetek.  

### <a name="pipelines-versus-bridges"></a>És a hidak csővezeték
Ez a dokumentum hello kifejezés "folyamatok" és "hidak" van megegyezik. Mindkét alapvetően azonos hello jelenti dolog, ami, egy üzenet feldolgozása egység BizTalk szolgáltatások telepíthetők.  

### <a name="concepts"></a>Alapelvek
[BizTalk szolgáltatások](https://msdn.microsoft.com/library/azure/hh689864.aspx)   

