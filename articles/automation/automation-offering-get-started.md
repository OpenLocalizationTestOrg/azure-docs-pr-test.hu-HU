---
title: "aaa Ismerkedés az Azure Automation szolgáltatásban |} Microsoft Docs"
description: "Ez a cikk áttekintést Azure Automation szolgáltatás előkészítése tooonboard hello Azure piactérről ajánlat hello tervezésének és megvalósításának részletei alapján."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 434e8ea28c55ff9bda1d2e46a7a6b8378a3baa0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation"></a>Bevezetés az Azure Automation használatába

Az első lépésekről szóló útmutatót az alapvető fogalmakat kapcsolódó toohello telepítése Azure Automation vezet be. Ha új tooAutomation az Azure-ban vagy már ismerik a automation munkafolyamat szoftverek, mint a System Center Orchestrator, az útmutató segítségével megtudhatja, hogy hogyan tooprepare és az alaplapi automatizálás.  Ezután lesz kell előkészített toobegin fejlesztése runbookok folyamat automation igényei támogatásához. 


## <a name="automation-architecture-overview"></a>Az Automation-architektúra áttekintése

![Az Azure Automation áttekintése](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Azure Automation a szoftver, mint egy szolgáltatott szoftverként (SaaS) alkalmazást, amely méretezhető és megbízható, több-bérlős környezet tooautomate dolgozza fel a runbookokkal és kezelheti a konfigurációs módosítások tooWindows és Linux rendszerek célállapot-konfiguráció használata (DSC) Azure, más felhőalapú szolgáltatásokat, vagy a helyszínen. Az Automation-fiókban tárolt entitások, például a runbookok, az adategységek, és a futtató fiókok elkülönülnek az adott előfizetésen és az egyéb előfizetéseken belüli Automation-fiókoktól.  

Az Azure-ban futtatott runbookok Automation-tesztkörnyezetekben futnak, amelyek szolgáltatásként nyújtott platformalapú (PaaS) Azure virtuális gépeken üzemelnek.  Az Automation-tesztkörnyezetek a runbookok futtatásának minden aspektusában biztosítják a bérlők elkülönítését, legyen szó modulokról, tárterületről, memóriáról, hálózati kommunikációról, feladatstreamekről stb. Ez a szerepkör hello szolgáltatás kezeli, és nem érhető el az Azure vagy az Azure Automation-fiókjához, toocontrol.         

tooautomate hello telepítése és kezelése a helyi adatközpontban, illetve az Automation-fiók létrehozása után más felhőszolgáltatásaival erőforrások jelölhet ki egy vagy több gépek toorun hello [hibrid forgatókönyv-feldolgozó (HRW)](automation-hybrid-runbook-worker.md) szerepkör.  Minden egyes HRW hello Microsoft Management Agent ügynököt a kapcsolat tooa Naplóelemzési munkaterület és Automation-fiók szükséges.  Naplóelemzési egy használt toobootstrap hello telepített, karbantartása hello Microsoft Management Agent ügynököt, és figyelje a hello HRW hello funkcióit.  Azure Automation hajtja végre őket a runbookok és hello utasítás toorun hello kézbesítését.

A runbookok telepítése több HRW tooprovide magas rendelkezésre állású, egyenleg runbook feladatok betöltése, és egyes esetekben jelölt ki azokat az adott munkaterhelés vagy környezetekben.  hello HRW a Microsoft Monitoring Agent hello kezdeményezi a kommunikációt az Automation szolgáltatás hello 443-as TCP-porton keresztül, és nincs tűzfal bejövő vonatkozó követelmény.  Miután egy HRW hello környezeten belül futó runbook rendelkezik, és szeretné, hogy hello runbook tooperform felügyeleti feladatok szemben a más gépeket vagy szolgáltatásokat, hogy a környezetben, lehet, hogy van más hello portokat kell runbook hozzá kell férnie.  Ha az IT-biztonsági házirendeknek nem engedélyezi a hálózati tooconnect toohello Internet a számítógépek, tekintse át a hello cikk [OMS átjáró](../log-analytics/log-analytics-oms-gateway.md), mely hello HRW toocollect proxyjaként tevékenységéért feladat állapota, és fogadni a konfigurációs adatait az Automation-fiók.

Egy HRW hello hello hello számítógépen helyi rendszerfiók környezetében futnak a futó Runbookok, amelyek hello ajánlott biztonsági környezet hello helyi számítógépen a Windows felügyeleti műveletek végrehajtása során. Ha azt szeretné, hogy hello runbook toorun feladatok kívül a helyi számítógép hello erőforrásokon, szükség lehet a biztonságos hitelesítő adatok eszközök toodefine hello Automation-fiók, hogy hello runbookból elérheti és tooauthenticate használata hello külső erőforrás. Használhat [hitelesítő adat](automation-credentials.md), [tanúsítvány](automation-certificates.md), és [kapcsolat](automation-connections.md) eszközök parancsmagokat, amelyek lehetővé teszik toospecify hitelesítő adatokat, azokat hitelesítheti a runbookban.

Tárolódnak az Azure Automation DSC-konfigurációk közvetlenül alkalmazott tooAzure virtuális gépek is lehet. Egyéb fizikai és virtuális gépek használói kérhetnek konfigurációk hello Azure Automation DSC lekérési kiszolgálójával.  A helyi fizikai vagy virtuális Windows és Linux rendszerek konfigurációk kezelése, nincs szükség toodeploy bármilyen infrastruktúra toosupport hello Automation DSC lekérési kiszolgálójával, csak kimenő Internet-hozzáférést a minden rendszer toobe kezeli az Automation DSC ,-en keresztüli kommunikációra TCP 443-as porton toohello OMS szolgáltatáshoz.   

## <a name="prerequisites"></a>Előfeltételek

### <a name="automation-dsc"></a>Automation DSC
Az Azure Automation DSC lehet használt toomanage különböző gépek:

* Windows vagy Linux rendszerű (klasszikus) Azure-beli virtuális gépek
* Windows vagy Linux rendszerű Azure-beli virtuális gépek
* Windows vagy Linux rendszerű Amazon Web Services (AWS) virtuális gépek
* Fizikai/virtuális Windows rendszerű számítógépek a helyszínen, illetve nem Azure- és AWS-alapú felhőben
* Fizikai/virtuális Linux-számítógépek a helyszínen, illetve nem Azure- és AWS-alapú felhőben

a WMF 5 legújabb verziójának hello hello PowerShell DSC ügynök Windows toobe toocommunicate tudja az Azure Automation szolgáltatásban, a rendszerre telepíthető. hello legújabb verziójának hello [PowerShell DSC Linux-ügynök](https://www.microsoft.com/en-us/download/details.aspx?id=49150) Linux toobe képes toocommunicate Azure Automation telepítve kell lennie.

### <a name="hybrid-runbook-worker"></a>hibrid runbook-feldolgozó  
Egy számítógép toorun hibrid forgatókönyv kijelölésekor feladatok, a számítógép hello következő kell rendelkeznie:

* Windows Server 2012 vagy újabb
* Windows PowerShell 4.0 vagy újabb.  Azt javasoljuk, hogy a Windows PowerShell 5.0 telepítése nagyobb megbízhatóságot hello számítógépre. Hello új verziója letölthető hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=50395)
* Minimum két mag
* Legalább 4 GB RAM

### <a name="permissions-required-toocreate-automation-account"></a>Toocreate Automation-fiók szükséges jogosultságok
toocreate vagy frissítés Automation-fiók, rendelkeznie kell a következő bizonyos jogokat hello és engedélyek szükséges toocomplete ebben a témakörben.   
 
* A sorrend toocreate Automation-fiók, az AD-felhasználói fiókot igények toobe hozzáadott tooa szerepkör-engedélyek egyenértékű toohello tulajdonosi szerepkör Microsoft.Automation erőforrások cikkben leírt módon történő [szerepköralapú hozzáférés-vezérlés az Azure Automationben ](automation-role-based-access-control.md).  
* Ha hello App regisztrációk beállítás értéke túl**Igen**, az Azure AD-bérlő nem rendszergazdai felhasználók számára is [AD-alkalmazásokat regisztrálni](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Ha hello app regisztrációk beállítás értéke túl**nem**, a művelet végrehajtása hello felhasználói globális rendszergazdának kell lennie az Azure ad-ben. 

Ha nem tagja Active Directory-példányban hello előfizetés toohello globális rendszergazda/járművezető-administrator szerepkör hello előfizetés vannak adása előtt, akkor tooActive Directory vendégként kerülnek. Ebben az esetben a megjelenik a "nem rendelkezik engedélyekkel toocreate..." Figyelmeztetés-hello **Automation-fiók hozzáadása** panelen. Felhasználók, akik toohello globális rendszergazda/járművezető-administrator szerepkör először eltávolíthatja hello előfizetés Active Directory-példányban, és újból hozzáadták toomake hozzáadták azokat a teljes felhasználói az Active Directoryban. tooverify ebben az esetben a hello **Azure Active Directory** hello Azure portálon, válassza a panelen **felhasználók és csoportok**, jelölje be **minden felhasználó** és hello kiválasztása után adott felhasználó, jelölje be **profil**. hello értékének hello **felhasználótípust** attribútum hello felhasználó profil alapján nem értékűnek kell lennie **vendég**.

## <a name="authentication-planning"></a>A hitelesítés tervezése
Azure Automation szolgáltatással tooautomate feladatok erőforrásokon Azure, a helyszínen, és más szolgáltatók.  Ahhoz, hogy egy runbook tooperform a szükséges műveleteket kell rendelkeznie engedélyek toosecurely hello erőforrások eléréséhez szükséges hello előfizetésen belül hello minimális jogosultságokkal.  

### <a name="what-is-an-automation-account"></a>Mi az az Automation-fiók? 
Minden hello automatizálási feladatok hello Azure-parancsmagokat használ az Azure Automationben erőforrásokon Azure Active Directory szervezeti azonosító hitelesítőadat-alapú hitelesítést használó tooAzure hitelesítéséhez.  Automation-fiók nem azonos hello fiókra toohello portál tooconfigure toosign használata Azure-erőforrások használatára.  Egy olyan fiókkal szerepeltetett Automation erőforrásokat hello következő:

* **Tanúsítványok** – runbookból vagy DSC-konfigurációból történő hitelesítéshez vagy azok hozzáadásához használt tanúsítványt tartalmaz.
* **Kapcsolatok** -hitelesítés és a konfiguráció szükséges adatokat tooconnect tooan külső szolgáltatás vagy alkalmazás a runbookot vagy a DSC-konfiguráció tartalmazza.
* **Hitelesítő adatok** – például egy felhasználónév és jelszó szükséges hitelesítő adatokat tartalmazó PSCredential objektum tooauthenticate egy runbookot vagy a DSC-konfiguráció.
* **Integrációs modulok** -rendszer részét képező parancsmagok runbookok és a DSC-konfigurációk belül az egy Azure Automation toomake használatának PowerShell-modulok.
* **Ütemezések** – a runbookokat adott időpontban elindító vagy leállító ütemezéseket tartalmaz, beleértve az ismétlődő gyakoriságot is.
* **Változók** – runbookból vagy DSC-konfigurációból elérhető értékeket tartalmaznak.
* **A DSC-konfigurációk** -PowerShell-parancsfájlok, amely ismerteti, hogyan tooconfigure az operációs rendszer szolgáltatása vagy beállítása, vagy egy alkalmazás telepítése a Windows vagy Linux számítógépen.  
* **Runbookok** – feladatkészletek, amelyek bizonyos automatizált folyamatokat hajtanak végre az Azure Automationben a Windows PowerShell alapján.    

hello Automation erőforrások minden Automation-fiókhoz társított egyetlen Azure-régió, de az Automation-fiók kezelhető az összes hello erőforrást az előfizetésében. Különböző régiókban Automation-fiók létrehozása, ha házirendekben, amelyek az adatok és erőforrások toobe adott elkülönített tooa-terület szükséges.

> [!NOTE]
> Automation-fiókok és hello erőforrásokat tartalmaznak, amelyek hello Azure-portálon létrehozott, a klasszikus Azure portálon hello nem érhető el. Ha azt szeretné toomanage ezeket a fiókokat, vagy a Windows PowerShell-lel erőforrásaikat, hello Azure Resource Manager modulok kell használnia.
> 

Automation-fiók létrehozásakor a hello Azure-portál automatikusan két hitelesítési entitás hozza létre:

* Egy futtató fiók. Ez a fiók létrehoz egy egyszerű szolgáltatást az Azure Active Directory-ban (Azure AD), valamint egy tanúsítványt. Azt is rendel hello közreműködői szerepköralapú hozzáférés-vezérlést (RBAC), amely kezeli a Resource Managerhez tartozó erőforrások runbookok használatával.
* Egy klasszikus futtató fiókot. Ez a fiók feltölt egy felügyeleti tanúsítványt, amely használt toomanage hagyományos erőforrások runbookok használatával.

Szerepköralapú hozzáférés-vezérlés érhető el az Azure Resource Manager toogrant engedélyezett műveletek tooan Azure AD-felhasználói fiókot és a Futtatás mint fiókot, és a szolgáltatás egyszerű hitelesítést.  Olvasási [szerepköralapú hozzáférés-vezérlés az Azure Automation-cikkben](automation-role-based-access-control.md) további tájékoztatást talál toohelp fejlesztése a kezelésének Automation engedélyek modelljét.  

#### <a name="authentication-methods"></a>Hitelesítési módszerek
hello alábbi táblázat foglalja össze különböző hitelesítési módszerekkel hello Azure Automation által támogatott környezetben.

| Módszer | Környezet 
| --- | --- | 
| Azure futtató és klasszikus futtató fiókok |Azure Resource Manager és klasszikus Azure üzemelő példány |  
| Azure AD felhasználói fiók |Azure Resource Manager és klasszikus Azure üzemelő példány |  
| Windows-hitelesítés |Helyi adatközpontban, illetve a más felhőszolgáltatóként hello hibrid forgatókönyv-feldolgozók használata |  
| AWS hitelesítő adatok |Amazon webszolgáltatások |  

A hello **hogyan to\Authentication és biztonsági** szakaszban, támogatja a cikkek biztosító áttekintése és megvalósítási lépéseket tooconfigure hitelesítés az adott környezetben, vagy egy meglévő vagy új fiókot, Ezek az adott környezetben.  Hello Azure-beli futtató és a klasszikus futtató fiókot, a témakör hello [frissítés Automation Futtatás mint fiók](automation-create-runas-account.md) ismerteti, hogyan tooupdate a meglévő Automation-fiókot a hello futtató fiókok hello portál vagy a PowerShell használatával, ha az nem volt eredetileg konfigurálták a Futtatás mint vagy a klasszikus futtató fiókkal. Ha azt szeretné toocreate egy olyan futtató és a klasszikus futtató fiókot a vállalati hitelesítésszolgáltató (CA) által kiadott tanúsítvánnyal, tekintse át a cikk toolearn hogyan toocreate hello fiókok ezt a konfigurációt.     
 
## <a name="network-planning"></a>Hálózattervezés
Hello hibrid forgatókönyv-feldolgozó tooconnect tooand regisztrálása a Microsoft Operations Management Suite (OMS), az access toohello portszámot kell rendelkeznie, és hello URL-címeket az alábbiakban.  Ez a továbbá toohello [portok és a szükséges URL-címek hello Microsoft-Figyelőügynök](../log-analytics/log-analytics-windows-agents.md#network) tooconnect tooOMS. Ha hello ügynök és hello OMS közötti kommunikáció proxykiszolgálót használ, meg kell, hogy elérhetők-e a megfelelő erőforrásokon hello tooensure. Ha a tűzfal toorestrict Internet access toohello használja, tooconfigure a tűzfal toopermit hozzáférés szükséges.

hello információ alatti lista hello port és URL-címek hello hibrid forgatókönyv-feldolgozó toocommunicate az Automation szolgáltatásban, a szükséges.

* Port: a kimenő internetkapcsolathoz csak a 443-as TCP port szükséges
* Globális URL: *.azure-automation.net

Ha egy adott területre definiált Automation-fiók rendelkezik, és azt szeretné, hogy a regionális adatközpontok toorestrict kommunikál, hello következő táblázat hello DNS-rekord mindegyik régióhoz.

| **Régió** | **DNS-rekord** |
| --- | --- |
| USA déli középső régiója |scus-jobruntimedata-prod-su1.azure-automation.net |
| USA 2. keleti régiója |eus2-jobruntimedata-prod-su1.azure-automation.net |
| USA nyugati középső régiója | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Nyugat-Európa |we-jobruntimedata-prod-su1.azure-automation.net |
| Észak-Európa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Közép-Kanada |cc-jobruntimedata-prod-su1.azure-automation.net |
| Délkelet-Ázsia |sea-jobruntimedata-prod-su1.azure-automation.net |
| Közép-India |cid-jobruntimedata-prod-su1.azure-automation.net |
| Kelet-Japán |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Délkelet-Ausztrália |ase-jobruntimedata-prod-su1.azure-automation.net |
| Az Egyesült Királyság déli régiója | uks-jobruntimedata-prod-su1.azure-automation.net |
| USA-beli államigazgatás – Virginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Nevek helyett IP-címek listájának megtekintéséhez töltse le, és tekintse át a hello [Azure Datacenter IP-cím](https://www.microsoft.com/download/details.aspx?id=41653) hello Microsoft Download Center az XML-fájl. 

> [!NOTE]
> Ez a fájl hello használt IP-címtartományok (beleértve a számítási, SQL-és tárolási) a Microsoft Azure Adatközpontjaiban hello tartalmazza. A frissített fáljra visszaküldi hetente amely tükrözi a jelenleg telepített hello tartományokkal és a jövőbeni változtatásokról toohello IP-címtartományok. Új tartományok hello fájlban szereplő nem használható hello adatközpontokban legalább egy hétig. Adja a letöltési hello új xml minden héten fájlt, és hajtsa végre a szükséges módosításokat hello a következőn: toocorrectly azonosításához az Azure-ban futó szolgáltatásokat. Expressz útvonal előfordulhat, hogy figyelembe ezt a fájlt tooupdate hello BGP-hirdetményt a hello Azure terület minden hónap első hetében használja. 
> 

## <a name="creating-an-automation-account"></a>Automation-fiók létrehozása

Többféleképpen hello Azure-portálon létrehozhat egy Automation-fiók.  a következő táblázat hello üzembe helyezését és a kettő közötti vezet be.  

|Módszer | Leírás |
|-------|-------------|
| Jelölje be automatizálási és vezérlő hello piactér | Az ajánlat, amely létrehoz egy Automation-fiók és az OMS-munkaterület kapcsolódó tooone másik hello ugyanazt az erőforráscsoportot és régióban.  OMS integrációja is magában foglalja a Naplóelemzési toomonitor használatának előnye hello és elemez runbook feladat állapotát és a feladat adatfolyamok időbeli és szolgáltatás használ a speciális funkciók tooescalate vagy vizsgálja meg a problémákat. hello ajánlat is telepíti a hello változáskövetési & frissítéskezelés megoldások, amelyek alapértelmezés szerint engedélyezve vannak. |
| Válassza ki Automation hello piactér | Létrehoz egy új vagy meglévő erőforráscsoportot, ami nem csatolt tooan OMS-munkaterület, és nem tartalmaz minden elérhető megoldásokat hello Automation & vezérlő ajánlat az Automation-fiók. Ez egy alapkonfiguráció tooAutomation szolgáltatásban, és megtudhatja, hogyan toowrite runbookokat, a DSC-konfigurációk és használatra hello hello szolgáltatás képességeket nyújt segítséget. |
| Kiválasztott felügyeleti megoldások | Ha a megoldás –  **[frissítéskezelés](../operations-management-suite/oms-solution-update-management.md)**,  **[indítása/leállítása virtuális gépek során munkaidőn kívüli](automation-solution-vm-management.md)**, vagy  **[ A változáskövetés](../log-analytics/log-analytics-change-tracking.md)**  tooselect egy meglévő Automation és az OMS-munkaterület kéri, vagy akkor is, az előfizetésében telepített hello megoldás toobe kötelező beállítás toocreate hello kínálnak. |

Ez a témakör bemutatja, hogyan bevezetési hello Automation & vezérlő ajánlat az Automation-fiók és az OMS-munkaterület létrehozása.  Automation-fiók tesztelési, illetve toopreview hello szolgáltatás, a következő cikkben tekintse át hello önálló toocreate [önálló Automation-fiók létrehozása](automation-create-standalone-account.md).  

### <a name="create-automation-account-integrated-with-oms"></a>OMS-sel integrált Automation-fiók létrehozása
hello ajánlott módszer tooonboard Automation hello Automation & vezérlő ajánlat kijelölésével hello piactér van.  Ez mindkét automatizálási fiókot hoz létre, és létrehozza az OMS-munkaterület, beleértve a hello beállítás tooinstall hello megoldások hello ajánlat elérhető hello integrációja.  

1. Bejelentkezés toohello egy olyan fiókkal, amely hello előfizetés-Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként Azure-portálon.

2. Kattintson az **Új** lehetőségre.<br><br> ![Válassza az Új lehetőséget az Azure Portalon](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  

3. Keresse meg **Automation** és majd hello a keresési eredményeket megjelenítő válassza **Automation & vezérlő***.<br><br> ![Az Automatizálás és vezérlés elem keresése és kiválasztása a Marketplace-en](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png).<br>   

4. Olvasási hello ajánlat hello leírását, után kattintson a **létrehozása**.  

5. A hello **Automation & vezérlő** beállítási paneljén válassza **OMS-munkaterület**.  A hello **OMS-munkaterület** panelen válassza ki az OMS társított munkaterület toohello azonos Azure-előfizetéshez Automation-fiók hello van, vagy hozzon létre egy OMS-munkaterület.  Ha nem rendelkezik az OMS-munkaterület, válassza ki a **új munkaterület létrehozása** a hello **OMS-munkaterület** panel hello következőket hajthatja végre: 
   - Adjon meg egy nevet az új hello **OMS-munkaterület**.
   - Válassza ki a **előfizetés** toolink tooby kijelölésével hello legördülő lista esetén hello alapértelmezett beállítás nem megfelelő.
   - Az **Erőforráscsoport** területen létrehozhat egy erőforráscsoportot, vagy kiválaszthat egy meglévőt.  
   - Válasszon ki egy **helyet**.  Hello csak helyeken érhető el jelenleg **Ausztrália délkeleti**, **USA keleti régiója**, **Délkelet-Ázsia**, **nyugati középső Régiójában**, és  **Nyugat-Európában**.
   - Válasszon egy tarifacsomagot a **Tarifacsomag** területen.  a két réteg tartományregisztráció hello megoldás: Szabadítson fel, és egy csomópont (OMS) szinten.  ingyenes szint hello hello összeg naponta, megőrzési időtartam és runbook-feladat futásidejű perc adatgyűjtés van korlátozva.  hello / csomópont (OMS) réteg nincs maximális hello naponta gyűjtött adatok mennyiségét.  
   - Válassza az **Automation-fiók** elemet.  Új OMS-munkaterület létrehozásakor, a következőket kell végrehajtania tooalso, amely kapcsolódik hello új OMS-munkaterület megadott korábbi, beleértve a Azure-előfizetéssel, az erőforráscsoport és a terület az Automation-fiók létrehozása.  Kiválaszthatja **Automation-fiók létrehozása** a hello **Automation-fiók** panelen adja meg a következő hello: 
  - A hello **neve** mezőbe írja be a hello hello Automation-fiók nevét.

    Egyéb beállítások automatikusan fel van töltve hello OMS-munkaterület kijelölt alapján, és ezek a beállítások nem módosíthatók.  Egy Azure-beli futtató fiókot a hello alapértelmezett hitelesítési módszer a hello ajánlat.  Miután rákattintott **OK**, hello konfigurációs beállításokat érvényesíti, és hello Automation-fiók jön létre.  A folyamat állapotát nyomon követheti **értesítések** hello menüből. 

    Ellenkező esetben válasszon egy meglévő Automation futtató fiókot.  választja hello fiók már nem lehet csatolt tooanother OMS-munkaterület, ellenkező esetben számára jelenik meg egy értesítési üzenetet hello panelen.  Már kapcsolódik, ha egy másik Automation Futtatás mint fiók tooselect kell, vagy hozzon létre egyet.

    Szükséges hello adatokat befejezése után kattintson **létrehozása**.  hello információ ellenőrzése és hello Automation-fiókot és futtató fiókok létrehozásához szükségesek.  A rendszer visszairányítja toohello **OMS-munkaterület** panel automatikusan.  

6. A hello hello szükséges információkat biztosít után **OMS-munkaterület** panelen kattintson a **létrehozása**.  Hello információk ellenőrizve, és hello munkaterület jön létre, nyomon követheti a folyamat állapotát **értesítések** hello menüből.  A rendszer visszairányítja toohello **megoldás hozzáadása** panelen.  

7. A hello **Automation & vezérlő** beállítási paneljén jóváhagyásához tooinstall hello ajánlott az előre kijelölt megoldásokat. Ha bármelyiket kihagyja, később külön is telepítheti.  

8. Kattintson a **létrehozása** tooproceed bevezetési automatizálási és az OMS-munkaterület. Minden beállítás ellenőrzését, és majd megkísérel toodeploy hello ajánlat az előfizetésében.  A folyamat eltarthat néhány másodpercig toocomplete, és nyomon követheti a folyamat állapotát **értesítések** hello menüből. 

Miután hello ajánlat előkészítve, runbookokat hello használata engedélyezte a megoldások létrehozásának megkezdéséhez, központi telepítése egy [hibrid forgatókönyv-feldolgozó](automation-hybrid-runbook-worker.md) szerepkör, illetve a munka megkezdéséhez [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics) a felhő vagy a helyszíni környezetben erőforrások által létrehozott toocollect adatokat.   

## <a name="next-steps"></a>Következő lépések
* Ellenőrizheti, hogy az új Automation-fiók el tudja végezni a hitelesítést az Azure-erőforrásokkal: [Azure Automation futtató fiók hitelesítésének tesztelése](automation-verify-runas-authentication.md).
* a runbookok létrehozása használatába tooget, először tekintse át a hello [automatizálási runbook típusokat](automation-runbook-types.md) támogatott és a kapcsolódó szempontok, mielőtt elkezdené a szerzői műveletekhez.


