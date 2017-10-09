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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Hogyan tooMake a telefonhívás használatával Twilio Azure Java-alkalmazásokban
hello következő példa bemutatja, hogyan használható Twilio toomake Azure-ban üzemeltetett weblapok hívásakor. hello eredményül kapott alkalmazás fogja kérni a telefonhívás értékeket hello felhasználót, ahogy az alábbi képernyőfelvétel a hello.

![Az Azure hívás űrlap Twilio és Java használatával][twilio_java]

Szüksége lesz a toodo hello következő toouse hello kód ebben a témakörben:

1. Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat. lépések a Twilio, tooget kiértékelése árképzési [http://www.twilio.com/pricing][twilio_pricing]. Iratkozzon fel a következő [https://www.twilio.com/try-twilio][try_twilio]. Hello Twilio által nyújtott API kapcsolatos információkért lásd: [http://www.twilio.com/api][twilio_api].
2. Szerezze be a Twilio-JAR hello. A [https://github.com/twilio/twilio-java][twilio_java_github], hello GitHub forrásból töltse le és létrehozhat saját JAR, vagy egy előre elkészített JAR (vagy anélkül függőségek) letöltése.
   Ebben a témakörben hello kód előzetesen elkészített TwilioJava-3.3.8-az-függőségek JAR hello használatával készült.
3. Adja hozzá a hello JAR tooyour Java build elérési útja.
4. Eclipse toocreate a Java-alkalmazást használ, ha Twilio JAR hello szerepeljenek az alkalmazás központi telepítési fájl (WAR) Eclipse meg központi telepítési szerelvény funkció használata. Ha az Eclipse toocreate a Java-alkalmazás nem használ, győződjön meg arról, Twilio JAR hello hello szerepel a Java-alkalmazás, és a hozzáadott toohello osztály az alkalmazás elérési útjaként azonos Azure szerepkör.
5. Győződjön meg arról, a cacerts keystore hello Equifax biztonságos hitelesítésszolgáltató tanúsítványát az MD5 ujjlenyomat 67:CB:9 D tartalmazza: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello soros száma 35:DE:F4:CF és hello SHA1 ujjlenyomat D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Ez az hello tanúsítvány hitelesítésszolgáltatói tanúsítvány hello [https://api.twilio.com] [ twilio_api_service] szolgáltatást, amelyet a Twilio API-k használatakor nevezik. A Hitelesítésszolgáltatói tanúsítvány tooyour JDK cacert tároló hozzáadásával kapcsolatos további információkért lásd: [hozzáadása a Java-hitelesítésszolgáltató tanúsítványtároló tanúsítvány toohello][add_ca_cert].

Emellett a tesztkörnyezet hello információkat, [Hello World használó hello Azure eszköztára Eclipse létrehozása][azure_java_eclipse_hello_world], vagy egyéb technikák, ha az Azure Java-alkalmazások, nem használja az eclipse-ben, erősen ajánlott.

## <a name="create-a-web-form-for-making-a-call"></a>A következő hívással webes űrlap létrehozása
hello a következő kód bemutatja, hogyan toocreate egy webes űrlap tooretrieve felhasználó adatai a következő hívással. Ebben a példában egy új dinamikus webes projekt alkalmazásában nevű **TwilioCloud**, hozták létre, és **callform.jsp** JSP-fájlként lett hozzáadva.

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

## <a name="create-hello-code-toomake-hello-call"></a>Hello kód toomake hello hívás létrehozása
hello alábbi kód, amely hello felhasználói befejezéséről hello űrlap callform.jsp által megjelenített neve, hívás üdvözlőüzenetére hoz létre és hello hívás állít elő. Ebben a példában a hello JSP-fájl neve **makecall.jsp** és hozzá lett adva toohello **TwilioCloud** projekt. (Twilio-fiókja és -hitelesítési token helyett túl hozzárendelt hello helyőrző értékeket használja**accountSID** és **authToken** a hello kódot.)

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

Ezenkívül toomaking hello hívás, makecall.jsp hello Twilio-végpont, API-verziót és hello hívás állapotát jeleníti meg. Példa: a következő képernyőfelvétel hello:

![Azure hívási válasz Twilio és Java használatával][twilio_java_response]

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
Következő vannak hello a magas szintű lépései toorun az alkalmazás; a következő lépéseket találhatók információk [létrehozása egy Hello World használó hello Azure eszköztára Eclipse][azure_java_eclipse_hello_world].

1. Exportálja a TwilioCloud WAR toohello Azure **approot** mappát. 
2. Módosítsa **startup.cmd** toounzip a TwilioCloud WAR.
3. Az alkalmazás hello compute Emulator össze.
4. Indítsa el a központi telepítés hello compute emulator.
5. Nyisson meg egy böngészőt, és futtassa **http://localhost:8080/TwilioCloud/callform.jsp**.
6. A hello formában adja meg az értékeket, kattintson a **hívás kezdeményezéséhez**, és olvassa el a makecall.jsp hello eredményez.

Amikor készen áll a toodeploy tooAzure, telepítési toohello felhő újrafordítása, tooAzure telepítéséhez és futtatásához http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp hello böngészőben (helyettesítse be a következő *your_hosted_name*).

## <a name="next-steps"></a>Következő lépések
Ez a kód lett megadva tooshow Ön alapvető funkciókat Azure Java nyelven Twilio használatával. Mielőtt éles környezetben tooAzure, érdemes lehet tooadd további hibakezelés és egyéb szolgáltatások esetében. Példa:

* Egy webes űrlap használata helyett sikerült Azure storage blobsba vagy SQL adatbázis toostore telefonszámokat, és hívja a szöveg. Az Azure storage-blobot, amely Java használatával kapcsolatos információkért lásd: [hogyan tooUse hello Blob Storage szolgáltatással való Java][howto_blob_storage_java]. SQL-adatbázis a Java használatával kapcsolatos információkért lásd: [SQL-adatbázis használata a Java][howto_sql_azure_java].
* Használhat **RoleEnvironment.getConfigurationSettings** tooretrieve hello Twilio Fiókazonosítót és a hitelesítési token a központi telepítés konfigurációs beállításai, makecall.jsp hello értékek rögzített megadás helyett. Hello információt **roleenvironment-et** osztály című [Using hello Azure szolgáltatás futásidejű kódtár a JSP] [ azure_runtime_jsp] és hello Azure szolgáltatás futásidejű csomag dokumentációban, [http://dl.windowsazure.com/javadoc][azure_javadoc].
* hello makecall.jsp kód hozzárendel egy Twilio-megadott URL-címet, [http://twimlets.com/message][twimlet_message_url], toohello **URL-cím** változó. Az URL-cím, amely tájékoztatja arról, hogyan hívható meg a hello tooproceed Twilio Twilio Markup Language (TwiML) választ biztosít. Hello visszaadott TwiML tartalmazhat például egy  **&lt;szóbeli&gt;**  művelet, amely éppen szóbeli toohello hívás címzettje szöveg eredményez. Hello Twilio által megadott URL-cím helyett a saját szolgáltatás toorespond tooTwilio kérelem; létrehozása sikerült További információkért lásd: [hogyan tooUse hang-és SMS képességei Java Twilio][howto_twilio_voice_sms_java]. További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml], és további információ a  **&lt;szóbeli&gt;**  és egyéb Twilio művelet található a [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Olvassa el a hello Twilio biztonsági előírásait a [https://www.twilio.com/docs/security][twilio_docs_security].

További információ a Twilio: [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Lásd még:
* [Hogyan tooUse Twilio hang-és Java SMS képességei][howto_twilio_voice_sms_java]
* [A tanúsítvány toohello Java hitelesítésszolgáltató tanúsítványtároló hozzáadása][add_ca_cert]

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
