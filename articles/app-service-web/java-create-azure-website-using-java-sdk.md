---
title: "egy webalkalmazást az Azure App Service hello Azure SDK-t használó Java aaaCreate"
description: "Ismerje meg, hogyan toocreate programozott módon, Azure App Service webalkalmazás hello Javához készült Azure SDK-t."
tags: azure-classic-portal
services: app-service-web
documentationcenter: Java
author: donntrenton
manager: erikre
editor: jimbe
ms.assetid: 8954c456-1275-4d57-aff4-ca7d6374b71e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/25/2016
ms.author: v-donntr
ms.openlocfilehash: 42ba86b7fbb5668b3675198d0c5bb454525f706b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-hello-azure-sdk-for-java"></a>Az Azure App Service hello Azure SDK-t használó Java-webalkalmazás létrehozása
<!-- Azure Active Directory workflow is not yet available on hello Azure Portal -->

## <a name="overview"></a>Áttekintés
Ez a forgatókönyv bemutatja, hogyan toocreate egy Java-alkalmazást hoz létre egy webalkalmazást az Azure SDK [Azure App Service][Azure App Service], majd telepítenie kell egy alkalmazás tooit. Két részből áll:

* 1. rész bemutatja, hogyan toobuild Java-alkalmazás, amely létrehoz egy webalkalmazást.
* 2. rész bemutatja, hogyan toocreate egy egyszerű "Hello, World" JSP alkalmazást, majd használja az FTP ügyfél toodeploy kód tooApp szolgáltatás.

## <a name="prerequisites"></a>Előfeltételek
### <a name="software-installations"></a>Szoftvertelepítés
Ebben a cikkben AzureWebDemo alkalmazáskód hello Azure Java SDK 0.7.0, amelyek segítségével lehet telepíteni használatával készült hello [Webplatform-telepítő] [ Web Platform Installer] (WebPI). Ezenkívül győződjön meg arról, hogy toouse hello legújabb verziójának hello [Eclipse Azure eszköztára][Azure Toolkit for Eclipse]. Hello SDK telepítése után frissítse az Eclipse-projekt hello függőséget futtatásával **frissítése** a **Maven Tárházak**, majd újra vegye fel a hello minden csomag legújabb verzióját hello  **Függőségek** ablak. Az eclipse-ben telepített szoftvert hello verziója kattintva ellenőrizheti **Súgó > telepítésének részletei**; meg kell rendelkeznie legalább hello következő verziói:

* A Microsoft Azure-tárak Java 0.7.0.20150309 csomag
* Java EE-fejlesztőknek 4.4.2.20150219 készült eclipse IDE

### <a name="create-and-configure-cloud-resources-in-azure"></a>Hozza létre és konfigurálja a felhőben található erőforrásokat az Azure-ban
Ez az eljárás megkezdése előtt kell toohave egy aktív Azure-előfizetéssel, és beállítható az alapértelmezett Active Directory (az AD) az Azure-on.

### <a name="create-an-active-directory-ad-in-azure"></a>Hozzon létre egy Active Directory (AD) az Azure-ban
Ha még nem rendelkezik az Active Directory (AD) az Azure-előfizetése, jelentkezzen be hello [a klasszikus Azure portálon] [ Azure classic portal] Microsoft-fiókjával. Ha több előfizetése van, kattintson a **előfizetések** és select hello alapértelmezett címtár hello előfizetés keresi toouse ebben a projektben. Kattintson a **alkalmaz** tooswitch toothat előfizetési nézet.

1. Válassza ki **Active Directory** a bal oldali menüben hello. **Új kattintson > Directory > egyéni létrehozás**.
2. A **könyvtár hozzáadása**, jelölje be **új könyvtár létrehozása**.
3. A **neve**, adja meg a könyvtár nevét.
4. A **tartomány**, írja be a tartomány nevét. Ez az alapszintű tartománynevet, hogy a címtárban; alapértelmezés szerint hello űrlap rendelkezik `<domain_name>.onmicrosoft.com`. Hello directory vagy egy másik, saját tartománynév alapján nevére. Később a szervezet már használja egy másik tartománynevet is hozzáadhat.
5. A **ország vagy régió**, válassza ki a területi beállítás.

AD a további információkért lásd: [Mi az Azure AD-címtár][What is an Azure AD directory]?

### <a name="create-a-management-certificate-for-azure"></a>Az Azure felügyeleti tanúsítvány létrehozása
hello Javához készült Azure SDK-t használ, felügyeleti tanúsítványok tooauthenticate Azure-előfizetések. Ezek a X.509 v3 alapján létrehozott tanúsítványok tooauthenticate hello szolgáltatásfelügyeleti API tooact nevében hello előfizetés tulajdonosának toomanage előfizetéshez kapcsolódó erőforrásokat használó ügyfél-alkalmazás használja.

az alábbi eljárással hello kód egy önaláírt tanúsítványt tooauthenticate Azure használ. Az eljárás használatához toocreate tanúsítványt kell, és töltse fel toohello [a klasszikus Azure portálon] [ Azure classic portal] előre. Ebbe beletartozik az alábbi lépésekkel hello:

* Az ügyféltanúsítvány képviselő PFX-fájl létrehozása, és mentse helyileg.
* Felügyeleti tanúsítvány (CER-fájljával) hello PFX-fájlból jön létre.
* Töltse fel a hello CER fájl tooyour Azure-előfizetés.
* Hello PFX-fájl átalakítása JKS, mert Java használ, hogy formátum tooauthenticate tanúsítványok használatával.
* Hello alkalmazás hitelesítési kód, amely hivatkozik a toohello helyi JKS fájl írása.

Ez az eljárás befejezésekor hello CER tanúsítvány legyen elhelyezve, az Azure-előfizetése, és hello JKS tanúsítványt a helyi meghajtón legyen elhelyezve. A felügyeleti tanúsítványokról további információ: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal][Create and Upload a Management Certificate for Azure].

#### <a name="create-a-certificate"></a>Tanúsítvány létrehozása
toocreate a saját önaláírt tanúsítványt, nyissa meg a parancskonzolról, az operációs rendszerben, és futtassa a következő parancsok hello.

> **Megjegyzés:** hello számítógép, amelyen ezt a parancsot futtatja hello telepített JDK kell rendelkeznie. Hello elérési toohello kulcseszközről is, hello hely telepítésének hello JDK függ. További információkért lásd: [kulcs és a tanúsítvány eszköz (kulcseszközről)] [ Key and Certificate Management Tool (keytool)] a hello Java online dokumentációt.
> 
> 

toocreate hello .pfx-fájlt:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

toocreate hello .cer fájl:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Ahol:

* `<java-install-dir>`az hello elérési toohello könyvtár Java van telepítve.
* `<keystore-id>`hello keystore bejegyzés azonosítója (például `AzureRemoteAccess`).
* `<cert-store-dir>`hello elérési toohello könyvtár, amelyben toostore tanúsítványokat kívánja (például `C:/Certificates`).
* `<cert-file-name>`hello tanúsítványfájl hello neve (például `AzureWebDemoCert`).
* `<password>`hello jelszó tooprotect hello tanúsítvány; kiválasztása legalább 6 karakter hosszúságúnak kell lennie. Nincs jelszava, akkor adhatja meg, bár ez nem ajánlott.
* `<dname>`hello X.500 megkülönböztető név toobe társított alias, és hello kibocsátó és tulajdonos mezőket hello önaláírt tanúsítványt használt.

További információkért lásd: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal][Create and Upload a Management Certificate for Azure].

#### <a name="upload-hello-certificate"></a>Hello-tanúsítvány feltöltése
tooupload egy önaláírt tanúsítványt tooAzure lépjen toohello **beállítások** hello klasszikus portál lapon, majd kattintson az hello **felügyeleti tanúsítványok** fülre. Kattintson a **feltöltése** hello hello alján lapon, és keresse meg a létrehozott hello CER-fájljával toohello helyét.

#### <a name="convert-hello-pfx-file-into-jks"></a>Hello PFX-fájl átalakítása JKS
Hello Windows parancssort (rendszergazdaként fut), a cd toohello könyvtárat tartalmazó hello tanúsítványokat, és futtassa a következő parancsot, hello ahol `<java-install-dir>` hello könyvtár, amelyben a számítógépre telepítette Java:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Amikor a rendszer kéri, adja meg a hello cél keystore jelszót; Ez lesz hello jelszót hello JKS fájl.
2. Amikor a rendszer kéri, adja meg a hello forrás keystore jelszót; Ez a megadott PFX-fájljának hello hello jelszót.

hello két jelszó azonos hello toobe nem rendelkezik. Nincs jelszava, akkor adhatja meg, bár ez nem ajánlott.

## <a name="build-a-web-app-creation-application"></a>A webalkalmazás létrehozása alkalmazás létrehozása
### <a name="create-hello-eclipse-workspace-and-maven-project"></a>Hozzon létre Eclipse munkaterületet, és Maven Project hello
Ez a szakasz a munkaterületet, és egy Maven-projektjét, amely hello alkalmazás létrehozását, nevű webalkalmazás AzureWebDemo hoz létre.

1. Hozzon létre egy új Maven-projektet. Kattintson a **fájl > Új > Maven Project**. A **új Maven Project**, jelölje be **hozzon létre egy egyszerű projektet** és **alapértelmezett munkaterület helyet**.
2. Második lapján hello **új Maven Project**, adja meg a következőket hello:
   
   * Csoport azonosítója:`com.<username>.azure.webdemo`
   * Összetevő-azonosító: AzureWebDemo
   * Verzió: 0.0.1-SNAPSHOT
   * Csomagolására: jar
   * Name: AzureWebDemo
     
     Kattintson a **Befejezés** gombra.
3. Nyissa meg a Project Explorer hello új projekt pom.xml fájlt. Jelölje be hello **függőségek** fülre. Mivel ez egy új projektet, még felsorolt csomagok száma.
4. Nyissa meg hello Maven adattárak megtekintése. **Kattintson az ablak > nézet megjelenítése > más > Maven > Maven Tárházak** kattintson **OK**. Hello **Maven Tárházak** nézete hello IDE hello alján jelenik meg.
5. Nyissa meg **globális adattárak**, kattintson a jobb gombbal hello **központi** tárház, és válassza ki **Rebuild Index**.
   
    ![][1]
   
    Ez a lépés attól függően, hogy a kapcsolat sebességétől hello több percig is eltarthat. Hello index újraépíti, amikor látnia kell a Microsoft Azure hello csomagok hello **központi** Maven-tárházat.
6. A **függőségek**, kattintson a **Hozzáadás**. A **adja meg a csoport azonosítója...**  meg `azure-management`. Válassza ki az alapszintű és az App Service Web Apps hello csomagok:
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > **Megjegyzés:** hello függőségek követően egy új verzióra frissít, ha szüksége van-e toore-adjon hozzá minden hello függőségek ezen a listán.
   > Miután rákattintott **Hozzáadás** és válassza az egyes függőségek hello hello az új verziószámmal valószínűleg **függőségek** lista.
   > 
   > 

Kattintson az **OK** gombra. hello Azure csomagok akkor jelennek meg hello **függőségek** listája.

### <a name="writing-java-code-toocreate-a-web-app-by-calling-hello-azure-sdk"></a>A webes alkalmazás Java-kóddal tooCreate írása hívása hello Azure SDK által
Következő lépésként írja meg hello behívó kód API-k hello Azure SDK-t a Java toocreate hello App Service-webalkalmazások számára.

1. Hozzon létre egy Java class toocontain hello fő belépési pont kódot. A Project Explorer hello projekt csomóponton kattintson a jobb gombbal, és válassza ki **új > osztály**.
2. A **új Java-osztály**, hello osztály neve `WebCreator` , és ellenőrizze a hello **nyilvános statikus "void" main** jelölőnégyzetet. hello beállításokat a következőképpen kell megjelennie:
   
    ![][2]
3. Kattintson a **Befejezés** gombra. hello WebCreator.java fájl Project Explorer jelenik meg.

### <a name="calling-hello-azure-api-toocreate-an-app-service-web-app"></a>Hello Azure API tooCreate egy App Service Web App hívja.
#### <a name="add-necessary-imports"></a>Adja hozzá a szükséges importálása
WebCreator.java adja hozzá a következő importálási utasításban; hello Ezek importálása nyújt hozzáférést tooclasses hello a kezelési kódtárakat Azure API-k fel:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;

    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;

    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;

    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;

    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-hello-main-entry-point-class"></a>Adja meg a hello fő belépési pont osztály
Hello hello AzureWebDemo alkalmazás célja toocreate egy App Service Web App, mert az alkalmazás neve hello fő osztály `WebAppCreator`. Ez az osztály hello fő belépési pont behívó kód hello Azure szolgáltatásfelügyeleti API toocreate hello webalkalmazás biztosít.

Adja hozzá az alábbi paraméterdefiníciókra hello webes alkalmazáshoz és webtárhely hello. Szüksége lesz tooprovide saját Azure-előfizetés azonosítója és a tanúsítvány adatait.

    public class WebAppCreator {

        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";

        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

Ahol:

* `<subscription-id>`a rendszer használni kívánt toocreate hello erőforrás hello Azure-előfizetés Azonosítóját.
* `<certificate-store-path>`hello elérési út és fájlnév toohello JKS fájlt a helyi tanúsítványtárolóban tároló könyvtárban van. Például `C:/Certificates/CertificateName.jks` Linux és `C:\Certificates\CertificateName.jks` Windows.
* `<certificate-password>`az a JKS tanúsítvány létrehozásakor megadott hello jelszó.
* `webAppName`bármilyen nevet választania; Ez az eljárás hello nevét használja `WebDemoWebApp`. hello teljes tartománynév megadása hello `webAppName` a hello `domainName` fűzött, így ebben az esetben hello teljes tartománya `webdemowebapp.azurewebsites.net`.
* `domainName`meg kell adni a fentiek szerint.
* `webSpaceName`hello definiált hello értékek egyike lehet: [WebSpaceNames] [ WebSpaceNames] osztály.
* `appServicePlanName`meg kell adni a fentiek szerint.

> **Megjegyzés:** minden alkalommal, amikor az alkalmazás futtatásához, toochange hello értékének kell `webAppName` és `appServicePlanName` (vagy az Azure portál hello hello a webalkalmazás törlése) hello alkalmazás ismételt futtatása előtt. Ellenkező esetben végrehajtása sikertelen lesz, mert hello ugyanaz az erőforrás már létezik az Azure-on.
> 
> 

#### <a name="define-hello-web-creation-method"></a>Hello webes létrehozási módjának megadása
A következő határozza meg a metódus toocreate hello webes alkalmazás. Ezzel a módszerrel `createWebApp`, hello web app és hello webtárhely hello paramétereit. Emellett hoz létre, és beállítja az hello App Service Web Apps felügyeleti ügyfél, hello által definiált [WebSiteManagementClient] [ WebSiteManagementClient] objektum. hello felügyeleti ügyfél kulcs toocreating webalkalmazások. Engedélyező alkalmazások toomanage webes alkalmazásokat (például létrehozása, update és delete, a műveletek végrehajtása) hello service management API meghívásával RESTful webes szolgáltatásokat nyújt.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for hello App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path toohello JKS file
            keyStorePassword,  // Password for hello JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create hello App Service Web Apps management client toocall Azure APIs
        // and pass it hello App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for hello web app with hello specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that hello server farm name takes hello Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define hello web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create hello web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output hello HTTP status code of hello response; 200 indicates hello request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output hello name of hello web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

hello kód kimeneteként hello HTTP-állapot – hello válasz sikerességét vagy sikertelenségét jelző, és ha sikeres, kimeneteként webalkalmazás létrehozása hello hello nevét.

#### <a name="define-hello-main-method"></a>Hello main() módszer megadása
Adja meg a hello main() metódus kódot hívások createWebApp() toocreate hello webes alkalmazást.

Végezetül hívás `createWebApp` a `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-hello-application-and-verify-web-app-creation"></a>Hello alkalmazás fut, és ellenőrizze a webalkalmazás létrehozása
Kattintson az alkalmazást futtató tooverify **futtatása > futtassa**. Hello alkalmazás befejeződésekor kell megjelennie a következő kimeneti hello Eclipse konzolon hello:

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

Jelentkezzen be a klasszikus Azure portálon hello, majd kattintson a **webalkalmazások**. Új webalkalmazás hello meg kell jelennie hello webalkalmazások listájában néhány percen belül.

## <a name="deploying-an-application-toohello-web-app"></a>Egy alkalmazás toohello webalkalmazás üzembe helyezése
Után futtatta AzureWebDemo hello új webalkalmazás létrehozása, jelentkezzen be a klasszikus portálon hello gombra **Web Apps**, és válassza ki **WebDemoWebApp** a hello **Web Apps** lista. Hello webes alkalmazás irányítópult-oldalon, kattintson **Tallózás** (hello URL-címet, vagy `webdemowebapp.azurewebsites.net`) toonavigate tooit. Egy üres helyőrző lap jelenik meg, mert nincs tartalma még közzétett toohello webalkalmazás le lett.

Ezután létrehoz egy "Hello, World" alkalmazást, és toohello webalkalmazás telepítése az.

### <a name="create-a-jsp-hello-world-application"></a>JSP Hello World-alkalmazás létrehozása
#### <a name="create-hello-application"></a>Hello alkalmazás létrehozása
A sorrend toodemonstrate hogyan toodeploy egy alkalmazás toohello webes hello a következő eljárás bemutatja, hogyan toocreate egy egyszerű "Hello World" Java-alkalmazást, és töltse fel toohello App Service Web App, az alkalmazás hozott létre.

1. Kattintson a **fájl > Új > dinamikus webes projekt**. Nevezze el a következőképpen: `JSPHello`. Nem kell toochange ezen a párbeszédpanelen egyéb beállításait. Kattintson a **Befejezés** gombra.
   
    ![][3]
2. A Project Explorer bontsa ki a hello **JSPHello** projektre, kattintson a jobb gombbal **WebContent**, majd kattintson a **új > JSP-fájl**. Hello új JSP-fájl létrehozása párbeszédpanelen hello új fájl neve `index.jsp`. Kattintson a **Tovább** gombra.
3. A hello **JSP-sablon kiválasztása** párbeszédpanelen válassza **új JSP-fájl (html)** kattintson **Befejezés**.
4. Index.jsp, adja hozzá a következő kódot a hello hello `<head>` és `<body>` szakaszok címkét:
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, hello time is <%= date %> 
        </body>

#### <a name="run-hello-hello-world-application-in-localhost"></a>Hello Hello World alkalmazást futtat localhost
Ez az alkalmazás futtatása előtt kell tooconfigure bizonyos tulajdonságait.

1. Kattintson a jobb gombbal hello **JSPHello** projektre, és válassza ki **tulajdonságok**.
2. A hello **tulajdonságok** párbeszédpanel: Jelölje ki **Java Build elérési**, jelölje be hello **rendezés és exportálási** lapon jelölje **JRE rendszerkönyvtár**, kattintson a **Mentése** toomove azt toohello hello lista elejére.
   
    ![][4]
3. Is a hello **tulajdonságok** párbeszédpanel: válasszon **megcélzott futtatókörnyezetek** kattintson **új**.
4. A hello **új kiszolgáló Futtatókörnyezetét** párbeszédpanelen válasszon ki egy kiszolgálót, mint **Apache Tomcat v7.0** kattintson **következő**. A hello **Tomcat kiszolgálót** párbeszédpanelen, a set **neve** túl`Apache Tomcat v7.0`, és állítsa be **Tomcat telepítési könyvtárát** toohello directory hello verziója van telepítve Azt szeretné, hogy toouse tomcat kiszolgálót.
   
    ![][5]
   
    Kattintson a **Befejezés** gombra.
5. Majd térjen vissza toohello **megcélzott futtatókörnyezetek** hello oldalán **tulajdonságok** párbeszédpanel. Válassza ki **Apache Tomcat v7.0**, majd kattintson a **OK**.
   
    ![][6]
6. Az hello Eclipse **futtassa** menüben kattintson a **futtatása**. A hello **futtató** párbeszédablakban válassza **futtassa a kiszolgálón**. A hello **futtassa a kiszolgálón** párbeszédablakban válassza **Tomcat v7.0 Server**:
   
    ![][7]
   
    Kattintson a **Befejezés** gombra.
7. Ha hello az alkalmazás fut, megtekintheti az hello **JSPHello** lap jelenik meg az eclipse-ben localhost ablakban (`http://localhost:8080/JSPHello/`), megjelenítés hello a következő üzenetet:
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-hello-application-as-a-war"></a>Exportálás hello alkalmazás WAR-fájlként
Hello web project fájlok exportálása egy webes archívumfájl (WAR), hogy Ezután telepítheti azt toohello webalkalmazás. hello következő web project fájlok találhatók hello WebContent mappában:

    META-INF
    WEB-INF
    index.jsp

1. Kattintson a jobb gombbal a hello WebContent mappa, és válassza ki **exportálása**.
2. A hello **exportálása kiválasztása** párbeszédpanel, kattintson a **webes > WAR** fájlt, majd kattintson az **tovább**.
3. A hello **WAR exportálása** párbeszédpanelen válassza ki a hello src directory hello aktuális projektben, és hello WAR-fájlt a hello végén hello nevét tartalmazza. Példa:
   
    `<project-path>/JSPHello/src/JSPHello.war`

WAR fájlok telepítésével kapcsolatos további információkért lásd: [hozzáadása a Java-alkalmazás tooAzure App Service Web Apps](web-sites-java-add-app.md).

### <a name="deploying-hello-hello-world-application-using-ftp"></a>Hello Hello World alkalmazás használatával FTP telepítése
Jelöljön ki egy külső FTP ügyfél toopublish hello alkalmazást. Ez az eljárás ismerteti a két lehetőség közül választhat: hello Kudu konzol épített Azure; és FileZilla, olyan népszerű eszköz egy kényelmes, grafikus felhasználói felületen.

> **Megjegyzés:** hello Azure eszközkészlet az eclipse-ben támogatja a központi telepítés toostorage fiókokat és a felhőalapú szolgáltatások, de jelenleg nem támogatja a telepítési tooweb alkalmazások. Toostorage fiókok telepítheti, és a felhőalapú Azure telepítési projekt segítségével, a szolgáltatások [egy Hello World alkalmazás létrehozása az Azure az eclipse-ben](http://msdn.microsoft.com/library/azure/hh690944.aspx), de nem tooweb alkalmazások. Más módszerekkel például az FTP- vagy GitHub tootransfer fájlok tooyour webalkalmazás.
> 
> **Megjegyzés:** FTP hello Windows parancssorból (hello parancssori FTP.EXE segédprogram, amely a Windows részét képező) használata nem ajánlott. FTP-ügyfelek, amelyek aktív FTP FTP.EXE, például gyakran sikertelen toowork tűzfalon keresztül. Aktív FTP belső LAN-alapú címét adja meg, akkor toowhich egy FTP-kiszolgáló valószínűleg tooconnect sikertelen lesz.
> 
> 

További információ a központi telepítés tooan App Service web app használatával FTP tekintse meg a következő témakörök hello:

* [Az FTP-segédprogrammal történő telepítése](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a>Üzembe helyezési hitelesítő adatok beállítása
Ellenőrizze, hogy futtatta hello **AzureWebDemo** alkalmazás toocreate egy webalkalmazást. Akkor fogja átvinni fájlokat toothis helyét.

1. Jelentkezzen be hello klasszikus portálra, és kattintson a **webalkalmazások**. Győződjön meg arról, hogy **WebDemoWebApp** azokból a webalkalmazásokból hello listájában jelenik meg, és győződjön meg arról, hogy fut-e. Kattintson a **WebDemoWebApp** tooopen a **irányítópult** lap.
2. A hello **irányítópult** lap **gyors áttekintése**, kattintson a **az üzembe helyezési hitelesítő adatok beállítása** (már üzembe helyezési hitelesítő adatokat, ha ez olvassa be  **A központi telepítési hitelesítő adatok alaphelyzetbe állítása**).
   
    Üzembe helyezési hitelesítő adatok társított Microsoft-fiókkal. Toospecify egy felhasználónév és jelszó használható Git és az FTP használatával toodeploy van szüksége. A Microsoft-fiókjához társított összes Azure-előfizetések is használhatja ezeket a hitelesítő adatok toodeploy tooany webalkalmazás. Adja meg a Git és az FTP telepítési hitelesítő adatok a hello párbeszédpanel, és a rekord hello felhasználónév és a jelszó későbbi használatra.

#### <a name="get-ftp-connection-information"></a>FTP-kiszolgáló kapcsolati adatainak lekérése
toouse FTP toodeploy alkalmazás fájlok az újonnan létrehozott toohello web app alkalmazásban kell tooobtain kapcsolódási információt. Két módon tooobtain kapcsolódási információt. Egyirányú van toovisit hello webes alkalmazás **irányítópult** lapon; hello más módon toodownload hello webes alkalmazás közzétételi profil. hello közzétételi profil egy XML-fájl, amely például FTP állomás nevét és a bejelentkezési hitelesítő adatokat biztosít a webalkalmazások Azure App Service-ben. A felhasználónév és jelszó toodeploy tooany webalkalmazás hello Azure-fiókra, nem csak az egyik társított előfizetéseket használhatja.

tooobtain FTP csatlakozási adataival hello webalkalmazása panelén a hello [Azure Portal][Azure Portal]:

1. A **Essentials**, található, és másolja a hello **FTP-állomásnév**. Ez az hasonló URI túl`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.
2. A **Essentials**, található, és másolja **FTP vagy üzembe helyező felhasználónév**. Ez lesz a hello űrlap *webappname\deployment-username*, például `WebDemoWebApp\deployer77`.

a közzétételi profil tooobtain FTP csatlakozási adataival hello:

1. Hello webes alkalmazás paneljén kattintson **Get közzétételi profil**. A .publishsettings fájlt tooyour helyi meghajtót letöltése.
2. Nyissa meg a hello .publishsettings fájlt egy XML-szerkesztőben vagy szövegszerkesztőben, és megkeresi hello `<publishProfile>` elemet tartalmazó `publishMethod="FTP"`. Az alábbihoz hasonló hello:
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. Vegye figyelembe, hogy hello webalkalmazás `publishProfile` beállítások leképezése toohello FileZilla kezelő beállításokat az alábbiak szerint:

* `publishUrl`ugyanaz, mint hello **FTP-állomás neve**, hello beállítása érték **állomás**.
* `publishMethod="FTP"`azt jelenti, hogy megadta **protokoll** túl**FTP - File Transfer Protocol**, és **titkosítási** túl**egyszerű FTP használata**.
* `userName`és `userPWD` értékei hello kulcsok tényleges felhasználónevet és jelszót meg, ha alaphelyzetbe állít egy hello üzembe helyezési hitelesítő adatokat. `userName`ugyanaz, mint hello **telepítési / FTP-felhasználó**. Túl leképezik**felhasználói** és **jelszó** a FileZilla.
* `ftpPassiveMode="True"`azt jelenti, hogy a hello FTP-hely használja a passzív FTP-átvitel; Válassza ki **passzív** a hello **átvitel beállításainak** fülre.

#### <a name="configure-hello-web-app-toohost-a-java-application"></a>Hello webalkalmazás toohost Java-alkalmazások konfigurálása
Hello alkalmazás közzététele előtt kell toochange néhány konfigurációs beállítás, hogy hello webalkalmazás üzemeltethet Java-alkalmazások.

1. Hello klasszikus portálon lépjen toohello webes alkalmazás **irányítópult** lapot, és kattintson **konfigurálása**. A hello **konfigurálása** adja meg azokat a beállításokat a következő hello.
2. A **Java-verziót** hello alapértelmezett érték a **ki**; válassza hello Java-verziót a alkalmazás célkitűzések, például a 1.7.0_51. Ezt követően ellenőrizze azt is, amely **webes tároló** Tomcat kiszolgálót tooa verziója van beállítva.
3. A **alapértelmezett dokumentumok**adja hozzá az index.jsp és toohello hello lista elejére helyezze. (hello alapértelmezett webalkalmazások fájlja hostingstart.html.)
4. Kattintson a **Save** (Mentés) gombra.

#### <a name="publish-your-application-using-kudu"></a>A Kudu használó alkalmazások közzététele
Egyirányú toopublish hello alkalmazás Kudu hibakereső konzol az Azure beépített toouse hello. A kudu ismert toobe stabil és konzisztens legyen az App Service Web Apps és Tomcat kiszolgálót. Hello konzol hello webalkalmazás hozzáférni a keresse meg a következő képernyő hello tooa URL-címe:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Az eljárás használatához hello Kudu konzol itt található a következő URL-címet; hello: Keresse meg a toothis helye:
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. Hello felső menüben válassza ki a **konzol Debug > CMD**.
3. Hello konzol parancssorban lépjen a túl`/site/wwwroot` (vagy kattintson a `site`, majd `wwwroot` hello könyvtár nézetben hello oldal hello tetején):
   
    `cd /site/wwwroot`
4. Miután megadta **Java-verziót**, Tomcat kiszolgálót kell létrehoznia a webapps könyvtárba. Hello konzol parancssorban lépjen a toohello webapps könyvtárba:
   
    `mkdir webapps`
   
    `cd webapps`
5. Húzza a JSPHello.war `<project-path>/JSPHello/src/` , és helyezze a hello Kudu könyvtár nézetben a `/site/wwwroot/webapps`. Nem húzza toohello "Húzzon ide tooupload és zip" területen, mert a Tomcat fog csomagolja ki.
   
   ![][8]

Első JSPHello.war: hello directory területen megjelenik önmagában:

  ![][9]

Rövid időn belül (valószínűleg 5 percnél kevesebb) Tomcat kiszolgálót fog csomagolja ki hello WAR-fájlt egy kicsomagolt JSPHello könyvtárba. Kattintson a hello ROOT directory toosee, hogy index.jsp unzipped és másolásához. Ha igen, lépjen vissza toohello webapps directory toosee e hello kicsomagolása JSPHello könyvtár létrehozása. Ha nem látja ezeket az elemeket, várja meg, és ismételje meg.

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a>A FileZilla (nem kötelező) használó alkalmazások közzététele
Egy másik toopublish hello alkalmazásnak eszköze FileZilla, a népszerű külső FTP-ügyfél kényelmesen, grafikus felhasználói felületen. Töltse le, és telepítse a FileZilla [http://filezilla-project.org/](http://filezilla-project.org/) Ha még nem rendelkezik azt. Hello ügyfél használatával kapcsolatos további információkért lásd: hello [FileZilla dokumentáció](https://wiki.filezilla-project.org/Documentation) és ezt a blogbejegyzést [FTP-ügyfelek - rész 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. Kattintson a FileZilla, **fájl > kezelő**.
2. A hello **kezelő** párbeszédpanel, kattintson a **új hely**. Megjelenik egy új, üres FTP-hely **válasszon bejegyzés** tooprovide nevét kéri. Az alábbi eljárással nevezze el `AzureWebDemo-FTP`.
   
    A hello **általános** lapra, adja meg a következő beállítások hello:
   
   * **Állomás:** Enter hello **FTP-állomás neve** hello irányítópultról másolt.
   * **Port:** (hagyja üresen a mezőt, a passzív átvitel és hello server hello port toouse határozza meg.)
   * **Protokoll:** FTP fájlátviteli protokoll
   * **Titkosítás:** egyszerű FTP használata
   * **Bejelentkezési típusa:** normál
   * **Felhasználó:** Enter hello telepítési / FTP-felhasználó hello irányítópultról másolt. Ez a hello teljes FTP felhasználónév, amelynek hello űrlap *webappname\username*.
   * **Jelszó:** hello jelszó megadni, hogy a megadott hello üzembe helyezési hitelesítő adatok megadása után.
     
     A hello **átvitel beállításainak** lapon jelölje be **passzív**.
3. Kattintson a **Connect** (Csatlakozás) gombra. Ha sikeres, FileZilla tartozó konzol megjeleníti a `Status: Connected` üzenet és a probléma egy `LIST` toolist hello könyvtár tartalma parancsot.
4. A hello **helyi** hely panelen, jelölje be hello forráskönyvtár mely hello JSPHello.war fájlban található; hello elérési hasonló toohello következő lesz:
   
    `<project-path>/JSPHello/src/`
5. A hello **távoli** hely panelen, jelölje be hello célmappát. Telepíti a WAR-fájl toohello hello `webapps` hello webes alkalmazás legfelső szintű könyvtárába. Keresse meg a túl`/site/wwwroot`, kattintson a jobb gombbal a `wwwroot`, és válassza ki **könyvtár létrehozása**. Egyező hello könyvtár `webapps` , és írja be a könyvtárhoz.
6. Átviteli túl JSPHello.war`/site/wwwroot/webapps`. Válassza ki a JSPHello.war hello **helyi** lista fájlt, kattintson a jobb gombbal a, és válassza ki **feltöltése**. Akkor jelenik meg kell megjelennie `/site/wwwroot/webapps`.
7. Miután másolta JSPHello.war toohello webapps könyvtárba, Tomcat kiszolgálót automatikusan csomagolja ki (csomagolja ki) hello hello WAR-fájlt a fájlok. Bár Tomcat kiszolgálót megkezdése kicsomagolása szinte azonnal is telhet, mire hosszú idő (valószínűleg óra) hello fájlok tooappear hello FTP-ügyfél számára.

#### <a name="run-hello-hello-world-application-on-hello-web-app"></a>A webalkalmazás hello hello Hello World az alkalmazás futtatása
1. Miután hello WAR-fájl feltöltése, és ellenőrizte a Tomcat kiszolgálón egy kicsomagolt létrehozott `JSPHello` könyvtár, keresse meg a túl`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello alkalmazás.
   
   > **Megjegyzés:** választva **Tallózás** hello klasszikus portálon, kaphat hello alapértelmezett weblap, mely szerint a "a Java-alapú webalkalmazás létrehozása sikeresen befejeződött." Lehetséges, hogy toorefresh hello weblap rendelés tooview hello alkalmazás kimenet hello alapértelmezett weblap helyett.
   > 
   > 
2. Hello alkalmazás futásakor kell: weblapot a következő kimeneti hello
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a>Azure-erőforrások törlése
Ez az eljárás egy App Service webalkalmazásba hoz. Számlázott hello erőforrás mindaddig, amíg az létezik. Kivéve, ha azt tervezi, hogy a tesztelési, illetve a fejlesztési hello web app használatával toocontinue, érdemes lehet leállítása vagy törlése. A webes alkalmazás, amely le lett állítva továbbra is fel Önnek egy kis kell fizetni, de bármikor újraindíthatja azt. A webes alkalmazás törlésével lévő tooit feltöltött adat elvész.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

[1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
[2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
[3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
[4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
[5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
[6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
[7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
[8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
[9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
[10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png


[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure classic portal]: https://manage.windowsazure.com
[What is an Azure AD directory]: http://technet.microsoft.com/library/jj573650.aspx
[Create and Upload a Management Certificate for Azure]: ../cloud-services/cloud-services-certs-create.md
[Key and Certificate Management Tool (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
