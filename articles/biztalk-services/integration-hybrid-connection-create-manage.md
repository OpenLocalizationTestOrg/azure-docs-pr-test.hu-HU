---
title: "aaaCreate és hibrid kapcsolatok kezelése |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy hibrid kapcsolat hello kapcsolat kezelése és hello Hybrid Connection Manager telepítéséhez. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 561d8f3dd97318130a05c3bb2874ee8022e7f417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Hibridkapcsolatok létrehozása és felügyelete

> [!IMPORTANT]
> A BizTalk Hybrid Connections ki van vezetve, helyét az App Service Hybrid Connections vette át. További információt, beleértve a hogyan toomanage meglévő BizTalk a hibrid kapcsolat: [Azure App Service hibrid kapcsolatok](../app-service/app-service-hybrid-connections.md).


## <a name="overview-of-hello-steps"></a>Hello lépéseket áttekintése
1. A hibrid kapcsolat létrehozása hello megadásával **állomásnév** vagy **FQDN** hello helyszíni erőforrás a magánhálózaton.
2. Kapcsolódás az Azure-webalkalmazásokban vagy az Azure mobile apps toohello a hibrid kapcsolat.
3. Hello Hybrid Connection Manager telepíthető a helyi erőforrás, és csatlakozzon a toohello adott hibrid kapcsolat. hello Azure portál egy kattintással indítható élmény tooinstall biztosít, és csatlakozzon.
4. Hibrid kapcsolatok és a kapcsolat kulcsok kezelése.

Ez a témakör az alábbi lépéseket. 

> [!IMPORTANT]
> Már lehetséges tooset egy hibrid kapcsolat végpont tooan IP-címet. Ha egy IP-címet használ, előfordulhat, hogy, vagy nem jut el hello helyszíni erőforrás, attól függően, hogy az ügyfél. a hibrid kapcsolat hello hello ügyfél DNS-címkeresést Ez függ. A legtöbb esetben hello **ügyfél** van az alkalmazás kódjában. Ha hello ügyfél nem végez egy DNS-címkeresést, (ez nem kísérli meg tooresolve hello IP-cím, mintha egy tartomány nevét (x.x.x.x)), majd a forgalmat nem hello hibrid kapcsolaton keresztül küld el.
> 
> Megadhatja például (pseudocode) **10.4.5.6** helyszíni gazdakörnyezeteként:
> 
> **a következő forgatókönyv works hello:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves too127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **a következő forgatókönyv hello nem működik:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route toohost`
> 
> 

## <a name="CreateHybridConnection"></a>A hibrid kapcsolat létrehozása
A hibrid kapcsolat hozhatók létre hello Azure Web Apps portálra **vagy** BizTalk szolgáltatások használatával. 

**Hibrid kapcsolatok Web Apps használatával toocreate**, lásd: [csatlakozás Azure Web Apps tooan helyszíni erőforrás](../app-service-web/web-sites-hybrid-connection-get-started.md). A webes alkalmazás, amely hello előnyben részesített módszer hello hibrid kapcsolat Manager (HCM) is telepíthet. 

**Hibrid kapcsolatok a BizTalk szolgáltatások toocreate**:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello bal oldali navigációs panelen, jelölje ki a **BizTalk szolgáltatások** , és válassza a BizTalk szolgáltatás. 
   
    Ha még nem rendelkezik meglévő BizTalk szolgáltatás, akkor [BizTalk szolgáltatás létrehozása](biztalk-provision-services.md).
3. Jelölje be hello **hibrid kapcsolatok** lapon:  
   ![Hibrid kapcsolatok lapra][HybridConnectionTab]
4. Válassza ki **egy hibrid kapcsolat létrehozása** vagy select hello **hozzáadása** hello tálcán gombra. Írja be a következő hello:
   
   | Tulajdonság | Leírás |
   | --- | --- |
   | Név |hello hibrid kapcsolat nevének egyedinek kell lennie, és nem lehet hello azonos nevet, amint hello BizTalk szolgáltatás. Adja meg a nevét, de konkrét, annak oka lehet. Példák erre vonatkozóan:<br/><br/>Bérlista*SQLServer*<br/>SupplyList*SharepointServer*<br/>Az ügyfelek*OracleServer* |
   | Állomásnév |Adja meg a hello teljesen minősített állomásnév, csak egy hello állomásnév, vagy hello hello helyszíni erőforrás IPv4-címét. Példák erre vonatkozóan:<br/><br/>mySQLServer<br/>*mySQLServer*. *Tartomány*. corp.*cegnev*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *cegnev*.com<br/>10.100.10.10<br/><br/>Ha hello IPv4-címet használ, vegye figyelembe, hogy az ügyfél vagy az alkalmazás kódja nem megoldhatják hello IP-címet. Lásd: hello fontos Megjegyzés: Ez a témakör hello tetején. |
   | Port |Adja meg a helyi erőforrás hello hello portszámot. Webalkalmazások használata, adja meg például port a 80-as vagy 443-as portot. Ha az SQL Server használata esetén adja meg az 1433-as port. |
5. Válassza ki a hello pipa toocomplete hello beállítása. 

#### <a name="additional"></a>További
* Több hibrid kapcsolatok hozhatók létre. Lásd: hello [BizTalk szolgáltatások: kiadások diagram](biztalk-editions-feature-chart.md) az engedélyezett kapcsolatok hello számát. 
* Minden egyes hibrid kapcsolat jön létre a kapcsolati karakterláncok két: alkalmazás kulcsok, a KÜLDÉS és a helyszíni kulcsok figyelő. Minden párt tartalmaz egy elsődleges és másodlagos kulcsot. 

## <a name="LinkWebSite"></a>Az Azure App Service webalkalmazás vagy mobilalkalmazás hivatkozás
Válassza a webes alkalmazás vagy a Mobile Apps toolink az Azure App Service tooan meglévő hibrid kapcsolat **meglévő hibrid kapcsolat használata** hello hibrid kapcsolatok panelen. Lásd: [hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Hello Hybrid Connection Manager helyi telepítése
A hibrid kapcsolat létrehozása után telepítse hello Hybrid Connection Manager hello helyszíni erőforrás. Az Azure-webalkalmazásokban, illetve a BizTalk szolgáltatás is letölthetők. BizTalk szolgáltatások lépéseket: 

1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello bal oldali navigációs panelen, jelölje ki a **BizTalk szolgáltatások** , és válassza a BizTalk szolgáltatás. 
3. Jelölje be hello **hibrid kapcsolatok** lapon:  
   ![Hibrid kapcsolatok lapra][HybridConnectionTab]
4. Hello tálcán, válassza ki a **helyszíni telepítő**:  
   ![A helyszíni beállítása][HCOnPremSetup]
5. Válassza ki **telepítése és konfigurálása** a hello toorun vagy letöltési hello Hybrid Connection Manager a helyi rendszer. 
6. Válassza ki a hello pipa toostart hello telepítést. 

<!--
You can also download hello Hybrid Connection Manager MSI file and copy hello file tooyour on-premises resource. Specific steps:

1. Copy hello on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for hello specific steps.
2. Download hello Hybrid Connection Manager MSI file. 
3. On hello on-premises resource, install hello Hybrid Connection Manager from hello MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>További
* Hybrid Connection Manager a következő operációs rendszerek hello telepíthető:
  
  * Windows Server 2008 R2 (a .NET-keretrendszer 4.5-ös + és a Windows Management Framework 4.0-s vagy újabb verzió szükséges)
  * Windows Server 2012 (a Windows Management Framework 4.0-s vagy újabb verzió szükséges)
  * Windows Server 2012 R2
* Hello Hybrid Connection Manager telepítése után hello következő történik: 
  
  * hello Azure-platformon futó hibrid kapcsolat, automatikusan konfigurált toouse hello elsődleges alkalmazás kapcsolati karakterláncot. 
  * hello helyszíni erőforrás, automatikusan konfigurált toouse hello elsődleges helyszíni kapcsolati karakterlánc.
* hello Hybrid Connection Manager engedélyezési érvényes a helyszíni kapcsolati karakterláncot kell használnia. hello Azure Web Apps vagy a Mobile Apps engedélyezési érvényes kapcsolati karakterláncot kell használnia.
* Hibrid kapcsolatok bővítse hello Hybrid Connection Manager egy másik példánya egy másik kiszolgálóra telepíti. Hello helyszíni figyelő toouse hello azonos cím beállítása hello első helyszíni figyelő. Ebben a helyzetben a hello forgalom hello aktív helyszíni figyelői között véletlenszerűen elosztott (ciklikus időszeletelés). 

## <a name="ManageHybridConnection"></a>Hibrid kapcsolatok kezelése
toomanage a hibrid kapcsolatok is:

* Azure-portálon hello alkalmazza, és tooyour BizTalk szolgáltatás. 
* Használjon [REST API-k](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-hello-hybrid-connection-strings"></a>Másolási/újragenerálása hello hibrid kapcsolati karakterláncok
1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello bal oldali navigációs panelen, jelölje ki a **BizTalk szolgáltatások** , és válassza a BizTalk szolgáltatás. 
3. Jelölje be hello **hibrid kapcsolatok** lapon:  
   ![Hibrid kapcsolatok lapra][HybridConnectionTab]
4. Válassza ki a hibrid kapcsolat hello. Hello tálcán, válassza ki a **kapcsolat kezelése**:  
   ![Beállítások kezelése][HCManageConnection]
   
    **Kapcsolat kezelése** listák hello alkalmazás és a helyszíni kapcsolati karakterláncokat. Másolja a kapcsolati karakterláncok hello, vagy újragenerálni a hozzáférési kulcsot használt hello kapcsolat-karakterláncban hello. 
   
    **Ha Regenerate**, megosztott elérési kulcsot használt kapcsolati karakterlánc megváltozott hello belül hello. A következő hello:
   
   * Hello a klasszikus Azure portálon, válassza ki **szinkronizálási kulcsok** a hello Azure-alkalmazást.
   * Futtassa újra a hello **helyszíni telepítő**. Hello újbóli futtatásakor a helyszíni telepítés, a helyi erőforrást a rendszer automatikusan hello konfigurált toouse hello frissített elsődleges kapcsolódási karakterlánc.

#### <a name="use-group-policy-toocontrol-hello-on-premises-resources-used-by-a-hybrid-connection"></a>Csoport használatával házirend toocontrol hello helyszíni egy hibrid kapcsolat által használt erőforrások
1. Töltse le a hello [hibrid Csatlakozáskezelő felügyeleti sablonok](http://www.microsoft.com/download/details.aspx?id=42963).
2. Bontsa ki a hello fájlokat.
3. Az hello hello számítógépen, amely a csoportházirend módosítja, a következő:  
   
   * Hello másolja. ADMX fájljainak toohello *%WINROOT%\PolicyDefinitions* mappa.
   * Hello másolja. ADML fájlok toohello *%WINROOT%\PolicyDefinitions\en-us* mappa.

A másolást követően a Helyicsoportházirend-szerkesztő toochange hello házirend használható.

## <a name="next"></a>Következő lépés
[Csatlakozás Azure Web Apps tooan helyszíni erőforrás](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Kapcsolódó tooon helyszíni SQL Server Azure Web Apps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Hibrid kapcsolatok – áttekintés](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Lásd még:
[A Microsoft Azure BizTalk szolgáltatások kezelésére szolgáló REST API-n](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Kiadások diagramja](biztalk-editions-feature-chart.md)  
[Klasszikus Azure portál használatával BizTalk szolgáltatás létrehozása](biztalk-provision-services.md)  
[BizTalk Services: Irányítópult, Figyelés és Méret lapok](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
