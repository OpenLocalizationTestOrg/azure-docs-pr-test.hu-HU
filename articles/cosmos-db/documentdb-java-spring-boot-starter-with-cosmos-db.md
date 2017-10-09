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
# <a name="how-toouse-hello-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a>Hogyan toouse hello rugó rendszerindító alapszintű Azure Cosmos DB DocumentDB API-hoz

## <a name="overview"></a>Áttekintés

Hello  **[rugó keretrendszer]**  egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat. Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása. toohelp fejlesztők rugó rendszerindító az első lépései, több minta rugó rendszerindító csomagok érhetők el: <https://github.com/spring-guides/>. Ezenkívül rugó rendszerindító hello listája toochoosing projektek, hello  **[rugó Initializr]**  a fejlesztőket az egyéni rendszerindító rugó-alkalmazások létrehozásának első lépései.

Azure Cosmos-adatbázis egy olyan globálisan elosztott adatbázis-szolgáltatás, amely lehetővé teszi a fejlesztők toowork számos szabványos API-k, például a DocumentDB MongoDB, Graph vagy tábla API-k segítségével adatokkal. A Microsoft rugó rendszerindító alapszintű lehetővé teszi, hogy a fejlesztők toouse rugó rendszerindító alkalmazásokat, amelyek könnyen integrálhatja Azure Cosmos DB DocumentDB API-k használatával.

Ez a cikk bemutatja, hogy egy Azure Cosmos DB hello Azure-portál használatával, majd hello segítségével létrehozása **rugó Initializr** toocreate egy egyéni java-alkalmazást, majd adja hozzá a hello rugó rendszerindító alapszintű funkciókat tooyour egyéni az alkalmazásadatok toostore és olvashatók be adatokat az Azure Cosmos DB hello DocumentDB API használatával.

## <a name="prerequisites"></a>Előfeltételek

a következő előfeltételek hello rendelés toofollow hello cikkben leírt lépéseket kell megadni:

* Azure-előfizetés; Ha még nem rendelkezik Azure-előfizetéssel, aktiválhatja a [MSDN-előfizetői előnyeit] vagy regisztráljon egy [ingyenes Azure-fiókot].

* A [Java fejlesztői készlet (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1.7 vagy újabb verziója.

* [Apache Maven](http://maven.apache.org/), 3.0-s vagy újabb verziója.

## <a name="create-an-azure-cosmos-db-by-using-hello-azure-portal"></a>Hozzon létre egy Azure Cosmos DB hello Azure-portál használatával

1. Keresse meg a toohello Azure portál, <https://portal.azure.com/> kattintson **+ új**.

   ![Azure Portal][AZ01]

1. Kattintson a **adatbázisok**, és kattintson a **Azure Cosmos DB**.

   ![Azure Portal][AZ02]

1. A hello **Azure Cosmos DB** lapján adja meg a következő információ hello:

   * Adjon meg egy egyedi **azonosító**, amely, az adatbázis URI hello fogja használni. Például: *wingtiptoysdata.documents.azure.com*.
   * Válasszon **SQL (Document DB rendszerbe)** hello API számára.
   * Válassza ki a hello **előfizetés** toouse szeretné az adatbázis számára.
   * Adja meg, hogy új toocreate **erőforráscsoport** az adatbázishoz, vagy válasszon egy meglévő erőforráscsoportot.
   * Adja meg a hello **hely** az adatbázis számára.
   
   Ezek a beállítások megadását, kattintson a **létrehozása** toocreate az adatbázis.

   ![Azure Portal][AZ03]

1. Az adatbázis létrehozásakor az szerepel az Azure **irányítópult**, valamint hello alatt **összes erőforrás** és **Azure Cosmos DB** lapokat. Az adatbázis bármely adott helyek tooopen hello tulajdonságai lapon kattintson a gyorsítótárhoz.

   ![Azure Portal][AZ04]

1. Az adatbázis hello tulajdonságai lapon megjelenített, **hívóbetűk** és a URI és hozzáférési kulcsokkal az adatbázis másolása; használhatja ezeket az értékeket az rugó rendszerindító alkalmazásban.

   ![Azure Portal][AZ05]

## <a name="create-a-simple-spring-boot-application-with-hello-spring-initializr"></a>Hozzon létre egy egyszerű rugó rendszerindító alkalmazást hello rugó Initializr

1. Keresse meg a túl<https://start.spring.io/>.

1. Adja meg, hogy toogenerate egy **Maven** a projekt **Java**, adja meg a hello **csoport** és **összetevő** az alkalmazás nevét és majd gombra hello túl**készítése a projekt**.

   ![Basic rugó Initializr beállítások][SI01]

   > [!NOTE]
   >
   > hello rugó Initializr használ hello **csoport** és **összetevő** nevek toocreate hello csomag neve, pl.: *com.example.wintiptoys*.
   >

1. Amikor a rendszer kéri, töltse le a hello projekt tooa elérési útja a helyi számítógépen.

   ![Egyéni rugó rendszerindító-projekt letöltése][SI02]

1. A helyi rendszeren hello fájlok kibontását követően az egyszerű rugó rendszerindító alkalmazás készen áll a szerkesztésre lesz.

   ![Egyéni rugó rendszerindító project fájlok][SI03]

## <a name="configure-your-spring-boot-app-toouse-hello-azure-spring-boot-starter"></a>A rugó rendszerindító alkalmazás toouse hello Azure rugó rendszerindító alapszintű konfigurálása

1. Keresse meg a hello *pom.xml* hello fájl az az alkalmazás; például:

   `C:\SpringBoot\wingtiptoys\pom.xml`

   – vagy –

   `/users/example/home/wingtiptoys/pom.xml`

   ![Keresse meg a pom.xml fájlt hello][PM01]

1. Nyissa meg hello *pom.xml* fájlt egy szövegszerkesztőben, és adja hozzá a következő sorokat toolist a hello `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Hello pom.xml fájl szerkesztése][PM02]

1. Mentse és zárja be a hello *pom.xml* fájlt.

## <a name="configure-your-spring-boot-app-toouse-your-azure-cosmos-db"></a>A rugó rendszerindító alkalmazás toouse konfigurálása az Azure Cosmos DB

1. Keresse meg a hello *application.properties* hello fájlban *erőforrások* az alkalmazás könyvtárába, például:

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   – vagy –

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Keresse meg a hello application.properties fájl][RE01]

1. Nyissa meg hello *application.properties* fájlt egy szövegszerkesztőben, és adja hozzá a következő sorokat toohello fájl hello és hello mintaértékek cserélje le az adatbázis megfelelő tulajdonságainak hello:

   ```yaml
   # Specify hello DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify hello access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify hello name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Hello application.properties fájl szerkesztése][RE02]

1. Mentse és zárja be a hello *application.properties* fájlt.

## <a name="add-sample-code-tooimplement-basic-database-functionality"></a>Minta kód tooimplement alapvető adatbázisokkal kapcsolatos funkciók hozzáadása

Ebben a szakaszban hoz létre két Java-osztályok a felhasználói adatok tárolásához, és majd módosítja a fő alkalmazás osztály toocreate hello felhasználói osztály egy példányát, és mentse tooyour adatbázis.

### <a name="define-a-basic-class-for-storing-user-data"></a>Adja meg a felhasználói adatok tárolásához alapvető osztály

1. Hozzon létre egy új fájlt *User.java* a hello a fő alkalmazásfájl Java könyvtárába.

1. Nyissa meg hello *User.java* fájlt egy szövegszerkesztőben, és adja hozzá a következő hello sorok toohello fájl toodefine egy általános felhasználói osztály, amely tárolja, és az adatbázis értékek lekérését:

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

1. Mentse és zárja be a hello *User.java* fájlt.

### <a name="define-a-data-repository-interface"></a>A tárház adatillesztő meghatározása

1. Hozzon létre egy új fájlt *UserRepository.java* a hello a fő alkalmazásfájl Java könyvtárába.

1. Nyissa meg hello *UserRepository.java* fájlt egy szövegszerkesztőben, és adja hozzá a következő hello sorok toohello fájl toodefine hello alapértelmezett DocumentDB tárház felület egy felhasználó tárház felület:

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. Mentse és zárja be a hello *UserRepository.java* fájlt.

### <a name="modify-hello-main-application-class"></a>Hello fő alkalmazásosztály módosítása

1. Keresse meg a hello fő Java alkalmazásfájl hello csomag könyvtárában az alkalmazás; Példa:

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   – vagy –

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Keresse meg a Java hello alkalmazásfájl.][JV01]

1. Nyissa meg a hello fő alkalmazás Java fájlt egy szövegszerkesztőben, és adja hozzá a következő sorokat toohello fájl hello:

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

1. Mentse, és zárja be a hello fő alkalmazás Java fájlt.

## <a name="build-and-test-your-app"></a>Hozza létre, és az alkalmazás tesztelése

1. Nyisson meg egy parancssort, és módosítsa a toohello könyvtármappát ahol a *pom.xml* -fájl, például:

   `cd C:\SpringBoot\wingtiptoys`

   – vagy –

   `cd /users/example/home/wingtiptoys`

1. A rugó rendszerindító alkalmazás Maven létrehozása és futtatása. Példa:

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. Az alkalmazás több futásidejű üzenet jelenik meg, és hello üzenet `User: testFirstName testLastName` jelenik meg, hogy értékek sikeresen tárolt és az adatbázisból beolvasott tooindicate.

   ![Hello alkalmazás sikeres kimenete][JV02]

1. Választható lehetőség: Használható hello Azure portál tooview hello tartalmát a Azure Cosmos DB hello tulajdonságai lapon az adatbázis számára kattintva **dokumentumkezelő**, és jelölje be, és megjelenik hello lista tooview hello elemet a tartalom.

   ![Hello dokumentumkezelő tooview az adatok használata][JV03]

## <a name="next-steps"></a>Következő lépések

Azure Cosmos DB és Java használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [Az Azure Cosmos DB dokumentáció].

* [Azure Cosmos DB: Egy DocumentDB API-alkalmazást Java Build és hello Azure-portálon][Build a DocumentDB API app with Java]

Azure rugó rendszerindító alkalmazások használatával kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [Rugó rendszerindító DocumenDB alapszintű az Azure-bA](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [A rugó rendszerindító alkalmazás toohello Azure App Service telepítése](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [A rugó rendszerindító alkalmazást futtat egy Kubernetes hello Azure Tárolószolgáltatási fürt](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].

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
