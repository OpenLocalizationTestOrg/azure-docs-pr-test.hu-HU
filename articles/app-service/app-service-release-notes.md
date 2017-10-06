---
title: "aaaAzure SDK for .NET 2.5.1-es verzióval készült kibocsátási megjegyzések"
description: "Az Azure SDK for .NET 2.5.1-es verzióval készült kibocsátási megjegyzések"
services: app-service
documentationcenter: .net,nodejs,java
author: Juliako
manager: erikre
editor: 
ms.assetid: 8d3d815f-bb58-447e-8ff0-f9b9603c7b00
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/10/2016
ms.author: juliako
ms.openlocfilehash: 5ee7688617c966baa409045881c172bbbc55ff63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-251-release-notes"></a>Az Azure SDK for .NET 2.5.1-es verzióval készült kibocsátási megjegyzések
Ez a dokumentum .NET 2.5.1-es verzióval készült kibocsátási hello kibocsátási megjegyzések a hello Azure SDK-t tartalmaz. 

## <a name="azure-sdk-for-net-251-release-notes"></a>Az Azure SDK for .NET 2.5.1-es verzióval készült kibocsátási megjegyzések
hello a következők új szolgáltatásokat és hello Azure SDK for .NET 2.5.1-es verzióval készült frissítések.

* Kapcsolódó túl új features\scenarios**Web eszközök Extensions**. 
  
  * Azure-webhelyek átnevezett tooAzure App Service volt. További információ: [Azure App Service és a meglévő Azure-szolgáltatások](../app-service-web/app-service-changes-existing-services.md).
  * Az Azure API-alkalmazások (előzetes verzió) már támogatja az, hogy az ügyfelek projekteket ASP.NET API-alkalmazások közzététele, és ezután használja az hello Hozzáadás > Azure API App ügyfél kézmozdulat kódban C# projektek toogenerate hello hello szerkezete alapján telepített API-alkalmazásba. 
  * a Server Explorer hello webhelyek csomópont Csoportalapú csoportosítása Azure API-alkalmazások, a Mobile Apps és a Web Apps erőforrás támogatást tartalmaz hello Azure App Service csomópont helyett már elavult.
  * Az Azure Mobile Apps (előzetes verzió) már támogatja az, hogy az ügyfelek hozhatnak létre új Mobile Apps-projektek, vegye fel a Mobile Apps-vezérlők, hello projektek közzététele, és távolról az-alkalmazások hibakeresését.
  * Adja hozzá > Azure API App ügyfél-hitelesítési mód mostantól támogatja a helyi Swagger JSON-fájlokat, így Web API a fejlesztők a külső NuGets például a Swashbuckle toogenerate Swagger, vagy azt manuálisan hozhatnak létre. Ezzel a módszerrel ügyfél a fejlesztők a hello kódgenerálás szolgáltatások tooconsume bármely Swagger-végpont C#-projektben. 
  * Webalkalmazás és az API-alkalmazás közzétételi párbeszédpanelek volt továbbfejlesztett toosupport hello Azure Portal fogalma erőforrások csoportosítása és kiválasztása vagy létrehozása Azure-csoportok és az App Service-csomagok hello új Web App és az API-alkalmazás üzembe helyezési párbeszédpanelen jelennek meg. 
  * Az Azure API App Server Explorer csomópontok részletes API-alkalmazások hivatkozásra hello Azure portál, valamint egyéb szolgáltatások, mint az adatfolyam-napló és a távoli hibakereséshez hivatkozások toohello adja meg.
    
    Ismert problémák és a jelenlegi korlátozások az Azure SDK .NET 2.5.1-es verzióval készült [ez](app-service-release-notes.md#known_issues_2_5_1) az alábbi szakasz.
* Kapcsolódó túl új features\scenarios**a HDInsight Tools** ebben a kiadásban engedélyezve vannak a Visual Studióban. 
  
  * Hive parancsfájlok helyi érvényesítése. Kattintson a hello ellenőrzése parancsfájl gombra a hello eszköztár toosee, ha hiba történik a parancsfájlban. 
  * Javult a Hive-feladatok a hibakeresés. Hive-feladatok a Yarn naplók a Visual Studio elérésével most hibakeresési. Ha az alkalmazás teljesítményproblémákat, vizsgálja a YARN naplóit lesz hasznos információt biztosító...
  * (Nyilvános előzetes verzió) Kulcsszó automatikus-befejezését és az IntelliSense támogatja a Hive. Hive parancsfájlokat, a HDInsight Tools for Visual Studio szerzőként toohelp hozzáadott kulcsszó automatikus-végrehajtási és Hive IntelliSense támogatása.
  * A Storm-támogatás. Most már használhatja a HDInsight Tools Visual Studio toodevelop Storm topológiák/spoutokkal kapcsolatban/Boltokhoz C# nyelven íródtak. Hello topológia tooa Storm-fürt fejlesztett és hello topológia/bolt/spout állapotát tekintheti meg, majd küldje el. Használhatja a rendszer naplóit, és az ügyfél naplózza tootroubleshoot a Storm topológiák/Boltokhoz/spoutokkal kapcsolatban. A HDInsight alatt futó Storm használhatja a meglévő JAVA-eszközök.
    
    További információkért lásd: [első lépései a HDInsight Hadoop Tools for Visual Studio használatával](../hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

## <a id="known_issues_2_5_1"></a>Az Azure SDK for .NET 2.5.1-es verzióval készült ismert problémák és korlátozások
* Az Azure API-alkalmazások esetén a Mobile Apps telepítési cél látható. Web Apps kell lennie a Mobile Apps-célpontot csak hello csak egy későbbi kiadásban. 
* Az Azure API-alkalmazás kiépítés sikeres, de időnként sikertelen tooupdate hello folyamatot hello Azure App Service-tevékenység ablakában a eredményezhet. Kerülő megoldás lehet hello Azure Portal hello új Azure API App toocheck állapotát. 
* Fájl > Új projekt > API-alkalmazás > F5 élmény HTTP hibát okoz, mivel nincs default/index.html. Kerülő megoldás lehet toomanually Tallózás toohello/api/értékek URL-CÍMÉT. 
* Időnként, Server Explorer ikonok jelennek meg egybesimított. VS újraindítás megszünteti ezt a problémát. 
* Ha a rendszer kivételt hoz létre Web App és az API-alkalmazások (például a kvóta túllépése hibák vagy ismétlődő Azure API App átjáró neve) kiépítése során, a hello hibák megjelenítése néhány nyers JSON-szöveg. 
* Időszakos projekt-létrehozásával kapcsolatos problémák, ha az Application Insights be van jelölve, a projekt létrehozás időpontjában.
* Alkalmanként generált hello Azure API App Ügyfélkód hiányzik a névterek, manuálisan szereplő toobe kell (vagy a Visual Studio kötegek keresztül automatikusan importált) a kód toocompile. 
* Mobilalkalmazás-projektek közzétett tooweb alkalmazásokat kell lennie, de válasszon ki egy helyet, mivel a mobilalkalmazás-projektek igényel adatbázis az hello Azure-portál a mobilalkalmazás alkalmazásként létrehozott. 
* a Mobile Apps hello kezdőlapot hello kifejezést "mobilszolgáltatás" helyett "mobilalkalmazások" használja 
* Mobilalkalmazás-projekt létrehozása tooa perces toocreate is tarthat. 
* API-alkalmazás során (egyes esetekben) létesítési hiba ismét elérhető lesz a hello Azure API tükröző, hogy hello engedélyek nem állítható be megfelelően, miközben hello API-alkalmazás megfelelően lettek kiosztva és használatra kész. Engedélyt hello Azure portál használatával kézzel is beállíthatja.
* Az Application Insights API-alkalmazás sablonok és a mobilalkalmazás-sablonok nem támogatott.
* API-alkalmazás projektek Cloud Service projektek együtt nem használható.
* A C# API-alkalmazás projekt sablonok csak érhetők el.
* API-alkalmazás felhasználás keresztüli hello "Azure API App ügyfél hozzáadása" helyi menü csak akkor támogatott a C#.

