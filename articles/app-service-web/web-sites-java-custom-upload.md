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
# <a name="upload-a-custom-java-web-app-tooazure"></a><span data-ttu-id="4084d-103">Olyan egyéni Java web app tooAzure feltöltése</span><span class="sxs-lookup"><span data-stu-id="4084d-103">Upload a custom Java web app tooAzure</span></span>
<span data-ttu-id="4084d-104">Ez a témakör azt ismerteti, hogyan a tooupload olyan egyéni Java web app túl[Azure App Service] webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="4084d-104">This topic explains how tooupload a custom Java web app too[Azure App Service] Web Apps.</span></span> <span data-ttu-id="4084d-105">Tartalmazza az tooany Java webhelyen vagy a web app és az adott alkalmazásokra vonatkozó példákat is érvényes információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="4084d-105">Included is information that applies tooany Java website or web app, and also some examples for specific applications.</span></span>

<span data-ttu-id="4084d-106">Vegye figyelembe, hogy Azure arra szolgál, hogy hello Azure Portal konfigurációs UI használó Java-webalkalmazások létrehozása, és hello Azure piactér következő dokumentált [Java-webalkalmazás létrehozása az Azure App Service](web-sites-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4084d-106">Note that Azure provides a means for creating Java web apps using hello Azure Portal's configuration UI, and hello Azure Marketplace, as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md).</span></span> <span data-ttu-id="4084d-107">Ez az oktatóanyag forgatókönyvek, amelyben nem szeretné, hogy toouse hello Azure Portal konfigurációs UI vagy hello Azure piactér van.</span><span class="sxs-lookup"><span data-stu-id="4084d-107">This tutorial is for scenarios in which you do not want toouse hello Azure Portal configuration UI or hello Azure Marketplace.</span></span>  

## <a name="configuration-guidelines"></a><span data-ttu-id="4084d-108">Beállítási útmutatója</span><span class="sxs-lookup"><span data-stu-id="4084d-108">Configuration guidelines</span></span>
<span data-ttu-id="4084d-109">hello következő várt, de egyéni Azure Java-webalkalmazások hello-beállításokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4084d-109">hello following describes hello settings expected for custom Java web apps on Azure.</span></span>

* <span data-ttu-id="4084d-110">hello Java-folyamat által használt hello HTTP-port dinamikusan hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="4084d-110">hello HTTP port used by hello Java process is dynamically assigned.</span></span>  <span data-ttu-id="4084d-111">hello folyamat hello portjával hello környezeti változót kell használnia `HTTP_PLATFORM_PORT`.</span><span class="sxs-lookup"><span data-stu-id="4084d-111">hello process must use hello port from hello environment variable `HTTP_PLATFORM_PORT`.</span></span>
* <span data-ttu-id="4084d-112">Az összes figyelési portokat eltérő hello egyetlen HTTP-figyelő le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="4084d-112">All listen ports other than hello single HTTP listener should be disabled.</span></span>  <span data-ttu-id="4084d-113">A Tomcat is tartalmazó hello leállítás, HTTPS és AJP portok.</span><span class="sxs-lookup"><span data-stu-id="4084d-113">In Tomcat, that includes hello shutdown, HTTPS, and AJP ports.</span></span>
* <span data-ttu-id="4084d-114">hello tároló toobe csak IPv4-forgalom konfigurálni kell.</span><span class="sxs-lookup"><span data-stu-id="4084d-114">hello container needs toobe configured for IPv4 traffic only.</span></span>
* <span data-ttu-id="4084d-115">Hello **indítási** parancsot a hello alkalmazásnak kell toobe hello konfiguráció beállítása.</span><span class="sxs-lookup"><span data-stu-id="4084d-115">hello **startup** command for hello application needs toobe set in hello configuration.</span></span>
* <span data-ttu-id="4084d-116">Mappák írási engedélye igénylő alkalmazások kell hello Azure web apphoz tartalom könyvtárban, amely toobe **D:\home**.</span><span class="sxs-lookup"><span data-stu-id="4084d-116">Applications that require directories with write permission need toobe located in hello Azure web app's content directory,  which is **D:\home**.</span></span>  <span data-ttu-id="4084d-117">hello környezeti változó `HOME` tooD:\home hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="4084d-117">hello environmental variable `HOME` refers tooD:\home.</span></span>  

<span data-ttu-id="4084d-118">Szükség szerint hello web.config fájlt a környezeti változók állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="4084d-118">You can set environment variables as required in hello web.config file.</span></span>

## <a name="webconfig-httpplatform-configuration"></a><span data-ttu-id="4084d-119">Web.config httpPlatform konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4084d-119">web.config httpPlatform configuration</span></span>
<span data-ttu-id="4084d-120">hello következő ismerteti hello **httpPlatform** formátumban web.config belül.</span><span class="sxs-lookup"><span data-stu-id="4084d-120">hello following information describes hello **httpPlatform** format within web.config.</span></span>

<span data-ttu-id="4084d-121">**argumentumok** (alapértelmezett = "").</span><span class="sxs-lookup"><span data-stu-id="4084d-121">**arguments** (Default="").</span></span> <span data-ttu-id="4084d-122">Argumentumok toohello végrehajtható fájl vagy parancsfájl hello megadott **processPath** beállítást.</span><span class="sxs-lookup"><span data-stu-id="4084d-122">Arguments toohello executable or script specified in hello **processPath** setting.</span></span>

<span data-ttu-id="4084d-123">Példák (megjelenítve **processPath** része):</span><span class="sxs-lookup"><span data-stu-id="4084d-123">Examples (shown with **processPath** included):</span></span>

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


<span data-ttu-id="4084d-124">**processPath** -elérési út toohello végrehajtható vagy parancsfájlt, amely figyeli a HTTP-kérelmek folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="4084d-124">**processPath** - Path toohello executable or script that will launch a process listening for HTTP requests.</span></span>

<span data-ttu-id="4084d-125">Példák:</span><span class="sxs-lookup"><span data-stu-id="4084d-125">Examples:</span></span>

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

<span data-ttu-id="4084d-126">**rapidFailsPerMinute** (alapértelmezett = 10.) Hány alkalommal megadott hello folyamat **processPath** percenként toocrash engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="4084d-126">**rapidFailsPerMinute** (Default=10.) Number of times hello process specified in **processPath** is allowed toocrash per minute.</span></span> <span data-ttu-id="4084d-127">Ha túllépi ezt a határt, **HttpPlatformHandler** hello perc hello hátralévő hello folyamatának indítási állapotára leáll.</span><span class="sxs-lookup"><span data-stu-id="4084d-127">If this limit is exceeded, **HttpPlatformHandler** will stop launching hello process for hello remainder of hello minute.</span></span>

<span data-ttu-id="4084d-128">**requestTimeout** (alapértelmezett = "00: 02:00".) Időtartam, amelynek **HttpPlatformHandler** figyel hello folyamat válaszára várakozik `%HTTP_PLATFORM_PORT%`.</span><span class="sxs-lookup"><span data-stu-id="4084d-128">**requestTimeout** (Default="00:02:00".) Duration for which **HttpPlatformHandler** will wait for a response from hello process listening on `%HTTP_PLATFORM_PORT%`.</span></span>

<span data-ttu-id="4084d-129">**startupRetryCount** (alapértelmezett = 10.) Ennyiszer **HttpPlatformHandler** megpróbál toolaunch hello folyamat megadott **processPath**.</span><span class="sxs-lookup"><span data-stu-id="4084d-129">**startupRetryCount** (Default=10.) Number of times **HttpPlatformHandler** will try toolaunch hello process specified in **processPath**.</span></span> <span data-ttu-id="4084d-130">Lásd: **startupTimeLimit** további részleteket.</span><span class="sxs-lookup"><span data-stu-id="4084d-130">See **startupTimeLimit** for more details.</span></span>

<span data-ttu-id="4084d-131">**startupTimeLimit** (alapértelmezett = 10 másodperc.) Időtartam, amelynek **HttpPlatformHandler** várakozik hello végrehajtható fájl, parancsfájl toostart egy folyamat hello portot figyel.</span><span class="sxs-lookup"><span data-stu-id="4084d-131">**startupTimeLimit** (Default=10 seconds.) Duration for which **HttpPlatformHandler** will wait for hello executable/script toostart a process listening on hello port.</span></span>  <span data-ttu-id="4084d-132">Ha az időkorlát túl lett lépve, **HttpPlatformHandler** fog hello folyamat leállítása és toolaunch próbálja újból **startupRetryCount** alkalommal.</span><span class="sxs-lookup"><span data-stu-id="4084d-132">If this time limit is exceeded, **HttpPlatformHandler** will kill hello process and try toolaunch it again **startupRetryCount** times.</span></span>

<span data-ttu-id="4084d-133">**stdoutLogEnabled** (alapértelmezett = "true".) Igaz értéke esetén **stdout** és **stderr** hello megadott hello folyamat **processPath** beállítás lesz a átirányított toohello fájl  **stdoutLogFile** (lásd: **stdoutLogFile** szakaszban).</span><span class="sxs-lookup"><span data-stu-id="4084d-133">**stdoutLogEnabled** (Default="true".) If true, **stdout** and **stderr** for hello process specified in hello **processPath** setting will be redirected toohello file specified in **stdoutLogFile** (see **stdoutLogFile** section).</span></span>

<span data-ttu-id="4084d-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Fájl abszolút elérési útja, amelyhez **stdout** és **stderr** hello folyamat során megadott **processPath** bekerülnek a naplóba.</span><span class="sxs-lookup"><span data-stu-id="4084d-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolute file path for which **stdout** and **stderr** from hello process specified in **processPath** will be logged.</span></span>

> [!NOTE]
> <span data-ttu-id="4084d-135">`%HTTP_PLATFORM_PORT%`amely toospecified részeként kell különleges helyőrző **argumentumok** vagy hello részeként **httpPlatform** **environmentVariables** listája.</span><span class="sxs-lookup"><span data-stu-id="4084d-135">`%HTTP_PLATFORM_PORT%` is a special placeholder which needs toospecified either as part of **arguments** or as part of hello **httpPlatform** **environmentVariables** list.</span></span> <span data-ttu-id="4084d-136">Ez egy által belsőleg generált port váltják **HttpPlatformHandler** , hogy a megadott hello folyamat **processPath** figyelheti a ezt a portot.</span><span class="sxs-lookup"><span data-stu-id="4084d-136">This will be replaced by an internally generated port by **HttpPlatformHandler** so that hello process specified by **processPath** can listen on this port.</span></span>
> 
> 

## <a name="deployment"></a><span data-ttu-id="4084d-137">Környezet</span><span class="sxs-lookup"><span data-stu-id="4084d-137">Deployment</span></span>
<span data-ttu-id="4084d-138">Java-alapú webalkalmazások könnyen hello azonos azt jelenti, hogy a legtöbb hello-alapú Internet Information Services (IIS) webes alkalmazások használt keresztül is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="4084d-138">Java based web apps can be deployed easily through most of hello same means that are used with hello Internet Information Services (IIS) based web applications.</span></span>  <span data-ttu-id="4084d-139">FTP, Git és a Kudu használata az összes támogatott mechanizmusok, mint van a web Apps integrált SCM funkció hello.</span><span class="sxs-lookup"><span data-stu-id="4084d-139">FTP, Git and Kudu are all supported as deployment mechanisms, as is hello integrated SCM capability for web apps.</span></span> <span data-ttu-id="4084d-140">WebDeploy works protokollként, azonban Java nem kidolgozott a Visual Studio WebDeploy nem felelnek meg a Java web app telepítési használati eseteket.</span><span class="sxs-lookup"><span data-stu-id="4084d-140">WebDeploy works as a protocol, however, as Java is not developed in Visual Studio, WebDeploy does not fit with Java web app deployment use cases.</span></span>

## <a name="application-configuration-examples"></a><span data-ttu-id="4084d-141">Alkalmazáskonfiguráció példák</span><span class="sxs-lookup"><span data-stu-id="4084d-141">Application configuration Examples</span></span>
<span data-ttu-id="4084d-142">A következő alkalmazások, a web.config fájl- és hello hello Alkalmazáskonfiguráció biztosítja példák tooshow, hogyan tooenable a Java-alkalmazás az App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="4084d-142">For hello following applications, a web.config file and hello application configuration is provided as examples tooshow how tooenable your Java application on App Service Web Apps.</span></span>

### <a name="tomcat"></a><span data-ttu-id="4084d-143">Tomcat</span><span class="sxs-lookup"><span data-stu-id="4084d-143">Tomcat</span></span>
<span data-ttu-id="4084d-144">Nincsenek App Service Web Apps végzik a Tomcat két változata, de napjainkban továbbra is meglehetősen lehetséges tooupload ügyfél olyan specifikus példányai.</span><span class="sxs-lookup"><span data-stu-id="4084d-144">While there are two variations on Tomcat that are supplied with App Service Web Apps, it is still quite possible tooupload customer specific instances.</span></span> <span data-ttu-id="4084d-145">Alább példája egy másik Java virtuális gép (JVM) a Tomcat telepítését.</span><span class="sxs-lookup"><span data-stu-id="4084d-145">Below is an example of an install of Tomcat with a different Java Virtual Machine (JVM).</span></span>

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

<span data-ttu-id="4084d-146">A Tomcat ügyféloldali hello, néhány konfigurációs módosítások vannak végrehajtott toobe igénylő.</span><span class="sxs-lookup"><span data-stu-id="4084d-146">On hello Tomcat side, there are a few configuration changes that need toobe made.</span></span> <span data-ttu-id="4084d-147">hello server.xml kell szerkeszteni toobe tooset:</span><span class="sxs-lookup"><span data-stu-id="4084d-147">hello server.xml needs toobe edited tooset:</span></span>

* <span data-ttu-id="4084d-148">Leállítás port = -1</span><span class="sxs-lookup"><span data-stu-id="4084d-148">Shutdown port = -1</span></span>
* <span data-ttu-id="4084d-149">HTTP-összekötő port = {port.http} $</span><span class="sxs-lookup"><span data-stu-id="4084d-149">HTTP connector port = ${port.http}</span></span>
* <span data-ttu-id="4084d-150">HTTP-összekötő cím = "127.0.0.1"</span><span class="sxs-lookup"><span data-stu-id="4084d-150">HTTP connector address = "127.0.0.1"</span></span>
* <span data-ttu-id="4084d-151">HTTPS- és AJP összekötők megjegyzéssé</span><span class="sxs-lookup"><span data-stu-id="4084d-151">Comment out HTTPS and AJP connectors</span></span>
* <span data-ttu-id="4084d-152">hello IPv4 beállítás is megadható a hello catalina.properties fájlban adhat hozzá`java.net.preferIPv4Stack=true`</span><span class="sxs-lookup"><span data-stu-id="4084d-152">hello IPv4 setting can also be set in hello catalina.properties file where you can add     `java.net.preferIPv4Stack=true`</span></span>

<span data-ttu-id="4084d-153">App Service Web Apps Direct3d hívások nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="4084d-153">Direct3d calls are not supported on App Service Web Apps.</span></span> <span data-ttu-id="4084d-154">toodisable, adja hozzá a következő Java beállítás kell az alkalmazás hívásokat ilyen hello:`-Dsun.java2d.d3d=false`</span><span class="sxs-lookup"><span data-stu-id="4084d-154">toodisable those, add hello following Java option should your application make such calls: `-Dsun.java2d.d3d=false`</span></span>

### <a name="jetty"></a><span data-ttu-id="4084d-155">Jetty</span><span class="sxs-lookup"><span data-stu-id="4084d-155">Jetty</span></span>
<span data-ttu-id="4084d-156">Hello helyzet a Tomcat, mivel az ügyfelek feltöltheti saját példányok a Jetty.</span><span class="sxs-lookup"><span data-stu-id="4084d-156">As is hello case for Tomcat, customers can upload their own instances for Jetty.</span></span> <span data-ttu-id="4084d-157">Futó hello teljes telepítése Jetty hello esetben hello konfigurációs néz ki:</span><span class="sxs-lookup"><span data-stu-id="4084d-157">In hello case of running hello full install of Jetty, hello configuration would look like this:</span></span>

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

<span data-ttu-id="4084d-158">hello Jetty beállításokat kell hello start.ini tooset módosított toobe `java.net.preferIPv4Stack=true`.</span><span class="sxs-lookup"><span data-stu-id="4084d-158">hello Jetty configuration needs toobe changed in hello start.ini tooset `java.net.preferIPv4Stack=true`.</span></span>

### <a name="springboot"></a><span data-ttu-id="4084d-159">Springboot</span><span class="sxs-lookup"><span data-stu-id="4084d-159">Springboot</span></span>
<span data-ttu-id="4084d-160">A sorrend tooget egy Springboot alkalmazást futtató tooupload a JAR vagy WAR-fájl szükséges, és adja hozzá a következő web.config fájl hello.</span><span class="sxs-lookup"><span data-stu-id="4084d-160">In order tooget a Springboot application running you need tooupload your JAR or WAR file and add hello following web.config file.</span></span> <span data-ttu-id="4084d-161">hello web.config fájl hello wwwroot mappába kerül.</span><span class="sxs-lookup"><span data-stu-id="4084d-161">hello web.config file goes into hello wwwroot folder.</span></span> <span data-ttu-id="4084d-162">A hello web.config módosítása hello argumentumok toopoint tooyour JAR-fájlra, a hello példa hello JAR-fájlt a következő mappában található hello wwwroot is.</span><span class="sxs-lookup"><span data-stu-id="4084d-162">In hello web.config adjust hello arguments toopoint tooyour JAR file, in hello following example hello JAR file is located in hello wwwroot folder as well.</span></span>  

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


### <a name="hudson"></a><span data-ttu-id="4084d-163">Hudson</span><span class="sxs-lookup"><span data-stu-id="4084d-163">Hudson</span></span>
<span data-ttu-id="4084d-164">A tesztben használt Hudson 3.1.2 war hello és hello alapértelmezett Tomcat 7.0.50 példány, de hello felhasználói felület tooset művelet használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="4084d-164">Our test used hello Hudson 3.1.2 war and hello default Tomcat 7.0.50 instance but without using hello UI tooset things up.</span></span>  <span data-ttu-id="4084d-165">Mivel Hudson build szoftvereszköz, elkeverni tooinstall is azt a dedikált példányokat, ahol hello **AlwaysOn** jelző hello webalkalmazás állítható be.</span><span class="sxs-lookup"><span data-stu-id="4084d-165">Because Hudson is a software build tool, it is advised tooinstall it on dedicated instances where hello **AlwaysOn** flag can be set on hello web app.</span></span>

1. <span data-ttu-id="4084d-166">A webalkalmazás a gyökérkönyvtár, azaz **d:\home\site\wwwroot**, hozzon létre egy **webapps** könyvtár (Ha még nincs letöltve), és helyezze a Hudson.war **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="4084d-166">In your web app’s root directory, i.e., **d:\home\site\wwwroot**, create a **webapps** directory (if not already present), and place Hudson.war in **d:\home\site\wwwroot\webapps**.</span></span>
2. <span data-ttu-id="4084d-167">Töltse le az apache maven 3.0.5 (kompatibilis Hudson), és helyezze el a **d:\home\site\wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="4084d-167">Download apache maven 3.0.5 (compatible with Hudson) and place it in **d:\home\site\wwwroot**.</span></span>
3. <span data-ttu-id="4084d-168">A web.config létrehozása **d:\home\site\wwwroot** és a Beillesztés hello tartalmát, a következő:</span><span class="sxs-lookup"><span data-stu-id="4084d-168">Create web.config in **d:\home\site\wwwroot** and paste hello following contents in it:</span></span>
   
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
   
    <span data-ttu-id="4084d-169">Ezen a ponton hello webalkalmazás lehet indítani tootake hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="4084d-169">At this point hello web app can be restarted tootake hello changes.</span></span>  <span data-ttu-id="4084d-170">Csatlakozás toohttp://yourwebapp/hudson toostart Hudson.</span><span class="sxs-lookup"><span data-stu-id="4084d-170">Connect toohttp://yourwebapp/hudson toostart Hudson.</span></span>
4. <span data-ttu-id="4084d-171">Miután Hudson konfigurálja magát, a következő képernyő hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4084d-171">After Hudson configures itself, you should see hello following screen:</span></span>
   
    ![Hudson](./media/web-sites-java-custom-upload/hudson1.png)
5. <span data-ttu-id="4084d-173">Hozzáférés hello Hudson konfigurálólapját: kattintson a **kezelése Hudson**, és kattintson a **rendszer konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="4084d-173">Access hello Hudson configuration page: Click **Manage Hudson**, and then click **Configure System**.</span></span>
6. <span data-ttu-id="4084d-174">Alább látható módon JDK hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="4084d-174">Configure hello JDK as shown below:</span></span>
   
    ![Hudson konfiguráció](./media/web-sites-java-custom-upload/hudson2.png)
7. <span data-ttu-id="4084d-176">Maven adja meg alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="4084d-176">Configure Maven as shown below:</span></span>
   
    ![Maven-konfiguráció](./media/web-sites-java-custom-upload/maven.png)
8. <span data-ttu-id="4084d-178">Hello beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4084d-178">Save hello settings.</span></span> <span data-ttu-id="4084d-179">Hudson kell konfigurált és használatra kész.</span><span class="sxs-lookup"><span data-stu-id="4084d-179">Hudson should now be configured and ready for use.</span></span>

<span data-ttu-id="4084d-180">A Hudson további információkért lásd: [http://hudson-ci.org](http://hudson-ci.org).</span><span class="sxs-lookup"><span data-stu-id="4084d-180">For additional information on Hudson, see [http://hudson-ci.org](http://hudson-ci.org).</span></span>

### <a name="liferay"></a><span data-ttu-id="4084d-181">Liferay</span><span class="sxs-lookup"><span data-stu-id="4084d-181">Liferay</span></span>
<span data-ttu-id="4084d-182">App Service Web Apps Liferay támogatott.</span><span class="sxs-lookup"><span data-stu-id="4084d-182">Liferay is supported on App Service Web Apps.</span></span> <span data-ttu-id="4084d-183">Liferay jelentős memória lehet szükség, mert egy közepes vagy nagyméretű speciális feldolgozó, amely elegendő memóriával biztosít a toorun kell hello webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4084d-183">Since Liferay can require significant memory, hello web app needs toorun on a medium or large dedicated worker, which can provide enough memory.</span></span> <span data-ttu-id="4084d-184">Liferay több percig toostart is foglalja el.</span><span class="sxs-lookup"><span data-stu-id="4084d-184">Liferay also takes several minutes toostart up.</span></span> <span data-ttu-id="4084d-185">Ezért ajánlott hello webalkalmazás túl beállítása**mindig a**.</span><span class="sxs-lookup"><span data-stu-id="4084d-185">For that reason, it is recommended that you set hello web app too**Always On**.</span></span>  

<span data-ttu-id="4084d-186">Tomcat Liferay Community Edition GA3 kötegelt 6.1.2 használ, hello következő fájlok volt módosítani Liferay letöltése után:</span><span class="sxs-lookup"><span data-stu-id="4084d-186">Using Liferay 6.1.2 Community Edition GA3 bundled with Tomcat, hello following files were edited after downloading Liferay:</span></span>

<span data-ttu-id="4084d-187">**Server.XML**</span><span class="sxs-lookup"><span data-stu-id="4084d-187">**Server.xml**</span></span>

* <span data-ttu-id="4084d-188">Módosítsa a leállítási port túl-1.</span><span class="sxs-lookup"><span data-stu-id="4084d-188">Change Shutdown port too-1.</span></span>
* <span data-ttu-id="4084d-189">HTTP-összekötő túl módosítása`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span><span class="sxs-lookup"><span data-stu-id="4084d-189">Change HTTP connector too       `<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span></span>
* <span data-ttu-id="4084d-190">Hello AJP összekötő megjegyzéssé.</span><span class="sxs-lookup"><span data-stu-id="4084d-190">Comment out hello AJP connector.</span></span>

<span data-ttu-id="4084d-191">A hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** mappa, hozzon létre egy fájlt **portal-ext.properties**.</span><span class="sxs-lookup"><span data-stu-id="4084d-191">In hello **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** folder, create a file named **portal-ext.properties**.</span></span> <span data-ttu-id="4084d-192">A fájlnak kell toocontain egy sorban, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="4084d-192">This file needs toocontain one line, as shown here:</span></span>

    liferay.home=%HOME%/site/wwwroot/liferay

<span data-ttu-id="4084d-193">Hello: hello tomcat-7.0.40 mappa, azonos könyvtár szinten hozzon létre egy fájlt **web.config** a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="4084d-193">At hello same directory level as hello tomcat-7.0.40 folder, create a file named **web.config** with hello following content:</span></span>

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

<span data-ttu-id="4084d-194">A hello **httpPlatform** blokkolja, hello **requestTimeout** túl értéke "00: 10:00".</span><span class="sxs-lookup"><span data-stu-id="4084d-194">Under hello **httpPlatform** block, hello **requestTimeout** is set too“00:10:00”.</span></span>  <span data-ttu-id="4084d-195">Akkor csökkenteni lehet, de esetén a rendszer valószínűleg toosee során időtúllépési hibák Liferay rendszerindítása van.</span><span class="sxs-lookup"><span data-stu-id="4084d-195">It can be reduced but then you are likely toosee some timeout errors while Liferay is bootstrapping.</span></span>  <span data-ttu-id="4084d-196">Ha ez az érték megváltozik, majd hello **connectionTimeout** a hello tomcat server.xml is módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="4084d-196">If this value is changed, then hello **connectionTimeout** in hello tomcat server.xml should also be modified.</span></span>  

<span data-ttu-id="4084d-197">Érdemes megjegyezni, hogy hello JRE_HOME környezetben varariable hello web.config toopoint toohello fent megadott 64 bites JDK.</span><span class="sxs-lookup"><span data-stu-id="4084d-197">It is worth noting that hello JRE_HOME environnment varariable is specified in hello above web.config toopoint toohello 64-bit JDK.</span></span> <span data-ttu-id="4084d-198">hello alapértelmezett érték a 32 bites, de Liferay memória magas szintű igényelhetnek, mivel toouse javasolt 64 bites JDK hello.</span><span class="sxs-lookup"><span data-stu-id="4084d-198">hello default is 32-bit, but since Liferay may require high levels of memory, it is recommended toouse hello 64-bit JDK.</span></span>

<span data-ttu-id="4084d-199">Miután a módosítások, indítsa újra a webes alkalmazás Liferay fut, majd, nyissa meg a http://yourwebapp.</span><span class="sxs-lookup"><span data-stu-id="4084d-199">Once you make these changes, restart your web app running Liferay, Then, open http://yourwebapp.</span></span> <span data-ttu-id="4084d-200">hello Liferay portal hello web app legfelső szintű érhető el.</span><span class="sxs-lookup"><span data-stu-id="4084d-200">hello Liferay portal is available from hello web app root.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4084d-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4084d-201">Next steps</span></span>
<span data-ttu-id="4084d-202">Liferay kapcsolatos további információkért lásd: [http://www.liferay.com](http://www.liferay.com).</span><span class="sxs-lookup"><span data-stu-id="4084d-202">For more information about Liferay, see [http://www.liferay.com](http://www.liferay.com).</span></span>

<span data-ttu-id="4084d-203">További információt a Java [Azure Java-fejlesztőknek](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="4084d-203">For more information about Java, visit [Azure for Java developers](/java/azure).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
