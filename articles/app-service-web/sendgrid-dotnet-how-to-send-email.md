---
title: "aaaHow toouse hello SendGrid e-mail szolgáltatás (.NET) |} Microsoft Docs"
description: "Megtudhatja, hogyan küldjön e-maileket hello SendGrid e-mail szolgáltatás az Azure-on. Kódminták C# és -felhasználási hello .NET API-t."
services: app-service-web
documentationcenter: .net
author: thinkingserious
manager: erikre
editor: 
ms.assetid: 21bf4028-9046-476b-9799-3d3082a0f84c
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2017
ms.author: dx@sendgrid.com
ms.openlocfilehash: b3d77bb67898b991c7293e6b9086b263f6bcb755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-with-azure"></a><span data-ttu-id="5ad4b-104">Hogyan tooSend E-mail használatával SendGrid az Azure-ral</span><span class="sxs-lookup"><span data-stu-id="5ad4b-104">How tooSend Email Using SendGrid with Azure</span></span>
## <a name="overview"></a><span data-ttu-id="5ad4b-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5ad4b-105">Overview</span></span>
<span data-ttu-id="5ad4b-106">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok a sendgrid e-mail szolgáltatás az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-106">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="5ad4b-107">hello minták C nyelven íródtak\# és támogatja a .NET-szabvány 1.3.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-107">hello samples are written in C\# and supports .NET Standard 1.3.</span></span> <span data-ttu-id="5ad4b-108">hello ismertetett forgatókönyvek hozhat létre, e-mailek, e-mailek küldéséhez, hozzáadása a mellékleteket, és különböző mail engedélyezése és nyomon követési beállítások közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-108">hello scenarios covered include constructing email, sending email, adding attachments, and enabling various mail and tracking settings.</span></span> <span data-ttu-id="5ad4b-109">A SendGrid és e-mailek küldéséhez további információkért lásd: hello [további lépések] [ Next steps] szakasz.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-109">For more information on SendGrid and sending email, see hello [Next steps][Next steps] section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="5ad4b-110">Mi az az üdvözlő E-mail szolgáltatás SendGrid?</span><span class="sxs-lookup"><span data-stu-id="5ad4b-110">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="5ad4b-111">SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-111">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="5ad4b-112">Közös SendGrid használati esetek a következők:</span><span class="sxs-lookup"><span data-stu-id="5ad4b-112">Common SendGrid use cases include:</span></span>

* <span data-ttu-id="5ad4b-113">Visszaigazolások vagy beszerzési visszaigazolások toocustomers automatikus küldése.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-113">Automatically sending receipts or purchase confirmations toocustomers.</span></span>
* <span data-ttu-id="5ad4b-114">Az ügyfelek havi szórólapok és előléptetések terjesztési felügyelete sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-114">Administering distribution lists for sending customers monthly fliers and promotions.</span></span>
* <span data-ttu-id="5ad4b-115">Többek között a letiltott e-mailek és az ügyfél engagement valós idejű metrikáját gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-115">Collecting real-time metrics for things like blocked email and customer engagement.</span></span>
* <span data-ttu-id="5ad4b-116">Továbbító ügyfél lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-116">Forwarding customer inquiries.</span></span>
* <span data-ttu-id="5ad4b-117">A bejövő e-mailek feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-117">Processing incoming emails.</span></span>

<span data-ttu-id="5ad4b-118">További információkért látogasson el a [https://sendgrid.com](https://sendgrid.com) vagy SendGrid tartozó [C# könyvtár] [ sendgrid-csharp] GitHub-tárház.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-118">For more information, visit [https://sendgrid.com](https://sendgrid.com) or SendGrid's [C# library][sendgrid-csharp] GitHub repo.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="5ad4b-119">A SendGrid-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ad4b-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-net-class-library"></a><span data-ttu-id="5ad4b-120">Hivatkozás hello SendGrid .NET Class Library</span><span class="sxs-lookup"><span data-stu-id="5ad4b-120">Reference hello SendGrid .NET Class Library</span></span>
<span data-ttu-id="5ad4b-121">Hello [SendGrid NuGet-csomag](https://www.nuget.org/packages/Sendgrid) hello legegyszerűbb módja tooget hello SendGrid API és tooconfigure van az alkalmazás összes függőségét.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-121">hello [SendGrid NuGet package](https://www.nuget.org/packages/Sendgrid) is hello easiest way tooget hello SendGrid API and tooconfigure your application with all dependencies.</span></span> <span data-ttu-id="5ad4b-122">NuGet egy Visual Studio bővítmény a Microsoft Visual Studio 2015-öt és a fent, amely segítségével könnyen tooinstall és frissítés kódtárak és eszközök.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-122">NuGet is a Visual Studio extension included with Microsoft Visual Studio 2015 and above that makes it easy tooinstall and update libraries and tools.</span></span>

> [!NOTE]
> <span data-ttu-id="5ad4b-123">tooinstall NuGet Ha futtatja a Visual Studio verziója korábbi, mint a Visual Studio 2015-öt, látogasson el [http://www.nuget.org](http://www.nuget.org), és kattintson a hello **NuGet telepítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-123">tooinstall NuGet if you are running a version of Visual Studio earlier than Visual Studio 2015, visit [http://www.nuget.org](http://www.nuget.org), and click hello **Install NuGet** button.</span></span>
>
>

<span data-ttu-id="5ad4b-124">az alkalmazás SendGrid NuGet-csomagot tooinstall hello hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5ad4b-124">tooinstall hello SendGrid NuGet package in your application, do hello following:</span></span>

1. <span data-ttu-id="5ad4b-125">Kattintson a **új projekt** válassza ki a **sablon**.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-125">Click on **New Project** and select a **Template**.</span></span>

   ![Új projekt létrehozása][create-new-project]
2. <span data-ttu-id="5ad4b-127">A **Megoldáskezelőben**, kattintson a jobb gombbal **hivatkozások**, majd kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-127">In **Solution Explorer**, right-click **References**, then click **Manage NuGet Packages**.</span></span>

   ![SendGrid NuGet-csomag][SendGrid-NuGet-package]
3. <span data-ttu-id="5ad4b-129">Keresse meg **SendGrid** és select hello **SendGrid** elem az eredménylistában.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-129">Search for **SendGrid** and select hello **SendGrid** item in the results list.</span></span>
4. <span data-ttu-id="5ad4b-130">Hello verzió legördülő toobe képes toowork hello hálózatiobjektum-modellje és API-k, a cikkben bemutatott hello legújabb stabil verziójának hello Nuget-csomag kiválasztása</span><span class="sxs-lookup"><span data-stu-id="5ad4b-130">Select hello latest stable version of hello Nuget package from hello version dropdown toobe able toowork with hello object model and APIs demonstrated in this article.</span></span>

   ![SendGrid csomag][sendgrid-package]
5. <span data-ttu-id="5ad4b-132">Kattintson a **telepítése** toocomplete hello telepítését, és zárja be ezt a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-132">Click **Install** toocomplete hello installation, and then close this dialog.</span></span>

<span data-ttu-id="5ad4b-133">SendGrid tartozó .NET osztálytár nevezik **SendGrid**.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-133">SendGrid's .NET class library is called **SendGrid**.</span></span> <span data-ttu-id="5ad4b-134">A következő névterek hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5ad4b-134">It contains hello following namespaces:</span></span>

* <span data-ttu-id="5ad4b-135">**SendGrid** SendGrid tartozó API való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-135">**SendGrid** for communicating with SendGrid’s API.</span></span>
* <span data-ttu-id="5ad4b-136">**SendGrid.Helpers.Mail** segítő módszerek tooeasily adja meg, hogyan toosend e-mailt küld SendGridMessage objektumokat hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-136">**SendGrid.Helpers.Mail** for helper methods tooeasily create SendGridMessage objects that specify how toosend emails.</span></span>

<span data-ttu-id="5ad4b-137">Adja hozzá a következő kód névtér nyilatkozatok toohello felső bármely C# fájl tooprogrammatically hello SendGrid e-mail szolgáltatás kívánt hello.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-137">Add hello following code namespace declarations toohello top of any C# file in which you want tooprogrammatically access hello SendGrid email service.</span></span>

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a><span data-ttu-id="5ad4b-138">Hogyan: hozzon létre egy e-mailt</span><span class="sxs-lookup"><span data-stu-id="5ad4b-138">How to: Create an Email</span></span>
<span data-ttu-id="5ad4b-139">Használjon hello **SendGridMessage** objektum toocreate e-mailt.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-139">Use hello **SendGridMessage** object toocreate an email message.</span></span> <span data-ttu-id="5ad4b-140">Hello üzenet objektum létrehozása után beállíthatja a tulajdonságokat és metódusokat, beleértve az üdvözlő e-mailt olyasvalaki, hello e-mail címzettje, és hello tulajdonos üdvözlő e-mail törzsét.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-140">Once hello message object is created, you can set properties and methods, including hello email sender, hello email recipient, and hello subject and body of hello email.</span></span>

<span data-ttu-id="5ad4b-141">hello következő példa bemutatja, hogyan toocreate egy teljes mértékben ki van töltve e-mail objektum:</span><span class="sxs-lookup"><span data-stu-id="5ad4b-141">hello following example demonstrates how toocreate a fully populated email object:</span></span>

    var msg = new SendGridMessage();

    msg.SetFrom(new EmailAddress("dx@example.com", "SendGrid DX Team"));

    var recipients = new List<EmailAddress>
    {
        new EmailAddress("jeff@example.com", "Jeff Smith"),
        new EmailAddress("anna@example.com", "Anna Lidman"),
        new EmailAddress("peter@example.com", "Peter Saddow")
    };
    msg.AddTos(recipients);

    msg.SetSubject("Testing hello SendGrid C# Library");

    msg.AddContent(MimeType.Text, "Hello World plain text!");
    msg.AddContent(MimeType.Html, "<p>Hello World!</p>");

<span data-ttu-id="5ad4b-142">Minden olyan tulajdonságok és módszerek által támogatott további információt a **SendGrid** írja be, lásd: [sendgrid-c Sharp] [ sendgrid-csharp] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-142">For more information on all properties and methods supported by the **SendGrid** type, see [sendgrid-csharp][sendgrid-csharp] on GitHub.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="5ad4b-143">Útmutató: az e-mailek küldése</span><span class="sxs-lookup"><span data-stu-id="5ad4b-143">How to: Send an Email</span></span>
<span data-ttu-id="5ad4b-144">Miután létrehozta az e-mailt, elküldheti azt a SendGrid tartozó API-val.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-144">After creating an email message, you can send it using SendGrid's API.</span></span> <span data-ttu-id="5ad4b-145">Másik megoldásként használhat [. NET meg a szalagtár beépített][NET-library].</span><span class="sxs-lookup"><span data-stu-id="5ad4b-145">Alternatively, you may use [.NET's built in library][NET-library].</span></span>

<span data-ttu-id="5ad4b-146">E-mailek küldéséhez, hogy megadnia a SendGrid API-kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-146">Sending email requires that you supply your SendGrid API Key.</span></span> <span data-ttu-id="5ad4b-147">Ha részletes tájékoztatást kell tooconfigure API-kulcsokat, látogasson el a SendGrid tartozó API-kulcsokat [dokumentáció][documentation].</span><span class="sxs-lookup"><span data-stu-id="5ad4b-147">If you need details about how tooconfigure API Keys, please visit SendGrid's API Keys [documentation][documentation].</span></span>

<span data-ttu-id="5ad4b-148">Ezeket a hitelesítő adatokat az Azure portálon gombra az alkalmazásbeállítások és hozzáadását hello kulcs/érték párok az alkalmazásbeállítások tárolhatjuk.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-148">You may store these credentials via your Azure Portal by clicking Application settings and adding hello key/value pairs under App settings.</span></span>

 ![Az Azure alkalmazás beállításai][azure_app_settings]

 <span data-ttu-id="5ad4b-150">Majd hogy a hozzáférés az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5ad4b-150">Then, you may access them as follows:</span></span>

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

<span data-ttu-id="5ad4b-151">hello a következő példák bemutatják, hogyan egy üzenet használatával toosend hello Web API.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-151">hello following examples show how toosend a message using hello Web API.</span></span>

    using System;
    using System.Threading.Tasks;
    using SendGrid;
    using SendGrid.Helpers.Mail;

    namespace Example
    {
        internal class Example
        {
            private static void Main()
            {
                Execute().Wait();
            }

            static async Task Execute()
            {
                var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
                var client = new SendGridClient(apiKey);
                var msg = new SendGridMessage()
                {
                    From = new EmailAddress("test@example.com", "DX Team"),
                    Subject = "Hello World from hello SendGrid CSharp SDK!",
                    PlainTextContent = "Hello, Email!",
                    HtmlContent = "<strong>Hello, Email!</strong>"
                };
                msg.AddTo(new EmailAddress("test@example.com", "Test User"));
                var response = await client.SendEmailAsync(msg);
            }
        }
    }

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="5ad4b-152">Hogyan: hozzá mellékletet</span><span class="sxs-lookup"><span data-stu-id="5ad4b-152">How to: Add an attachment</span></span>
<span data-ttu-id="5ad4b-153">Mellékletek adható tooa üzenet hívó hello **AddAttachment** metódus és a minimálisan megadó hello fájl nevét és a Base64-kódolású tartalom szeretné tooattach.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-153">Attachments can be added tooa message by calling hello **AddAttachment** method and minimally specifying hello file name and Base64 encoded content you want tooattach.</span></span> <span data-ttu-id="5ad4b-154">A metódus meghívása után a fájl tooattach kívánja vagy hello segítségével megadhat több mellékletek **AddAttachments** metódust.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-154">You can include multiple attachments by calling this method once for each file you wish tooattach or by using hello **AddAttachments** method.</span></span> <span data-ttu-id="5ad4b-155">hello a következő példa bemutatja, hogy egy mellékletet tooa üzenet hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="5ad4b-155">hello following example demonstrates adding an attachment tooa message:</span></span>

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="5ad4b-156">Útmutató: e-mail beállítások tooenable láblécben, nyomon követési és analytics</span><span class="sxs-lookup"><span data-stu-id="5ad4b-156">How to: Use mail settings tooenable footers, tracking, and analytics</span></span>
<span data-ttu-id="5ad4b-157">SendGrid a levelezési beállításokat és a nyomon követési beállítások hello használata további e-mail funkciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-157">SendGrid provides additional email functionality through hello use of mail settings and tracking settings.</span></span> <span data-ttu-id="5ad4b-158">Ezek a beállítások is hozzáadhatók a tooan e-mail üzenet tooenable adott funkciók, például kattintson nyomon követése, Google analytics, előfizetés nyomon követése és stb.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-158">These settings can be added tooan email message tooenable specific functionality such as click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="5ad4b-159">Alkalmazások teljes listáját lásd: hello [beállítások dokumentáció][settings-documentation].</span><span class="sxs-lookup"><span data-stu-id="5ad4b-159">For a full list of apps, see hello [Settings Documentation][settings-documentation].</span></span>

<span data-ttu-id="5ad4b-160">Alkalmazások túl alkalmazható**SendGrid** e-mailek hello részeként megvalósított metódusok használatával **SendGridMessage** osztály.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-160">Apps can be applied too**SendGrid** email messages using methods implemented as part of hello **SendGridMessage** class.</span></span> <span data-ttu-id="5ad4b-161">hello alábbi példák bemutatják, hello élőláb, majd kattintson szűrők nyomon követése:</span><span class="sxs-lookup"><span data-stu-id="5ad4b-161">hello following examples demonstrate hello footer and click tracking filters:</span></span>

<span data-ttu-id="5ad4b-162">hello alábbi példák bemutatják, hello élőláb, majd kattintson szűrők nyomon követése:</span><span class="sxs-lookup"><span data-stu-id="5ad4b-162">hello following examples demonstrate hello footer and click tracking filters:</span></span>

### <a name="footer-settings"></a><span data-ttu-id="5ad4b-163">Élőláb beállításai</span><span class="sxs-lookup"><span data-stu-id="5ad4b-163">Footer settings</span></span>
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a><span data-ttu-id="5ad4b-164">Kattintson a nyomon követése</span><span class="sxs-lookup"><span data-stu-id="5ad4b-164">Click tracking</span></span>
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="5ad4b-165">Útmutató: további SendGrid szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="5ad4b-165">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="5ad4b-166">SendGrid több API-k és webhookokkal, használhatja az Azure alkalmazáson belül tooleverage további funkciókat kínál.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-166">SendGrid offers several APIs and webhooks that you can use tooleverage additional functionality within your Azure application.</span></span> <span data-ttu-id="5ad4b-167">További részletekért lásd: hello [SendGrid API-referencia][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="5ad4b-167">For more details, see hello [SendGrid API Reference][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ad4b-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ad4b-168">Next steps</span></span>
<span data-ttu-id="5ad4b-169">Most, hogy megismerte a SendGrid E-mail szolgáltatás hello hello alapjait, kövesse a további hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="5ad4b-169">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="5ad4b-170">SendGrid C\# könyvtár tárház: [sendgrid-c Sharp][sendgrid-csharp]</span><span class="sxs-lookup"><span data-stu-id="5ad4b-170">SendGrid C\# library repo: [sendgrid-csharp][sendgrid-csharp]</span></span>
* <span data-ttu-id="5ad4b-171">SendGrid API dokumentációjának: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="5ad4b-171">SendGrid API documentation: <https://sendgrid.com/docs></span></span>

[Next steps]: #next-steps
[What is hello SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference hello SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters tooEnable Footers, Tracking, and Analytics]: #usefilters
[How to: Use Additional SendGrid Services]: #useservices

[create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/new-project.png
[SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/reference.png
[sendgrid-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid-package.png
[azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/azure-app-settings.png
[sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
[SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
[App Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/api_v3.html
[NET-library]: https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html#-Using-NETs-Builtin-SMTP-Library
[documentation]: https://sendgrid.com/docs/Classroom/Send/api_keys.html
[settings-documentation]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html

[felhőalapú szolgáltatás]: https://sendgrid.com/solutions
[tranzakciós e-mailben kézbesítésre]: https://sendgrid.com/use-cases/transactional-email

