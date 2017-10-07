---
title: "Felügyeleti megoldás az OMS aaaUpdate |} Microsoft Docs"
description: "Ez a cikk tervezett toohelp, hogy tudomásul veszi, hogy toouse a megoldás toomanage frissíti a Windows és Linux számítógépek."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/27/2017
ms.author: magoedte
ms.openlocfilehash: 2dd321913bf049ab1996fd60a2f74b8417084dcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-management-solution-in-oms"></a>Frissítéskezelési megoldás az OMS-ben

![Frissítéskezelési szimbólum](./media/oms-solution-update-management/update-management-symbol.png)

hello OMS frissítés felügyeleti megoldás lehetővé teszi toomanage operációs rendszer biztonsági frissítések üzembe helyezett Azure, a Windows és Linux rendszerű számítógépek a helyszíni környezetben, vagy más szolgáltatók.  Gyorsan hello állapotának a rendelkezésre álló frissítések ügynököt minden olyan számítógépre felmérése és kezelése hello kiszolgálók kötelező frissítések telepítésének folyamatát.


## <a name="solution-overview"></a>Megoldás áttekintése
OMS által felügyelt számítógépek használja hello következő értékelési és a frissítés központi telepítések elvégzéséhez:

* OMS Windows- vagy Linux-ügynök
* PowerShell-célállapotkonfiguráció (DSC) Linux rendszerre
* Automation hibrid runbook-feldolgozó
* Microsoft Update vagy Windows Server Update Services Windows-számítógépekhez

a következő ábrákon hello jeleníti meg, hogyan hello megoldás értékelésére, és alkalmazza a biztonsági frissítések tooall viselkedésének és adatfolyam hello konceptuális ábrázolása csatlakoztatott Windows Server és Linux rendszerű számítógépek a munkaterületen.    

#### <a name="windows-server"></a>Windows Server
![A Windows Server frissítéskezelési folyamatdiagramja](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![A Linux frissítéskezelési folyamatdiagramja](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

Miután hello számítógép frissítési megfelelőség szempontjából vizsgálatot végez, hello OMS-ügynököt továbbítja hello a tömeges tooOMS. A Windows-számítógépen hello megfelelőségi vizsgálat alapértelmezett 12 óránként végzi.  Továbbá toohello vizsgálati ütemezés szerint hello vizsgálata frissítési megfelelőség szempontjából a lehet kezdeményezni, amennyiben a Microsoft Monitoring Agent (MMA) hello indítani, előzetes tooupdate telepítés 15 percen belül, és a frissítés telepítése után.  Linux rendszerű számítógép hello megfelelőségi vizsgálat alapértelmezett 3 óránként végzi, és a megfelelőségi vizsgálat kezdeményezett 15 percen belül történik, ha hello MMA ügynök újraindítása.  

hello a megfelelőségi információkat majd feldolgozásra és hello irányítópultok szerepel hello megoldás vagy felhasználó által definiált kereshető használatával vagy előre összegzett-lekérdezések által definiált.  hello megoldást jelent hogyan naprakész hello számítógép milyen forrás a konfigurált toosynchronize alapul.  Ha hello Windows rendszerű számítógépen konfigurált tooreport tooWSUS, attól függően, hogy ha a WSUS utoljára szinkronizálva a Microsoft Update hello eredmények eltérhet a Microsoft Updates jeleníti meg.  Linux rendszerű számítógépek, amelyek a konfigurált tooreport tooa helyi tárház és egy nyilvános tárház hello azonos.   

Telepítheti és szoftverfrissítések telepítéséhez hozzon létre egy ütemezett telepítési hello frissítést igénylő számítógépeken.  Frissítés besorolása *nem kötelező* nem szerepelnek az hello telepítési hatókör Windows rendszerű számítógépek esetén csak a szükséges frissítéseket.  hello ütemezett telepítés meghatározása milyen célszámítógépeken kap hello megfelelő frissítéseket, vagy explicit módon adja meg a számítógépeket, vagy jelöljön ki egy [számítógépcsoport](../log-analytics/log-analytics-computer-groups.md) ki egy meghatározott napló átvizsgálása alapuló azok a számítógépek.  Is adjon meg egy ütemezést tooapprove, és jelölje ki az adott időszakban, amikor a frissítések telepítve vannak toobe engedélyezettek.  A telepítést az Azure Automation runbookjai végzik.  A runbookok nem tekinthetők meg, és nem kívánnak semmilyen konfigurálást.  Amikor egy központi telepítésének jön létre, ütemezése, hogy elindítja hello fő frissítés runbook hello szereplő számítógépek számára hoz létre.  A mesterrunbook minden olyan ügynökön egy gyermekrunbookot indít el, amely elvégzi a szükséges frissítések telepítését.       

A hello hello központi telepítést a megadott időpontban hello célszámítógépeken hello telepítési párhuzamosan hajtja végre.  A vizsgálat első elvégezni tooverify hello frissítések továbbra is szükséges, és telepíti azokat.  Fontos, a WSUS-ügyfélszámítógépek számára, ha hello frissítések WSUS, hello központi telepítést a jóvá nem hagyott toonote sikertelen lesz.  hello alkalmazott frissítések hello eredményeit a rendszer továbbítja tooOMS toobe dolgoz fel, és a hello irányítópultok vagy hello események hello foglalja össze.     

## <a name="prerequisites"></a>Előfeltételek
* hello megoldás teljesítő frissítés vizsgálatok során a Windows Server 2008 és újabb támogatja, és Windows Server 2008 R2 SP1 és újabb telepítésének folyamatát.  A Server Core és a Nano Server telepítési lehetőségek nem támogatottak.

    > [!NOTE]
    > Frissítések tooWindows Server 2008 R2 SP1 telepítése támogatása a .NET-keretrendszer 4.5 és WMF 5.0-s vagy újabb szükséges.
    >  
* Az ügyféloldali Windows operációs rendszerek nem támogatottak.  
* Windows-ügynökök vagy egy Windows Server Update Services (WSUS) kiszolgálóhoz konfigurált toocommunicate vagy szükséges hozzáférési tooMicrosoft frissítés.  

    > [!NOTE]
    > Windows hello-ügynök nem kezelhető egyidejűleg a System Center Configuration Manager által.  
    >
* CentOS 6 (x86/x64) és 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) és 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) és 12 (x64)  
* Ubuntu 12.04 LTS és újabb x86/x64   
    > [!NOTE]  
    > tooavoid frissítések alkalmazásakor az Ubuntu, a karbantartási időszakon kívül konfigurálja újra a felügyelet nélküli frissítési csomag toodisable automatikus frissítések. Információ tooconfigure a, lásd: [hello Ubuntu Server útmutató témakörében automatikus frissítések](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

* Linux-ügynökök tooan tárházához hozzáféréssel kell rendelkeznie.  

    > [!NOTE]
    > Az OMS-ügynököt Linux beállítva tooreport toomultiple OMS-munkaterület nem támogatott ezen megoldás.  
    >

További tudnivalókat tooinstall OMS-ügynököt hello Linux és a hello legújabb verzió letöltéséhez lásd túl[Operations Management Suite-ügynök Linux](https://github.com/microsoft/oms-agent-for-linux).  Hogyan a tooinstall OMS ügynök a Windows hello információkért tekintse át [Operations Management Suite Windowsra ügynök](../log-analytics/log-analytics-windows-agents.md).  

### <a name="permissions"></a>Engedélyek
A sorrend toocreate frissítse a központi telepítések, toobe hello közreműködő szerepkört az Automation-fiók és a Naplóelemzési munkaterület engedélyezni kell.  

## <a name="solution-components"></a>Megoldás-összetevők
Ez a megoldás a következő erőforrások tooyour Automation-fiók és a közvetlenül csatlakoztatott ügynökök vagy az Operations Manager csatlakoztatott felügyeleti csoporthoz hozzáadott hello áll.

### <a name="management-packs"></a>Felügyeleti csomagok
Ha a System Center Operations Manager felügyeleti csoport csatlakoztatott tooan OMS-munkaterület, az Operations Manager felügyeleti csomagok a következő hello vannak telepítve.  Ezeket a felügyeleti csomagokat a megoldás hozzáadását követően a rendszer a közvetlenül kapcsolódó Windows rendszerű számítógépekre is telepíti. Nincs mit tooconfigure vagy a felügyeleti csomagok kezelése.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Frissítéstelepítő felügyeleti csomag

A megoldás felügyeleti csomagok frissítésének további információkért lásd: [csatlakozás az Operations Manager tooLog Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="hybrid-worker-groups"></a>Hibridfeldolgozó-csoportok
Miután engedélyezte a megoldás, a Windows-számítógép közvetlenül csatlakoztatott OMS-munkaterület automatikusan konfigurálja a megoldásban a hibrid forgatókönyv-feldolgozó toosupport hello runbookok tooyour.  Hello megoldás által kezelt Windows számítógépenként jelenik meg a hello Hybrid Runbook Worker csoportok panelen található a következő hello elnevezési hello Automation-fiók *állomásnév FQDN_GUID*.  Ezeket a csoportokat nem célozhatja meg, ha runbookok vannak a fiókjában, ellenkező esetben sikertelenek lesznek. Ezek a csoportok csak tervezett toosupport hello-kezelési megoldás.   

Ugyanakkor hozzáadása hello Windows-számítógép tooa hibrid forgatókönyv-feldolgozó csoport az Automation-fiók toosupport Automation-forgatókönyveket, mindaddig, amíg hello hello megoldás és a hibrid forgatókönyv-feldolgozó csoport tagsága is fiókot használja.  Ez a funkció a hibrid forgatókönyv-feldolgozó hello 7.2.12024.0 tooversion bővült.  

## <a name="configuration"></a>Konfiguráció
Hajtsa végre a következő lépéseket tooadd hello frissítéskezelés megoldás tooyour OMS-munkaterület hello, és erősítse meg az ügynökök jelentik. Windows-ügynökök már csatlakoztatott tooyour munkaterület automatikusan hozzáadja a további konfigurációra.

A következő módszerek hello használó hello megoldás telepítése:

* Az Azure-portálon hello Automation & vezérlő ajánlat vagy frissítés felügyeleti megoldás hello Azure piactérről
* A hello OMS megoldások gyűjteményt az OMS-munkaterület

Ha már rendelkezik egy Automation-fiók és a kapcsolódó OMS-munkaterület hello kiírni ugyanabban az erőforráscsoportban régió automatizálás és a vezérlő kiválasztása a konfiguráció ellenőrzése és csak hello megoldás telepítése és mindkét szolgáltatás konfigurálását.  Hello frissítés felügyeleti megoldás kiválasztása az Azure piactérről kézbesíti hello kívánt viselkedést eredményező beállítást.  Ha nincs telepítve az előfizetés vagy szolgáltatások, kövesse hello hello **hozzon létre új megoldás** panel és erősítse meg más tooinstall hello előre kiválasztott ajánlott megoldásokat.  Opcionálisan hozzáadhat hello frissítéskezelés megoldás tooyour leírt lépésekkel hello OMS-munkaterület [hozzáadása OMS megoldások](../log-analytics/log-analytics-add-solutions.md) a hello megoldások gyűjteménye.  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-toooms"></a>Erősítse meg az OMS-ügynököt, és az Operations Manager felügyeleti csoport csatlakoztatva tooOMS

tooconfirm közvetlenül csatlakoztatott OMS-ügynököt a Linux és Windows kommunikáló OMS-ben, néhány perc múlva futtatása hello napló keresése a következő:

* Linux – `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`.  

* Windows – `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

A Windows-számítógépen a következő tooverify ügynök kapcsolatairól az OMS hello tekinthetők át:

1.  Nyissa meg a Microsoft Monitoring Agent, a Vezérlőpulton, és az hello **Azure Naplóelemzés (OMS)** lapján hello ügynök jeleníti meg a következő üzenet: **hello Microsoft Monitoring Agent sikeresen csatlakozott a Microsoft toohello Operations Management Suite szolgáltatás**.   
2.  Nyissa meg a Windows Eseménynapló hello, keresse meg a túl**alkalmazások és szolgáltatások Logs\Operations kezelője** , és keressen a Event ID 3000 és 5002 Service Connector forrásból.  Ezek az események azt jelzik, hello számítógép hello OMS-munkaterület regisztrált, és konfigurációs fogadja.  

Ha hello ügynök nem képes toocommunicate a hello OMS szolgáltatáshoz, és a hello konfigurált toocommunicate internet egy tűzfal vagy proxykiszolgálón keresztül, győződjön meg arról, tűzfal hello vagy proxykiszolgáló helyesen van konfigurálva a megtekintésével [hálózati Windows-ügynök](../log-analytics/log-analytics-windows-agents.md#network) vagy [hálózati konfigurációt a Linux-ügynök](../log-analytics/log-analytics-agent-linux.md#network).

> [!NOTE]
> Ha a Linux rendszereket a proxy vagy az OMS-átjáró a konfigurált toocommunicate és bevezetési ebben a megoldásban frissítse a hello *proxy.conf* engedélyek toogrant hello omiuser csoport olvasási jogosultsággal a fájl hello a következő parancsok hello végrehajtása:  
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`  
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`


Az újonnan hozzáadott Linux-ügynökök **Frissítve** állapotúak, miután a rendszer végrehajt egy elemzést.  Ez a folyamat akár too6 üzemideje (óra) is igénybe vehet.

tooconfirm az Operations Manager felügyeleti csoport kommunikál OMS-ben, lásd: [ellenőrzése az Operations Manager integrációja OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms).

## <a name="data-collection"></a>Adatgyűjtés
### <a name="supported-agents"></a>Támogatott ügynökök
a következő táblázat hello hello csatlakoztatott adatforrások, ez a megoldás által támogatott ismerteti.

| Összekapcsolt forrás | Támogatott | Leírás |
| --- | --- | --- |
| Windows-ügynökök |Igen |hello megoldás rendszer frissítésével kapcsolatos információkat gyűjti össze a Windows-ügynökök, és kezdeményezi a szükséges frissítések telepítését. |
| Linux-ügynökök |Igen |hello megoldás rendszer frissítésével kapcsolatos információkat gyűjti össze az Linux-ügynököt, és a támogatott disztribúciókkal kötelező frissítések telepítésének kezdeményez. |
| Az Operations Manager felügyeleti csoportja |Igen |hello megoldás rendszerfrissítések adatokat gyűjt az ügynökök a csatlakoztatott felügyeleti csoport.<br>Közvetlen kapcsolat az Operations Manager-ügynök tooLog hello Analytics nincs szükség. Adattárból hello felügyeleti csoport toohello OMS adat továbbítódik. |
| Azure Storage-fiók |Nem |Az Azure Storage nem tartalmaz rendszerfrissítésekkel kapcsolatos információt. |

### <a name="collection-frequency"></a>A gyűjtés gyakorisága
Minden felügyelt Windows-számítógép esetében naponta kétszer történik vizsgálat. Minden 15 perc hello Windows API hívása tooquery az utolsó frissítés időpontja toodetermine hello Ha állapota megváltozott, és ha igen a megfelelőségi vizsgálat lehet kezdeményezni.  Linux-számítógépek esetében a vizsgálat három óránként történik.

Azt is igénybe vehet 30 perc too6 órában hello irányítópult toodisplay frissített adatokat a felügyelt számítógépekről.   

## <a name="using-hello-solution"></a>Hello megoldással
Hello frissítéskezelés megoldás tooyour OMS-munkaterület felvételekor hello **frissítéskezelés** csempe megkapja tooyour OMS irányítópult. Ez a csempe a környezetben, és a frissítési megfelelőség szempontjából egy száma és a grafikus ábrázolása hello számítógépek számát jeleníti meg.<br><br>
![Frissítéskezelés – áttekintő csempe](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>A frissítési felmérések megtekintése
Kattintson a hello **frissítéskezelés** csempe tooopen hello **frissítéskezelés** irányítópult.<br><br> ![Frissítéskezelés – áttekintő irányítópult](./media/oms-solution-update-management/update-management-dashboard.png)<br>

Az irányítópult részletes áttekintést kínál a frissítési állapotokról az operációs rendszer típusa és frissítési besorolása alapján kategorizálva – kritikus, biztonsági vagy egyéb (például egy definíciófrissítés). Mindegyik mozaiknál ezen az irányítópulton hello eredményez csak a központi telepítés, amely hello számítógépek szinkronizálási forrás alapján jóváhagyott frissítések tükrözik.   Hello **szoftverfrissítés-telepítések** csempére, amikor a kijelölt, ahol megtekintheti a folyamatban, befejezett központi telepítések ütemezésének vagy egy új központi telepítés ütemezése toohello szoftverfrissítés-telepítések lap irányítja.  

Amely visszaadja az összes rekord kattintva hello adott csempe vagy toorun egy lekérdezést egy adott kategória és az előre meghatározott feltételeknek napló keresést futtat, válasszon egyet a hello listából hello kategóriában elérhető **általános frissítési lekérdezések** oszlop.    

## <a name="installing-updates"></a>Frissítések telepítése
Miután frissítések az összes hello Linux és Windows rendszerű számítógépek a munkaterületen volna értékelni, akkor is rendelkezik olyan telepített szükséges frissítés létrehozásával egy *központi telepítésének*.  A frissítéstelepítés egy vagy több számítógép szükséges frissítéseinek ütemezett telepítése.  Hello adjon meg, és időt az hello telepítési továbbá tooa számítógép vagy számítógépek csoportja, amelyek a központi telepítés hello hatókör szerepelnie kell.  További információ a számítógépcsoportokat, toolearn lásd: [számítógépcsoportokat a Naplóelemzési](../log-analytics/log-analytics-computer-groups.md).  Ha a frissítés telepítése a számítógépcsoportokat, csoport memnbership hello ütemezés létrehozása során csak egyszer értékeli ki.  További változások tooa csoport nem jelennek meg.  Ennek, toowork törli hello ütemezett frissítés központi telepítését, és hozza létre újra.

> [!NOTE]
> A Windows hello Azure piactér alapértelmezés szerint vannak beállítva tooreceive automatikus frissítések szolgáltatás a Windows Update szolgáltatásból a telepített virtuális gépek.  Ez a viselkedés nem változtatja meg a megoldást vagy a Windows virtuális gépek tooyour munkaterület hozzáadása után.  Ha így tesz, ez a megoldás az aktívan kezelt frissítések, alapértelmezett viselkedést hello (automatikus frissítések alkalmazása) érvényes.  

Hello igény Red Hat Enterprise Linux (RHEL) lemezképeket Azure piactéren alapján létrehozott virtuális gépeken, a regisztrált tooaccess hello azok [Red Hat frissítés infrastruktúra (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) Azure szolgáltatásba telepített.  Más a Linux-disztribúció adattárból hello disztribúciókkal online fájlt a támogatott módszerek követően frissíteni kell.  

### <a name="viewing-update-deployments"></a>Frissítéstelepítések megtekintése
Kattintson a hello **központi telepítésének** csempe tooview hello meglévő központi telepítések listája.  A lista a frissítések állapota szerinti csoportosításban jelenik meg: **Ütemezett**, **Futó** és **Befejezett**.<br><br> ![Frissítéstelepítési ütemezési lap](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

minden központi telepítési megjelenített hello tulajdonságok hello a következő táblázat ismerteti.

| Tulajdonság | Leírás |
| --- | --- |
| Név |Hello központi telepítés nevét. |
| Ütemezés |Az ütemezés típusa.  Az elérhető lehetőségek: *Egyszer*, *Hetente*, vagy *Havonta*. |
| Kezdési idő |Dátum és idő, amely a központi telepítés hello ütemezett toostart. |
| Időtartam |Frissítés telepítése engedélyezett toorun perc hello száma.  Ha minden frissítés nincsenek telepítve az ezen időtartam belül frissítések fennmaradó hello várnia kell, majd hello a következő központi telepítést. |
| Kiszolgálók |Hello központi telepítés által érintett számítógépek számát.  |
| status |Központi telepítés hello aktuális állapota.<br><br>Lehetséges értékek:<br>– Nincs elindítva<br>– Fut<br>– Befejeződött |

Válassza ki a befejezett központi telepítés tooview hello részletes képernyő hello oszlopok a következő táblázat hello tartalmazó.  Ezekben az oszlopokban fog megadni, ha a központi telepítés hello nem rendelkezik még nincs elindítva.<br><br> ![Frissítéstelepítési eredmények áttekintése](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| Oszlop | Leírás |
| --- | --- |
| **Számítógépnézet** | |
| Windows rendszerű számítógépek |Hello hello frissítés központi telepítési állapot szerint a Windows rendszerű számítógépek számát tartalmazza.  Kattintson az egy állapot toorun vissza rekordok minden frissítés hello központi telepítésének állapotát a napló keresést. |
| Linux rendszerű számítógépek |Felsorolja a Linux rendszerű számítógépek hello frissítés központi telepítési állapot szerint hello számát.  Kattintson az egy állapot toorun vissza rekordok minden frissítés hello központi telepítésének állapotát a napló keresést. |
| Számítógép telepítési állapota |Hello számítógépek részt hello központi telepítést és a frissítések, amelyek sikeresen telepítették hello százalékos sorolja fel. Hello bejegyzések toorun egyik egy napló kattintson a Keresés gombra az összes hiányzó és kritikus frissítések vissza. |
| **Frissítésnézet** | |
| Windows-frissítések |Felsorolja a hello központi telepítésének és azok telepítési állapotát minden egyes frissítés egy Windows-frissítések.  Egy frissítés toorun vissza az összes adott frissítés rekordok frissítése, vagy kattintson a hello állapot toorun visszaadó hello telepítéshez rekordok frissítése az összes napló keresés napló keresés kijelölése. |
| Linux-frissítések |Linux-frissítések a hello központi telepítésének és azok telepítési állapotát minden egyes frissítések alapján sorolja fel.  Egy frissítés toorun vissza az összes adott frissítés rekordok frissítése, vagy kattintson a hello állapot toorun visszaadó hello telepítéshez rekordok frissítése az összes napló keresés napló keresés kijelölése. |

### <a name="creating-an-update-deployment"></a>Frissítéstelepítés létrehozása
Hozzon létre egy új központi telepítés hello kattintva **Hozzáadás** hello képernyő tooopen hello hello tetején gomb **új központi telepítést** lap.  A következő táblázat hello hello tulajdonságok értékeket kell megadnia.

| Tulajdonság | Leírás |
| --- | --- |
| Név |Egyedi nevet tooidentify hello frissítés telepítése. |
| Időzóna |Időzóna toouse hello kezdési ideje. |
| Ütemezés típusa | Az ütemezés típusa.  Az elérhető lehetőségek: *Egyszer*, *Hetente*, vagy *Havonta*.  
| Kezdési idő |Dátum és idő toostart hello frissítés központi telepítése. **Megjegyzés:** hello kizárása futtathatja a központi telepítés esetén az aktuális időponthoz képest 30 perc toodeploy azonnal kell. |
| Időtartam |Frissítés telepítése engedélyezett toorun perc hello száma.  Ha minden frissítés nincsenek telepítve az ezen időtartam belül frissítések fennmaradó hello várnia kell, majd hello a következő központi telepítést. |
| Számítógépek |Számítógépek vagy csoportok tooinclude számítógép és hello központi telepítést a cél nevét.  Válasszon egy vagy több bejegyzés hello legördülő listából. |

<br><br> ![Új frissítéstelepítés lapja](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Időtartomány
Alapértelmezés szerint hello hello frissítés felügyeleti megoldás elemezheti hello adatok hatóköre utolsó 1 napon belül hello generált az összes kapcsolódó felügyeleti csoportokból származó.

toochange hello időtartománya hello adatokat, válassza ki **adatok alapján** hello irányítópult hello tetején. Rögzíti a létrehozás vagy frissítés belül hello 7 napja, 1 nap vagy 6 óra választhatja ki. Az **Egyéni** lehetőség választásával egyéni dátumtartományt is megadhat.

## <a name="log-analytics-records"></a>Log Analytics-rekordok
frissítéskezelés megoldás hello kétféle típusú rekordok hello OMS-tárház hoz létre.

### <a name="update-records"></a>Update típusú rekordok
Az egyes számítógépekhez szükséges vagy telepített minden egyes frissítéshez egy**Update** (frissítés) típusú rekord készül. Frissítés rögzíti a következő táblázat hello hello jellemzőkkel rendelkezik.

| Tulajdonság | Leírás |
| --- | --- |
| Típus |*Update* |
| SourceSystem |hello forrás, amely jóváhagyva a hello frissítés telepítését.<br>Lehetséges értékek:<br>– Microsoft Update (Microsoft-frissítés)<br>– Windows Update<br>– SCCM<br>– Linux Servers (Linux-kiszolgálók – a csomagkezelőkből lekért információ alapján) |
| Approved |Meghatározza, hogy hello frissítést jóváhagyták a telepítéshez.<br> Linux-kiszolgálók esetén ez nem kötelező, mivel a javításokat nem az OMS kezeli. |
| Kategóriák Windows esetén |Hello frissítés besorolása.<br>Lehetséges értékek:<br>– Applications (Alkalmazások)<br>– Critical Updates (Kritikus frissítések)<br>– Definition Updates (Definíciófrissítés)<br>– Feature Packs (Funkciócsomag)<br>– Security Updates (Biztonsági frissítések)<br>– Service Packs (Szervizcsomagok)<br>– Update Rollups (Kumulatív frissítések)<br>– Updates (Frissítések) |
| Kategóriák Linux esetén |Cassification hello frissítés.<br>Lehetséges értékek:<br>– Critical Updates (Kritikus frissítések)<br>– Security Updates (Biztonsági frissítések)<br>– Other Updates (Másféle frissítések) |
| Computer |Hello számítógép neve. |
| InstallTimeAvailable |Megadja, hogy hello a telepítéshez szükséges idő a más elérhető-e a telepített ügynökök hello azonos frissítése. |
| InstallTimePredictionSeconds |Várható telepítési idő másodpercben más alapján telepített ügynökök hello ugyanazt a frissítést. |
| KBID |A cikk hello hello KB cikk azonosítója. |
| ManagementGroupName |SCOM-ügynököt hello felügyeleti csoport neve.  Más ügynökök esetén ez AOI-<workspace ID>. |
| MSRCBulletinID |Hello-frissítést leíró hello biztonsági közlemény azonosítója. |
| MSRCSeverity |Hello Microsoft security bulletin súlyossága.<br>Lehetséges értékek:<br>– Critical (Kritikus)<br>– Important (Fontos)<br>– Moderate (Közepes) |
| Optional |Megadja, hogy hello frissítés nem kötelező. |
| Product |Hello termék hello frissítés neve van.  Kattintson a **nézet** tooopen hello cikk a böngészőben. |
| PackageSeverity |Ez a frissítés rögzített hello Linux distro szállítók által jelentett hello biztonsági rés hello súlyossága. |
| PublishDate |Dátum és idő hello frissítés telepítése sikeres volt. |
| RebootBehavior |Itt adhatja meg, ha hello frissítés kényszeríti-e újraindítást.<br>Lehetséges értékek:<br>- canrequestreboot (újraindítást igényelhet)<br>- neverreboots (nem igényel újraindítást) |
| RevisionNumber |Hello frissítés változat száma. |
| SourceComputerId |GUID toouniquely hello számítógép azonosításához. |
| TimeGenerated |Utolsó frissítés dátuma és időpontja hello rekord. |
| Cím |Hello frissítés címe. |
| UpdateID |GUID toouniquely hello frissítés azonosításához. |
| UpdateState |Meghatározza, hogy hello frissítés telepítve van-e ezen a számítógépen.<br>Lehetséges értékek:<br>-Telepített - hello frissítés telepítve van ezen a számítógépen.<br>-Szükséges – hello frissítés nincs telepítve, és ezen a számítógépen van szükség. |

Bármely típusú rekordok visszaadó napló keresési végrehajtásakor **frissítés** kiválaszthatja hello **frissítések** megjelenítése mozaikok hello keresés által visszaadott hello frissítések összefoglalójához nézetben. Rákattinthat a hello hello bejegyzések **hiányzó és alkalmazott frissítések** és **szükséges és választható frissítések** tartalmazó csempék éppen úgy tooscope hello megjelenítése toothat beállítása a frissítések. Jelölje be hello **lista** vagy **tábla** tooreturn hello egyes rekordok megtekintéséhez.<br>

![Naplókeresés, frissítés nézet, Update típusú rekorddal](./media/oms-solution-update-management/update-la-view-updates.png)  

A hello **tábla** nézetet, rákattinthat a hello **KBID** az összes rekord tooopen hello KB cikk egy böngészőt. Ez lehetővé teszi, hogy Ön tooquickly olvassa el hello adott frissítés hello részleteit.<br>

![Naplókeresés, táblázat nézet csempékkel, frissítések típusú rekorddal](./media/oms-solution-update-management/update-la-view-table.png)

A hello **lista** nézetben hello kattint **nézet** hivatkozás tovább toohello KBID tooopen hello KB cikk.<br>

![Naplókeresés, lista nézet csempékkel, frissítések típusú rekorddal](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>UpdateSummary típusú rekordok
Minden Windows-ügynök esetében egy **UpdateSummary** típusú rekord készül. Ez a bejegyzés frissül minden alkalommal, amikor hello számítógép üzemeltet a frissítéseket. **UpdateSummary** rekordok hello jellemzőkkel rendelkezik a következő táblázat hello.

| Tulajdonság | Leírás |
| --- | --- |
| Típus |UpdateSummary |
| SourceSystem |OpsManager |
| Computer |Hello számítógép neve. |
| CriticalUpdatesMissing |Hiányzó hello számítógépen kritikus frissítések száma. |
| ManagementGroupName |SCOM-ügynököt hello felügyeleti csoport neve. Más ügynökök esetén ez AOI-<workspace ID>. |
| NETRuntimeVersion |Hello .NET-futtatókörnyezet hello számítógépre telepített verzióját. |
| OldestMissingSecurityUpdateBucket |Gyűjtő toocategorize hello idő hello legrégebbi hiányzó frissítés ezen a számítógépen közzététele óta.<br>Lehetséges értékek:<br>– Older (Régebbi)<br>– 180 days ago (180 napja)<br>– 150 days ago (150 napja)<br>– 120 days ago (120 napja)<br>– 90 days ago (90 napja)<br>– 60 days ago (60 napja)<br>– 30 days ago (30 napja)<br>– Recent (Friss) |
| OldestMissingSecurityUpdateInDays |Hello legrégebbi hiányzó frissítés ezen a számítógépen közzététele óta eltelt napok száma. |
| OsVersion |Hello számítógépre telepített hello operációs rendszer verzióját. |
| OtherUpdatesMissing |Más hello számítógépen hiányzó frissítések száma. |
| SecurityUpdatesMissing |Hello számítógépen hiányzó biztonsági frissítések száma. |
| SourceComputerId |GUID toouniquely hello számítógép azonosításához. |
| TimeGenerated |Utolsó frissítés dátuma és időpontja hello rekord. |
| TotalUpdatesMissing |Hello számítógépen hiányzó frissítések teljes száma. |
| WindowsUpdateAgentVersion |Hello számítógépen hello Windows Update agent verziószáma. |
| WindowsUpdateSetting |Beállítás a hogyan hello számítógép a fontos frissítések telepítéséhez.<br>Lehetséges értékek:<br>– Disabled (Letiltva)<br>– Notify before installation (Értesítés telepítés előtt)<br>– Scheduled installation (Ütemezett telepítése) |
| WSUSServer |Ha hello számítógép URL-címet a WSUS server egyik toouse konfigurálva. |

## <a name="sample-log-searches"></a>Naplókeresési minták
a következő táblázat hello minta napló keres, ez a megoldás által gyűjtött frissítési rekordok biztosít.

| Lekérdezés | Leírás |
| --- | --- |
| Type:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false &#124; measure count() by Computer |Frissítést igénylő Windows-alapú kiszolgáló-számítógépek |
| Type:Update OSType=Linux UpdateState!="Not needed" &#124; measure count() by Computer |Frissítést igénylő Linux-kiszolgálók | 
| Type=Update UpdateState=Needed Optional=false &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |Minden számítógép, amelyről hiányzik frissítés |
| Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" &#124; select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate |Egy adott számítógépről hiányzó frissítések (cserélje le az értéket a saját számítógépnevére)|
| Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") |Minden számítógép, amelyről kritikus vagy biztonsági frissítés hiányzik | 
| Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual &#124; Distinct Computer} &#124; Distinct KBID |Hiányzó kritikus vagy biztonsági frissítések olyan számítógépeken, amelyeken kézi frissítés van beállítva |
| Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false &#124; Distinct Computer} |Olyan gépek hibaeseményei, amelyeknél kritikus vagy biztonsági szükséges frissítések hiányoznak |
| Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |Minden olyan számítógép, amelynél kumulatív frissítések hiányoznak | 
| Type=Update UpdateState=Needed Optional=false &#124; Egyedi név |Egyedi frissítések minden számítógépnél | 
| Type:UpdateRunProgress InstallationStatus=failed &#124; measure count() by Computer, Title, UpdateRunName |Windows-alapú kiszolgáló-számítógép, amelynek frissítései sikertelenek voltak a frissítés futtatásakor | 
| Type:UpdateRunProgress InstallationStatus=failed &#124; measure count() by Computer, Product, UpdateRunName |Linux-kiszolgáló, amelynek frissítései sikertelenek voltak a frissítés futtatásakor | 
| Type=UpdateSummary &#124; measure count() by WSUSServer |WSUS-t használó számítógépek | 
| Type=UpdateSummary &#124; measure count() by WindowsUpdateSetting |Automatikus frissítési beállítások | 
| Type=UpdateSummary WindowsUpdateSetting=Manual |Azok a számítógépek, amelyeken az automatikus frissítés le van tiltva | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" &#124; measure count() by Computer |A csomag frissítése elérhető rendelkező összes hello Linux számítógépek listája | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") &#124; measure count() by Computer |Elérhető Csomagfrissítés rendelkező összes hello Linux számítógépek listája, amely kritikus vagy biztonsági rést | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" |Minden olyan csomag, amelyhez frissítés érhető el | 
| Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") |Minden olyan csomag, amelyhez kritikus vagy biztonsági rést javító frissítés érhető el | 
| Type:UpdateRunProgress &#124; measure Count() by UpdateRunName |Azon frissítéstelepítések listája, amelyekhez módosított számítógépek találhatóak | 
| Type:UpdateRunProgress UpdateRunName="DeploymentName" &#124; measure Count() by Computer |Az ebben a frissítésfuttatásban frissített számítógépek (cserélje le az értéket a saját frissítéstelepítésének nevére) | 
| Type=Update and OSType=Linux and OSName = Ubuntu &#124; measure count() by Computer |Minden frissítés érhető el az összes hello "Ubuntu" számítógépek listája | 

## <a name="troubleshooting"></a>Hibaelhárítás

Ez a szakasz toohelp elhárítása hello frissítés felügyeleti megoldás információkat nyújt.  

### <a name="how-do-i-troubleshoot-onboarding-issues"></a>Hogyan háríthatom el a bevezetési hibákat?
Ha hibát tapasztal tooonboard hello megoldás vagy egy virtuális gép közben, ellenőrizze a hello **alkalmazások és szolgáltatások Logs\Operations kezelője** Eseménynapló Azonosítójú 4502 és esemény eseményüzenet tartalmazóeseményeit**Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.  hello a következő táblázat az egyes vonatkozó hibaüzeneteket és egy lehetséges megoldás emeli ki.  

| Üzenet | Ok | Megoldás |   
|----------|----------|----------|  
| Nem lehet tooRegister javítás felügyeleti gépen<br>A regisztráció kivétel miatt meghiúsult<br>System.InvalidOperationException: {„Üzenet”:„A gép már<br>regisztrált tooa másik fiókot. "} | Gép már a frissítéskezelés előkészítve tooanother munkaterülete | Karbantartást végez, a régi összetevők által [hello hibrid forgatókönyv-csoport törlése](../automation/automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups)|  
| Nem lehet regisztrálni túl javítás felügyeleti gépen<br>A regisztráció kivétel miatt meghiúsult<br>System.Net.Http.HttpRequestException: Hiba történt a hello kérelem küldése során. ---><br>System.Net.WebException: hello alapul szolgáló kapcsolatot<br>megszakadt: Váratlan hiba<br>történt a fogadó oldalon. ---> System.ComponentModel.Win32Exception:<br>hello ügyfél és a kiszolgáló nem tud kommunikálni,<br>mert nem rendelkeznek közös algoritmussal | A proxy/átjáró/tűzfal blokkolja a kommunikációt | [A rendszerkövetelmények áttekintése](../automation/automation-offering-get-started.md#network-planning)|  
| Nem lehet tooRegister javítás felügyeleti gépen<br>A regisztráció kivétel miatt meghiúsult<br>Newtonsoft.Json.JsonReaderException: Hiba történt a pozitív végtelen érték elemzése közben. | A proxy/átjáró/tűzfal blokkolja a kommunikációt | [A rendszerkövetelmények áttekintése](../automation/automation-offering-get-started.md#network-planning)| 
| hello szolgáltatás hello tanúsítvány <wsid>. oms.opinsights.azure.com<br>nem a Microsoft szolgáltatásaihoz használt<br>hitelesítésszolgáltató bocsátotta ki. Kérjük, érdeklődjön<br>a hálózati rendszergazda toosee, ha a proxy, amely elfogja futnak<br>TLS/SSL-kommunikációt. |A proxy/átjáró/tűzfal blokkolja a kommunikációt | [A rendszerkövetelmények áttekintése](../automation/automation-offering-get-started.md#network-planning)|  
| Nem lehet tooRegister javítás felügyeleti gépen<br>A regisztráció kivétel miatt meghiúsult<br>AgentService.HybridRegistration.<br>PowerShell.Certificates.CertificateCreationException:<br>Nem sikerült toocreate egy önaláírt tanúsítványt. ---><br>System.UnauthorizedAccessException: A hozzáférés megtagadva. | Hiba az önaláírt tanúsítvány létrehozásakor | Ellenőrizze, hogy a rendszerfióknak<br>olvasási hozzáférés toofolder:<br>**C:\ProgramData\Microsoft\**<br>**Crypto\RSA**|  

### <a name="how-do-i-troubleshoot-update-deployments"></a>Hogyan háríthatom el a frissítéstelepítési hibákat?
Az Automation-fiók kapcsolódó hello OMS-munkaterület támogató ebben a megoldásban a hello feladatok paneljéről hello ütemezett frissítés központi telepítésben lévő hello frissítések telepítésével foglalkozó hello runbook hello eredmények megtekinthetők.  hello runbook **javítás-MicrosoftOMSComputer** gyermekrunbook, amely egy adott kezelt számítógépet célozza, és tekintse át a részletes adatfolyam hello jelent, hogy a központi telepítés részletes adatait.  hello kimeneti fog jeleníti meg, amely szükséges frissítései alkalmazhatók, állapot, telepítési állapotát, és további részleteket.<br><br> ![Frissítéstelepítési feladat állapota](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

További információért tekintse meg az [Automation runbook-kimeneteivel és -üzeneteivel](../automation/automation-runbook-output-and-messages.md) kapcsolatos részt.   

## <a name="next-steps"></a>Következő lépések
* A napló kereséssel [Naplóelemzési](../log-analytics/log-analytics-log-searches.md) tooview részletes adatok frissítése.
* [Saját irányítópult létrehozásával](../log-analytics/log-analytics-dashboards.md) megjelenítheti a felügyelt számítógépek frissítési megfelelőségét.
* [Létrehozhat riasztásokat](../log-analytics/log-analytics-alerts.md) a számítógépekről hiányzó kritikus frissítések jelzésére vagy arra az estre, ha egy számítógép automatikus frissítése letiltott állapotba kerül.  
