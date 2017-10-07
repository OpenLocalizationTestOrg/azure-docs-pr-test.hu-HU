---
title: "aaaHow tooMigrate és a közzététel a webes alkalmazás tooan Azure felhőalapú szolgáltatás, a Visual Studio |} Microsoft Docs"
description: "Megtudhatja, hogyan toomigrate és a webes alkalmazás tooan Azure-felhőszolgáltatásban közzététele a Visual Studio használatával."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: a2832c37d2ebdbc1e69a307d16b65b1c87e9070c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-migrate-and-publish-a-web-application-tooan-azure-cloud-service-from-visual-studio"></a>Hogyan: át, és tegye közzé a webes alkalmazás tooan Azure Cloud Service, a Visual Studio eszközből
tootake előnyeit hello üzemeltetési szolgáltatásokat és méretezhetőséget biztosít a Azure, előfordulhat, hogy szeretné, hogy toomigrate és a webes alkalmazás tooan Azure felhőalapú szolgáltatást teszik közzé. Minimális módosításait tooyour meglévő alkalmazással futtathatja egy webalkalmazást az Azure-ban.

> [!NOTE]
> Ez a témakör tárgya toocloud szolgáltatások, nem tooweb helyek telepítése. Tooweb helyek telepítésével kapcsolatos információkért lásd: [webalkalmazás üzembe helyezése az Azure App Service](app-service-web/web-sites-deploy.md).
>
>

Visual C# és Visual Basic támogatott adott sablonok listáját című hello **támogatott Projektsablonjai** a témakör későbbi részében.

Először engedélyeznie kell a webes alkalmazás az Azure-hoz a Visual Studio eszközből. a következő ábra hello a már meglévő webalkalmazás mutatja hello fontos lépések toopublish központi telepítés egy Azure-projekt toouse hozzáadásával. Ez a folyamat hozzáadja egy Azure-projekt szükséges hello webes szerepkör tooyour megoldás. Alapján hello webes projekt, amely rendelkezik, hello projekt Tulajdonságok szerelvények is frissülnek Ha hello szolgáltatás csomag telepítési további szerelvények igényel.

![A webes alkalmazás tooMicrosoft Azure közzététele](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

> [!NOTE]
> Hello **átalakítása**, **tooAzure Felhőszolgáltatás-projekt átalakítása** parancs csak a hello webes projektet a megoldásban jelenik meg. Hello parancs például egy Silverlight-projektet a megoldásban a nem érhető el.
> Amikor service-csomag létrehozása vagy az alkalmazás tooAzure közzététele, a figyelmeztetések vagy hibák fordulhatnak elő. Ezek a figyelmeztetések és hibák segítségével kapcsolatos problémák megoldása tooAzure központi telepítése előtt. Például akkor fordulhat elő egy figyelmeztetést jelenít meg hiányzott egy szerelvény. További információ a hogyan tootreat, hibákat, figyelmeztetéseket: [egy Azure Felhőszolgáltatás-projekt konfigurálása a Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Ha építenie az alkalmazást, futtassa helyileg a hello compute emulator használatával, vagy tegye közzé tooAzure, megjelenhet a következő hiba történt a hello hello **Hibalista** ablak: **hello megadott elérési út, a fájl nevét, vagy mindkettő túl hosszú** . Ez a hiba akkor fordul elő, mert hello hosszát hello teljesen minősített Azure-projekt neve túl hosszú. hello projektnevet hello teljes elérési útja, beleértve hello hossza nem lehet több mint 146 karaktereket. Például ez az hello teljes projekt név, beleértve a fájl elérési útját, amely egy Silverlight-alkalmazást hoz létre Azure-projekt: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Lehetséges, hogy toomove a megoldás tooa másik címtárhoz, amely rövidebb elérési utat tooreduce hello hossza hello teljesen minősített projekt nevét.
>
>

toomigrate és a webes alkalmazás tooAzure a Visual Studio eszközből közzétételéhez kövesse az alábbi lépéseket.

## <a name="enable-a-web-application-for-deployment-tooazure"></a>A webalkalmazás központi telepítési tooAzure engedélyezése
### <a name="tooenable-a-web-application-for-deployment-tooazure"></a>a webalkalmazás központi telepítési tooAzure tooenable
1. a webalkalmazás központi telepítési tooAzure, nyissa meg hello helyi menüje a webes tooenable projektre a megoldásban, és válassza ki az Azure-telepítési projekt hozzáadása.

    a következő műveletek hello fordulhat elő:

   * Azure-projekt nevű `<name of hello web project>.Azure` jelenik meg az alkalmazás toohello megoldás.
   * A webes szerepkör hello webes projekt toothis Azure-projekt hozzáadásakor.
   * Hello **másolása helyi** tulajdonsága tootrue az MVC-2, MVC 3, MVC 4 és Silverlight üzleti alkalmazások a szükséges szerelvényeket. Ez a gyorsjavítás a szerelvények toohello szolgáltatáscsomagot, amely a központi telepítéshez használt.

   > [!IMPORTANT]
   > Ha más szerelvényeknek vagy a webalkalmazás szükséges fájlokat, a fájlok hello tulajdonságok manuálisan meg kell adni. Hogyan tooset ezeket a tulajdonságokat meg információt szakasz hello **hello szolgáltatáscsomag fájlok közé tartoznak** című cikkben.
   >
   > [!NOTE]
   > Ha a megoldás hello Azure-projekt már létezik egy meghatározott webes projekt webes szerepkör **átalakítása**, **tooAzure Felhőszolgáltatás-projekt átalakítása** nem jelenik meg a webes projekt helyi menüjének hello .
   >
   >

   Ha több webes projektet a webalkalmazásban és minden webes projekt toocreate webes szerepkörök használni szeretne, lépésekkel hello ebben az eljárásban az minden webes projektet. Ez létrehoz minden webes szerepkör külön Azure projektek. Minden webes projekt külön tehetők közzé. Azt is megteheti kézzel is hozzáadhatja egy másik webes szerepkör tooan meglévő Azure projektet a webalkalmazásban. toodo a, nyissa meg hello helyi menüje hello **szerepkörök** Azure célverzióját, válasszon **Hozzáadás**, majd **webes szerepkör projekt megoldásban**, hello projekt tooadd egy kiválasztása szerepkör, és válassza a hello **OK** gombra.

## <a name="use-an-azure-sql-database-for-your-application"></a>Az alkalmazás az Azure SQL adatbázis használata
Ha egy kapcsolati karakterláncot a webalkalmazás a hello helyszíni SQL Server adatbázist használó, módosítania kell a kapcsolati karakterlánc toouse SQL-adatbázis példányának Azure futtató helyette.

> [!IMPORTANT]
> Az előfizetés engedélyeznie kell azt toouse SQL-adatbázis. Ha az előfizetés elérje hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), meghatározhatja, hogy mely szolgáltatásokat nyújtja az előfizetéshez. hello következő utasítások vonatkoznak, amely a toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885). Hello használata [Azure-portálon](http://portal.microsoft.com), hagyja ki a következő eljárással toohello.
>
>

### <a name="toouse-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>egy SQL Database-példányt a kapcsolati karakterláncot kell megadnia a webes szerepkörben toouse
1. egy SQL-adatbázis példánya hello toocreate [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), hajtsa végre a következő cikket hello hello lépéseket: [hozzon létre egy SQL-adatbáziskiszolgáló](http://go.microsoft.com/fwlink/?LinkId=225109).

   > [!NOTE]
   > Hello tűzfalszabályokat az SQL-adatbázis példányának beállításakor ki kell választania hello **más Azure-szolgáltatások tooaccess engedélyezése ehhez a kiszolgálóhoz** jelölőnégyzetet.
   >
   >
2. egy példányát a kapcsolati karakterláncot az SQL-adatbázis toouse toocreate hello a következő szakaszban található, a következő cikket hello hello lépéseket kövesse: [SQL-adatbázis létrehozása](http://go.microsoft.com/fwlink/?LinkId=225110).
3. toocopy hello ADO.NET kapcsolati karakterlánc toouse a kapcsolati karakterláncot kell megadnia, hajtsa végre a következő lépéseket a hello hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).  

   1. Válassza ki a hello **adatbázis** gombra, és ezután nyissa meg hello csomópont hello előfizetés, amellyel toocreate az SQL-adatbázis-példány.
   2. toodisplay hello elérhető példányok SQL-adatbázis, válassza ki a hello **SQL-adatbázisok** csomópont.
   3. toodisplay hello adatbázis tulajdonságainak megadása hello, válassza ki a hello adatbázist. Hello **tulajdonságok** nézet jelenik meg.

      > [!NOTE]
      > Ha hello **tulajdonságok** nézet nem jelenik meg, előfordulhat, hogy azt a hello elválasztó tooopen.
      >
      >
   4. toodisplay hello kapcsolati karakterláncok, válassza ki a következő tooView a hello három ponttal (…) gombra.

      Hello **kapcsolati karakterláncok** párbeszédpanel jelenik meg.
   5. toocopy hello ADO.NET kapcsolati karakterláncot, jelölje ki a hello szöveg, és válassza hello Ctrl + C kulcsok.
   6. tooclose hello párbeszédpanelen válassza ki a hello **Bezárás** gombra.
4. tooreplace hello kapcsolati karakterlánc hello web.config fájl toouse SQL-adatbázis ezen példányának, hello web.config fájl megnyitásához, jelölje ki a meglévő kapcsolati karakterlánc bejegyzés hello, és válassza a hello Ctrl + V kulcsok. hello ADO.NET kapcsolati karakterlánc SQL-adatbázis hello példányának a felváltja hello meglévő kapcsolati karakterláncot.
5. Is fel kell vennie hello paraméter `MultipleActiveResultSets=True` toohello kapcsolati karakterláncot. hello kapcsolati karakterlánc formátuma a következő hello kell rendelkezniük:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```
6. (Választható) Egy másik megoldásként toochanging hello kapcsolati karakterlánc közvetlenül hello web.config fájl tooadd oszthatók hello web.config átalakítása fájlok, attól függően, hogy hello build konfigurációját, hogy használjon toocreate a service-csomag szakasz:. Nyissa meg a hello Web.Debug.Config fájl vagy hello Web.Release.Config fájlt. A szakasz ebben a fájlban a következő hello hozzáadása:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```
7. Ön által módosított hello fájl mentéséhez, és közzé az alkalmazást.

### <a name="toouse-an-instance-of-sql-database-by-using-hello-azure-classic-portal"></a>toouse SQL-adatbázis példányának hello klasszikus Azure portál használatával
1. A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a hello SQL-adatbázisok csomópont.

   * Ha SQL-adatbázis, amelyet az toouse hello példánya jelenik meg, válassza ki azt a tooopen azt.
   * Ha esetleges példányai még nem hozott létre, válassza a hello megfelelő hivatkozásra, és majd hozzon létre egy példányt.
2. Miután megnyitásához, vagy hozzon létre egy adatbázis-példányt, válassza ki azt a hello **kapcsolati karakterláncok** hivatkozásra.
3. Alján hello hello hello hivatkozás tooconfigure tűzfal beállításokat, és fogadja el a hello alapértelmezett értékeket, vagy hello értékeket kell konfigurálni.
4. Másolja hello ADO.NET kapcsolati karakterláncot, illessze be a web.config fájl keresztül hello a helyi adatbázis hello régi kapcsolati karakterláncot, és lehet, hogy tooadd `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-tooazure"></a>A webes alkalmazás tooAzure közzététele
### <a name="toopublish-a-web-application-tooazure"></a>a webes alkalmazás tooAzure toopublish
1. tootest hello alkalmazást hello helyi fejlesztési környezetben hello Azure compute emulator használatával, nyissa meg hello helyi menüje hello Azure hello webes szerepkör projekt és válassza a **beállítás kezdőprojektként**. Válassza a **Debug**, **Start Debugging** (billentyűzet: **F5**).

    Hello **Start hello Azure-hibakeresés környezethez** párbeszédpanel, és hello alkalmazás indítása hello böngészőben. Hogyan toostart minden webalkalmazást a hello típusa kapcsolatos részleteket compute emulator, ebben a szakaszban hello táblázatban találja.
2. hello szolgáltatások esetében az alkalmazás toopublish tooAzure tooset, a Microsoft-fiókkal és az Azure-előfizetéssel kell rendelkeznie. A következő témakör tooset a szolgáltatások hello használata hello szükséges lépések: [toopublish előkészítéséhez, illetve az Azure alkalmazás a Visual Studio telepítése](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
3. toopublish hello webes alkalmazás tooAzure hello hello webes projekt helyi menüjének megnyitásához, és válassza **tooAzure közzététele**.

    Hello **Azure-alkalmazás közzététele** párbeszédpanel, és a Visual Studio megkezdi hello telepítési folyamat. Hogyan toopublish hello alkalmazás kapcsolatos további információkért lásd: hello szakasz **közzététele az Azure-alkalmazás, a Visual Studio** a [közzététele a felhőalapú szolgáltatás hello Azure eszközökkel](vs-azure-tools-publishing-a-cloud-service.md).

   > [!NOTE]
   > Hello webalkalmazás az Azure-projekt hello is közzéteheti. toodo, hello hello Azure-projekt helyi menüjének megnyitásához, és válassza a **közzététel**.
   >
   >
4. toosee hello hello központi telepítés végrehajtási állapotát, a hello megtekintheti **Azure tevékenységnapló** ablak. Ez a napló automatikusan hello központi telepítési folyamat indításakor jelenik meg. Hello sorelemet hello műveletnaplóban bővítheti tooshow részletes adatait, ahogy az ábra a következő hello:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)
5. (Választható) toocancel hello telepítési folyamatot, nyissa meg a hello helyi menüje hello sorelemet hello műveletnaplóban, és válassza **megszakítás és Eltávolítás**. Ezzel a hello telepítési folyamat leáll, és törli a hello környezet az Azure-ból.

   > [!NOTE]
   > azt követően a központi telepítési környezet tooremove le lett telepítve, hello kell használnia [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
   >
   >
6. (Választható) Miután elindította a szerepkörpéldányok, a Visual Studio automatikusan megjeleníti hello környezet a hello **Azure számítási** csomópontja **Cloud Explorer** vagy **Server Explorer**. Itt megtekintheti a hello hello egyéni szerepkörpéldányok állapotát.

    hello alábbi ábrán látható hello szerepkörpéldányt **Server Explorer** során továbbra is hello inicializálás állapota:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)
7. tooaccess telepítést követően az alkalmazás kiválasztása hello nyíl következő tooyour telepítési amikor állapotot **befejezve** hello megjelenik **Azure tevékenységnapló**. A webes alkalmazás URL-címe hello megjelenik az Azure-ban. Tekintse meg a következő táblázat hello vonatkozó további információért hogyan toostart egy adott írja be az Azure-webalkalmazás hello.

    a következő táblázat hello hogyan toostart specifikus webalkalmazások Azure vagy toorun, vagy helyileg a hello Azure Compute Emulator használatával webes alkalmazások hibakeresése hello részleteit sorolja fel:

   | Webes alkalmazás típusa | Compute Emulator hello Futtatás/Debug helyileg használatával. | Azure-beli |
   | --- | --- | --- |
   | ASP.NET-webalkalmazás |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Válassza ki a hello URL-cím hivatkozás jelenik meg hello **telepítési** hello lapján **Azure tevékenységnapló** tooload hello start lap hello böngészőben. |
   | 2. az ASP.NET MVC webalkalmazás |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Válassza ki a hello URL-cím hivatkozás jelenik meg hello **telepítési** hello lapján **Azure tevékenységnapló** tooload hello start lap hello böngészőben. |
   | 3. az ASP.NET MVC webalkalmazás |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Válassza ki a hello URL-cím hivatkozás jelenik meg hello **telepítési** hello lapján **Azure tevékenységnapló** tooload hello start lap hello böngészőben. |
   | 4. az ASP.NET MVC webalkalmazás |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Válassza ki a hello URL-cím hivatkozás jelenik meg hello **telepítési** hello lapján **Azure tevékenységnapló** tooload hello start lap hello böngészőben. |
   | Üres ASP.NET-webalkalmazás |Fel kell vennie az alkalmazáshoz, amely a webes projekthez hello kezdőlapot állítja .aspx lapot. Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Ha egy alapértelmezett .aspx lapot az alkalmazásban, válassza a hello URL-cím hiperhivatkozás hello megjelenő **telepítési** hello lapján **Azure tevékenységnapló** és ezen a lapon be van töltve, hello böngészőben. Ha egy másik .aspx lapot, kell toonavigate toothis adott lap hello formátum az URL-cím a következő használatával:`<url for deployment>/<name of page>.aspx` |
   | A Silverlight alkalmazás |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Toonavigate toohello adott lap van szüksége az alkalmazás hello formátum az URL-cím a következő használatával:`<url for deployment>/<name of page>.aspx` |
   | A Silverlight üzleti alkalmazás |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Toonavigate toohello adott lap van szüksége az alkalmazás hello formátum az URL-cím a következő használatával:`<url for deployment>/<name of page>.aspx` |
   | A Silverlight navigációs alkalmazás |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Toonavigate toohello adott lap van szüksége az alkalmazás hello formátum az URL-cím a következő használatával:`<url for deployment>/<name of page>.aspx` |
   | WCF-alkalmazás |A hello kezdési a WCF-szolgáltatások projekthez lapon meg kell adni az hello .svc kiterjesztésű fájl. Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Az alkalmazás használatával hello formátum az URL-cím a következő toonavigate toohello svc fájl szükséges:`<url for deployment>/<name of service file>.svc` |
   | Munkafolyamat-szolgáltatás WCF-alkalmazás |A hello kezdési a WCF-szolgáltatások projekthez lapon meg kell adni az hello .svc kiterjesztésű fájl. Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Az alkalmazás használatával hello formátum az URL-cím a következő toonavigate toohello svc fájl szükséges:`<url for deployment>/<name of service file>.svc` |
   | Az ASP.NET dinamikus entitások |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Frissítenie kell a hello kapcsolati karakterlánc (lásd a következő szakaszt). Az alkalmazás hello formátum tekintse meg az URL-cím való használatával is kell toonavigate toohello adott lap:`<url for deployment>/<name of page>.aspx` |
   | Az ASP.NET dinamikus adatok Linq tooSQL |Hello menüsávban válassza **Debug**, **Start Debugging** (billentyűzet: hello válasszon **F5** kulcs.). |Hajtsa végre az alábbi eljárással hello lépéseket: SQL Azure adatbázis használata az alkalmazás (Ez a témakör korábbi szakaszában talál). Az alkalmazás hello formátum tekintse meg az URL-cím való használatával is kell toonavigate toohello adott lap:`<url for deployment>/<name of page>.aspx` |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Frissítse a kapcsolati karakterláncot az ASP.NET dinamikus entitások
### <a name="tooupdate-a-connection-string-for-aspnet-dynamic-entities"></a>a kapcsolati karakterlánc az ASP.NET dinamikus entitások tooUpdate
1. toocreate dinamikus entitások ASP.NET webalkalmazás, használható SQL Azure-adatbázis hello hello eljárás lépéseit kövesse **SQL Azure-adatbázis használata az alkalmazás** a témakör korábbi részében.
2. Adja hozzá a hello táblák és a mezőket, amelyekre szüksége van az adatbázishoz a hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
3. Ez az alkalmazástípus hello kapcsolati karakterláncot hello web.config fájl formátuma a következő hello rendelkezik:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Frissítés hello *connectionString* hello ADO.NET kapcsolati karakterláncot az SQL Azure database az érték az alábbiak szerint:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```
4. toosave hello web.config fájl toohello kapcsolati karakterláncot, hello menüsávban válassza el hello módosításokkal **fájl**, **menteni a web.config**.

## <a name="supported-project-templates"></a>Támogatott Projektsablonjai
a webes alkalmazás tooAzure toopublish, hello alkalmazás kell sablonok valamelyikét használja hello projekt C# és Visual Basic hello az alábbi táblázatban szereplő.

| Sablon projektcsoport | Projektsablon |
| --- | --- |
| Web |ASP.NET-webalkalmazás |
| Web |2. az ASP.NET MVC webalkalmazás |
| Web |3. az ASP.NET MVC webalkalmazás |
| Web |ASP.NET MVC4-webalkalmazás |
| Web |Üres ASP.NET-webalkalmazás |
| Web |ASP.NET MVC 2 üres webalkalmazás |
| Web |Az ASP.NET dinamikus adatok entitások webes alkalmazás |
| Web |Az ASP.NET dinamikus adatok Linq tooSQL webalkalmazás |
| A Silverlight |A Silverlight alkalmazás |
| A Silverlight |A Silverlight üzleti alkalmazás |
| A Silverlight |A Silverlight navigációs alkalmazás |
| WCF |WCF-alkalmazás |
| WCF |Munkafolyamat-szolgáltatás WCF-alkalmazás |
| Munkafolyamat |Munkafolyamat-szolgáltatás WCF-alkalmazás |

## <a name="next-steps"></a>Következő lépések
Közzétételi további információkért lásd: [tooPublish előkészítéséhez, illetve az Azure-alkalmazás, a Visual Studio telepítése](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Emellett olvassa el [beállítás mentése nevű hitelesítő adatok](vs-azure-tools-setting-up-named-authentication-credentials.md).
