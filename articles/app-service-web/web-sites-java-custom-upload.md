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
# <a name="upload-a-custom-java-web-app-to-azure"></a><span data-ttu-id="b7d5b-103">Upload a custom Java web app to Azure (Egyéni Java-webalkalmazás feltöltése az Azure-ba)</span><span class="sxs-lookup"><span data-stu-id="b7d5b-103">Upload a custom Java web app to Azure</span></span>
<span data-ttu-id="b7d5b-104">Ez a témakör azt ismerteti, hogyan egyéni Java-webalkalmazás való feltöltéséhez [Azure App Service] webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-104">This topic explains how to upload a custom Java web app to [Azure App Service] Web Apps.</span></span> <span data-ttu-id="b7d5b-105">Szereplő információkat a Java webhelyre vagy webalkalmazásra alkalmazás, és adott alkalmazásokra vonatkozó példákat is van.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-105">Included is information that applies to any Java website or web app, and also some examples for specific applications.</span></span>

<span data-ttu-id="b7d5b-106">Vegye figyelembe, hogy Azure arra szolgál, hogy az Azure portál konfiguráció felhasználói felületi, és az Azure piactér Java-webalkalmazások létrehozása a dokumentált [Java-webalkalmazás létrehozása az Azure App Service](web-sites-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b7d5b-106">Note that Azure provides a means for creating Java web apps using the Azure Portal's configuration UI, and the Azure Marketplace, as documented at [Create a Java web app in Azure App Service](web-sites-java-get-started.md).</span></span> <span data-ttu-id="b7d5b-107">Ez az oktatóanyag, amelyben nem szeretné használni az Azure portál konfigurációs forgatókönyvek felhasználói felületén vagy az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-107">This tutorial is for scenarios in which you do not want to use the Azure Portal configuration UI or the Azure Marketplace.</span></span>  

## <a name="configuration-guidelines"></a><span data-ttu-id="b7d5b-108">Beállítási útmutatója</span><span class="sxs-lookup"><span data-stu-id="b7d5b-108">Configuration guidelines</span></span>
<span data-ttu-id="b7d5b-109">A következő egyéni Azure Java-webalkalmazások vár-beállításokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-109">The following describes the settings expected for custom Java web apps on Azure.</span></span>

* <span data-ttu-id="b7d5b-110">A Java-folyamat által használt HTTP-port dinamikusan hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-110">The HTTP port used by the Java process is dynamically assigned.</span></span>  <span data-ttu-id="b7d5b-111">A folyamat kell használni a környezeti változó portjával `HTTP_PLATFORM_PORT`.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-111">The process must use the port from the environment variable `HTTP_PLATFORM_PORT`.</span></span>
* <span data-ttu-id="b7d5b-112">Az egyetlen HTTP-figyelő eltérő összes figyelési port le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-112">All listen ports other than the single HTTP listener should be disabled.</span></span>  <span data-ttu-id="b7d5b-113">A Tomcat, amely tartalmazza a Leállítás, HTTPS és AJP portok.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-113">In Tomcat, that includes the shutdown, HTTPS, and AJP ports.</span></span>
* <span data-ttu-id="b7d5b-114">A tároló kell megadni a csak IPv4-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-114">The container needs to be configured for IPv4 traffic only.</span></span>
* <span data-ttu-id="b7d5b-115">A **indítási** az alkalmazást kell a konfigurációban állítható be a parancsot.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-115">The **startup** command for the application needs to be set in the configuration.</span></span>
* <span data-ttu-id="b7d5b-116">Mappák írási engedélye igénylő alkalmazásokat kell könyvtárban az Azure web app tartalom, amely **D:\home**.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-116">Applications that require directories with write permission need to be located in the Azure web app's content directory,  which is **D:\home**.</span></span>  <span data-ttu-id="b7d5b-117">A környezeti változó `HOME` D:\home hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-117">The environmental variable `HOME` refers to D:\home.</span></span>  

<span data-ttu-id="b7d5b-118">Szükség szerint a web.config fájlt a környezeti változók állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-118">You can set environment variables as required in the web.config file.</span></span>

## <a name="webconfig-httpplatform-configuration"></a><span data-ttu-id="b7d5b-119">Web.config httpPlatform konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b7d5b-119">web.config httpPlatform configuration</span></span>
<span data-ttu-id="b7d5b-120">Az alábbiakban ismertetett a **httpPlatform** formátumban web.config belül.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-120">The following information describes the **httpPlatform** format within web.config.</span></span>

<span data-ttu-id="b7d5b-121">**argumentumok** (alapértelmezett = "").</span><span class="sxs-lookup"><span data-stu-id="b7d5b-121">**arguments** (Default="").</span></span> <span data-ttu-id="b7d5b-122">A végrehajtható fájl vagy parancsfájl a megadott argumentumok a **processPath** beállítást.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-122">Arguments to the executable or script specified in the **processPath** setting.</span></span>

<span data-ttu-id="b7d5b-123">Példák (megjelenítve **processPath** része):</span><span class="sxs-lookup"><span data-stu-id="b7d5b-123">Examples (shown with **processPath** included):</span></span>

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"

    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


<span data-ttu-id="b7d5b-124">**processPath** -elérési utat a végrehajtható fájlt vagy parancsfájlt, amely figyeli a HTTP-kérelmek folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-124">**processPath** - Path to the executable or script that will launch a process listening for HTTP requests.</span></span>

<span data-ttu-id="b7d5b-125">Példák:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-125">Examples:</span></span>

    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"

<span data-ttu-id="b7d5b-126">**rapidFailsPerMinute** (alapértelmezett = 10.) A folyamat a megadott számú **processPath** percenként összeomlási engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-126">**rapidFailsPerMinute** (Default=10.) Number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b7d5b-127">Ha túllépi ezt a határt, **HttpPlatformHandler** leáll, a perc hátralevő folyamatának indítási állapotára.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-127">If this limit is exceeded, **HttpPlatformHandler** will stop launching the process for the remainder of the minute.</span></span>

<span data-ttu-id="b7d5b-128">**requestTimeout** (alapértelmezett = "00: 02:00".) Időtartam, amelynek **HttpPlatformHandler** a figyelést a következő folyamat válaszára várakozik `%HTTP_PLATFORM_PORT%`.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-128">**requestTimeout** (Default="00:02:00".) Duration for which **HttpPlatformHandler** will wait for a response from the process listening on `%HTTP_PLATFORM_PORT%`.</span></span>

<span data-ttu-id="b7d5b-129">**startupRetryCount** (alapértelmezett = 10.) Ennyiszer **HttpPlatformHandler** próbál meg elindítani a megadott **processPath**.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-129">**startupRetryCount** (Default=10.) Number of times **HttpPlatformHandler** will try to launch the process specified in **processPath**.</span></span> <span data-ttu-id="b7d5b-130">Lásd: **startupTimeLimit** további részleteket.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-130">See **startupTimeLimit** for more details.</span></span>

<span data-ttu-id="b7d5b-131">**startupTimeLimit** (alapértelmezett = 10 másodperc.) Időtartam, amelynek **HttpPlatformHandler** megvárja, hogy a végrehajtható fájl, a parancsfájl a porton figyel a folyamat elindításához.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-131">**startupTimeLimit** (Default=10 seconds.) Duration for which **HttpPlatformHandler** will wait for the executable/script to start a process listening on the port.</span></span>  <span data-ttu-id="b7d5b-132">Ha az időkorlát túl lett lépve, **HttpPlatformHandler** lesz a folyamat leállítása, és indítsa el újra megpróbálja **startupRetryCount** alkalommal.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-132">If this time limit is exceeded, **HttpPlatformHandler** will kill the process and try to launch it again **startupRetryCount** times.</span></span>

<span data-ttu-id="b7d5b-133">**stdoutLogEnabled** (alapértelmezett = "true".) Igaz értéke esetén **stdout** és **stderr** a megadott folyamat a **processPath** beállítás irányítja át a megadott fájl **stdoutLogFile** (lásd: **stdoutLogFile** szakaszban).</span><span class="sxs-lookup"><span data-stu-id="b7d5b-133">**stdoutLogEnabled** (Default="true".) If true, **stdout** and **stderr** for the process specified in the **processPath** setting will be redirected to the file specified in **stdoutLogFile** (see **stdoutLogFile** section).</span></span>

<span data-ttu-id="b7d5b-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Fájl abszolút elérési útja, amelyhez **stdout** és **stderr** a megadott folyamatának **processPath** bekerülnek a naplóba.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-134">**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log".) Absolute file path for which **stdout** and **stderr** from the process specified in **processPath** will be logged.</span></span>

> [!NOTE]
> <span data-ttu-id="b7d5b-135">`%HTTP_PLATFORM_PORT%`amelyet részeként megadott különleges helyőrző **argumentumok** vagy részeként a **httpPlatform** **environmentVariables** listája.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-135">`%HTTP_PLATFORM_PORT%` is a special placeholder which needs to specified either as part of **arguments** or as part of the **httpPlatform** **environmentVariables** list.</span></span> <span data-ttu-id="b7d5b-136">Ez egy által belsőleg generált port váltják **HttpPlatformHandler** , hogy a folyamat által megadott **processPath** figyelheti a ezt a portot.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-136">This will be replaced by an internally generated port by **HttpPlatformHandler** so that the process specified by **processPath** can listen on this port.</span></span>
> 
> 

## <a name="deployment"></a><span data-ttu-id="b7d5b-137">Környezet</span><span class="sxs-lookup"><span data-stu-id="b7d5b-137">Deployment</span></span>
<span data-ttu-id="b7d5b-138">Alapú webalkalmazások telepíthető egyszerűen a azonos azt jelenti, hogy az Internet Information Services (IIS) használata a legtöbb Java-alapú webes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-138">Java based web apps can be deployed easily through most of the same means that are used with the Internet Information Services (IIS) based web applications.</span></span>  <span data-ttu-id="b7d5b-139">FTP, Git és a Kudu használata az összes támogatott mechanizmusok, mint az integrált SCM képesség a web Apps.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-139">FTP, Git and Kudu are all supported as deployment mechanisms, as is the integrated SCM capability for web apps.</span></span> <span data-ttu-id="b7d5b-140">WebDeploy works protokollként, azonban Java nem kidolgozott a Visual Studio WebDeploy nem felelnek meg a Java web app telepítési használati eseteket.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-140">WebDeploy works as a protocol, however, as Java is not developed in Visual Studio, WebDeploy does not fit with Java web app deployment use cases.</span></span>

## <a name="application-configuration-examples"></a><span data-ttu-id="b7d5b-141">Alkalmazáskonfiguráció példák</span><span class="sxs-lookup"><span data-stu-id="b7d5b-141">Application configuration Examples</span></span>
<span data-ttu-id="b7d5b-142">A következő alkalmazások a web.config fájl-és az alkalmazás konfigurációs van példaként engedélyezése a Java-alkalmazás az App Service Web Apps megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-142">For the following applications, a web.config file and the application configuration is provided as examples to show how to enable your Java application on App Service Web Apps.</span></span>

### <a name="tomcat"></a><span data-ttu-id="b7d5b-143">Tomcat</span><span class="sxs-lookup"><span data-stu-id="b7d5b-143">Tomcat</span></span>
<span data-ttu-id="b7d5b-144">Miközben Tomcat két változata, amelyeket az App Service Web Apps, még meglehetősen lehetséges töltse fel az ügyfél olyan specifikus példányai.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-144">While there are two variations on Tomcat that are supplied with App Service Web Apps, it is still quite possible to upload customer specific instances.</span></span> <span data-ttu-id="b7d5b-145">Alább példája egy másik Java virtuális gép (JVM) a Tomcat telepítését.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-145">Below is an example of an install of Tomcat with a different Java Virtual Machine (JVM).</span></span>

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

<span data-ttu-id="b7d5b-146">A Tomcat oldalon van néhány konfigurációs módosításokat kell végezni.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-146">On the Tomcat side, there are a few configuration changes that need to be made.</span></span> <span data-ttu-id="b7d5b-147">A server.xml kell módosítani őket beállítani:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-147">The server.xml needs to be edited to set:</span></span>

* <span data-ttu-id="b7d5b-148">Leállítás port = -1</span><span class="sxs-lookup"><span data-stu-id="b7d5b-148">Shutdown port = -1</span></span>
* <span data-ttu-id="b7d5b-149">HTTP-összekötő port = {port.http} $</span><span class="sxs-lookup"><span data-stu-id="b7d5b-149">HTTP connector port = ${port.http}</span></span>
* <span data-ttu-id="b7d5b-150">HTTP-összekötő cím = "127.0.0.1"</span><span class="sxs-lookup"><span data-stu-id="b7d5b-150">HTTP connector address = "127.0.0.1"</span></span>
* <span data-ttu-id="b7d5b-151">HTTPS- és AJP összekötők megjegyzéssé</span><span class="sxs-lookup"><span data-stu-id="b7d5b-151">Comment out HTTPS and AJP connectors</span></span>
* <span data-ttu-id="b7d5b-152">Az IPv4-beállítás is megadható a catalina.properties fájlban adhat hozzá`java.net.preferIPv4Stack=true`</span><span class="sxs-lookup"><span data-stu-id="b7d5b-152">The IPv4 setting can also be set in the catalina.properties file where you can add     `java.net.preferIPv4Stack=true`</span></span>

<span data-ttu-id="b7d5b-153">App Service Web Apps Direct3d hívások nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-153">Direct3d calls are not supported on App Service Web Apps.</span></span> <span data-ttu-id="b7d5b-154">Tiltsa le azokat, adja hozzá a következő Java-beállítást kell az alkalmazás hívásokat ilyen:`-Dsun.java2d.d3d=false`</span><span class="sxs-lookup"><span data-stu-id="b7d5b-154">To disable those, add the following Java option should your application make such calls: `-Dsun.java2d.d3d=false`</span></span>

### <a name="jetty"></a><span data-ttu-id="b7d5b-155">Jetty</span><span class="sxs-lookup"><span data-stu-id="b7d5b-155">Jetty</span></span>
<span data-ttu-id="b7d5b-156">Mint esetében a Tomcat, az ügyfelek feltöltheti saját példányok a Jetty.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-156">As is the case for Tomcat, customers can upload their own instances for Jetty.</span></span> <span data-ttu-id="b7d5b-157">Esetén az Jetty teljes telepítését futtatja, a konfiguráció néz ki:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-157">In the case of running the full install of Jetty, the configuration would look like this:</span></span>

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

<span data-ttu-id="b7d5b-158">A Jetty beállításokat kell beállítani a start.ini módosíthatók `java.net.preferIPv4Stack=true`.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-158">The Jetty configuration needs to be changed in the start.ini to set `java.net.preferIPv4Stack=true`.</span></span>

### <a name="springboot"></a><span data-ttu-id="b7d5b-159">Springboot</span><span class="sxs-lookup"><span data-stu-id="b7d5b-159">Springboot</span></span>
<span data-ttu-id="b7d5b-160">Egy Springboot eléréséhez alkalmazást futtató kell a JAR vagy WAR-fájl feltöltése, és adja hozzá a következő web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-160">In order to get a Springboot application running you need to upload your JAR or WAR file and add the following web.config file.</span></span> <span data-ttu-id="b7d5b-161">A web.config fájlt a wwwroot mappába kerül.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-161">The web.config file goes into the wwwroot folder.</span></span> <span data-ttu-id="b7d5b-162">A Web.config fájlban, az alábbi példában a JAR-fájlra, valamint a wwwroot mappában található a JAR-fájlra mutasson az argumentumok módosítása</span><span class="sxs-lookup"><span data-stu-id="b7d5b-162">In the web.config adjust the arguments to point to your JAR file, in the following example the JAR file is located in the wwwroot folder as well.</span></span>  

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


### <a name="hudson"></a><span data-ttu-id="b7d5b-163">Hudson</span><span class="sxs-lookup"><span data-stu-id="b7d5b-163">Hudson</span></span>
<span data-ttu-id="b7d5b-164">A tesztben a Hudson 3.1.2 war és az alapértelmezett Tomcat 7.0.50 példányt használja, de a felhasználói felületen dolgot beállítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-164">Our test used the Hudson 3.1.2 war and the default Tomcat 7.0.50 instance but without using the UI to set things up.</span></span>  <span data-ttu-id="b7d5b-165">Mivel Hudson szoftver build eszköz, amely telepíti azt a dedikált példányok javasoljuk ahol a **AlwaysOn** jelző állítható be a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-165">Because Hudson is a software build tool, it is advised to install it on dedicated instances where the **AlwaysOn** flag can be set on the web app.</span></span>

1. <span data-ttu-id="b7d5b-166">A webalkalmazás a gyökérkönyvtár, azaz **d:\home\site\wwwroot**, hozzon létre egy **webapps** könyvtár (Ha még nincs letöltve), és helyezze a Hudson.war **d:\home\site\wwwroot\webapps**.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-166">In your web app’s root directory, i.e., **d:\home\site\wwwroot**, create a **webapps** directory (if not already present), and place Hudson.war in **d:\home\site\wwwroot\webapps**.</span></span>
2. <span data-ttu-id="b7d5b-167">Töltse le az apache maven 3.0.5 (kompatibilis Hudson), és helyezze el a **d:\home\site\wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-167">Download apache maven 3.0.5 (compatible with Hudson) and place it in **d:\home\site\wwwroot**.</span></span>
3. <span data-ttu-id="b7d5b-168">A web.config létrehozása **d:\home\site\wwwroot** és illessze be az alábbiakat:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-168">Create web.config in **d:\home\site\wwwroot** and paste the following contents in it:</span></span>
   
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
   
    <span data-ttu-id="b7d5b-169">Ezen a ponton a webes alkalmazás is újra kell indítani a módosítások érvénybe.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-169">At this point the web app can be restarted to take the changes.</span></span>  <span data-ttu-id="b7d5b-170">Csatlakozás http://yourwebapp/hudson Hudson elindításához.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-170">Connect to http://yourwebapp/hudson to start Hudson.</span></span>
4. <span data-ttu-id="b7d5b-171">Miután Hudson konfigurálja magát, a következő képernyő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-171">After Hudson configures itself, you should see the following screen:</span></span>
   
    ![Hudson](./media/web-sites-java-custom-upload/hudson1.png)
5. <span data-ttu-id="b7d5b-173">A Hudson lap eléréséhez: kattintson a **kezelése Hudson**, és kattintson a **rendszer konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-173">Access the Hudson configuration page: Click **Manage Hudson**, and then click **Configure System**.</span></span>
6. <span data-ttu-id="b7d5b-174">Adja meg a JDK alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-174">Configure the JDK as shown below:</span></span>
   
    ![Hudson konfiguráció](./media/web-sites-java-custom-upload/hudson2.png)
7. <span data-ttu-id="b7d5b-176">Maven adja meg alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-176">Configure Maven as shown below:</span></span>
   
    ![Maven-konfiguráció](./media/web-sites-java-custom-upload/maven.png)
8. <span data-ttu-id="b7d5b-178">A beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-178">Save the settings.</span></span> <span data-ttu-id="b7d5b-179">Hudson kell konfigurált és használatra kész.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-179">Hudson should now be configured and ready for use.</span></span>

<span data-ttu-id="b7d5b-180">A Hudson további információkért lásd: [http://hudson-ci.org](http://hudson-ci.org).</span><span class="sxs-lookup"><span data-stu-id="b7d5b-180">For additional information on Hudson, see [http://hudson-ci.org](http://hudson-ci.org).</span></span>

### <a name="liferay"></a><span data-ttu-id="b7d5b-181">Liferay</span><span class="sxs-lookup"><span data-stu-id="b7d5b-181">Liferay</span></span>
<span data-ttu-id="b7d5b-182">App Service Web Apps Liferay támogatott.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-182">Liferay is supported on App Service Web Apps.</span></span> <span data-ttu-id="b7d5b-183">Liferay jelentős memória lehet szükség, mert a webalkalmazás kell futnia egy közepes vagy nagyméretű speciális feldolgozó, amely elegendő memóriával biztosít.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-183">Since Liferay can require significant memory, the web app needs to run on a medium or large dedicated worker, which can provide enough memory.</span></span> <span data-ttu-id="b7d5b-184">Liferay is elindításához több percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-184">Liferay also takes several minutes to start up.</span></span> <span data-ttu-id="b7d5b-185">Ezért javasoljuk, hogy beállította a web app **mindig a**.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-185">For that reason, it is recommended that you set the web app to **Always On**.</span></span>  

<span data-ttu-id="b7d5b-186">Tomcat Liferay Community Edition GA3 kötegelt 6.1.2 használ, a következő fájlok volt módosítani Liferay letöltése után:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-186">Using Liferay 6.1.2 Community Edition GA3 bundled with Tomcat, the following files were edited after downloading Liferay:</span></span>

<span data-ttu-id="b7d5b-187">**Server.XML**</span><span class="sxs-lookup"><span data-stu-id="b7d5b-187">**Server.xml**</span></span>

* <span data-ttu-id="b7d5b-188">Leállítás portjának módosítása – 1.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-188">Change Shutdown port to -1.</span></span>
* <span data-ttu-id="b7d5b-189">Módosítsa a HTTP-összekötő`<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span><span class="sxs-lookup"><span data-stu-id="b7d5b-189">Change HTTP connector to       `<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`</span></span>
* <span data-ttu-id="b7d5b-190">A AJP összekötő megjegyzéssé.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-190">Comment out the AJP connector.</span></span>

<span data-ttu-id="b7d5b-191">Az a **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** mappa, hozzon létre egy fájlt **portal-ext.properties**.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-191">In the **liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** folder, create a file named **portal-ext.properties**.</span></span> <span data-ttu-id="b7d5b-192">Ez a fájl tartalmaznia kell egy sorban, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-192">This file needs to contain one line, as shown here:</span></span>

    liferay.home=%HOME%/site/wwwroot/liferay

<span data-ttu-id="b7d5b-193">Ugyanazon a szinten directory a tomcat-7.0.40 mappaként, hozzon létre egy fájlt **web.config** a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="b7d5b-193">At the same directory level as the tomcat-7.0.40 folder, create a file named **web.config** with the following content:</span></span>

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

<span data-ttu-id="b7d5b-194">Az a **httpPlatform** blokkot, a **requestTimeout** értéke "00: 10:00".</span><span class="sxs-lookup"><span data-stu-id="b7d5b-194">Under the **httpPlatform** block, the **requestTimeout** is set to “00:10:00”.</span></span>  <span data-ttu-id="b7d5b-195">Akkor csökkenteni lehet, de majd valószínűleg néhány időtúllépés hibába ütközik, miközben Liferay rendszerindítása van.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-195">It can be reduced but then you are likely to see some timeout errors while Liferay is bootstrapping.</span></span>  <span data-ttu-id="b7d5b-196">Ha ez az érték megváltozik, akkor a **connectionTimeout** a tomcat a server.xml is módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-196">If this value is changed, then the **connectionTimeout** in the tomcat server.xml should also be modified.</span></span>  

<span data-ttu-id="b7d5b-197">Érdemes megjegyezni, hogy a JRE_HOME környezetben varariable van megadva a fenti Web.config úgy, hogy a 64 bites JDK mutasson.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-197">It is worth noting that the JRE_HOME environnment varariable is specified in the above web.config to point to the 64-bit JDK.</span></span> <span data-ttu-id="b7d5b-198">Az alapértelmezett érték a 32 bites, de Liferay memória magas szintű igényelhetnek, mivel a 64 bites JDK használata javasolt.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-198">The default is 32-bit, but since Liferay may require high levels of memory, it is recommended to use the 64-bit JDK.</span></span>

<span data-ttu-id="b7d5b-199">Miután a módosítások, indítsa újra a webes alkalmazás Liferay fut, majd, nyissa meg a http://yourwebapp.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-199">Once you make these changes, restart your web app running Liferay, Then, open http://yourwebapp.</span></span> <span data-ttu-id="b7d5b-200">A Liferay portál webes alkalmazás gyökérkönyvtárában érhető el.</span><span class="sxs-lookup"><span data-stu-id="b7d5b-200">The Liferay portal is available from the web app root.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b7d5b-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7d5b-201">Next steps</span></span>
<span data-ttu-id="b7d5b-202">Liferay kapcsolatos további információkért lásd: [http://www.liferay.com](http://www.liferay.com).</span><span class="sxs-lookup"><span data-stu-id="b7d5b-202">For more information about Liferay, see [http://www.liferay.com](http://www.liferay.com).</span></span>

<span data-ttu-id="b7d5b-203">További információt a Java [Azure Java-fejlesztőknek](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="b7d5b-203">For more information about Java, visit [Azure for Java developers](/java/azure).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
