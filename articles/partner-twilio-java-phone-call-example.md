---
title: "aaaHow tooMake Twilio (Java) a telefonhívás |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefon felelnek meg, és egy weblapot Twilio Azure Java-alkalmazásokban."
services: 
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 0381789e-e775-41a0-a784-294275192b1d
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 04fe5a78d431a79790dee3ca75c2b004aea4345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-java-application-on-azure"></a><span data-ttu-id="62a31-103">Hogyan tooMake a telefonhívás használatával Twilio Azure Java-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="62a31-103">How tooMake a Phone Call Using Twilio in a Java Application on Azure</span></span>
<span data-ttu-id="62a31-104">hello következő példa bemutatja, hogyan használható Twilio toomake Azure-ban üzemeltetett weblapok hívásakor.</span><span class="sxs-lookup"><span data-stu-id="62a31-104">hello following example shows you how you can use Twilio toomake a call from a web page hosted in Azure.</span></span> <span data-ttu-id="62a31-105">hello eredményül kapott alkalmazás fogja kérni a telefonhívás értékeket hello felhasználót, ahogy az alábbi képernyőfelvétel a hello.</span><span class="sxs-lookup"><span data-stu-id="62a31-105">hello resulting application will prompt hello user for phone call values, as shown in hello following screen shot.</span></span>

![Az Azure hívás űrlap Twilio és Java használatával][twilio_java]

<span data-ttu-id="62a31-107">Szüksége lesz a toodo hello következő toouse hello kód ebben a témakörben:</span><span class="sxs-lookup"><span data-stu-id="62a31-107">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="62a31-108">Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="62a31-108">Acquire a Twilio account and authentication token.</span></span> <span data-ttu-id="62a31-109">lépések a Twilio, tooget kiértékelése árképzési [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="62a31-109">tooget started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="62a31-110">Iratkozzon fel a következő [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="62a31-110">You can sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="62a31-111">Hello Twilio által nyújtott API kapcsolatos információkért lásd: [http://www.twilio.com/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="62a31-111">For information about hello API provided by Twilio, see [http://www.twilio.com/api][twilio_api].</span></span>
2. <span data-ttu-id="62a31-112">Szerezze be a Twilio-JAR hello.</span><span class="sxs-lookup"><span data-stu-id="62a31-112">Obtain hello Twilio JAR.</span></span> <span data-ttu-id="62a31-113">A [https://github.com/twilio/twilio-java][twilio_java_github], hello GitHub forrásból töltse le és létrehozhat saját JAR, vagy egy előre elkészített JAR (vagy anélkül függőségek) letöltése.</span><span class="sxs-lookup"><span data-stu-id="62a31-113">At [https://github.com/twilio/twilio-java][twilio_java_github], you can download hello GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
   <span data-ttu-id="62a31-114">Ebben a témakörben hello kód előzetesen elkészített TwilioJava-3.3.8-az-függőségek JAR hello használatával készült.</span><span class="sxs-lookup"><span data-stu-id="62a31-114">hello code in this topic was written using hello pre-built TwilioJava-3.3.8-with-dependencies JAR.</span></span>
3. <span data-ttu-id="62a31-115">Adja hozzá a hello JAR tooyour Java build elérési útja.</span><span class="sxs-lookup"><span data-stu-id="62a31-115">Add hello JAR tooyour Java build path.</span></span>
4. <span data-ttu-id="62a31-116">Eclipse toocreate a Java-alkalmazást használ, ha Twilio JAR hello szerepeljenek az alkalmazás központi telepítési fájl (WAR) Eclipse meg központi telepítési szerelvény funkció használata.</span><span class="sxs-lookup"><span data-stu-id="62a31-116">If you are using Eclipse toocreate this Java application, include hello Twilio JAR in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="62a31-117">Ha az Eclipse toocreate a Java-alkalmazás nem használ, győződjön meg arról, Twilio JAR hello hello szerepel a Java-alkalmazás, és a hozzáadott toohello osztály az alkalmazás elérési útjaként azonos Azure szerepkör.</span><span class="sxs-lookup"><span data-stu-id="62a31-117">If you are not using Eclipse toocreate this Java application, ensure hello Twilio JAR is included within hello same Azure role as your Java application, and added toohello class path of your application.</span></span>
5. <span data-ttu-id="62a31-118">Győződjön meg arról, a cacerts keystore hello Equifax biztonságos hitelesítésszolgáltató tanúsítványát az MD5 ujjlenyomat 67:CB:9 D tartalmazza: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello soros száma 35:DE:F4:CF és hello SHA1 ujjlenyomat D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="62a31-118">Ensure your cacerts keystore contains hello Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello serial number is 35:DE:F4:CF and hello SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="62a31-119">Ez az hello tanúsítvány hitelesítésszolgáltatói tanúsítvány hello [https://api.twilio.com] [ twilio_api_service] szolgáltatást, amelyet a Twilio API-k használatakor nevezik.</span><span class="sxs-lookup"><span data-stu-id="62a31-119">This is hello certificate authority (CA) certificate for hello [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="62a31-120">A Hitelesítésszolgáltatói tanúsítvány tooyour JDK cacert tároló hozzáadásával kapcsolatos további információkért lásd: [hozzáadása a Java-hitelesítésszolgáltató tanúsítványtároló tanúsítvány toohello][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="62a31-120">For information about adding this CA certificate tooyour JDK's cacert store, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="62a31-121">Emellett a tesztkörnyezet hello információkat, [Hello World használó hello Azure eszköztára Eclipse létrehozása][azure_java_eclipse_hello_world], vagy egyéb technikák, ha az Azure Java-alkalmazások, nem használja az eclipse-ben, erősen ajánlott.</span><span class="sxs-lookup"><span data-stu-id="62a31-121">Additionally, familiarity with hello information at [Creating a Hello World Application Using hello Azure Toolkit for Eclipse][azure_java_eclipse_hello_world], or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="62a31-122">A következő hívással webes űrlap létrehozása</span><span class="sxs-lookup"><span data-stu-id="62a31-122">Create a web form for making a call</span></span>
<span data-ttu-id="62a31-123">hello a következő kód bemutatja, hogyan toocreate egy webes űrlap tooretrieve felhasználó adatai a következő hívással.</span><span class="sxs-lookup"><span data-stu-id="62a31-123">hello following code shows how toocreate a web form tooretrieve user data for making a call.</span></span> <span data-ttu-id="62a31-124">Ebben a példában egy új dinamikus webes projekt alkalmazásában nevű **TwilioCloud**, hozták létre, és **callform.jsp** JSP-fájlként lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="62a31-124">For purposes of this example, a new dynamic web project, named **TwilioCloud**, was created, and **callform.jsp** was added as a JSP file.</span></span>

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is hello call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-hello-code-toomake-hello-call"></a><span data-ttu-id="62a31-125">Hello kód toomake hello hívás létrehozása</span><span class="sxs-lookup"><span data-stu-id="62a31-125">Create hello code toomake hello call</span></span>
<span data-ttu-id="62a31-126">hello alábbi kód, amely hello felhasználói befejezéséről hello űrlap callform.jsp által megjelenített neve, hívás üdvözlőüzenetére hoz létre és hello hívás állít elő.</span><span class="sxs-lookup"><span data-stu-id="62a31-126">hello following code, which is called when hello user completes hello form displayed by callform.jsp, creates hello call message and generates hello call.</span></span> <span data-ttu-id="62a31-127">Ebben a példában a hello JSP-fájl neve **makecall.jsp** és hozzá lett adva toohello **TwilioCloud** projekt.</span><span class="sxs-lookup"><span data-stu-id="62a31-127">For purposes of this example, hello JSP file is named **makecall.jsp** and was added toohello **TwilioCloud** project.</span></span> <span data-ttu-id="62a31-128">(Twilio-fiókja és -hitelesítési token helyett túl hozzárendelt hello helyőrző értékeket használja**accountSID** és **authToken** a hello kódot.)</span><span class="sxs-lookup"><span data-stu-id="62a31-128">(Use your Twilio account and authentication token instead of hello placeholder values assigned too**accountSID** and **authToken** in hello code below.)</span></span>

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of hello placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of hello Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve hello account, used later tooretrieve hello CallFactory.
         Account account = client.getAccount();

         // Display hello client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display hello API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve hello values entered by hello user.
         String callTo = request.getParameter("callTo");  
         // hello Outgoing Caller ID, used for hello From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in hello user's text with '%20', 
         // toomake hello text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using hello Twilio message and hello user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display hello message URL.
         out.println("<p>");
         out.println("hello URL is " + Url);
         out.println("</p>");

         // Place hello call From, tooand URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);

         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

<span data-ttu-id="62a31-129">Ezenkívül toomaking hello hívás, makecall.jsp hello Twilio-végpont, API-verziót és hello hívás állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="62a31-129">In addition toomaking hello call, makecall.jsp displays hello Twilio endpoint, API version, and hello call status.</span></span> <span data-ttu-id="62a31-130">Példa: a következő képernyőfelvétel hello:</span><span class="sxs-lookup"><span data-stu-id="62a31-130">An example is hello following screen shot:</span></span>

![Azure hívási válasz Twilio és Java használatával][twilio_java_response]

## <a name="run-hello-application"></a><span data-ttu-id="62a31-132">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="62a31-132">Run hello application</span></span>
<span data-ttu-id="62a31-133">Következő vannak hello a magas szintű lépései toorun az alkalmazás; a következő lépéseket találhatók információk [létrehozása egy Hello World használó hello Azure eszköztára Eclipse][azure_java_eclipse_hello_world].</span><span class="sxs-lookup"><span data-stu-id="62a31-133">Following are hello high-level steps toorun your application; details for these steps can be found at [Creating a Hello World Application Using hello Azure Toolkit for Eclipse][azure_java_eclipse_hello_world].</span></span>

1. <span data-ttu-id="62a31-134">Exportálja a TwilioCloud WAR toohello Azure **approot** mappát.</span><span class="sxs-lookup"><span data-stu-id="62a31-134">Export your TwilioCloud WAR toohello Azure **approot** folder.</span></span> 
2. <span data-ttu-id="62a31-135">Módosítsa **startup.cmd** toounzip a TwilioCloud WAR.</span><span class="sxs-lookup"><span data-stu-id="62a31-135">Modify **startup.cmd** toounzip your TwilioCloud WAR.</span></span>
3. <span data-ttu-id="62a31-136">Az alkalmazás hello compute Emulator össze.</span><span class="sxs-lookup"><span data-stu-id="62a31-136">Compile your application for hello compute emulator.</span></span>
4. <span data-ttu-id="62a31-137">Indítsa el a központi telepítés hello compute emulator.</span><span class="sxs-lookup"><span data-stu-id="62a31-137">Start your deployment in hello compute emulator.</span></span>
5. <span data-ttu-id="62a31-138">Nyisson meg egy böngészőt, és futtassa **http://localhost:8080/TwilioCloud/callform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="62a31-138">Open a browser, and run **http://localhost:8080/TwilioCloud/callform.jsp**.</span></span>
6. <span data-ttu-id="62a31-139">A hello formában adja meg az értékeket, kattintson a **hívás kezdeményezéséhez**, és olvassa el a makecall.jsp hello eredményez.</span><span class="sxs-lookup"><span data-stu-id="62a31-139">Enter values in hello form, click **Make this call**, and then see hello results in makecall.jsp.</span></span>

<span data-ttu-id="62a31-140">Amikor készen áll a toodeploy tooAzure, telepítési toohello felhő újrafordítása, tooAzure telepítéséhez és futtatásához http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp hello böngészőben (helyettesítse be a következő *your_hosted_name*).</span><span class="sxs-lookup"><span data-stu-id="62a31-140">When you are ready toodeploy tooAzure, recompile for deployment toohello cloud, deploy tooAzure, and run http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in hello browser (substitute your value for *your_hosted_name*).</span></span>

## <a name="next-steps"></a><span data-ttu-id="62a31-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62a31-141">Next steps</span></span>
<span data-ttu-id="62a31-142">Ez a kód lett megadva tooshow Ön alapvető funkciókat Azure Java nyelven Twilio használatával.</span><span class="sxs-lookup"><span data-stu-id="62a31-142">This code was provided tooshow you basic functionality using Twilio in Java on Azure.</span></span> <span data-ttu-id="62a31-143">Mielőtt éles környezetben tooAzure, érdemes lehet tooadd további hibakezelés és egyéb szolgáltatások esetében.</span><span class="sxs-lookup"><span data-stu-id="62a31-143">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="62a31-144">Példa:</span><span class="sxs-lookup"><span data-stu-id="62a31-144">For example:</span></span>

* <span data-ttu-id="62a31-145">Egy webes űrlap használata helyett sikerült Azure storage blobsba vagy SQL adatbázis toostore telefonszámokat, és hívja a szöveg.</span><span class="sxs-lookup"><span data-stu-id="62a31-145">Instead of using a web form, you could use Azure storage blobs or SQL Database toostore phone numbers and call text.</span></span> <span data-ttu-id="62a31-146">Az Azure storage-blobot, amely Java használatával kapcsolatos információkért lásd: [hogyan tooUse hello Blob Storage szolgáltatással való Java][howto_blob_storage_java].</span><span class="sxs-lookup"><span data-stu-id="62a31-146">For information about using Azure storage blobs in Java, see [How tooUse hello Blob Storage Service from Java][howto_blob_storage_java].</span></span> <span data-ttu-id="62a31-147">SQL-adatbázis a Java használatával kapcsolatos információkért lásd: [SQL-adatbázis használata a Java][howto_sql_azure_java].</span><span class="sxs-lookup"><span data-stu-id="62a31-147">For information about using SQL Database in Java, see [Using SQL Database in Java][howto_sql_azure_java].</span></span>
* <span data-ttu-id="62a31-148">Használhat **RoleEnvironment.getConfigurationSettings** tooretrieve hello Twilio Fiókazonosítót és a hitelesítési token a központi telepítés konfigurációs beállításai, makecall.jsp hello értékek rögzített megadás helyett.</span><span class="sxs-lookup"><span data-stu-id="62a31-148">You could use **RoleEnvironment.getConfigurationSettings** tooretrieve hello Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding hello values in makecall.jsp.</span></span> <span data-ttu-id="62a31-149">Hello információt **roleenvironment-et** osztály című [Using hello Azure szolgáltatás futásidejű kódtár a JSP] [ azure_runtime_jsp] és hello Azure szolgáltatás futásidejű csomag dokumentációban, [http://dl.windowsazure.com/javadoc][azure_javadoc].</span><span class="sxs-lookup"><span data-stu-id="62a31-149">For information about hello **RoleEnvironment** class, see [Using hello Azure Service Runtime Library in JSP][azure_runtime_jsp] and hello Azure Service Runtime package documentation at [http://dl.windowsazure.com/javadoc][azure_javadoc].</span></span>
* <span data-ttu-id="62a31-150">hello makecall.jsp kód hozzárendel egy Twilio-megadott URL-címet, [http://twimlets.com/message][twimlet_message_url], toohello **URL-cím** változó.</span><span class="sxs-lookup"><span data-stu-id="62a31-150">hello makecall.jsp code assigns a Twilio-provided URL, [http://twimlets.com/message][twimlet_message_url], toohello **Url** variable.</span></span> <span data-ttu-id="62a31-151">Az URL-cím, amely tájékoztatja arról, hogyan hívható meg a hello tooproceed Twilio Twilio Markup Language (TwiML) választ biztosít.</span><span class="sxs-lookup"><span data-stu-id="62a31-151">This URL provides a Twilio Markup Language (TwiML) response that informs Twilio how tooproceed with hello call.</span></span> <span data-ttu-id="62a31-152">Hello visszaadott TwiML tartalmazhat például egy  **&lt;szóbeli&gt;**  művelet, amely éppen szóbeli toohello hívás címzettje szöveg eredményez.</span><span class="sxs-lookup"><span data-stu-id="62a31-152">For example, hello TwiML that is returned can contain a **&lt;Say&gt;** verb that results in text being spoken toohello call recipient.</span></span> <span data-ttu-id="62a31-153">Hello Twilio által megadott URL-cím helyett a saját szolgáltatás toorespond tooTwilio kérelem; létrehozása sikerült További információkért lásd: [hogyan tooUse hang-és SMS képességei Java Twilio][howto_twilio_voice_sms_java].</span><span class="sxs-lookup"><span data-stu-id="62a31-153">Instead of using hello Twilio-provided URL, you could build your own service toorespond tooTwilio's request; for more information, see [How tooUse Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java].</span></span> <span data-ttu-id="62a31-154">További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml], és további információ a  **&lt;szóbeli&gt;**  és egyéb Twilio művelet található a [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="62a31-154">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about **&lt;Say&gt;** and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="62a31-155">Olvassa el a hello Twilio biztonsági előírásait a [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="62a31-155">Read hello Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="62a31-156">További információ a Twilio: [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="62a31-156">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="62a31-157">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="62a31-157">See Also</span></span>
* <span data-ttu-id="62a31-158">[Hogyan tooUse Twilio hang-és Java SMS képességei][howto_twilio_voice_sms_java]</span><span class="sxs-lookup"><span data-stu-id="62a31-158">[How tooUse Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java]</span></span>
* <span data-ttu-id="62a31-159">[A tanúsítvány toohello Java hitelesítésszolgáltató tanúsítványtároló hozzáadása][add_ca_cert]</span><span class="sxs-lookup"><span data-stu-id="62a31-159">[Adding a Certificate toohello Java CA Certificate Store][add_ca_cert]</span></span>

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
