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
# <a name="using-java-command-line-app-tooaccess-an-api-with-azure-ad"></a><span data-ttu-id="5ad50-103">Java parancssori alkalmazás tooAccess az API használatával az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="5ad50-103">Using Java Command Line App tooAccess An API with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="5ad50-104">Az Azure AD egyszerű és magától értetődő toooutsource teszi a webalkalmazás az Identitáskezelés, így az egyszeri bejelentkezés és kijelentkezés, az csak néhány sornyi kódot.</span><span class="sxs-lookup"><span data-stu-id="5ad50-104">Azure AD makes it simple and straightforward toooutsource your web app's identity management, providing single sign-in and sign-out with only a few lines of code.</span></span>  <span data-ttu-id="5ad50-105">A Java-webalkalmazások Ez elvégezhető a Microsoft hello közösségi szerkesztésű ADAL4J végrehajtásának használatával.</span><span class="sxs-lookup"><span data-stu-id="5ad50-105">In Java web apps, you can accomplish this using Microsoft's implementation of hello community-driven ADAL4J.</span></span>

  <span data-ttu-id="5ad50-106">Itt a ADAL4J fogjuk használni:</span><span class="sxs-lookup"><span data-stu-id="5ad50-106">Here we'll use ADAL4J to:</span></span>

* <span data-ttu-id="5ad50-107">Bejelentkezési hello felhasználói hello alkalmazásba az Azure AD hello identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="5ad50-107">Sign hello user into hello app using Azure AD as hello identity provider.</span></span>
* <span data-ttu-id="5ad50-108">Hello felhasználói információkat jelenítsen meg.</span><span class="sxs-lookup"><span data-stu-id="5ad50-108">Display some information about hello user.</span></span>
* <span data-ttu-id="5ad50-109">Bejelentkezési hello felhasználói hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="5ad50-109">Sign hello user out of hello app.</span></span>

<span data-ttu-id="5ad50-110">Ennek rendelés toodo, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="5ad50-110">In order toodo this, you'll need to:</span></span>

1. <span data-ttu-id="5ad50-111">Alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="5ad50-111">Register an application with Azure AD</span></span>
2. <span data-ttu-id="5ad50-112">Állítsa be az alkalmazás toouse hello ADAL4J könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5ad50-112">Set up your app toouse hello ADAL4J library.</span></span>
3. <span data-ttu-id="5ad50-113">Hello ADAL4J könyvtár tooissue bejelentkezés és kijelentkezési kérések tooAzure AD használatára.</span><span class="sxs-lookup"><span data-stu-id="5ad50-113">Use hello ADAL4J library tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="5ad50-114">Nyomtassa ki hello felhasználó adatait.</span><span class="sxs-lookup"><span data-stu-id="5ad50-114">Print out data about hello user.</span></span>

<span data-ttu-id="5ad50-115">elindult, tooget [hello app vázat letöltése](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="5ad50-115">tooget started, [download hello app skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) or [download hello completed sample](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span></span>  <span data-ttu-id="5ad50-116">Biztosítani kell az Azure AD-bérlő mely tooregister a az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5ad50-116">You'll also need an Azure AD tenant in which tooregister your application.</span></span>  <span data-ttu-id="5ad50-117">Ha Ön nem rendelkezik ilyennel, [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="5ad50-117">If you don't have one already, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1--register-an-application-with-azure-ad"></a><span data-ttu-id="5ad50-118">1.  Alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="5ad50-118">1.  Register an Application with Azure AD</span></span>
<span data-ttu-id="5ad50-119">tooenable a tooauthenticate felhasználók, először tooregister egy új alkalmazás az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="5ad50-119">tooenable your app tooauthenticate users, you'll first need tooregister a new application in your tenant.</span></span>

1. <span data-ttu-id="5ad50-120">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5ad50-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="5ad50-121">Hello felső sávon, kattintson a fiókjában, és a hello **Directory** menüben válassza ki, hol kívánja tooregister, az alkalmazás hello Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5ad50-121">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="5ad50-122">Kattintson a **több szolgáltatások** a bal oldali navigációs hello, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5ad50-122">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="5ad50-123">Kattintson a **App regisztrációk** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5ad50-123">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="5ad50-124">Hello utasításokat követve, és hozzon létre egy új **webalkalmazás és/vagy WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="5ad50-124">Follow hello prompts and create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="5ad50-125">Hello **neve** hello az alkalmazás ismerteti, az alkalmazás tooend-felhasználók</span><span class="sxs-lookup"><span data-stu-id="5ad50-125">hello **name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="5ad50-126">Hello **bejelentkezési URL-cím** hello alap URL-cím az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5ad50-126">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="5ad50-127">hello vázat alapértelmezett érték a `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="5ad50-127">hello skeleton's default is `http://localhost:8080/adal4jsample/`.</span></span>
6. <span data-ttu-id="5ad50-128">Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="5ad50-128">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="5ad50-129">Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="5ad50-129">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="5ad50-130">A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése.</span><span class="sxs-lookup"><span data-stu-id="5ad50-130">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="5ad50-131">Hello **App ID URI** az alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5ad50-131">hello **App ID URI** is a unique identifier for your application.</span></span>  <span data-ttu-id="5ad50-132">hello konvenció: toouse `https://<tenant-domain>/<app-name>`, pl. `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="5ad50-132">hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `http://localhost:8080/adal4jsample/`.</span></span>

<span data-ttu-id="5ad50-133">Egyszer hello portálon az alkalmazás létrehozása egy **kulcs** a hello **beállítások** lapon az alkalmazáshoz, és másolja le.</span><span class="sxs-lookup"><span data-stu-id="5ad50-133">Once in hello portal for your app create a **Key** from hello **Settings** page for your application and copy it down.</span></span>  <span data-ttu-id="5ad50-134">Erre hamarosan szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="5ad50-134">You will need it shortly.</span></span>

## <a name="2-set-up-your-app-toouse-adal4j-library-and-prerequisites-using-maven"></a><span data-ttu-id="5ad50-135">2. Az alkalmazás toouse ADAL4J könyvtár és Maven használatával Előfeltételek beállítása</span><span class="sxs-lookup"><span data-stu-id="5ad50-135">2. Set up your app toouse ADAL4J library and prerequisites using Maven</span></span>
<span data-ttu-id="5ad50-136">Itt konfigurálását végezzük el ADAL4J toouse hello OpenID Connect hitelesítési protokoll.</span><span class="sxs-lookup"><span data-stu-id="5ad50-136">Here, we'll configure ADAL4J toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="5ad50-137">ADAL4J kell használt tooissue be- és kijelentkezési kérések, hello felhasználói munkamenet kezeli, és többek között a hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5ad50-137">ADAL4J will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

* <span data-ttu-id="5ad50-138">A projekt gyökérkönyvtárában hello, megnyitni vagy létrehozni `pom.xml` , és keresse meg a hello `// TODO: provide dependencies for Maven` és cserélje le a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5ad50-138">In hello root directory of your project, open/create `pom.xml` and locate hello `// TODO: provide dependencies for Maven` and replace with hello following:</span></span>

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



## <a name="3-create-hello-java-publicclient-file"></a><span data-ttu-id="5ad50-139">3. Hello Java PublicClient fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ad50-139">3. Create hello Java PublicClient file</span></span>
<span data-ttu-id="5ad50-140">A fentiek alapján fogjuk használni hello bejelentkezett felhasználó hello Graph API tooget adatait.</span><span class="sxs-lookup"><span data-stu-id="5ad50-140">As indicated above, we will be using hello Graph API tooget data about hello logged in user.</span></span> <span data-ttu-id="5ad50-141">Ez az USA könnyen toobe azt létrehoznia mindkét egy fájl toorepresent egy **könyvtárobjektum** és az egyes fájl toorepresent hello **felhasználói** , hogy hello Java OO mintáját is használható.</span><span class="sxs-lookup"><span data-stu-id="5ad50-141">For this toobe easy for us we should create both a file toorepresent a **Directory Object** and an individual file toorepresent hello **User** so that hello OO pattern of Java can be used.</span></span>

* <span data-ttu-id="5ad50-142">Hozzon létre egy nevű fájlt `DirectoryObject.java` amely toostore bármely DirectoryObject (úgy is látja szabad toouse ezt később más diagramhoz lekérdezések a következőkre) vonatkozó alapvető adatokat fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="5ad50-142">Create a file called `DirectoryObject.java` which we will use toostore basic data about any DirectoryObject (you can feel free toouse this later for any other Graph Queries you may do).</span></span> <span data-ttu-id="5ad50-143">Akkor is kivágás-beillesztés ezt az alábbi lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="5ad50-143">You can cut/paste this from below:</span></span>

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


## <a name="compile-and-run-hello-sample"></a><span data-ttu-id="5ad50-144">Fordítsa le és futtasson hello mintát</span><span class="sxs-lookup"><span data-stu-id="5ad50-144">Compile and run hello sample</span></span>
<span data-ttu-id="5ad50-145">Vissza kimenő tooyour gyökérkönyvtár módosítsa, majd futtassa a következő parancs toobuild hello minta csak állapotba használata hello `maven`.</span><span class="sxs-lookup"><span data-stu-id="5ad50-145">Change back out tooyour root directory and run hello following command toobuild hello sample you just put together using `maven`.</span></span> <span data-ttu-id="5ad50-146">Ez a beállítás használja hello `pom.xml` függőségek megírt fájlt.</span><span class="sxs-lookup"><span data-stu-id="5ad50-146">This will use hello `pom.xml` file you wrote for dependencies.</span></span>

`$ mvn package`

<span data-ttu-id="5ad50-147">Most már rendelkeznie kell egy `adal4jsample.war` fájlt a `/targets` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5ad50-147">You should now have a `adal4jsample.war` file in your `/targets` directory.</span></span> <span data-ttu-id="5ad50-148">Előfordulhat, hogy telepíteni, amely a Tomcat tárolóban, és látogasson el a hello URL-címe</span><span class="sxs-lookup"><span data-stu-id="5ad50-148">You may deploy that in your Tomcat container and visit hello URL</span></span> 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> <span data-ttu-id="5ad50-149">Nagyon könnyen toodeploy hello legújabb Tomcat kiszolgálókkal WAR.</span><span class="sxs-lookup"><span data-stu-id="5ad50-149">It is very easy toodeploy a WAR with hello latest Tomcat servers.</span></span> <span data-ttu-id="5ad50-150">Egyszerűen nyissa meg a túl`http://localhost:8080/manager/` és feltöltésével kapcsolatos hello utasítások a "adal4jsample.war" fájl.</span><span class="sxs-lookup"><span data-stu-id="5ad50-150">Simply navigate too`http://localhost:8080/manager/` and follow hello instructions on uploading your ``adal4jsample.war\` file.</span></span> <span data-ttu-id="5ad50-151">A következőket hajtja végre autodeploy meg hello megfelelő végponttal.</span><span class="sxs-lookup"><span data-stu-id="5ad50-151">It will autodeploy for you with hello correct endpoint.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="5ad50-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ad50-152">Next Steps</span></span>
<span data-ttu-id="5ad50-153">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="5ad50-153">Congratulations!</span></span> <span data-ttu-id="5ad50-154">Ön most Java-alkalmazás, amely rendelkezik hello képességét tooauthenticate felhasználók, biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja, és működik alapszintű hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5ad50-154">You now have a working Java application that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="5ad50-155">Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.</span><span class="sxs-lookup"><span data-stu-id="5ad50-155">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>

<span data-ttu-id="5ad50-156">Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [is letöltheti a .zip Itt](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), vagy a Githubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="5ad50-156">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

