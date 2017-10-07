---
title: "Azure Web Apps toodeploy egy indexelése rugó rendszerindító alkalmazás tooAzure aaaHow toouse hello Maven beépülő modul"
description: "Ismerje meg, hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy rugó rendszerindító alkalmazás tooAzure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: e7e760d4ef5bd4c92a4126a50a2b12e5c8f2b4a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-containerized-spring-boot-app-tooazure"></a>Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy indexelése rugó rendszerindító alkalmazás tooAzure

Hello [Maven beépülő modul Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) a [Apache Maven](http://maven.apache.org/) Maven-projektek az Azure App Service zökkenőmentes integrációt biztosít, és megkönnyíti a fejlesztők toodeploy web Apps hello folyamat App Service tooAzure.

Ez a cikk bemutatja az Azure Web Apps toodeploy hello Maven beépülő modul használatával egy Docker-tároló tooAzure alkalmazásszolgáltatások a mintaalkalmazás rugó rendszerindító.

> [!NOTE]
>
> hello Azure Web Apps Maven beépülő modul jelenleg tartozik által megtekinthető villámnézetként. Egyelőre csak az FTP-közzététel támogatott, bár a további szolgáltatások későbbi hello tervbe van véve.
>

## <a name="prerequisites"></a>Előfeltételek

A sorrend toocomplete hello lépések ebben az oktatóanyagban kell toohave hello a következő előfeltételek:

* Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].
* Hello [Azure parancssori felület (CLI)].
* Egy naprakész [Java fejlesztői készlet (JDK)], 1.7 vagy újabb verziója.
* Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.
* A [Git] ügyfél.
* A [Docker] ügyfél.

> [!NOTE]
>
> Ez az oktatóanyag toohello virtualizálási követelményeinek miatt nem lépések hello ebben a cikkben; virtuális gépen a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a>Klónozás hello minta rugó rendszerindító Docker-webalkalmazás

Ebben a szakaszban egy indexelése rugó rendszerindító alkalmazás klónozása, és helyben tesztelheti.

1. Nyisson meg egy parancssort vagy terminálablakot, és hozzon létre egy helyi könyvtár toohold, a rugó rendszerindító alkalmazás, és a Könyvtárváltás toothat; Példa:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   – vagy –
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klónozás hello [rugó rendszerindító a Docker bevezetés] mintaprojektet könyvtárba, amely hello létrehozott; például:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. Directory befejeződött toohello projekt; módosítása Példa:
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. Build Maven; használatával hello JAR-fájlra Példa:
   ```shell
   mvn clean package
   ```

1. Hello webalkalmazás létrehozásakor Maven; használatával hello webalkalmazás elindítása Példa:
   ```shell
   mvn spring-boot:run
   ```

1. Hello webes alkalmazás tesztelése a helyileg a webböngésző segítségével tooit megkeresésével. Például a következő parancsot, ha van elérhető curl hello használata:
   ```shell
   curl http://localhost:8080
   ```

1. A következő üzenet jelenik meg hello kell megjelennie: **Hello Docker World**

## <a name="create-an-azure-service-principal"></a>Hozzon létre egy Azure szolgáltatás egyszerű

Ez a szakasz hozzon létre egy Azure hello Maven beépülő modul által használt, a tároló tooAzure telepítésekor egyszerű szolgáltatást.

1. Nyisson meg egy parancssort.

1. Jelentkezzen be az Azure-fiók használatával hello Azure parancssori felület:
   ```shell
   az login
   ```
   Hajtsa végre a hello utasításokat toocomplete hello bejelentkezési folyamat.

1. Hozzon létre egy Azure-szolgáltatás egyszerű:
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   Ha `uuuuuuuu` hello felhasználónév és `pppppppp` hello szolgáltatás egyszerű hello-jelszó.

1. Azure válaszol, a következő példa hello levő JSON:
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > A JSON-válasz hello értékeket fogja használni, a tároló tooAzure hello Maven beépülő modul toodeploy konfigurálásakor. Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, és `tttttttt` helyőrző értékeket, amelyek vannak használatban a példa toomake azt könnyebb toomap ezen értékek tootheir megfelelő elemek a Maven konfigurálásakor `settings.xml` hello a következő fájl a szakasz.
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a>Maven toouse konfigurálása az Azure szolgáltatás egyszerű

Ebben a szakaszban a hello értékek a az Azure szolgáltatás egyszerű tooconfigure hello authentication Maven által használt, a tároló tooAzure telepítésekor használhat.

1. Nyissa meg a Maven `settings.xml` fájlt egy szövegszerkesztőben; lehet, hogy a fájl olyan elérési útja, például a következő példák hello:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Az Azure-szolgáltatás egyszerű beállítások hozzáadása az előző szakaszából hello ezen oktatóanyag toohello `<servers>` hello gyűjtemény *settings.xml* fájl; például:

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   Az elemek magyarázata:
   Elem | Leírás
   ---|---|---
   `<id>` | Adja meg egy egyedi nevet, amely Maven toolook a biztonsági beállításokat használja, a webes alkalmazás tooAzure központi telepítésekor.
   `<client>` | Hello tartalmaz `appId` a szolgáltatás egyszerű közötti értéket.
   `<tenant>` | Hello tartalmaz `tenant` a szolgáltatás egyszerű közötti értéket.
   `<key>` | Hello tartalmaz `password` a szolgáltatás egyszerű közötti értéket.
   `<environment>` | Hello Azure felhőben célkörnyezet, amely definiálja `AZURE` ebben a példában. (Környezetek teljes listája megtalálható hello [Maven beépülő modul Azure Web Apps] dokumentációja)

1. Mentse és zárja be a hello *settings.xml* fájlt.

## <a name="optional-deploy-your-local-docker-file-toodocker-hub"></a>Választható lehetőség: A helyi Docker-fájl tooDocker központi telepítése

Ha egy Docker-fiókot, létrehozhatja a Docker tároló kép helyileg és leküldeni tooDocker Hub. toodo Igen, használja a lépéseket követve hello.

1. Nyissa meg hello `pom.xml` fájlt egy szövegszerkesztőben rugó rendszerindító alkalmazás.

1. Keresse meg a hello `<imageName>` hello gyermekeleme `<containerSettings>` elemet.

1. Frissítés hello `${docker.image.prefix}` érték és a Docker-fiók neve:
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. A következő központi telepítési módszerekkel hello közül választhat:

   * A tároló lemezkép helyileg fejlesztheti az Maven, és a Docker toopush a tároló tooDocker Hub:
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * Ha hello [Docker beépülő modul Maven] automatikusan létrehozhat telepítve, és a tároló kép tooDocker központ használatával hello `-DpushImage` paraméter:
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-tooazure"></a>Választható lehetőség: Testre szabhatja a pom.xml a tároló tooAzure telepítése előtt

Nyissa meg hello `pom.xml` fájlt egy szövegszerkesztőben rugó rendszerindító alkalmazás, és keresse a hello `<plugin>` elem `azure-webapp-maven-plugin`. Ez az elem a következő példa hello kell hasonlítania:

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

Több érték, amely hello Maven beépülő modul módosíthatja, és az ilyen elemek részletes leírását érhető el hello [Maven beépülő modul Azure Web Apps] dokumentációját. Amely éppen említett, több érték, amelyek érdemes ebben a cikkben kiemelve:

Elem | Leírás
---|---|---
`<version>` | Adja meg a hello hello verziója [Maven beépülő modul Azure Web Apps]. Ellenőrizni kell a felsorolt hello hello verzió [Maven központi tárház](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure, Ön által használt hello legújabb verziójára.
`<authentication>` | Adja meg a hello hitelesítési adatokat az Azure-hoz, amely ebben a példában tartalmaz egy `<serverId>` elem, amely tartalmazza `azure-auth`; Maven adott érték toolook hello Azure szolgáltatás egyszerű értékeket használja a Maven *settings.xml* fájl, ez a cikk korábbi részében megadott.
`<resourceGroup>` | Adja meg a hello célként megadott erőforráscsoportja, amely `maven-plugin` ebben a példában. hello erőforráscsoport üzembe helyezése során létrejön, ha még nem létezik.
`<appName>` | A webalkalmazás hello cél nevét határozza meg. Ebben a példában hello cél neve van `maven-linux-app-${maven.build.timestamp}`, ahol hello `${maven.build.timestamp}` utótagot fűz hozzá a példa tooavoid ütközik. (hello időbélyeg választható; akkor is megadhat bármilyen hello alkalmazásnév egyedi karakterláncot.)
`<region>` | Adja meg a cél hello régió, amely ebben a példában `westus`. (Teljes listája megtalálható a hello [Maven beépülő modul Azure Web Apps] dokumentációját.)
`<appSettings>` | Meghatározza a Maven toouse bármely egyedi beállításait, a webes alkalmazás tooAzure telepítésekor. Ebben a példában egy `<property>` elem név-érték párból, amelyek adja meg az alkalmazás hello portot tartalmaz.

> [!NOTE]
>
> hello beállítások toochange hello portszám ebben a példában csak akkor szükség, ha hello port hello alapértelmezett módosítani.
>

## <a name="build-and-deploy-your-container-tooazure"></a>Hozza létre, és a tároló tooAzure telepítése

Amennyiben az összes hello beállítás állított be a cikk szakaszaiban megelőző hello, készen áll a toodeploy áll a tároló tooAzure. toodo Igen, használja a lépéseket követve hello:

1. Hello parancssort vagy terminálablakot korábban használt, építse újra a Maven használatával, ha bármely módosítások toohello hello JAR-fájlra *pom.xml* fájl; például:
   ```shell
   mvn clean package
   ```

1. A webes alkalmazás tooAzure telepíteni a Maven; Példa:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven telepíti a webes alkalmazás tooAzure; Ha hello webalkalmazás már nem létezik, a rendszer létrehozza.

> [!NOTE]
>
> Ha hello régió hello megadható `<region>` eleme a *pom.xml* fájl nem rendelkezik elegendő kiszolgáló érhető el, ha a telepítés megkezdése, egy hasonló toohello hiba, a következő példában láthatja:
>
> ```
> [INFO] Start deploying tooWeb App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed tooexecute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> Ha ez történik, adjon meg egy másik régióban, és futtassa újra a hello Maven parancs toodeploy az alkalmazás.
>
>

A webes telepítésekor fogja tudni toomanage azt hello segítségével [Azure-portálon].

* A webalkalmazás megjelenik **alkalmazásszolgáltatások**:

   ![A webes alkalmazás szerepel az Azure-portálon App Service szolgáltatások][AP01]

* Hello URL-címet a hello megjelenik a webalkalmazás és **áttekintése** webalkalmazáshoz:

   ![A webalkalmazás URL-címe hello meghatározása][AP02]

<!--
##  OPTIONAL: Configure hello embedded Tomcat server toorun on a different port

hello embedded Tomcat server in hello sample Spring Boot application is configured toorun on port 8080 by default. However, if you want toorun hello embedded Tomcat server toorun on a different port, such as port 80 for local testing, you can configure hello port by using hello following steps.

1. Go toohello *resources* directory (or create hello directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open hello *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify hello **server** setting so that hello server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close hello *application.yml* file.
-->

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben ismertetett különböző technológiákkal hello további információ: a következő cikkek hello:

* [Maven beépülő modul Azure Web Apps]

* [Jelentkezzen be az Azure CLI hello tooAzure](/azure/xplat-cli-connect)

* [Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy rugó rendszerindító alkalmazás tooAzure App Service](app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [Hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven-beállítások referenciája](https://maven.apache.org/settings.html)

* [Docker beépülő modul Maven]

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Docker beépülő modul Maven]: https://github.com/spotify/docker-maven-plugin
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven beépülő modul Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
