---
title: "Hogyan Twilio (Java) a telefonhívás |} Microsoft Docs"
description: "Útmutató: Azure Java-alkalmazások használatával Twilio weblapról telefonhívás."
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
ms.openlocfilehash: 04ecb80a2a9e15b549b47138caf71c7e64bda500
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a><span data-ttu-id="5e45d-103">Hogyan telefonhívás Twilio Azure Java-alkalmazások használata</span><span class="sxs-lookup"><span data-stu-id="5e45d-103">How to Make a Phone Call Using Twilio in a Java Application on Azure</span></span>
<span data-ttu-id="5e45d-104">A következő példa bemutatja, hogyan használható fel a Twilio Azure-ban üzemeltetett weblapok hívást.</span><span class="sxs-lookup"><span data-stu-id="5e45d-104">The following example shows you how you can use Twilio to make a call from a web page hosted in Azure.</span></span> <span data-ttu-id="5e45d-105">Az eredményül kapott alkalmazás fogja kérni a felhasználót a telefonhívás-értékeket, az az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="5e45d-105">The resulting application will prompt the user for phone call values, as shown in the following screen shot.</span></span>

![Az Azure hívás űrlap Twilio és Java használatával][twilio_java]

<span data-ttu-id="5e45d-107">A következő használatára az ebben a témakörben lévő lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="5e45d-107">You'll need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="5e45d-108">Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="5e45d-108">Acquire a Twilio account and authentication token.</span></span> <span data-ttu-id="5e45d-109">Twilio megkezdéséhez, értékelje ki a következő árképzési [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="5e45d-109">To get started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="5e45d-110">Iratkozzon fel a következő [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="5e45d-110">You can sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="5e45d-111">Az API-t Twilio által biztosított kapcsolatos információkért lásd: [http://www.twilio.com/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="5e45d-111">For information about the API provided by Twilio, see [http://www.twilio.com/api][twilio_api].</span></span>
2. <span data-ttu-id="5e45d-112">Szerezze be a Twilio JAR.</span><span class="sxs-lookup"><span data-stu-id="5e45d-112">Obtain the Twilio JAR.</span></span> <span data-ttu-id="5e45d-113">A [https://github.com/twilio/twilio-java][twilio_java_github], töltse le a GitHub-adatforrások és létrehozhat saját JAR, vagy egy előre elkészített JAR (vagy anélkül függőségek) letöltése.</span><span class="sxs-lookup"><span data-stu-id="5e45d-113">At [https://github.com/twilio/twilio-java][twilio_java_github], you can download the GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
   <span data-ttu-id="5e45d-114">Ebben a témakörben a kódot az előre elkészített TwilioJava-3.3.8-az-függőségek JAR használatával készült.</span><span class="sxs-lookup"><span data-stu-id="5e45d-114">The code in this topic was written using the pre-built TwilioJava-3.3.8-with-dependencies JAR.</span></span>
3. <span data-ttu-id="5e45d-115">A JAR hozzáadása a Java build elérési útját.</span><span class="sxs-lookup"><span data-stu-id="5e45d-115">Add the JAR to your Java build path.</span></span>
4. <span data-ttu-id="5e45d-116">Használatakor Eclipse létrehozni a Java-alkalmazást, vegye fel a Twilio-JAR az alkalmazás központi telepítési fájl (WAR) Eclipse meg központi telepítési szerelvény funkció használata.</span><span class="sxs-lookup"><span data-stu-id="5e45d-116">If you are using Eclipse to create this Java application, include the Twilio JAR in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="5e45d-117">Ha nem használ Eclipse létrehozni a Java-alkalmazást, győződjön meg arról, a Twilio-JAR belüli, a Java-alkalmazások Azure ugyanarra a szerepkörre, és megadható az alkalmazás osztály elérési.</span><span class="sxs-lookup"><span data-stu-id="5e45d-117">If you are not using Eclipse to create this Java application, ensure the Twilio JAR is included within the same Azure role as your Java application, and added to the class path of your application.</span></span>
5. <span data-ttu-id="5e45d-118">Győződjön meg arról, a cacerts keystore az MD5 ujjlenyomat 67:CB:9 D Equifax biztonságos hitelesítésszolgáltató tanúsítványát tartalmazza: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (sorozatszám 35:DE:F4:CF pedig a SHA1 ujjlenyomat D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="5e45d-118">Ensure your cacerts keystore contains the Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (the serial number is 35:DE:F4:CF and the SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="5e45d-119">Ez az tanúsítvány hitelesítésszolgáltatói tanúsítványa a [https://api.twilio.com] [ twilio_api_service] szolgáltatást, amelyet a Twilio API-k használatakor nevezik.</span><span class="sxs-lookup"><span data-stu-id="5e45d-119">This is the certificate authority (CA) certificate for the [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="5e45d-120">A CA-tanúsítványt a JDK cacert tárolóban történő hozzáadásával kapcsolatos további információkért lásd: [tanúsítvány hozzáadása a Java hitelesítésszolgáltató tanúsítványtároló][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="5e45d-120">For information about adding this CA certificate to your JDK's cacert store, see [Adding a Certificate to the Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="5e45d-121">Emellett az információkért ismeretét [létrehozása egy Hello World használó Eclipse Azure eszköztára][azure_java_eclipse_hello_world], vagy az egyéb technikák üzemeltetéséhez Java-alkalmazások az Azure-ban, ha nem használja az eclipse-ben, erősen ajánlott.</span><span class="sxs-lookup"><span data-stu-id="5e45d-121">Additionally, familiarity with the information at [Creating a Hello World Application Using the Azure Toolkit for Eclipse][azure_java_eclipse_hello_world], or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="5e45d-122">A következő hívással webes űrlap létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e45d-122">Create a web form for making a call</span></span>
<span data-ttu-id="5e45d-123">A következő kód bemutatja, hogyan hozzon létre egy webes űrlap egy hívás a felhasználói adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5e45d-123">The following code shows how to create a web form to retrieve user data for making a call.</span></span> <span data-ttu-id="5e45d-124">Ebben a példában egy új dinamikus webes projekt alkalmazásában nevű **TwilioCloud**, hozták létre, és **callform.jsp** JSP-fájlként lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="5e45d-124">For purposes of this example, a new dynamic web project, named **TwilioCloud**, was created, and **callform.jsp** was added as a JSP file.</span></span>

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
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
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

## <a name="create-the-code-to-make-the-call"></a><span data-ttu-id="5e45d-125">A kód a híváshoz létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e45d-125">Create the code to make the call</span></span>
<span data-ttu-id="5e45d-126">Az alábbi kód, amely nevezzük, amikor a felhasználó befejezi az űrlap callform.jsp által megjelenített, a hívás-üzenetet hoz létre, és állít elő, a hívást.</span><span class="sxs-lookup"><span data-stu-id="5e45d-126">The following code, which is called when the user completes the form displayed by callform.jsp, creates the call message and generates the call.</span></span> <span data-ttu-id="5e45d-127">Az ebben a példában a JSP-fájl neve **makecall.jsp** és hozzá lett adva a **TwilioCloud** projekt.</span><span class="sxs-lookup"><span data-stu-id="5e45d-127">For purposes of this example, the JSP file is named **makecall.jsp** and was added to the **TwilioCloud** project.</span></span> <span data-ttu-id="5e45d-128">(Twilio-fiókja és -hitelesítési token helyett a rendelt helyőrző értékeket használja **accountSID** és **authToken** az alábbi kódban.)</span><span class="sxs-lookup"><span data-stu-id="5e45d-128">(Use your Twilio account and authentication token instead of the placeholder values assigned to **accountSID** and **authToken** in the code below.)</span></span>

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
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");

         // Place the call From, To and URL values into a hash map. 
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

<span data-ttu-id="5e45d-129">Azonkívül, hogy a hívás, makecall.jsp a Twilio-végpont, API-verziót és a hívás állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5e45d-129">In addition to making the call, makecall.jsp displays the Twilio endpoint, API version, and the call status.</span></span> <span data-ttu-id="5e45d-130">Példa: az alábbi képernyőfelvételhez:</span><span class="sxs-lookup"><span data-stu-id="5e45d-130">An example is the following screen shot:</span></span>

![Azure hívási válasz Twilio és Java használatával][twilio_java_response]

## <a name="run-the-application"></a><span data-ttu-id="5e45d-132">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="5e45d-132">Run the application</span></span>
<span data-ttu-id="5e45d-133">Az alábbiakban a magas szintű lépéseket kell futtatni az alkalmazást; a következő lépéseket találhatók információk [létrehozása egy Hello World használó Eclipse Azure eszköztára][azure_java_eclipse_hello_world].</span><span class="sxs-lookup"><span data-stu-id="5e45d-133">Following are the high-level steps to run your application; details for these steps can be found at [Creating a Hello World Application Using the Azure Toolkit for Eclipse][azure_java_eclipse_hello_world].</span></span>

1. <span data-ttu-id="5e45d-134">Exportálja a TwilioCloud WAR az Azure **approot** mappát.</span><span class="sxs-lookup"><span data-stu-id="5e45d-134">Export your TwilioCloud WAR to the Azure **approot** folder.</span></span> 
2. <span data-ttu-id="5e45d-135">Módosítsa **startup.cmd** a TwilioCloud WAR kibontásához.</span><span class="sxs-lookup"><span data-stu-id="5e45d-135">Modify **startup.cmd** to unzip your TwilioCloud WAR.</span></span>
3. <span data-ttu-id="5e45d-136">A compute Emulator az alkalmazás fordítása.</span><span class="sxs-lookup"><span data-stu-id="5e45d-136">Compile your application for the compute emulator.</span></span>
4. <span data-ttu-id="5e45d-137">A központi telepítés elindítása a compute emulator.</span><span class="sxs-lookup"><span data-stu-id="5e45d-137">Start your deployment in the compute emulator.</span></span>
5. <span data-ttu-id="5e45d-138">Nyisson meg egy böngészőt, és futtassa **http://localhost:8080/TwilioCloud/callform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="5e45d-138">Open a browser, and run **http://localhost:8080/TwilioCloud/callform.jsp**.</span></span>
6. <span data-ttu-id="5e45d-139">Adja meg az értékeket a képernyőn, kattintson a **hívás kezdeményezéséhez**, majd tekintse meg az eredményeket a makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="5e45d-139">Enter values in the form, click **Make this call**, and then see the results in makecall.jsp.</span></span>

<span data-ttu-id="5e45d-140">Amikor készen áll az Azure-ba, recompile a felhőbe történő központi telepítéséhez telepítse az Azure, és futtassa a http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp a böngészőben (helyettesítse be a következő *your_hosted_name*).</span><span class="sxs-lookup"><span data-stu-id="5e45d-140">When you are ready to deploy to Azure, recompile for deployment to the cloud, deploy to Azure, and run http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in the browser (substitute your value for *your_hosted_name*).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e45d-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e45d-141">Next steps</span></span>
<span data-ttu-id="5e45d-142">Ez a kód Azure Java nyelven Twilio használatával alapvető funkciókat mutatjuk be lett megadva.</span><span class="sxs-lookup"><span data-stu-id="5e45d-142">This code was provided to show you basic functionality using Twilio in Java on Azure.</span></span> <span data-ttu-id="5e45d-143">Mielőtt telepítené az Azure éles környezetben, érdemes lehet további hibakezelés vagy egyéb szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="5e45d-143">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="5e45d-144">Példa:</span><span class="sxs-lookup"><span data-stu-id="5e45d-144">For example:</span></span>

* <span data-ttu-id="5e45d-145">Egy webes űrlap használata helyett segítségével az Azure storage blobsba vagy SQL-adatbázis tárolja a telefonszámokat, és hívja meg a szöveg.</span><span class="sxs-lookup"><span data-stu-id="5e45d-145">Instead of using a web form, you could use Azure storage blobs or SQL Database to store phone numbers and call text.</span></span> <span data-ttu-id="5e45d-146">Az Azure storage-blobot, amely Java használatával kapcsolatos információkért lásd: [használata a Blob Storage szolgáltatást Java][howto_blob_storage_java].</span><span class="sxs-lookup"><span data-stu-id="5e45d-146">For information about using Azure storage blobs in Java, see [How to Use the Blob Storage Service from Java][howto_blob_storage_java].</span></span> <span data-ttu-id="5e45d-147">SQL-adatbázis a Java használatával kapcsolatos információkért lásd: [SQL-adatbázis használata a Java][howto_sql_azure_java].</span><span class="sxs-lookup"><span data-stu-id="5e45d-147">For information about using SQL Database in Java, see [Using SQL Database in Java][howto_sql_azure_java].</span></span>
* <span data-ttu-id="5e45d-148">Használhat **RoleEnvironment.getConfigurationSettings** beolvasni a Twilio Fiókazonosítót és a hitelesítési token a központi telepítés konfigurációs beállításai, bekódolásának makecall.jsp szereplő értékek helyett.</span><span class="sxs-lookup"><span data-stu-id="5e45d-148">You could use **RoleEnvironment.getConfigurationSettings** to retrieve the Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding the values in makecall.jsp.</span></span> <span data-ttu-id="5e45d-149">További információ a **roleenvironment-et** osztály című [JSP az Azure-szolgáltatás futásidejű kódtár használatával] [ azure_runtime_jsp] és az Azure szolgáltatás futásidejű csomag dokumentációját a [http://dl.windowsazure.com/javadoc][azure_javadoc].</span><span class="sxs-lookup"><span data-stu-id="5e45d-149">For information about the **RoleEnvironment** class, see [Using the Azure Service Runtime Library in JSP][azure_runtime_jsp] and the Azure Service Runtime package documentation at [http://dl.windowsazure.com/javadoc][azure_javadoc].</span></span>
* <span data-ttu-id="5e45d-150">A makecall.jsp kódot hozzárendel egy Twilio-megadott URL-címet, [http://twimlets.com/message][twimlet_message_url], hogy a **URL-cím** változó.</span><span class="sxs-lookup"><span data-stu-id="5e45d-150">The makecall.jsp code assigns a Twilio-provided URL, [http://twimlets.com/message][twimlet_message_url], to the **Url** variable.</span></span> <span data-ttu-id="5e45d-151">Az URL-cím, amely arról értesíti a Twilio való hívása Twilio Markup Language (TwiML) választ biztosít.</span><span class="sxs-lookup"><span data-stu-id="5e45d-151">This URL provides a Twilio Markup Language (TwiML) response that informs Twilio how to proceed with the call.</span></span> <span data-ttu-id="5e45d-152">A visszaadott TwiML tartalmazhat például egy  **&lt;szóbeli&gt;**  művelet, amely a hívás címzett felolvasását szöveg eredményez.</span><span class="sxs-lookup"><span data-stu-id="5e45d-152">For example, the TwiML that is returned can contain a **&lt;Say&gt;** verb that results in text being spoken to the call recipient.</span></span> <span data-ttu-id="5e45d-153">Ahelyett, hogy a Twilio-megadott URL-címet, a saját Twilio tartozó kérelem; szolgáltatás létrehozása sikerült További információkért lásd: [hang-és SMS-képességek a Java használatát Twilio hogyan][howto_twilio_voice_sms_java].</span><span class="sxs-lookup"><span data-stu-id="5e45d-153">Instead of using the Twilio-provided URL, you could build your own service to respond to Twilio's request; for more information, see [How to Use Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java].</span></span> <span data-ttu-id="5e45d-154">További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml], és további információ a  **&lt;szóbeli&gt;**  és egyéb Twilio művelet található a [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="5e45d-154">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about **&lt;Say&gt;** and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="5e45d-155">Olvassa el a Twilio biztonsági irányelvek a [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="5e45d-155">Read the Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="5e45d-156">További információ a Twilio: [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="5e45d-156">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="5e45d-157">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5e45d-157">See Also</span></span>
* <span data-ttu-id="5e45d-158">[Hang-és SMS képességei Java Twilio használata][howto_twilio_voice_sms_java]</span><span class="sxs-lookup"><span data-stu-id="5e45d-158">[How to Use Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java]</span></span>
* <span data-ttu-id="5e45d-159">[A tanúsítvány felvétele a Java Hitelesítésszolgáltatói tanúsítványok tárába][add_ca_cert]</span><span class="sxs-lookup"><span data-stu-id="5e45d-159">[Adding a Certificate to the Java CA Certificate Store][add_ca_cert]</span></span>

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
