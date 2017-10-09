---
title: "a rugó rendszerindító alkalmazás tooAzure Azure Web Apps toodeploy aaaHow toouse hello Maven beépülő modul"
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
ms.openlocfilehash: 376fe90fe20621e15d7c9856214937c78b66026a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-tooazure"></a>Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy rugó rendszerindító alkalmazás tooAzure

Hello [Maven beépülő modul Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) a [Apache Maven](http://maven.apache.org/) Maven-projektek az Azure App Service zökkenőmentes integrációt biztosít, és megkönnyíti a fejlesztők toodeploy web Apps hello folyamat App Service tooAzure.

Ez a cikk bemutatja az Azure Web Apps toodeploy hello Maven beépülő modul használatával egy minta rugó rendszerindító alkalmazás tooAzure alkalmazásszolgáltatások.

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

## <a name="clone-hello-sample-spring-boot-web-app"></a>Klónozás hello rugó rendszerindító webes mintaalkalmazás

Ebben a szakaszban egy befejezett rugó rendszerindító alkalmazás klónozása, és helyben tesztelheti.

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

1. Klónozás hello [rugó rendszerindító bevezetés] mintaprojektet könyvtárba, amely hello létrehozott; például:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. Directory befejeződött toohello projekt; módosítása Példa:
   ```shell
   cd gs-spring-boot/complete
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

1. A következő üzenet jelenik meg hello kell megjelennie: **hónap rugó rendszerindításból!**

## <a name="create-an-azure-service-principal"></a>Hozzon létre egy Azure szolgáltatás egyszerű

Ez a szakasz hozzon létre egy Azure hello Maven beépülő modul által használt, a webes alkalmazás tooAzure telepítésekor egyszerű szolgáltatást.

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
   > A JSON-válasz hello értékeket fogja használni, a webes alkalmazás tooAzure hello Maven beépülő modul toodeploy konfigurálásakor. Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, és `tttttttt` helyőrző értékeket, amelyek vannak használatban a példa toomake azt könnyebb toomap ezen értékek tootheir megfelelő elemek a Maven konfigurálásakor `settings.xml` hello a következő fájl a szakasz.
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a>Maven toouse konfigurálása az Azure szolgáltatás egyszerű

Ebben a szakaszban a hello értékek a az Azure-szolgáltatás egyszerű tooconfigure hello authentication Maven használó a webes alkalmazás tooAzure telepítésekor használhat.

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

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-tooazure"></a>Választható lehetőség: Testre szabhatja a pom.xml a webes alkalmazás tooAzure telepítése előtt

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

Több érték, amely hello Maven beépülő modul módosíthatja, és az ilyen elemek részletes leírását érhető el hello [Maven beépülő modul Azure Web Apps] dokumentációját. Amely éppen említett, több érték, amelyek érdemes ebben a cikkben kiemelve:

Elem | Leírás
---|---|---
`<version>` | Adja meg a hello hello verziója [Maven beépülő modul Azure Web Apps]. Ellenőrizni kell a felsorolt hello hello verzió [Maven központi tárház](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure, Ön által használt hello legújabb verziójára.
`<authentication>` | Adja meg a hello hitelesítési adatokat az Azure-hoz, amely ebben a példában tartalmaz egy `<serverId>` elem, amely tartalmazza `azure-auth`; Maven adott érték toolook hello Azure szolgáltatás egyszerű értékeket használja a Maven *settings.xml* fájl, ez a cikk korábbi részében megadott.
`<resourceGroup>` | Adja meg a hello célként megadott erőforráscsoportja, amely `maven-plugin` ebben a példában. hello erőforráscsoport a telepítés során jön létre, ha még nem létezik.
`<appName>` | A webalkalmazás hello cél nevét határozza meg. Ebben a példában hello cél neve van `maven-web-app-${maven.build.timestamp}`, ahol hello `${maven.build.timestamp}` utótagot fűz hozzá a példa tooavoid ütközik. (hello időbélyeg választható; akkor is megadhat bármilyen hello alkalmazásnév egyedi karakterláncot.)
`<region>` | Adja meg a cél hello régió, amely ebben a példában `westus`. (Teljes listája megtalálható a hello [Maven beépülő modul Azure Web Apps] dokumentációját.)
`<javaVersion>` | Megadja a webalkalmazás hello Java futásidejű verzióját. (Teljes listája megtalálható a hello [Maven beépülő modul Azure Web Apps] dokumentációját.)
`<deploymentType>` | Adja meg a webalkalmazás központi telepítési típus. Egyelőre csak `ftp` támogatott, bár a más központi telepítési típusok a fejlesztési támogatja.
`<resources>` | Megadja az erőforrások és a célként megadott célhelyekre Maven használja, a webes alkalmazás tooAzure telepítésekor. Ebben a példában két `<resource>` elemet adja meg, hogy Maven helyezik üzembe a hello JAR-fájlra a webes alkalmazáshoz és hello *web.config* fájl hello rugó rendszerindító projektből.

## <a name="build-and-deploy-your-web-app-tooazure"></a>Hozza létre, és a webes alkalmazás tooAzure telepítése

Amennyiben az összes hello beállítás állított be a cikk szakaszaiban megelőző hello, készen áll a toodeploy áll a webes alkalmazás tooAzure. toodo Igen, használja a lépéseket követve hello:

1. Hello parancssort vagy terminálablakot korábban használt, építse újra a Maven használatával, ha bármely módosítások toohello hello JAR-fájlra *pom.xml* fájl; például:
   ```shell
   mvn clean package
   ```

1. A webes alkalmazás tooAzure telepíteni a Maven; Példa:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven telepíti a webes alkalmazás tooAzure; Ha hello webalkalmazás már nem létezik, a rendszer létrehozza.

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

* [Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy indexelése rugó rendszerindító alkalmazás tooAzure](app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [Hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven-beállítások referenciája](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[rugó rendszerindító bevezetés]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven beépülő modul Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
