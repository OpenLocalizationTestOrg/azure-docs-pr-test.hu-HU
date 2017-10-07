---
title: "BizTalk szolgáltatások kiadások szolgáltatásairól aaaLearn |} Microsoft Docs"
description: "Hasonlítsa össze a hello képességek hello BizTalk szolgáltatások kiadásainak: ingyenes, fejlesztői, Basic, Standard és Premium. MABS, WABS."
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c589629f-06b1-44bb-b8ca-1db71826ea59
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 81626fa743a7190e7c78a0fd90b3054a08982b02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-editions-chart"></a>BizTalk Services: Kiadások diagramja

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Az Azure BizTalk Services számos kiadást biztosít. Ez a cikk toodetermine melyik kiadást az a forgatókönyv és üzleti igényeinek megfelelő használja.

## <a name="compare-hello-editions"></a>Hello kiadások összehasonlítása
**Ingyenes (előzetes verzió)**

Hibrid kapcsolatokat hozhat létre és felügyelhet. A hibrid kapcsolat egy egyszerűen tooconnect egy Azure-webhelyen tooan a helyszíni rendszer, például az SQL Server nem.

**Fejlesztői**

Hibrid kapcsolatokat, a kereskedelmi partnerek felügyeletére szolgáló, könnyen használható portállal feldolgozható EAI- és EDI-üzenetfeldolgozást, az általános EDI-sémák támogatását, valamint az X12-n és AS2-n keresztüli részletes EDI-feldolgozást tartalmaz. Létrehozhat hello felhőbeli szolgáltatások összekapcsolása a HTTP/S, a többi, a FTP, a WCF és az SFTP protokoll tooread szabhatják EAI-Összekötők és írásakor.  Kapcsolat tooon helyszíni LOB rendszerek hasznosítani használatra kész SAP, Oracle eBusiness, Oracle adatbázis, Siebel, és az SQL Server-adapterrel. Fejlesztőközpontú környezetet használhat Visual Studio-eszközökkel az egyszerű fejlesztés és üzembe helyezés érdekében. Korlátozott toodevelopment és tesztelési célú csak a nem szolgáltatási szint szerződés (SLA).

**Basic**

Hibrid kapcsolatok, EAI-Összekötők hidak, EDI-szerződések, és a BizTalk Adapter Pack kapcsolatok hello fejlesztői funkciói növeli a legtöbb tartalmazza. Magas rendelkezésre állású, és hello beállítás tooscale egy szolgáltatási szint szerződés (SLA) rendelkező is kínál.

**Standard**

Minden hello alapvető funkciói növeli a hibrid kapcsolatok, EAI-Összekötők hidak, EDI-szerződések, és a BizTalk Adapter Pack kapcsolatok tartalmazza. Magas rendelkezésre állású, és hello beállítás tooscale egy szolgáltatási szint szerződés (SLA) rendelkező is kínál.

**Prémium**

Minden hello szabványos funkciói növeli a hibrid kapcsolatok, EAI-Összekötők hidak, EDI-szerződések, és a BizTalk Adapter Pack kapcsolatok tartalmazza. Archiválás, a magas rendelkezésre állás és a hello beállítás tooscale az egy szolgáltatási szint szerződés (SLA) is tartalmaz.

## <a name="editions-chart"></a>Kiadások diagramja
hello következő táblázatban hello eltéréseket.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Ingyenes (előzetes verzió)</th>
        <th>Fejlesztői</th>
        <th>Alapszintű</th>
        <th>Standard</th>
        <th>Prémium</th>
</tr>

<tr>
<td><strong>Kezdő ár</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Az Azure BizTalk Services árképzése</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure díjkalkulátor</a></td>
</tr>
<tr>
<td><strong>Alapértelmezett minimális konfiguráció</strong></td>
<td>1 ingyenes egység</td>
<td>1 fejlesztői egység</td>
<td>1 alapszintű egység</td>
<td>1 standard egység</td>
<td>1 prémium egység</td>
</tr>
<tr>
<td><strong>Méretezés</strong></td>
<td>Nincs méretezés</td>
<td>Nincs méretezés</td>
<td>Igen, 1 alapszintű egységnyi növekményekben</td>
<td>Igen, 1 standard egységnyi növekményekben</td>
<td>Igen, 1 prémium egységnyi növekményekben</td>
</tr>
<tr>
<td><strong>Maximális engedélyezett horizontális felskálázás</strong></td>
<td>Nincs méretezés</td>
<td>Nincs méretezés</td>
<td>Környezet too8 egység</td>
<td>Környezet too8 egység</td>
<td>Környezet too8 egység</td>
</tr>
<tr>
<td><strong>EAI-hidak egységenként</strong></td>
<td>Nem tartalmazza</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI, AS2</strong>
<br/><br/>
TPM-egyezményeket tartalmaz</td>
<td>Nem tartalmazza</td>
<td>Tartalmazza 10 egyezmény egységenként.</td>
<td>Tartalmazza 50 egyezmény egységenként.</td>
<td>Tartalmazza 250 egyezmény egységenként.</td>
<td>Tartalmazza 1000 egyezmény egységenként.</td>
</tr>
<tr>
<td><strong>Hibrid kapcsolatok egységenként</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hibrid kapcsolat adatátvitele (GB) egységenként</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>BizTalk szolgáltatás kapcsolatok tooon helyszíni LOB rendszerek</strong></td>
<td>Nem tartalmazza</td>
<td>1 kapcsolat</td>
<td>2 kapcsolat</td>
<td>5 kapcsolat</td>
<td>25 kapcsolat</td>
</tr>
<tr>
<td align="left"><strong>Támogatott protokollok/rendszerek:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Service Bus (SB)</li>
<li>Azure-blob</li>
<li>REST API-k</li>
</ul>
</td>
<td>Nem tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
</tr>
<tr>
<td><strong>Magas rendelkezésre állás</strong>
<br/><br/>
A szolgáltatói szerződést (SLA) lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Az Azure BizTalk Services árképzése</a>.
</td>
<td>Nem tartalmazza</td>
<td>Nem tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
</tr>
<tr>
<td><strong>Biztonsági mentés és visszaállítás</strong></td>
<td>Nem tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
</tr>
<tr>
<td><strong>Nyomon követés</strong></td>
<td>Nem tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
</tr>
<tr>
<td><strong>Archiválás</strong><br/><br/>
Tartalmazza a számla letagadhatatlanságát (NRR) és a követett üzenetek letöltését</td>
<td>Nem tartalmazza</td>
<td>Tartalmazza</td>
<td>Nem tartalmazza</td>
<td>Nem tartalmazza</td>
<td>Tartalmazza</td>
</tr>
<tr>
<td><strong>Egyéni kód használata</strong></td>
<td>Nem tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
</tr>
<tr>
<td><strong>Átalakítások használata, beleértve az egyéni XSLT-t</strong></td>
<td>Nem tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
<td>Tartalmazza</td>
</tr>
</table>

> [!NOTE]
> A hardverhibák elleni rugalmasság érdekében a magas rendelkezésre állás azt jelenti, hogy több virtuális gép van egyetlen BizTalk-egységben.
> 
> 

## <a name="faqs"></a>Gyakori kérdések
#### <a name="what-is-a-biztalk-unit"></a>Mi a BizTalk-egység?
Egy "egység" hello atomi szint Azure BizTalk szolgáltatások üzembe helyezésének. Mindegyik kiadás egységének számítási kapacitása és memóriája különböző. Az alapszintű egység például több számításra képes, mint a fejlesztői, a standard több számításra képes, mint az alapszintű stb. A BizTalk Service méretezése egységekben történik.

#### <a name="what-is-hello-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Mi az a BizTalk szolgáltatások és az Azure BizTalk VM hello különbségének?
BizTalk szolgáltatások integrációs megoldásokat hello felhőben készítéséhez igaz Platform,--szolgáltatás (PaaS) architektúrát nyújt. Hello PaaS modellel teljesen összpontosítani hello úgy az alkalmazáslogikát, és hagyja meg az összes hello infrastruktúra felügyeleti tooMicrosoft, beleértve:

* Nincs szükség toomanage vagy javítás virtuális gépek.
* A Microsoft biztosítja a rendelkezésre állást.
* Igény szerinti kérésével egyszerűen több vagy kevesebb kapacitás hello Azure-portálon keresztül méretezési szabályozhatja.

Az Azure Virtual Machinesben futó BizTalk Server IaaS-architektúrát biztosít. Hozzon létre virtuális gépeket, és úgy konfigurálja azokat, akárcsak a helyszíni környezetben, így könnyebben toorun meglévő alkalmazások hello felhőben kód változtatás nélkül. Az infrastruktúra-szolgáltatási, továbbra is való telepítésért felelős hello a virtuális gépek konfigurálása hello virtuális gépek (például a szoftverek telepítéséhez és az operációsrendszer-javítások), és kezelésére újratervezni a magas rendelkezésre állású hello alkalmazás.

A BizTalk Services segítségével az infrastruktúra kezelését megkönnyítő integrációs megoldásokat készíthet. Ha a tooquickly telepítse át a meglévő BizTalk-megoldásokról vagy egy igény szerinti környezetet toodevelop és a BizTalk Server tesztelési keresése alkalmazások, BizTalk Server az Azure virtuális gép.

#### <a name="what-is-hello-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Mi az a BizTalk szolgáltatás és a hibrid kapcsolatok hello különbségének?
egy Azure BizTalk szolgáltatás által használt hello BizTalk szolgáltatás. BizTalk szolgáltatás hello hello BizTalk Adapter Pack tooconnect tooan helyszíni sor az üzleti (LOB) rendszert használ. A hibrid kapcsolat biztosít egy egyszerű és kényelmesen tooconnect Azure-alkalmazások, mint az Azure App Service Web Apps-szolgáltatás hello és Azure Mobile Services tooan helyszíni erőforrás.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-hello-limit-is-reached"></a>Mit jelent a „hibrid kapcsolat adatátvitele (GB) egységenként”? Ez percenként/óránként/naponta/hetente/havonta értendő? Mi történik, amikor hello a határt?
a hibrid kapcsolat egységköltség hello hello BizTalk szolgáltatások edition függ. A költségek egyszerűen attól függnek, hogy mennyi adatot visz át. Napi 10 GB adat átvitele például kevesebbe kerül, mint napi 100 GB adat átvitele. Használjon hello [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/?scenario=full) BizTalk szolgáltatások toodetermine adott költségek. Általában hello vannak korlátozva naponta. Ha hello korláton bármely túlhasználati díjfizetéssel 1 USD / GB hello sebessége.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-hello-number-of-bridges-go-up-by-two-instead-of-just-one"></a>BizTalk szolgáltatások létrehozásakor egy szerződést, miért nem hidak hello száma lépjen két helyett csak egy?
Minden egyezmény két különböző hídból áll: egy küldőoldali kommunikációs hídból és egy fogadóoldali kommunikációs hídból.

#### <a name="what-happens-when-i-hit-hello-quota-limit-on-hello-number-of-bridges-or-agreements"></a>Mi történik, ha szeretnék találati hello kvótakorlátot hidak vagy megállapodások hello számára?
Nem sikerült toodeploy bármely új hidak, vagy hozzon létre új szerződéseket. További toodeploy tooscale toomore egységek hello BizTalk szolgáltatás, vagy a frissítési tooa magasabb edition kell.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-tooanother"></a>Hogyan tooanother BizTalk szolgáltatás egy rétegének át?
hello ingyenes kiadás nem telepíthető át, és a "kiterjesztett" tooanother réteg, és nem készíthető biztonsági másolat és tooanother szint visszaállítása. Ha egy másik réteghez, hello új réteget használó új BizTalk szolgáltatás létrehozása. Hello ingyenes kiadás, beleértve a hibrid kapcsolatok, újból létre kell toobe használatával létrehozott összetevőihez hello új BizTalk szolgáltatás. 

Hello fennmaradó kiadás esetén hello biztonságimásolat-készítő és az összetevők áttelepítéséhez egy réteg tooanother visszaállítása. Például biztonsági mentését a összetevők hello normál rétegben, és hajtsa végre a visszaállítást toohello prémium csomagban. [BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](biztalk-backup-restore.md) ismerteti a hello támogatott áttelepítési útvonalakat, és felsorolja, milyen összetevőihez készül biztonsági másolat. Vegye figyelembe, hogy a hibrid kapcsolatokról nem készül biztonsági másolat. Miután biztonsági mentése és visszaállítása tooa új réteget, majd ismételt létrehozásakor hello hibrid kapcsolatok.  

#### <a name="is-hello-biztalk-adapter-service-included-in-hello-service-how-do-i-receive-hello-software"></a>Hello BizTalk szolgáltatás szerepel hello szolgáltatást? Hogyan jelenik meg hello szoftver?
Igen, a BizTalk Adapter Pack hello BizTalk szolgáltatás hello érhetők el a hello Azure BizTalk szolgáltatások SDK [letöltése](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Következő lépések
toocreate Azure BizTalk szolgáltatások az Azure portálon lépjen túl hello[BizTalk szolgáltatások: Azure-portálon használó telepítés hello](biztalk-provision-services.md). alkalmazások, lépjen túl létrehozásának toostart[Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>További források
* [BizTalk szolgáltatások: Kiépítés hello Azure-portál használatával](biztalk-provision-services.md)<br/>
* [BizTalk Services: Kiépítési állapot diagramja](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Irányítópult, Figyelés és Méret lapok](biztalk-dashboard-monitor-scale-tabs.md)<br/>
* [BizTalk Services: Biztonsági mentés és visszaállítás](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Szabályozás](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Kiállító neve és kiállító kulcsa](biztalk-issuer-name-issuer-key.md)<br/>
* [Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>

