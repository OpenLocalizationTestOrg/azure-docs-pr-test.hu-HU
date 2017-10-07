---
title: "a rugó rendszerindítás-inicializáló app toouse Redis Cache aaaHow tooconfigure"
description: "Ismerje meg, hogyan tooconfigure egy rugó rendszerindító alkalmazás létre hello rugó Initializr toouse Azure Redis Cache."
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Redis gyorsítótár rugó, rugó rendszerindító Starter,"
ms.assetid: 
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 7/21/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: ad532c88d2d67b97079eeb0e0e392add29ac365b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-a-spring-boot-initializer-app-toouse-redis-cache"></a>Hogyan tooconfigure egy rugó rendszerindítás-inicializáló app toouse Redis gyorsítótár

## <a name="overview"></a>Áttekintés

Hello  **[rugó keretrendszer]**  nyílt forráskódú megoldás, amely segít a vállalati szintű alkalmazásokat Java fejlesztői. Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása. toohelp fejlesztők rugó rendszerindító az első lépései, több minta rugó rendszerindító csomagok érhetők el: <https://github.com/spring-guides/>. Ezenkívül rugó rendszerindító hello listája toochoosing projektek, hello  **[rugó Initializr]**  a fejlesztőket az egyéni rendszerindító rugó-alkalmazások létrehozásának első lépései.

Ez a cikk bemutatja, hogyan hello Azure portálra, majd hello Redis gyorsítótár létrehozása **rugó Initializr** toocreate egyéni alkalmazás, majd hozza létre egy Java-webalkalmazás tárolja, és lekéri az adatokat a Redis gyorsítótár.

## <a name="prerequisites"></a>Előfeltételek

a következő előfeltételek hello rendelés toofollow hello cikkben leírt lépéseket kell megadni:

* Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].

* A [Java fejlesztői készlet (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1.7 vagy újabb verziója.

* [Apache Maven](http://maven.apache.org/), 3.0-s vagy újabb verziója.

## <a name="create-a-redis-cache-on-azure"></a>Redis Cache gyorsítótár létrehozása az Azure-ban

1. Keresse meg a toohello Azure portál, <https://portal.azure.com/> hello elemet, majd **+ új**.

   ![Azure Portal][AZ01]

1. Kattintson a **adatbázis**, és kattintson a **Redis Cache**.

   ![Azure Portal][AZ02]

1. A hello **új Redis Cache** lapján adja meg a hello **DNS-név** a gyorsítótárhoz, majd adja meg a **előfizetés**, **erőforráscsoport**,  **Hely**, és **tarifacsomag**. Ezek a beállítások megadását, kattintson a **létrehozása** toocreate a gyorsítótárhoz.

   ![Azure Portal][AZ03]

1. Miután befejeződött a gyorsítótárhoz, akkor jelenik meg az Azure megjelenik **irányítópult**, valamint hello alatt **összes erőforrás**, és **Redis Cache-gyorsítótárak** lapokat. Rákattinthat a bármely adott helyek tooopen hello tulajdonságok lapján a gyorsítótár a gyorsítótárhoz.

   ![Azure Portal][AZ04]

1. Ha a gyorsítótár tulajdonságai hello listáját tartalmazó hello lap jelenik meg, kattintson **hívóbetűk** , és másolja a tárelérési kulcsok a gyorsítótárhoz.

   ![Azure Portal][AZ05]

## <a name="create-a-custom-application-using-hello-spring-initializr"></a>Hozzon létre egy egyéni alkalmazást rugó Initializr hello használata

1. Keresse meg a túl<https://start.spring.io/>.

1. Adja meg, hogy toogenerate egy **Maven** a projekt **Java**, adja meg a hello **csoport** és **Aritifact** az alkalmazás nevét majd hello hivatkozásra túl**toohello teljes verzióját Switch** a hello rugó Initializr.

   ![Basic rugó Initializr beállítások][SI01]

   > [!NOTE]
   >
   > hello rugó Initializr fogja használni a hello **csoport** és **Aritifact** nevek toocreate hello csomag neve, pl.: *com.contoso.myazuredemo*.
   >

1. Görgessen lefelé toohello **webes** szakaszt, és a hello jelölőnégyzetet **webes**, majd görgessen lefelé toohello **NoSQL** szakaszt, és a hello jelölőnégyzetet **Redis**, majd görgessen a toohello a hello lap alján, és hello gombra túl**készítése a projekt**.

   ![Teljes rugó Initializr beállítások][SI02]

1. Amikor a rendszer kéri, töltse le a hello projekt tooa elérési útja a helyi számítógépen.

   ![Egyéni rugó rendszerindító-projekt letöltése][SI03]

1. A helyi rendszeren hello fájlok kibontását követően az egyéni rugó rendszerindító alkalmazás készen áll a szerkesztésre lesz.

   ![Egyéni rugó rendszerindító project fájlok][SI04]

## <a name="configure-your-custom-spring-boot-toouse-your-redis-cache"></a>Az egyéni rugó rendszerindító toouse a Redis Cache-gyorsítótár konfigurálása

1. Keresse meg a hello *application.properties* hello fájlban *erőforrások* az alkalmazás könyvtárába, vagy hozzon létre hello fájlt, ha még nem létezik.

   ![Keresse meg a hello application.properties fájl][RE01]

1. Nyissa meg hello *application.properties* fájlt egy szövegszerkesztőben, és adja hozzá a következő sorokat toohello fájl hello, és cserélje le a hello mintaértékek hello megfelelő tulajdonságokkal a gyorsítótárból:

   ```yaml
   # Specify hello DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify hello port for your Redis cache.
   spring.redis.port=6380

   # Specify hello access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Hello application.properties fájl szerkesztése][RE02]

1. Mentse és zárja be a hello *application.properties* fájlt.

1. Hozza létre a *vezérlő* alatt hello forrásmappát a csomag; például:

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   – vagy –

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. Hozzon létre egy új fájlt *HelloController.java* a hello *vezérlő* mappát. Nyissa meg a hello fájlt egy szövegszerkesztőben, és adja hozzá a következő kód tooit hello:

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Value;
   import redis.clients.jedis.Jedis;
   import redis.clients.jedis.JedisShardInfo;

   @RestController
   public class HelloController {
   
      // Retrieve hello DNS name for your cache.
      @Value("${spring.redis.host}")
      private String redisHost;

      // Retrieve hello port for your cache.
      @Value("${spring.redis.port}")
      private int redisPort;

      // Retrieve hello access key for your cache.
      @Value("${spring.redis.password}")
      private String redisPassword;

      @RequestMapping("/")
      // Define hello Hello World controller.
      public String hello() {
      
         // Create a JedisShardInfo object tooconnect tooyour Redis cache.
         JedisShardInfo jedisShardInfo = new JedisShardInfo(redisHost, redisPort, true);
         // Specify your access key.
         jedisShardInfo.setPassword(redisPassword);
         // Create a Jedis object toostore/retrieve information from your cache.
         Jedis jedis = new Jedis(jedisShardInfo);

         // Add a Hello World string tooyour cache.
         jedis.set("greeting", "Hello World!");

         // Return hello string from your cache.
         return jedis.get("greeting");
      }
   }
   ```
   
   Ha szüksége lesz tooreplace `com.contoso.myazuredemo` hello csomag névvel a projekthez.

1. Mentse és zárja be a hello *HelloController.java* fájlt.

1. A rugó rendszerindító alkalmazás Maven létrehozása és futtatása. Példa:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Keresse meg webböngészővel toohttp://localhost:8080 hello webes alkalmazás tesztelése, vagy használja a következő példa, ha van elérhető curl hello hello szintaxist:

   ```shell
   curl http://localhost:8080
   ```

   Megtekintheti az hello "Hello World!" üzenet tartományvezérlőről a minta jelenik meg, amely dinamikusan lekérésére a Redis-gyorsítótárból.

## <a name="next-steps"></a>Következő lépések

Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [A rugó rendszerindító alkalmazás toohello Azure App Service telepítése](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [A rugó rendszerindító alkalmazást futtat egy Kubernetes hello Azure Tárolószolgáltatási fürt](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].

További információ a kezdeti Redis Cache használatának az Azure, a Java című [hogyan toouse Azure Redis Cache-gyorsítótár Java][Redis Cache with Java].

<!-- URL List -->

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó Initializr]: https://start.spring.io/
[rugó keretrendszer]: https://spring.io/
[Redis Cache with Java]: cache-java-get-started.md

<!-- IMG List -->

[AZ01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ01.png
[AZ02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ02.png
[AZ03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ03.png
[AZ04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ04.png
[AZ05]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ05.png

[SI01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI01.png
[SI02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI02.png
[SI03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI03.png
[SI04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI04.png

[RE01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE01.png
[RE02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE02.png
