---
title: "a rugó rendszerindító alkalmazások az Azure-tároló beállításjegyzék tooAzure App Service Azure Web Apps toodeploy aaaHow toouse hello Maven beépülő modul"
description: "Ez az oktatóanyag részletesen ismerteti, ha hello lépéseket toodeploy egy rugó rendszerindító alkalmazás az Azure-tároló beállításjegyzék tooAzure tooAzure App Service egy Maven beépülő modul használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 55b95e310c9ee186a6d77d941c5a620c2e259d8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-in-azure-container-registry-tooazure-app-service"></a>Hogyan toouse hello Maven beépülő modul Azure Web Apps toodeploy egy rugó rendszerindító alkalmazást az Azure-tároló beállításjegyzék tooAzure App Service

Hello  **[rugó keretrendszer]**  egy népszerű nyílt forráskódú keretrendszer, amely a fejlesztőket Java webes, mobil és API-alkalmazások létrehozása. Ez az oktatóanyag használja egy mintaalkalmazást használatával létrehozott [rugó rendszerindító], egy egyezmény adatvezérelt megközelítés rugó tooget használatával gyorsan lépéseket.

Ez a cikk bemutatja, hogyan toodeploy egy minta rugó rendszerindító alkalmazás tooAzure tároló beállításjegyzék, majd használja hello Maven beépülő modul Azure Web Apps toodeploy az alkalmazás tooAzure App Service.

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
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
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

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-service-principal"></a>Hozzon létre egy Azure szolgáltatás egyszerű

Ez a szakasz hozzon létre egy Azure hello Maven beépülő modul által használt, a tároló tooAzure telepítésekor egyszerű szolgáltatást.

1. Nyisson meg egy parancssort.

1. Jelentkezzen be az Azure-fiók használatával hello Azure parancssori felület:
   ```azurecli
   az login
   ```
   Hajtsa végre a hello utasításokat toocomplete hello bejelentkezési folyamat.

1. Hozzon létre egy Azure-szolgáltatás egyszerű:
   ```azurecli
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

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a>Hozzon létre egy Azure tároló beállításjegyzék hello Azure parancssori felület használatával

1. Nyisson meg egy parancssort.

1. Jelentkezzen be tooyour Azure-fiók:
   ```azurecli
   az login
   ```

1. Hozzon létre egy erőforráscsoportot hello Azure-erőforrások fogja használni a cikkben:
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   Cserélje le `wingtiptoysresources` ebben a példában az erőforráscsoport számára egyedi névvel.

1. Hozzon létre egy saját Azure-tárolót beállításjegyzék hello erőforráscsoport rugó rendszerindító alkalmazás: 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   Cserélje le `wingtiptoysregistry` ebben a példában a tároló beállításjegyzék egyedi nevére.

1. Lekéri a tároló beállításjegyzék hello jelszavát:
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   Azure válaszol, és a jelszavát; Példa:
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-tooyour-maven-settings"></a>Adja hozzá az Azure-tárolót beállításjegyzék és az Azure szolgáltatás egyszerű tooyour Maven beállításai

1. Nyissa meg a Maven `settings.xml` fájlt egy szövegszerkesztőben; lehet, hogy a fájl olyan elérési útja, például a következő példák hello:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Az Azure-tároló beállításjegyzék-hozzáférési beállítások hozzáadása az előző szakaszából hello Ez a cikk toohello `<servers>` hello gyűjtemény *settings.xml* fájl; például:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   Az elemek magyarázata:
   Elem | Leírás
   ---|---|---
   `<id>` | A saját Azure-tárolót beállításjegyzék hello nevét tartalmazza.
   `<username>` | A saját Azure-tárolót beállításjegyzék hello nevét tartalmazza.
   `<password>` | Ez a cikk korábbi részében hello lekért hello jelszót tartalmaz.

1. Ez a cikk toohello egy korábbi szakaszában az Azure-szolgáltatás egyszerű beállítás hozzáadása `<servers>` hello gyűjtemény *settings.xml* fájl; például:

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

## <a name="build-your-docker-container-image-and-push-it-tooyour-azure-container-registry"></a>A Docker-tároló lemezképet létre, és leküldeni tooyour Azure tároló beállításjegyzék

1. Keresse meg a rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában (pl. "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), és nyissa meg hello *pom.xml* fájlt egy szövegszerkesztőben.

1. Frissítés hello `<properties>` hello gyűjtemény *pom.xml* hello bejelentkezési kiszolgáló értéke az hello Ez az oktatóanyag előző szakaszából a Azure tároló rendszerleíró fájlt például:

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   Az elemek magyarázata:
   Elem | Leírás
   ---|---|---
   `<azure.containerRegistry>` | A saját Azure-tárolót beállításjegyzék hello nevét adja meg.
   `<docker.image.prefix>` | Adja meg a saját Azure-tárolót a rendszerleíró adatbázis hozzáfűzésével származtatott hello URL-címe ". azurecr.io" a személyes tárolót beállításjegyzék toohello nevét.

1. Ellenőrizze, hogy `<plugin>` hello Docker beépülő modul a számára a *pom.xml* fájl hello bejelentkezési cím és a beállításjegyzék kiszolgálónév hello előző lépésben hello megfelelő tulajdonságait tartalmazza az oktatóanyag. Példa:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   Az elemek magyarázata:
   Elem | Leírás
   ---|---|---
   `<serverId>` | Adja meg azt a hello tulajdonságot, mely tartalmazza a saját Azure-tárolót beállításjegyzék nevét.
   `<registryUrl>` | Adja meg a saját Azure-tárolót beállításjegyzék hello URL-CÍMÉT tartalmazó hello tulajdonság.

1. A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg és futtassa a következő parancs toorebuild hello alkalmazás hello és hello tároló tooyour Azure tároló beállításjegyzék leküldéses:

   ```
   mvn package docker:build -DpushImage 
   ```

1. Választható lehetőség: Tallózás toohello [Azure-portálon] , és ellenőrizze, hogy van-e Docker-tároló kép nevű **gs-rugó-rendszerindítás – docker** a tároló beállításjegyzékben.

   ![Ellenőrizze a tároló az Azure-portálon][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-tooazure"></a>Testre szabhatja a pom.xml, majd és üzembe helyezheti a tároló tooAzure

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
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
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
`<resourceGroup>` | Adja meg a hello célként megadott erőforráscsoportja, amely `wingtiptoysresources` ebben a példában. hello erőforráscsoport üzembe helyezése során létrejön, ha még nem létezik.
`<appName>` | A webalkalmazás hello cél nevét határozza meg. Ebben a példában hello cél neve van `maven-linux-app-${maven.build.timestamp}`, ahol hello `${maven.build.timestamp}` utótagot fűz hozzá a példa tooavoid ütközik. (hello időbélyeg választható; akkor is megadhat bármilyen hello alkalmazásnév egyedi karakterláncot.)
`<region>` | Adja meg a cél hello régió, amely ebben a példában `westus`. (Teljes listája megtalálható a hello [Maven beépülő modul Azure Web Apps] dokumentációját.)
`<containerSettings>` | Hello tulajdonságokat tartalmazó hello nevét és a tároló URL-CÍMÉT adja meg.
`<appSettings>` | Meghatározza a Maven toouse bármely egyedi beállításait, a webes alkalmazás tooAzure telepítésekor. Ebben a példában egy `<property>` elem név-érték párból, amelyek adja meg az alkalmazás hello portot tartalmaz.

> [!NOTE]
>
> hello beállítások toochange hello portszám ebben a példában csak akkor szükség, ha hello port hello alapértelmezett módosítani.
>

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

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben ismertetett különböző technológiákkal hello további információ: a következő cikkek hello:

* [Maven beépülő modul Azure Web Apps]

* [Jelentkezzen be az Azure CLI hello tooAzure](/azure/xplat-cli-connect)

* [Hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven-beállítások referenciája](https://maven.apache.org/settings.html)

* [Docker Maven beépülő modul]

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[Maven beépülő modul Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Docker Maven beépülő modul]: https://github.com/spotify/docker-maven-plugin
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
