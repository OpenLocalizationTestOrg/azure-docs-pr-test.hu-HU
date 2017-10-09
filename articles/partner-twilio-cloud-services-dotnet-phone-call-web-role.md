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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-web-role-on-azure"></a><span data-ttu-id="c00bd-104">Hogyan toomake egy telefonhívási Twilio webes szerepkörrel rendelkező Azure használatával</span><span class="sxs-lookup"><span data-stu-id="c00bd-104">How toomake a phone call using Twilio in a web role on Azure</span></span>
<span data-ttu-id="c00bd-105">Ez az útmutató ismerteti, hogyan toouse Twilio toomake weblapról hívás Azure-ban üzemeltetett.</span><span class="sxs-lookup"><span data-stu-id="c00bd-105">This guide demonstrates how toouse Twilio toomake a call from a web page hosted in Azure.</span></span> <span data-ttu-id="c00bd-106">hello eredményül kapott alkalmazás kérdéseket hello felhasználói toomake hívás hello adott számát és az üzenet, ahogy az alábbi képernyőfelvétel a hello.</span><span class="sxs-lookup"><span data-stu-id="c00bd-106">hello resulting application prompts hello user toomake a call with hello given number and message, as shown in hello following screenshot.</span></span>

![Azure hívás űrlap Twilio- és ASP.NET használatával][twilio_dotnet_basic_form]

## <span data-ttu-id="c00bd-108"><a name="twilio-prereqs"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c00bd-108"><a name="twilio-prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="c00bd-109">Szüksége lesz a toodo hello következő toouse hello kód ebben a témakörben:</span><span class="sxs-lookup"><span data-stu-id="c00bd-109">You will need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="c00bd-110">Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat a hello [Twilio-konzol][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="c00bd-110">Acquire a Twilio account and authentication token from hello [Twilio Console][twilio_console].</span></span> <span data-ttu-id="c00bd-111">tooget használatába Twilio, jelentkezzen a [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="c00bd-111">tooget started with Twilio, sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="c00bd-112">Kiértékelheti a díjszabás [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="c00bd-112">You can evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="c00bd-113">Hello Twilio által nyújtott API kapcsolatos információkért lásd: [http://www.twilio.com/voice/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="c00bd-113">For information about hello API provided by Twilio, see [http://www.twilio.com/voice/api][twilio_api].</span></span>
2. <span data-ttu-id="c00bd-114">Adja hozzá a hello *Twilio .NET kódtár* tooyour webes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="c00bd-114">Add hello *Twilio .NET libary* tooyour web role.</span></span> <span data-ttu-id="c00bd-115">Lásd: **tooadd hello Twilio szalagtárak tooyour webes szerepkör projekt**, ez a témakör későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="c00bd-115">See **tooadd hello Twilio libraries tooyour web role project**, later in this topic.</span></span>

<span data-ttu-id="c00bd-116">Meg kell ismernie az alapvető létrehozása [webes szerepkör Azure][azure_webroles_get_started].</span><span class="sxs-lookup"><span data-stu-id="c00bd-116">You should be familiar with creating a basic [Web Role on Azure][azure_webroles_get_started].</span></span>

## <span data-ttu-id="c00bd-117"><a name="howtocreateform"></a>Útmutató: a következő hívással webes űrlap létrehozása</span><span class="sxs-lookup"><span data-stu-id="c00bd-117"><a name="howtocreateform"></a>How to: Create a web form for making a call</span></span>
<span data-ttu-id="c00bd-118"><a id="use_nuget"></a>tooadd hello Twilio szalagtárak tooyour webes szerepkör projekt:</span><span class="sxs-lookup"><span data-stu-id="c00bd-118"><a id="use_nuget"></a>tooadd hello Twilio libraries tooyour web role project:</span></span>

1. <span data-ttu-id="c00bd-119">Nyissa meg a megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="c00bd-119">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="c00bd-120">Kattintson a jobb gombbal **hivatkozások**.</span><span class="sxs-lookup"><span data-stu-id="c00bd-120">Right-click **References**.</span></span>
3. <span data-ttu-id="c00bd-121">Kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="c00bd-121">Click **Manage NuGet Packages**.</span></span>
4. <span data-ttu-id="c00bd-122">Kattintson a **Online**.</span><span class="sxs-lookup"><span data-stu-id="c00bd-122">Click **Online**.</span></span>
5. <span data-ttu-id="c00bd-123">Hello online a keresőmezőbe írja be a *twilio*.</span><span class="sxs-lookup"><span data-stu-id="c00bd-123">In hello search online box, type *twilio*.</span></span>
6. <span data-ttu-id="c00bd-124">Kattintson a **telepítése** hello Twilio-csomag.</span><span class="sxs-lookup"><span data-stu-id="c00bd-124">Click **Install** on hello Twilio package.</span></span>

<span data-ttu-id="c00bd-125">hello a következő kód bemutatja, hogyan toocreate egy webes űrlap tooretrieve felhasználó adatai a következő hívással.</span><span class="sxs-lookup"><span data-stu-id="c00bd-125">hello following code shows how toocreate a web form tooretrieve user data for making a call.</span></span> <span data-ttu-id="c00bd-126">Ebben a példában egy ASP.NET webes szerepkör nevű **TwilioCloud** jön létre.</span><span class="sxs-lookup"><span data-stu-id="c00bd-126">In this example, an ASP.NET Web Role named **TwilioCloud** is created.</span></span>

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

## <span data-ttu-id="c00bd-127"><a id="howtocreatecode"></a>Hogyan: hello kód toomake hello hívás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c00bd-127"><a id="howtocreatecode"></a>How to: Create hello code toomake hello call</span></span>
<span data-ttu-id="c00bd-128">hello alábbi kód, amely hello felhasználói befejezéséről hello űrlap nevezik, hívás üdvözlőüzenetére hoz létre és hello hívás állít elő.</span><span class="sxs-lookup"><span data-stu-id="c00bd-128">hello following code, which is called when hello user completes hello form, creates hello call message and generates hello call.</span></span> <span data-ttu-id="c00bd-129">Ebben a példában a hello kód hello űrlap hello onclick eseménykezelő hello gomb futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c00bd-129">In this example, hello code is run in hello onclick event handler of hello button on hello form.</span></span> <span data-ttu-id="c00bd-130">(Twilio-fiókja és -hitelesítési token helyett túl hozzárendelt hello helyőrző értékeket használja`accountSID` és `authToken` a hello kódot.)</span><span class="sxs-lookup"><span data-stu-id="c00bd-130">(Use your Twilio account and authentication token instead of hello placeholder values assigned too`accountSID` and `authToken` in hello code below.)</span></span>

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

<span data-ttu-id="c00bd-131">hello kezdeményezték, és hello Twilio-végpont, API-verziót és hello hívás állapotát megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="c00bd-131">hello call is made, and hello Twilio endpoint, API version, and hello call status are displayed.</span></span> <span data-ttu-id="c00bd-132">hello képernyőképe látható kimeneti következő egy minta futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c00bd-132">hello following screenshot shows output from a sample run.</span></span>

![Azure hívási válasz Twilio-és ASP.NET][twilio_dotnet_basic_form_output]

<span data-ttu-id="c00bd-134">További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml].</span><span class="sxs-lookup"><span data-stu-id="c00bd-134">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml].</span></span> <span data-ttu-id="c00bd-135">További információ a &lt;szóbeli&gt; és egyéb Twilio művelet található a [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="c00bd-135">More information about &lt;Say&gt; and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>

## <span data-ttu-id="c00bd-136"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c00bd-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="c00bd-137">Ez a kód lett megadva tooshow Ön alapvető funkciókat Twilio ASP.NET webes szerepkört az Azure használatával.</span><span class="sxs-lookup"><span data-stu-id="c00bd-137">This code was provided tooshow you basic functionality using Twilio in an ASP.NET web role on Azure.</span></span> <span data-ttu-id="c00bd-138">Mielőtt éles környezetben tooAzure, érdemes lehet tooadd további hibakezelés és egyéb szolgáltatások esetében.</span><span class="sxs-lookup"><span data-stu-id="c00bd-138">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="c00bd-139">Példa:</span><span class="sxs-lookup"><span data-stu-id="c00bd-139">For example:</span></span>

* <span data-ttu-id="c00bd-140">Egy webes űrlap használata helyett sikerült Azure Blob-tároló vagy egy Azure SQL Database példány toostore telefonszámokat, és hívja szöveg.</span><span class="sxs-lookup"><span data-stu-id="c00bd-140">Instead of using a web form, you could use Azure Blob storage or an Azure SQL Database instance toostore phone numbers and call text.</span></span> <span data-ttu-id="c00bd-141">További információ a Blobs használata az Azure-ban: [hogyan toouse hello Azure Blob storage szolgáltatást a .NET][howto_blob_storage_dotnet].</span><span class="sxs-lookup"><span data-stu-id="c00bd-141">For information about using Blobs in Azure, see [How toouse hello Azure Blob storage service in .NET][howto_blob_storage_dotnet].</span></span> <span data-ttu-id="c00bd-142">SQL-adatbázis használatával kapcsolatos információkért lásd: [hogyan toouse Azure SQL-adatbázis a .NET-alkalmazások][howto_sql_azure_dotnet].</span><span class="sxs-lookup"><span data-stu-id="c00bd-142">For information about using SQL Database, see [How toouse Azure SQL Database in .NET applications][howto_sql_azure_dotnet].</span></span>
* <span data-ttu-id="c00bd-143">Használhat `RoleEnvironment.getConfigurationSettings` tooretrieve hello Twilio Fiókazonosítót és a hitelesítési token a központi telepítés konfigurációs beállítások, az űrlap hello értékei rögzített megadás helyett.</span><span class="sxs-lookup"><span data-stu-id="c00bd-143">You could use `RoleEnvironment.getConfigurationSettings` tooretrieve hello Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding hello values in your form.</span></span> <span data-ttu-id="c00bd-144">További információ a hello `RoleEnvironment` osztály című [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span><span class="sxs-lookup"><span data-stu-id="c00bd-144">For information about hello `RoleEnvironment` class, see [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span></span>
* <span data-ttu-id="c00bd-145">Olvassa el a hello Twilio biztonsági előírásait a [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="c00bd-145">Read hello Twilio Security Guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>
* <span data-ttu-id="c00bd-146">További információ a Twilio [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="c00bd-146">Learn more about Twilio at [https://www.twilio.com/docs][twilio_docs].</span></span>

## <span data-ttu-id="c00bd-147"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c00bd-147"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="c00bd-148">Hogyan toouse Twilio hang- és SMS képességeinek az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="c00bd-148">How toouse Twilio for Voice and SMS capabilities from Azure</span></span>](twilio-dotnet-how-to-use-for-voice-sms.md)

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
