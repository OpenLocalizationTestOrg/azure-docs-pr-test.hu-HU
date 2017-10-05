---
title: "A SendGrid e-mail szolgáltatás (.NET) használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan küldjön e-maileket a SendGrid e-mail szolgáltatás az Azure-on. A Kódminták C# nyelven íródtak, és a .NET API-t használja."
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
ms.openlocfilehash: b3a48b3c838763b022a18e55817ec7455fe94c85
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-send-email-using-sendgrid-with-azure"></a><span data-ttu-id="1694b-104">Hogyan e-mailek küldése SendGrid az Azure-ral</span><span class="sxs-lookup"><span data-stu-id="1694b-104">How to Send Email Using SendGrid with Azure</span></span>
## <a name="overview"></a><span data-ttu-id="1694b-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1694b-105">Overview</span></span>
<span data-ttu-id="1694b-106">Ez az útmutató bemutatja, hogyan Azure a SendGrid e-mail szolgáltatás közös programozási feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1694b-106">This guide demonstrates how to perform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="1694b-107">A Kódminták C nyelven íródtak\# és támogatja a .NET-szabvány 1.3.</span><span class="sxs-lookup"><span data-stu-id="1694b-107">The samples are written in C\# and supports .NET Standard 1.3.</span></span> <span data-ttu-id="1694b-108">Az ismertetett forgatókönyvek hozhat létre, e-mailek, e-mailek küldéséhez, hozzáadása a mellékleteket, és különböző mail engedélyezése és nyomon követési beállítások közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="1694b-108">The scenarios covered include constructing email, sending email, adding attachments, and enabling various mail and tracking settings.</span></span> <span data-ttu-id="1694b-109">A SendGrid és e-mailek küldéséhez további információkért lásd: a [további lépések] [ Next steps] szakasz.</span><span class="sxs-lookup"><span data-stu-id="1694b-109">For more information on SendGrid and sending email, see the [Next steps][Next steps] section.</span></span>

## <a name="what-is-the-sendgrid-email-service"></a><span data-ttu-id="1694b-110">Mi az a SendGrid E-mail szolgáltatás?</span><span class="sxs-lookup"><span data-stu-id="1694b-110">What is the SendGrid Email Service?</span></span>
<span data-ttu-id="1694b-111">SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt.</span><span class="sxs-lookup"><span data-stu-id="1694b-111">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="1694b-112">Közös SendGrid használati esetek a következők:</span><span class="sxs-lookup"><span data-stu-id="1694b-112">Common SendGrid use cases include:</span></span>

* <span data-ttu-id="1694b-113">Az ügyfelek automatikusan küldése visszaigazolások vagy beszerzési visszaigazolások.</span><span class="sxs-lookup"><span data-stu-id="1694b-113">Automatically sending receipts or purchase confirmations to customers.</span></span>
* <span data-ttu-id="1694b-114">Az ügyfelek havi szórólapok és előléptetések terjesztési felügyelete sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="1694b-114">Administering distribution lists for sending customers monthly fliers and promotions.</span></span>
* <span data-ttu-id="1694b-115">Többek között a letiltott e-mailek és az ügyfél engagement valós idejű metrikáját gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="1694b-115">Collecting real-time metrics for things like blocked email and customer engagement.</span></span>
* <span data-ttu-id="1694b-116">Továbbító ügyfél lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="1694b-116">Forwarding customer inquiries.</span></span>
* <span data-ttu-id="1694b-117">A bejövő e-mailek feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="1694b-117">Processing incoming emails.</span></span>

<span data-ttu-id="1694b-118">További információkért látogasson el a [https://sendgrid.com](https://sendgrid.com) vagy SendGrid tartozó [C# könyvtár] [ sendgrid-csharp] GitHub-tárház.</span><span class="sxs-lookup"><span data-stu-id="1694b-118">For more information, visit [https://sendgrid.com](https://sendgrid.com) or SendGrid's [C# library][sendgrid-csharp] GitHub repo.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="1694b-119">A SendGrid-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="1694b-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a><span data-ttu-id="1694b-120">A SendGrid .NET osztálytár referencia</span><span class="sxs-lookup"><span data-stu-id="1694b-120">Reference the SendGrid .NET Class Library</span></span>
<span data-ttu-id="1694b-121">A [SendGrid NuGet-csomag](https://www.nuget.org/packages/Sendgrid) a legegyszerűbb módja a SendGrid API beszerzésének, és konfigurálja az alkalmazás összes függőségét.</span><span class="sxs-lookup"><span data-stu-id="1694b-121">The [SendGrid NuGet package](https://www.nuget.org/packages/Sendgrid) is the easiest way to get the SendGrid API and to configure your application with all dependencies.</span></span> <span data-ttu-id="1694b-122">NuGet Microsoft Visual Studio 2015-höz mellékelt Visual Studio bővítménye, és fölött, amely megkönnyíti a telepítése és frissítése, kódtárak és eszközök.</span><span class="sxs-lookup"><span data-stu-id="1694b-122">NuGet is a Visual Studio extension included with Microsoft Visual Studio 2015 and above that makes it easy to install and update libraries and tools.</span></span>

> [!NOTE]
> <span data-ttu-id="1694b-123">NuGet Ha futtatja a Visual Studio Visual Studio 2015-nél korábbi verziójának telepítéséhez látogasson el a [http://www.nuget.org](http://www.nuget.org), és kattintson a **NuGet telepítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1694b-123">To install NuGet if you are running a version of Visual Studio earlier than Visual Studio 2015, visit [http://www.nuget.org](http://www.nuget.org), and click the **Install NuGet** button.</span></span>
>
>

<span data-ttu-id="1694b-124">A SendGrid NuGet-csomagot az alkalmazás telepítéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1694b-124">To install the SendGrid NuGet package in your application, do the following:</span></span>

1. <span data-ttu-id="1694b-125">Kattintson a **új projekt** válassza ki a **sablon**.</span><span class="sxs-lookup"><span data-stu-id="1694b-125">Click on **New Project** and select a **Template**.</span></span>

   ![Új projekt létrehozása][create-new-project]
2. <span data-ttu-id="1694b-127">A **Megoldáskezelőben**, kattintson a jobb gombbal **hivatkozások**, majd kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="1694b-127">In **Solution Explorer**, right-click **References**, then click **Manage NuGet Packages**.</span></span>

   ![SendGrid NuGet-csomag][SendGrid-NuGet-package]
3. <span data-ttu-id="1694b-129">Keresse meg **SendGrid** válassza ki a **SendGrid** elem az eredménylistában.</span><span class="sxs-lookup"><span data-stu-id="1694b-129">Search for **SendGrid** and select the **SendGrid** item in the results list.</span></span>
4. <span data-ttu-id="1694b-130">Válassza ki a Nuget-csomag stabil legújabb verzióját kell lennie a hálózatiobjektum-modellje dolgozni a verzió legördülő listából, és az ebben a cikkben egy API-k.</span><span class="sxs-lookup"><span data-stu-id="1694b-130">Select the latest stable version of the Nuget package from the version dropdown to be able to work with the object model and APIs demonstrated in this article.</span></span>

   ![SendGrid csomag][sendgrid-package]
5. <span data-ttu-id="1694b-132">Kattintson a **telepítése** a telepítés befejezéséhez, és zárja be ezt a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="1694b-132">Click **Install** to complete the installation, and then close this dialog.</span></span>

<span data-ttu-id="1694b-133">SendGrid tartozó .NET osztálytár nevezik **SendGrid**.</span><span class="sxs-lookup"><span data-stu-id="1694b-133">SendGrid's .NET class library is called **SendGrid**.</span></span> <span data-ttu-id="1694b-134">A következő névterek tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1694b-134">It contains the following namespaces:</span></span>

* <span data-ttu-id="1694b-135">**SendGrid** SendGrid tartozó API való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="1694b-135">**SendGrid** for communicating with SendGrid’s API.</span></span>
* <span data-ttu-id="1694b-136">**SendGrid.Helpers.Mail** segítő módszerek könnyen hozzanak létre az e-mailek küldése megadott SendGridMessage objektumokat.</span><span class="sxs-lookup"><span data-stu-id="1694b-136">**SendGrid.Helpers.Mail** for helper methods to easily create SendGridMessage objects that specify how to send emails.</span></span>

<span data-ttu-id="1694b-137">A következő kód névtér-deklarációk hozzáadása bármely kívánja programon keresztüli eléréséhez a SendGrid e-mail szolgáltatás C#-fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="1694b-137">Add the following code namespace declarations to the top of any C# file in which you want to programmatically access the SendGrid email service.</span></span>

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a><span data-ttu-id="1694b-138">Hogyan: hozzon létre egy e-mailt</span><span class="sxs-lookup"><span data-stu-id="1694b-138">How to: Create an Email</span></span>
<span data-ttu-id="1694b-139">Használja a **SendGridMessage** objektum e-mailt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1694b-139">Use the **SendGridMessage** object to create an email message.</span></span> <span data-ttu-id="1694b-140">Az üzenet-objektum létrehozása után beállíthatja a tulajdonságokat és metódusokat, beleértve az e-mailt olyasvalaki, az e-mail címzettjét, és a tárgy és törzs az e-mailek.</span><span class="sxs-lookup"><span data-stu-id="1694b-140">Once the message object is created, you can set properties and methods, including the email sender, the email recipient, and the subject and body of the email.</span></span>

<span data-ttu-id="1694b-141">A következő példa bemutatja, hogyan hozhat létre teljesen ki van töltve e-mail objektumot:</span><span class="sxs-lookup"><span data-stu-id="1694b-141">The following example demonstrates how to create a fully populated email object:</span></span>

    var msg = new SendGridMessage();

    msg.SetFrom(new EmailAddress("dx@example.com", "SendGrid DX Team"));

    var recipients = new List<EmailAddress>
    {
        new EmailAddress("jeff@example.com", "Jeff Smith"),
        new EmailAddress("anna@example.com", "Anna Lidman"),
        new EmailAddress("peter@example.com", "Peter Saddow")
    };
    msg.AddTos(recipients);

    msg.SetSubject("Testing the SendGrid C# Library");

    msg.AddContent(MimeType.Text, "Hello World plain text!");
    msg.AddContent(MimeType.Html, "<p>Hello World!</p>");

<span data-ttu-id="1694b-142">Minden olyan tulajdonságok és módszerek által támogatott további információt a **SendGrid** írja be, lásd: [sendgrid-c Sharp] [ sendgrid-csharp] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="1694b-142">For more information on all properties and methods supported by the **SendGrid** type, see [sendgrid-csharp][sendgrid-csharp] on GitHub.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="1694b-143">Útmutató: az e-mailek küldése</span><span class="sxs-lookup"><span data-stu-id="1694b-143">How to: Send an Email</span></span>
<span data-ttu-id="1694b-144">Miután létrehozta az e-mailt, elküldheti azt a SendGrid tartozó API-val.</span><span class="sxs-lookup"><span data-stu-id="1694b-144">After creating an email message, you can send it using SendGrid's API.</span></span> <span data-ttu-id="1694b-145">Másik megoldásként használhat [. NET meg a szalagtár beépített][NET-library].</span><span class="sxs-lookup"><span data-stu-id="1694b-145">Alternatively, you may use [.NET's built in library][NET-library].</span></span>

<span data-ttu-id="1694b-146">E-mailek küldéséhez, hogy megadnia a SendGrid API-kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="1694b-146">Sending email requires that you supply your SendGrid API Key.</span></span> <span data-ttu-id="1694b-147">Ha API-kulcsokat konfigurálásával kapcsolatos részletek van szüksége, látogasson el a SendGrid tartozó API-kulcsokat [dokumentáció][documentation].</span><span class="sxs-lookup"><span data-stu-id="1694b-147">If you need details about how to configure API Keys, please visit SendGrid's API Keys [documentation][documentation].</span></span>

<span data-ttu-id="1694b-148">Az Azure portálon kattintson az alkalmazás beállításait, és vegye fel a kulcs/érték párok az alkalmazásbeállítások tárolhatjuk ezeket a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="1694b-148">You may store these credentials via your Azure Portal by clicking Application settings and adding the key/value pairs under App settings.</span></span>

 ![Az Azure alkalmazás beállításai][azure_app_settings]

 <span data-ttu-id="1694b-150">Majd hogy a hozzáférés az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1694b-150">Then, you may access them as follows:</span></span>

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

<span data-ttu-id="1694b-151">A következő példák bemutatják, hogyan küldhető a webes API-val.</span><span class="sxs-lookup"><span data-stu-id="1694b-151">The following examples show how to send a message using the Web API.</span></span>

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
                    Subject = "Hello World from the SendGrid CSharp SDK!",
                    PlainTextContent = "Hello, Email!",
                    HtmlContent = "<strong>Hello, Email!</strong>"
                };
                msg.AddTo(new EmailAddress("test@example.com", "Test User"));
                var response = await client.SendEmailAsync(msg);
            }
        }
    }

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="1694b-152">Hogyan: hozzá mellékletet</span><span class="sxs-lookup"><span data-stu-id="1694b-152">How to: Add an attachment</span></span>
<span data-ttu-id="1694b-153">Mellékletek meghívásával adhatók hozzá egy üzenetet a **AddAttachment** metódus és minimálisan megadása a fájl nevét és a Base64-kódolású tartalmat kíván csatolni.</span><span class="sxs-lookup"><span data-stu-id="1694b-153">Attachments can be added to a message by calling the **AddAttachment** method and minimally specifying the file name and Base64 encoded content you want to attach.</span></span> <span data-ttu-id="1694b-154">A metódus hívása után minden fájlt csatolni szeretné vagy segítségével megadhat több mellékletet a **AddAttachments** metódust.</span><span class="sxs-lookup"><span data-stu-id="1694b-154">You can include multiple attachments by calling this method once for each file you wish to attach or by using the **AddAttachments** method.</span></span> <span data-ttu-id="1694b-155">A következő példa bemutatja, egy üzenet melléklet hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="1694b-155">The following example demonstrates adding an attachment to a message:</span></span>

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-to-enable-footers-tracking-and-analytics"></a><span data-ttu-id="1694b-156">Hogyan: láblécben, nyomon követési és elemzés engedélyezése levelezési beállítások használatával</span><span class="sxs-lookup"><span data-stu-id="1694b-156">How to: Use mail settings to enable footers, tracking, and analytics</span></span>
<span data-ttu-id="1694b-157">SendGrid a levelezési beállításokat és a nyomon követési beállítások további e-mail funkciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="1694b-157">SendGrid provides additional email functionality through the use of mail settings and tracking settings.</span></span> <span data-ttu-id="1694b-158">Ezek a beállítások is hozzáadhatók az e-mailt adott funkciónak például kattintson nyomon követése, Google analytics, követési előfizetés, stb.</span><span class="sxs-lookup"><span data-stu-id="1694b-158">These settings can be added to an email message to enable specific functionality such as click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="1694b-159">Alkalmazások teljes listáját lásd: a [beállítások dokumentáció][settings-documentation].</span><span class="sxs-lookup"><span data-stu-id="1694b-159">For a full list of apps, see the [Settings Documentation][settings-documentation].</span></span>

<span data-ttu-id="1694b-160">Alkalmazások alkalmazhatók **SendGrid** e-mailek részeként megvalósított metódusok használata a **SendGridMessage** osztály.</span><span class="sxs-lookup"><span data-stu-id="1694b-160">Apps can be applied to **SendGrid** email messages using methods implemented as part of the **SendGridMessage** class.</span></span> <span data-ttu-id="1694b-161">Az alábbi példák bemutatják, a láblécben, majd kattintson a szűrő nyomon követése:</span><span class="sxs-lookup"><span data-stu-id="1694b-161">The following examples demonstrate the footer and click tracking filters:</span></span>

<span data-ttu-id="1694b-162">Az alábbi példák bemutatják, a láblécben, majd kattintson a szűrő nyomon követése:</span><span class="sxs-lookup"><span data-stu-id="1694b-162">The following examples demonstrate the footer and click tracking filters:</span></span>

### <a name="footer-settings"></a><span data-ttu-id="1694b-163">Élőláb beállításai</span><span class="sxs-lookup"><span data-stu-id="1694b-163">Footer settings</span></span>
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a><span data-ttu-id="1694b-164">Kattintson a nyomon követése</span><span class="sxs-lookup"><span data-stu-id="1694b-164">Click tracking</span></span>
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="1694b-165">Útmutató: további SendGrid szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="1694b-165">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="1694b-166">SendGrid több API-k és webhookokkal, amelyek segítségével kihasználhatja az Azure alkalmazáson belül további funkciókat kínál.</span><span class="sxs-lookup"><span data-stu-id="1694b-166">SendGrid offers several APIs and webhooks that you can use to leverage additional functionality within your Azure application.</span></span> <span data-ttu-id="1694b-167">További részletekért lásd: a [SendGrid API-referencia][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="1694b-167">For more details, see the [SendGrid API Reference][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="1694b-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1694b-168">Next steps</span></span>
<span data-ttu-id="1694b-169">Most, hogy megismerte a SendGrid E-mail szolgáltatás alapjait, az alábbi hivatkozásokból további.</span><span class="sxs-lookup"><span data-stu-id="1694b-169">Now that you've learned the basics of the SendGrid Email service, follow these links to learn more.</span></span>

* <span data-ttu-id="1694b-170">SendGrid C\# könyvtár tárház: [sendgrid-c Sharp][sendgrid-csharp]</span><span class="sxs-lookup"><span data-stu-id="1694b-170">SendGrid C\# library repo: [sendgrid-csharp][sendgrid-csharp]</span></span>
* <span data-ttu-id="1694b-171">SendGrid API dokumentációjának: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="1694b-171">SendGrid API documentation: <https://sendgrid.com/docs></span></span>

[Next steps]: #next-steps
[What is the SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference the SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
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

<span data-ttu-id="1694b-172">[felhőalapú szolgáltatás]: https://sendgrid.com/solutions</span><span class="sxs-lookup"><span data-stu-id="1694b-172">[cloud-based email service]: https://sendgrid.com/solutions</span></span>
<span data-ttu-id="1694b-173">[tranzakciós e-mailben kézbesítésre]: https://sendgrid.com/use-cases/transactional-email</span><span class="sxs-lookup"><span data-stu-id="1694b-173">[transactional email delivery]: https://sendgrid.com/use-cases/transactional-email</span></span>

