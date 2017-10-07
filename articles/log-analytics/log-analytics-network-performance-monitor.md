---
title: "Teljesítményfigyelő-megoldás az Azure Naplóelemzés aaaNetwork |} Microsoft Docs"
description: "Hálózati Teljesítményfigyelő az Azure Log Analytics segítségével megfigyelheti a hello teljesítményét a hálózatok a közelében real egyszeri toodetect, és keresse meg a hálózati szűk keresztmetszetek."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.openlocfilehash: e074948221fdd003c640861d759c4ce69ced1cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-performance-monitor-solution-in-log-analytics"></a>A hálózati Naplóelemzési Teljesítményfigyelő megoldás

![Hálózati Teljesítményfigyelő szimbólum](./media/log-analytics-network-performance-monitor/npm-symbol.png)

Ez a dokumentum azt ismerteti, hogyan tooset felfelé és -felhasználási hello Naplóelemzési, amely segít a hálózatok a közelében real egyszeri toodetect hello teljesítmény figyeléséhez, és keresse meg a hálózati teljesítmény szűk hálózati Teljesítményfigyelő megoldás. Hello hálózati Teljesítményfigyelő megoldás segítségével figyelheti hello veszteséget és késéseket két hálózatot, alhálózatot vagy a kiszolgálók között. Hálózati teljesítmény a figyelő észleli, például a forgalom blackholing útválasztási hibák és problémák, győződjön meg arról, hogy a hagyományos hálózati figyelési módszerek nem tud toodetect legyenek a hálózati problémák. Hálózati Teljesítményfigyelő riasztásokat állít elő, és értesíti, amikor a küszöbérték megsérül, a hálózati kapcsolat. Ezeket a küszöbértékeket is ismernie kell automatikusan hello rendszer, vagy konfigurálhatja azokat toouse egyéni riasztási szabályok. Hálózati Teljesítményfigyelő időben történő észlelése érdekében a hálózati teljesítményproblémák biztosítja, és localizes hello probléma tooa hálózati szegmens vagy az eszköz hello forrását.

Hello megoldás-irányítópultot, amely a legutóbbi hálózati állapotával kapcsolatos események, nem megfelelő állapotú hálózati kapcsolatok és alhálózati kapcsolat, amely használata során szembesülnek magas csomag veszteséget és késéseket hálózati összefoglaló információkat jelenít meg a hálózati problémák észlelését. Akkor is leásási azokat a hálózati kapcsolat tooview hello aktuális állapotát alhálózati kapcsolat, valamint a csomópontok hivatkozásokat. Hello korábbi trendjét veszteséget és késéseket hello hálózati, az alhálózat és a csomópontok szintjén is megtekintheti. Átmeneti hálózati problémák észlelése korábbi trend diagramok csomag veszteséget és késéseket megtekintésével, és keresse meg a hálózati szűk keresztmetszeteket topológia-térképként. hello interaktív topológia graph lehetővé teszi a toovisualize hello Ugrás által Ugrás hálózati útvonalak és hello hello probléma forrása határozza meg. Más megoldások, például használhatja a naplóban keresse meg a különböző analytics követelmények toocreate egyéni jelentések hálózati Teljesítményfigyelő által összegyűjtött hello adatok alapján.

hello megoldás szintetikus tranzakciók használja, mint egy elsődleges mechanizmus toodetect hálózati hibák. Ebben az esetben használhatja egy adott hálózati eszköz szállítójával, vagy a modell tekintet nélkül. A helyszíni, a felhőben (IaaS) és a hibrid környezetek között működik. hello megoldás automatikusan felderíti a hello hálózati topológia és a hálózat különböző útvonalak.

Tipikus hálózati figyelési termékek helyezi a hangsúlyt hello figyelési hálózati eszköz (útválasztók, kapcsolók stb.) állapotát, de nem ad meg hálózati Teljesítményfigyelő viszont két pont közötti hálózati kapcsolat tényleges minőségének hello betekintést.

### <a name="using-hello-solution-standalone"></a>Hello megoldás önálló használata
Ha azt szeretné, hogy a kritikus fontosságú munkaterhelésekhez közötti hálózati kapcsolat toomonitor hello minőségének, hálózatok, adatközpontok vagy office helyek, akkor használhatja hello hálózati Teljesítményfigyelő megoldás önmagában toomonitor kapcsolat állapota között:

* több adatközpontban vagy office helyre, egy nyilvános vagy privát hálózaton keresztül csatlakozó
* kritikus fontosságú munkaterhelésekhez, üzletági alkalmazásokat futtató
* nyilvános felhő szolgáltatásokat, mint a Microsoft Azure vagy az Amazon Web Services (AWS) és a helyszíni hálózatokhoz, ha IaaS (VM) rendelkezésre álló és átjárók van konfigurálva a helyszíni hálózatokhoz és a felhőalapú hálózatokhoz tooallow kommunikációját
* Azure és a helyszíni hálózat Express Route használatakor

### <a name="using-hello-solution-with-other-networking-tools"></a>Hello megoldással rendelkező más hálózati eszközök
Ha azt szeretné, hogy toomonitor egy üzletági alkalmazás, egy kiegészítő megoldás tooother hálózati eszközök hello hálózati Teljesítményfigyelő megoldás is használhatja. A lassú hálózati tooslow alkalmazások vezethet, és hálózati Teljesítményfigyelő segítségével megvizsgálhatja a mögöttes hálózati probléma miatt fellépő alkalmazás teljesítményproblémákat. Hello megoldás nem igényel hozzáférést toonetwork eszközök, mert a hello alkalmazás-rendszergazda nem szükséges a hálózati csoport tooprovide információ a hogyan hello hálózati alkalmazások érinti toorely.

Is ha már más hálózati eszközök figyelési fektet, majd hello megoldás is kiegészíteni ezekhez az eszközökhöz, mert a hagyományos hálózati figyelési megoldások nem betekintést teljesítménymutatók végpontok közötti hálózati veszteségeket és késéseket hasonlóan.  hello hálózati Teljesítményfigyelő megoldás segítségével töltse ki az adott térközét.

## <a name="installing-and-configuring-agents-for-hello-solution"></a>Hello megoldás ügynökök telepítése és konfigurálása
Hello basic folyamatok tooinstall ügynökök, segítségével [csatlakozás Windows számítógépek tooLog Analytics](log-analytics-windows-agents.md) és [csatlakozás az Operations Manager tooLog Analytics](log-analytics-om-agents.md).

> [!NOTE]
> Kell kell rendelés toohave tooinstall legalább 2-ügynökök elég adat toodiscover és figyelheti a hálózati erőforrásokhoz. Ellenkező esetben hello megoldás marad konfigurálása állapotban telepítése és konfigurálása további ügynökök.
>
>

### <a name="where-tooinstall-hello-agents"></a>Ha tooinstall hello ügynökök
Ügynökök telepítése előtt fontolja meg a hálózat topológiája hello és hello részeinek hálózati azt szeretné, hogy toomonitor. Azt javasoljuk, hogy az egyes alhálózatokon, amelyet az toomonitor egynél több ügynök telepítése. Más szóval miden alhálózatában, amelyet az toomonitor, válassza a kiszolgálók vagy a virtuális gépek két vagy több, és hello ügynök telepítését.

Ha biztos a hálózat topológiája hello, telepítse a hello ügynököket a kritikus fontosságú munkaterhelésekhez, ha azt szeretné, hogy toomonitor hello hálózati teljesítmény kiszolgálókon. Például érdemes tookeep nyomon követése a hálózati kapcsolat a webkiszolgáló és egy SQL Servert futtató kiszolgáló között. Ebben a példában kellene egy ügynököt telepít mindkét kiszolgálón.

Ügynökök közötti hálózati kapcsolat megfelelő (hivatkozások) állomások nem maguk hello gazdagépek figyelésére. Igen, toomonitor egy hálózati kapcsolat mindkét végpont, hogy a hivatkozás ügynököket kell telepítenie.

### <a name="configure-agents"></a>Ügynökök konfigurálása

Ha azt tervezi, toouse hello ICMP protokoll szintetikus tranzakciók, a következő tűzfalszabályok megbízhatóan okhoz ICMP tooenable hello szüksége:

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow
```


Ha azt tervezi, hogy a TCP protokoll toouse hello ezen ügynökök kommunikálni képes számítógép tooensure kell tooopen tűzfal portjai. Toodownload kell, és futtassa a hello [EnableRules.ps1 PowerShell-parancsfájl](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) egy PowerShell-ablakban rendszergazdai jogosultságokkal rendelkező paraméterek nélkül.

hello parancsfájl hello hálózati Teljesítményfigyelő szükséges beállításkulcsot hoz létre, és azt a Windows tűzfal szabályai tooallow ügynökök toocreate TCP-kapcsolatot hoz létre egymással. hello parancsfájl által létrehozott hello beállításkulcsok azt is megadhatja, hogy toolog hello hibakeresési naplókat, valamint hello hello naplók fájl elérési útját. Hello TCP-port ügynök-kommunikációhoz használt is meghatározza. hello ezek a kulcsok automatikusan értéke hello parancsfájl, így nem kell manuálisan módosítani ezeket a kulcsokat.

Alapértelmezés szerint megnyitott hello port 8084. Egyéni portot is használhat hello paraméter megadásával `portNumber` toohello parancsfájl. Azonban hello ugyanazt a portot kell használni minden hello számítógépen ahol hello parancsfájlt futtatja.

> [!NOTE]
> hello EnableRules.ps1 parancsfájl hello számítógép, amelyen hello parancsfájl futtatása csak a Windows tűzfal-szabályokat konfigurálja. Ha egy hálózati tűzfalat, meg kell győződnie arról, hogy lehetővé teszi a hálózati teljesítményt figyelő által használt TCP-port hello adatforgalmat.
>
>

## <a name="configuring-hello-solution"></a>Hello megoldás konfigurálása
A következő információk tooinstall hello használja, és hello megoldás konfigurálása.

1. hello hálózati Teljesítményfigyelő megoldás szerez be adatok a Windows Server 2008 SP 1 vagy újabb rendszerű számítógépek vagy a Windows 7 SP1 vagy újabb, amely vannak hello ugyanaz a Microsoft Monitoring Agent (MMA) hello követelményekkel. NPM ügynökök is futtathatja a Windows asztali/ügyfél operációs rendszereken (Windows 10, Windows 8.1, Windows 8 és Windows 7).
    >[!NOTE]
    >hello ügynökök a Windows server operációs rendszerek támogatják a TCP és az ICMP hello protokollok szintetikus tranzakció. Azonban a Windows ügyfélalapú operációs rendszerekbe hello ügynökök csak támogatják az ICMP hello protokoll, szintetikus tranzakció.

2. Hello hálózati Teljesítményfigyelő megoldás tooyour munkaterület hozzáadása [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).  
   ![Hálózati Teljesítményfigyelő szimbólum](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. Az hello OMS-portálon, megjelenik egy új csempe jelenik **hálózati Teljesítményfigyelő** hello üzenettel *megoldás további konfigurálást igényel*. Tooconfigure hello megoldás tooadd hálózatok alhálózatokra és -ügynökök által felderített csomópontok alapján lesz szüksége. Kattintson a **hálózati Teljesítményfigyelő** toostart hello alapértelmezett hálózat konfigurálása.  
   ![megoldás további konfigurációt igényel](./media/log-analytics-network-performance-monitor/npm-config.png)

### <a name="configure-hello-solution-with-a-default-network"></a>Egy alapértelmezett hálózati hello megoldás konfigurálása
A hello konfiguráció lapon látni fogja, egy egyetlen hálózati nevű **alapértelmezett**. Ha olyan hálózatra, még nem aktív, minden hello automatikusan felderített alhálózat hello alapértelmezett hálózati kerülnek.

Amikor hálózatot hoz létre, hozzáadhat egy alhálózat tooit és alhálózaton eltávolítják az alapértelmezett hálózati hello. Ha törli a hálózaton, az összes alhálózatot automatikusan is megjelennek a toohello alapértelmezett hálózati.

Más szóval a hello alapértelmezett hálózati egy hello tároló minden hello alhálózat nem található a felhasználó által definiált hálózati. Nem szerkeszthetők vagy hello alapértelmezett hálózat törlése. Mindig hello rendszer marad. Azonban létrehozhat annyi hálózatok csak szeretne.

A legtöbb esetben a szervezet hello alhálózatok kell összeállítani, egynél több hálózati és hozzon létre egy vagy több hálózatok toologically csoportban az alhálózatok.

### <a name="create-new-networks"></a>Új hálózatok létrehozása
A hálózat hálózati Teljesítményfigyelőben egy olyan tároló, alhálózatok esetében. A hálózat bármely nevű szeretne, és adja hozzá az alhálózatok toohello hálózati hozhat létre. Például létrehozhat egy hálózati nevű *épület1* és adja meg az alhálózatot, vagy létrehozhat egy hálózati nevű *DMZ* majd toodemilitarized zóna toothis hálózati tartozó összes alhálózatot.

#### <a name="toocreate-a-new-network"></a>egy új hálózati toocreate
1. Kattintson a **Hozzáadás hálózati** és írja be a hello hálózati nevet és leírást.
2. Válassza ki egy vagy több alhálózatból, és kattintson a **Hozzáadás**.
3. Kattintson a **mentése** toosave hello konfigurációs.  
   ![hálózat hozzáadása](./media/log-analytics-network-performance-monitor/npm-add-network.png)

### <a name="wait-for-data-aggregation"></a>Várjon, amíg adatösszesítés
Először hello konfigurációs mentett, hello megoldás elindul hello csomópontok, amelyen telepítve vannak-e az ügynökök közötti hálózati csomag veszteséget és késéseket adatok gyűjtése. Ez a folyamat eltarthat egy ideig, néha 30 perc. Ebben az állapotban során hello hálózati Teljesítményfigyelő csempe hello – Áttekintés lapon megjeleníti a következő üzenet *adatösszesítés folyamat*.

![adatösszesítés folyamatban](./media/log-analytics-network-performance-monitor/npm-aggregation.png)

Hello adatok feltöltése helyezésekor itt látható hello hálózati Teljesítményfigyelő csempe frissítése ábrázoló adatokat.

![Hálózati teljesítmény figyelése csempe](./media/log-analytics-network-performance-monitor/npm-tile.png)

Kattintson a hello csempe tooview hello hálózati Teljesítményfigyelő irányítópult.

![Hálózati teljesítmény figyelő irányítópult](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Alhálózatok figyelési beállításainak szerkesztése
Ha legalább egy ügynök telepítése minden alhálózat szerepel hello **alhálózatai** lapon hello konfigurálása lapon.

#### <a name="tooenable-or-disable-monitoring-for-particular-subnetworks"></a>tooenable vagy adott alhálózatai a figyelés letiltásakor
1. Válassza ki, vagy törölje a jelet hello mezőben következő toohello **produkáló alhálózati azonosító** és ellenőrizze, hogy **figyelés használható** kijelölt vagy üres, szükség. Válassza ki, vagy törölje a több alhálózattal. Le van tiltva, az alhálózatok nem mert hello ügynökök más ügynökök pingelés frissített toostop lesz figyelve.
2. Válassza ki a megjeleníteni kívánt toomonitor az adott alhálózat hello lista és a áthelyezése hello alhálózat kiválasztásával hello csomópontok hello szükséges csomópontokat hello listák tartalmazó nem figyelt és a felügyelt csomópontok között.
   Hozzáadhat egy egyéni **leírás** toohello produkáló alhálózati, ha szeretné.
3. Kattintson a **mentése** toosave hello konfigurációs.  
   ![alhálózatának szerkesztése](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-toomonitor"></a>Válassza ki a csomópontok toomonitor
Hello csomópontjaihoz, amelyeken az ügynököt futtató hello szereplő **csomópontok** fülre.

#### <a name="tooenable-or-disable-monitoring-for-nodes"></a>tooenable vagy csomópontot a figyelés letiltásakor
1. Válassza ki, vagy törölni szeretné, hogy toomonitor, vagy a figyelés leállításának hello csomópontok.
2. Kattintson a **figyelés használható**, vagy törölje a jelölést, szükség szerint.
3. Kattintson a **Save** (Mentés) gombra.  
   ![csomópont figyelésének engedélyezése](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)

### <a name="set-monitoring-rules"></a>Állapotfigyelési szabályainak beállítása
Hálózati Teljesítményfigyelő állapotával kapcsolatos események két csomópont vagy a küszöbérték megsérül alhálózat vagy hálózati hivatkozások között hello címtárkapcsolatra vonatkozó állít elő. Ezeket a küszöbértékeket is ismernie kell automatikusan hello rendszer, vagy konfigurálhatja azokat egyéni riasztási szabályok.

Hello *alapértelmezett szabály* hello rendszer hozta létre, és létrehoz egy egészségügyi eseményt, ha a veszteség vagy bármely pár hálózatok vagy alhálózat közötti késés hivatkozásait megszegése hello rendszer megtanulta küszöbértéket. Válasszon toodisable hello alapértelmezett szabályt, és egyéni ellenőrzési szabályokat létrehozni

#### <a name="toocreate-custom-monitoring-rules"></a>egyéni szabályok figyelési toocreate
1. Kattintson a **szabály hozzáadása** a hello **figyelő** lapra, és írja be a hello szabály nevét és leírását.
2. Válassza ki a hálózati vagy alhálózat hivatkozások toomonitor hello pár hello listákról.
3. Először melyik hello az első alhálózat/s érdeklő található hello hálózati hello hálózati legördülő menüből, majd válassza ki és hello alhálózat/s hello megfelelő alhálózati legördülő menüből.
   Válassza ki **összes alhálózatai** Ha azt szeretné, hogy toomonitor összes hello alhálózatai egy hálózati kapcsolat. Hasonlóképpen válassza hello más alhálózat/s iránt. És kattinthat **vegye fel kivételhiba** hajtott végre tooexclude figyelés, az adott alhálózat hivatkozásokat biztosít a hello kijelölt elemek közül.
4. ICMP és TCP protokollok a szintetikus tranzakciók végrehajtása választhat.
5. Ha toocreate állapotával kapcsolatos események választott hello elemek nem szeretné, majd törölje a jelet **állapotfigyelés hello hivatkozások fedi le ez a szabály engedélyezése**.
6. Válassza ki a feltételek figyelése.
   Adhatja meg egyéni küszöbökkel állapotát az eseménygenerálás írja be a küszöbértékekhez. Hello feltétel hello értéke a megadott küszöbértéknél hello kijelölt hálózati/alhálózat pár fölé megy, amikor a rendszerállapot esemény jön létre.
7. Kattintson a **mentése** toosave hello konfigurációs.  
   ![Egyéni figyelési szabály létrehozása](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)

Figyelési szabály mentése után integrálható, hogy a szabály Riasztáskezelési kattintva **riasztás létrehozása**. Riasztási szabály automatikusan létrejön a hello keresési lekérdezés és más szükséges paramétereket automatikusan kitöltött-a. Riasztási szabályt használ, is fogadhatja az e-mail alapú értesítések, továbbá NPM belül toohello meglévő riasztást. Riasztások is elindítható a runbookok javító műveleteket, vagy azok integrálható a meglévő szolgáltatási felügyeleti megoldások webhookok használatával. Kattinthat **kezelése riasztás** tooedit hello riasztási beállításokat.

### <a name="choose-hello-right-protocol-icmp-or-tcp"></a>Válassza ki a hello jobb protokoll-ICMP vagy TCP

Hálózati teljesítmény figyelése (NPM) szintetikus tranzakciók toocalculate hálózati teljesítmény metrikákat például a csomag és a kapcsolat késési használ. toounderstand ez jobban, fontolja meg a hálózati kapcsolat NPM ügynök csatlakoztatott tooone végpontot. NPM ügynök küld mintavételi csomagok tooa második NPM ügynök csatlakoztatott tooanother ennek hello hálózat. hello második ügynök válaszok válasz csomagokkal. Ez a folyamat ismétlődik néhány alkalommal. Válaszok és az igénybe vett idő tooreceive hello száma minden válasz mérésével hello első NPM ügynök kapcsolat késési értékelésére, és az elveszett csomagokat.

hello formátumát, mérete és ezek a csomagok sorozatát határozza meg az Ön által figyelési szabályok létrehozásakor hello protokoll. Közbenső hálózati eszközök (útválasztók, kapcsolók stb.) a hello csomagok protokollon alapuló, hello előfordulhat, hogy másképp feldolgozni ezeket a csomagokat. A protokoll választott következésképpen hello eredmények pontosságának hello hatással van. És a protokoll választott azt is meghatározza, hogy bármely manuális lépéseket hello NPM megoldás telepítése után kell végrehajtania.

NPM kínál, hogy hello ICMP és a TCP protokoll a szintetikus tranzakciók végrehajtása közötti választás.
Ha ICMP szintetikus tranzakció szabály létrehozásakor, hello NPM ügynökök használja az ICMP ECHO üzenetek toocalculate hello hálózati késés és a csomagveszteség. ICMP ECHO használ hello azonos üzenet, amely hello hagyományos Ping eszköz által küldött. A TCP protokoll hello használata, amikor a NPM ügynökök TCP SZIN csomagot küldjön hello hálózaton keresztül. Ide kerül a TCP-kézfogás befejezési és majd a RST csomagok használatával hello-kapcsolat eltávolítása.

#### <a name="points-tooconsider-before-choosing-hello-protocol"></a>Pontok tooconsider hello protokoll kiválasztása előtt
Vegye figyelembe a következő információ, mielőtt úgy dönt, hogy a protokoll toouse hello:

##### <a name="discovering-multiple-network-routes"></a>Több hálózati útvonal felderítése
TCP pontosabb, ha több útvonal felderítéséhez és kevesebb ügynökök minden alhálózatban kell. Például egy vagy két ügynökök TCP használatával felderíthetők az alhálózatok közötti összes redundáns elérési utat. Azonban hogy több ügynökök ICMP tooachieve hasonló eredményeket kell. ICMP, ha rendelkezik segítségével *N* száma több, mint 5 kell két alhálózat között útvonalak*N* ügynökök vagy a forrás-, vagy a cél alhálózaton.

##### <a name="accuracy-of-results"></a>Eredmények pontosságának
Útválasztók és kapcsolók általában tooassign alacsonyabb prioritású virtuális gép tooICMP képest csomagok tooTCP csomagok. Bizonyos helyzetekben előfordulhat Ha a hálózati eszközök van terhelve, hello adatokat TCP jobban tükrözi hello veszteséget és késéseket azok az alkalmazások. Ennek oka az, hogy a legtöbb hello alkalmazás forgalom TCP protokollon keresztül zajlik. Ilyen esetekben ICMP kevésbé pontos eredményeket képest tooTCP biztosít.

##### <a name="firewall-configuration"></a>Tűzfal-konfiguráció
A TCP protokoll szükséges TCP-csomagokat küldött tooa célport. hello alapértelmezett port NPM-ügynökök által használt az 8084, de ez az ügynökök konfigurálásakor módosítható. Így kell tooensure, hogy a hálózati tűzfalak vagy (az Azure-ban) NSG-szabályok hello port forgalmát engedélyezi. Toomake is kell, hogy van konfigurálva, hogy hello a helyi tűzfal hello számítógépeken, ahol telepítve van-e az ügynök tooallow forgalom ezen a porton.

Használhatja PowerShell parancsfájlok tooconfigure tűzfal-szabályokat a Windows rendszert futtató számítógépek azonban tooconfigure van szüksége a hálózati tűzfal manuálisan.

Ezzel szemben ICMP nem működik a portot használja. A legtöbb vállalati környezetben ICMP-forgalmat megengedett hello tűzfalak tooallow toouse hálózati diagnosztika eszközök, például a Ping segédprogram hello keresztül. Igen ha egy gép másik pingelhető, majd használhatja hello ICMP protokollt anélkül, hogy manuálisan tooconfigure tűzfalak.

> [!NOTE]
> Abban az esetben, ha nem biztos abban, hogy milyen protokoll toouse, válassza ki az ICMP toostart. Ha elégedett nem hello eredményeket, mindig átválthat tooTCP később.


#### <a name="how-tooswitch-hello-protocol"></a>Hogyan tooswitch hello protokoll

Ha úgy dönt, toouse ICMP üzembe helyezése során, megváltoztathatja az tooTCP bármikor hello alapértelmezett szabály figyelési szerkesztésével.

##### <a name="tooedit-hello-default-monitoring-rule"></a>tooedit hello alapértelmezett figyelési szabály
1.  Keresse meg a túl**hálózati teljesítmény** > **figyelő** > **konfigurálása** > **figyelő** majd **alapértelmezett szabály**.
2.  Görgessen toohello **protokoll** szakaszt, és válassza ki a megjeleníteni kívánt toouse hello protokoll.
3.  Kattintson a **mentése** tooapply hello beállítást.

Akkor is, ha hello alapértelmezett szabály egy adott protokollt használ, a másik protokollal létrehozhat új szabályokat. Ha egyes hello szabályok ICMP, illetve egy másik használja TCP szabályok vegyesen is létrehozhat.




## <a name="data-collection-details"></a>Az gyűjtemény adatait
Hálózati Teljesítményfigyelő TCP SZIN-SYNACK-Nyugtázási kézfogás csomagokat használ, ha TCP kiválasztott és az ICMP ECHO ICMP ECHO REPLY, ha az ICMP protokoll toocollect veszteséget és késéseket adatait hello választja. Útválasztás nyomkövetése egyben használt tooget topológiainfomációja.

hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan hálózati Teljesítményfigyelő részleteit.

| Platform | Közvetlen ügynök | SCOM-ügynököt | Azure Storage | SCOM szükséges? | Felügyeleti csoport keresztül küldött SCOM ügynök adatok | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP kézfogások/ICMP ECHO üzenetek 5 másodpercentként adatok küldése át 3 percenként |

hello megoldás szintetikus tranzakciók tooassess hello állapotának hello hálózati használ. OMS-ügynököt egymással pontján különböző hello hálózati exchange TCP- vagy ICMP Echo (attól függően, hogy hello protokoll jelölni figyelésre) telepíteni. Hello folyamat ügynökök elsajátíthatja hello üzenetváltási időt és a csomag adatvesztés, ha van ilyen. Minden ügynök rendszeres időközönként is végez egy nyomkövetési útvonal tooother ügynökök toofind összes hello különböző útvonalak hello hálózatban kell vizsgálni. Ezen adatok hello ügynökök is kikövetkeztetni hello hálózati késés és a csomag bontásban. hello tesztek összes öt másodpercenként, és az adatok szerint van összesítve három perces időtartamra hello ügynökök toohello Naplóelemzés szolgáltatás feltöltés előtt meg kell ismételni.

> [!NOTE]
> Ügynökök gyakran egymással kommunikálnak, de azok nem hoznak létre nagy mennyiségű hálózati forgalom hello tesztek végrehajtása közben. Ügynökök használja csak a TCP SZIN-SYNACK-Nyugtázási kézfogás csomagok toodetermine hello veszteséget és késéseket--továbbított csomagok nincsenek adatok. A folyamat során ügynökök kommunikálnak egymással csak szükség esetén és hello ügynök kommunikációs topológiát arra optimalizálták tooreduce hálózati forgalmat.
>
>

## <a name="using-hello-solution"></a>Hello megoldással
Ez a szakasz ismerteti az összes hello irányítópult funkciók és hogyan toouse őket.

### <a name="solution-overview-tile"></a>Megoldás áttekintés csempe
Hello hálózati Teljesítményfigyelő megoldás engedélyezését, hello megoldás csempe hello OMS áttekintése lapon gyors áttekintést nyújt az hello hálózati állapota. A perecdiagram megfelelő és nem megfelelő állapotú alhálózati kapcsolat hello számát jeleníti meg. Hello csempére kattintva hello megoldás irányítópultja nyílik meg.

![Hálózati teljesítmény figyelése csempe](./media/log-analytics-network-performance-monitor/npm-tile.png)

### <a name="network-performance-monitor-solution-dashboard"></a>Hálózati Teljesítményfigyelő megoldás irányítópultja
Hello **hálózati összefoglaló** panel hello hálózatok és a relatív méretük összegzését jeleníti meg. A hálózati kapcsolatokon, az alhálózati hivatkozások és az elérési út teljes száma (hello IP-címek ügynökkel két gazdagépek és a közöttük minden hello ugrások áll egy elérési utat) hello rendszer megjelenítő csempék követi.

Hello **felső hálózati állapotával kapcsolatos események** panel felsorolja legutóbbi állapotával kapcsolatos események és riasztások hello rendszer és hello idő óta módosították, hogy hello esemény aktív. A rendszerállapot-esemény vagy riasztás jön létre, amikor hello csomagvesztés vagy a hálózati vagy alhálózat hivatkozás várakozási ideje meghaladja a küszöbértéket.

Hello **felső nem megfelelő állapotú hálózati kapcsolatok** panelen lévő nem megfelelő állapotú hálózati kapcsolatok listáját jeleníti meg. Ezek a hello hálózati hivatkozások, amelyek egy vagy több káros állapotesemény őket hello pillanatban.

Hello **felső alhálózati kapcsolat legtöbb adatvesztés** és **alhálózati kapcsolat és a késleltetés** paneleken csomagvesztés hello felső alhálózati kapcsolat megjelenítése és alhálózati kapcsolat rendre felső késés korlátozza. Nagy késleltetésű vagy bizonyos csomagvesztés várható bizonyos hálózati kapcsolatokról. Ilyen kapcsolatok hello felső tíz listán, de nem különbözteti meg a nem megfelelő.

Hello **közös lekérdezések** panel, amely a nyers hálózati figyelési közvetlenül az adatok lehívása keresési lekérdezések készletét tartalmazza. Ezeket a lekérdezéseket kiindulási pontként használható a saját testreszabott jelentéskészítéshez lekérdezések létrehozásáról.

![Hálózati teljesítmény figyelő irányítópult](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="drill-down-for-depth"></a>Részletezés az mélysége
Hello megoldás irányítópult toodrill lefelé mélyebb különböző mutató hivatkozások be bármely területekre érdeklő gombra. Például amikor megjelenik egy riasztás vagy egy nem megfelelő állapotú hálózati kapcsolat hello irányítópulton megjelenik, kattinthat, további tooinvestigate. Tooa lap, amely tartalmazza az összes hello alhálózati kapcsolat hello adott hálózati kapcsolat fog venni. Fogja tudni toosee hello adatvesztés, a késleltetés és a rendszerállapot minden alhálózat hivatkozások állapota, és gyorsan megtudhatja, milyen alhálózati kapcsolat hello probléma okozza. Ezután kattintson **csomóponti kapcsolat megtekintése** toosee hello nem megfelelő állapotú alhálózati minden hello csomóponti kapcsolat hivatkozásra. Ezután tekintse meg az egyes csomópontok hivatkozásokat, és hello nem megfelelő állapotú csomóponti kapcsolat található.

Kattinthat **nézet topológia** tooview hello Ugrás által Ugrás topológia hello útvonalak hello forrás és cél csomópontok között. a nem megfelelő útvonalak hello, vagy ugrások piros színnel jelennek meg, hogy gyorsan azonosíthatja a hello probléma tooa adott részének hello hálózati.

![részletes adatok](./media/log-analytics-network-performance-monitor/npm-drill.png)

### <a name="network-state-recorder"></a>Hálózati állapotát rögzítő

Minden a nézet jeleníti meg, időben pillanatképet a hálózat állapotát az adott helyen. Alapértelmezés szerint hello legutóbbi állapot jelenik meg. hello hello oldal hello tetején látható hello pontot, amelynek hello állapot jelenik meg időben. Választhatja ki toogo vissza a hálózati állapotfigyelő idő és a nézet hello pillanatképe parancsával hello sávon **műveletek**. Válassza ki a tooenable vagy lapok automatikus frissítésének letiltása, amíg hello legfrissebb állapotának megtekintése is.

![hálózati állapota](./media/log-analytics-network-performance-monitor/network-state.png)

#### <a name="trend-charts"></a>Trend diagramok
Minden szinten, hogy részletes, hello trendjét veszteséget és késéseket hálózati kapcsolat látható. Trend diagramok alhálózat és a csomópont hivatkozások is elérhetők. Hello graph tooplot hello időintervallumát hello diagram hello tetején hello vezérlő használatával módosíthatja.

Trend diagramok egy korábbi szempontjából a hálózati kapcsolaton hello teljesítményének megjelenítése. Egyes hálózati probléma átmeneti jellegű, és hello hello hálózati aktuális állapotának megtekintésével csak rögzített toocatch lenne. Ennek az az oka a problémák gyorsan surface, és mielőtt bárki megjegyzések, csak később időben tooreappear eltűnnek. Ilyen átmeneti probléma is nehézkes lehet alkalmazás-rendszergazdák számára, mert azok gyakran felület ad ki a megtévesztő növekedése az alkalmazás válaszideje, még akkor is, amikor az összes alkalmazás-összetevők toorun zökkenőmentesen megjelennek.

Ahol jelenik meg a hálózati késést vagy a csomag adatvesztéshez hirtelen csúcs hello probléma trend diagram megtekintésével könnyen képes észlelni az ilyen típusú problémák.

![Trend diagram](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Ugrás az Ugrás által topológia térkép
A hálózati Teljesítményfigyelő azt mutatja be, akkor hello Ugrás által Ugrás topológia útvonalak egy interaktív topológia-térképként két csomópont között. Jelölje ki a csomópont hivatkozást, majd kattintson hello topológia térkép megtekintheti **nézet topológia**. Emellett megtekintheti hello topológia térkép kattintva **elérési utak** hello irányítópult csempét. Amikor rákattint **elérési utak** hello irányítópult összekapcsolta tooselect hello forrás és cél csomópontok hello bal oldali panelen, és kattintson a **tőzsdei** tooplot hello útvonalak hello két csomópont között.

hello topológia térképen jeleníti meg, hány útvonalak hello között két csomópontot, és milyen elérési utak hello csomagok érvénybe adatok. Hálózati teljesítmény szűk hello topológia-térképként piros színnel van jelölve. Megkeresheti a hibás hálózathoz vagy egy hibás hálózati eszköz hello topológia-térképként piros színes elemek felmérésével.

Kattintson a csomópont vagy rámutatáskor felett hello topológia-térképként, helyezésekor itt látható hello tulajdonságok hello csomópont teljes tartománynév és IP-címnek. Kattintson egy Ugrás toosee azt az IP-cím. Választhat toofilter adott útvonalak hello összecsukható műveletpanelen hello szűrők segítségével. És hello hálózati topológiák hello ugrások hello csúszkával hello műveletpanelen elrejtésével is egyszerűsíthető. Nagyítás növelésére, de az egér kerekének használatával hello topológia térkép ki.

Vegye figyelembe, hogy hello topológia látható hello térkép 3 rétegbeli topológia, nem tartalmazza 2. rétegbeli eszközök és kapcsolatok.

![Ugrás az Ugrás által topológia térkép](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Hibák megkeresése
Hálózati teljesítmény figyelőjének értéke a anélkül toohello hálózati eszközöket képes toofind hello hálózati szűk keresztmetszeteket. Hello hello hálózatról, és speciális algoritmusok alkalmazásával hello hálózati grafikonon összegyűjtött adatokon alapulnak, hálózati Teljesítményfigyelő teszi hello hálózat részei, amelyek hello probléma legvalószínűbb hello forrása probabilisztikus becslése.

Ez a megközelítés hasznos toodetermine hello hálózati szűk keresztmetszeteket akkor, ha a hozzáférési toohops nem érhető el, mert nem igényel semmilyen hello hálózati eszközök, például az útválasztók és kapcsolók gyűjtött adatok toobe. Ez esetén is hasznos hello ugrások két csomópont között nem szerepelnek a rendszergazdai felügyeletet. Például a hello ugrások Internetszolgáltató útválasztók lehet.

### <a name="log-analytics-search"></a>A Naplóelemzési keresése
Minden adat, amely grafikusan hello hálózati Teljesítményfigyelő irányítópult és a részletes oldalak érhető el natív módon Naplóelemzési keresési. Hello keresési lekérdezés nyelvének segítségével hello adatait, és egyéni jelentések készítése hello adatok tooExcel vagy a Power bi exportálásával. Hello **közös lekérdezések** hello irányítópult paneljén rendelkezik néhány hasznos lekérdezést, amely akár hello kiindulási pontjaként a saját lekérdezések és jelentések létrehozására is használhatja.

![keresési lekérdezések](./media/log-analytics-network-performance-monitor/npm-queries.png)

## <a name="investigate-hello-root-cause-of-a-health-alert"></a>Vizsgálja meg a hello alapvető oka egy állapotriasztás
Most, hogy tekintse át a hálózati teljesítmény figyelése, egyszerű vizsgálatot a hello kiváltó okának állapotfigyelő esemény vizsgáljuk meg.

1. Hello – Áttekintés lapon jelenik meg a hálózat hello állapotának gyors pillanatképe hello betartásával **hálózati Teljesítményfigyelő** csempére. Figyelje meg, hogy kívül hello 6 alhálózatai figyelt hivatkozik, a 2 sérült állapotban. Ez garantálja, hogy a vizsgálat. Kattintson a hello csempe tooview hello megoldás irányítópultja.  
   ![Hálózati teljesítmény figyelése csempe](./media/log-analytics-network-performance-monitor/npm-investigation01.png)
2. Hello az alábbi példa kép láthatja, hogy nincs olyan állapotesemény egy sérült hálózati kapcsolat. Döntse el, tooinvestigate hello probléma, és kattintson a hello **DMZ2-DMZ1** hálózati kapcsolat toofind kimenő hello probléma hello gyökérmappájában.  
   ![nem megfelelő állapotú hálózati kapcsolat, példa](./media/log-analytics-network-performance-monitor/npm-investigation02.png)
3. hello részletező lapon látható minden hello alhálózati kapcsolat a **DMZ2-DMZ1** hálózati kapcsolat. Láthatja, hogy mindkét hello alhálózati kapcsolat hello késés elért hello küszöbértéket, így hello hálózati kapcsolat nem kifogástalan. Hello késés trendjeit mindkét hello alhálózati kapcsolat is látható. Használhatja a hello kijelölés hello graph toofocus vezérlőre szükséges időtartomány hello idő. Hello időpontot hello Ha várakozási ideje elérte a maximális tekintheti meg. Hello naplókat a probléma idő időszak tooinvestigate hello később rá tud keresni. Kattintson a **csomóponti kapcsolat megtekintése** toodrill ki további.  
   ![nem megfelelő állapotú alhálózati hivatkozások – példa](./media/log-analytics-network-performance-monitor/npm-investigation03.png)
4. Hasonló toohello előző lapon, hello leásási hello adott alhálózat hivatkozás laplistákhoz le a bennük foglalt csomóponti kapcsolat. Hasonló műveletek végezhetők itt hello előző lépésben hasonló módon. Kattintson a **nézet topológia** tooview hello topológia hello 2-csomópontok között.  
   ![nem megfelelő állapotú csomóponti hivatkozások – példa](./media/log-analytics-network-performance-monitor/npm-investigation04.png)
5. Hello 2 kijelölt csomópontok közötti görbékhez hello ábrázolási hello topológia a térképen. Hello Ugrás által Ugrás topológia hello topológia-térképként két csomópont között útvonalak jelenítheti meg. Biztosít világossá, hogy hány útvonalak léteznek hello két csomópont között, és milyen elérési utak hello adatcsomagok tart. Hálózati teljesítmény szűk piros színnel jelöli. Megkeresheti a hibás hálózathoz vagy egy hibás hálózati eszköz hello topológia-térképként piros színes elemek felmérésével.  
   ![a nem megfelelő topológia nézet – példa](./media/log-analytics-network-performance-monitor/npm-investigation05.png)
6. hello adatvesztés, a késés és az egyes elérési utakat az ugrások számát hello hello tekinthetők át **művelet** ablaktáblán. Használja a hello görgetősáv tooview hello adatait azok a nem megfelelő elérési útja.  Nem megfelelő állapotú Ugrás hello használata hello hello szűrők tooselect elérési utak, hogy csak hello hello topológia kiválasztott elérési utak ábrázolja. Az egér kerekének toozoom hello topológia térkép kívül vagy belül is használhatja.

   A kép alatt hello jól látható kiváltó okának hello probléma területek toohello adott részének hello hálózati hello hello elérési útja és a piros szín ugrások megtekintésével. Kattintson a hello topológia térkép csomópontja tárja fel a hello tulajdonságok hello csomópont, beleértve a hello teljes tartománynév és IP-cím. Hello IP-címét hello hop kattintva jeleníti meg.  
   ![a nem megfelelő topológia – példa az útvonalra részletei](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="provide-feedback"></a>Visszajelzés küldése

- **UserVoice** -könyvelés ötleteit a Teljesítményfigyelő hálózati szolgáltatásokat, amelyet velünk toowork a. Látogasson el a [UserVoice lap](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring).
- **Csatlakozás a kohorszok** -még mindig a kohorszok csatlakozás új ügyfelek rendelkező iránt érdeklődik. Része, akkor lesz korai juthatnak toonew szolgáltatásokat, és Hálózatfigyelő teljesítményének javítása érdekében. Érdekli való csatlakozás, ha kitöltés kibővített ez [gyors felmérés](https://aka.ms/npmcohort).

## <a name="next-steps"></a>Következő lépések
* [Naplók keresése](log-analytics-log-searches.md) tooview részletes rekordok hálózati teljesítmény.
