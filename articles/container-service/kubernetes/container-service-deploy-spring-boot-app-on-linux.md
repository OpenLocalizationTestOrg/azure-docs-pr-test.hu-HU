---
title: "az Azure Tárolószolgáltatásban Linux rugó rendszerindító webalkalmazás aaaDeploy |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan azonban hello lépéseket toodeploy egy rugó rendszerindító alkalmazásra, amely Linux webes alkalmazás a Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 2c44be1c7f66a38f48239001f0be9e90c7e6edef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-hello-azure-container-service"></a>Az Azure Tárolószolgáltatás hello Linux rugó rendszerindító-alkalmazás központi telepítése

Hello  **[rugó keretrendszer]**  egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat. Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.

**[Docker]**  van nyílt forráskódú megoldások, amelyek segítségével a fejlesztők automatizálásához hello telepítési méretezés és a tárolók a futó alkalmazások kezelését.

Ez az oktatóanyag bemutatja, hogyan használja a Docker toodevelop és rugó rendszerindító alkalmazás tooa Linux-állomást telepíthet hello [Azure tároló szolgáltatás (ACS)].

## <a name="prerequisites"></a>Előfeltételek

A sorrend toocomplete hello lépések ebben az oktatóanyagban kell toohave hello a következő előfeltételek:

* Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].
* Hello [Azure parancssori felület (CLI)].
* Egy naprakész [Java fejlesztői készlet (JDK)].
* Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.
* A [Git] ügyfél.
* A [Docker] ügyfél.

> [!NOTE]
>
> Ez az oktatóanyag toohello virtualizálási követelményeinek miatt nem lépések hello ebben a cikkben; virtuális gépen a fizikai számítógép engedélyezett virtualizációs szolgáltatások kell használnia.
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a>A webalkalmazás első lépések a Docker hello rugó rendszerindító létrehozása

hello következő lépések végigvezetik szükséges toocreate rugó rendszerindító egyszerű webalkalmazások és tesztelik azt helyileg hello lépéseket.

1. Nyisson meg egy parancssort, és hozzon létre egy helyi könyvtár toohold, az alkalmazáshoz, majd a Könyvtárváltás toothat; Példa:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   – vagy –
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klónozás hello [rugó rendszerindító a Docker bevezetés] mintaprojektet könyvtárba, amely hello létrehozott; például:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Directory befejeződött toohello projekt; módosítása Példa:
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Build Maven; használatával hello JAR-fájlra Példa:
   ```
   mvn package
   ```

1. Hello webalkalmazás létrehozása, módosítása directory toohello `target` könyvtárat, ahol hello JAR-fájlra található start hello webalkalmazás; például:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. Hello webes alkalmazás tesztelése a helyileg a webböngésző segítségével tooit megkeresésével. Például ha a curl érhető el, és úgy hello Tomcat kiszolgálón toorun 80-as porton:
   ```
   curl http://localhost
   ```

1. A következő üzenet jelenik meg hello kell megjelennie: **Hello Docker World!**

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-container-registry-toouse-as-a-private-docker-registry"></a>Hozzon létre egy Azure-tároló beállításjegyzék toouse titkos Docker-beállításjegyzék

hello következő lépések végigvezetik hello Azure portál toocreate egy Azure-tároló beállításjegyzék használatával.

> [!NOTE]
>
> Ha azt szeretné, hogy toouse hello Azure parancssori felület helyett hello Azure-portálon, kövesse hello [hozza létre a titkos Docker tároló beállításkulcs használatával hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).
>

1. Keresse meg a toohello [Azure-portálon] , jelentkezzen be.

   Miután bejelentkezett fiók tooyour hello Azure-portálon, a lépésekkel hello a hello [hozza létre a titkos Docker tároló beállításkulcs hello Azure-portál használatával] cikket, amely a következő lépéseket a hello hello átfogalmazni vannak szakét célszerűségi.

1. Hello menü ikonra a **+ új**, majd kattintson a **tárolók**, és kattintson a **Azure tároló beállításjegyzék**.
   
   ![Hozzon létre egy új Azure-tároló beállításjegyzék][AR01]

1. Amikor hello Azure tároló beállításjegyzék sablon hello adatai lap megjelenik, kattintson a **létrehozása**. 

   ![Hozzon létre egy új Azure-tároló beállításjegyzék][AR02]

1. Ha hello **létrehozás tároló beállításjegyzék** lap is megjelenik, írja be a **beállításjegyzék neve** és **erőforráscsoport**, válassza a **engedélyezése** a Hello **rendszergazdai jogú felhasználó**, és kattintson a **létrehozása**.

   ![Azure-tároló beállításjegyzék-beállítások konfigurálása][AR03]

1. Miután létrejött a tároló beállításjegyzék, keresse meg a tároló regisztrációs tooyour hello Azure-portálon, és kattintson a **hívóbetűk**. Jegyezze fel a hello felhasználónév és jelszó hello további lépéseket.

   ![Az Azure tároló beállításkulcsok hozzáférés][AR04]

## <a name="configure-maven-toouse-your-azure-container-registry-access-keys"></a>Maven toouse az Azure tároló hozzáférés beállításkulcsok konfigurálása

1. Nyissa meg a toohello konfigurációs könyvtára a Maven telepítéséhez, és nyissa meg a hello *settings.xml* fájlt egy szövegszerkesztőben.

1. Az Azure-tároló beállításjegyzék-hozzáférési beállítások hozzáadása az előző szakaszából hello ezen oktatóanyag toohello `<servers>` hello gyűjtemény *settings.xml* fájl; például:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg (például: "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker / teljes*"), és nyissa meg hello *pom.xml* fájlt egy szövegszerkesztőben.

1. Frissítés hello `<properties>` hello gyűjtemény *pom.xml* hello bejelentkezési kiszolgáló értéke az hello Ez az oktatóanyag előző szakaszából a Azure tároló rendszerleíró fájlt például:

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Frissítés hello `<plugins>` hello gyűjtemény *pom.xml* fájlt úgy, hogy hello `<plugin>` hello bejelentkezési cím és a beállításjegyzék kiszolgálónév a hello Ez az oktatóanyag előző szakaszából a Azure tároló beállításjegyzék tartalmazza. Példa:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg és futtassa a következő parancs toorebuild hello alkalmazás hello és hello tároló tooyour Azure tároló beállításjegyzék leküldéses:

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> Amikor a Docker-tároló tooAzure leküldendő, amely hasonló tooone a hello annak ellenére, hogy a Docker-tároló létrehozása sikerült a következő hibaüzenet jelenhet meg:
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Ha ez történik, szükség lehet a toosign a tooyour hello Docker parancssori; az Azure-fiók Példa:
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Majd tolható ki a tároló parancssorból hello; Példa:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Webalkalmazás létrehozása az Azure App Service a tároló lemezkép használata Linux rendszeren

1. Keresse meg a toohello [Azure-portálon] , jelentkezzen be.

1. Hello menü ikonra a **+ új**, majd kattintson a **Web + mobil**, és kattintson a **Linux webalkalmazás**.
   
   ![Egy új webalkalmazás létrehozása az Azure-portálon hello][LX01]

1. Ha hello **Linux webalkalmazás** lap is megjelenik, írja be a következő információ hello:

   a. Adjon meg egy egyedi nevet hello **alkalmazásnév**; például: "*wingtiptoyslinux*."

   b. Válassza ki a **előfizetés** hello legördülő listából.

   c. Válasszon egy meglévő **erőforráscsoport**, vagy adjon meg egy nevet toocreate egy új erőforráscsoportot.

   d. Kattintson a **konfigurálása tároló** , és írja be a következő információ hello:

      * Válasszon **titkos beállításjegyzék**.

      * **Kép és a nem kötelező címke**: írja be a tároló nevét, a korábbi, például: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"

      * **URL-címe**: Adja meg a beállításjegyzék URL-CÍMÉT, korábban; például: "*https://wingtiptoysregistry.azurecr.io*"

      * **Bejelentkezési felhasználónevének** és **jelszó**: Adja meg a bejelentkezési hitelesítő adatait a **hívóbetűk** , amelyet az előző lépésben használt.
   
   e. Miután megadta az összes hello fent információkat, kattintson a **OK**.

   ![A webalkalmazás-beállítások konfigurálása][LX02]

1. Kattintson a **Create** (Létrehozás) gombra.

> [!NOTE]
>
> Azure automatikusan felelteti meg internetes kérelmek tooembedded Tomcat kiszolgálót, hogy fut-hello szabványos 80-as vagy a 8080-as port. Azonban ha a beágyazott Tomcat kiszolgálón toorun konfigurált egy egyéni portot, kell tooadd egy környezeti változó tooyour webalkalmazás, amely meghatározza a beágyazott Tomcat kiszolgálót hello port. toodo Igen, használja a lépéseket követve hello:
>
> 1. Keresse meg a toohello [Azure-portálon] , jelentkezzen be.
> 
> 2. Hello ikonra a **alkalmazásszolgáltatások**. (Lásd az alábbi képen hello #1 elemére.)
>
> 3. Válassza ki a webalkalmazás hello listából. (#2 elem az alábbi képen hello.)
>
> 4. Kattintson a **Alkalmazásbeállítások**. (Eleme #3 az alábbi képen hello.)
>
> 5. A hello **Alkalmazásbeállítások** területen adjon hozzá egy új környezeti változó nevű **PORT** , és írja be az egyéni portszám hello értékhez. (Eleme #4 hello az alábbi képen.)
>
> 6. Kattintson a **Save** (Mentés) gombra. (#5 a eleme az alábbi képen hello.)
>
> ![Elmenti egy egyéni portszám hello Azure-portálon][LX03]
>

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

Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [A rugó rendszerindító alkalmazás toohello Azure App Service telepítése](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [A rugó rendszerindító alkalmazás Kubernetes gazdagépfürtökön hello Azure Tárolószolgáltatás a központi telepítése](container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].

További hello rugó rendszerindító a Docker mintaprojektet kapcsolatos további információkért lásd: [rugó rendszerindító a Docker bevezetés].

Első lépések a saját rugó rendszerindító alkalmazásokkal kapcsolatban lásd: hello **rugó Initializr** https://start.spring.io/ címen.

Első lépések egy egyszerű rugó rendszerindító alkalmazás létrehozásával kapcsolatos további információkért lásd: hello rugó Initializr https://start.spring.io/ címen.

További példák a hogyan toouse egyéni Docker lemezképek az Azure-ral [Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával].

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure tároló szolgáltatás (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[hozza létre a titkos Docker tároló beállításkulcs hello Azure-portál használatával]: /azure/container-registry/container-registry-get-started-portal
[Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
