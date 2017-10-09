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
# <a name="how-toosend-email-using-sendgrid-with-azure"></a>Hogyan tooSend E-mail használatával SendGrid az Azure-ral
## <a name="overview"></a>Áttekintés
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok a sendgrid e-mail szolgáltatás az Azure-on. hello minták C nyelven íródtak\# és támogatja a .NET-szabvány 1.3. hello ismertetett forgatókönyvek hozhat létre, e-mailek, e-mailek küldéséhez, hozzáadása a mellékleteket, és különböző mail engedélyezése és nyomon követési beállítások közé tartozik. A SendGrid és e-mailek küldéséhez további információkért lásd: hello [további lépések] [ Next steps] szakasz.

## <a name="what-is-hello-sendgrid-email-service"></a>Mi az az üdvözlő E-mail szolgáltatás SendGrid?
SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt. Közös SendGrid használati esetek a következők:

* Visszaigazolások vagy beszerzési visszaigazolások toocustomers automatikus küldése.
* Az ügyfelek havi szórólapok és előléptetések terjesztési felügyelete sorolja fel.
* Többek között a letiltott e-mailek és az ügyfél engagement valós idejű metrikáját gyűjtése.
* Továbbító ügyfél lekérdezések.
* A bejövő e-mailek feldolgozása.

További információkért látogasson el a [https://sendgrid.com](https://sendgrid.com) vagy SendGrid tartozó [C# könyvtár] [ sendgrid-csharp] GitHub-tárház.

## <a name="create-a-sendgrid-account"></a>A SendGrid-fiók létrehozása
[!INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-net-class-library"></a>Hivatkozás hello SendGrid .NET Class Library
Hello [SendGrid NuGet-csomag](https://www.nuget.org/packages/Sendgrid) hello legegyszerűbb módja tooget hello SendGrid API és tooconfigure van az alkalmazás összes függőségét. NuGet egy Visual Studio bővítmény a Microsoft Visual Studio 2015-öt és a fent, amely segítségével könnyen tooinstall és frissítés kódtárak és eszközök.

> [!NOTE]
> tooinstall NuGet Ha futtatja a Visual Studio verziója korábbi, mint a Visual Studio 2015-öt, látogasson el [http://www.nuget.org](http://www.nuget.org), és kattintson a hello **NuGet telepítése** gombra.
>
>

az alkalmazás SendGrid NuGet-csomagot tooinstall hello hello a következő:

1. Kattintson a **új projekt** válassza ki a **sablon**.

   ![Új projekt létrehozása][create-new-project]
2. A **Megoldáskezelőben**, kattintson a jobb gombbal **hivatkozások**, majd kattintson a **NuGet-csomagok kezelése**.

   ![SendGrid NuGet-csomag][SendGrid-NuGet-package]
3. Keresse meg **SendGrid** és select hello **SendGrid** elem az eredménylistában.
4. Hello verzió legördülő toobe képes toowork hello hálózatiobjektum-modellje és API-k, a cikkben bemutatott hello legújabb stabil verziójának hello Nuget-csomag kiválasztása

   ![SendGrid csomag][sendgrid-package]
5. Kattintson a **telepítése** toocomplete hello telepítését, és zárja be ezt a párbeszédpanelt.

SendGrid tartozó .NET osztálytár nevezik **SendGrid**. A következő névterek hello tartalmazza:

* **SendGrid** SendGrid tartozó API való kommunikációhoz.
* **SendGrid.Helpers.Mail** segítő módszerek tooeasily adja meg, hogyan toosend e-mailt küld SendGridMessage objektumokat hozzon létre.

Adja hozzá a következő kód névtér nyilatkozatok toohello felső bármely C# fájl tooprogrammatically hello SendGrid e-mail szolgáltatás kívánt hello.

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a>Hogyan: hozzon létre egy e-mailt
Használjon hello **SendGridMessage** objektum toocreate e-mailt. Hello üzenet objektum létrehozása után beállíthatja a tulajdonságokat és metódusokat, beleértve az üdvözlő e-mailt olyasvalaki, hello e-mail címzettje, és hello tulajdonos üdvözlő e-mail törzsét.

hello következő példa bemutatja, hogyan toocreate egy teljes mértékben ki van töltve e-mail objektum:

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

Minden olyan tulajdonságok és módszerek által támogatott további információt a **SendGrid** írja be, lásd: [sendgrid-c Sharp] [ sendgrid-csharp] a Githubon.

## <a name="how-to-send-an-email"></a>Útmutató: az e-mailek küldése
Miután létrehozta az e-mailt, elküldheti azt a SendGrid tartozó API-val. Másik megoldásként használhat [. NET meg a szalagtár beépített][NET-library].

E-mailek küldéséhez, hogy megadnia a SendGrid API-kulcs szükséges. Ha részletes tájékoztatást kell tooconfigure API-kulcsokat, látogasson el a SendGrid tartozó API-kulcsokat [dokumentáció][documentation].

Ezeket a hitelesítő adatokat az Azure portálon gombra az alkalmazásbeállítások és hozzáadását hello kulcs/érték párok az alkalmazásbeállítások tárolhatjuk.

 ![Az Azure alkalmazás beállításai][azure_app_settings]

 Majd hogy a hozzáférés az alábbiak szerint:

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

hello a következő példák bemutatják, hogyan egy üzenet használatával toosend hello Web API.

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

## <a name="how-to-add-an-attachment"></a>Hogyan: hozzá mellékletet
Mellékletek adható tooa üzenet hívó hello **AddAttachment** metódus és a minimálisan megadó hello fájl nevét és a Base64-kódolású tartalom szeretné tooattach. A metódus meghívása után a fájl tooattach kívánja vagy hello segítségével megadhat több mellékletek **AddAttachments** metódust. hello a következő példa bemutatja, hogy egy mellékletet tooa üzenet hozzáadása:

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-tooenable-footers-tracking-and-analytics"></a>Útmutató: e-mail beállítások tooenable láblécben, nyomon követési és analytics
SendGrid a levelezési beállításokat és a nyomon követési beállítások hello használata további e-mail funkciókat biztosítja. Ezek a beállítások is hozzáadhatók a tooan e-mail üzenet tooenable adott funkciók, például kattintson nyomon követése, Google analytics, előfizetés nyomon követése és stb. Alkalmazások teljes listáját lásd: hello [beállítások dokumentáció][settings-documentation].

Alkalmazások túl alkalmazható**SendGrid** e-mailek hello részeként megvalósított metódusok használatával **SendGridMessage** osztály. hello alábbi példák bemutatják, hello élőláb, majd kattintson szűrők nyomon követése:

hello alábbi példák bemutatják, hello élőláb, majd kattintson szűrők nyomon követése:

### <a name="footer-settings"></a>Élőláb beállításai
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a>Kattintson a nyomon követése
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Útmutató: további SendGrid szolgáltatásokkal
SendGrid több API-k és webhookokkal, használhatja az Azure alkalmazáson belül tooleverage további funkciókat kínál. További részletekért lásd: hello [SendGrid API-referencia][SendGrid API documentation].

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a SendGrid E-mail szolgáltatás hello hello alapjait, kövesse a további hivatkozások toolearn.

* SendGrid C\# könyvtár tárház: [sendgrid-c Sharp][sendgrid-csharp]
* SendGrid API dokumentációjának: <https://sendgrid.com/docs>

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

