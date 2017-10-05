---
title: "Helyszíni erőforrások elérése hibrid kapcsolatok használatával az Azure App Service-ben"
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
ms.openlocfilehash: fbd22e6e285c5ddaef2a473671d4a06a97384b4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a>Helyszíni erőforrások elérése hibrid kapcsolatok használatával az Azure App Service-ben
Kapcsolódás az Azure App Service-alkalmazások összes helyszíni erőforrás a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatásokat használó. Ez a cikk bemutatja, hogyan hozzon létre egy hibrid kapcsolat App Service és a helyszíni SQL Server-adatbázis között.

> [!NOTE]
> A Web Apps része a hibrid kapcsolatok szolgáltatás csak a érhető el a [Azure Portal](https://portal.azure.com). BizTalk szolgáltatások VPN-kapcsolat létrehozásához lásd: [hibrid kapcsolatok](http://go.microsoft.com/fwlink/p/?LinkID=397274). 
> 
> Ezt a tartalmat az Azure App Service Mobile Apps is vonatkozik. 
> 
> 

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Ingyenes előfizetésre, lásd: [Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/). 
  
    Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
* A helyszíni SQL Server vagy SQL Server Express adatbázis használata hibrid kapcsolaton keresztül, TCP/IP kell engedélyezni a statikus port. Az SQL Server alapértelmezett példánnyal használata ajánlott, mivel a 1433-as port statikus használ. Információ a telepítése és konfigurálása az SQL Server Express hibrid kapcsolatok való használathoz: [csatlakozás egy helyszíni SQL Server egy hibrid kapcsolatok használata az Azure webhelyén](http://go.microsoft.com/fwlink/?LinkID=397979).
* A számítógép, amelyre a cikk későbbi részében a helyszíni Hybrid Connection Manager ügynök leírt telepíti:
  
  * Port: 5671 keresztül csatlakozni az Azure-bA képesnek kell lennie
  * Kell tudni érnie a *állomásnév*:*portszám* a helyi erőforrás. 

> [!NOTE]
> Ebben a cikkben szereplő lépések azt feltételezik, hogy a számítógépről, amely üzemelteti a helyszíni hibrid kapcsolat ügynök böngészőt használ.
> 
> 

## <a name="create-a-web-app-in-the-azure-portal"></a>Webalkalmazás létrehozása az Azure portálon
> [!NOTE]
> Ha már létrehozott egy webes alkalmazás vagy a Mobile Apps-háttéralkalmazás az Azure portálon, hogy a jelen oktatóanyag használni kívánt, ugorjon előre [egy hibrid kapcsolat és a BizTalk szolgáltatás létrehozása](#CreateHC) , és indítsa el onnan.
> 
> 

1. A bal felső sarkában található a [Azure Portal](https://portal.azure.com), kattintson a **új** > **Web + mobil** > **webalkalmazás**.
   
    ![Új webalkalmazás][NewWebsite]
2. Az a **webalkalmazás** panelen adjon meg egy URL-címet, és kattintson **létrehozása**. 
   
    ![Webhely neve][WebsiteCreationBlade]
3. Néhány másodpercen belül a web app jön létre, és a webalkalmazás panelen jelenik meg. A panel egy függőleges görgethető irányítópultot, amely lehetővé teszi, hogy a hely kezeléséhez.
   
    ![Futtató webhely][WebSiteRunningBlade]
4. Annak ellenőrzéséhez, hogy a hely élő, kattintson a **Tallózás** ikonra kattintva jelenítse meg az alapértelmezett lapon.
   
    ![Kattintson a Tallózás gombra, hogy a webalkalmazás][Browse]
   
    ![Alapértelmezett weblap-alkalmazás][DefaultWebSitePage]

A következő létrehozhat egy hibrid kapcsolat és a BizTalk szolgáltatás a webalkalmazás.

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a>A hibrid kapcsolat és a BizTalk szolgáltatás létrehozása
1. A webalkalmazás panelen kattintson a **összes beállítás** > **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.
   
    ![Hibrid kapcsolatok][CreateHCHCIcon]
2. A hibrid kapcsolatok paneljén kattintson **Hozzáadás**.
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. A **egy hibrid kapcsolat hozzáadása a** panel nyílik meg.  Az első hibrid kapcsolat, mert a **új hibrid kapcsolat** lehetőség van megadva, és a **hibrid kapcsolat létrehozása** panel megnyitása meg.
   
    ![A hibrid kapcsolat létrehozása][TwinCreateHCBlades]
   
    Az a **létrehozás hibrid kapcsolat panel**:
   
   * A **neve**, adja meg a kapcsolat nevét.
   * A **állomásnév**, adja meg a helyi számítógépen, amelyen az erőforrás nevét.
   * A **Port**, adja meg a portszámot, hogy a helyi erőforrás (SQL Server alapértelmezett példány 1433) használja-e.
   * Kattintson a **Biz előadás szolgáltatás**
4. A **BizTalk szolgáltatás létrehozása** panel nyílik meg. Adjon meg egy nevet a BizTalk szolgáltatás, és kattintson **OK**.
   
    ![BizTalk szolgáltatás létrehozása][CreateHCCreateBTS]
   
    A **BizTalk szolgáltatás létrehozása** panel bezárása után, és a rendszer visszairányítja a **hibrid kapcsolat létrehozása** panelen.
5. A hibrid kapcsolat létrehozása panelen kattintson a **OK**. 
   
    ![Kattintson az OK gombra][CreateBTScomplete]
6. A folyamat befejezése után, az értesítési területen a portálon figyelmeztet, hogy a kapcsolat sikeresen létrejött.
   
    <!--- TODO
   
    Everything fails at this step. I can't create a BizTalk service in the dogfood portal. I switch to the classic portal
    (full portal) and created the BizTalk service but it doesn't seem to let you connnect them - When you finish the
    Create hybrid conn step, you get the following error
    Failed to create hybrid connection RelecIoudHC. The 
    resource type could not be found in the namespace 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
   
    The error indicates it couldn't find the type, not the instance.
    ![Success notification][CreateHCSuccessNotification]
    -->
7. A webalkalmazás panelen a **hibrid kapcsolatok** ikon már helyesen jelenik meg, hogy 1 hibrid kapcsolat létrejött.
   
    ![Egy hibrid kapcsolat létrehozása][CreateHCOneConnectionCreated]

Fontos része a hibrid kapcsolat felhőalapú infrastruktúra ezen a ponton befejeződött. Ezután létrehozhat egy helyszíni megfelelő adat.

<a name="InstallHCM"></a>

## <a name="install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>A kapcsolat a helyszíni hibrid Csatlakozáskezelő telepítése
1. A webes alkalmazás paneljén kattintson **összes beállítás** > **hálózati** > **hibrid kapcsolati végpontok konfigurálása**. 
   
    ![Hibrid kapcsolatok ikonja][HCIcon]
2. Az a **hibrid kapcsolatok** panelen a **állapot** a legutóbb hozzáadott végpont jeleníti meg az oszlop **nincs csatlakoztatva**. Kattintson a kapcsolat konfigurálását.
   
    ![Nincs csatlakoztatva][NotConnected]
   
    A hibrid kapcsolat panel nyílik meg.
   
    ![NotConnectedBlade][NotConnectedBlade]
3. Kattintson a panel **figyelő telepítő**.
   
    ![Kattintson a figyelő beállítás][ClickListenerSetup]
4. A **hibrid kapcsolat tulajdonságai** panel nyílik meg. A **helyszíni Hybrid Connection Manager**, válassza a **telepítéséhez kattintson ide**.
   
    ![Kattintson ide][ClickToInstallHCM]
5. Az alkalmazás futtatása biztonsági figyelmeztetés párbeszédpanel, válassza a **futtatása** folytatja.
   
    ![Válassza a Futtatás a folytatáshoz][ApplicationRunWarning]
6. Az a **felhasználói fiókok felügyelete** párbeszédpanelen válasszon **Igen**.
   
   ![Igen választása][UAC]
7. A Hybrid Connection Manager letöltötte és telepítette a. 
   
    ![Telepítése][HCMInstalling]
8. A telepítés befejeztével kattintson **Bezárás**.
   
    ![Kattintson a Bezárás gombra][HCMInstallComplete]
   
    Az a **hibrid kapcsolatok** panelen a **állapot** most az oszlop látható **csatlakoztatva**. 
   
    ![Csatlakoztatott állapot][HCStatusConnected]

Most, hogy a hibrid kapcsolat infrastruktúra befejeződött, létrehozhat egy hibrid alkalmazás, amely használja ezt a szolgáltatást. 

> [!NOTE]
> Az alábbi szakaszok bemutatják a hibrid kapcsolat használata egy Mobile Apps .NET háttérrendszer-projekt esetében.
> 
> 

## <a name="configure-the-mobile-app-net-backend-project-to-connect-to-the-sql-server-database"></a>Az SQL Server-adatbázishoz való kapcsolódáshoz a Mobile App .NET háttérrendszer-projekt konfigurálása
Az App Service-ben a Mobile Apps .NET backend project csak ASP.NET webalkalmazás SDK-val egy további Mobile Apps telepítve és inicializálva. A webalkalmazás a Mobile Apps-háttéralkalmazás használja, le kell [töltse le és inicializálni a Mobile Apps .NET-háttérrendszer SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).  

Mobile Apps meg kell is adja meg a helyi adatbázis kapcsolati karakterláncot, és a háttér használata a kapcsolat módosításához. 

1. A Visual Studio Solution Explorerben nyissa meg a Web.config fájlt a Mobile App .NET-háttérrendszer, keresse meg a **connectionStrings** területen írja be egy új SqlClient bejegyzés a következő, amely a helyszíni SQL Server mutat adatbázis:
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    Ne felejtse el lecserélni `<**secure_password**>` ezt a jelszót, amelyet a karakterlánc a *HybridConnectionLogin*.
2. Kattintson a **mentése** a Visual Studio menteni a Web.config fájlt.
   
   > [!NOTE]
   > A kapcsolat a beállítással a helyi számítógéphez. Ha fut az Azure-ban, a beállítás akkor felülbírálva által a portálon megadott kapcsolati beállításokat.
   > 
   > 
3. Bontsa ki a **modellek** mappa, és nyissa meg az adatok modell fájlt, amely fejeződik be a *Context.cs*.
4. Módosítsa a **DbContext** példánykonstruktor felelt meg az érték `OnPremisesDBConnection` bázis **DbContext** konstruktor a következő kódrészletet hasonló:
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    A szolgáltatás most fogja használni az SQL Server-adatbázist az új kapcsolatot.

## <a name="update-the-mobile-app-backend-to-use-the-on-premises-connection-string"></a>A Mobile Apps-háttéralkalmazás használatára a helyszíni kapcsolati karakterlánc frissítése
Ezt követően kell az új kapcsolati karakterlánc az Alkalmazásbeállítás hozzáadása, hogy az Azure-ból is használható.  

1. Vissza a [Azure-portálon](https://portal.azure.com) a web Apps-háttéralkalmazás kódjához a Mobile Apps, kattintson **összes beállítás**, majd **Alkalmazásbeállítások**.
2. Az a **webalkalmazás-beállításainak** paneljén görgessen le a **kapcsolati karakterláncok** és adjon hozzá egy új **SQL Server** nevű kapcsolati karakterlánc `OnPremisesDBConnection` például értékű`Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.
   
    Cserélje le `<**secure_password**>` a helyi adatbázis biztonságos jelszavával.
   
    ![A helyi adatbázis kapcsolati karakterlánca](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. Nyomja le az **mentése** menteni a hibrid kapcsolat és a kapcsolati karakterlánc az imént létrehozott.

Ezen a ponton a projekt ismételt közzététele, és az új kapcsolat tesztelése a meglévő Mobile Apps-ügyfelekkel. Adatok olvasni és a helyi adatbázisba, a hibrid kapcsolat használatával.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Következő lépések
* Tudnivalók a hibrid kapcsolatot használó ASP.NET webalkalmazás létrehozásáról lásd: [csatlakozás egy helyszíni SQL Server egy hibrid kapcsolatok használata az Azure webhelyén](http://go.microsoft.com/fwlink/?LinkID=397979). 

### <a name="additional-resources"></a>További források
[A hibrid kapcsolatok áttekintése](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh a jelölés vezet be a hibrid kapcsolatok (videó a Channel 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Hibrid kapcsolatok webhely](https://azure.microsoft.com/services/biztalk-services/)

[BizTalk szolgáltatások: Irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Zökkenőmentes alkalmazások hordozhatóságát (videó a Channel 9) egy valós hibrid felhő felépítése](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Csatlakozzon egy helyi SQL Server az Azure Mobile Services használatával hibrid kapcsolatok (videó a Channel 9)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

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
