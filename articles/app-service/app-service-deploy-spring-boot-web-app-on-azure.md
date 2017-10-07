---
title: "a rugó rendszerindító alkalmazás toohello Azure App Service aaaDeploy |} Microsoft Docs"
description: "Ez az oktatóanyag hello lépéseket toodeploy hello rugó rendszerindító bevezetés web app tooAzure App Service segítségével a fejlesztők ismerteti."
services: app-service\web
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
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 69f9c4903fd740125194402cdb4b4db46a1f2773
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-toohello-azure-app-service"></a>A rugó rendszerindító alkalmazás toohello Azure App Service telepítése

Hello  **[rugó keretrendszer]**  nyílt forráskódú megoldás, amely segít a vállalati szintű alkalmazásokat Java fejlesztői, és egyik hello több népszerű projektek platformra épül [Rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.

Ez az oktatóanyag bemutatja, ha a hello minta rugó rendszerindító bevezetés webalkalmazás létrehozását és telepítését túl[Azure App Service].

### <a name="prerequisites"></a>Előfeltételek

A sorrend toocomplete hello lépések ebben az oktatóanyagban toohave hello következőkre lesz szüksége:

* Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].
* Egy naprakész [Java fejlesztői készlet (JDK)].
* Apache tartozó [Maven] eszköz (3-as verziójához) létrehozása.
* A [Git] ügyfél.

## <a name="create-hello-spring-boot-getting-started-web-app"></a>Hello rugó rendszerindító bevezetés webalkalmazás létrehozása

hello következő lépésekkel haladhat végig szükséges toocreate rugó rendszerindító egyszerű webalkalmazások és tesztelik azt helyileg hello lépéseket.

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

1. Klónozás hello [rugó rendszerindító bevezetés] mintaprojektet hello könyvtárba, újonnan létrehozott; például:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. Directory befejeződött toohello projekt; módosítása Példa:
   ```
   cd gs-spring-boot
   cd complete
   ```

1. Build Maven; használatával hello JAR-fájlra Példa:
   ```
   mvn package
   ```

1. Hello webalkalmazás létrehozása után módosítsa a könyvtárat toohello JAR-fájlra, és indítsa el a hello webalkalmazás; Példa:
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. Keresse meg webböngészővel toohttp://localhost:8080 hello webes alkalmazás tesztelése, vagy használja a következő példa, ha van elérhető curl hello hello szintaxist:
   ```
   curl http://localhost:8080
   ```

1. A következő üzenet jelenik meg hello kell megjelennie: **hónap rugó rendszerindításból!**

   ![Keresse meg a mintaalkalmazás][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>Java hozzon létre egy Azure webalkalmazás számára

hello lépések végigvezetik Önt hello lépéseket toocreate az Azure Web Apps, Java hello szükséges beállítások konfigurálása, és állítsa be az FTP hitelesítő adatait.

1. Keresse meg a toohello [Azure-portálon] , és jelentkezzen be.

1. Ha jelentkezett be fiókjába a hello Azure-portálon, kattintson a hello menü ikonja **alkalmazásszolgáltatások**:
   
   ![Azure Portal][AZ01]

1. Ha hello **alkalmazásszolgáltatások** lap is megjelenik, kattintson a **+ Hozzáadás** toocreate egy új App Service.

   ![Az App Service létrehozása][AZ02]

1. Amikor megjelenik a webes alkalmazás sablonok hello listája, hello hello hivatkozásra kattintva alapszintű Microsoft webes alkalmazást.

   ![Webes alkalmazás sablonok][AZ03]

1. Amikor hello Web App sablon hello adatai lap megjelenik, kattintson a **létrehozása**.

   ![Webalkalmazás létrehozása][AZ04]

1. Adjon egyedi nevet a webalkalmazás, és adja meg az esetleges egyéb beállításokat, majd **létrehozása**.

   ![A webalkalmazás-beállítások létrehozása][AZ05]

1. A webalkalmazás létrehozása után kattintson a hello ikonjára **alkalmazásszolgáltatások**, majd kattintson az újonnan létrehozott webalkalmazás:

   ![Lista webalkalmazások][AZ06]

1. Amikor megjelenik a webalkalmazás, adja meg a hello Java-verziót hello lépések használatával:

   a. Kattintson a hello **Alkalmazásbeállítások** menüpont.

   b. Válasszon **Java 8** hello Java-verzió.

   c. Válasszon **legújabb** hello kisebb Java-verzió.

   d. Válasszon **legújabb Tomcat 8.5** hello webes tároló. (Ez a tároló nem ténylegesen használja; Azure fogja használni a rugó rendszerindító alkalmazás hello tárolót.)

   e. Kattintson a **Save** (Mentés) gombra.

   ![Alkalmazásbeállítások][AZ07]

1. Adja meg az FTP telepítési hitelesítő adatokat a lépéseket követve hello használatával:

   a. Kattintson a hello **üzembe helyezési hitelesítő adatok** menüpont.

   b. Adja meg a felhasználónevét és jelszavát.

   c. Kattintson a **Save** (Mentés) gombra.

   ![Adja meg az üzembe helyezési hitelesítő adatok][AZ08]

1. Az FTP-kiszolgáló kapcsolati adatainak beolvasása hello lépések segítségével:

   a. Kattintson a hello **üzembe helyezési hitelesítő adatok** menüpont.

   b. Másolja a teljes FTP-felhasználónév és az URL-címet, és mentse őket az oktatóprogram következő szakaszában hello.

   ![FTP URL-CÍMEK és a hitelesítő adatok][AZ09]

## <a name="deploy-your-spring-boot-web-app-tooazure"></a>A rugó rendszerindító web app tooAzure telepítése

hello lépésekkel haladhat végig hello lépéseket toodeploy a rugó rendszerindító web app tooAzure.

1. Nyisson meg egy szövegszerkesztőt, például a Jegyzettömbben Windows és illessze be a következő szöveg egy új dokumentumba hello, majd mentse hello fájlt *web.config*:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. Hello mentése után *web.config* tooyour fájlrendszer, a tooyour web app használatával hello URL-cím, a felhasználónév és a jelszó az ebben az oktatóanyagban szakasza megelőző hello FTP-n keresztül csatlakozniuk. Példa:
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. Hello távoli könyvtár toohello legfelső szintű mappa módosítása, webalkalmazás (amely */hely/wwwroot*), majd másolja a rugó rendszerindító alkalmazás hello JAR-fájlra, és hello *web.config* a korábbi. Példa:
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. Miután telepítette a JAR és *web.config* fájlok tooyour web app alkalmazásban toorestart a webalkalmazás hello Azure-portál használatával van szüksége:

   ![][AZ10]

1. Hello webes alkalmazás tesztelése a tooyour webes alkalmazás URL-cím, egy webböngésző segítségével keresse meg azt, vagy használja a következő példa, ha van elérhető curl hello hello szintaxist:
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. A következő üzenet jelenik meg hello kell megjelennie: **hónap rugó rendszerindításból!**

   ![Keresse meg a mintaalkalmazás][SB02]

## <a name="next-steps"></a>Következő lépések

Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [A rugó rendszerindító alkalmazás Linux telepítése hello Azure Tárolószolgáltatás](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [A rugó rendszerindító alkalmazás Kubernetes gazdagépfürtökön hello Azure Tárolószolgáltatás a központi telepítése](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].

Depoying web apps tooAzure FTP használatával kapcsolatos további információkért lásd: [telepítheti az alkalmazást tooAzure használatával az FTP/S App Service].

További hello rugó rendszerindító mintaprojektet kapcsolatos további információkért lásd: [rugó rendszerindító bevezetés].

Első lépések a saját rugó rendszerindító alkalmazásokkal kapcsolatban lásd: hello **rugó Initializr** https://start.spring.io/ címen.

A webalkalmazás további beállítások konfigurálásával kapcsolatos további információkért lásd: [webes alkalmazások konfigurálása az Azure App Service].

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Azure-portálon]: https://portal.azure.com/
[webes alkalmazások konfigurálása az Azure App Service]: /azure/app-service-web/web-sites-configure
[telepítheti az alkalmazást tooAzure használatával az FTP/S App Service]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java fejlesztői készlet (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó rendszerindító bevezetés]: https://github.com/spring-guides/gs-spring-boot
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
