---
title: "az Azure-portálon hello Azure BizTalk szolgáltatások aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan tooprovision, vagy hozzon létre Azure-portálon; hello Azure BizTalk szolgáltatások MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 6781cadada8ac9c84e1fe045d2b0f995811f75b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-biztalk-services-using-hello-azure-portal"></a>Hello Azure-portál használatával BizTalk szolgáltatás létrehozása

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]


> [!TIP]
> az Azure-portálon toohello toosign, van szüksége egy Azure-fiókot és az Azure-előfizetés. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. Lásd: [Ingyenes Azure-próbafiók](http://go.microsoft.com/fwlink/p/?LinkID=239738).


## <a name="CreateService"></a>BizTalk-szolgáltatás létrehozása
Attól függően, hogy hello kiadását választja nem minden BizTalk szolgáltatás beállítások érhetők el.

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello alsó navigációs ablaktáblán válassza ki **új**:  
   ![Hello új gomb kiválasztása][NEWButton]
3. Válassza az **APP SERVICES** (ALKALMAZÁSSZOLGÁLTATÁSOK) > **BIZTALK SERVICE** > **CUSTOM CREATE** (EGYÉNI LÉTREHOZÁS) elemet:  
   ![Válassza a BizTalk Service elemet, majd a Custom Create (Egyéni létrehozás) elemet][NewBizTalkService]
4. Adja meg a hello BizTalk szolgáltatás beállításai:
   
    <table border="1">
    <tr>
    <td><strong>BizTalk-szolgáltatás neve</strong></td>
    <td>Bármilyen nevet megadhat, de használjon konkrét nevet. Néhány példa:<br/><br/>
    <em>mycompany</em>.biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>.biztalk.windows.net<br/>
    <em>myapplication</em>.biztalk.windows.net<br/><br/>". biztalk.windows.net" automatikusan hozzáadott toohello neve meg. Ezzel létrehoz egy URL-címet, amely a BizTalk szolgáltatás, például használt tooaccess <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Kiadás</strong></td>
    <td>Ha hello fejlesztési/tesztelési fázisban, kattintson az <strong>fejlesztői</strong>. Ha hello gyártás szakaszban, akkor hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk szolgáltatások: kiadások diagram</a> toodetermine Ha <strong>prémium</strong>, <strong>szabványos</strong>, vagy <strong>Basic</strong>hello megfelelő választás az üzleti forgatókönyv szerint.
    </td>
    </tr>
    <tr>
    <td><strong>Régió</strong></td>
    <td>Válassza ki a földrajzi régióban toohost hello a BizTalk szolgáltatás.</td>
    </tr>
    <tr>
    <td><strong>Tartomány URL-je</strong></td>
    <td><strong>Választható</strong>. Alapértelmezés szerint hello tartomány URL-címe az <em>YourBizTalkServiceName</em>. biztalk.windows.net. Egyéni tartomány is beírható. Ha a tartománya például a <em>contoso</em>, beírhatja a következőt: <br/><br/>
    <em>Sajátvállalat</em>.contoso.com<br/>
    <em>SajátVállalatSajátAlkalmazás</em>.contoso.com<br/>
    <em>SajátAlkalmazás</em>.contoso.com<br/>
    <em>BizTalkSzolgáltatásNeve</em>.contoso.com<br/>
    </td>
    </tr>
    </table>
Válassza ki a hello tovább nyílra.
5. Adja meg a tárolási hello és adatbázis-beállítások:  <table border="1">
    <tr>
    <td><strong>Megfigyelő/archiváló tárfiók</strong></td>
    <td>Válasszon meglévő Storage-fiókot, vagy hozzon létre egy új Storage-fiókot. <br/><br/>Ha létrehoz egy új tárfiókot, adja meg a hello <strong>Tárfióknév</strong>.</td>
    </tr>
    <tr>
    <td><strong>Nyomkövető adatbázis</strong></td>
    <td>Ha meglévő Azure SQL Database-adatbázist használ, azt nem használhatja másik BizTalk-szolgáltatás. Hello bejelentkezési nevet és jelszót adott Azure SQL adatbázis-kiszolgáló létrehozásakor megadott van szüksége.<br/><br/><strong>Tipp</strong> hello követési adatbázis létrehozása és a figyelés/archiválás tárfiók hello ugyanabban a régióban, hello BizTalk szolgáltatás.</td>
    </tr>
    </table>
Válassza ki a hello tovább nyílra.
6. Adja meg a hello adatbázis-beállításokat:  <table border="1">
    <tr>
    <td><strong>Name (Név)</strong></td>
    <td>Érhető el, ha <strong>hozzon létre egy új SQL-adatbázispéldány</strong> van megadva a hello előző képernyőre.
    <br/><br/>
Adjon meg egy SQL-adatbázis neve toobe a BizTalk szolgáltatás által használt.</td>
    </tr>
    <tr>
    <td><strong>Kiszolgáló</strong></td>
    <td>Érhető el, ha <strong>hozzon létre egy új SQL-adatbázispéldány</strong> van megadva a hello előző képernyőre.
    <br/><br/>
Válasszon egy meglévő SQL Database-kiszolgálót vagy hozzon létre új SQL Database-kiszolgálót.</td>
    </tr>
    <tr>
    <td><strong>Kiszolgáló bejelentkezési neve</strong></td>
    <td>Adjon meg hello bejelentkezési nevet.</td>
    </tr>
    <tr>
    <td><strong>Kiszolgáló bejelentkezési jelszava</strong></td>
    <td>Adja meg a hello bejelentkezési jelszót.</td>
    </tr>
    <tr>
    <td><strong>Régió</strong></td>
    <td>Akkor érhető el, ha az <strong>Új SQL Database-példány létrehozását</strong> választotta. Válassza ki a földrajzi régióban toohost hello az SQL-adatbázis.</td>
    </tr>
    </table>

Válassza ki a hello pipa toocomplete hello varázsló. hello folyamatban ikon jelenik meg:  
![Az állapotjelző ikon megjelenik, ha befejeződött][ProgressComplete]

Amikor végzett, a hello Azure BizTalk szolgáltatás létrehozott és az alkalmazások készen. hello alapértelmezett beállítások elegendőek. Ha toochange hello alapértelmezett beállításokat, válassza ki a **BIZTALK szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki a BizTalk szolgáltatás. További beállítások is megjelennek a hello [irányítópult, a figyelő és a skála lapon](biztalk-dashboard-monitor-scale-tabs.md) hello tetején.

Attól függően, hogy hello hello BizTalk szolgáltatás állapotát vannak bizonyos műveletek, amelyek nem fejezhető be. Ezek a műveletek listájának megtekintéséhez nyissa meg túl[BizTalk szolgáltatások állapota diagram](biztalk-service-state-chart.md).

## <a name="post-provisioning-steps"></a>Kiépítés utáni lépések
* [A helyi számítógép hello tanúsítvány telepítése](#InstallCert)
* [Termelésre kész tanúsítvány hozzáadása](#AddCert)
* [Hello hozzáférés-vezérlés névtér beolvasása](#ACS)

#### <a name="InstallCert"></a>A helyi számítógép hello tanúsítvány telepítése
A BizTalk-szolgáltatás kiépítésének részeként létrejön egy önaláírt tanúsítvány, amely társítva van a BizTalk-szolgáltatás előfizetésével. Töltse le a tanúsítványt kell, és ahol Ön BizTalk szolgáltatás alkalmazások központi telepítése, illetve üzenetek tooa BizTalk szolgáltatás végpontjának küldése számítógépekre telepíthető.

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Válassza ki **BIZTALK szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki a BizTalk szolgáltatás előfizetésében.
3. Jelölje be hello **irányítópult** fülre.
4. Válassza az **SSL-tanúsítvány letöltése** elemet:  
   ![SSL-tanúsítvány módosítása][QuickGlance]
5. Kattintson duplán a hello tanúsítványt, és futtasson hello varázsló tooinstall hello tanúsítvány használatával. Ellenőrizze, hogy a hello hello tanúsítványát telepítenie **megbízható legfelső szintű hitelesítésszolgáltatók** tárolja.

#### <a name="AddCert"></a>Termelésre kész tanúsítvány hozzáadása
hello önaláírt tanúsítványt, amely automatikusan jön létre, amikor csak a fejlesztési környezetben használatra szánt BizTalk szolgáltatás létrehozása. A termelési forgatókönyvekhez cserélje le egy termelésre kész tanúsítványra.

1. A hello **irányítópult** lapon jelölje be **frissítés SSL-tanúsítvány**.
2. Keresse meg a saját SSL-tanúsítvány tooyour (*CertificateName*.pfx), amely tartalmazza a BizTalk szolgáltatás neve, hello jelszót adjon meg, és kattintson a pipa hello.

#### <a name="ACS"></a>Hello hozzáférés-vezérlés névtér beolvasása
1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Válassza ki **BIZTALK szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki a BizTalk szolgáltatás.
3. Hello tálcán, válassza ki a **kapcsolatadatok**:  
   ![Kattintson a Connection Information (Kapcsolatadatok) elemre.][ACSConnectInfo]
4. Hello hozzáférés-vezérlés értékek másolása.

Amikor BizTalk Service-projektet helyez üzembe a Visual Studióból, ezt a hozzáférés-vezérlési névteret írja be. hello hozzáférés-vezérlés névtér automatikusan létrejön a BizTalk szolgáltatás.

hello hozzáférés-vezérlés értékek bármilyen alkalmazással használható. Azure BizTalk szolgáltatás létrehozása a hozzáférés-vezérlés névtér hello hitelesítés és a BizTalk szolgáltatás központi telepítése az szabályozza. Ha szeretné, hogy toochange hello előfizetés vagy hello névtér kezelésére, válassza ki a **ACTIVE DIRECTORY** a hello bal oldali navigációs panelen, és válassza ki a névteret. hello tálcán felsorolja a beállításokat.

Kattintson a **kezelése** megnyílik hello Access Control felügyeleti portálon. Hello Access Control felügyeleti portálon, a BizTalk szolgáltatás által használt hello **identitások szolgáltatás**:  
![Szolgáltatás-identitások ACS a hello Access Control felügyeleti portálon][ACSServiceIdentities]

Hozzáférés-vezérlés szolgáltatásidentitás hello olyan hitelesítő adatokat, amelyek lehetővé teszik az alkalmazások és ügyfelek tooauthenticate közvetlenül a hozzáférés-vezérlés és fogadhatnak jogkivonatot.

> [!IMPORTANT]
> hello BizTalk szolgáltatás használja **tulajdonos** hello alapértelmezett szolgáltatásidentitás és hello **jelszó** érték. Hello szimmetrikus kulcs-érték helyett hello jelszó érték használatakor hello alábbi hiba történhet.<br/><br/>*Nem lehet kapcsolódni a megadott hello toohello Access Control Management Service fiók hitelesítő adatait*
> 
> 

[Az ACS-névtér felügyeletét](https://msdn.microsoft.com/library/azure/hh674478.aspx) ismertető dokumentum tartalmaz néhány útmutatást és javaslatot.

## <a name="requirements-explained"></a>A követelmények részletei
Ezeket a követelményeket csak akkor érvényesíthetők toohello ingyenes kiadásban.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Mi szükséges</strong></td>
        <td><strong>Miért szükséges</strong></td>
</tr>
<tr>
<td>Azure-előfizetés</td>
<td>hello előfizetés határozza meg, akik toohello Azure-portálon bejelentkezhet. hello fióktulajdonos hoz létre hello előfizetést, <a HREF="https://account.windowsazure.com/Subscriptions"> Azure-előfizetések</a>.
<br/><br/>
hello Azure-fiók több előfizetéssel is rendelkezhet, és kezelheti bárki, aki engedélyezve van. Például az Azure-fiók jogosult egy nevű előfizetést hoz létre <em>BizTalkServiceSubscription</em> és által biztosított BizTalk rendszergazdák hello a vállalaton belül (például ContosoBTSAdmins@live.com) toothis előfizetés eléréséhez. Ebben a forgatókönyvben hello BizTalk rendszergazdák bejelentkezik toohello Azure-portálon, de rendelkezik teljes rendszergazdai jogosultságok tooall hello üzemeltetett szolgáltatások hello az előfizetést, beleértve az Azure BizTalk szolgáltatások. hello BizTalk rendszergazdák nincsenek hello Azure-fiókhoz tulajdonosainak, és ezért nem rendelkezik hozzáféréssel tooany számlázási adatokat.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Előfizetések és a Storage-fiókok kezelése az Azure-portálon hello</a> nyújt részletesebb információt.
</td>
</tr>
<tr>
<td>Azure SQL Database</td>
<td>Hello táblákat, nézeteket és tárolt eljárások hello BizTalk szolgáltatás, így az hello nyomon követési adatok által használt tárolja.
<br/><br/>
BizTalk-szolgáltatások létrehozásakor használhat meglévő Azure SQL Server-kiszolgálót vagy Azure SQL Database-adatbázist, vagy automatikusan létrehozhat egy új kiszolgálót vagy adatbázist.
<br/><br/>
SQL adatbázis-méretezési hello automatikusan úgy van beállítva. Hello alapértelmezett méretezési általában elegendő a BizTalk szolgáltatás számára. Változó hello méretezési hatással van a díjszabás. Lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">Fiókok és számlázás az Azure SQL Database-ben</a>
<br/><br/>
<strong>Megjegyzések</strong>
<br/>
<ul>
<li> Új Azure SQL Server és adatbázis létrehozásakor az Azure-szolgáltatások automatikusan engedélyezettek. hello BizTalk szolgáltatás Azure-szolgáltatások szükségesek engedélyezni kell.</li>
<li>Ha egy meglévő Azure SQL Server hoz létre egy új Azure SQL Database, hello tűzfalszabályt hello kiszolgáló, nem módosulnak. Ennek eredményeképpen lehetőség más Azure-szolgáltatások használata nem engedélyezett a hozzáférés toohello Server-adatbázisokat.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure hozzáférés-vezérlési névtér</td>
<td>Az Azure BizTalk Services modullal végez hitelesítést. Amikor BizTalk Service-projektet helyez üzembe a Visual Studióból, ezt a hozzáférés-vezérlési névteret írja be. BizTalk szolgáltatás létrehozása, amikor a rendszer automatikusan létrehoz hello hozzáférés-vezérlés névtér.</td>
</tr>

<tr>
<td>Azure Storage-fiók</td>
<td>Által biztosított hozzáférés tootables, blobok és a BizTalk szolgáltatás toosave hello következő által használt:

<ul>
<li>A naplófájlok, hogy a figyelő hello BizTalk szolgáltatás. kimeneti figyelési hello megjelenik a hello **figyelés** hello Azure-portálon lapján.</li>
<li>Egy X12 vagy AS2 megállapodást a partnerek között létrehozásakor hello Archiválás szolgáltatás toostore üzenettulajdonságok engedélyezheti. Ezek az adatok hello Storage-fiók mentése.</li>
</ul>
<br/>
BizTalk Services-szolgáltatások létrehozásakor használhat meglévő Storage-fiókot, vagy automatikusan létrehozhat egy új Storage-fiókot.
<br/><br/>
a BizTalk szolgáltatás hello alapértelmezett tárolási beállítások elegendőek.
<br/><br/>
Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs. Ezeket a kulcsokat a tárfiók hozzáférési tooyour szabályozza. hello BizTalk szolgáltatás automatikusan hello elsődleges kulcsot használja.
<br/><br/>
További információért lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">Storage</a>.
</td>
</tr>

<tr>
<td>SSL személyes tanúsítvány</td>
<td>
Azure BizTalk Services-szolgáltatások létrehozásakor létrejön a BizTalk Services-szolgáltatás nevét tartalmazó HTTPS URL. Az URL-cím, automatikusan konfigurált toouse csak fejlesztési önaláírt tanúsítványt. A termeléshez személyes SSL-tanúsítvány szükséges.
<br/><br/>
<strong>SSL-tanúsítvánnyal kapcsolatos fontos információk</strong>

<ul>
<li>hello tanúsítvány lejárati dátumnak kell lennie a kisebb, mint 5 év.</li>
<li>Minden személyes tanúsítványhoz jelszó szükséges. Jegyezze meg ezt a jelszót, és ajánlott eljárásként ossza meg a rendszergazdáival.</li>
<li>A tesztelési/fejlesztési környezetekben önaláírt tanúsítványokat használnak. Ha önaláírt tanúsítványokat használ, hello tanúsítvány tooyour személyes tanúsítványtárolójába importálni és hello megbízható legfelső szintű hitelesítésszolgáltatók tanúsítványtárolójába.</li>
</ul>
<br/>Hello éles tanúsítvány kérelem tooyour hitelesítésszolgáltató küldésekor adja meg a következő tanúsítvány tulajdonságai hello:
<br/>

<ul>
<li><strong>Kibővített kulcshasználat</strong>: Az Azure BizTalk Services legalább kiszolgálói hitelesítést igényel.</li>
<li><strong>Köznapi név</strong>: hello teljesen minősített tartományneve (FQDN) az Azure BizTalk szolgáltatás URL-CÍMÉT adja meg. Lásd ezen cikk <a HREF="#CreateService">BizTalk Services-szolgáltatás létrehozása</a> szakaszát.</li>
</ul>
<br/>
Egy új vagy eltérő tanúsítvány hello BizTalk szolgáltatás létrehozása után adhatók hozzá.
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting tooany location.--->



## <a name="hybrid-connections"></a>Hibrid kapcsolatok
Egy Azure BizTalk szolgáltatás létrehozásakor hello **hibrid kapcsolatok** lap jelenik meg:

![Hibrid kapcsolatok lap][HybridConnectionTab]

Hibrid kapcsolatok használt tooconnect egy Azure webhely vagy az Azure-mobilszolgáltatás tooany helyszíni erőforrása, amely a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k, a Mobile Services és a legtöbb egyéni webszolgáltatások használja.  Hibrid kapcsolatok és a BizTalk szolgáltatás hello eltérőek. BizTalk szolgáltatás hello rendszer használt tooconnect Azure BizTalk szolgáltatások tooan helyszíni sor az üzleti (LOB).

 Lásd: [hibrid kapcsolatok](integration-hybrid-connection-overview.md) toolearn további, beleértve a létrehozása és a hibrid kapcsolatok kezelése.

## <a name="next-steps"></a>Következő lépések
Most, hogy a BizTalk szolgáltatás létrehozása, ismerkedjen meg különböző hello [BizTalk szolgáltatások: irányítópult, a figyelő és a skála lapon](biztalk-dashboard-monitor-scale-tabs.md). Az Azure BizTalk-szolgáltatás készen áll az alkalmazásokhoz. alkalmazások, lépjen túl létrehozásának toostart[Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Lásd még:
* [BizTalk Services: Kiadások diagramja](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: Állapottáblázat](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Biztonsági mentés és visszaállítás](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Szabályozás](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Kiállító neve és kiállító kulcsa](biztalk-issuer-name-issuer-key.md)<br/>
* [Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Hibrid kapcsolatok](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
