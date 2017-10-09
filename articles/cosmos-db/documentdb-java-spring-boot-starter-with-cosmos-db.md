---
title: "aaaHow toouse hello rugó rendszerindító alapszintű egy Azure Cosmos DB DocumentDB API-hoz"
description: "Ismerje meg, hogyan tooconfigure alkalmazás létre hello rugó rendszerindítás-inicializáló a hello Azure Cosmos DB DocumentDB API."
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Rugó, rugó rendszerindító Starter, Cosmos DB"
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/08/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: a2c6de678f850676cb2887e224e5c12950db0e53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="e183c-104">Hogyan toouse hello rugó rendszerindító alapszintű Azure Cosmos DB DocumentDB API-hoz</span><span class="sxs-lookup"><span data-stu-id="e183c-104">How toouse hello Spring Boot Starter with Azure Cosmos DB DocumentDB API</span></span>

## <a name="overview"></a><span data-ttu-id="e183c-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e183c-105">Overview</span></span>

<span data-ttu-id="e183c-106">Hello  **[rugó keretrendszer]**  egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="e183c-106">hello **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="e183c-107">Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e183c-107">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="e183c-108">toohelp fejlesztők rugó rendszerindító az első lépései, több minta rugó rendszerindító csomagok érhetők el: <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="e183c-108">toohelp developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="e183c-109">Ezenkívül rugó rendszerindító hello listája toochoosing projektek, hello  **[rugó Initializr]**  a fejlesztőket az egyéni rendszerindító rugó-alkalmazások létrehozásának első lépései.</span><span class="sxs-lookup"><span data-stu-id="e183c-109">In addition toochoosing from hello list of basic Spring Boot projects, hello **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="e183c-110">Azure Cosmos-adatbázis egy olyan globálisan elosztott adatbázis-szolgáltatás, amely lehetővé teszi a fejlesztők toowork számos szabványos API-k, például a DocumentDB MongoDB, Graph vagy tábla API-k segítségével adatokkal.</span><span class="sxs-lookup"><span data-stu-id="e183c-110">Azure Cosmos DB is a globally-distributed database service that allows developers toowork with data using a variety of standard APIs, such as DocumentDB, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="e183c-111">A Microsoft rugó rendszerindító alapszintű lehetővé teszi, hogy a fejlesztők toouse rugó rendszerindító alkalmazásokat, amelyek könnyen integrálhatja Azure Cosmos DB DocumentDB API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="e183c-111">Microsoft's Spring Boot Starter enables developers toouse Spring Boot applications that easily integrate with Azure Cosmos DB by using DocumentDB APIs.</span></span>

<span data-ttu-id="e183c-112">Ez a cikk bemutatja, hogy egy Azure Cosmos DB hello Azure-portál használatával, majd hello segítségével létrehozása **rugó Initializr** toocreate egy egyéni java-alkalmazást, majd adja hozzá a hello rugó rendszerindító alapszintű funkciókat tooyour egyéni az alkalmazásadatok toostore és olvashatók be adatokat az Azure Cosmos DB hello DocumentDB API használatával.</span><span class="sxs-lookup"><span data-stu-id="e183c-112">This article demonstrates creating an Azure Cosmos DB using hello Azure portal, then using hello **Spring Initializr** toocreate a custom java application, and then add hello Spring Boot Starter functionality tooyour custom application toostore data in and retrieve data from your Azure Cosmos DB by using hello DocumentDB API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e183c-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e183c-113">Prerequisites</span></span>

<span data-ttu-id="e183c-114">a következő előfeltételek hello rendelés toofollow hello cikkben leírt lépéseket kell megadni:</span><span class="sxs-lookup"><span data-stu-id="e183c-114">hello following prerequisites are required in order toofollow hello steps in this article:</span></span>

* <span data-ttu-id="e183c-115">Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].</span><span class="sxs-lookup"><span data-stu-id="e183c-115">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="e183c-116">A [Java fejlesztői készlet (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1.7 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="e183c-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="e183c-117">[Apache Maven](http://maven.apache.org/), 3.0-s vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="e183c-117">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-hello-azure-portal"></a><span data-ttu-id="e183c-118">Hozzon létre egy Azure Cosmos DB hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="e183c-118">Create an Azure Cosmos DB by using hello Azure portal</span></span>

1. <span data-ttu-id="e183c-119">Keresse meg a toohello Azure portál, <https://portal.azure.com/> kattintson **+ új**.</span><span class="sxs-lookup"><span data-stu-id="e183c-119">Browse toohello Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure Portal][AZ01]

1. <span data-ttu-id="e183c-121">Kattintson a **adatbázisok**, és kattintson a **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="e183c-121">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure Portal][AZ02]

1. <span data-ttu-id="e183c-123">A hello **Azure Cosmos DB** lapján adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="e183c-123">On hello **Azure Cosmos DB** page, enter hello following information:</span></span>

   * <span data-ttu-id="e183c-124">Adjon meg egy egyedi **azonosító**, amely, az adatbázis URI hello fogja használni.</span><span class="sxs-lookup"><span data-stu-id="e183c-124">Enter a unique **ID**, which you will use as hello URI for your database.</span></span> <span data-ttu-id="e183c-125">Például: *wingtiptoysdata.documents.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="e183c-125">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="e183c-126">Válasszon **SQL (Document DB rendszerbe)** hello API számára.</span><span class="sxs-lookup"><span data-stu-id="e183c-126">Choose **SQL (Document DB)** for hello API.</span></span>
   * <span data-ttu-id="e183c-127">Válassza ki a hello **előfizetés** toouse szeretné az adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="e183c-127">Choose hello **Subscription** you want toouse for your database.</span></span>
   * <span data-ttu-id="e183c-128">Adja meg, hogy új toocreate **erőforráscsoport** az adatbázishoz, vagy válasszon egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e183c-128">Specify whether toocreate a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="e183c-129">Adja meg a hello **hely** az adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="e183c-129">Specify hello **Location** for your database.</span></span>
   
   <span data-ttu-id="e183c-130">Ezek a beállítások megadását, kattintson a **létrehozása** toocreate az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e183c-130">When you have specified these options, click **Create** toocreate your database.</span></span>

   ![Azure Portal][AZ03]

1. <span data-ttu-id="e183c-132">Az adatbázis létrehozásakor az szerepel az Azure **irányítópult**, valamint hello alatt **összes erőforrás** és **Azure Cosmos DB** lapokat.</span><span class="sxs-lookup"><span data-stu-id="e183c-132">When your database has been created, it is listed on your Azure **Dashboard**, as well as under hello **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="e183c-133">Az adatbázis bármely adott helyek tooopen hello tulajdonságai lapon kattintson a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="e183c-133">You can click on your database on any of those locations tooopen hello properties page for your cache.</span></span>

   ![Azure Portal][AZ04]

1. <span data-ttu-id="e183c-135">Az adatbázis hello tulajdonságai lapon megjelenített, **hívóbetűk** és a URI és hozzáférési kulcsokkal az adatbázis másolása; használhatja ezeket az értékeket az rugó rendszerindító alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e183c-135">When hello properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure Portal][AZ05]

## <a name="create-a-simple-spring-boot-application-with-hello-spring-initializr"></a><span data-ttu-id="e183c-137">Hozzon létre egy egyszerű rugó rendszerindító alkalmazást hello rugó Initializr</span><span class="sxs-lookup"><span data-stu-id="e183c-137">Create a simple Spring Boot application with hello Spring Initializr</span></span>

1. <span data-ttu-id="e183c-138">Keresse meg a túl<https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="e183c-138">Browse too<https://start.spring.io/>.</span></span>

1. <span data-ttu-id="e183c-139">Adja meg, hogy toogenerate egy **Maven** a projekt **Java**, adja meg a hello **csoport** és **összetevő** az alkalmazás nevét és majd gombra hello túl**készítése a projekt**.</span><span class="sxs-lookup"><span data-stu-id="e183c-139">Specify that you want toogenerate a **Maven** project with **Java**, enter hello **Group** and **Artifact** names for your application, and then click hello button too**Generate Project**.</span></span>

   ![Basic rugó Initializr beállítások][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="e183c-141">hello rugó Initializr használ hello **csoport** és **összetevő** nevek toocreate hello csomag neve, pl.: *com.example.wintiptoys*.</span><span class="sxs-lookup"><span data-stu-id="e183c-141">hello Spring Initializr uses hello **Group** and **Artifact** names toocreate hello package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="e183c-142">Amikor a rendszer kéri, töltse le a hello projekt tooa elérési útja a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e183c-142">When prompted, download hello project tooa path on your local computer.</span></span>

   ![Egyéni rugó rendszerindító-projekt letöltése][SI02]

1. <span data-ttu-id="e183c-144">A helyi rendszeren hello fájlok kibontását követően az egyszerű rugó rendszerindító alkalmazás készen áll a szerkesztésre lesz.</span><span class="sxs-lookup"><span data-stu-id="e183c-144">After you have extracted hello files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![Egyéni rugó rendszerindító project fájlok][SI03]

## <a name="configure-your-spring-boot-app-toouse-hello-azure-spring-boot-starter"></a><span data-ttu-id="e183c-146">A rugó rendszerindító alkalmazás toouse hello Azure rugó rendszerindító alapszintű konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e183c-146">Configure your Spring Boot app toouse hello Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="e183c-147">Keresse meg a hello *pom.xml* hello fájl az az alkalmazás; például:</span><span class="sxs-lookup"><span data-stu-id="e183c-147">Locate hello *pom.xml* file in hello directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="e183c-148">– vagy –</span><span class="sxs-lookup"><span data-stu-id="e183c-148">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![Keresse meg a pom.xml fájlt hello][PM01]

1. <span data-ttu-id="e183c-150">Nyissa meg hello *pom.xml* fájlt egy szövegszerkesztőben, és adja hozzá a következő sorokat toolist a hello `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="e183c-150">Open hello *pom.xml* file in a text editor, and add hello following lines toolist of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Hello pom.xml fájl szerkesztése][PM02]

1. <span data-ttu-id="e183c-152">Mentse és zárja be a hello *pom.xml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e183c-152">Save and close hello *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-toouse-your-azure-cosmos-db"></a><span data-ttu-id="e183c-153">A rugó rendszerindító alkalmazás toouse konfigurálása az Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e183c-153">Configure your Spring Boot app toouse your Azure Cosmos DB</span></span>

1. <span data-ttu-id="e183c-154">Keresse meg a hello *application.properties* hello fájlban *erőforrások* az alkalmazás könyvtárába, például:</span><span class="sxs-lookup"><span data-stu-id="e183c-154">Locate hello *application.properties* file in hello *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="e183c-155">– vagy –</span><span class="sxs-lookup"><span data-stu-id="e183c-155">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Keresse meg a hello application.properties fájl][RE01]

1. <span data-ttu-id="e183c-157">Nyissa meg hello *application.properties* fájlt egy szövegszerkesztőben, és adja hozzá a következő sorokat toohello fájl hello és hello mintaértékek cserélje le az adatbázis megfelelő tulajdonságainak hello:</span><span class="sxs-lookup"><span data-stu-id="e183c-157">Open hello *application.properties* file in a text editor, and add hello following lines toohello file, and replace hello sample values with hello appropriate properties for your database:</span></span>

   ```yaml
   # Specify hello DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify hello access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify hello name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Hello application.properties fájl szerkesztése][RE02]

1. <span data-ttu-id="e183c-159">Mentse és zárja be a hello *application.properties* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e183c-159">Save and close hello *application.properties* file.</span></span>

## <a name="add-sample-code-tooimplement-basic-database-functionality"></a><span data-ttu-id="e183c-160">Minta kód tooimplement alapvető adatbázisokkal kapcsolatos funkciók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e183c-160">Add sample code tooimplement basic database functionality</span></span>

<span data-ttu-id="e183c-161">Ebben a szakaszban hoz létre két Java-osztályok a felhasználói adatok tárolásához, és majd módosítja a fő alkalmazás osztály toocreate hello felhasználói osztály egy példányát, és mentse tooyour adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e183c-161">In this section you create two Java classes for storing user data, and then you modify your main application class toocreate an instance of hello user class and save it tooyour database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="e183c-162">Adja meg a felhasználói adatok tárolásához alapvető osztály</span><span class="sxs-lookup"><span data-stu-id="e183c-162">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="e183c-163">Hozzon létre egy új fájlt *User.java* a hello a fő alkalmazásfájl Java könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="e183c-163">Create a new file named *User.java* in hello same directory as your main application Java file.</span></span>

1. <span data-ttu-id="e183c-164">Nyissa meg hello *User.java* fájlt egy szövegszerkesztőben, és adja hozzá a következő hello sorok toohello fájl toodefine egy általános felhasználói osztály, amely tárolja, és az adatbázis értékek lekérését:</span><span class="sxs-lookup"><span data-stu-id="e183c-164">Open hello *User.java* file in a text editor, and add hello following lines toohello file toodefine a generic user class that stores and retrieve values in your database:</span></span>

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. <span data-ttu-id="e183c-165">Mentse és zárja be a hello *User.java* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e183c-165">Save and close hello *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="e183c-166">A tárház adatillesztő meghatározása</span><span class="sxs-lookup"><span data-stu-id="e183c-166">Define a data repository interface</span></span>

1. <span data-ttu-id="e183c-167">Hozzon létre egy új fájlt *UserRepository.java* a hello a fő alkalmazásfájl Java könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="e183c-167">Create a new file named *UserRepository.java* in hello same directory as your main application Java file.</span></span>

1. <span data-ttu-id="e183c-168">Nyissa meg hello *UserRepository.java* fájlt egy szövegszerkesztőben, és adja hozzá a következő hello sorok toohello fájl toodefine hello alapértelmezett DocumentDB tárház felület egy felhasználó tárház felület:</span><span class="sxs-lookup"><span data-stu-id="e183c-168">Open hello *UserRepository.java* file in a text editor, and add hello following lines toohello file toodefine a user repository interface that extends hello default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="e183c-169">Mentse és zárja be a hello *UserRepository.java* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e183c-169">Save and close hello *UserRepository.java* file.</span></span>

### <a name="modify-hello-main-application-class"></a><span data-ttu-id="e183c-170">Hello fő alkalmazásosztály módosítása</span><span class="sxs-lookup"><span data-stu-id="e183c-170">Modify hello main application class</span></span>

1. <span data-ttu-id="e183c-171">Keresse meg a hello fő Java alkalmazásfájl hello csomag könyvtárában az alkalmazás; Példa:</span><span class="sxs-lookup"><span data-stu-id="e183c-171">Locate hello main application Java file in hello package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="e183c-172">– vagy –</span><span class="sxs-lookup"><span data-stu-id="e183c-172">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Keresse meg a Java hello alkalmazásfájl.][JV01]

1. <span data-ttu-id="e183c-174">Nyissa meg a hello fő alkalmazás Java fájlt egy szövegszerkesztőben, és adja hozzá a következő sorokat toohello fájl hello:</span><span class="sxs-lookup"><span data-stu-id="e183c-174">Open hello main application Java file in a text editor, and add hello following lines toohello file:</span></span>

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. <span data-ttu-id="e183c-175">Mentse, és zárja be a hello fő alkalmazás Java fájlt.</span><span class="sxs-lookup"><span data-stu-id="e183c-175">Save and close hello main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="e183c-176">Hozza létre, és az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="e183c-176">Build and test your app</span></span>

1. <span data-ttu-id="e183c-177">Nyisson meg egy parancssort, és módosítsa a toohello könyvtármappát ahol a *pom.xml* -fájl, például:</span><span class="sxs-lookup"><span data-stu-id="e183c-177">Open a command prompt and change directory toohello folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="e183c-178">– vagy –</span><span class="sxs-lookup"><span data-stu-id="e183c-178">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="e183c-179">A rugó rendszerindító alkalmazás Maven létrehozása és futtatása. Példa:</span><span class="sxs-lookup"><span data-stu-id="e183c-179">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="e183c-180">Az alkalmazás több futásidejű üzenet jelenik meg, és hello üzenet `User: testFirstName testLastName` jelenik meg, hogy értékek sikeresen tárolt és az adatbázisból beolvasott tooindicate.</span><span class="sxs-lookup"><span data-stu-id="e183c-180">Your application will display several runtime messages, and you should see hello message `User: testFirstName testLastName` displayed tooindicate that values have been successfully stored and retrieved from your database.</span></span>

   ![Hello alkalmazás sikeres kimenete][JV02]

1. <span data-ttu-id="e183c-182">Választható lehetőség: Használható hello Azure portál tooview hello tartalmát a Azure Cosmos DB hello tulajdonságai lapon az adatbázis számára kattintva **dokumentumkezelő**, és jelölje be, és megjelenik hello lista tooview hello elemet a tartalom.</span><span class="sxs-lookup"><span data-stu-id="e183c-182">OPTIONAL: You can use hello Azure portal tooview hello contents of your Azure Cosmos DB from hello properties page for your database by clicking  **Document Explorer**, and then selecting and item from hello displayed list tooview hello contents.</span></span>

   ![Hello dokumentumkezelő tooview az adatok használata][JV03]

## <a name="next-steps"></a><span data-ttu-id="e183c-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e183c-184">Next steps</span></span>

<span data-ttu-id="e183c-185">Azure Cosmos DB és Java használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="e183c-185">For more information about using Azure Cosmos DB and Java, see hello following articles:</span></span>

* <span data-ttu-id="e183c-186">[Az Azure Cosmos DB dokumentáció].</span><span class="sxs-lookup"><span data-stu-id="e183c-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="e183c-187">[Azure Cosmos DB: Egy DocumentDB API-alkalmazást Java Build és hello Azure-portálon][Build a DocumentDB API app with Java]</span><span class="sxs-lookup"><span data-stu-id="e183c-187">[Azure Cosmos DB: Build a DocumentDB API app with Java and hello Azure portal][Build a DocumentDB API app with Java]</span></span>

<span data-ttu-id="e183c-188">Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="e183c-188">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="e183c-189">Rugó rendszerindító DocumenDB alapszintű az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="e183c-189">Spring Boot DocumenDB Starter for Azure</span></span>](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [<span data-ttu-id="e183c-190">A rugó rendszerindító alkalmazás toohello Azure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="e183c-190">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [<span data-ttu-id="e183c-191">A rugó rendszerindító alkalmazást futtat egy Kubernetes hello Azure Tárolószolgáltatási fürt</span><span class="sxs-lookup"><span data-stu-id="e183c-191">Running a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="e183c-192">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="e183c-192">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Az Azure Cosmos DB dokumentáció]: /azure/cosmos-db/
[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[ingyenes Azure-fiókot]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN-előfizetői előnyeit]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó Initializr]: https://start.spring.io/
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ01.png
[AZ02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ02.png
[AZ03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ03.png
[AZ04]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ04.png
[AZ05]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ05.png

[SI01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI01.png
[SI02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI02.png
[SI03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI03.png

[RE01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE01.png
[RE02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE02.png

[PM01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM01.png
[PM02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM02.png

[JV01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV01.png
[JV02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV02.png
[JV03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV03.png
