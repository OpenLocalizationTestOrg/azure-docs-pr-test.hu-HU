---
title: "aaaInstall helyszíni adatátjáró - Azure Logic Apps |} Microsoft Docs"
description: "A helyszíni adatforrások eléréséhez előtt telepítse a hello helyszíni adatátjáró gyors adatátvitel és a titkosításhoz a helyszíni adatforrások és a logic Apps alkalmazások között"
keywords: "adatok, a helyszíni, az adatok átvitele, a titkosítás és a adatforrások eléréséhez"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 01386a904d856ff545f2eca8eb1b008dcdc08574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-on-premises-data-gateway-for-azure-logic-apps"></a>Az Azure Logic Apps hello helyszíni adatátjáró telepítése

A logic apps a helyszíni adatforrások eléréséhez, telepítésekor, és a helyszíni adatátjáró hello beállítása. hello átjáró működik, amely gyors adatátvitel és a titkosítás a helyszínen és a logic Apps alkalmazások közötti hídként. hello átjáró továbbítja a titkosított csatornákon keresztül hello Azure Service Bus helyszíni forrásból származó adatokat. Az összes forgalom származik, mint hello átjáró ügynök biztonságos kimenő forgalmát. További információ [hello adatátjáró működése](#gateway-cloud-service).

hello átjárót a helyszíni kapcsolatok toothese adatforrások támogatja:

*   BizTalk Server 2016
*   DB2  
*   Fájlrendszer
*   Informix
*   MQ
*   MySQL
*   Oracle Database
*   PostgreSQL
*   SAP-alkalmazáskiszolgáló 
*   SAP üzenet kiszolgáló
*   SharePoint
*   SQL Server
*   Teradata

Ezeket a lépéseket megjelenítése hogyan toofirst telepítés hello helyszíni adatátjáró előtt [hello átjáró és a logic Apps alkalmazások közötti kapcsolat beállítása](./logic-apps-gateway-connection.md). További információ a támogatott összekötők: [az Azure Logic Apps összekötők](https://docs.microsoft.com/azure/connectors/apis-list). 

Hogyan toouse hello átjáró más szolgáltatásokkal kapcsolatos információkért lásd: ezek a cikkek:

*   [Microsoft Power BI helyszíni adatátjáró](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Az Azure Analysis Services helyi adatátjáró](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow helyszíni adatátjáró](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps helyszíni adatátjáró](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>
## <a name="requirements"></a>Követelmények

**Minimális**:

* .NET 4.5 keretrendszer
* 64 bites Windows 7 vagy Windows Server 2008 R2 (vagy újabb)

**Ajánlott**:

* 8 mag Processzor
* 8 GB memória
* 64 bites Windows 2012 R2 (vagy újabb)

**Fontos tudnivalók találhatók**:

* Hello helyszíni adatátjáró telepítése csak a helyi számítógépen.
Hello átjáró nem telepíthet tartományvezérlőre.

   > [!TIP]
   > Nincs a hello tooinstall hello átjáró ugyanazon a számítógépen a adatforrásként. toominimize késés, a legközelebb lehetséges tooyour adatforrásként, vagy a hello azonos hello átjárót telepítheti számítógép, feltéve, hogy Ön jogosult.

* Hello átjáró nem telepíthető olyan számítógépre, kikapcsolja, toosleep kerül vagy toohello Internet nem csatlakoztatható, mert a hello átjáró ilyen körülmények nem futtatható. Emellett átjáró teljesítménye csökkenhet, vezeték nélküli hálózaton keresztül.

* A telepítés során be kell jelentkeznie a egy [munkahelyi vagy iskolai fiók](https://docs.microsoft.com/azure/active-directory/sign-up-organization) , amely az Azure Active Directory (Azure AD), nem Microsoft-fiók felügyeli. 

  Rendelkezik toouse hello ugyanazzal a munkahelyi vagy iskolai fiók később a hello Azure portal létrehozásakor, és rendelje hozzá az átjáró telepítése egy átjáró-erőforráshoz. Az átjáró-erőforráshoz, majd válassza ki, a logic app és hello helyszíni adatforrás között hello kapcsolat létrehozásakor. [Miért kell használni az Azure AD munkahelyi vagy iskolai fiókkal?](#why-azure-work-school-account)

  > [!TIP]
  > Ha regisztrált az Office 365 ajánlat, és nem adja meg a tényleges munkahelyi e-mail címét, a bejelentkezési címe nézhet jeff@contoso.onmicrosoft.com. 

* Ha egy meglévő átjárót, a korábbi verziónál 14.16.6317.4 telepítő beállított, az átjáró helyen futó hello legújabb telepítő által nem módosítható. Azonban ehelyett kívánt hello hellyel rendelkező hello legújabb telepítő tooset be egy új átjárót is használhatja.
  
  Ha egy átjáró installer korábbi verziója 14.16.6317.4, de még nem telepítette az átjárót, letöltheti és hello legújabb telepítővel.

<a name="install-gateway"></a>

## <a name="install-hello-data-gateway"></a>Hello adatátjáró telepítése

1.  [Töltse le és futtassa a hello gateway installer a helyi számítógép](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

2. Tekintse át és fogadja el hello használati feltételei és adatvédelmi nyilatkozata.

3. Hello utat adjon meg a helyi számítógépre, amelyre tooinstall hello átjáró.

4. Amikor a rendszer kéri, jelentkezzen be a a Azure munkahelyi vagy iskolai fiókkal, nem Microsoft-fiókkal.

   ![Jelentkezzen be Azure munkahelyi vagy iskolai fiók](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. Most regisztrálja a telepített átjárót hello [átjáró felhőszolgáltatáshoz](#gateway-cloud-service). Válasszon **ezen a számítógépen egy új átjáró regisztrálása**.

   hello átjáró felhőszolgáltatáshoz titkosítja, és tárolja az adatforrás hitelesítő adatai és az átjáró beállításai között. 
   hello szolgáltatást is irányítja a lekérdezések és az eredmények között a Logic Apps alkalmazást, a helyszíni adatátjáró hello és az adatforrás a helyszínen.

6. Adjon meg egy nevet az átjáró telepítése. Hozzon létre helyreállítási kulcsot, majd erősítse meg a helyreállítási kulcsot. 

   > [!IMPORTANT] 
   > A helyreállítási kulcs legalább nyolc karakterből kell állnia. Győződjön meg arról, hogy mentse el, és hello kulcs tartsa biztonságos helyen. Ha azt szeretné, hogy toomigrate, is kell a kulcs visszaállítása, vagy vegye át egy meglévő átjáróhoz.

   1. Válassza ki a toochange hello alapértelmezett régió hello átjáró felhőalapú szolgáltatás és az átjáró telepítése által használt Azure Service Bus **régió módosítása**.

      ![Régió módosítása](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      hello alapértelmezett terület az Azure AD bérlőhöz társított hello régióban.

   2. A következő ablaktábla hello, nyissa meg a hello **válasszon régiót** túl válasszon egy másik régióban.

      ![Válasszon ki egy másik régiót](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      Például akkor lehet, hogy válassza ki a Logic Apps alkalmazást és ugyanabban a régióban hello, vagy válassza hello régió legközelebbi tooyour helyszíni adatforrás, a késés csökkentése érdekében. Az átjáró-erőforráshoz és logikai alkalmazás különböző helyen is rendelkezhetnek.

      > [!IMPORTANT]
      > Ebben a régióban a telepítés után nem módosítható. Ebben a régióban is határozza meg, és korlátozza a hello helyre, ahol az átjáró létrehozhat hello Azure-erőforrás. Ezért az átjáró erőforrás létrehozásakor az Azure-ban győződjön meg arról, hogy hello erőforrás helye megegyezik-e az átjáró telepítésekor kiválasztott hello régióban.
      > 
      > Ha szeretne egy másik régióban toouse az átjáró később, be kell állítania egy új átjárót.

   3. Ha elkészült, válassza ki a **végzett**.

7. Most tegye a következőket a hello Azure-portálon, így [hozzon létre egy Azure-erőforrás az átjáró](../logic-apps/logic-apps-gateway-connection.md). 

További információ [hello adatátjáró működése](#gateway-cloud-service).

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a>Telepítse át, állítsa vissza, vagy vegye át egy meglévő átjáróhoz

Ezek a feladatok tooperform, hello helyreállítási kulcs hello átjáró telepítésekor megadott rendelkeznie kell.

1. A számítógép Start menüből **helyszíni adatátjáró**.

2. Miután hello telepítő megnyílik, jelentkezzen be az hello azonos Azure munkahelyi vagy iskolai fiókkal, amely korábban használt tooinstall hello átjáró.

3. Válasszon **áttelepítése, visszaállítása vagy átvétele egy meglévő átjáróhoz**.

4. Adja meg a kívánt toomigrate, visszaállítása vagy hajtsa végre a megfelelő hello átjáró nevét és a helyreállítási kulcs hello.

<a name="restart-gateway"></a>
## <a name="restart-hello-gateway"></a>Indítsa újra a hello átjáró

hello átjáró fut a Windows-szolgáltatás. Mint bármely más Windows-szolgáltatás indítsa el, és többféle módon hello szolgáltatás leállítása. Például nyisson meg egy parancssort emelt jogosultsági szintű hello hello átjárót futtató számítógépen, és futtassa az alábbi parancsokat vagy:

* toostop hello szolgáltatást, futtassa a parancsot:
  
    `net stop PBIEgwService`

* toostart hello szolgáltatást, futtassa a parancsot:
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a>Windows-szolgáltatásfiók

hello helyszíni adatátjáró toouse be van állítva `NT SERVICE\PBIEgwService` Windows hello szolgáltatás bejelentkezési hitelesítő adatokat. Alapértelmezés szerint hello átjáró hello "Bejelentkezés szolgáltatásként" jogosultsággal rendelkezik hello gép hello átjáró telepítéséhez használt.

> [!NOTE]
> szolgáltatásfiók Windows hello tooon helyi adatforrások csatlakozáshoz használt hello fiókból, illetve a hello Azure munkahelyi értéke vagy iskolai fiókjával használt toosign toocloud szolgáltatásban.

## <a name="configure-a-firewall-or-proxy"></a>Egy tűzfal vagy a proxy konfigurálása

hello átjáró létrehoz egy kimenő kapcsolat túl [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Tekintse meg a proxyadatokat tooprovide az átjáró [konfigurálja a proxybeállításokat](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).

toocheck, hogy a tűzfal, vagy a proxy, előfordulhat, hogy kapcsolat tiltása, győződjön meg róla hogy a gép ténylegesen kapcsolódhat toohello internet és hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Futtassa ezt a parancsot egy PowerShell-parancssorba:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> A parancs csak a hálózati kapcsolatot, valamint kapcsolat toohello Azure Service Bus teszteli. Így hello parancs nem kell semmi toodo hello átjáróval vagy hello átjáró felhőszolgáltatás, amely titkosítja, és tárolja a hitelesítő adatait, és az átjáró beállításai között. 
>
> Ez a parancs is csak a Windows Server 2012 R2 vagy későbbi, és a Windows 8.1 vagy újabb. Az operációs rendszer korábbi verzióiban, használhatja a Telnet túl a kapcsolat tesztelése. További információ [Azure Service Bus vagy hibrid megoldások](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

Az eredmények alábbihoz hasonló toothis példa:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Ha **TcpTestSucceeded** értéke túl**igaz**, előfordulhat, hogy blokkolja tűzfal. Ha azt szeretné, toobe átfogó, helyettesítse hello **számítógépnév** és **Port** felsorolt értékek hello értékekkel [portok konfigurálása](#configure-ports) ebben a témakörben.

hello tűzfal is megakadályozhatja, hogy kapcsolatok adott hello Azure Service Bus révén toohello Azure adatközpontjaiban. Ha ebben a forgatókönyvben, jóváhagyása (feloldása) összes hello ezeket a saját régiójában adatközpontok IP-címet. Azon IP-címek [get hello Azure IP-címek listája itt](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Portok konfigurálása

hello átjáró létrehoz egy kimenő kapcsolat túl[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) és kimenő portok folytat: TCP 443-as (alapértelmezett), 5671, 5672, 9350 – 9354-es. hello átjáró nincs szükség a bejövő portra. További információ [Azure Service Bus vagy hibrid megoldások](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| TARTOMÁNYNEVEK | KIMENŐ PORTOK | LEÍRÁS |
| --- | --- | --- |
| *. analysis.windows.net | 443 | HTTPS | 
| *. login.windows.net | 443 | HTTPS | 
| *. servicebus.windows.net | 5671-5672 | Speciális üzenetsor-kezelési protokoll (AMQP) | 
| *. servicebus.windows.net | 443, 9350-9354 | A Service Bus Relay (a 443-as kér a hozzáférés-vezérlés jogkivonat beszerzése) TCP-n keresztül figyelői | 
| *. frontend.clouddatahub.net | 443 | HTTPS | 
| *. core.windows.net | 443 | HTTPS | 
| login.microsoftonline.com | 443 | HTTPS | 
| *. msftncsi.com | 443 | Hello átjáró nem érhető el Power BI-ban hello által használt tootest internetkapcsolat. | 

Ha tooapprove IP-címek hello tartomány helyett, töltse le és használja hello [Microsoft Azure Datacenter IP-címtartományok lista](https://www.microsoft.com/download/details.aspx?id=41653). Bizonyos esetekben hello Azure Service Bus-kapcsolatokat válnak, teljes tartománynevek helyett IP-címet.

<a name="gateway-cloud-service"></a>
## <a name="how-does-hello-data-gateway-work"></a>Hogyan működik a hello az adatátjárót?

hello adatátjáró elősegíti a Logic Apps alkalmazást, hello átjáró felhőalapú szolgáltatás és a helyszíni adatforrás között gyors és biztonságos kommunikációt. 

![diagram-for-on-premises-Data-Gateway-flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Ezért ha hello felhőben hello felhasználó csatlakozott tooyour elemen kommunikációja a helyszíni adatforrás:

1. hello átjáró felhőszolgáltatáshoz lekérdezést, hello titkosított hitelesítő adatok hello adatforrás, valamint hoz létre, és elküldi a hello lekérdezés toohello várólista hello átjáró tooprocess.

2. hello átjáró felhőszolgáltatáshoz hello lekérdezés elemzi, és leküldéses értesítések hello kérelem toohello Azure Service Bus.

3. hello helyszíni adatátjáró hello Azure Service Bus a függőben lévő kérelmek kérdezi le.

4. hello átjáró hello lekérdezés lekérdezi, visszafejti hello hitelesítő adatokat, és toohello adatforrás összekapcsolja ezeket a hitelesítő adatokat.

5. hello átjáró küldi hello lekérdezési toohello adatforrás végrehajtásra.

6. hello eredmények küldi hello adatforrás hátsó toohello átjáró és toohello átjáró felhőszolgáltatáshoz. hello átjáró felhőszolgáltatáshoz ezután hello eredmények.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Gyakori kérdések

### <a name="general"></a>Általános kérdések

**A Q**: van egy átjáró hello felhőben, például az SQL Azure adatforrások? <br/>
**A**: nem. Egy átjáró tooon helyi adatforrások csak csatlakozik.

**A Q**: rendelkezik hello átjáró hello hello adatforrásként azonos számítógépre telepített toobe? <br/>
**A**: nem. hello átjáró toohello az adatforráshoz megadott hello kapcsolat adatainak felhasználásával csatlakozik. Vegye figyelembe a hello átjáró abban az értelemben, ügyfél-alkalmazásként. hello csak regisztrálnia kell az hello funkció tooconnect toohello kiszolgáló megadott tartománynévvel.

<a name="why-azure-work-school-account"></a>

**A Q**: Miért kell I használja az Azure munkahelyi vagy iskolai fiók toosign a? <br/>
**A**: csak használja az Azure munkahelyi vagy iskolai fiókkal, hello helyszíni az átjáró telepítésekor. A bejelentkezési fiók egy Azure Active Directory (Azure AD) által felügyelt bérlői tárolja. Az Azure AD-fiókot egyszerű felhasználónév (UPN) általában hello e-mail cím megfelelő.

**A Q**: a hitelesítő adataimat tároló? <br/>
**A**: hello adatforráshoz beírt hitelesítő adatok titkosítottak és a hello átjáró felhőszolgáltatásban tárolt. hello hitelesítő adatok lesznek visszafejtve hello a helyszíni adatok átjáróra.

**A Q**: vannak-e hálózati sávszélesség vonatkozó követelmények? <br/>
**A**: javasoljuk, hogy rendelkezik-e a hálózati kapcsolat megfelelő teljesítményt. Minden környezet más, és elküldött adatok mennyisége hello hello eredményekre van hatással. ExpressRoute használata segíthet a helyszíni és az Azure adatközpontjaiban hello közötti átviteli szintű tooguarantee.
Hello külső eszköz Azure sebesség teszt app toohelp mérőműszer használhatja a teljesítményt.

**A Q**: Mi az az hello késés futó lekérdezések tooa adatforrás hello átjáró? Mi az a legjobb architektúra hello? <br/>
**A**: tooreduce hálózati késés, telepítés hello átjáró lehető Bezárás toohello adatforrásként. Hello tényleges adatforrás hello átjáró telepíthető, ha a közelségi kapcsolat minimálisra hello késés rendszerben jelent meg. Érdemes lehet túl hello adatközpontokban. Például ha a szolgáltatás hello USA nyugati régiója datacenter használ, és egy Azure virtuális Gépen található SQL-kiszolgálóval rendelkezik, az Azure virtuális gép kell hello USA nyugati régiója túl. A közelségi kapcsolat minimálisra csökkenti a késést, és ezzel elkerülheti a kimenő forgalom költségek hello Azure virtuális gép.

**A Q**: hogyan történik az eredmények küldött vissza toohello felhő? <br/>
**A**: eredmények hello Azure Service Bus keresztül zajlik.

**A Q**: vannak-e a bejövő kapcsolatok toohello átjáró hello felhőből? <br/>
**A**: nem. hello átjáró kimenő kapcsolatok tooAzure Service Bus használja.

**A Q**: Mi történik, ha az általam letiltottak kimenő kapcsolatokat? Mire van szükségem az tooopen? <br/>
**A**: hello portok és a gazdagépekhez, amelyek hello átjárót használja.

**A Q**: hello tényleges Windows-szolgáltatás úgynevezett?<br/>
**A**: A szolgáltatások a hello átjáró a Power BI Enterprise Gateway szolgáltatás nevezik.

**A Q**: is hello átjáró Windows-szolgáltatás Azure Active Directory-fiókkal való futtatása? <br/>
**A**: nem. Windows-szolgáltatás hello rendelkeznie kell egy érvényes Windows-fiókot. Alapértelmezés szerint hello szolgáltatás SID NT SERVICE\PBIEgwService hello szolgáltatást futtatja.

### <a name="high-availability-and-disaster-recovery"></a>Magas rendelkezésre állás és vészhelyreállítás

**A Q**: milyen beállítások érhetők el a vész-helyreállítási? <br/>
**A**: hello helyreállítási kulcs toorestore használhatja, vagy helyezze át az átjárót. Hello átjáró telepítésekor adja meg a hello helyreállítási kulcsot.

**A Q**: Miért érdemes hello hello helyreállítási kulcs? <br/>
**A**: hello helyreállítási kulcs tartalmaz egy módja toomigrate, vagy az átjáró beállításainak katasztrófa utáni helyreállításhoz.

**A Q**: vannak-e a magas rendelkezésre állás elérésére hello átjáróval engedélyező tervekről? <br/>
**A**: ezek a forgatókönyvek szerepelnek a hello terv, de még nincs ütemterv.

## <a name="troubleshooting"></a>Hibaelhárítás

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**A Q**: hogyan láthatja meg, hogy milyen lekérdezések küldési toohello helyszíni adatforrás? <br/>
**A**: engedélyezheti a lekérdezés nyomon követése, beleértve a küldött hello lekérdezések. Ne felejtse el vissza toohello eredeti érték Miután befejezte a hibakeresési nyomkövetés toochange lekérdezés. A lekérdezés nyomon követése engedélyezve van a Kilépés hoz létre a nagyobb naplókat.

Is megtekintheti, hogy az adatforrás rendelkezik a nyomkövetési lekérdezések eszközök. Például használhatja bővített eseményektől vagy SQL Profiler az SQL Server és az Analysis Services.

**A Q**: Ha az hello átjáró naplói nem? <br/>
**A**: eszközök lásd a témakör későbbi részében.

### <a name="update-toohello-latest-version"></a>Frissítés toohello legújabb verziója

Számos problémájának is surface, amikor hello átjáró verziója elavult. Jó általános gyakorlat győződjön meg arról, hogy hello legújabb verzióját használja-e. Ha hello átjáró havonta vagy hosszabb ideig nem frissítette, akkor előfordulhat, hogy hello hello átjáró legújabb verzióját telepítse, és ha Reprodukálja hello probléma.

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a>Hiba: Nem sikerült tooadd felhasználói toogroup. (Felhasználói-2147463168 PBIEgwService teljesítmény)

Ez a hiba kaphat, ha tooinstall hello átjáró tartományvezérlőn, amely nem támogatott. Győződjön meg arról, hogy egy számítógép, amely a tartományvezérlő nem hello-átjáró üzembe helyezéséhez.

## <a name="tools"></a>Eszközök

### <a name="collect-logs-from-hello-gateway-configurer"></a>Hello átjáró configurer naplóinak gyűjtése

Hello átjáró több naplók hozhatja létre. Always start hello naplók!

#### <a name="installer-logs"></a>Telepítési naplókat

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfigurációs naplók

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Vállalati átjáró service naplóit

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Eseménynaplók

Hello az adatkezelési átjáró és PowerBIGateway naplók alapján is megtalálhatja **alkalmazás- és szolgáltatásnaplók**.

### <a name="fiddler-trace"></a>Fiddler nyomkövetési

[Fiddler](http://www.telerik.com/fiddler) van egy ingyenes eszközt, amely figyeli a HTTP-forgalom Telerik. A Power BI-ban hello ügyfél gépről hello forgalom tekintheti meg. Ez a szolgáltatás előfordulhat, hogy a hibák és egyéb kapcsolódó információk megjelenítése

## <a name="next-steps"></a>Következő lépések
    
* [Csatlakozás tooon helyszíni adatokhoz a logic Apps alkalmazásokból](../logic-apps/logic-apps-gateway-connection.md)
* [Vállalati integrációs szolgáltatások](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Az Azure Logic Apps összekötők](../connectors/apis-list.md)
