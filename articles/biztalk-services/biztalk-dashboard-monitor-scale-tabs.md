---
title: "aaaDashboard, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolatok a BizTalk szolgáltatások |} Microsoft Docs"
description: "További tudnivalók hello vezérlők, a BizTalk szolgáltatások hello klasszikus portál lapokon teljesítmény figyeléséhez: irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolatok. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c9fafdad20489571ee3849bbacd2c2b10933154f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="review-hello-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Tekintse át a hello irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Miután a BizTalk szolgáltatás létrehozása és az alkalmazás központi telepítése, néhány hello BizTalk szolgáltatás beállításainak módosítása, és hello alkalmazás teljesítményének figyelése. 

A klasszikus Azure portálon hello megnyitásakor automatikusan elhelyezve hello **minden elem** külön-külön tooview a BizTalk szolgáltatás, válassza ki a BizTalk szolgáltatás hello **minden elem** lapon vagy select hello **BIZTALK szolgáltatások** ; lapra, és válassza ki a BizTalk szolgáltatás neve.

Ez egy új ablakban nyílik meg a következő lapokon hello. Ez a témakör ismerteti az ezeken a lapokon.

## <a name="quickstart-quickstartquickstart"></a>Gyors üzembe helyezés)![Első lépések][Quickstart])
Hello BizTalk szolgáltatások Edition, attól függően, hogy az összes felsorolt beállítások nem érhetők el. 

<table border="1">
    <tr>
        <td><strong>Hello eszközök beszerzése</strong></td>
        <td>Töltse le a hello BizTalk szolgáltatások SDK tooinstall hello Visual Studio projektsablonjai a helyi fejlesztési számítógépen. Ezek a sablonok létrehozása hello <strong>BizTalk szolgáltatások</strong> (híd) és hello <strong>BizTalk szolgáltatás-összetevők</strong> (átalakítási) Visual Studio-projektek, amelyek a telepített tooyour BizTalk szolgáltatás.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK </a> és <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">telepítése hello Azure BizTalk szolgáltatások SDK</a> listák hello lépéseket tooget elindult.
        </td>
    </tr>
    <tr>
        <td><strong>Partner megállapodások létrehozása</strong></td>
        <td>Megnyílik a hello Azure BizTalk szolgáltatások portálja adja hozzá a partnerek, és ahol a X12, AS2, hozzon létre Azure-platformon futó és EDIFACT EDI-szerződések.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> listák hello lépéseket tooget elindult.
        </td>
    </tr>

<tr>
        <td><strong>További információ a BizTalk szolgáltatások</strong></td>
        <td>Nyissa meg toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">center tanulási</a> toolearn Azure BizTalk szolgáltatásokkal kapcsolatos további.</td>
</tr>
</table>


A tálcán hello hello lap alján, a következőket teheti:

<table border="1">

<tr>
<td><strong>Kezelése</strong> az alkalmazások központi telepítésének</td>
<td>Megnyílik a hello Azure BizTalk Services portálra. BizTalk szolgáltatások portálja hello hello be tooEDI konfiguráció – beleértve a partnerek hozzáadása és X12, AS2, létrehozása és EDIFACT-egyezmények.
<br/><br/>
Ez az azonos hello <strong>egyezmények létrehozása</strong> a hello <strong>gyors üzembe helyezés</strong> lapon.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> hello BizTalk szolgáltatások portálja nyújt részletesebb információt.</td>
</tr>

<tr>
<td><strong>Kapcsolat információi</strong> a hozzáférés-vezérlési Namespace hello</td>
<td>Kapcsolati adatokat, majd hello Access Control Namespace, alapértelmezett kibocsátó, és alapértelmezett kulcs jelennek meg. Ezek az értékek másolhatja.
<br/><br/>
Nyissa meg a hozzáférés-vezérlési Portal hello is. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Hozzon létre egy hozzáférés-vezérlés Namespace</a> hello hozzáférés-vezérlési Portal nyújt részletesebb információt.</td>
</tr>

<tr>
<td><strong>Kulcsok szinkronizálása</strong> a hello Storage-fiók</td>
<td>Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs. A titkosítási kulcsokat a Tárfiók hozzáférési tooyour szabályozza. A BizTalk szolgáltatás automatikusan használ hello elsődleges kulcs. <strong>Kulcsok szinkronizálása</strong> engedélyezése a felhasználók tooswitch hello elsődleges kulcs és a másodlagos kulcs hello közötti hello BizTalk szolgáltatás megszakítása nélkül.
<br/><br/>
Például azt szeretné, hello BizTalk szolgáltatás toouse hello Tárfiók új elsődleges kulcsot. toodo ezt:
<br/><br/>
<ol>
<li>Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>. Válassza ki a hello másodlagos kulcsát. Ekkor a hello BizTalk szolgáltatás elindul, hello másodlagos kulcs használatával.</li>
<li>A klasszikus Azure portálon hello válassza ki a tárfiók, és hello elsődleges kulcs újragenerálása. Ne feledje, hogy a BizTalk szolgáltatás hello másodlagos kulcsot használja.</li>
<li>Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>. Jelölje ki, hello elsődleges kulcs. Ez az hello új elsődleges kulcs, akkor a rendszer újra létrehozza.</li>
<li>A klasszikus Azure portálon hello válassza ki a tárfiók, és hello másodlagos kulcs újragenerálása.</li>
</ol>
<br/>
A folyamat elnevezése "helyettesítő kulcsok". hello célja tooenable felhasználók tooswitch hello elsődleges kulcs és a másodlagos kulcs hello közötti hello BizTalk szolgáltatás megszakítása nélkül.</td>
</tr>

<tr>
<td><strong>Törlés</strong> az alkalmazás</td>
<td>A BizTalk szolgáltatás törléséhez és a rendszer eltávolítja az összes telepített elemek tooit.</td>
</tr>
</table>


## <a name="dashboard"></a>Irányítópult
Hello BizTalk szolgáltatások Edition, attól függően, hogy az összes felsorolt beállítások nem érhetők el. 

A BizTalk szolgáltatás neve kiválasztásakor hello irányítópult lap akkor jelenik meg. Az irányítópult a következőket teheti:

##### <a name="usage-overview-shows-hello-number-of-used-hybrid-connections"></a>Használati áttekintés: A használt hibrid kapcsolatok hello számát jeleníti meg.
Hello használati adatok is megjelennek GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metrika Graph: Metrikák rögzített listáját tartalmazza.
Ezeket a mérési adja meg a valós idejű vonatkozó hello hello BizTalk szolgáltatás állapotát. Meg lehet hello **relatív** vagy **abszolút** értékek és hello időtartománynak **időköz** hello grafikonon megjelenített hello mérőszámokat. 

A metrikák leírását, nyissa meg túl[elérhető](#Metrics) ebben a témakörben.

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Gyors áttekintő: Felsorolja a BizTalk szolgáltatás tulajdonságai
<table border="1">

<tr>
<td><strong>Követés adatbázis-hitelesítő adatok frissítése</strong></td>
<td>Módosítások hello felhasználónév és jelszó toolog hello követési adatbázis azokat.</td>
</tr>
<tr>
<td><strong>SSL-tanúsítvány frissítése</strong></td>
<td>Frissítheti hello BizTalk szolgáltatás toouse egy másik SSL-tanúsítványt. Egy önaláírt SSL-tanúsítvány mikor automatikusan létrejön, <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">hello BizTalk szolgáltatás létrehozása</a>.</td>
</tr>
<tr>
<td><strong>Tanúsítvány letöltése</strong></td>
<td>A BizTalk szolgáltatás tooa helyi számítógép által használt SSL-tanúsítvány hello töltheti le.</td>
</tr>
<tr>
<td><strong>Állapot</strong></td>
<td>A BizTalk szolgáltatás hello aktuális állapotát jeleníti meg. Lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk szolgáltatások: szolgáltatás állapota diagram</a>. </td>
</tr>
<tr>
<td><strong>URL-címe</strong></td>
<td>a BizTalk szolgáltatás hello URL-címe Ez az hello ugyanaz, mint hello <strong>tartomány URL-cím</strong> a BizTalk szolgáltatás létrehozásakor megadott.</td>
</tr>
<tr>
<td><strong>Nyilvános virtuális IP-cím (VIP) címe</strong></td>
<td>hello IP-cím hozzárendelése tooyour BizTalk szolgáltatás. Bemeneti végpontjai szolgál, és a kimenő forgalom hello forráscím. Az IP-cím tartozik tooyour BizTalk szolgáltatás, mindaddig, amíg létrejön. Ha törli a hello BizTalk szolgáltatás, hello IP-cím hozzá van rendelve tooanother BizTalk szolgáltatás.</td>
</tr>
<tr>
<td><strong>Az ACS-Namespace</strong></td>
<td>Hello BizTalk szolgáltatás végzi a hitelesítést.</td>
</tr>
<tr>
<td><strong>Kiadás</strong></td>
<td>Edition hello BizTalk szolgáltatás létrehozásakor megadott hello sorolja fel.</td>
</tr>
<tr>
<td><strong>Hely</strong></td>
<td>Megjeleníti a földrajzi régióban, amelyen a BizTalk szolgáltatás hello.</td>
</tr>
<tr>
<td><strong>Létrehozva</strong></td>
<td>Megjeleníti hello dátum és idő hello BizTalk szolgáltatás létrejött.</td>
</tr>
<tr>
<td><strong>Adatbázis nyomon követése</strong></td>
<td>hello Azure SQL-adatbázis neve, amely nyomon követése a BizTalk szolgáltatás által használt táblák hello tárolja. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Követelmények Explained</a> hello követési adatbázis részleteit.</td>
</tr>
<tr>
<td><strong>Tárolási figyelés/archiválása</strong></td>
<td>hello Azure Tárfiók neve, amely a BizTalk szolgáltatás kimeneti figyelési hello tárolja.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Követelmények Explained</a> hello tárfiók részleteit.</td>
</tr>
<tr>
<td><strong>Előfizetés neve</strong></td>
<td>A BizTalk szolgáltatás üzemeltető hello előfizetés sorolja fel. hello előfizetés irányító hozzáférés toohello a klasszikus Azure portálon.</td>
</tr>
<tr>
<td><strong>Előfizetés-azonosító</strong></td>
<td>Előfizetés létrehozásakor a rendszer automatikusan előállítja az előfizetés-azonosító. REST API-k használata esetén szükség lehet a tooenter hello előfizetés-azonosító.</td>
</tr>
</table>

[BizTalk szolgáltatások: Telepítési használata Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=302280) listák hello lépéseket toocreate BizTalk szolgáltatás.

##### <a name="manage-connection-information-sync-keys-and-delete-in-hello-task-bar"></a>Kezelése, a kapcsolatadatok, a szinkronizálási kulcsokat, és hello tálcán törlése:
<table border="1">

<tr>
<td><strong>Kezelése</strong> az alkalmazások központi telepítésének</td>
<td>Megnyílik a hello Azure BizTalk Services portálra. BizTalk szolgáltatások portálja hello hello be tooEDI konfiguráció – beleértve a partnerek hozzáadása és X12, AS2, létrehozása és EDIFACT-egyezmények.
<br/><br/>
Ez az azonos hello <strong>egyezmények létrehozása</strong> a hello <strong>gyors üzembe helyezés</strong> lapon.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> hello BizTalk szolgáltatások portálja nyújt részletesebb információt.</td>
</tr>
<tr>
<td><strong>Kapcsolat információi</strong> a hozzáférés-vezérlési Namespace hello</td>
<td>Megjeleníti a hello hozzáférés-vezérlési Namespace, alapértelmezett kibocsátó és a kulcs alapértelmezett értékek; amely másolhatók.
<br/><br/>
Nyissa meg a hozzáférés-vezérlési Portal hello is. A hozzáférés-vezérlő portál van hello ugyanaz, mint a hello bal oldali navigációs panelen hello Active Directory beállítás használatával.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Az ACS-Namespace kezelése</a> hello hozzáférés-vezérlési Portal nyújt részletesebb információt.</td>
</tr>
<tr>
<td><strong>Kulcsok szinkronizálása</strong> a hello Storage-fiók</td>
<td>Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs. A titkosítási kulcsokat a Tárfiók hozzáférési tooyour szabályozza. A BizTalk szolgáltatás automatikusan használ hello elsődleges kulcs. <strong>Kulcsok szinkronizálása</strong> engedélyezése a felhasználók tooswitch hello elsődleges kulcs és a másodlagos kulcs hello közötti hello BizTalk szolgáltatás megszakítása nélkül.
<br/><br/>
Például azt szeretné, hello BizTalk szolgáltatás toouse hello Tárfiók új elsődleges kulcsot. toodo ezt:
<br/><br/>
<ol>
<li>Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>. Válassza ki a hello másodlagos kulcsát. Ekkor a hello BizTalk szolgáltatás elindul, hello másodlagos kulcs használatával.</li>
<li>A klasszikus Azure portálon hello válassza ki a tárfiók, és hello elsődleges kulcs újragenerálása. Ne feledje, hogy a BizTalk szolgáltatás hello másodlagos kulcsot használja.</li>
<li>Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>. Jelölje ki, hello elsődleges kulcs. Ez az hello új elsődleges kulcs, akkor a rendszer újra létrehozza.</li>
<li>A klasszikus Azure portálon hello válassza ki a tárfiók, és hello másodlagos kulcs újragenerálása.</li>
</ol>
<br/>
A folyamat elnevezése "helyettesítő kulcsok". hello célja tooenable felhasználók tooswitch hello elsődleges kulcs és a másodlagos kulcs hello közötti hello BizTalk szolgáltatás megszakítása nélkül.</td>
</tr>

<tr>
<td><strong>Törlés</strong> az alkalmazás</td>
<td>A BizTalk szolgáltatás és az összes telepített elemek tooit törlődnek.</td>
</tr>
</table>


## <a name="monitor"></a>Figyelés
Nem érvényes toohello ingyenes kiadásban.

A BizTalk szolgáltatás neve kiválasztásakor hello figyelő lapon érhető el, és hello következő jeleníti meg:

##### <a name="metric-graph-displays-hello-selected-performance-metrics"></a>Metrika Graph: Megjeleníti hello kijelölt metrikák
Ezeket a mérési adja meg a valós idejű vonatkozó hello hello BizTalk szolgáltatás állapotát. Úgy dönt, hogy mely metrikák jelennek meg. Egyszerre legfeljebb hat teljesítménymutatók megjelenítése. 

Másik lehetőségként hello **relatív** vagy **abszolút** értékek és hello időtartománynak **időköz** hello metrikák jelennek meg a. 

##### <a name="tooremove-or-display-metrics-in-hello-graph"></a>a hello graph tooremove vagy a megjelenítési metrikák:
1. Jelölje be hello **figyelő** fülre.
2. Válassza ki **metrikák hozzáadása** hello tálcán:  
   ![Válassza ki a metrikák hozzáadása][AddMetrics]
3. Ellenőrizze azt szeretné, hogy toodisplay hello teljesítménymutatók.
4. Válassza ki a hello pipa tooreturn toohello **figyelő** fülre.
5. Válasszon hello kör következő toohello metrika toodisplay adott metrika érték hello gráfban.  
   
    Például hello **CPU-használat** metrika szürkén jelenik meg; kimenetét hello Graph nem jelenik meg:  
   ![CPU-használat metrika szürkén jelenik meg][GrayedMetric]  
   
    Jelölje be hello szürkén jelenik meg kör tooenable hello **CPU-használat** metrika toodisplay hello Graph kimenetét:  
   ![CPU-használat metrika engedélyezve van][EnabledMetric]
6. tooremove metrika hello megjelenítési graph és hello listájában válassza ki **törölje metrikát** hello tálcán. tooadd hello metrika hátsó toohello listáról válassza ki **metrikák hozzáadása** hello tálcán, ellenőrizze a hello metrika, és jelölje be hello pipa tooreturn toohello **figyelő** fülre. Jelölje be hello kör tooenable hello metrika használhatók.

## <a name="Metrics"></a>Elérhető
a következő számlálók/teljesítménymutatók hello érhetők el:

<table border="1">

<tr>
<td><strong>RountdTrip késés</strong></td>
<td>Megjeleníti a hello átlagos idő ezredmásodpercben (ms) tooprocess hello idő üdvözlőüzenetére üzenetét érkezik, amíg üdvözlőüzenetére teljesen hello BizTalk szolgáltatás közötti összes hidak dolgoz fel. Csak a sikeresen feldolgozott üzeneteinek bájtjai számítanak.<br/><br/>
A következő események hello fordulhat elő, amikor egy Timestamp típusú jön létre:
<ul>
<li>Üzenet kerül hello átjáró</li>
<li>A következő irányított toohello cél:</li>
<li>Cél választ</li>
<li>Cél nyugtázási küldött válaszok toohello átjáró</li>
</ul>
<br/>
Ez a mérőszám a következő számítási hello hello eredményét mutatja:
<br/><br/>
[Cél nyugtázási küldött válaszok toohello átjáró] - [üzenet kerül hello átjáró]</td>
</tr>
<tr>
<td><strong>Hibákat forrásnál</strong></td>
<td>Megjeleníti a sikertelen üzenetek teljes száma hello hello BizTalk szolgáltatás húzza hello forrás végpontok érkező üzenetek által.</td>
</tr>
<tr>
<td><strong>CPU-használat</strong></td>
<td>Hello átlagos processzoridő % található összes szerepkörpéldány sorolja fel.</td>
</tr>
<tr>
<td><strong>Feldolgozási késleltetés</strong></td>
<td>Megjeleníti a hello átlagos idő ezredmásodpercben (ms) tooprocess hello BizTalk szolgáltatás közötti összes hidak, kivéve a hello idő szerint üzenet töltött célok szükséges. Csak a sikeresen feldolgozott üzeneteinek bájtjai számítanak.<br/><br/>
Egyes események hello fordulhat elő, amikor egy Timestamp típusú jön létre:

<ul>
<li>Üzenet kerül hello átjáró</li>
<li>A következő irányított toohello cél:</li>
<li>Cél választ</li>
<li>Cél nyugtázási küldött válaszok toohello átjáró</li>
</ul>
<br/>Ez a mérőszám a következő számítási hello hello eredményét mutatja:<br/><br/>
[Cél nyugtázási küldött válaszok toohello átjáró] - [üzenet kerül hello átjáró] - [cél választ] + [üzenet irányított toohello cél]</td>
</tr>
<tr>
<td><strong>A folyamat sikertelen</strong></td>
<td>Hello sikertelen feldolgozása során hello BizTalk szolgáltatás közötti összes hello hidak időintervallumon belül-kezelési üzenetek teljes számát mutatja.</td>
</tr>
<tr>
<td><strong>Küldött üzenetek</strong></td>
<td>Megjeleníti egy adott időintervallumban belül hello BizTalk szolgáltatás közötti összes hidak által küldött üzenetek teljes száma hello. Ez a metrika értéke akkor növekszik, ha a folyamat által küldött üzenet eléri hello útvonal cél. Ez a metrika nem jelzi, hogy egy üzenet feldolgozása sikeresen.<br/><br/>
A kérelem-válasz forgatókönyvek a hello metrika értéke eggyel növekszik, hello útvonal cél üzenetet küld egy fogadását nyugtázási hátsó toohello folyamatot.</td>
</tr>
<tr>
<td><strong>A Beérkezett üzenetek</strong></td>
<td>Megjeleníti egy adott időintervallumban belül hello BizTalk szolgáltatás közötti összes hidak által fogadott üzenetek száma hello. Ez a metrika értéke akkor nő, amikor egy új üzenet jelenik meg az adatcsatorna hello.</td>
</tr>
<tr>
<td><strong>A folyamat üzenetek</strong></td>
<td>Megjeleníti a hello időintervallumon belül hello BizTalk szolgáltatás által jelenleg feldolgozott üzenetek teljes száma.</td>
</tr>
<tr>
<td><strong>Az üzenetek feldolgozásának</strong></td>
<td>Megjeleníti a hello időintervallumon belül hello BizTalk szolgáltatás közötti összes hidak sikeresen feldolgozott üzenetek teljes száma. Ez a metrika értéke akkor nő, amikor egy üzenet sikeresen megkapta hello feldolgozási sorban lévő és sikeresen irányított toohello cél.</td>
</tr>
</table>


## <a name="scale"></a>Méretezés
Hello méretezési lapon adhat hozzá vagy a BizTalk szolgáltatás által használt egységek száma hello kivonandó időnél. Alapértelmezés szerint van egy egység konfigurálva. További egységek tooscale lehet hozzáadni a BizTalk szolgáltatás. Hello méretezési növelésével, amelyek növelve az adatátviteli sebességet. hello mennyiségű erőforrást is növeli, beleértve a telepített hidak, a megállapodások, a LOB-kapcsolatok, és feldolgozási kapacitása révén. Például növelheti hello méretezési 1 egység too2 egységektől. Ebben a helyzetben telepítése hidak, kettős hello megállapodások, kettős hello LOB kapcsolatok és dupla hello feldolgozási teljesítmény dupla hello száma.

BizTalk kiadásaiban nem képes a skálázási beállítást. Ebben a helyzetben egy egység engedélyezett. toodetermine darabszám a edition méretezhetők, tekintse meg a túl[BizTalk szolgáltatások: kiadások diagram](biztalk-editions-feature-chart.md).

Hello egységek számának növelése hatással lehet a díjszabás. Ha hello egységek növeléséhez kiválasztása **mentése** egy üzenetben arra kéri, ha a számlázási kihatással van. Ezután válasszon ki toocontinue. Hello egységek számának növelésével hello BizTalk szolgáltatás állapota aktív tooUpdating módosítja. A hello frissítési állapot a BizTalk szolgáltatás toorun továbbra is.

[BizTalk szolgáltatások: Kiadások diagram](biztalk-editions-feature-chart.md) "egység".

## <a name="configure"></a>Konfigurálás
Nem érvényes tooHybrid kapcsolatok.

Beállítja a biztonsági mentés állapotának tooNone hello vagy automatikus. Ha beállítása tooNone, nem készült biztonsági másolat automatikusan jönnek létre. Ha beállítása tooAutomatic, hello biztonsági mentési helyre, hello gyakorisága hello biztonsági másolat, és mennyi ideig tookeep hello biztonságimásolat-fájlok konfigurálásához. 

[BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](biztalk-backup-restore.md) hello részletes adatokat biztosít. 

## <a name="HybridConnections"></a>Hibrid kapcsolatok
Hibrid kapcsolatok csatlakoztatása az Azure-alkalmazások, például webalkalmazások vagy a Mobile Apps az Azure App Service szolgáltatásban a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatásokat használó tooan helyszíni erőforráshoz. Hibrid kapcsolatok a BizTalk szolgáltatások felügyelete az hello a klasszikus Azure portálon.

toocreate hibrid kapcsolatok az Azure App Service szolgáltatásban, lásd: [hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).

toocreate vagy az Azure BizTalk szolgáltatások hibrid kapcsolatok kezelése című [hibrid kapcsolatok](integration-hybrid-connection-overview.md).

## <a name="next"></a>Következő lépés
Most, hogy ismeri hello különböző lappal, részletesebben hello Azure BizTalk szolgáltatások szolgáltatásairól:

* [BizTalk Services: Szabályozás](biztalk-throttling-thresholds.md)  
* [BizTalk Services: Kiállító neve és kiállító kulcsa](biztalk-issuer-name-issuer-key.md)  
* [BizTalk Services: Biztonsági mentés és visszaállítás](biztalk-backup-restore.md)

## <a name="see-also"></a>Lásd még:
* [Hibrid kapcsolatok](integration-hybrid-connection-overview.md)  
* [BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram](biztalk-editions-feature-chart.md)  
* [BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál](biztalk-provision-services.md)  
* [BizTalk szolgáltatások: BizTalk szolgáltatás állapotának diagram](biztalk-service-state-chart.md)  
* [Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

