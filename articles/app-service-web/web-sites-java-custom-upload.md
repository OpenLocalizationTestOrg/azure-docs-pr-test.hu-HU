---
title: "olyan egyéni Java web app tooAzure aaaUpload"
description: "Az oktatóanyag bemutatja, hogyan tooupload olyan egyéni Java web app tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: eb2ccd83-e5c6-444e-a0fc-08fd5cc8326c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 0cb4a682bb25d86ff08bfd03628c89795c58451e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-java-web-app-tooazure"></a>Olyan egyéni Java web app tooAzure feltöltése
Ez a témakör azt ismerteti, hogyan a tooupload olyan egyéni Java web app túl[Azure App Service] webalkalmazások. Tartalmazza az tooany Java webhelyen vagy a web app és az adott alkalmazásokra vonatkozó példákat is érvényes információkat tartalmaznak.

Vegye figyelembe, hogy Azure arra szolgál, hogy hello Azure Portal konfigurációs UI használó Java-webalkalmazások létrehozása, és hello Azure piactér következő dokumentált [Java-webalkalmazás létrehozása az Azure App Service](web-sites-java-get-started.md). Ez az oktatóanyag forgatókönyvek, amelyben nem szeretné, hogy toouse hello Azure Portal konfigurációs UI vagy hello Azure piactér van.  

## <a name="configuration-guidelines"></a>Beállítási útmutatója
hello következő várt, de egyéni Azure Java-webalkalmazások hello-beállításokat ismerteti.

* hello Java-folyamat által használt hello HTTP-port dinamikusan hozzá van rendelve.  hello folyamat hello portjával hello környezeti változót kell használnia `HTTP_PLATFORM_PORT`.
* Az összes figyelési portokat eltérő hello egyetlen HTTP-figyelő le kell tiltani.  A Tomcat is tartalmazó hello leállítás, HTTPS és AJP portok.
* hello tároló toobe csak IPv4-forgalom konfigurálni kell.
* Hello **indítási** parancsot a hello alkalmazásnak kell toobe hello konfiguráció beállítása.
* Mappák írási engedélye igénylő alkalmazások kell hello Azure web apphoz tartalom könyvtárban, amely toobe **D:\home**.  hello környezeti változó `HOME` tooD:\home hivatkozik.  

Szükség szerint hello web.config fájlt a környezeti változók állíthatja be.

## <a name="webconfig-httpplatform-configuration"></a>Web.config httpPlatform konfiguráció
hello következő ismerteti hello **httpPlatform** formátumban web.config belül.

**argumentumok** (alapértelmezett = ""). Argumentumok toohello végrehajtható fájl vagy parancsfájl hello megadott **processPath** beállítást.

Példák (megjelenítve **processPath** része):

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


**processPath** -elérési út toohello végrehajtható vagy parancsfájlt, amely figyeli a HTTP-kérelmek folyamat elindul.

Példák:

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

**rapidFailsPerMinute** (alapértelmezett = 10.) Hány alkalommal megadott hello folyamat **processPath** percenként toocrash engedélyezett. Ha túllépi ezt a határt, **HttpPlatformHandler** hello perc hello hátralévő hello folyamatának indítási állapotára leáll.

**requestTimeout** (alapértelmezett = "00: 02:00".) Időtartam, amelynek **HttpPlatformHandler** figyel hello folyamat válaszára várakozik `%HTTP_PLATFORM_PORT%`.

**startupRetryCount** (alapértelmezett = 10.) Ennyiszer **HttpPlatformHandler** megpróbál toolaunch hello folyamat megadott **processPath**. Lásd: **startupTimeLimit** további részleteket.

**startupTimeLimit** (alapértelmezett = 10 másodperc.) Időtartam, amelynek **HttpPlatformHandler** várakozik hello végrehajtható fájl, parancsfájl toostart egy folyamat hello portot figyel.  Ha az időkorlát túl lett lépve, **HttpPlatformHandler** fog hello folyamat leállítása és toolaunch próbálja újból **startupRetryCount** alkalommal.

**stdoutLogEnabled** (alapértelmezett = "true".) Igaz értéke esetén **stdout** és **stderr** hello megadott hello folyamat **processPath** beállítás lesz a átirányított toohello fájl  **stdoutLogFile** (lásd: **stdoutLogFile** szakaszban).

**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Fájl abszolút elérési útja, amelyhez **stdout** és **stderr** hello folyamat során megadott **processPath** bekerülnek a naplóba.

> [!NOTE]
> `%HTTP_PLATFORM_PORT%`amely toospecified részeként kell különleges helyőrző **argumentumok** vagy hello részeként **httpPlatform** **environmentVariables** listája. Ez egy által belsőleg generált port váltják **HttpPlatformHandler** , hogy a megadott hello folyamat **processPath** figyelheti a ezt a portot.
> 
> 

## <a name="deployment"></a>Környezet
Java-alapú webalkalmazások könnyen hello azonos azt jelenti, hogy a legtöbb hello-alapú Internet Information Services (IIS) webes alkalmazások használt keresztül is telepíthető.  FTP, Git és a Kudu használata az összes támogatott mechanizmusok, mint van a web Apps integrált SCM funkció hello. WebDeploy works protokollként, azonban Java nem kidolgozott a Visual Studio WebDeploy nem felelnek meg a Java web app telepítési használati eseteket.

## <a name="application-configuration-examples"></a>Alkalmazáskonfiguráció példák
A következő alkalmazások, a web.config fájl- és hello hello Alkalmazáskonfiguráció biztosítja példák tooshow, hogyan tooenable a Java-alkalmazás az App Service Web Apps.

### <a name="tomcat"></a>Tomcat
Nincsenek App Service Web Apps végzik a Tomcat két változata, de napjainkban továbbra is meglehetősen lehetséges tooupload ügyfél olyan specifikus példányai. Alább példája egy másik Java virtuális gép (JVM) a Tomcat telepítését.

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat" 
            arguments="">
          <environmentVariables>
            <environmentVariable name="CATALINA_OPTS" value="-Dport.http=%HTTP_PLATFORM_PORT%" />
            <environmentVariable name="CATALINA_HOME" value="%HOME%\site\wwwroot\bin\tomcat" />
            <environmentVariable name="JRE_HOME" value="%HOME%\site\wwwroot\bin\java" /> <!-- optional, if not specified, this will default too%programfiles%\Java -->
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

A Tomcat ügyféloldali hello, néhány konfigurációs módosítások vannak végrehajtott toobe igénylő. hello server.xml kell szerkeszteni toobe tooset:

* Leállítás port = -1
* HTTP-összekötő port = {port.http} $
* HTTP-összekötő cím = "127.0.0.1"
* HTTPS- és AJP összekötők megjegyzéssé
* hello IPv4 beállítás is megadható a hello catalina.properties fájlban adhat hozzá`java.net.preferIPv4Stack=true`

App Service Web Apps Direct3d hívások nem támogatottak. toodisable, adja hozzá a következő Java beállítás kell az alkalmazás hívásokat ilyen hello:`-Dsun.java2d.d3d=false`

### <a name="jetty"></a>Jetty
Hello helyzet a Tomcat, mivel az ügyfelek feltöltheti saját példányok a Jetty. Futó hello teljes telepítése Jetty hello esetben hello konfigurációs néz ki:

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httppPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" 
             arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP_PLATFORM_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"
            startupTimeLimit="20"
          startupRetryCount="10"
          stdoutLogEnabled="true">
        </httpPlatform>
      </system.webServer>
    </configuration>

hello Jetty beállításokat kell hello start.ini tooset módosított toobe `java.net.preferIPv4Stack=true`.

### <a name="springboot"></a>Springboot
A sorrend tooget egy Springboot alkalmazást futtató tooupload a JAR vagy WAR-fájl szükséges, és adja hozzá a következő web.config fájl hello. hello web.config fájl hello wwwroot mappába kerül. A hello web.config módosítása hello argumentumok toopoint tooyour JAR-fájlra, a hello példa hello JAR-fájlt a következő mappában található hello wwwroot is.  

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
            arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\my-web-project.jar&quot;">
        </httpPlatform>
      </system.webServer>
    </configuration>


### <a name="hudson"></a>Hudson
A tesztben használt Hudson 3.1.2 war hello és hello alapértelmezett Tomcat 7.0.50 példány, de hello felhasználói felület tooset művelet használata nélkül.  Mivel Hudson build szoftvereszköz, elkeverni tooinstall is azt a dedikált példányokat, ahol hello **AlwaysOn** jelző hello webalkalmazás állítható be.

1. A webalkalmazás a gyökérkönyvtár, azaz **d:\home\site\wwwroot**, hozzon létre egy **webapps** könyvtár (Ha még nincs letöltve), és helyezze a Hudson.war **d:\home\site\wwwroot\webapps**.
2. Töltse le az apache maven 3.0.5 (kompatibilis Hudson), és helyezze el a **d:\home\site\wwwroot**.
3. A web.config létrehozása **d:\home\site\wwwroot** és a Beillesztés hello tartalmát, a következő:
   
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>
          <system.webServer>
            <handlers>
              <add name="httppPlatformHandler" path="*" verb="*" 
        modules="httpPlatformHandler" resourceType="Unspecified" />
            </handlers>
            <httpPlatform processPath="%AZURE_TOMCAT7_HOME%\bin\startup.bat"
        startupTimeLimit="20"
        startupRetryCount="10">
        <environmentVariables>
          <environmentVariable name="HUDSON_HOME" 
        value="%HOME%\site\wwwroot\hudson_home" />
          <environmentVariable name="JAVA_OPTS" 
        value="-Djava.net.preferIPv4Stack=true -Duser.home=%HOME%/site/wwwroot/user_home -Dhudson.DNSMultiCast.disabled=true" />
        </environmentVariables>            
            </httpPlatform>
          </system.webServer>
        </configuration>
   
    Ezen a ponton hello webalkalmazás lehet indítani tootake hello módosításokat.  Csatlakozás toohttp://yourwebapp/hudson toostart Hudson.
4. Miután Hudson konfigurálja magát, a következő képernyő hello kell megjelennie:
   
    ![Hudson](./media/web-sites-java-custom-upload/hudson1.png)
5. Hozzáférés hello Hudson konfigurálólapját: kattintson a **kezelése Hudson**, és kattintson a **rendszer konfigurálása**.
6. Alább látható módon JDK hello konfigurálása:
   
    ![Hudson konfiguráció](./media/web-sites-java-custom-upload/hudson2.png)
7. Maven adja meg alább látható módon:
   
    ![Maven-konfiguráció](./media/web-sites-java-custom-upload/maven.png)
8. Hello beállítások mentéséhez. Hudson kell konfigurált és használatra kész.

A Hudson további információkért lásd: [http://hudson-ci.org](http://hudson-ci.org).

### <a name="liferay"></a>Liferay
App Service Web Apps Liferay támogatott. Liferay jelentős memória lehet szükség, mert egy közepes vagy nagyméretű speciális feldolgozó, amely elegendő memóriával biztosít a toorun kell hello webes alkalmazást. Liferay több percig toostart is foglalja el. Ezért ajánlott hello webalkalmazás túl beállítása**mindig a**.  

Tomcat Liferay Community Edition GA3 kötegelt 6.1.2 használ, hello következő fájlok volt módosítani Liferay letöltése után:

**Server.XML**

* Módosítsa a leállítási port túl-1.
* HTTP-összekötő túl módosítása`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`
* Hello AJP összekötő megjegyzéssé.

A hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** mappa, hozzon létre egy fájlt **portal-ext.properties**. A fájlnak kell toocontain egy sorban, ahogy az itt látható:

    liferay.home=%HOME%/site/wwwroot/liferay

Hello: hello tomcat-7.0.40 mappa, azonos könyvtár szinten hozzon létre egy fájlt **web.config** a tartalom a következő hello:

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
    <add name="httpPlatformHandler" path="*" verb="*"
         modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%HOME%\site\wwwroot\tomcat-7.0.40\bin\catalina.bat" 
                      arguments="run" 
                      startupTimeLimit="10" 
                      requestTimeout="00:10:00" 
                      stdoutLogEnabled="true">
          <environmentVariables>
      <environmentVariable name="CATALINA_OPTS" value="-Dport.http=%HTTP_PLATFORM_PORT%" />
      <environmentVariable name="CATALINA_HOME" value="%HOME%\site\wwwroot\tomcat-7.0.40" />
            <environmentVariable name="JRE_HOME" value="D:\Program Files\Java\jdk1.7.0_51" /> 
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

A hello **httpPlatform** blokkolja, hello **requestTimeout** túl értéke "00: 10:00".  Akkor csökkenteni lehet, de esetén a rendszer valószínűleg toosee során időtúllépési hibák Liferay rendszerindítása van.  Ha ez az érték megváltozik, majd hello **connectionTimeout** a hello tomcat server.xml is módosítani kell.  

Érdemes megjegyezni, hogy hello JRE_HOME környezetben varariable hello web.config toopoint toohello fent megadott 64 bites JDK. hello alapértelmezett érték a 32 bites, de Liferay memória magas szintű igényelhetnek, mivel toouse javasolt 64 bites JDK hello.

Miután a módosítások, indítsa újra a webes alkalmazás Liferay fut, majd, nyissa meg a http://yourwebapp. hello Liferay portal hello web app legfelső szintű érhető el. 

## <a name="next-steps"></a>Következő lépések
Liferay kapcsolatos további információkért lásd: [http://www.liferay.com](http://www.liferay.com).

További információt a Java [Azure Java-fejlesztőknek](/java/azure).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
