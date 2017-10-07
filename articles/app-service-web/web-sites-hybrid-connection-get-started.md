---
title: "hibrid kapcsolatok használata az Azure App Service aaaAccess a helyszíni erőforrások"
description: "Hozzon létre egy webalkalmazást az Azure App Service-ben, és egy helyszíni erőforrás egy statikus TCP-portot használó közötti kapcsolat"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: a46ed26b-df8e-4fc3-8e05-2d002a6ee508
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: cephalin
ms.openlocfilehash: de7c57b94f4bd6250a93757817178e8455daae4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a>Helyszíni erőforrások elérése hibrid kapcsolatok használatával az Azure App Service-ben
Csatlakoztathatja egy Azure App Service alkalmazás tooany helyszíni-erőforrást, amely a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatások használja. Ez a cikk bemutatja, hogyan toocreate egy hibrid kapcsolat App Service és a helyszíni SQL Server-adatbázis között.

> [!NOTE]
> hello webalkalmazások hello hibrid kapcsolatok szolgáltatás része csak a hello [Azure Portal](https://portal.azure.com). BizTalk szolgáltatások, a kapcsolat toocreate lásd [hibrid kapcsolatok](http://go.microsoft.com/fwlink/p/?LinkID=397274). 
> 
> Ez a tartalom is érvényes tooMobile alkalmazások az Azure App Service-ben. 
> 
> 

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Ingyenes előfizetésre, lásd: [Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/). 
  
    Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
* toouse egy helyi SQL Server vagy SQL Server Express adatbázist egy hibrid kapcsolat, az TCP/IP toobe statikus port engedélyezve van szüksége. Az SQL Server alapértelmezett példánnyal használata ajánlott, mivel a 1433-as port statikus használ. Információ a telepítése és konfigurálása az SQL Server Express hibrid kapcsolatok való használathoz: [Connect tooan a helyszíni SQL Server egy hibrid kapcsolatok használata az Azure webhelyén](http://go.microsoft.com/fwlink/?LinkID=397979).
* hello számítógép, amelyre a cikk későbbi részében leírt hello helyszíni Hybrid Connection Manager ügynök telepíti:
  
  * Képes tooconnect tooAzure kell port: 5671
  * Képes tooreach hello kell *állomásnév*:*portszám* a helyi erőforrás. 

> [!NOTE]
> Ez a cikk hello lépések azt feltételezik, hogy hello helyszíni hibrid kapcsolat ügynököt futtató számítógépről hello hello böngészőt használ.
> 
> 

## <a name="create-a-web-app-in-hello-azure-portal"></a>A webalkalmazás létrehozása az Azure portál hello
> [!NOTE]
> Ha már létrehozott egy webes alkalmazás vagy a Mobile Apps-háttéralkalmazás hello Azure portálon megjeleníteni kívánt toouse ehhez az oktatóanyaghoz, akkor kihagyhatja azokat, amelyek túl[egy hibrid kapcsolat és a BizTalk szolgáltatás létrehozása](#CreateHC) , és indítsa el onnan.
> 
> 

1. A hello bal felső sarkában hello [Azure Portal](https://portal.azure.com), kattintson a **új** > **Web + mobil** > **webalkalmazás**.
   
    ![Új webalkalmazás][NewWebsite]
2. A hello **webalkalmazás** panelen adjon meg egy URL-címet, és kattintson **létrehozása**. 
   
    ![Webhely neve][WebsiteCreationBlade]
3. Néhány másodpercen belül hello webalkalmazás jön létre, és a webalkalmazás panelen jelenik meg. hello panel egy függőleges görgethető irányítópultot, amely lehetővé teszi, hogy a hely kezeléséhez.
   
    ![Futtató webhely][WebSiteRunningBlade]
4. élő tooverify hello hely, rákattinthat hello **Tallózás** ikon toodisplay hello alapértelmezett oldalt.
   
    ![Kattintson a Tallózás toosee a webalkalmazás][Browse]
   
    ![Alapértelmezett weblap-alkalmazás][DefaultWebSitePage]

Ezt követően egy hibrid kapcsolat és a BizTalk szolgáltatás hello webalkalmazás hoz létre.

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a>A hibrid kapcsolat és a BizTalk szolgáltatás létrehozása
1. A webalkalmazás panelen kattintson a **összes beállítás** > **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.
   
    ![Hibrid kapcsolatok][CreateHCHCIcon]
2. Hello hibrid kapcsolatok paneljén kattintson **Hozzáadás**.
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. Hello **egy hibrid kapcsolat hozzáadása a** panel nyílik meg.  Mivel ez az első a hibrid kapcsolat, hello **új hibrid kapcsolat** beállítás van megadva, és hello **hibrid kapcsolat létrehozása** panel nyílik meg.
   
    ![A hibrid kapcsolat létrehozása][TwinCreateHCBlades]
   
    A hello **létrehozás hibrid kapcsolat panel**:
   
   * A **neve**, hello kapcsolat nevét adja meg.
   * A **állomásnév**, adja meg, amelyen az erőforrás hello a helyi számítógép hello nevét.
   * A **Port**, adja meg, hogy a helyi erőforrás használja (SQL Server alapértelmezett példány 1433) hello portszámot.
   * Kattintson a **Biz előadás szolgáltatás**
4. Hello **BizTalk szolgáltatás létrehozása** panel nyílik meg. Adjon meg egy nevet hello BizTalk szolgáltatás, és kattintson **OK**.
   
    ![BizTalk szolgáltatás létrehozása][CreateHCCreateBTS]
   
    Hello **BizTalk szolgáltatás létrehozása** panel bezárása után, és ismét toohello **hibrid kapcsolat létrehozása** panelen.
5. Hello létrehozása a hibrid kapcsolat paneljén kattintson **OK**. 
   
    ![Kattintson az OK gombra][CreateBTScomplete]
6. Hello folyamat befejezése után a hello értesítési terület, a portál hello arról tájékoztatja, hogy hello kapcsolat sikeresen létrejött.
   
    <!--- TODO
   
    Everything fails at this step. I can't create a BizTalk service in hello dogfood portal. I switch toohello classic portal
    (full portal) and created hello BizTalk service but it doesn't seem toolet you connnect them - When you finish the
    Create hybrid conn step, you get hello following error
    Failed toocreate hybrid connection RelecIoudHC. hello 
    resource type could not be found in hello namespace 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
   
    hello error indicates it couldn't find hello type, not hello instance.
    ![Success notification][CreateHCSuccessNotification]
    -->
7. Hello webes alkalmazás paneljén hello **hibrid kapcsolatok** ikon már helyesen jelenik meg, hogy 1 hibrid kapcsolat létrejött.
   
    ![Egy hibrid kapcsolat létrehozása][CreateHCOneConnectionCreated]

Ezen a ponton befejeződött hello felhőalapú hibrid kapcsolat infrastruktúra fontos része. Ezután létrehozhat egy helyszíni megfelelő adat.

<a name="InstallHCM"></a>

## <a name="install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a>Hello helyszíni Hybrid Connection Manager toocomplete hello kapcsolat telepítése
1. Hello webes alkalmazás paneljén kattintson **összes beállítás** > **hálózati** > **hibrid kapcsolati végpontok konfigurálása**. 
   
    ![Hibrid kapcsolatok ikonja][HCIcon]
2. A hello **hibrid kapcsolatok** panelen, hello **állapot** hello oszlopában nemrég adta hozzá a végpont látható **nincs csatlakoztatva**. Kattintson a hello kapcsolat tooconfigure azt.
   
    ![Nincs csatlakoztatva][NotConnected]
   
    hello hibrid kapcsolat panel nyílik meg.
   
    ![NotConnectedBlade][NotConnectedBlade]
3. Hello paneljén kattintson **figyelő telepítő**.
   
    ![Kattintson a figyelő beállítás][ClickListenerSetup]
4. Hello **hibrid kapcsolat tulajdonságai** panel nyílik meg. A **helyszíni Hybrid Connection Manager**, válassza a **ide tooinstall**.
   
    ![Kattintson ide tooinstall][ClickToInstallHCM]
5. Hello alkalmazás futtatása biztonsági figyelmeztetés párbeszédpanel, válassza a **futtatása** toocontinue.
   
    ![Válassza a Futtatás toocontinue][ApplicationRunWarning]
6. A hello **felhasználói fiókok felügyelete** párbeszédpanelen válasszon **Igen**.
   
   ![Igen választása][UAC]
7. hello Hybrid Connection Manager letöltötte és telepítette a. 
   
    ![Telepítése][HCMInstalling]
8. Hello telepítés befejeztével kattintson **Bezárás**.
   
    ![Kattintson a Bezárás gombra][HCMInstallComplete]
   
    A hello **hibrid kapcsolatok** panelen, hello **állapot** most az oszlop látható **csatlakoztatva**. 
   
    ![Csatlakoztatott állapot][HCStatusConnected]

Most, hogy hello hibrid kapcsolat infrastruktúra befejeződött, létrehozhat egy hibrid alkalmazás, amely használja ezt a szolgáltatást. 

> [!NOTE]
> hello következő szakaszok bemutatják, hogyan toouse egy hibrid kapcsolat egy Mobile Apps .NET háttérrendszer-projekt esetében.
> 
> 

## <a name="configure-hello-mobile-app-net-backend-project-tooconnect-toohello-sql-server-database"></a>Hello Mobile App .NET backend project tooconnect toohello SQL Server-adatbázis konfigurálása
Az App Service-ben a Mobile Apps .NET backend project csak ASP.NET webalkalmazás SDK-val egy további Mobile Apps telepítve és inicializálva. toouse kell a webalkalmazást, mint a Mobile Apps-háttéralkalmazás, [töltse le és hello Mobile Apps .NET-háttérrendszer SDK inicializálása](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).  

A Mobile Apps is hello a helyi adatbázis toodefine egy kapcsolati karakterláncot kell, majd módosítsa hello háttér toouse a kapcsolatot. 

1. A Solution Explorerben a Visual Studio, nyissa meg hello Web.config fájlja a Mobile App .NET-háttérrendszer, keresse meg a hello **connectionStrings** területen hasonló hello, mely pontok toohello a helyszíni SQL új SqlClient bejegyzés hozzáadása Server-adatbázist:
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    Ne feledje tooreplace `<**secure_password**>` a karakterláncban létrehozott hello jelszóval *HybridConnectionLogin*.
2. Kattintson a **mentése** Visual Studio toosave hello Web.config fájlban.
   
   > [!NOTE]
   > A beállítás akkor használatos, ha a hello helyi számítógépen futó. Ha fut az Azure-ban, a beállítás akkor felülbírálva hello portálon definiált hello-kapcsolat beállítása.
   > 
   > 
3. Bontsa ki a hello **modellek** mappa és fejeződik be a nyitott hello adatok modell fájljából *Context.cs*.
4. Hello módosítása **DbContext** példány konstruktor toopass hello érték `OnPremisesDBConnection` alap toohello **DbContext** konstruktor, hasonló toohello a következő kódrészletet:
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    hello szolgáltatás most hello új kapcsolat toohello SQL Server-adatbázist használja.

## <a name="update-hello-mobile-app-backend-toouse-hello-on-premises-connection-string"></a>Hello mobilalkalmazás háttér toouse hello helyszíni kapcsolati karakterlánc frissítése
A következő lépésben tooadd Alkalmazásbeállítás a új kapcsolati karakterlánccal, hogy az Azure-ból is használható.  

1. Vissza a hello [Azure-portálon](https://portal.azure.com) hello web Apps-háttéralkalmazás kódjához a Mobile Apps, kattintson **összes beállítás**, majd **Alkalmazásbeállítások**.
2. A hello **webalkalmazás-beállításainak** panelen görgessen lefelé, túl**kapcsolati karakterláncok** és adjon hozzá egy új **SQL Server** nevű kapcsolati karakterlánc `OnPremisesDBConnection` például értékkel`Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.
   
    Cserélje le `<**secure_password**>` hello biztonságos jelszó a helyi adatbázis.
   
    ![A helyi adatbázis kapcsolati karakterlánca](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. Nyomja le az **mentése** toosave hello hibrid kapcsolat és a kapcsolati karakterlánc újonnan létrehozott.

Ezen a ponton hello kiszolgálóprojektet ismételt közzététele, és hello új kapcsolat tesztelése a meglévő Mobile Apps-ügyfelekkel. Adatok olvasni és írt toohello, helyszíni adatbázisokhoz hello hibrid kapcsolat használatával.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Következő lépések
* Tudnivalók a hibrid kapcsolatot használó ASP.NET webalkalmazás létrehozásáról lásd: [Connect tooan a helyszíni SQL Server egy hibrid kapcsolatok használata az Azure webhelyén](http://go.microsoft.com/fwlink/?LinkID=397979). 

### <a name="additional-resources"></a>További források
[A hibrid kapcsolatok áttekintése](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh a jelölés vezet be a hibrid kapcsolatok (videó a Channel 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Hibrid kapcsolatok webhely](https://azure.microsoft.com/services/biztalk-services/)

[BizTalk szolgáltatások: Irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Zökkenőmentes alkalmazások hordozhatóságát (videó a Channel 9) egy valós hibrid felhő felépítése](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Csatlakozás tooan a helyszíni SQL Server az Azure Mobile Services használatával hibrid kapcsolatok (videó a Channel 9)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[New]:./media/web-sites-hybrid-connection-get-started/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-get-started/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-get-started/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-get-started/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-get-started/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-get-started/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-get-started/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-get-started/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-get-started/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-get-started/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-get-started/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-get-started/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-get-started/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-get-started/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-get-started/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-get-started/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-get-started/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-get-started/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-get-started/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-get-started/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-get-started/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-get-started/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-get-started/D10HCStatusConnected.png
