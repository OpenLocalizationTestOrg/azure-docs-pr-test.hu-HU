---
title: "aaaHow toomake telefonhívást a Twilio (.NET) |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. Kódminták .NET."
services: 
documentationcenter: .net
author: devinrader
manager: timlt
editor: 
ms.assetid: 789185ad-69dc-4e9e-a936-42e0a25315c8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/04/2016
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 857d89961c563a51fef944f4a72828036af79b43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Hogyan toomake egy telefonhívási Twilio webes szerepkörrel rendelkező Azure használatával
Ez az útmutató ismerteti, hogyan toouse Twilio toomake weblapról hívás Azure-ban üzemeltetett. hello eredményül kapott alkalmazás kérdéseket hello felhasználói toomake hívás hello adott számát és az üzenet, ahogy az alábbi képernyőfelvétel a hello.

![Azure hívás űrlap Twilio- és ASP.NET használatával][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Előfeltételek
Szüksége lesz a toodo hello következő toouse hello kód ebben a témakörben:

1. Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat a hello [Twilio-konzol][twilio_console]. tooget használatába Twilio, jelentkezzen a [https://www.twilio.com/try-twilio][try_twilio]. Kiértékelheti a díjszabás [http://www.twilio.com/pricing][twilio_pricing]. Hello Twilio által nyújtott API kapcsolatos információkért lásd: [http://www.twilio.com/voice/api][twilio_api].
2. Adja hozzá a hello *Twilio .NET kódtár* tooyour webes szerepkör. Lásd: **tooadd hello Twilio szalagtárak tooyour webes szerepkör projekt**, ez a témakör későbbi részében.

Meg kell ismernie az alapvető létrehozása [webes szerepkör Azure][azure_webroles_get_started].

## <a name="howtocreateform"></a>Útmutató: a következő hívással webes űrlap létrehozása
<a id="use_nuget"></a>tooadd hello Twilio szalagtárak tooyour webes szerepkör projekt:

1. Nyissa meg a megoldást a Visual Studióban.
2. Kattintson a jobb gombbal **hivatkozások**.
3. Kattintson a **NuGet-csomagok kezelése**.
4. Kattintson a **Online**.
5. Hello online a keresőmezőbe írja be a *twilio*.
6. Kattintson a **telepítése** hello Twilio-csomag.

hello a következő kód bemutatja, hogyan toocreate egy webes űrlap tooretrieve felhasználó adatai a következő hívással. Ebben a példában egy ASP.NET webes szerepkör nevű **TwilioCloud** jön létre.

```aspx
<%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
    AutoEventWireup="true" CodeBehind="Default.aspx.cs"
    Inherits="WebRole1._Default" %>

<asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
</asp:Content>
<asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
    <div>
        <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
        </asp:BulletedList>
    </div>
    <div>
        <p>Fill in all fields and click <b>Make this call</b>.</p>
        <div>
            To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
            Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
            <asp:Button ID="callpage" runat="server" Text="Make this call"
                onclick="callpage_Click" />
        </div>
    </div>
</asp:Content>
```

## <a id="howtocreatecode"></a>Hogyan: hello kód toomake hello hívás létrehozása
hello alábbi kód, amely hello felhasználói befejezéséről hello űrlap nevezik, hívás üdvözlőüzenetére hoz létre és hello hívás állít elő. Ebben a példában a hello kód hello űrlap hello onclick eseménykezelő hello gomb futtatásához. (Twilio-fiókja és -hitelesítési token helyett túl hozzárendelt hello helyőrző értékeket használja`accountSID` és `authToken` a hello kódot.)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using Twilio;
using Twilio.Http;
using Twilio.Types;
using Twilio.Rest.Api.V2010;

namespace WebRole1
{
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void callpage_Click(object sender, EventArgs e)
        {
            // Call porcessing happens here.

            // Use your account SID and authentication token instead of
            // hello placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of hello Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve hello account, used later tooretrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve hello values entered by hello user.
                var too= PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using hello Twilio message and hello user-entered
                // text. You must replace spaces in hello user's text with '%20'
                // toomake hello text suitable for a URL.
                var url = $"http://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display hello endpoint, API version, and hello URL for hello message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"hello URL is {url}");

                // Place hello call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

hello kezdeményezték, és hello Twilio-végpont, API-verziót és hello hívás állapotát megjeleníti. hello képernyőképe látható kimeneti következő egy minta futtatásához.

![Azure hívási válasz Twilio-és ASP.NET][twilio_dotnet_basic_form_output]

További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml]. További információ a &lt;szóbeli&gt; és egyéb Twilio művelet található a [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Következő lépések
Ez a kód lett megadva tooshow Ön alapvető funkciókat Twilio ASP.NET webes szerepkört az Azure használatával. Mielőtt éles környezetben tooAzure, érdemes lehet tooadd további hibakezelés és egyéb szolgáltatások esetében. Példa:

* Egy webes űrlap használata helyett sikerült Azure Blob-tároló vagy egy Azure SQL Database példány toostore telefonszámokat, és hívja szöveg. További információ a Blobs használata az Azure-ban: [hogyan toouse hello Azure Blob storage szolgáltatást a .NET][howto_blob_storage_dotnet]. SQL-adatbázis használatával kapcsolatos információkért lásd: [hogyan toouse Azure SQL-adatbázis a .NET-alkalmazások][howto_sql_azure_dotnet].
* Használhat `RoleEnvironment.getConfigurationSettings` tooretrieve hello Twilio Fiókazonosítót és a hitelesítési token a központi telepítés konfigurációs beállítások, az űrlap hello értékei rögzített megadás helyett. További információ a hello `RoleEnvironment` osztály című [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Olvassa el a hello Twilio biztonsági előírásait a [https://www.twilio.com/docs/security][twilio_docs_security].
* További információ a Twilio [https://www.twilio.com/docs][twilio_docs].

## <a name="seealso"></a>Lásd még:
* [Hogyan toouse Twilio hang- és SMS képességeinek az Azure-ból](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
[azure_webroles_get_started]: https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-dotnet-get-started
