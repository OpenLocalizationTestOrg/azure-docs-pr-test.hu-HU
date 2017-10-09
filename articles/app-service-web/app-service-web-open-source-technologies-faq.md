---
title: "aaaOpen-forrás technológiák gyakori kérdések az Azure web apps |} Microsoft Docs"
description: "Az Azure App Service Web Apps szolgáltatása hello nyílt forráskódú technológiákkal kapcsolatos kérdések válaszok toofrequently beolvasása."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 35cff4f322859d25972747cf55aa7c4316381a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a>Gyakori kérdések az Azure Web Apps nyílt forráskódú technológiák

Ez a cikk kérdések (GYIK) válaszok toofrequently rendelkezik hello nyílt forráskódú technológiák összefüggő problémákkal kapcsolatos [az Azure App Service Web Apps szolgáltatásának](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a>A ClearDB adatbázist nem működik. Hogyan lehet elhárítani ezt?

Adatbázissal kapcsolatos problémák esetén kérje [ClearDB támogatási](https://www.cleardb.com/developers/help/support). 

Válaszok toocommon kérdésekre ClearDB, lásd: [ClearDB – gyakori kérdések](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="why-isnt-my-cleardb-database-listed-in-hello-portal"></a>A ClearDB adatbázist miért nem szerepel a hello portálon?

Ha egy ClearDB adatbázist hoz létre a hello [Azure-portálon](http://portal.azure.com/), hello adatbázis nem jelenik meg a hello [a klasszikus Azure portálon](http://manage.windowsazure.com/). Ez elkerülhető toowork, manuálisan társíthatja az adatbázis toohello webes alkalmazás.

Hasonló módon ha hello egy ClearDB adatbázist hoz létre [a klasszikus Azure portálon](http://manage.windowsazure.com/), nem jelenik meg az adatbázis a hello [Azure-portálon](http://portal.azure.com/). Ebben az esetben nincs megkerülő megoldás érhető el. 

További információkért lásd: [ClearDB MySQL-adatbázisok – gyakori kérdések az Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a>A ClearDB adatbázist miért nem volt át az előfizetést az áttelepítés során?

Erőforrás-áttelepítés végrehajtása előfizetések között, bizonyos korlátozások vonatkoznak. A ClearDB MySQL-adatbázis egy külső szolgáltatás, és nem telepíti át az Azure-előfizetés az áttelepítés során.

Ha nem Ön hello a MySQL-adatbázis áttelepítése az Azure-erőforrások áttelepítése előtt, előfordulhat, hogy a ClearDB MySQL-adatbázis nem elérhető. tooavoid ez, először, manuálisan telepítse át a ClearDB adatbázist, majd utána áttelepíteni az hello Azure-előfizetés a webalkalmazás.

További információkért lásd: [ClearDB MySQL-adatbázisok – gyakori kérdések az Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="how-do-i-turn-on-php-logging-tootroubleshoot-php-issues"></a>Hogyan PHP tootroubleshoot PHP problémák naplózás bekapcsolása?

a PHP-naplózást tooturn:

1. Jelentkezzen be tooyour [Kudu webhely](https://*yourwebsitename*.scm.azurewebsites.net).
2. Hello felső menüben válassza ki a **Debug konzol** > **CMD**.
3. Jelölje be hello **hely** mappát.
4. Jelölje be hello **wwwroot** mappát.
5. Jelölje be hello  **+**  ikonra, és válassza **új fájl**.
6. Állítsa be a hello fájlnév túl**. user.ini**.
7. Válasszon hello ceruza ikont mellett túl**. user.ini**.
8. Hello fájlban adja hozzá ezt a kódot:`log_errors=on`
9. Kattintson a **Mentés** gombra.
10. Válasszon hello ceruza ikont mellett túl**wp-config.php**.
11. A következő kód hello szöveg toohello módosítása:
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging too/wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings tooscreendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors tooscreenini_set('display_errors', 0);
   ```
12. Indítsa újra a webes alkalmazás hello hello webes alkalmazás menü, az Azure-portálon.

További információkért lásd: [engedélyezése WordPress hibanaplókat](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a>Hogyan Python-alkalmazások hibáinak jelentkezni az App Service szolgáltatásban üzemeltetett alkalmazásokat?

Python-alkalmazások hibáinak toocapture:

1. Válassza ki a webalkalmazás az Azure-portálon hello **beállítások**.
2. A hello **beállítások** lapon jelölje be **Alkalmazásbeállítások**.
3. A **Alkalmazásbeállítások**, adja meg a következő kulcs/érték pár hello:
    * Kulcs: WSGI_LOG
    * Érték: D:\home\site\wwwroot\logs.txt (adja meg a kívánt fájlnevet)

Meg kell jelennie hibák hello logs.txt fájlban hello wwwroot mappában.

## <a name="how-do-i-change-hello-version-of-hello-nodejs-application-that-is-hosted-in-app-service"></a>Hogyan változtathatom meg a Node.js-alkalmazás az App Service-ben üzemeltetett hello hello verzióját?

hello Node.js-alkalmazás toochange hello verzióját használja hello alábbi beállítások egyikét:

*   Hello Azure-portálon, használjon **Alkalmazásbeállítások**.
    1. A hello Azure-portálon válassza a tooyour webalkalmazás.
    2. A hello **beállítások** panelen válassza **Alkalmazásbeállítások**.
    3. A **Alkalmazásbeállítások**, WEBSITE_NODE_DEFAULT_VERSION hello kulcsként is felvehet, és azt hello Node.js verziója hello értékként.
    4. Nyissa meg tooyour [Kudu konzol](https://*yourwebsitename*.scm.azurewebsites.net).
    5. toocheck hello Node.js verziót, írja be a következő parancs hello:  
   ```
   node -v
   ```
*   Módosítsa a hello iisnode.yml fájlt. Változó hello Node.js verziót a hello iisnode.yml fájlt csak beállítja hello futtatókörnyezetben, hogy az iisnode használja. A Kudu cmd, míg mások továbbra is használhatják az beállított hello Node.js-verzió **Alkalmazásbeállítások** a hello Azure-portálon.

    tooset hello iisnode.yml fájlt, hozzon létre manuálisan egy iisnode.yml fájlt az alkalmazás gyökérmappájába. Hello fájlban a következő sor hello a következők:
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   Be hello iisnode.yml fájlt package.json forrás vezérlő üzembe helyezése során.
    hello Azure forrás vezérlő telepítési folyamata magában foglalja a hello a következő lépéseket:
    1. Azure-webalkalmazás tartalmi toohello helyezi.
    2. Egy alapértelmezett telepítési parancsfájl hoz létre, ha nincs hello webes alkalmazás gyökérmappájában (Deploy.cmd fájl, .deployment fájlok) ilyen.
    3. Futtatja a telepítési parancsfájlt, amelyben létrehoz egy iisnode.yml fájlt Ha, említse meg a Node.js-verzió hello hello package.json fájl > motor`"engines": {"node": "5.9.1","npm": "3.7.3"}`
    4. hello iisnode.yml fájlt a következő kódsort hello rendelkezik:
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-hello-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a>A WordPress alkalmazás az App Service-ben üzemeltetett üdvözlőüzenetére "Adatbázis-kapcsolatot létrehozó Error" látható. Hogyan hibaelhárítása Ez?

Ha ez a hiba a Azure WordPress alkalmazás, tooenable php_errors.log és debug.log című teljes hello lépéseket részletesen [engedélyezése WordPress hibanaplókat](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

Hello naplókat, ha engedélyezve vannak Reprodukálja hello hibát, és ellenőrizze az hello naplók toosee, ha nincs kapcsolat:
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded hello ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

Ha ez a hiba a debug.log vagy php_errors.log fájlok jelenik meg, az alkalmazás meghaladja hello kapcsolatok száma. A ClearDB tárolja, ha ellenőrizze a rendelkezésre álló kapcsolatok hello száma a [service-csomag](https://www.cleardb.com/pricing.view).

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a>Hogyan debug a Node.js-alkalmazás az App Service-ben üzemeltetett?

1.  Nyissa meg tooyour [Kudu konzol](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).
2.  Nyissa meg tooyour application logs mappában (D:\home\LogFiles\Application).
3.  Hello logging_errors.txt fájlban ellenőrizze a tartalom.

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a>Natív Python-modulok telepítése egy App Service webalkalmazás vagy API-alkalmazás

Néhány csomag Előfordulhat, hogy telepítse az Azure-ban a pip használatával. hello csomag valószínűleg csak akkor érhető el a Python-Csomagindexet hello, vagy lehet, hogy egy fordító szükséges (a fordító nem áll rendelkezésre hello futtató hello webalkalmazást az App Service szolgáltatásban). Az App Service web apps és API-alkalmazások natív modulok telepítésével kapcsolatos információkért lásd: [telepítése Python-modulok az App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).

## <a name="how-do-i-deploy-a-django-app-tooapp-service-by-using-git-and-hello-new-version-of-python"></a>Hogyan telepíthetem a Django app tooApp Service Python a Git és hello verziójának használatával?

A Django telepítésével kapcsolatos információkért lásd: [egy Django app tooApp szolgáltatás telepítése](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).

## <a name="where-are-hello-tomcat-log-files-located"></a>Hol vannak hello Tomcat naplófájlok található?

Az Azure piactér és egyéni telepítésekhez:

* Mappájának helye: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs
* Fontos fájlok:
    * catalina. *éééé-hh-nn*.log
    * gazdagép-kezelő. *éééé-hh-nn*.log
    * localhost. *éééé-hh-nn*.log
    * a Manager objektum. *éééé-hh-nn*.log
    * site_access_log. *éééé-hh-nn*.log


A portál **Alkalmazásbeállítások** központi telepítések:

* Mappájának helye: D:\home\LogFiles
* Fontos fájlok:
    * catalina. *éééé-hh-nn*.log
    * gazdagép-kezelő. *éééé-hh-nn*.log
    * localhost. *éééé-hh-nn*.log
    * a Manager objektum. *éééé-hh-nn*.log
    * site_access_log. *éééé-hh-nn*.log

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a>Hogyan hibáinak elhárítása JDBC kapcsolat hibái?

A következő üzenet a Tomcat naplókban hello tapasztalhatja:

```
hello web application[ROOT] registered hello JDBC driver [com.mysql.jdbc.Driver] but failed toounregister it when hello web application was stopped. tooprevent a memory leak,hello JDBC Driver has been forcibly unregistered
```

tooresolve hello hiba:

1. Távolítsa el az alkalmazás/lib mappában hello sqljdbc*.jar fájlt.
2. Ha hello egyéni Tomcat vagy Azure piactér Tomcat webkiszolgálókat használ, a .jar fájl toohello Tomcat lib mappa másolása.
3. Ha engedélyezi a Java hello Azure portal (válasszon **Java 1.8** > **Tomcat kiszolgálót**), másolása hello sqljdbc.* jar-fájlra, amely párhuzamos tooyour app hello mappában. Adja hozzá a következő classpath beállítás toohello web.config fájl hello:

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path toohello sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-toocopy-live-log-files"></a>Miért látom hibák, amikor élő naplófájlok toocopy?

Ha élő naplófájlok toocopy a Java-alkalmazások (például Tomcat), láthatja az FTP-hiba:

```
Error transferring file [filename] Copying files from remote side failed.
    
hello process cannot access hello file because it is being used by another process.
```

hello hibaüzenet hello FTP-ügyfél függően változhat.

Minden Java-alkalmazások rendelkeznek a zárolási problémát. Csak a Kudu támogatja, a fájl letöltése hello alkalmazás futtatása közben.

Leállítása hello app engedélyezi az FTP-hozzáférést toothese fájlok.

Egy másik megoldás, toowrite webjobs-feladat ütemezés szerint fut, és másolja át a fájlok tooa másik címtárhoz. Egy minta-projekt lásd: hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projekt.

## <a name="where-do-i-find-hello-log-files-for-jetty"></a>Hol található a Jetty hello naplófájlok?

A piactéren, és egyéni telepítésekhez hello naplófájl hello D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs mappában van. Vegye figyelembe, hogy hello mappájának helye hello használt Jetty módjától függ. Például hello elérési utat az itt megadott a Jetty 9.1.2. Keresse meg jetty_*YYYY_MM_DD*. stderrout.log.

A portál Alkalmazásbeállítás telepítések esetében hello naplófájl D:\home\LogFiles van. Keresse meg jetty_*YYYY_MM_DD*. stderrout.log

## <a name="can-i-send-email-from-my-azure-web-app"></a>Küldhetek e-mailt a saját Azure-webalkalmazás?

App Service beépített e-mail-szolgáltatás nem rendelkezik. Az e-mailek küldése az alkalmazásból, néhány jó alternatíva ez című [Stack Overflow vitafórum](http://stackoverflow.com/questions/17666161/sending-email-from-azure).

## <a name="why-does-my-wordpress-site-redirect-tooanother-url"></a>Miért saját WordPress-webhely tooanother URL-Címének átirányítása?

Ha nemrégiben telepítette át tooAzure, a WordPress előfordulhat, hogy toohello régi tartomány URL-Címének átirányítása. Ennek oka egy beállítás hello MySQL-adatbázisban.

WordPress ismerős + használható tooupdate hello átirányítási URL-címet közvetlenül a hello adatbázis bővítmény Azure hely. WordPress ismerős + használatával kapcsolatos további információkért lásd: [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

Másik lehetőségként toomanually frissítési hello átirányítási URL-Címének használatával, az SQL-lekérdezések vagy PHPMyAdmin tetszés szerint lásd: [WordPress: toowrong URL-cím átirányítása](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a>A WordPress bejelentkezési jelszó módosítása

Ha elfelejtette a jelszavát WordPress-bejelentkezés, WordPress ismerős + tooupdate használhatja azt. a jelszó, a telepítés hello WordPress ismerős + Azure hely bővítmény és a majd teljes hello lépéseket tooreset [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="i-cant-sign-in-toowordpress-how-do-i-resolve-this"></a>Nem tud bejelentkezni tooWordPress. Hogyan lehet elhárítani ezt?

Ha nemrég a beépülő modul telepítése után WordPress tévedéssel, lehetséges, hogy egy hibás beépülő modult. WordPress ismerős +, amelyek segítségével Azure hely bővítmény letiltása a WordPress beépülő modulok. További információkért lásd: [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="how-do-i-migrate-my-wordpress-database"></a>Hogyan telepítse át a WordPress adatbázist?

Egy csatlakoztatott tooyour WordPress-webhely áttelepítése hello MySQL adatbázis több lehetőség közül választhat:

* A fejlesztők: Használata hello [parancssort vagy PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)
* Nem-Fejlesztők: [WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)

## <a name="how-do-i-help-make-wordpress-more-secure"></a>Hogyan segít, WordPress biztonságosabbá?

Tekintse meg a WordPress, ajánlott biztonsági eljárásokkal kapcsolatos toolearn [ajánlott eljárások az Azure-ban WordPress biztonsági](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).

## <a name="i-am-trying-toouse-phpmyadmin-and-i-see-hello-message-access-denied-how-do-i-resolve-this"></a>Próbálok toouse PHPMyAdmin, és látható üdvözlőüzenetére "Hozzáférés megtagadva." Hogyan lehet elhárítani ezt?

A probléma tapasztalhat, ha hello MySQL alkalmazásbeli funkció még nem fut. Ez App Service-példányban. tooresolve hello problémát, próbálja meg tooaccess a webhelyet. Hello szükséges eljárások, beleértve a hello MySQL alkalmazáson belüli folyamat elindul. hello folyamatok jelenik meg, hogy az alkalmazás fut, a Process Explorer MySQL győződjön meg arról, hogy mysqld.exe tooverify.

Miután meggyőződött arról, hogy fut-e a MySQL alkalmazásbeli, próbálja toouse PHPMyAdmin.

## <a name="i-get-an-http-403-error-when-i-try-tooimport-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a>A HTTP 403-as hiba jelenik meg: tooimport próbálja vagy alkalmazásbeli MySQL adatbázis exportálása PHPMyadmin használatával. Hogyan lehet elhárítani ezt?

Ha a Látványelem egy régebbi verzióját használja, akkor léptek fel egy ismert hiba. tooresolve hello problémát, a frissítési tooa Chrome újabb verziója. Is próbálkozzon másik böngészővel, például az Internet Explorer vagy Edge, ahol hello nem fordul elő.
