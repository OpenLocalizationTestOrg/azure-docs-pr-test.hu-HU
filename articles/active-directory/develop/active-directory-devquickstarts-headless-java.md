---
title: "Ismerkedés az Azure AD Java parancssori |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre egy Java parancssori alkalmazás, amely a felhasználó bejelentkezik az API-k eléréséhez."
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
ms.openlocfilehash: 91e4a7b2ac454465d5cce4948a4d5f0b542d2b55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a><span data-ttu-id="026a1-103">Az Azure AD az API-k elérésére használt Java parancssori alkalmazás</span><span class="sxs-lookup"><span data-stu-id="026a1-103">Using Java Command Line App To Access An API with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="026a1-104">Az Azure AD teszi egyszerű és magától értetődő kihelyező a webalkalmazás az Identitáskezelés, így egyetlen be- és kijelentkezési rendelkező csak néhány sornyi kódot.</span><span class="sxs-lookup"><span data-stu-id="026a1-104">Azure AD makes it simple and straightforward to outsource your web app's identity management, providing single sign-in and sign-out with only a few lines of code.</span></span>  <span data-ttu-id="026a1-105">A Java-webalkalmazások Ez elvégezhető a közösségi szerkesztésű ADAL4J Microsoft végrehajtásának használatával.</span><span class="sxs-lookup"><span data-stu-id="026a1-105">In Java web apps, you can accomplish this using Microsoft's implementation of the community-driven ADAL4J.</span></span>

  <span data-ttu-id="026a1-106">Itt a ADAL4J fogjuk használni:</span><span class="sxs-lookup"><span data-stu-id="026a1-106">Here we'll use ADAL4J to:</span></span>

* <span data-ttu-id="026a1-107">A felhasználó jelentkezzen be az alkalmazást az Azure AD az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="026a1-107">Sign the user into the app using Azure AD as the identity provider.</span></span>
* <span data-ttu-id="026a1-108">A felhasználói információkat jelenítsen meg.</span><span class="sxs-lookup"><span data-stu-id="026a1-108">Display some information about the user.</span></span>
* <span data-ttu-id="026a1-109">Jelentkezzen ki az alkalmazásból a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="026a1-109">Sign the user out of the app.</span></span>

<span data-ttu-id="026a1-110">Ehhez szüksége:</span><span class="sxs-lookup"><span data-stu-id="026a1-110">In order to do this, you'll need to:</span></span>

1. <span data-ttu-id="026a1-111">Alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="026a1-111">Register an application with Azure AD</span></span>
2. <span data-ttu-id="026a1-112">Az alkalmazás beállítása a ADAL4J könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="026a1-112">Set up your app to use the ADAL4J library.</span></span>
3. <span data-ttu-id="026a1-113">A ADAL4J könyvtár segítségével be- és kijelentkezési kérések kiállítása az Azure ad Szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="026a1-113">Use the ADAL4J library to issue sign-in and sign-out requests to Azure AD.</span></span>
4. <span data-ttu-id="026a1-114">Nyomtassa ki a felhasználóval kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="026a1-114">Print out data about the user.</span></span>

<span data-ttu-id="026a1-115">A kezdéshez [töltse le az alkalmazás vázat](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) vagy [töltse le az elkészült mintát](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="026a1-115">To get started, [download the app skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) or [download the completed sample](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span></span>  <span data-ttu-id="026a1-116">Biztosítani kell az Azure AD-bérlő, amelyben az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="026a1-116">You'll also need an Azure AD tenant in which to register your application.</span></span>  <span data-ttu-id="026a1-117">Ha Ön nem rendelkezik ilyennel, [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="026a1-117">If you don't have one already, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="1--register-an-application-with-azure-ad"></a><span data-ttu-id="026a1-118">1.  Alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="026a1-118">1.  Register an Application with Azure AD</span></span>
<span data-ttu-id="026a1-119">Ahhoz, hogy az alkalmazás a felhasználók hitelesítésére, először egy új alkalmazás regisztrálásához az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="026a1-119">To enable your app to authenticate users, you'll first need to register a new application in your tenant.</span></span>

1. <span data-ttu-id="026a1-120">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="026a1-120">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="026a1-121">A felső eszköztáron kattintson a fiókhoz, majd a a **Directory** menüben válassza ki az Active Directory-bérlőt, ahová be szeretné-e az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="026a1-121">On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.</span></span>
3. <span data-ttu-id="026a1-122">Kattintson a **több szolgáltatások** a bal oldali navigációs válassza **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="026a1-122">Click on **More Services** in the left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="026a1-123">Kattintson a **App regisztrációk** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="026a1-123">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="026a1-124">Kövesse az utasításokat, és hozzon létre egy új **webalkalmazás és/vagy WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="026a1-124">Follow the prompts and create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="026a1-125">A **neve** az alkalmazás ismerteti az alkalmazást a végfelhasználók számára</span><span class="sxs-lookup"><span data-stu-id="026a1-125">The **name** of the application will describe your application to end-users</span></span>
  * <span data-ttu-id="026a1-126">A **bejelentkezési URL-cím** az alkalmazás alap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="026a1-126">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="026a1-127">A vázat alapértelmezett érték a `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="026a1-127">The skeleton's default is `http://localhost:8080/adal4jsample/`.</span></span>
6. <span data-ttu-id="026a1-128">Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="026a1-128">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="026a1-129">Ez az érték kell a következő szakaszokban lévő, másolja az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="026a1-129">You'll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="026a1-130">Az a **beállítások** -> **tulajdonságok** az alkalmazás lapján frissítse a App ID URI.</span><span class="sxs-lookup"><span data-stu-id="026a1-130">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="026a1-131">A **App ID URI** az alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="026a1-131">The **App ID URI** is a unique identifier for your application.</span></span>  <span data-ttu-id="026a1-132">Az egyezmény használandó `https://<tenant-domain>/<app-name>`, pl. `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="026a1-132">The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `http://localhost:8080/adal4jsample/`.</span></span>

<span data-ttu-id="026a1-133">Egyszer a portálon, az alkalmazás létrehozása egy **kulcs** a a **beállítások** lapon az alkalmazáshoz, és másolja le.</span><span class="sxs-lookup"><span data-stu-id="026a1-133">Once in the portal for your app create a **Key** from the **Settings** page for your application and copy it down.</span></span>  <span data-ttu-id="026a1-134">Erre hamarosan szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="026a1-134">You will need it shortly.</span></span>

## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a><span data-ttu-id="026a1-135">2. Az alkalmazás beállítása ADAL4J könyvtár és előfeltételek Maven használatával</span><span class="sxs-lookup"><span data-stu-id="026a1-135">2. Set up your app to use ADAL4J library and prerequisites using Maven</span></span>
<span data-ttu-id="026a1-136">Itt konfigurálását végezzük el ADAL4J az OpenID Connect hitelesítési protokoll használatára.</span><span class="sxs-lookup"><span data-stu-id="026a1-136">Here, we'll configure ADAL4J to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="026a1-137">Be- és kijelentkezési kérések kiállítása, a felhasználói munkamenet kezelésére, és többek között a felhasználó adatai ADAL4J fogja használni.</span><span class="sxs-lookup"><span data-stu-id="026a1-137">ADAL4J will be used to issue sign-in and sign-out requests, manage the user's session, and get information about the user, amongst other things.</span></span>

* <span data-ttu-id="026a1-138">A projekt gyökérkönyvtárában, megnyitni vagy létrehozni `pom.xml` , és keresse meg a `// TODO: provide dependencies for Maven` , és cserélje le a következőre:</span><span class="sxs-lookup"><span data-stu-id="026a1-138">In the root directory of your project, open/create `pom.xml` and locate the `// TODO: provide dependencies for Maven` and replace with the following:</span></span>

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



## <a name="3-create-the-java-publicclient-file"></a><span data-ttu-id="026a1-139">3. A Java PublicClient fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="026a1-139">3. Create the Java PublicClient file</span></span>
<span data-ttu-id="026a1-140">A fentiek alapján fogjuk használni a Graph API a bejelentkezett felhasználó kapcsolatos adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="026a1-140">As indicated above, we will be using the Graph API to get data about the logged in user.</span></span> <span data-ttu-id="026a1-141">Ez lesz az USA könnyen mindkét fájlt képviselő azt létrehoznia egy **könyvtárobjektum** és képviselő fájlhoz a **felhasználói** , hogy a Java OO mintáját használható.</span><span class="sxs-lookup"><span data-stu-id="026a1-141">For this to be easy for us we should create both a file to represent a **Directory Object** and an individual file to represent the **User** so that the OO pattern of Java can be used.</span></span>

* <span data-ttu-id="026a1-142">Hozzon létre egy nevű fájlt `DirectoryObject.java` , amelyek segítségével bármely DirectoryObject (, nyugodtan később használhatja más diagramhoz lekérdezések a következőkre) kapcsolatos alapvető adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="026a1-142">Create a file called `DirectoryObject.java` which we will use to store basic data about any DirectoryObject (you can feel free to use this later for any other Graph Queries you may do).</span></span> <span data-ttu-id="026a1-143">Akkor is kivágás-beillesztés ezt az alábbi lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="026a1-143">You can cut/paste this from below:</span></span>

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


## <a name="compile-and-run-the-sample"></a><span data-ttu-id="026a1-144">Fordítsa le, és futtathatja a</span><span class="sxs-lookup"><span data-stu-id="026a1-144">Compile and run the sample</span></span>
<span data-ttu-id="026a1-145">A gyökérkönyvtár vissza kimenő módosítsa, majd futtassa a következő parancsot a minta csak állapotba használata `maven`.</span><span class="sxs-lookup"><span data-stu-id="026a1-145">Change back out to your root directory and run the following command to build the sample you just put together using `maven`.</span></span> <span data-ttu-id="026a1-146">Ez a beállítás használja a `pom.xml` függőségek megírt fájlt.</span><span class="sxs-lookup"><span data-stu-id="026a1-146">This will use the `pom.xml` file you wrote for dependencies.</span></span>

`$ mvn package`

<span data-ttu-id="026a1-147">Most már rendelkeznie kell egy `adal4jsample.war` fájlt a `/targets` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="026a1-147">You should now have a `adal4jsample.war` file in your `/targets` directory.</span></span> <span data-ttu-id="026a1-148">Előfordulhat, hogy telepíteni, amely a Tomcat tárolóban, és látogasson el az URL-cím</span><span class="sxs-lookup"><span data-stu-id="026a1-148">You may deploy that in your Tomcat container and visit the URL</span></span> 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> <span data-ttu-id="026a1-149">Rendkívül egyszerűen telepíthető a legújabb Tomcat kiszolgálókkal WAR.</span><span class="sxs-lookup"><span data-stu-id="026a1-149">It is very easy to deploy a WAR with the latest Tomcat servers.</span></span> <span data-ttu-id="026a1-150">Egyszerűen lépjen `http://localhost:8080/manager/` feltöltésével kapcsolatos kövesse az utasításokat a "adal4jsample.war" fájl.</span><span class="sxs-lookup"><span data-stu-id="026a1-150">Simply navigate to `http://localhost:8080/manager/` and follow the instructions on uploading your ``adal4jsample.war\` file.</span></span> <span data-ttu-id="026a1-151">A következőket hajtja végre autodeploy meg a helyes végpontját.</span><span class="sxs-lookup"><span data-stu-id="026a1-151">It will autodeploy for you with the correct endpoint.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="026a1-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="026a1-152">Next Steps</span></span>
<span data-ttu-id="026a1-153">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="026a1-153">Congratulations!</span></span> <span data-ttu-id="026a1-154">Most már rendelkezik egy Java-alkalmazás, amely képes biztonságosan hitelesíti a felhasználókat, működő hívja fel a webes API-k, az OAuth 2.0 verziót használja, és a felhasználó alapszintű adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="026a1-154">You now have a working Java application that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="026a1-155">Ha még nem tette meg, most már az egyes felhasználóival a bérlő feltölti idő.</span><span class="sxs-lookup"><span data-stu-id="026a1-155">If you haven't already, now is the time to populate your tenant with some users.</span></span>

<span data-ttu-id="026a1-156">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként [is letöltheti a .zip Itt](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), vagy a Githubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="026a1-156">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

