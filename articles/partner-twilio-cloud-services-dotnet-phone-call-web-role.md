---
title: "Hogyan telefonhívás a Twilio (.NET) |} Microsoft Docs"
description: "Útmutató a telefonhívás, és a Twilio API szolgáltatás SMS üzenet küldése az Azure-on. Kódminták .NET."
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
ms.openlocfilehash: 0899a49cbfda775017dab7fc6d8963bbeb86d74c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a><span data-ttu-id="d6f17-104">Hogyan telefonhívás Twilio webes szerepkörrel rendelkező Azure használatával</span><span class="sxs-lookup"><span data-stu-id="d6f17-104">How to make a phone call using Twilio in a web role on Azure</span></span>
<span data-ttu-id="d6f17-105">Ez az útmutató bemutatja, hogyan Twilio használata Azure-ban üzemeltetett weblapok hívásakor.</span><span class="sxs-lookup"><span data-stu-id="d6f17-105">This guide demonstrates how to use Twilio to make a call from a web page hosted in Azure.</span></span> <span data-ttu-id="d6f17-106">Az eredményül kapott alkalmazás felszólítja a felhasználót egy hívás a megadott és az üzenet, az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="d6f17-106">The resulting application prompts the user to make a call with the given number and message, as shown in the following screenshot.</span></span>

![Azure hívás űrlap Twilio- és ASP.NET használatával][twilio_dotnet_basic_form]

## <span data-ttu-id="d6f17-108"><a name="twilio-prereqs"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6f17-108"><a name="twilio-prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="d6f17-109">Szüksége lesz a kódot használja ebben a témakörben érdekében tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d6f17-109">You will need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="d6f17-110">Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat a a [Twilio-konzol][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="d6f17-110">Acquire a Twilio account and authentication token from the [Twilio Console][twilio_console].</span></span> <span data-ttu-id="d6f17-111">A regisztráció megkezdéséhez Twilio, [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="d6f17-111">To get started with Twilio, sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="d6f17-112">Kiértékelheti a díjszabás [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="d6f17-112">You can evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="d6f17-113">Az API-t Twilio által biztosított kapcsolatos információkért lásd: [http://www.twilio.com/voice/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="d6f17-113">For information about the API provided by Twilio, see [http://www.twilio.com/voice/api][twilio_api].</span></span>
2. <span data-ttu-id="d6f17-114">Adja hozzá a *Twilio .NET kódtár* a webes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="d6f17-114">Add the *Twilio .NET libary* to your web role.</span></span> <span data-ttu-id="d6f17-115">Lásd: **a Twilio-kódtárak hozzáadása a webes szerepkör projekt**, ez a témakör későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="d6f17-115">See **To add the Twilio libraries to your web role project**, later in this topic.</span></span>

<span data-ttu-id="d6f17-116">Meg kell ismernie az alapvető létrehozása [webes szerepkör Azure][azure_webroles_get_started].</span><span class="sxs-lookup"><span data-stu-id="d6f17-116">You should be familiar with creating a basic [Web Role on Azure][azure_webroles_get_started].</span></span>

## <span data-ttu-id="d6f17-117"><a name="howtocreateform"></a>Útmutató: a következő hívással webes űrlap létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6f17-117"><a name="howtocreateform"></a>How to: Create a web form for making a call</span></span>
<span data-ttu-id="d6f17-118"><a id="use_nuget"></a>A Twilio-kódtárak hozzáadása a webes szerepkör projekt:</span><span class="sxs-lookup"><span data-stu-id="d6f17-118"><a id="use_nuget"></a>To add the Twilio libraries to your web role project:</span></span>

1. <span data-ttu-id="d6f17-119">Nyissa meg a megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="d6f17-119">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="d6f17-120">Kattintson a jobb gombbal **hivatkozások**.</span><span class="sxs-lookup"><span data-stu-id="d6f17-120">Right-click **References**.</span></span>
3. <span data-ttu-id="d6f17-121">Kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="d6f17-121">Click **Manage NuGet Packages**.</span></span>
4. <span data-ttu-id="d6f17-122">Kattintson a **Online**.</span><span class="sxs-lookup"><span data-stu-id="d6f17-122">Click **Online**.</span></span>
5. <span data-ttu-id="d6f17-123">Online keresőmezőbe, írja be a *twilio*.</span><span class="sxs-lookup"><span data-stu-id="d6f17-123">In the search online box, type *twilio*.</span></span>
6. <span data-ttu-id="d6f17-124">Kattintson a **telepítése** a Twilio-csomagra.</span><span class="sxs-lookup"><span data-stu-id="d6f17-124">Click **Install** on the Twilio package.</span></span>

<span data-ttu-id="d6f17-125">A következő kód bemutatja, hogyan hozzon létre egy webes űrlap egy hívás a felhasználói adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d6f17-125">The following code shows how to create a web form to retrieve user data for making a call.</span></span> <span data-ttu-id="d6f17-126">Ebben a példában egy ASP.NET webes szerepkör nevű **TwilioCloud** jön létre.</span><span class="sxs-lookup"><span data-stu-id="d6f17-126">In this example, an ASP.NET Web Role named **TwilioCloud** is created.</span></span>

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

## <span data-ttu-id="d6f17-127"><a id="howtocreatecode"></a>Útmutató: a kód a híváshoz létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6f17-127"><a id="howtocreatecode"></a>How to: Create the code to make the call</span></span>
<span data-ttu-id="d6f17-128">Az alábbi kód, amely nevezzük, amikor a felhasználó befejezi az űrlapot, a hívás-üzenetet hoz létre, és állít elő, a hívást.</span><span class="sxs-lookup"><span data-stu-id="d6f17-128">The following code, which is called when the user completes the form, creates the call message and generates the call.</span></span> <span data-ttu-id="d6f17-129">Ebben a példában a kódot az űrlap gomb onclick eseménykezelőjét futtatásához.</span><span class="sxs-lookup"><span data-stu-id="d6f17-129">In this example, the code is run in the onclick event handler of the button on the form.</span></span> <span data-ttu-id="d6f17-130">(Twilio-fiókja és -hitelesítési token helyett a rendelt helyőrző értékeket használja `accountSID` és `authToken` az alábbi kódban.)</span><span class="sxs-lookup"><span data-stu-id="d6f17-130">(Use your Twilio account and authentication token instead of the placeholder values assigned to `accountSID` and `authToken` in the code below.)</span></span>

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
            // the placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of the Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve the account, used later to retrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve the values entered by the user.
                var to = PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using the Twilio message and the user-entered
                // text. You must replace spaces in the user's text with '%20'
                // to make the text suitable for a URL.
                var url = $"http://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display the endpoint, API version, and the URL for the message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"The URL is {url}");

                // Place the call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

<span data-ttu-id="d6f17-131">A rendszer telefonhívást indít, és a Twilio-végpont, API-verzió és az állapot jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d6f17-131">The call is made, and the Twilio endpoint, API version, and the call status are displayed.</span></span> <span data-ttu-id="d6f17-132">Az alábbi képernyőfelvételen látható minta futtató kimenetét.</span><span class="sxs-lookup"><span data-stu-id="d6f17-132">The following screenshot shows output from a sample run.</span></span>

![Azure hívási válasz Twilio-és ASP.NET][twilio_dotnet_basic_form_output]

<span data-ttu-id="d6f17-134">További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml].</span><span class="sxs-lookup"><span data-stu-id="d6f17-134">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml].</span></span> <span data-ttu-id="d6f17-135">További információ a &lt;szóbeli&gt; és egyéb Twilio művelet található a [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="d6f17-135">More information about &lt;Say&gt; and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>

## <span data-ttu-id="d6f17-136"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6f17-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="d6f17-137">Ez a kód Twilio használatát az ASP.NET webes szerepkör Azure alapvető funkciókat mutatjuk be lett megadva.</span><span class="sxs-lookup"><span data-stu-id="d6f17-137">This code was provided to show you basic functionality using Twilio in an ASP.NET web role on Azure.</span></span> <span data-ttu-id="d6f17-138">Mielőtt telepítené az Azure éles környezetben, érdemes lehet további hibakezelés vagy egyéb szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="d6f17-138">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="d6f17-139">Példa:</span><span class="sxs-lookup"><span data-stu-id="d6f17-139">For example:</span></span>

* <span data-ttu-id="d6f17-140">Egy webes űrlap használata helyett használhat Azure Blob-tároló vagy egy Azure SQL Database-példányt a telefonszámok tárolja, és hívja meg a szöveg.</span><span class="sxs-lookup"><span data-stu-id="d6f17-140">Instead of using a web form, you could use Azure Blob storage or an Azure SQL Database instance to store phone numbers and call text.</span></span> <span data-ttu-id="d6f17-141">További információ a Blobs használata az Azure-ban: [az Azure Blob storage szolgáltatás használata a .NET][howto_blob_storage_dotnet].</span><span class="sxs-lookup"><span data-stu-id="d6f17-141">For information about using Blobs in Azure, see [How to use the Azure Blob storage service in .NET][howto_blob_storage_dotnet].</span></span> <span data-ttu-id="d6f17-142">SQL-adatbázis használatával kapcsolatos információkért lásd: [hogyan használható az Azure SQL Database .NET-alkalmazások][howto_sql_azure_dotnet].</span><span class="sxs-lookup"><span data-stu-id="d6f17-142">For information about using SQL Database, see [How to use Azure SQL Database in .NET applications][howto_sql_azure_dotnet].</span></span>
* <span data-ttu-id="d6f17-143">Használhat `RoleEnvironment.getConfigurationSettings` beolvasni a Twilio Fiókazonosítót és a hitelesítési token a központi telepítés konfigurációs beállításai, bekódolásának az űrlapon szereplő értékek helyett.</span><span class="sxs-lookup"><span data-stu-id="d6f17-143">You could use `RoleEnvironment.getConfigurationSettings` to retrieve the Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding the values in your form.</span></span> <span data-ttu-id="d6f17-144">További információ a `RoleEnvironment` osztály című [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span><span class="sxs-lookup"><span data-stu-id="d6f17-144">For information about the `RoleEnvironment` class, see [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].</span></span>
* <span data-ttu-id="d6f17-145">Olvassa el a Twilio biztonsági irányelvek a [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="d6f17-145">Read the Twilio Security Guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>
* <span data-ttu-id="d6f17-146">További információ a Twilio [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="d6f17-146">Learn more about Twilio at [https://www.twilio.com/docs][twilio_docs].</span></span>

## <span data-ttu-id="d6f17-147"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d6f17-147"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="d6f17-148">Az Azure-ból a hang- és SMS képességeinek Twilio használata</span><span class="sxs-lookup"><span data-stu-id="d6f17-148">How to use Twilio for Voice and SMS capabilities from Azure</span></span>](twilio-dotnet-how-to-use-for-voice-sms.md)

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
