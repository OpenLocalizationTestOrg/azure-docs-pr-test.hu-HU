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
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a><span data-ttu-id="81763-103">Building a multi-factor Authentication egyéni alkalmazásokba (SDK)</span><span class="sxs-lookup"><span data-stu-id="81763-103">Building Multi-Factor Authentication into Custom Apps (SDK)</span></span>

<span data-ttu-id="81763-104">bejelentkezési hello hello Azure multi-factor Authentication Software Development Kit (SDK) segítségével készít közvetlenül a kétlépéses ellenőrzést, vagy a tranzakció dolgozza fel az alkalmazások az Azure AD-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="81763-104">hello Azure Multi-Factor Authentication Software Development Kit (SDK) lets you build two-step verification directly into hello sign-in or transaction processes of applications in your Azure AD tenant.</span></span>

<span data-ttu-id="81763-105">a multi-factor Authentication SDK hello C#, Visual Basic (.NET), Java, Perl, PHP és Ruby érhető el.</span><span class="sxs-lookup"><span data-stu-id="81763-105">hello Multi-Factor Authentication SDK is available for C#, Visual Basic (.NET), Java, Perl, PHP, and Ruby.</span></span> <span data-ttu-id="81763-106">hello SDK kínál egy vékony kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="81763-106">hello SDK provides a thin wrapper around two-step verification.</span></span> <span data-ttu-id="81763-107">Mindent, amire szüksége toowrite a kódot, beleértve a megjegyzésként forráskódfájl, például fájlok és a részletes információs fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="81763-107">It includes everything you need toowrite your code, including commented source code files, example files, and a detailed ReadMe file.</span></span> <span data-ttu-id="81763-108">Minden SDK is a tanúsítvány és a titkos kulcs titkosításához tranzakciók, amelyek egyedi tooyour többtényezős hitelesítésszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="81763-108">Each SDK also includes a certificate and private key for encrypting transactions that are unique tooyour Multi-Factor Authentication Provider.</span></span> <span data-ttu-id="81763-109">Mindaddig, amíg még meg olyan szolgáltatót, letöltheti hello SDK annyi nyelveket és formátumban, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="81763-109">As long as you have a provider, you can download hello SDK in as many languages and formats as you need.</span></span>

<span data-ttu-id="81763-110">a multi-factor Authentication SDK hello API-k hello hello szerkezete felettébb egyszerű.</span><span class="sxs-lookup"><span data-stu-id="81763-110">hello structure of hello APIs in hello Multi-Factor Authentication SDK is simple.</span></span> <span data-ttu-id="81763-111">Ellenőrizze a hello többtényezős beállítás paraméterek (például a hitelesítési mód) és a felhasználói adatok (például a telefon száma toocall hello vagy PIN-kód számú toovalidate hello) tooan API hívása egy funkcióval.</span><span class="sxs-lookup"><span data-stu-id="81763-111">Make a single function call tooan API with hello multi-factor option parameters (like verification mode) and user data (like hello telephone number toocall or hello PIN number toovalidate).</span></span> <span data-ttu-id="81763-112">hello API-k fordítása hello függvény hívása web services kérelmek toohello felhőalapú Azure multi-factor Authentication szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="81763-112">hello APIs translate hello function call into web services requests toohello cloud-based Azure Multi-Factor Authentication Service.</span></span> <span data-ttu-id="81763-113">Az összes hívás hivatkozás toohello személyes tanúsítványt, amely minden SDK megtalálható tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="81763-113">All calls must include a reference toohello private certificate that is included in every SDK.</span></span>

<span data-ttu-id="81763-114">Mivel hello API-k nincsenek regisztrálva az Azure Active Directory hozzáférési toousers, meg kell adni egy fájl vagy az adatbázis felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="81763-114">Because hello APIs do not have access toousers registered in Azure Active Directory, you must provide user information in a file or database.</span></span> <span data-ttu-id="81763-115">Is hello API-k nem tartalmaz regisztrációs vagy a felhasználó felügyeleti funkciókat, így toobuild kell ezeket a folyamatokat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="81763-115">Also, hello APIs do not provide enrollment or user management features, so you need toobuild these processes into your application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="81763-116">toodownload hello SDK, kell, hogy az Azure multi-factor Auth Provider toocreate akkor is, ha az Azure MFA-, prémium szintű, vagy az EMS licenccel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="81763-116">toodownload hello SDK, you need toocreate an Azure Multi-Factor Auth Provider even if you have Azure MFA, AAD Premium, or EMS licenses.</span></span> <span data-ttu-id="81763-117">Ha erre a célra az Azure többtényezős hitelesítésszolgáltató létrehozása, és már rendelkezik licenccel, győződjön meg arról, hogy toocreate hello szolgáltató a hello **Per Enabled User** modell.</span><span class="sxs-lookup"><span data-stu-id="81763-117">If you create an Azure Multi-Factor Auth Provider for this purpose and already have licenses, make sure toocreate hello Provider with hello **Per Enabled User** model.</span></span> <span data-ttu-id="81763-118">Ezt követően kapcsolja hello szolgáltató toohello tartalmazó könyvtár hello Azure MFA, a prémium szintű Azure AD vagy az EMS-licenceket.</span><span class="sxs-lookup"><span data-stu-id="81763-118">Then, link hello Provider toohello directory that contains hello Azure MFA, Azure AD Premium, or EMS licenses.</span></span> <span data-ttu-id="81763-119">Ez a konfiguráció biztosítja, hogy Ön csak számlázása Amennyiben több egyedi felhasználók hello mint saját licencek száma hello SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="81763-119">This configuration ensures that you are only billed if you have more unique users using hello SDK than hello number of licenses you own.</span></span>


## <a name="download-hello-sdk"></a><span data-ttu-id="81763-120">Hello SDK letöltése</span><span class="sxs-lookup"><span data-stu-id="81763-120">Download hello SDK</span></span>
<span data-ttu-id="81763-121">Hello Azure multi-factor Authentication SDK letöltése szükséges egy [Azure többtényezős hitelesítésszolgáltató](multi-factor-authentication-get-started-auth-provider.md).</span><span class="sxs-lookup"><span data-stu-id="81763-121">Downloading hello Azure Multi-Factor SDK requires an [Azure Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md).</span></span>  <span data-ttu-id="81763-122">Ehhez a teljes Azure-előfizetéssel, akkor is, ha az Azure MFA, az Azure AD Premium vagy a nagyvállalati mobilitási csomag licencek a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="81763-122">This requires a full Azure subscription, even if Azure MFA, Azure AD Premium, or Enterprise Mobility Suite licenses are owned.</span></span>  <span data-ttu-id="81763-123">toodownload hello SDK, keresse meg a multi-factor Authentication kezelési portál toohello.</span><span class="sxs-lookup"><span data-stu-id="81763-123">toodownload hello SDK, navigate toohello Multi-Factor Management Portal.</span></span> <span data-ttu-id="81763-124">Hello portal hello közvetlenül a többtényezős hitelesítésszolgáltató kezelése, vagy hello kattintva képes elérni **"Ugrás toohello portal"** hello multi-factor Authentication szolgáltatás beállításainak lapon hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="81763-124">You can reach hello portal either by managing hello Multi-Factor Auth Provider directly, or by clicking hello **"Go toohello portal"** link on hello MFA service settings page.</span></span>

### <a name="download-from-hello-azure-classic-portal"></a><span data-ttu-id="81763-125">A klasszikus Azure portálon hello letöltése</span><span class="sxs-lookup"><span data-stu-id="81763-125">Download from hello Azure classic portal</span></span>
1. <span data-ttu-id="81763-126">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="81763-126">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="81763-127">Hello bal oldalon válassza ki a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="81763-127">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="81763-128">Az oldalon hello Active Directory hello felső válassza ki a **többtényezős hitelesítésszolgáltatók**</span><span class="sxs-lookup"><span data-stu-id="81763-128">On hello Active Directory page, at hello top select **Multi-Factor Auth Providers**</span></span>
4. <span data-ttu-id="81763-129">Hello alján válassza **kezelése**.</span><span class="sxs-lookup"><span data-stu-id="81763-129">At hello bottom select **Manage**.</span></span> <span data-ttu-id="81763-130">Megnyílik egy új lap.</span><span class="sxs-lookup"><span data-stu-id="81763-130">A new page opens.</span></span>
5. <span data-ttu-id="81763-131">A hello lap alján, bal oldali hello kattintson **SDK**.</span><span class="sxs-lookup"><span data-stu-id="81763-131">On hello left, at hello bottom, click **SDK**.</span></span>
   <span data-ttu-id="81763-132"><center>![Letöltése](./media/multi-factor-authentication-sdk/download.png)</center></span><span class="sxs-lookup"><span data-stu-id="81763-132"><center>![Download](./media/multi-factor-authentication-sdk/download.png)</center></span></span>
6. <span data-ttu-id="81763-133">Válassza ki, majd kattintson egy hello letöltési hivatkozás hello nyelvét.</span><span class="sxs-lookup"><span data-stu-id="81763-133">Select hello language you want and click one hello associated download links.</span></span>
7. <span data-ttu-id="81763-134">Hello menteni.</span><span class="sxs-lookup"><span data-stu-id="81763-134">Save hello download.</span></span>

### <a name="download-from-hello-service-settings"></a><span data-ttu-id="81763-135">Töltse le a hello szolgáltatás beállításai</span><span class="sxs-lookup"><span data-stu-id="81763-135">Download from hello service settings</span></span>
1. <span data-ttu-id="81763-136">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="81763-136">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="81763-137">Hello bal oldalon válassza ki a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="81763-137">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="81763-138">Kattintson duplán az Azure AD-példányra.</span><span class="sxs-lookup"><span data-stu-id="81763-138">Double-click your instance of Azure AD.</span></span>
4. <span data-ttu-id="81763-139">A hello felső kattintson **konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="81763-139">At hello top click **Configure**</span></span>
5. <span data-ttu-id="81763-140">Válassza ki a multi-factor Authentication hitelesítés **szolgáltatás beállításainak kezelése**
   ![letöltése](./media/multi-factor-authentication-sdk/download2.png)</span><span class="sxs-lookup"><span data-stu-id="81763-140">Under multi-factor authentication, select **Manage service settings**
![Download](./media/multi-factor-authentication-sdk/download2.png)</span></span>
6. <span data-ttu-id="81763-141">A hello services beállítások oldalán üdvözlő képernyőt hello alján kattintson **Ugrás toohello portal**.</span><span class="sxs-lookup"><span data-stu-id="81763-141">On hello services settings page, at hello bottom of hello screen click **Go toohello portal**.</span></span> <span data-ttu-id="81763-142">Megnyílik egy új lap.</span><span class="sxs-lookup"><span data-stu-id="81763-142">A new page opens.</span></span>
   <span data-ttu-id="81763-143">![Letöltés](./media/multi-factor-authentication-sdk/download3a.png)</span><span class="sxs-lookup"><span data-stu-id="81763-143">![Download](./media/multi-factor-authentication-sdk/download3a.png)</span></span>
7. <span data-ttu-id="81763-144">A hello lap alján, bal oldali hello kattintson **SDK**.</span><span class="sxs-lookup"><span data-stu-id="81763-144">On hello left, at hello bottom, click **SDK**.</span></span>
8. <span data-ttu-id="81763-145">Válassza ki, majd kattintson egy hello letöltési hivatkozás hello nyelvét.</span><span class="sxs-lookup"><span data-stu-id="81763-145">Select hello language you want and click one hello associated download links.</span></span>
9. <span data-ttu-id="81763-146">Hello menteni.</span><span class="sxs-lookup"><span data-stu-id="81763-146">Save hello download.</span></span>

## <a name="whats-in-hello-sdk"></a><span data-ttu-id="81763-147">A hello SDK</span><span class="sxs-lookup"><span data-stu-id="81763-147">What's in hello SDK</span></span>
<span data-ttu-id="81763-148">hello SDK hello a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="81763-148">hello SDK includes hello following items:</span></span>

* <span data-ttu-id="81763-149">**INFORMÁCIÓS**.</span><span class="sxs-lookup"><span data-stu-id="81763-149">**README**.</span></span> <span data-ttu-id="81763-150">Ismerteti, hogyan toouse hello egy új vagy meglévő alkalmazáshoz a multi-factor Authentication API-k.</span><span class="sxs-lookup"><span data-stu-id="81763-150">Explains how toouse hello Multi-Factor Authentication APIs in a new or existing application.</span></span>
* <span data-ttu-id="81763-151">**Forrásfájlokat** a többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="81763-151">**Source files** for Multi-Factor Authentication</span></span>
* <span data-ttu-id="81763-152">**Ügyféltanúsítvány** toocommunicate használ multi-factor Authentication szolgáltatás hello</span><span class="sxs-lookup"><span data-stu-id="81763-152">**Client certificate** that you use toocommunicate with hello Multi-Factor Authentication service</span></span>
* <span data-ttu-id="81763-153">**Titkos kulcs** hello tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="81763-153">**Private key** for hello certificate</span></span>
* <span data-ttu-id="81763-154">**Hívás eredményeit.**</span><span class="sxs-lookup"><span data-stu-id="81763-154">**Call results.**</span></span> <span data-ttu-id="81763-155">Hívási eredménykódok listáját.</span><span class="sxs-lookup"><span data-stu-id="81763-155">A list of call result codes.</span></span> <span data-ttu-id="81763-156">tooopen ezt a fájlt, a szövegformázási, például a WordPad alkalmazás használjon.</span><span class="sxs-lookup"><span data-stu-id="81763-156">tooopen this file, use an application with text formatting, such as WordPad.</span></span> <span data-ttu-id="81763-157">Használjon hello hívás eredménye kódok tootest, és az alkalmazás hello multi-factor Authentication végrehajtása hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="81763-157">Use hello call result codes tootest and troubleshoot hello implementation of Multi-Factor Authentication in your application.</span></span> <span data-ttu-id="81763-158">Nincsenek hitelesítési állapotkódok.</span><span class="sxs-lookup"><span data-stu-id="81763-158">They are not authentication status codes.</span></span>
* <span data-ttu-id="81763-159">**Példák.**</span><span class="sxs-lookup"><span data-stu-id="81763-159">**Examples.**</span></span> <span data-ttu-id="81763-160">A multi-factor Authentication használatának megvalósítási mintakód.</span><span class="sxs-lookup"><span data-stu-id="81763-160">Sample code for a basic working implementation of Multi-Factor Authentication.</span></span>

> [!WARNING]
> <span data-ttu-id="81763-161">hello ügyféltanúsítvány egy egyedi személyes tanúsítvány kifejezetten jött létre.</span><span class="sxs-lookup"><span data-stu-id="81763-161">hello client certificate is a unique private certificate that was generated especially for you.</span></span> <span data-ttu-id="81763-162">Ne ossza, vagy elveszíti a fájl.</span><span class="sxs-lookup"><span data-stu-id="81763-162">Do not share or lose this file.</span></span> <span data-ttu-id="81763-163">A kulcs tooensuring hello biztonsági hello multi-factor Authentication szolgáltatással folytatott kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="81763-163">It’s your key tooensuring hello security of your communications with hello Multi-Factor Authentication service.</span></span>

## <a name="code-sample"></a><span data-ttu-id="81763-164">Kódminta</span><span class="sxs-lookup"><span data-stu-id="81763-164">Code sample</span></span>
<span data-ttu-id="81763-165">A mintakód bemutatja, hogyan toouse hello API-k a hello Azure multi-factor Authentication SDK tooadd szabványos mód hang hívás ellenőrzési tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="81763-165">This code sample shows you how toouse hello APIs in hello Azure Multi-Factor Authentication SDK tooadd standard mode voice call verification tooyour application.</span></span> <span data-ttu-id="81763-166">Szabványos módban a telefonhívások, amely a felhasználó megválaszolja tooby hello # billentyű lenyomásával hello.</span><span class="sxs-lookup"><span data-stu-id="81763-166">Standard mode is a telephone call that hello user responds tooby pressing hello # key.</span></span>

<span data-ttu-id="81763-167">Ez a példa egy egyszerű ASP.NET-alkalmazást a C# .NET 2.0 multi-factor Authentication SDK hello C# kiszolgálóoldali logikával, de hello folyamat hasonlít más nyelveken.</span><span class="sxs-lookup"><span data-stu-id="81763-167">This example uses hello C# .NET 2.0 Multi-Factor Authentication SDK in a basic ASP.NET application with C# server-side logic, but hello process is similar in other languages.</span></span> <span data-ttu-id="81763-168">Hello SDK tartalmaz forrásfájlokat, nem végrehajtható fájlok, mert hello fájlok létrehozása és azok hivatkozik, vagy azokat felvehetik az alkalmazás közvetlenül az.</span><span class="sxs-lookup"><span data-stu-id="81763-168">Because hello SDK includes source files, not executable files, you can build hello files and reference them or include them directly in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="81763-169">A multi-factor Authentication végrehajtásakor módszerekkel hello további (telefonhívást vagy SMS-üzenet) másodlagos vagy harmadlagos ellenőrzési toosupplement, az elsődleges hitelesítési módszert (felhasználónév és jelszó).</span><span class="sxs-lookup"><span data-stu-id="81763-169">When implementing Multi-Factor Authentication, use hello additional methods (phone call or text message) as secondary or tertiary verification toosupplement your primary authentication method (username and password).</span></span> <span data-ttu-id="81763-170">Ezek a módszerek nem elsődleges hitelesítési módszerek tervezték.</span><span class="sxs-lookup"><span data-stu-id="81763-170">These methods are not designed as primary authentication methods.</span></span>

### <a name="code-sample-overview"></a><span data-ttu-id="81763-171">Kód a minta áttekintése</span><span class="sxs-lookup"><span data-stu-id="81763-171">Code Sample Overview</span></span>
<span data-ttu-id="81763-172">A mintakód egyszerű bemutató webalkalmazás használ a telefonhívások egy # kulcs válasz tooverify hello felhasználói hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="81763-172">This sample code for a simple web demo application uses a telephone call with a # key response tooverify hello user's authentication.</span></span> <span data-ttu-id="81763-173">A telefonhívás tényező szabványos mód néven a multi-factor Authentication szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="81763-173">This telephone call factor is known in Multi-Factor Authentication as standard mode.</span></span>

<span data-ttu-id="81763-174">hello ügyféloldali kódot nem tartalmazza a multi-factor Authentication-specifikus elemeket.</span><span class="sxs-lookup"><span data-stu-id="81763-174">hello client-side code does not include any Multi-Factor Authentication-specific elements.</span></span> <span data-ttu-id="81763-175">Mert hello további hitelesítési tényezők független hello elsődleges hitelesítéshez, adja hozzá őket hello meglévő bejelentkezés felület módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="81763-175">Because hello additional authentication factors are independent of hello primary authentication, you can add them without changing hello existing sign-on interface.</span></span> <span data-ttu-id="81763-176">a hello multi-factor Authentication SDK API-k hello segítségével testre szabható a hello felhasználói felületet, de előfordulhat, hogy nem kell toochange semmit egyáltalán.</span><span class="sxs-lookup"><span data-stu-id="81763-176">hello APIs in hello Multi-Factor SDK let you customize hello user experience, but you might not need toochange anything at all.</span></span>

<span data-ttu-id="81763-177">hello kiszolgálóoldali kód hozzáadása a normál módú hitelesítést 2. lépésben.</span><span class="sxs-lookup"><span data-stu-id="81763-177">hello server-side code adds standard-mode authentication in Step 2.</span></span> <span data-ttu-id="81763-178">Létrehoz egy PfAuthParams objektum normál módú ellenőrzéshez szükséges hello paraméterekkel: felhasználónév, telefon számát, valamint módot, és hello elérési toohello ügyféltanúsítvány (CertFilePath), amelyre azonban szükség van minden egyes hívásban.</span><span class="sxs-lookup"><span data-stu-id="81763-178">It creates a PfAuthParams object with hello parameters that are required for standard-mode verification: username, telephone number, and mode, and hello path toohello client certificate (CertFilePath), which is required in each call.</span></span> <span data-ttu-id="81763-179">A minden paraméter PfAuthParams bemutatója, lásd: hello SDK hello példa fájlt.</span><span class="sxs-lookup"><span data-stu-id="81763-179">For a demonstration of all parameters in PfAuthParams, see hello Example file in hello SDK.</span></span>

<span data-ttu-id="81763-180">A következő hello kód hello PfAuthParams objektum toohello pf_authenticate() függvény adja át.</span><span class="sxs-lookup"><span data-stu-id="81763-180">Next, hello code passes hello PfAuthParams object toohello pf_authenticate() function.</span></span> <span data-ttu-id="81763-181">hello visszatérési érték hello sikerességét vagy sikertelenségét hello hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="81763-181">hello return value indicates hello success or failure of hello authentication.</span></span> <span data-ttu-id="81763-182">Kimenő paraméterek callStatus és errorID hello, további hívás eredménye információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="81763-182">hello out parameters, callStatus, and errorID, contain additional call result information.</span></span> <span data-ttu-id="81763-183">hello hívási eredménykódok hello hívás eredményfájl hello SDK szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="81763-183">hello call result codes are documented in hello call results file in hello SDK.</span></span>

<span data-ttu-id="81763-184">A minimális végrehajtása néhány sor is beírhatók.</span><span class="sxs-lookup"><span data-stu-id="81763-184">This minimal implementation can be written in a few lines.</span></span> <span data-ttu-id="81763-185">Azonban az éles kódban, hogy tartalmazhat kifinomultabb hibakezelés, az adatbázis további kódot és jobb felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="81763-185">However, in production code, you would include more sophisticated error handling, additional database code, and an enhanced user experience.</span></span>

### <a name="web-client-code"></a><span data-ttu-id="81763-186">Webalkalmazás ügyféloldali kódot</span><span class="sxs-lookup"><span data-stu-id="81763-186">Web Client Code</span></span>
<span data-ttu-id="81763-187">hello webes Ügyfélkód bemutató lapon látható.</span><span class="sxs-lookup"><span data-stu-id="81763-187">hello following is web client code for a demo page.</span></span>

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


### <a name="server-side-code"></a><span data-ttu-id="81763-188">Kiszolgálóoldali kódolás</span><span class="sxs-lookup"><span data-stu-id="81763-188">Server-Side Code</span></span>
<span data-ttu-id="81763-189">Hello kiszolgálóoldali kód a következő, a multi-factor Authentication konfigurálva, és futtassa a 2.</span><span class="sxs-lookup"><span data-stu-id="81763-189">In hello following server-side code, Multi-Factor Authentication is configured and run in Step 2.</span></span> <span data-ttu-id="81763-190">Szabványos mód (MODE_STANDARD), a telefonhívás toowhich hello felhasználó válaszol hello # billentyű lenyomása mellett.</span><span class="sxs-lookup"><span data-stu-id="81763-190">Standard mode (MODE_STANDARD) is a telephone call toowhich hello user responds by pressing hello # key.</span></span>

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
