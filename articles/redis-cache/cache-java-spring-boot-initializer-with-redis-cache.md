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
# <a name="how-tooconfigure-a-spring-boot-initializer-app-toouse-redis-cache"></a><span data-ttu-id="54887-104">Hogyan tooconfigure egy rugó rendszerindítás-inicializáló app toouse Redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="54887-104">How tooconfigure a Spring Boot Initializer app toouse Redis Cache</span></span>

## <a name="overview"></a><span data-ttu-id="54887-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="54887-105">Overview</span></span>

<span data-ttu-id="54887-106">Hello  **[rugó keretrendszer]**  nyílt forráskódú megoldás, amely segít a vállalati szintű alkalmazásokat Java fejlesztői.</span><span class="sxs-lookup"><span data-stu-id="54887-106">hello **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="54887-107">Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="54887-107">One of hello more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="54887-108">toohelp fejlesztők rugó rendszerindító az első lépései, több minta rugó rendszerindító csomagok érhetők el: <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="54887-108">toohelp developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="54887-109">Ezenkívül rugó rendszerindító hello listája toochoosing projektek, hello  **[rugó Initializr]**  a fejlesztőket az egyéni rendszerindító rugó-alkalmazások létrehozásának első lépései.</span><span class="sxs-lookup"><span data-stu-id="54887-109">In addition toochoosing from hello list of basic Spring Boot projects, hello **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="54887-110">Ez a cikk bemutatja, hogyan hello Azure portálra, majd hello Redis gyorsítótár létrehozása **rugó Initializr** toocreate egyéni alkalmazás, majd hozza létre egy Java-webalkalmazás tárolja, és lekéri az adatokat a Redis gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="54887-110">This article walks you through creating a Redis cache using hello Azure portal, then using hello **Spring Initializr** toocreate a custom application, and then creating a Java web application which stores and retrieves data using your Redis cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54887-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="54887-111">Prerequisites</span></span>

<span data-ttu-id="54887-112">a következő előfeltételek hello rendelés toofollow hello cikkben leírt lépéseket kell megadni:</span><span class="sxs-lookup"><span data-stu-id="54887-112">hello following prerequisites are required in order toofollow hello steps in this article:</span></span>

* <span data-ttu-id="54887-113">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="54887-113">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="54887-114">A [Java fejlesztői készlet (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1.7 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="54887-114">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="54887-115">[Apache Maven](http://maven.apache.org/), 3.0-s vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="54887-115">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="54887-116">Redis Cache gyorsítótár létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="54887-116">Create a Redis cache on Azure</span></span>

1. <span data-ttu-id="54887-117">Keresse meg a toohello Azure portál, <https://portal.azure.com/> hello elemet, majd **+ új**.</span><span class="sxs-lookup"><span data-stu-id="54887-117">Browse toohello Azure portal at <https://portal.azure.com/> and click hello item for **+New**.</span></span>

   ![Azure Portal][AZ01]

1. <span data-ttu-id="54887-119">Kattintson a **adatbázis**, és kattintson a **Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="54887-119">Click **Database**, and then click **Redis Cache**.</span></span>

   ![Azure Portal][AZ02]

1. <span data-ttu-id="54887-121">A hello **új Redis Cache** lapján adja meg a hello **DNS-név** a gyorsítótárhoz, majd adja meg a **előfizetés**, **erőforráscsoport**,  **Hely**, és **tarifacsomag**.</span><span class="sxs-lookup"><span data-stu-id="54887-121">On hello **New Redis Cache** page, enter hello **DNS name** for your cache, then specify your **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span> <span data-ttu-id="54887-122">Ezek a beállítások megadását, kattintson a **létrehozása** toocreate a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="54887-122">When you have specified these options, click **Create** toocreate your cache.</span></span>

   ![Azure Portal][AZ03]

1. <span data-ttu-id="54887-124">Miután befejeződött a gyorsítótárhoz, akkor jelenik meg az Azure megjelenik **irányítópult**, valamint hello alatt **összes erőforrás**, és **Redis Cache-gyorsítótárak** lapokat.</span><span class="sxs-lookup"><span data-stu-id="54887-124">Once your cache has been completed, you will see it listed on your Azure **Dashboard**, as well as under hello **All Resources**, and **Redis Caches** pages.</span></span> <span data-ttu-id="54887-125">Rákattinthat a bármely adott helyek tooopen hello tulajdonságok lapján a gyorsítótár a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="54887-125">You can click on your cache on any of those locations tooopen hello properties page for your cache.</span></span>

   ![Azure Portal][AZ04]

1. <span data-ttu-id="54887-127">Ha a gyorsítótár tulajdonságai hello listáját tartalmazó hello lap jelenik meg, kattintson **hívóbetűk** , és másolja a tárelérési kulcsok a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="54887-127">When hello page which contains hello list of properties for your cache is displayed, click **Access keys** and copy your access keys for your cache.</span></span>

   ![Azure Portal][AZ05]

## <a name="create-a-custom-application-using-hello-spring-initializr"></a><span data-ttu-id="54887-129">Hozzon létre egy egyéni alkalmazást rugó Initializr hello használata</span><span class="sxs-lookup"><span data-stu-id="54887-129">Create a custom application using hello Spring Initializr</span></span>

1. <span data-ttu-id="54887-130">Keresse meg a túl<https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="54887-130">Browse too<https://start.spring.io/>.</span></span>

1. <span data-ttu-id="54887-131">Adja meg, hogy toogenerate egy **Maven** a projekt **Java**, adja meg a hello **csoport** és **Aritifact** az alkalmazás nevét majd hello hivatkozásra túl**toohello teljes verzióját Switch** a hello rugó Initializr.</span><span class="sxs-lookup"><span data-stu-id="54887-131">Specify that you want toogenerate a **Maven** project with **Java**, enter hello **Group** and **Aritifact** names for your application, and then click hello link too**Switch toohello full version** of hello Spring Initializr.</span></span>

   ![Basic rugó Initializr beállítások][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="54887-133">hello rugó Initializr fogja használni a hello **csoport** és **Aritifact** nevek toocreate hello csomag neve, pl.: *com.contoso.myazuredemo*.</span><span class="sxs-lookup"><span data-stu-id="54887-133">hello Spring Initializr will use hello **Group** and **Aritifact** names toocreate hello package name; for example: *com.contoso.myazuredemo*.</span></span>
   >

1. <span data-ttu-id="54887-134">Görgessen lefelé toohello **webes** szakaszt, és a hello jelölőnégyzetet **webes**, majd görgessen lefelé toohello **NoSQL** szakaszt, és a hello jelölőnégyzetet **Redis**, majd görgessen a toohello a hello lap alján, és hello gombra túl**készítése a projekt**.</span><span class="sxs-lookup"><span data-stu-id="54887-134">Scroll down toohello **Web** section and check hello box for **Web**, then scroll down toohello **NoSQL** section and check hello box for **Redis**, then scroll toohello bottom of hello page and click hello button too**Generate Project**.</span></span>

   ![Teljes rugó Initializr beállítások][SI02]

1. <span data-ttu-id="54887-136">Amikor a rendszer kéri, töltse le a hello projekt tooa elérési útja a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="54887-136">When prompted, download hello project tooa path on your local computer.</span></span>

   ![Egyéni rugó rendszerindító-projekt letöltése][SI03]

1. <span data-ttu-id="54887-138">A helyi rendszeren hello fájlok kibontását követően az egyéni rugó rendszerindító alkalmazás készen áll a szerkesztésre lesz.</span><span class="sxs-lookup"><span data-stu-id="54887-138">After you have extracted hello files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![Egyéni rugó rendszerindító project fájlok][SI04]

## <a name="configure-your-custom-spring-boot-toouse-your-redis-cache"></a><span data-ttu-id="54887-140">Az egyéni rugó rendszerindító toouse a Redis Cache-gyorsítótár konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54887-140">Configure your custom Spring Boot toouse your Redis Cache</span></span>

1. <span data-ttu-id="54887-141">Keresse meg a hello *application.properties* hello fájlban *erőforrások* az alkalmazás könyvtárába, vagy hozzon létre hello fájlt, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="54887-141">Locate hello *application.properties* file in hello *resources* directory of your app, or create hello file if it does not already exist.</span></span>

   ![Keresse meg a hello application.properties fájl][RE01]

1. <span data-ttu-id="54887-143">Nyissa meg hello *application.properties* fájlt egy szövegszerkesztőben, és adja hozzá a következő sorokat toohello fájl hello, és cserélje le a hello mintaértékek hello megfelelő tulajdonságokkal a gyorsítótárból:</span><span class="sxs-lookup"><span data-stu-id="54887-143">Open hello *application.properties* file in a text editor, and add hello following lines toohello file, and replace hello sample values with hello appropriate properties from your cache:</span></span>

   ```yaml
   # Specify hello DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify hello port for your Redis cache.
   spring.redis.port=6380

   # Specify hello access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Hello application.properties fájl szerkesztése][RE02]

1. <span data-ttu-id="54887-145">Mentse és zárja be a hello *application.properties* fájlt.</span><span class="sxs-lookup"><span data-stu-id="54887-145">Save and close hello *application.properties* file.</span></span>

1. <span data-ttu-id="54887-146">Hozza létre a *vezérlő* alatt hello forrásmappát a csomag; például:</span><span class="sxs-lookup"><span data-stu-id="54887-146">Create a folder named *controller* under hello source folder for your package; for example:</span></span>

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   <span data-ttu-id="54887-147">– vagy –</span><span class="sxs-lookup"><span data-stu-id="54887-147">-or-</span></span>

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. <span data-ttu-id="54887-148">Hozzon létre egy új fájlt *HelloController.java* a hello *vezérlő* mappát.</span><span class="sxs-lookup"><span data-stu-id="54887-148">Create a new file named *HelloController.java* in hello *controller* folder.</span></span> <span data-ttu-id="54887-149">Nyissa meg a hello fájlt egy szövegszerkesztőben, és adja hozzá a következő kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="54887-149">Open hello file in a text editor and add hello following code tooit:</span></span>

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
   
   <span data-ttu-id="54887-150">Ha szüksége lesz tooreplace `com.contoso.myazuredemo` hello csomag névvel a projekthez.</span><span class="sxs-lookup"><span data-stu-id="54887-150">Where you will need tooreplace `com.contoso.myazuredemo` with hello package name for your project.</span></span>

1. <span data-ttu-id="54887-151">Mentse és zárja be a hello *HelloController.java* fájlt.</span><span class="sxs-lookup"><span data-stu-id="54887-151">Save and close hello *HelloController.java* file.</span></span>

1. <span data-ttu-id="54887-152">A rugó rendszerindító alkalmazás Maven létrehozása és futtatása. Példa:</span><span class="sxs-lookup"><span data-stu-id="54887-152">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="54887-153">Keresse meg webböngészővel toohttp://localhost:8080 hello webes alkalmazás tesztelése, vagy használja a következő példa, ha van elérhető curl hello hello szintaxist:</span><span class="sxs-lookup"><span data-stu-id="54887-153">Test hello web app by browsing toohttp://localhost:8080 using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="54887-154">Megtekintheti az hello "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="54887-154">You should see hello "Hello World!"</span></span> <span data-ttu-id="54887-155">üzenet tartományvezérlőről a minta jelenik meg, amely dinamikusan lekérésére a Redis-gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="54887-155">message from your sample controller displayed, which is being retrieved dynamically from your Redis cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54887-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54887-156">Next steps</span></span>

<span data-ttu-id="54887-157">Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="54887-157">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="54887-158">A rugó rendszerindító alkalmazás toohello Azure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="54887-158">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [<span data-ttu-id="54887-159">A rugó rendszerindító alkalmazást futtat egy Kubernetes hello Azure Tárolószolgáltatási fürt</span><span class="sxs-lookup"><span data-stu-id="54887-159">Running a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="54887-160">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="54887-160">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="54887-161">További információ a kezdeti Redis Cache használatának az Azure, a Java című [hogyan toouse Azure Redis Cache-gyorsítótár Java][Redis Cache with Java].</span><span class="sxs-lookup"><span data-stu-id="54887-161">For more information about getting started using Redis Cache with Java on Azure, see [How toouse Azure Redis Cache with Java][Redis Cache with Java].</span></span>

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
