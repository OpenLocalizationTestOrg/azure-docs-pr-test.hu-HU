---
title: "Upload a custom Java web app to Azure (Egyéni Java-webalkalmazás feltöltése az Azure-ba)"
description: "Az oktatóanyag bemutatja, hogyan tölthet fel egyéni Java-webalkalmazás Azure App Service Web Apps."
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
ms.openlocfilehash: 9c8f9ee7780859f7640ac82d6ebce85082170ad7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upload-a-custom-java-web-app-to-azure"></a>Upload a custom Java web app to Azure (Egyéni Java-webalkalmazás feltöltése az Azure-ba)
Ez a témakör azt ismerteti, hogyan egyéni Java-webalkalmazás való feltöltéséhez [Azure App Service] webalkalmazások. Szereplő információkat a Java webhelyre vagy webalkalmazásra alkalmazás, és adott alkalmazásokra vonatkozó példákat is van.

Vegye figyelembe, hogy Azure arra szolgál, hogy az Azure portál konfiguráció felhasználói felületi, és az Azure piactér Java-webalkalmazások létrehozása a dokumentált [Java-webalkalmazás létrehozása az Azure App Service](web-sites-java-get-started.md). Ez az oktatóanyag, amelyben nem szeretné használni az Azure portál konfigurációs forgatókönyvek felhasználói felületén vagy az Azure piactéren.  

## <a name="configuration-guidelines"></a>Beállítási útmutatója
A következő egyéni Azure Java-webalkalmazások vár-beállításokat ismerteti.

* A Java-folyamat által használt HTTP-port dinamikusan hozzá van rendelve.  A folyamat kell használni a környezeti változó portjával `HTTP_PLATFORM_PORT`.
* Az egyetlen HTTP-figyelő eltérő összes figyelési port le kell tiltani.  A Tomcat, amely tartalmazza a Leállítás, HTTPS és AJP portok.
* A tároló kell megadni a csak IPv4-forgalmat.
* A **indítási** az alkalmazást kell a konfigurációban állítható be a parancsot.
* Mappák írási engedélye igénylő alkalmazásokat kell könyvtárban az Azure web app tartalom, amely **D:\home**.  A környezeti változó `HOME` D:\home hivatkozik.  

Szükség szerint a web.config fájlt a környezeti változók állíthatja be.

## <a name="webconfig-httpplatform-configuration"></a>Web.config httpPlatform konfiguráció
Az alábbiakban ismertetett a **httpPlatform** formátumban web.config belül.

**argumentumok** (alapértelmezett = ""). A végrehajtható fájl vagy parancsfájl a megadott argumentumok a **processPath** beállítást.

Példák (megjelenítve **processPath** része):

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


**processPath** -elérési utat a végrehajtható fájlt vagy parancsfájlt, amely figyeli a HTTP-kérelmek folyamat elindul.

Példák:

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

**rapidFailsPerMinute** (alapértelmezett = 10.) A folyamat a megadott számú **processPath** percenként összeomlási engedélyezett. Ha túllépi ezt a határt, **HttpPlatformHandler** leáll, a perc hátralevő folyamatának indítási állapotára.

**requestTimeout** (alapértelmezett = "00: 02:00".) Időtartam, amelynek **HttpPlatformHandler** a figyelést a következő folyamat válaszára várakozik `%HTTP_PLATFORM_PORT%`.

**startupRetryCount** (alapértelmezett = 10.) Ennyiszer **HttpPlatformHandler** próbál meg elindítani a megadott **processPath**. Lásd: **startupTimeLimit** további részleteket.

**startupTimeLimit** (alapértelmezett = 10 másodperc.) Időtartam, amelynek **HttpPlatformHandler** megvárja, hogy a végrehajtható fájl, a parancsfájl a porton figyel a folyamat elindításához.  Ha az időkorlát túl lett lépve, **HttpPlatformHandler** lesz a folyamat leállítása, és indítsa el újra megpróbálja **startupRetryCount** alkalommal.

**stdoutLogEnabled** (alapértelmezett = "true".) Igaz értéke esetén **stdout** és **stderr** a megadott folyamat a **processPath** beállítás irányítja át a megadott fájl **stdoutLogFile** (lásd: **stdoutLogFile** szakaszban).

**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Fájl abszolút elérési útja, amelyhez **stdout** és **stderr** a megadott folyamatának **processPath** bekerülnek a naplóba.

> [!NOTE]
> `%HTTP_PLATFORM_PORT%`amelyet részeként megadott különleges helyőrző **argumentumok** vagy részeként a **httpPlatform** **environmentVariables** listája. Ez egy által belsőleg generált port váltják **HttpPlatformHandler** , hogy a folyamat által megadott **processPath** figyelheti a ezt a portot.
> 
> 

## <a name="deployment"></a>Környezet
Alapú webalkalmazások telepíthető egyszerűen a azonos azt jelenti, hogy az Internet Information Services (IIS) használata a legtöbb Java-alapú webes alkalmazásokhoz.  FTP, Git és a Kudu használata az összes támogatott mechanizmusok, mint az integrált SCM képesség a web Apps. WebDeploy works protokollként, azonban Java nem kidolgozott a Visual Studio WebDeploy nem felelnek meg a Java web app telepítési használati eseteket.

## <a name="application-configuration-examples"></a>Alkalmazáskonfiguráció példák
A következő alkalmazások a web.config fájl-és az alkalmazás konfigurációs van példaként engedélyezése a Java-alkalmazás az App Service Web Apps megjelenítéséhez.

### <a name="tomcat"></a>Tomcat
Miközben Tomcat két változata, amelyeket az App Service Web Apps, még meglehetősen lehetséges töltse fel az ügyfél olyan specifikus példányai. Alább példája egy másik Java virtuális gép (JVM) a Tomcat telepítését.

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
            <environmentVariable name="JRE_HOME" value="%HOME%\site\wwwroot\bin\java" /> <!-- optional, if not specified, this will default to %programfiles%\Java -->
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

A Tomcat oldalon van néhány konfigurációs módosításokat kell végezni. A server.xml kell módosítani őket beállítani:

* Leállítás port = -1
* HTTP-összekötő port = {port.http} $
* HTTP-összekötő cím = "127.0.0.1"
* HTTPS- és AJP összekötők megjegyzéssé
* Az IPv4-beállítás is megadható a catalina.properties fájlban adhat hozzá`java.net.preferIPv4Stack=true`

App Service Web Apps Direct3d hívások nem támogatottak. Tiltsa le azokat, adja hozzá a következő Java-beállítást kell az alkalmazás hívásokat ilyen:`-Dsun.java2d.d3d=false`

### <a name="jetty"></a>Jetty
Mint esetében a Tomcat, az ügyfelek feltöltheti saját példányok a Jetty. Esetén az Jetty teljes telepítését futtatja, a konfiguráció néz ki:

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

A Jetty beállításokat kell beállítani a start.ini módosíthatók `java.net.preferIPv4Stack=true`.

### <a name="springboot"></a>Springboot
Egy Springboot eléréséhez alkalmazást futtató kell a JAR vagy WAR-fájl feltöltése, és adja hozzá a következő web.config fájlt. A web.config fájlt a wwwroot mappába kerül. A Web.config fájlban, az alábbi példában a JAR-fájlra, valamint a wwwroot mappában található a JAR-fájlra mutasson az argumentumok módosítása  

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
A tesztben a Hudson 3.1.2 war és az alapértelmezett Tomcat 7.0.50 példányt használja, de a felhasználói felületen dolgot beállítása nélkül.  Mivel Hudson szoftver build eszköz, amely telepíti azt a dedikált példányok javasoljuk ahol a **AlwaysOn** jelző állítható be a webalkalmazásban.

1. A webalkalmazás a gyökérkönyvtár, azaz **d:\home\site\wwwroot**, hozzon létre egy **webapps** könyvtár (Ha még nincs letöltve), és helyezze a Hudson.war **d:\home\site\wwwroot\webapps**.
2. Töltse le az apache maven 3.0.5 (kompatibilis Hudson), és helyezze el a **d:\home\site\wwwroot**.
3. A web.config létrehozása **d:\home\site\wwwroot** és illessze be az alábbiakat:
   
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
   
    Ezen a ponton a webes alkalmazás is újra kell indítani a módosítások érvénybe.  Csatlakozás http://yourwebapp/hudson Hudson elindításához.
4. Miután Hudson konfigurálja magát, a következő képernyő kell megjelennie:
   
    ![Hudson](./media/web-sites-java-custom-upload/hudson1.png)
5. A Hudson lap eléréséhez: kattintson a **kezelése Hudson**, és kattintson a **rendszer konfigurálása**.
6. Adja meg a JDK alább látható módon:
   
    ![Hudson konfiguráció](./media/web-sites-java-custom-upload/hudson2.png)
7. Maven adja meg alább látható módon:
   
    ![Maven-konfiguráció](./media/web-sites-java-custom-upload/maven.png)
8. A beállítások mentéséhez. Hudson kell konfigurált és használatra kész.

A Hudson további információkért lásd: [http://hudson-ci.org](http://hudson-ci.org).

### <a name="liferay"></a>Liferay
App Service Web Apps Liferay támogatott. Liferay jelentős memória lehet szükség, mert a webalkalmazás kell futnia egy közepes vagy nagyméretű speciális feldolgozó, amely elegendő memóriával biztosít. Liferay is elindításához több percet vesz igénybe. Ezért javasoljuk, hogy beállította a web app **mindig a**.  

Tomcat Liferay Community Edition GA3 kötegelt 6.1.2 használ, a következő fájlok volt módosítani Liferay letöltése után:

**Server.XML**

* Leállítás portjának módosítása – 1.
* Módosítsa a HTTP-összekötő`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`
* A AJP összekötő megjegyzéssé.

Az a **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** mappa, hozzon létre egy fájlt **portal-ext.properties**. Ez a fájl tartalmaznia kell egy sorban, ahogy az itt látható:

    liferay.home=%HOME%/site/wwwroot/liferay

Ugyanazon a szinten directory a tomcat-7.0.40 mappaként, hozzon létre egy fájlt **web.config** a következő tartalommal:

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

Az a **httpPlatform** blokkot, a **requestTimeout** értéke "00: 10:00".  Akkor csökkenteni lehet, de majd valószínűleg néhány időtúllépés hibába ütközik, miközben Liferay rendszerindítása van.  Ha ez az érték megváltozik, akkor a **connectionTimeout** a tomcat a server.xml is módosítani kell.  

Érdemes megjegyezni, hogy a JRE_HOME környezetben varariable van megadva a fenti Web.config úgy, hogy a 64 bites JDK mutasson. Az alapértelmezett érték a 32 bites, de Liferay memória magas szintű igényelhetnek, mivel a 64 bites JDK használata javasolt.

Miután a módosítások, indítsa újra a webes alkalmazás Liferay fut, majd, nyissa meg a http://yourwebapp. A Liferay portál webes alkalmazás gyökérkönyvtárában érhető el. 

## <a name="next-steps"></a>Következő lépések
Liferay kapcsolatos további információkért lásd: [http://www.liferay.com](http://www.liferay.com).

További információt a Java [Azure Java-fejlesztőknek](/java/azure).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
