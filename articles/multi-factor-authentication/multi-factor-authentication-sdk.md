---
title: "aaaMFA software development kit egyéni alkalmazásokba |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toodownload és -felhasználási hello Azure MFA SDK tooenable kétlépéses ellenőrzés az egyéni alkalmazások."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1c152f67-be02-42a5-a0c7-246fb6b34377
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 10e02e844bf3928575bfca79dbc34717a31a08b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Building a multi-factor Authentication egyéni alkalmazásokba (SDK)

bejelentkezési hello hello Azure multi-factor Authentication Software Development Kit (SDK) segítségével készít közvetlenül a kétlépéses ellenőrzést, vagy a tranzakció dolgozza fel az alkalmazások az Azure AD-bérlőben.

a multi-factor Authentication SDK hello C#, Visual Basic (.NET), Java, Perl, PHP és Ruby érhető el. hello SDK kínál egy vékony kétlépéses ellenőrzést. Mindent, amire szüksége toowrite a kódot, beleértve a megjegyzésként forráskódfájl, például fájlok és a részletes információs fájl tartalmazza. Minden SDK is a tanúsítvány és a titkos kulcs titkosításához tranzakciók, amelyek egyedi tooyour többtényezős hitelesítésszolgáltató. Mindaddig, amíg még meg olyan szolgáltatót, letöltheti hello SDK annyi nyelveket és formátumban, szükség szerint.

a multi-factor Authentication SDK hello API-k hello hello szerkezete felettébb egyszerű. Ellenőrizze a hello többtényezős beállítás paraméterek (például a hitelesítési mód) és a felhasználói adatok (például a telefon száma toocall hello vagy PIN-kód számú toovalidate hello) tooan API hívása egy funkcióval. hello API-k fordítása hello függvény hívása web services kérelmek toohello felhőalapú Azure multi-factor Authentication szolgáltatást. Az összes hívás hivatkozás toohello személyes tanúsítványt, amely minden SDK megtalálható tartalmaznia kell.

Mivel hello API-k nincsenek regisztrálva az Azure Active Directory hozzáférési toousers, meg kell adni egy fájl vagy az adatbázis felhasználói adatokat. Is hello API-k nem tartalmaz regisztrációs vagy a felhasználó felügyeleti funkciókat, így toobuild kell ezeket a folyamatokat az alkalmazásba.

> [!IMPORTANT]
> toodownload hello SDK, kell, hogy az Azure multi-factor Auth Provider toocreate akkor is, ha az Azure MFA-, prémium szintű, vagy az EMS licenccel rendelkezik. Ha erre a célra az Azure többtényezős hitelesítésszolgáltató létrehozása, és már rendelkezik licenccel, győződjön meg arról, hogy toocreate hello szolgáltató a hello **Per Enabled User** modell. Ezt követően kapcsolja hello szolgáltató toohello tartalmazó könyvtár hello Azure MFA, a prémium szintű Azure AD vagy az EMS-licenceket. Ez a konfiguráció biztosítja, hogy Ön csak számlázása Amennyiben több egyedi felhasználók hello mint saját licencek száma hello SDK használatával.


## <a name="download-hello-sdk"></a>Hello SDK letöltése
Hello Azure multi-factor Authentication SDK letöltése szükséges egy [Azure többtényezős hitelesítésszolgáltató](multi-factor-authentication-get-started-auth-provider.md).  Ehhez a teljes Azure-előfizetéssel, akkor is, ha az Azure MFA, az Azure AD Premium vagy a nagyvállalati mobilitási csomag licencek a tulajdonosa.  toodownload hello SDK, keresse meg a multi-factor Authentication kezelési portál toohello. Hello portal hello közvetlenül a többtényezős hitelesítésszolgáltató kezelése, vagy hello kattintva képes elérni **"Ugrás toohello portal"** hello multi-factor Authentication szolgáltatás beállításainak lapon hivatkozásra.

### <a name="download-from-hello-azure-classic-portal"></a>A klasszikus Azure portálon hello letöltése
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) rendszergazdaként.
2. Hello bal oldalon válassza ki a **Active Directory**.
3. Az oldalon hello Active Directory hello felső válassza ki a **többtényezős hitelesítésszolgáltatók**
4. Hello alján válassza **kezelése**. Megnyílik egy új lap.
5. A hello lap alján, bal oldali hello kattintson **SDK**.
   <center>![Letöltése](./media/multi-factor-authentication-sdk/download.png)</center>
6. Válassza ki, majd kattintson egy hello letöltési hivatkozás hello nyelvét.
7. Hello menteni.

### <a name="download-from-hello-service-settings"></a>Töltse le a hello szolgáltatás beállításai
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) rendszergazdaként.
2. Hello bal oldalon válassza ki a **Active Directory**.
3. Kattintson duplán az Azure AD-példányra.
4. A hello felső kattintson **konfigurálása**
5. Válassza ki a multi-factor Authentication hitelesítés **szolgáltatás beállításainak kezelése**
   ![letöltése](./media/multi-factor-authentication-sdk/download2.png)
6. A hello services beállítások oldalán üdvözlő képernyőt hello alján kattintson **Ugrás toohello portal**. Megnyílik egy új lap.
   ![Letöltés](./media/multi-factor-authentication-sdk/download3a.png)
7. A hello lap alján, bal oldali hello kattintson **SDK**.
8. Válassza ki, majd kattintson egy hello letöltési hivatkozás hello nyelvét.
9. Hello menteni.

## <a name="whats-in-hello-sdk"></a>A hello SDK
hello SDK hello a következő elemeket tartalmazza:

* **INFORMÁCIÓS**. Ismerteti, hogyan toouse hello egy új vagy meglévő alkalmazáshoz a multi-factor Authentication API-k.
* **Forrásfájlokat** a többtényezős hitelesítés
* **Ügyféltanúsítvány** toocommunicate használ multi-factor Authentication szolgáltatás hello
* **Titkos kulcs** hello tanúsítvány
* **Hívás eredményeit.** Hívási eredménykódok listáját. tooopen ezt a fájlt, a szövegformázási, például a WordPad alkalmazás használjon. Használjon hello hívás eredménye kódok tootest, és az alkalmazás hello multi-factor Authentication végrehajtása hibaelhárítása. Nincsenek hitelesítési állapotkódok.
* **Példák.** A multi-factor Authentication használatának megvalósítási mintakód.

> [!WARNING]
> hello ügyféltanúsítvány egy egyedi személyes tanúsítvány kifejezetten jött létre. Ne ossza, vagy elveszíti a fájl. A kulcs tooensuring hello biztonsági hello multi-factor Authentication szolgáltatással folytatott kommunikáció.

## <a name="code-sample"></a>Kódminta
A mintakód bemutatja, hogyan toouse hello API-k a hello Azure multi-factor Authentication SDK tooadd szabványos mód hang hívás ellenőrzési tooyour alkalmazás. Szabványos módban a telefonhívások, amely a felhasználó megválaszolja tooby hello # billentyű lenyomásával hello.

Ez a példa egy egyszerű ASP.NET-alkalmazást a C# .NET 2.0 multi-factor Authentication SDK hello C# kiszolgálóoldali logikával, de hello folyamat hasonlít más nyelveken. Hello SDK tartalmaz forrásfájlokat, nem végrehajtható fájlok, mert hello fájlok létrehozása és azok hivatkozik, vagy azokat felvehetik az alkalmazás közvetlenül az.

> [!NOTE]
> A multi-factor Authentication végrehajtásakor módszerekkel hello további (telefonhívást vagy SMS-üzenet) másodlagos vagy harmadlagos ellenőrzési toosupplement, az elsődleges hitelesítési módszert (felhasználónév és jelszó). Ezek a módszerek nem elsődleges hitelesítési módszerek tervezték.

### <a name="code-sample-overview"></a>Kód a minta áttekintése
A mintakód egyszerű bemutató webalkalmazás használ a telefonhívások egy # kulcs válasz tooverify hello felhasználói hitelesítéssel. A telefonhívás tényező szabványos mód néven a multi-factor Authentication szolgáltatásra.

hello ügyféloldali kódot nem tartalmazza a multi-factor Authentication-specifikus elemeket. Mert hello további hitelesítési tényezők független hello elsődleges hitelesítéshez, adja hozzá őket hello meglévő bejelentkezés felület módosítása nélkül. a hello multi-factor Authentication SDK API-k hello segítségével testre szabható a hello felhasználói felületet, de előfordulhat, hogy nem kell toochange semmit egyáltalán.

hello kiszolgálóoldali kód hozzáadása a normál módú hitelesítést 2. lépésben. Létrehoz egy PfAuthParams objektum normál módú ellenőrzéshez szükséges hello paraméterekkel: felhasználónév, telefon számát, valamint módot, és hello elérési toohello ügyféltanúsítvány (CertFilePath), amelyre azonban szükség van minden egyes hívásban. A minden paraméter PfAuthParams bemutatója, lásd: hello SDK hello példa fájlt.

A következő hello kód hello PfAuthParams objektum toohello pf_authenticate() függvény adja át. hello visszatérési érték hello sikerességét vagy sikertelenségét hello hitelesítés. Kimenő paraméterek callStatus és errorID hello, további hívás eredménye információkat tartalmaznak. hello hívási eredménykódok hello hívás eredményfájl hello SDK szerepelnek.

A minimális végrehajtása néhány sor is beírhatók. Azonban az éles kódban, hogy tartalmazhat kifinomultabb hibakezelés, az adatbázis további kódot és jobb felhasználói élményt.

### <a name="web-client-code"></a>Webalkalmazás ügyféloldali kódot
hello webes Ügyfélkód bemutató lapon látható.

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Kiszolgálóoldali kódolás
Hello kiszolgálóoldali kód a következő, a multi-factor Authentication konfigurálva, és futtassa a 2. Szabványos mód (MODE_STANDARD), a telefonhívás toowhich hello felhasználó válaszol hello # billentyű lenyomása mellett.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate hello username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from hello user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains hello private key for hello client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }
