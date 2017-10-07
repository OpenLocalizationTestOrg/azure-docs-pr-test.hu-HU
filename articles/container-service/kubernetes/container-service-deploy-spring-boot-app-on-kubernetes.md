---
title: "a rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Kubernetes aaaDeploy |} Microsoft Docs"
description: "Ez az oktatóanyag részletesen ismerteti, ha hello lépéseket toodeploy egy rugó rendszerindító alkalmazás Kubernetes fürtben Microsoft Azure-on."
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
ms.openlocfilehash: 2bf9df459f874a1f478f43cdd29992d86c370837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-hello-azure-container-service"></a>A rugó rendszerindító alkalmazás Kubernetes gazdagépfürtökön hello Azure Tárolószolgáltatás a központi telepítése

Hello  **[rugó keretrendszer]**  egy népszerű nyílt forráskódú keretrendszer, amely a fejlesztőket Java webes, mobil és API-alkalmazások létrehozása. Ez az oktatóanyag használja egy mintaalkalmazást használatával létrehozott [rugó rendszerindító], egy egyezmény adatvezérelt megközelítés rugó tooget használatával gyorsan lépéseket.

**[Kubernetes]**  és  **[Docker]**  vannak nyílt forráskódú megoldások, amelyek segítségével a fejlesztők hello telepítési, méretezés és automatizálását a futó alkalmazások kezelése a tárolók.

Ez az oktatóanyag bemutatja, hogyan abban az esetben, ha a két népszerű, nyílt forráskódú technológiák toodevelop kombinálásával, és telepítheti a rugó rendszerindító alkalmazás tooMicrosoft Azure. Pontosabban, használhat  *[rugó rendszerindító]*  alkalmazásfejlesztés,  *[Kubernetes]*  tároló üzembe helyezését és hello [Azure tároló szolgáltatás (ACS)] toohost az alkalmazást.

### <a name="prerequisites"></a>Előfeltételek

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

hello lépések végigvezetik a rugó rendszerindító webalkalmazás létrehozása, és helyi tesztelés keresztül.

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

1. Klónozás hello [rugó rendszerindító a Docker bevezetés] mintaprojektet hello könyvtárba.
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Directory befejeződött toohello projekt módosítása.
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Használja a Maven toobuild és futtatási hello mintaalkalmazást.
   ```
   mvn package spring-boot:run
   ```

1. Teszt hello webalkalmazás toohttp://localhost:8080 megkeresésével, vagy hello következőre `curl` parancs:
   ```
   curl http://localhost:8080
   ```

1. A következő üzenet jelenik meg hello kell megjelennie: **Hello Docker World**

   ![Keresse meg a helyi mintaalkalmazás][SB01]

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a>Hozzon létre egy Azure tároló beállításjegyzék hello Azure parancssori felület használatával

1. Nyisson meg egy parancssort.

1. Jelentkezzen be tooyour Azure-fiók:
   ```azurecli
   az login
   ```

1. Hozzon létre egy erőforráscsoportot hello ebben az oktatóanyagban használt Azure-erőforrások.
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Hozzon létre egy saját Azure-tárolót beállításjegyzék hello erőforráscsoportban. hello oktatóanyag hello mintaalkalmazást, a későbbi lépésekben Docker kép toothis beállításjegyzékbeli leküldéses értesítések. Cserélje le `wingtiptoysregistry` a beállításjegyzék egyedi nevére.
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-toohello-container-registry"></a>Az alkalmazás toohello tároló beállításjegyzék leküldéses

1. Keresse meg a Maven telepítés toohello konfigurációs könyvtára (alapértelmezett ~/.m2/ vagy C:\Users\username\.m2) és a nyitott hello *settings.xml* fájlt egy szövegszerkesztőben.

1. A tároló beállításjegyzék hello jelszó lekérése hello Azure parancssori felület.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Adja hozzá az Azure-tároló beállításjegyzék ID azonosítója és jelszava tooa új `<server>` hello gyűjtemény *settings.xml* fájlt.
Hello `id` és `username` hello beállításjegyzék hello neve van. Használjon hello `password` hello előző parancs (idézőjelek nélkül) értéket.

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg (például "*C:\SpringBoot\gs-spring-boot-docker\complete*"vagy"*/users/robert/SpringBoot/gs-spring-boot-docker / teljes*"), és nyissa meg hello *pom.xml* fájlt egy szövegszerkesztőben.

1. Frissítés hello `<properties>` hello gyűjtemény *pom.xml* hello bejelentkezési kiszolgáló értéke az Azure-tároló rendszerleíró fájlt.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Frissítés hello `<plugins>` hello gyűjtemény *pom.xml* fájlt úgy, hogy hello `<plugin>` hello bejelentkezési kiszolgáló címét és a beállításkulcs neve az Azure-tároló beállításjegyzék tartalmazza.

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

1. A rugó rendszerindító alkalmazás befejeződött toohello projekt könyvtárában nyissa meg, és futtassa a következő parancs toobuild hello Docker-tároló és a leküldéses hello kép toohello beállításjegyzék hello:

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  Hello következő hasonló tooone esetén Maven leküldéses értesítések hello kép tooAzure hibaüzenet jelenhet meg:
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Ha ez a hibaüzenet jelenik meg, jelentkezzen be a Docker parancssori hello tooAzure.
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Majd küldje le a tároló:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-hello-azure-cli"></a>Hozzon létre egy Kubernetes fürtöt ACS hello Azure parancssori felület használatával

1. Az Azure Tárolószolgáltatásban Kubernetes fürt létrehozása. hello következő parancs létrehoz egy *kubernetes* hello fürt *tartománynév-kubernetes* erőforrás csoport és *tartománynév-tárolószolgáltatás* hello fürtként név, és *tartománynév-kubernetes* , hello DNS-előtagja:
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   Ez a parancs eltarthat egy ideig toocomplete.

1. Telepítés `kubectl` Azure parancssori felület használatával hello. Linux-felhasználók rendelkezhetnek tooprefix erre a parancsra `sudo` óta hello Kubernetes CLI túl telepíti`/usr/local/bin`.
   ```azurecli
   az acs kubernetes install-cli
   ```

1. Töltse le a hello fürt konfigurációs adatait, kezelheti a fürt hello Kubernetes webes felületet és `kubectl`. 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-hello-image-tooyour-kubernetes-cluster"></a>Hello kép tooyour Kubernetes fürt központi telepítése

Ez az oktatóanyag telepíti hello alkalmazás használatával `kubectl`, lehetővé teszi tooexplore hello telepítési hello Kubernetes webes felületen keresztül.

### <a name="deploy-with-hello-kubernetes-web-interface"></a>Hello Kubernetes webes felület telepítése

1. Nyisson meg egy parancssort.

1. Nyissa meg az alapértelmezett böngésző hello konfigurációs webhely a Kubernetes fürt:
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. Hello Kubernetes konfigurációs webhely megnyitása a böngészőben, hivatkozásra hello túl**indexelése alkalmazás telepítése**:

   ![Kubernetes konfigurációs webhely][KB01]

1. Ha hello **indexelése alkalmazás telepítése** lap is megjelenik, adja meg az alábbi beállítások hello:

   a. Válassza ki **adja meg az alkalmazás adatait az alábbi**.

   b. Adja meg a rugó rendszerindító alkalmazás neve hello **alkalmazásnév**; például: "*gs-rugó-rendszerindítás – docker*".

   c. Adja meg a bejelentkezési kiszolgáló és a tároló lemezkép korábbi hello **tároló kép**; például: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".

   d. Válasszon **külső** a hello **szolgáltatás**.

   e. Adja meg a külső és belső portok hello **Port** és **céloz port** szövegmezőket.

   ![Kubernetes konfigurációs webhely][KB02]


1. Kattintson a **telepítés** toodeploy hello tároló.

   ![Tároló üzembe][KB05]

1. Miután az alkalmazás telepítve van, megjelenik-e a felsorolt rugó rendszerindító alkalmazás **szolgáltatások**.

   ![Kubernetes szolgáltatások][KB06]

1. Ha hello hivatkozásra kattintva **külső végpontok száma**, az Azure-on futó rugó rendszerindító alkalmazás látható.

   ![Kubernetes szolgáltatások][KB07]

   ![Keresse meg a mintaalkalmazás az Azure-on][SB02]


### <a name="deploy-with-kubectl"></a>Kubectl üzembe helyezéséhez

1. Nyisson meg egy parancssort.

1. A tároló hello Kubernetes fürt hello használatával futtassa `kubectl run` parancsot. Adja meg a szolgáltatás nevét az alkalmazáshoz a Kubernetes és hello teljes kép neve. Példa:
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   Ebben a parancsban:

   * hello Tárolónév `gs-spring-boot-docker` hello után azonnal megadott `run` parancs

   * Hello `--image` hello kombinált bejelentkezési kiszolgáló és a lemezkép neve megegyezik a paraméter`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. Külsőleg hello segítségével teszi közzé a Kubernetes fürt `kubectl expose` parancsot. Adja meg a szolgáltatás nevét, hello nyilvánosan elérhető TCP használt port tooaccess hello alkalmazást, és a hello belső célportnak figyeli az alkalmazás. Példa:
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   Ebben a parancsban:

   * hello Tárolónév `gs-spring-boot-docker` hello után azonnal megadott `expose deployment` parancs

   * Hello `--type` paraméter határozza meg, hogy hello fürt terheléselosztót használ

   * Hello `--port` paraméterrel hello nyilvánosan elérhető TCP-port a 80-as. Ezen a porton hello alkalmazás érhető el.

   * Hello `--target-port` paraméterrel hello belső TCP-port a 8080-as. hello terheléselosztó kérelmek tooyour alkalmazás ezen a porton továbbítja.

1. Miután hello alkalmazás telepítve van a toohello fürt lekérdezése hello külső IP-cím, és nyissa meg a böngészőben:

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Keresse meg a mintaalkalmazás az Azure-on][SB02]


## <a name="next-steps"></a>Következő lépések

Azure rugó rendszerindítással kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [A rugó rendszerindító alkalmazás toohello Azure App Service telepítése](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Az Azure Tárolószolgáltatás hello Linux rugó rendszerindító-alkalmazás központi telepítése](container-service-deploy-spring-boot-app-on-linux.md)

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].

A Docker mintaprojektet hello rugó rendszerindító kapcsolatos további információkért lásd: [rugó rendszerindító a Docker bevezetés].

a következő hivatkozások hello rugó rendszerindító alkalmazások létrehozásával kapcsolatos további információkat:

* Egy egyszerű rugó rendszerindító alkalmazás létrehozásával kapcsolatos további információkért lásd: hello rugó Initializr https://start.spring.io/ címen.

hello következő hivatkozásokra kattintva további információt az Azure-ral Kubernetes használatáról:

* [A Tárolószolgáltatás Kubernetes fürt első lépések](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [Hello segítségével Kubernetes webes felhasználói felülete, az Azure Tárolószolgáltatásban](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

További információ a Kubernetes parancssori felület használatával érhető el hello **kubectl** felhasználói útmutatóban talál: <https://kubernetes.io/docs/user-guide/kubectl/>.

hello Kubernetes webhely TITKOS nyilvántartó képek használatát ismertetik, amelyek több cikkek rendelkezik:

* [Három munkaállomás-csoporttal fiókjainak szolgáltatás konfigurálása]
* [Névterek]
* [Húzza a lemezkép egy titkos beállításjegyzékből]

További példák a hogyan toouse egyéni Docker lemezképek az Azure-ral [Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával].

<!-- URL List -->

[Azure parancssori felület (CLI)]: /cli/azure/overview
[Azure tároló szolgáltatás (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Azure webalkalmazás Linux rendszeren egyéni Docker-lemezkép használatával]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó rendszerindító a Docker bevezetés]: https://github.com/spring-guides/gs-spring-boot-docker
[rugó keretrendszer]: https://spring.io/
[Három munkaállomás-csoporttal fiókjainak szolgáltatás konfigurálása]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Névterek]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Húzza a lemezkép egy titkos beállításjegyzékből]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
