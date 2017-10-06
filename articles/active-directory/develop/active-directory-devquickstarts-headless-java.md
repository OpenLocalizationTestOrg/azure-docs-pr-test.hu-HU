---
title: "első lépések AD Java parancssori aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild Java parancs, mely aláírja a felhasználók számára az API-k tooaccess sor alkalmazást."
services: active-directory
documentationcenter: java
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 51e1a8f9-6ff0-4643-a350-0ba794e26fd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 9ba1d1e794928a39ca1f091bd0e6eba57ce3d6aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-java-command-line-app-tooaccess-an-api-with-azure-ad"></a>Java parancssori alkalmazás tooAccess az API használatával az Azure ad szolgáltatással
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Az Azure AD egyszerű és magától értetődő toooutsource teszi a webalkalmazás az Identitáskezelés, így az egyszeri bejelentkezés és kijelentkezés, az csak néhány sornyi kódot.  A Java-webalkalmazások Ez elvégezhető a Microsoft hello közösségi szerkesztésű ADAL4J végrehajtásának használatával.

  Itt a ADAL4J fogjuk használni:

* Bejelentkezési hello felhasználói hello alkalmazásba az Azure AD hello identitás-szolgáltatóként.
* Hello felhasználói információkat jelenítsen meg.
* Bejelentkezési hello felhasználói hello alkalmazásból.

Ennek rendelés toodo, lesz szüksége:

1. Alkalmazás regisztrálása az Azure ad szolgáltatással
2. Állítsa be az alkalmazás toouse hello ADAL4J könyvtár.
3. Hello ADAL4J könyvtár tooissue bejelentkezés és kijelentkezési kérések tooAzure AD használatára.
4. Nyomtassa ki hello felhasználó adatait.

elindult, tooget [hello app vázat letöltése](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).  Biztosítani kell az Azure AD-bérlő mely tooregister a az alkalmazás.  Ha Ön nem rendelkezik ilyennel, [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1.  Alkalmazás regisztrálása az Azure ad szolgáltatással
tooenable a tooauthenticate felhasználók, először tooregister egy új alkalmazás az Ön bérelt szolgáltatásának.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon, kattintson a fiókjában, és a hello **Directory** menüben válassza ki, hol kívánja tooregister, az alkalmazás hello Active Directory-bérlő.
3. Kattintson a **több szolgáltatások** a bal oldali navigációs hello, és válassza a **Azure Active Directory**.
4. Kattintson a **App regisztrációk** válassza **Hozzáadás**.
5. Hello utasításokat követve, és hozzon létre egy új **webalkalmazás és/vagy WebAPI**.
  * Hello **neve** hello az alkalmazás ismerteti, az alkalmazás tooend-felhasználók
  * Hello **bejelentkezési URL-cím** hello alap URL-cím az alkalmazás.  hello vázat alapértelmezett érték a `http://localhost:8080/adal4jsample/`.
6. Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.  Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás lapján.
7. A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése. Hello **App ID URI** az alkalmazás egyedi azonosítója.  hello konvenció: toouse `https://<tenant-domain>/<app-name>`, pl. `http://localhost:8080/adal4jsample/`.

Egyszer hello portálon az alkalmazás létrehozása egy **kulcs** a hello **beállítások** lapon az alkalmazáshoz, és másolja le.  Erre hamarosan szüksége lesz.

## <a name="2-set-up-your-app-toouse-adal4j-library-and-prerequisites-using-maven"></a>2. Az alkalmazás toouse ADAL4J könyvtár és Maven használatával Előfeltételek beállítása
Itt konfigurálását végezzük el ADAL4J toouse hello OpenID Connect hitelesítési protokoll.  ADAL4J kell használt tooissue be- és kijelentkezési kérések, hello felhasználói munkamenet kezeli, és többek között a hello felhasználó adatainak beolvasása.

* A projekt gyökérkönyvtárában hello, megnyitni vagy létrehozni `pom.xml` , és keresse meg a hello `// TODO: provide dependencies for Maven` és cserélje le a következő hello:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-hello-java-publicclient-file"></a>3. Hello Java PublicClient fájl létrehozása
A fentiek alapján fogjuk használni hello bejelentkezett felhasználó hello Graph API tooget adatait. Ez az USA könnyen toobe azt létrehoznia mindkét egy fájl toorepresent egy **könyvtárobjektum** és az egyes fájl toorepresent hello **felhasználói** , hogy hello Java OO mintáját is használható.

* Hozzon létre egy nevű fájlt `DirectoryObject.java` amely toostore bármely DirectoryObject (úgy is látja szabad toouse ezt később más diagramhoz lekérdezések a következőkre) vonatkozó alapvető adatokat fogjuk használni. Akkor is kivágás-beillesztés ezt az alábbi lehetőségek közül:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


## <a name="compile-and-run-hello-sample"></a>Fordítsa le és futtasson hello mintát
Vissza kimenő tooyour gyökérkönyvtár módosítsa, majd futtassa a következő parancs toobuild hello minta csak állapotba használata hello `maven`. Ez a beállítás használja hello `pom.xml` függőségek megírt fájlt.

`$ mvn package`

Most már rendelkeznie kell egy `adal4jsample.war` fájlt a `/targets` könyvtár. Előfordulhat, hogy telepíteni, amely a Tomcat tárolóban, és látogasson el a hello URL-címe 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> Nagyon könnyen toodeploy hello legújabb Tomcat kiszolgálókkal WAR. Egyszerűen nyissa meg a túl`http://localhost:8080/manager/` és feltöltésével kapcsolatos hello utasítások a "adal4jsample.war" fájl. A következőket hajtja végre autodeploy meg hello megfelelő végponttal.
> 
> 

## <a name="next-steps"></a>Következő lépések
Gratulálunk! Ön most Java-alkalmazás, amely rendelkezik hello képességét tooauthenticate felhasználók, biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja, és működik alapszintű hello felhasználó adatainak beolvasása.  Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.

Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [is letöltheti a .zip Itt](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), vagy a Githubból is klónozhatja:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

