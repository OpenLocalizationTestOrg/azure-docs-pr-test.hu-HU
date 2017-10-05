---
title: "Webalkalmazás létrehozása az Azure App Service szolgáltatásban az Azure SDK for Java használatával"
description: "Útmutató az Azure App Service programozott módon, az Azure SDK Java-webalkalmazás létrehozása."
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
ms.openlocfilehash: 08bb53de8cf437a5a2b1c3b38bce9f81b8349493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Webalkalmazás létrehozása az Azure App Service szolgáltatásban az Azure SDK for Java használatával
<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Áttekintés
Ez a forgatókönyv bemutatja, hogyan hozzon létre egy Java-alkalmazást hoz létre egy webalkalmazást az Azure SDK [Azure App Service][Azure App Service], majd az alkalmazást telepíti. Két részből áll:

* 1. rész bemutatja, hogyan hozhat létre egy Java-alkalmazást, amely létrehozza a webalkalmazást.
* 2. rész bemutatja, hogyan hozzon létre egy egyszerű "Hello, World" JSP alkalmazást, majd használja az FTP-ügyfél központi telepítése a kódot az App Service.

## <a name="prerequisites"></a>Előfeltételek
### <a name="software-installations"></a>Szoftvertelepítés
Ebben a cikkben a AzureWebDemo alkalmazáskód Azure Java SDK 0.7.0, amelyek segítségével lehet telepíteni használatával íródott a [Webplatform-telepítő] [ Web Platform Installer] (WebPI). Emellett ügyeljen arra, hogy a legújabb verzióját használja a [Eclipse Azure eszköztára][Azure Toolkit for Eclipse]. Az SDK telepítése után frissítse a függőségek az Eclipse-projekt futtatásával **frissítése** a **Maven Tárházak**, majd újra vegye fel a minden csomag legújabb verzióját a  **Függőségek** ablak. Az eclipse-ben telepített szoftvert verziója kattintva ellenőrizheti **Súgó > telepítésének részletei**; kell legalább a következő verziók:

* A Microsoft Azure-tárak Java 0.7.0.20150309 csomag
* Java EE-fejlesztőknek 4.4.2.20150219 készült eclipse IDE

### <a name="create-and-configure-cloud-resources-in-azure"></a>Hozza létre és konfigurálja a felhőben található erőforrásokat az Azure-ban
Ez az eljárás megkezdése előtt kell egy aktív Azure-előfizetéssel rendelkezik, és állítsa be az alapértelmezett Active Directory (az AD) az Azure-on.

### <a name="create-an-active-directory-ad-in-azure"></a>Hozzon létre egy Active Directory (AD) az Azure-ban
Ha még nem rendelkezik az Active Directory (AD) az Azure-előfizetése, jelentkezzen be a [a klasszikus Azure portálon] [ Azure classic portal] Microsoft-fiókjával. Ha több előfizetése van, kattintson a **előfizetések** és jelöljön ki az alapértelmezett könyvtárat a projekthez használni kívánt előfizetést. Kattintson a **alkalmaz** , hogy előfizetési nézetet.

1. Válassza ki **Active Directory** a bal oldali menüből. **Új kattintson > Directory > egyéni létrehozás**.
2. A **könyvtár hozzáadása**, jelölje be **új könyvtár létrehozása**.
3. A **neve**, adja meg a könyvtár nevét.
4. A **tartomány**, írja be a tartomány nevét. Ez az alapszintű tartománynevet, hogy a címtárban; alapértelmezés szerint az űrlap rendelkezik `<domain_name>.onmicrosoft.com`. A könyvtárnév vagy egy másik, a saját tartománynév alapján nevére. Később a szervezet már használja egy másik tartománynevet is hozzáadhat.
5. A **ország vagy régió**, válassza ki a területi beállítás.

AD a további információkért lásd: [Mi az Azure AD-címtár][What is an Azure AD directory]?

### <a name="create-a-management-certificate-for-azure"></a>Az Azure felügyeleti tanúsítvány létrehozása
Az Azure SDK for Java felügyeleti tanúsítványokat használ az Azure-előfizetések való hitelesítéshez szükséges. Ezek a X.509 v3 alapján létrehozott tanúsítványok használatával egy ügyfélalkalmazást, amely a szolgáltatás felügyeleti API használatával előfizetési erőforrások kezeléséhez az előfizetés tulajdonosa nevében végezze a hitelesítést.

A kódot az alábbi eljárással egy önaláírt tanúsítványt használ az Azure-ral hitelesítéséhez. Ezzel az eljárással kell hozzon létre egy tanúsítványt, és töltse fel azt a [a klasszikus Azure portálon] [ Azure classic portal] előre. Ez a következő lépésekből áll:

* Az ügyféltanúsítvány képviselő PFX-fájl létrehozása, és mentse helyileg.
* Felügyeleti tanúsítvány (CER-fájljával) a PFX-fájlból jön létre.
* Töltse fel az Azure-előfizetéshez a CER-fájljával.
* A PFX-fájl átalakítása JKS, mert a Java, hogy a formátum segítségével hitelesíti a tanúsítványok használatával.
* Az alkalmazás hitelesítési kód, amely hivatkozik a helyi JKS fájl írása.

Ez az eljárás befejezésekor a CER-tanúsítványt az Azure-előfizetése legyen elhelyezve, és a JKS tanúsítványt a helyi meghajtón legyen elhelyezve. A felügyeleti tanúsítványokról további információ: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal][Create and Upload a Management Certificate for Azure].

#### <a name="create-a-certificate"></a>Tanúsítvány létrehozása
A saját önaláírt tanúsítvány létrehozása, nyissa meg a parancskonzolról, az operációs rendszerben, és futtassa a következő parancsokat.

> **Megjegyzés:** ezt a parancsot futtató számítógépen telepíteni kell a telepített JDK. Az elérési útját a kulcseszköz is, a JDK telepítésének helyét függ. További információkért lásd: [kulcs és a tanúsítvány eszköz (kulcseszközről)] [ Key and Certificate Management Tool (keytool)] a a Java online dokumentációt.
> 
> 

A .pfx fájl létrehozásához:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

A .cer fájl létrehozásához:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Ahol:

* `<java-install-dir>`a könyvtár elérési útját a Java telepítve van.
* `<keystore-id>`a kulcstár bejegyzés azonosítója (például `AzureRemoteAccess`).
* `<cert-store-dir>`a könyvtár elérési útját, amelyben a tanúsítványok tárolni szeretné van (például `C:/Certificates`).
* `<cert-file-name>`a tanúsítványfájl neve (például `AzureWebDemoCert`).
* `<password>`a jelszó úgy dönt, hogy megvédeni a tanúsítványt; legalább 6 karakter hosszúságúnak kell lennie. Nincs jelszava, akkor adhatja meg, bár ez nem ajánlott.
* `<dname>`alias társítandó X.500 megkülönböztető nevét, és a kibocsátó és a tulajdonos mezőt az önaláírt tanúsítvány használatos.

További információkért lásd: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal][Create and Upload a Management Certificate for Azure].

#### <a name="upload-the-certificate"></a>A tanúsítvány feltöltése
Önaláírt tanúsítvány feltöltése az Azure-ba, keresse fel a **beállítások** a klasszikus portálon lapon, majd kattintson a **felügyeleti tanúsítványok** fülre. Kattintson a **feltöltése** a lap alján, és keresse meg a létrehozott CER-fájl helyét.

#### <a name="convert-the-pfx-file-into-jks"></a>A PFX-fájl átalakítása JKS
A a Windows parancssor (admin néven fut), CD-ről a könyvtárba, a tanúsítványokat tartalmazó és a következő parancsot, ahol `<java-install-dir>` címtár, amelyben a számítógépen telepítette a Java:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Amikor a rendszer kéri, adja meg a cél keystore jelszót; Ez lesz a JKS fájlhoz tartozó jelszót.
2. Amikor a rendszer kéri, adja meg a forrás keystore jelszót; Ez az a PFX-fájljának megadott jelszót.

A két jelszó nem kell azonosnak lennie. Nincs jelszava, akkor adhatja meg, bár ez nem ajánlott.

## <a name="build-a-web-app-creation-application"></a>A webalkalmazás létrehozása alkalmazás létrehozása
### <a name="create-the-eclipse-workspace-and-maven-project"></a>Az Eclipse munkaterület és a Maven Project létrehozása
Ez a szakasz a munkaterületet, és egy Maven-projektjét, amely az alkalmazás létrehozását, nevű webalkalmazás AzureWebDemo hoz létre.

1. Hozzon létre egy új Maven-projektet. Kattintson a **fájl > Új > Maven Project**. A **új Maven Project**, jelölje be **hozzon létre egy egyszerű projektet** és **alapértelmezett munkaterület helyet**.
2. A második lapján **új Maven Project**, adja meg a következőket:
   
   * Csoport azonosítója:`com.<username>.azure.webdemo`
   * Összetevő-azonosító: AzureWebDemo
   * Verzió: 0.0.1-SNAPSHOT
   * Csomagolására: jar
   * Name: AzureWebDemo
     
     Kattintson a **Befejezés** gombra.
3. Nyissa meg az új projekt pom.xml fájlt a Project Explorer. Válassza ki a **függőségek** fülre. Mivel ez egy új projektet, még felsorolt csomagok száma.
4. A Maven adattárak nézet megnyitásához. **Kattintson az ablak > nézet megjelenítése > más > Maven > Maven Tárházak** kattintson **OK**. A **Maven Tárházak** nézete jelenik meg az IDE alján.
5. Nyissa meg **globális adattárak**, kattintson a jobb gombbal a **központi** tárház, és válassza ki **Rebuild Index**.
   
    ![][1]
   
    Ez a lépés a kapcsolat sebességétől függően több percig is eltarthat. Amikor újraépíti az indexet, megjelenik a Microsoft Azure csomagok a **központi** Maven-tárházat.
6. A **függőségek**, kattintson a **Hozzáadás**. A **adja meg a csoport azonosítója...**  meg `azure-management`. Válassza ki az alapvető felügyeleti és az App Service Web Apps felügyeleti csomagok:
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > **Megjegyzés:** követően egy új verzióra frissítésekor a függőségeket, azt újra hozzá kell minden, a lista a függőségi.
   > Miután rákattintott **Hozzáadás** akkor jelenik meg, az új verziószámmal az egyes függőségek válassza ki a **függőségek** listája.
   > 
   > 

Kattintson az **OK** gombra. Az Azure csomagok majd megjelennek a **függőségek** listája.

### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>A webalkalmazás létrehozása az Azure SDK meghívásával Java programozás
Következő lépésként írja meg az App Service webalkalmazás létrehozása Javához készült Azure SDK API-k behívó kód.

1. Hozzon létre egy Java-osztály a fő belépési pont kódot tartalmazhatnak. A Project Explorer, kattintson a jobb gombbal a projektcsomópontra, és válassza ki **új > osztály**.
2. A **új Java-osztály**, az osztály neve `WebCreator` , és ellenőrizze a **nyilvános statikus "void" main** jelölőnégyzetet. A megadott beállítások megjelenjen-e az alábbiak szerint:
   
    ![][2]
3. Kattintson a **Befejezés** gombra. A WebCreator.java fájl Project Explorer jelenik meg.

### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Az App Service webalkalmazás létrehozása az Azure API hívása
#### <a name="add-necessary-imports"></a>Adja hozzá a szükséges importálása
WebCreator.java adja hozzá az alábbi importálásokat; Ezek importálja az osztályokat a kezelési kódtárakat az Azure API-k fel hozzáférést biztosítanak:

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


#### <a name="define-the-main-entry-point-class"></a>Adja meg a fő belépési pont osztály
A AzureWebDemo alkalmazás célja egy App Service webalkalmazás létrehozása, mert az alkalmazás neve a fő osztályban `WebAppCreator`. Ez az osztály a fő belépési pont a webalkalmazás létrehozása az Azure Service Management API behívó kód biztosít.

Adja hozzá az alábbi paraméterdefiníciókra web app és webtárhely. Meg kell adnia a saját Azure-előfizetés azonosítója és a tanúsítvány adatait.

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

* `<subscription-id>`az Azure-előfizetése Azonosítóját, amelyen létrehozásához az erőforrás van.
* `<certificate-store-path>`az elérési út és fájlnév a JKS fájlt a helyi tanúsítványtárolóban tároló könyvtárban van. Például `C:/Certificates/CertificateName.jks` Linux és `C:\Certificates\CertificateName.jks` Windows.
* `<certificate-password>`az a jelszó a JKS tanúsítvány létrehozásakor megadott.
* `webAppName`bármilyen nevet választania; Ez az eljárás nevét használja `WebDemoWebApp`. A teljes tartománynév a `webAppName` rendelkező a `domainName` fűzött, így ebben az esetben a teljes tartomány pedig `webdemowebapp.azurewebsites.net`.
* `domainName`meg kell adni a fentiek szerint.
* `webSpaceName`megadott értékek egyike lehet: a [WebSpaceNames] [ WebSpaceNames] osztály.
* `appServicePlanName`meg kell adni a fentiek szerint.

> **Megjegyzés:** minden alkalommal, amikor az alkalmazás futtatásához, módosítania kell a értékének `webAppName` és `appServicePlanName` (vagy törölje a webalkalmazást az Azure-portál) az alkalmazás ismételt futtatása előtt. Ellenkező esetben végrehajtása sikertelen lesz, mert ugyanaz az erőforrás már létezik az Azure-on.
> 
> 

#### <a name="define-the-web-creation-method"></a>A webalkalmazás-létrehozási módjának megadása
A következő határozza meg a webalkalmazás létrehozása egy metódust. Ezzel a módszerrel `createWebApp`, a web app és az webtárhely paramétereket határozza meg. Emellett hoz létre, és konfigurálja az App Service Web Apps-felügyeleti ügyfél, által definiált a [WebSiteManagementClient] [ WebSiteManagementClient] objektum. A felügyeleti ügyfél webalkalmazások létrehozása a kulcs. RESTful webes szolgáltatásokat, amelyek lehetővé teszik az alkalmazások (például létrehozása, update és delete, a műveletek végrehajtása) webes alkalmazásainak kezeléséhez a szolgáltatáskezelő API meghívásával nyújt.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
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
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

A kód a HTTP-állapot, a válasz sikerességét vagy sikertelenségét jelző kimeneteként, és ha sikeres, a létrehozott webalkalmazás neve lesz kimeneti.

#### <a name="define-the-main-method"></a>A main() módszer megadása
Adja meg, amely behívja a webalkalmazás létrehozása createWebApp() main() metódus kódot.

Végezetül hívás `createWebApp` a `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Futtassa az alkalmazást, és ellenőrizze a webalkalmazás létrehozása
Győződjön meg arról, hogy az alkalmazás fut, kattintson a **futtatása > futtassa**. Az alkalmazás befejeződésekor az Eclipse-konzolon a következő kimenetet kell megjelennie:

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

Jelentkezzen be a klasszikus Azure portálra, majd kattintson a **webalkalmazások**. Az új webalkalmazásba meg kell jelennie a webes alkalmazások listájában néhány percen belül.

## <a name="deploying-an-application-to-the-web-app"></a>A webalkalmazás az alkalmazások központi telepítése
Miután futtatta AzureWebDemo, és kattintson az új webalkalmazásba, jelentkezzen be a klasszikus portálon létrehozott **Web Apps**, és válassza ki **WebDemoWebApp** a a **Web Apps** lista. A webes alkalmazás irányítópult-oldalon, kattintson **Tallózás** (kattintson az URL-címet, vagy `webdemowebapp.azurewebsites.net`) is. Egy üres helyőrző lap jelenik meg, mert nincs tartalom is közzé lett téve a webalkalmazáson még.

Ezután létrehoz a "Hello, World" alkalmazást, és központilag telepítenie kell a webalkalmazást.

### <a name="create-a-jsp-hello-world-application"></a>JSP Hello World-alkalmazás létrehozása
#### <a name="create-the-application"></a>Az alkalmazás létrehozása
Ahhoz, hogy bemutatják, hogyan lehet a webes alkalmazás központi telepítése, az alábbi eljárás bemutatja, hogyan hozzon létre egy egyszerű "Hello, World" Java-alkalmazást, és töltse fel az App Service Web-alkalmazást létrehozni az alkalmazást.

1. Kattintson a **fájl > Új > dinamikus webes projekt**. Nevezze el a következőképpen: `JSPHello`. Nem kell módosítania a ezen a párbeszédpanelen egyéb beállításait. Kattintson a **Befejezés** gombra.
   
    ![][3]
2. A Project Explorer bontsa ki a **JSPHello** projektre, kattintson a jobb gombbal **WebContent**, majd kattintson a **új > JSP-fájl**. Új JSP-fájl párbeszédablakban nevezze el az új fájlt `index.jsp`. Kattintson a **Tovább** gombra.
3. Az a **JSP-sablon kiválasztása** párbeszédpanelen válassza **új JSP-fájl (html)** kattintson **Befejezés**.
4. Index.jsp, adja hozzá a következő kódot a `<head>` és `<body>` szakaszok címkét:
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, the time is <%= date %> 
        </body>

#### <a name="run-the-hello-world-application-in-localhost"></a>Futtassa a Hello World alkalmazásról localhost
Ez az alkalmazás futtatása előtt kell néhány tulajdonságainak konfigurálása.

1. Kattintson a jobb gombbal a **JSPHello** projektre, és válassza ki **tulajdonságok**.
2. Az a **tulajdonságok** párbeszédpanel: Jelölje ki **Java Build elérési**, jelölje be a **rendezés és exportálása** lapon jelölje **JRE rendszerkönyvtár**, kattintson a **Mentése** áthelyezni a lista elejére.
   
    ![][4]
3. Is a **tulajdonságok** párbeszédpanel: válasszon **megcélzott futtatókörnyezetek** kattintson **új**.
4. Az a **új kiszolgáló Futtatókörnyezetét** párbeszédpanelen válasszon ki egy kiszolgálót, mint **Apache Tomcat v7.0** kattintson **következő**. Az a **Tomcat kiszolgálót** párbeszédpanelen, a set **neve** való `Apache Tomcat v7.0`, és állítsa be **Tomcat telepítési könyvtárát** azt a könyvtárat, amelyben a Tomcat verziójának telepítése használni kívánt kiszolgálót.
   
    ![][5]
   
    Kattintson a **Befejezés** gombra.
5. Majd térjen vissza a **megcélzott futtatókörnyezetek** oldalán a **tulajdonságok** párbeszédpanel. Válassza ki **Apache Tomcat v7.0**, majd kattintson a **OK**.
   
    ![][6]
6. Az az eclipse-ben **futtatása** menüben kattintson a **futtatása**. Az a **futtató** párbeszédablakban válassza **futtassa a kiszolgálón**. Az a **futtassa a kiszolgálón** párbeszédablakban válassza **Tomcat v7.0 Server**:
   
    ![][7]
   
    Kattintson a **Befejezés** gombra.
7. Az alkalmazás futtatásakor, kell megjelennie a **JSPHello** lap jelenik meg az eclipse-ben localhost ablakban (`http://localhost:8080/JSPHello/`), a következő üzenet:
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-the-application-as-a-war"></a>Az alkalmazások exportálásáról a WAR-fájlként
A web project fájlok exportálása egy webes archívumfájl (WAR), hogy a webes alkalmazás ezután telepítheti azt. A következő webes projekt fájljai a WebContent mappában találhatók:

    META-INF
    WEB-INF
    index.jsp

1. Kattintson a jobb gombbal a WebContent mappát, és válassza ki **exportálása**.
2. A a **exportálása kiválasztása** párbeszédpanel, kattintson a **Web > WAR** fájlt, majd kattintson az **tovább**.
3. Az a **WAR exportálása** párbeszédpanelen válassza ki a src könyvtár az aktuális projektben, és a végén a WAR-fájl nevét. Példa:
   
    `<project-path>/JSPHello/src/JSPHello.war`

WAR fájlok telepítésével kapcsolatos további információkért lásd: [hozzáadása a Java-alkalmazások az Azure App Service Web Apps](web-sites-java-add-app.md).

### <a name="deploying-the-hello-world-application-using-ftp"></a>A Hello World alkalmazás használatával az FTP telepítése
Válassza ki a külső FTP-ügyfél számára tegye közzé az alkalmazást. Ez az eljárás ismerteti a két lehetőség közül választhat: a Kudu konzol épített Azure; és FileZilla, olyan népszerű eszköz egy kényelmes, grafikus felhasználói felületen.

> **Megjegyzés:** Eclipse Azure eszköztára támogatja a központi telepítést, hogy a storage-fiókok és a felhőalapú szolgáltatások, de jelenleg nem támogatja a webalkalmazások központi telepítés. Storage-fiókokra telepítheti, és a felhőalapú Azure telepítési projekt segítségével, a szolgáltatások [egy Hello World alkalmazás létrehozása az Azure az eclipse-ben](http://msdn.microsoft.com/library/azure/hh690944.aspx), de nem a webalkalmazások. Más módszerekkel például az FTP- vagy GitHub fájlok átvitele a webes alkalmazást.
> 
> **Megjegyzés:** FTP-használ, a Windows parancssorából (a parancssori FTP.EXE segédprogram, amely a Windows részét képező) nem ajánlott. FTP-ügyfelek, amelyek aktív FTP, például a FTP.EXE, gyakran nem működik a tűzfalon keresztül. Aktív FTP címet adja meg egy belső LAN-alapú, amelyhez az FTP-kiszolgáló lesz valószínűleg nem tudnak majd kapcsolódni.
> 
> 

Egy App Service webalkalmazásba FTP használatával telepített további információkért lásd a következő témaköröket:

* [Az FTP-segédprogrammal történő telepítése](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a>Üzembe helyezési hitelesítő adatok beállítása
Győződjön meg arról, hogy futtatta a **AzureWebDemo** alkalmazás hozzon létre egy webalkalmazást. Ezen a helyen lesz átvitelhez.

1. Jelentkezzen be a klasszikus portálra, és kattintson a **webalkalmazások**. Győződjön meg arról, hogy **WebDemoWebApp** jelenik meg a webes alkalmazások listájának, és győződjön meg arról, hogy fut-e. Kattintson a **WebDemoWebApp** megnyitásához a **irányítópult** lap.
2. A a **irányítópult** lap **gyors áttekintése**, kattintson **az üzembe helyezési hitelesítő adatok beállítása** (Ha már rendelkezik üzembe helyezési hitelesítő adatokat, ez olvassa be  **A központi telepítési hitelesítő adatok alaphelyzetbe állítása**).
   
    Üzembe helyezési hitelesítő adatok társított Microsoft-fiókkal. Meg kell adnia a felhasználónevet és jelszót, amely segítségével telepítheti a Git és az FTP használatával. Ezek a hitelesítő adatok segítségével telepítése a Microsoft-fiókjához társított összes Azure-előfizetések bármely webalkalmazásban. Adja meg a Git és az FTP telepítési hitelesítő adatok a párbeszédpanelen, majd jegyezze fel a felhasználónevet és jelszót későbbi használatra.

#### <a name="get-ftp-connection-information"></a>FTP-kiszolgáló kapcsolati adatainak lekérése
FTP központi telepítéséhez használandó alkalmazásfájlok az újonnan létrehozott webalkalmazáshoz, meg kell szereznie kapcsolati adatokat. Két módon lehet kapcsolati információkhoz. Egyik módja az, hogy keresse fel a web app **irányítópult** lapon; a más módon kell letölteni a webes alkalmazás közzétételi profil. A közzétételi profil XML-fájl, amely például FTP állomás nevét és a bejelentkezési hitelesítő adatokat biztosít a webalkalmazások Azure App Service-ben. Felhasználónév és jelszó használatával telepítheti az az Azure-fiókra, nem csak a másikat társított előfizetéseket bármely webalkalmazásban.

A webalkalmazás panelen az FTP-kiszolgáló kapcsolati adatainak beszerzése a [Azure Portal][Azure Portal]:

1. A **Essentials**, található, és másolja a **FTP-állomásnév**. Ez a hasonló URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.
2. A **Essentials**, található, és másolja **FTP vagy üzembe helyező felhasználónév**. Ez lesz az űrlap *webappname\deployment-username*, például `WebDemoWebApp\deployer77`.

Az FTP-kapcsolati adatok lekérését a közzétételi profil:

1. A webalkalmazás panelen kattintson **Get közzétételi profil**. A .publishsettings fájl letöltése a helyi meghajtóra.
2. Nyissa meg a .publishsettings-fájlt egy XML-szerkesztőben vagy szövegszerkesztőben, és keresse a `<publishProfile>` elemet tartalmazó `publishMethod="FTP"`. Például a következő formában:
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. Vegye figyelembe, hogy a webalkalmazás `publishProfile` beállítások térkép FileZilla kezelő beállításai az alábbiak szerint:

* `publishUrl`ugyanaz, mint **FTP-állomás neve**, az érték beállítása **állomás**.
* `publishMethod="FTP"`azt jelenti, hogy megadta **protokoll** való **FTP - File Transfer Protocol**, és **titkosítási** való **egyszerű FTP használata**.
* `userName`és `userPWD` kulcsok tényleges felhasználónév és jelszó értékeinek meg, ha alaphelyzetbe állítja az üzembe helyezési hitelesítő adatok. `userName`ugyanaz, mint **telepítési / FTP-felhasználó**. A leképezik **felhasználói** és **jelszó** a FileZilla.
* `ftpPassiveMode="True"`azt jelenti, hogy az FTP-hely használja-e a passzív FTP-átvitel; Válassza ki **passzív** a a **átvitel beállításainak** fülre.

#### <a name="configure-the-web-app-to-host-a-java-application"></a>A webalkalmazás üzemeltetéséhez Java-alkalmazások konfigurálása
Az alkalmazás közzététele előtt kell néhány konfigurációs beállításokat módosítaná, így a web app, Java-alkalmazások rendelkezhet.

1. A klasszikus portálon lépjen a webes alkalmazás **irányítópult** lapot, és kattintson **konfigurálása**. Az a **konfigurálása** lapján adja meg a következő beállításokat.
2. A **Java-verziót** az alapértelmezett érték **ki**; válassza ki a Java verzióját a alkalmazás célkitűzések, például a 1.7.0_51. Ezt követően ellenőrizze azt is, amely **webes tároló** Tomcat Server verzióra van beállítva.
3. A **alapértelmezett dokumentumok**, adja hozzá az index.jsp, és akár a lista elejére helyezze. (Az alapértelmezett webes alkalmazások fájlja hostingstart.html.)
4. Kattintson a **Save** (Mentés) gombra.

#### <a name="publish-your-application-using-kudu"></a>A Kudu használó alkalmazások közzététele
Egy tegye közzé az alkalmazást módja az Azure beépített Kudu hibakereső konzolt használja. A kudu ismert, hogy stabil és konzisztens legyen az App Service Web Apps és Tomcat kiszolgálót. A webalkalmazás a konzol érhető próbálja elérni az egy URL-cím a következő:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Az eljárás végrehajtásához a Kudu konzol itt található: a következő URL-címet; Keresse meg a következő helyről:
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. A felső menüben válassza ki a **konzol Debug > CMD**.
3. A konzol parancssorban navigáljon `/site/wwwroot` (vagy kattintson a `site`, majd `wwwroot` a könyvtár nézetben a lap tetején):
   
    `cd /site/wwwroot`
4. Miután megadta **Java-verziót**, Tomcat kiszolgálót kell létrehoznia a webapps könyvtárba. A konzol parancssorban keresse meg a webapps könyvtárba:
   
    `mkdir webapps`
   
    `cd webapps`
5. Húzza a JSPHello.war `<project-path>/JSPHello/src/` , és helyezze be a Kudu könyvtár nézetben a `/site/wwwroot/webapps`. Nem húzza a "Húzza ide a zip- és feltöltése" területet, mert a Tomcat fog csomagolja ki.
   
   ![][8]

Első JSPHello.war, megjelenik a könyvtár munkaterület önmagában:

  ![][9]

Rövid időn belül (valószínűleg 5 percnél kevesebb) Tomcat kiszolgálót fog csomagolja ki a WAR-fájlt egy kicsomagolt JSPHello könyvtárba. Kattintson a gyökérkönyvtár, hogy index.jsp unzipped és másolásához. Ha igen, lépjen vissza a webapps könyvtárba, megtekintéséhez, hogy a kibontott JSPHello directory készült. Ha nem látja ezeket az elemeket, várja meg, és ismételje meg.

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a>A FileZilla (nem kötelező) használó alkalmazások közzététele
Egy másik eszköz segítségével teheti közzé az alkalmazást az FileZilla, a népszerű külső FTP-ügyfél kényelmesen, grafikus felhasználói felületen. Töltse le, és telepítse a FileZilla [http://filezilla-project.org/](http://filezilla-project.org/) Ha még nem rendelkezik azt. Az ügyfél használatáról további információkért lásd: a [FileZilla dokumentáció](https://wiki.filezilla-project.org/Documentation) és ezt a blogbejegyzést [FTP-ügyfelek - rész 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. Kattintson a FileZilla, **fájl > kezelő**.
2. Az a **kezelő** párbeszédpanel, kattintson a **új hely**. Megjelenik egy új, üres FTP-hely **válasszon bejegyzés** kéri, adjon meg egy nevet. Az alábbi eljárással nevezze el `AzureWebDemo-FTP`.
   
    Az a **általános** lapra, adja meg a következő beállításokat:
   
   * **Állomás:** adja meg a **FTP-állomás neve** az irányítópultról másolt.
   * **Port:** (ezt üresen hagyja, mert ez egy passzív átviteli, és a kiszolgáló használni kívánt portot határozza meg.)
   * **Protokoll:** FTP fájlátviteli protokoll
   * **Titkosítás:** egyszerű FTP használata
   * **Bejelentkezési típusa:** normál
   * **Felhasználó:** adja meg a központi telepítés / FTP-felhasználó, az irányítópultról másolt. Ez a teljes FTP felhasználónév, amely az űrlap *webappname\username*.
   * **Jelszó:** adja meg, hogy a megadott az üzembe helyezési hitelesítő adatok megadása után a jelszót.
     
     Az a **átvitel beállításainak** lapon jelölje be **passzív**.
3. Kattintson a **Connect** (Csatlakozás) gombra. Ha sikeres, FileZilla tartozó konzol megjeleníti a `Status: Connected` üzenet és a probléma egy `LIST` paranccsal listát készíthet a könyvtár tartalma.
4. Az a **helyi** hely panelen, jelölje ki a forráskönyvtár a JSPHello.war fájl található; az elérési út az alábbihoz hasonló lesz:
   
    `<project-path>/JSPHello/src/`
5. Az a **távoli** hely panelen, jelölje ki a célmappát. Ha telepíti a WAR-fájlt a `webapps` könyvtárhoz, a webes alkalmazás legfelső szintű. Navigáljon a `/site/wwwroot`, kattintson a jobb gombbal a `wwwroot`, és válassza ki **könyvtár létrehozása**. A könyvtár nevet `webapps` , és írja be a könyvtárhoz.
6. A JSPHello.war átviteli `/site/wwwroot/webapps`. Válassza ki a JSPHello.war a **helyi** lista fájlt, kattintson a jobb gombbal a, és válassza ki **feltöltése**. Akkor jelenik meg kell megjelennie `/site/wwwroot/webapps`.
7. Miután JSPHello.war másolta a webapps könyvtárba, Tomcat kiszolgálót automatikusan csomagolja ki (csomagolja ki) a fájlokat a WAR-fájlt. Bár Tomcat kiszolgálót megkezdése kicsomagolása szinte azonnal is telhet, mire hosszú idő (valószínűleg óra) a fájlok megjelennek az FTP-ügyfél számára.

#### <a name="run-the-hello-world-application-on-the-web-app"></a>A webalkalmazás a Hello World az alkalmazás futtatása
1. Miután a WAR-fájl feltöltése, és ellenőrizte a Tomcat kiszolgálón egy kicsomagolt létrehozott `JSPHello` könyvtár, tallózással keresse meg a `http://webdemowebapp.azurewebsites.net/JSPHello` az alkalmazás futtatásához.
   
   > **Megjegyzés:** választva **Tallózás** a klasszikus portálon kaphat az alapértelmezett weblap közli, hogy "a Java-alapú webalkalmazás létrehozása sikeresen befejeződött." Lehetséges, hogy az alkalmazás kimenete helyett az alapértelmezett weblap megtekintéséhez a képernyőn látható weblapon frissítéséhez.
   > 
   > 
2. Az alkalmazás futásakor, megjelenik egy weblap, a következő eredménnyel:
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a>Azure-erőforrások törlése
Ez az eljárás egy App Service webalkalmazásba hoz. A számlázás történik az erőforrás mindaddig, amíg az létezik. Ha szeretné folytatni a web app használatával tesztelési, illetve a fejlesztési, érdemes lehet leállítása vagy törlése. A webes alkalmazás, amely le lett állítva továbbra is fel Önnek egy kis kell fizetni, de bármikor újraindíthatja azt. A webes alkalmazás törlésével feltöltött az összes adatot törli.

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
