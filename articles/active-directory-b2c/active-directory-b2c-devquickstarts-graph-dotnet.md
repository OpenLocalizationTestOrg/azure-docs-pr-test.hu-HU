---
title: "Az Azure Active Directory B2C: Használata hello Graph API-val |} Microsoft Docs"
description: "Hogyan toocall hello Graph API a B2C-bérlő az alkalmazás identitását tooautomate hello folyamat használatával."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f9904516-d9f7-43b1-ae4f-e4d9eb1c67a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: parakhj
ms.openlocfilehash: a17cdc4adf57dbf22592d99ef8ecde41652763fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-use-hello-graph-api"></a><span data-ttu-id="3596e-103">Az Azure AD B2C: Hello Graph API használata</span><span class="sxs-lookup"><span data-stu-id="3596e-103">Azure AD B2C: Use hello Graph API</span></span>
<span data-ttu-id="3596e-104">Az Azure Active Directory (Azure AD) B2C-bérlők általában nagyon nagy toobe.</span><span class="sxs-lookup"><span data-stu-id="3596e-104">Azure Active Directory (Azure AD) B2C tenants tend toobe very large.</span></span> <span data-ttu-id="3596e-105">Ez azt jelenti, hogy a közös bérlői felügyeleti feladatok elvégzését toobe kell.</span><span class="sxs-lookup"><span data-stu-id="3596e-105">This means that many common tenant management tasks need toobe performed programmatically.</span></span> <span data-ttu-id="3596e-106">Egy elsődleges példája felhasználók kezelése.</span><span class="sxs-lookup"><span data-stu-id="3596e-106">A primary example is user management.</span></span> <span data-ttu-id="3596e-107">Szükség lehet egy meglévő felhasználó tároló tooa B2C-bérlő toomigrate.</span><span class="sxs-lookup"><span data-stu-id="3596e-107">You might need toomigrate an existing user store tooa B2C tenant.</span></span> <span data-ttu-id="3596e-108">Előfordulhat, hogy toohost felhasználói regisztráció szeretné, hogy a saját oldalon, és hozzon létre felhasználói fiókokat a hello háttérben Azure AD B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="3596e-108">You may want toohost user registration on your own page and create user accounts in your Azure AD B2C directory behind hello scenes.</span></span> <span data-ttu-id="3596e-109">Ilyen típusú feladatok hello képességét toocreate, olvasási, frissítési igényelnek, és törölje a felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="3596e-109">These types of tasks require hello ability toocreate, read, update, and delete user accounts.</span></span> <span data-ttu-id="3596e-110">Ezek a feladatok hello Azure AD Graph API használatával teheti meg.</span><span class="sxs-lookup"><span data-stu-id="3596e-110">You can do these tasks by using hello Azure AD Graph API.</span></span>

<span data-ttu-id="3596e-111">A B2C bérlőre nincsenek két elsődleges módja hello Graph API-val folytatott kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="3596e-111">For B2C tenants, there are two primary modes of communicating with hello Graph API.</span></span>

* <span data-ttu-id="3596e-112">Interaktív, egyszer futó feladatok meg kell összekötőként hello B2C-bérlő a rendszergazdai fiók hello feladatok végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="3596e-112">For interactive, run-once tasks, you should act as an administrator account in hello B2C tenant when you perform hello tasks.</span></span> <span data-ttu-id="3596e-113">Ebben a módban van szükség egy rendszergazda toosign hitelesítő adataival, hogy a rendszergazda minden hívások toohello Graph API végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="3596e-113">This mode requires an administrator toosign in with credentials before that admin can perform any calls toohello Graph API.</span></span>
* <span data-ttu-id="3596e-114">Automatikus, folyamatos feladatokat érdemes használni a valamilyen Ön által biztosított szolgáltatásfiók hello megfelelő jogokkal tooperform felügyeleti feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="3596e-114">For automated, continuous tasks, you should use some type of service account that you provide with hello necessary privileges tooperform management tasks.</span></span> <span data-ttu-id="3596e-115">Az Azure AD meg ehhez az alkalmazásnak, és tooAzure AD hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="3596e-115">In Azure AD, you can do this by registering an application and authenticating tooAzure AD.</span></span> <span data-ttu-id="3596e-116">Ehhez használja az **Alkalmazásazonosító** hello használó [OAuth 2.0 ügyfél hitelesítő adatai megadják](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="3596e-116">This is done by using an **Application ID** that uses hello [OAuth 2.0 client credentials grant](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span></span> <span data-ttu-id="3596e-117">Ebben az esetben hello alkalmazás úgy működik, mint maga nem felhasználóként, toocall hello Graph API-val.</span><span class="sxs-lookup"><span data-stu-id="3596e-117">In this case, hello application acts as itself, not as a user, toocall hello Graph API.</span></span>

<span data-ttu-id="3596e-118">Ez a cikk mutatjuk be, hogyan tooperform hello automatikus használati eset.</span><span class="sxs-lookup"><span data-stu-id="3596e-118">In this article, we'll discuss how tooperform hello automated-use case.</span></span> <span data-ttu-id="3596e-119">toodemonstrate, azt fogja összeállítása a .NET 4.5 `B2CGraphClient` , amely elvégzi a felhasználó létrehozása, olvasása, frissítése és Törlés (CRUD) típusú műveletek.</span><span class="sxs-lookup"><span data-stu-id="3596e-119">toodemonstrate, we'll build a .NET 4.5 `B2CGraphClient` that performs user create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="3596e-120">hello ügyfél egy Windows-parancssori felület (CLI), amely lehetővé teszi tooinvoke lesz a különböző módszereket.</span><span class="sxs-lookup"><span data-stu-id="3596e-120">hello client will have a Windows command-line interface (CLI) that allows you tooinvoke various methods.</span></span> <span data-ttu-id="3596e-121">Azonban hello kód írása toobehave nem interaktív, automatikus módon.</span><span class="sxs-lookup"><span data-stu-id="3596e-121">However, hello code is written toobehave in a noninteractive, automated fashion.</span></span>

## <a name="get-an-azure-ad-b2c-tenant"></a><span data-ttu-id="3596e-122">Az Azure AD B2C-bérlő beszerzése</span><span class="sxs-lookup"><span data-stu-id="3596e-122">Get an Azure AD B2C tenant</span></span>
<span data-ttu-id="3596e-123">Előtt hozzon létre az alkalmazások vagy felhasználók számára, vagy minden kommunikálni az Azure AD, szüksége lesz egy Azure AD B2C-bérlő és egy globális rendszergazdai fiók hello-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="3596e-123">Before you can create applications or users, or interact with Azure AD at all, you will need an Azure AD B2C tenant and a global administrator account in hello tenant.</span></span> <span data-ttu-id="3596e-124">Ha már, a bérlő nincs [Ismerkedés az Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3596e-124">If you don't have a tenant already, [get started with Azure AD B2C](active-directory-b2c-get-started.md).</span></span>

## <a name="register-your-application-in-your-tenant"></a><span data-ttu-id="3596e-125">Az alkalmazás regisztrálása-bérlőben</span><span class="sxs-lookup"><span data-stu-id="3596e-125">Register your application in your tenant</span></span>
<span data-ttu-id="3596e-126">Miután a B2C-bérlő, kell tooregister hello keresztül az alkalmazás [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3596e-126">After you have a B2C tenant, you need tooregister your application via hello [Azure Portal](https://portal.azure.com).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3596e-127">toouse hello Graph API-t a B2C bérlőre, szüksége lesz egy dedikált alkalmazás tooregister hello általános használatával *App regisztrációk* panel az Azure-portálon hello **nem** az Azure AD B2C  *Alkalmazások* panelen.</span><span class="sxs-lookup"><span data-stu-id="3596e-127">toouse hello Graph API with your B2C tenant, you will need tooregister a dedicated application by using hello generic *App Registrations* blade in hello Azure Portal, **NOT** Azure AD B2C's *Applications* blade.</span></span> <span data-ttu-id="3596e-128">Nem használhat újra hello már meglévő B2C alkalmazások az Azure AD B2C hello regisztrált *alkalmazások* panelen.</span><span class="sxs-lookup"><span data-stu-id="3596e-128">You can't reuse hello already-existing B2C applications that you registered in hello Azure AD B2C's *Applications* blade.</span></span>

1. <span data-ttu-id="3596e-129">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3596e-129">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3596e-130">Válassza ki az Azure AD B2C-bérlő fiókja hello jobb felső sarkában hello lap választásával.</span><span class="sxs-lookup"><span data-stu-id="3596e-130">Choose your Azure AD B2C tenant by selecting your account in hello top right corner of hello page.</span></span>
3. <span data-ttu-id="3596e-131">Hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3596e-131">In hello left-hand navigation pane, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
4. <span data-ttu-id="3596e-132">Hello utasításokat követve, és hozzon létre egy új alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3596e-132">Follow hello prompts and create a new application.</span></span> 
    1. <span data-ttu-id="3596e-133">Válassza ki **Web App / API** , hello alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="3596e-133">Select **Web App / API** as hello Application Type.</span></span>    
    2. <span data-ttu-id="3596e-134">Adjon meg **bármely átirányítási URI** (pl. https://B2CGraphAPI), mert nincs megfelelő ehhez a példához.</span><span class="sxs-lookup"><span data-stu-id="3596e-134">Provide **any redirect URI** (e.g. https://B2CGraphAPI) as it's not relevant for this example.</span></span>  
5. <span data-ttu-id="3596e-135">hello alkalmazás lesz most megjelenjenek az alkalmazások, hello listájában kattintson rá tooobtain hello **Alkalmazásazonosító** (más néven Ügyfélazonosítót).</span><span class="sxs-lookup"><span data-stu-id="3596e-135">hello application will now show up in hello list of applications, click on it tooobtain hello **Application ID** (also known as Client ID).</span></span> <span data-ttu-id="3596e-136">Másolja, szüksége lehet rájuk egy későbbi szakasz ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3596e-136">Copy it as you'll need it in a later section.</span></span>
6. <span data-ttu-id="3596e-137">Hello-beállítások panelen kattintson a **kulcsok** , és adja hozzá egy új kulcsot (más néven ügyfélkulcs).</span><span class="sxs-lookup"><span data-stu-id="3596e-137">In hello Settings blade, click on **Keys** and add a new key (also known as client secret).</span></span> <span data-ttu-id="3596e-138">Is másolja azt egy későbbi szakasz ismerteti a használatra.</span><span class="sxs-lookup"><span data-stu-id="3596e-138">Also copy it for use in a later section.</span></span>

## <a name="configure-create-read-and-update-permissions-for-your-application"></a><span data-ttu-id="3596e-139">Konfigurálása létrehozása, olvasása, és az alkalmazás engedélyeinek frissítése</span><span class="sxs-lookup"><span data-stu-id="3596e-139">Configure create, read and update permissions for your application</span></span>
<span data-ttu-id="3596e-140">Most tooconfigure az összes hello alkalmazás tooget szükséges engedélyek toocreate, olvasása, frissítése, és törli a felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="3596e-140">Now you need tooconfigure your application tooget all hello required permissions toocreate, read, update and delete users.</span></span>

1. <span data-ttu-id="3596e-141">A Folytatás hello Azure portal alkalmazás regisztrációk panelen, jelölje be az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3596e-141">Continuing in hello Azure portal's App Registrations blade, select your application.</span></span>
2. <span data-ttu-id="3596e-142">Hello-beállítások panelen kattintson a **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="3596e-142">In hello Settings blade, click on **Required permissions**.</span></span>
3. <span data-ttu-id="3596e-143">Hello szükséges engedélyek panelen kattintson a **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3596e-143">In hello Required permissions blade, click on **Windows Azure Active Directory**.</span></span>
4. <span data-ttu-id="3596e-144">Hello hozzáférés engedélyezése a panelen, jelölje ki a hello **címtáradatok olvasása és írása** engedélyt **Alkalmazásengedélyek** kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="3596e-144">In hello Enable Access  blade, select hello **Read and write directory data** permission from **Application Permissions** and click **Save**.</span></span>
5. <span data-ttu-id="3596e-145">Végezetül vissza hello szükséges engedélyek panelen kattintson a hello **engedélyt adjon** gombra.</span><span class="sxs-lookup"><span data-stu-id="3596e-145">Finally, back in hello Required permissions blade, click on hello **Grant Permissions** button.</span></span>

<span data-ttu-id="3596e-146">Most már rendelkezik egy alkalmazás, amely rendelkezik engedéllyel toocreate, olvassa el és frissítés felhasználóit a B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="3596e-146">You now have an application that has permission toocreate, read and update users from your B2C tenant.</span></span>

## <a name="configure-delete-permissions-for-your-application"></a><span data-ttu-id="3596e-147">Az alkalmazás törlése engedélyek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3596e-147">Configure delete permissions for your application</span></span>
<span data-ttu-id="3596e-148">Jelenleg hello *címtáradatok olvasása és írása* engedély does **nem** közé tartozik a hello képességét toodo bármely törlések, például a felhasználók törlése.</span><span class="sxs-lookup"><span data-stu-id="3596e-148">Currently, hello *Read and write directory data* permission does **NOT** include hello ability toodo any deletions such as deleting users.</span></span> <span data-ttu-id="3596e-149">Ha toogive az alkalmazás hello képességét toodelete felhasználóinak, toodo PowerShell érintő további lépések lesz szüksége, ellenkező esetben kihagyhatja toohello következő szakaszt.</span><span class="sxs-lookup"><span data-stu-id="3596e-149">If you want toogive your application hello ability toodelete users, you'll need toodo these extra steps that involve PowerShell, otherwise, you can skip toohello next section.</span></span>

<span data-ttu-id="3596e-150">Először töltse le és telepítse hello [Microsoft Online Services bejelentkezési segéd](http://go.microsoft.com/fwlink/?LinkID=286152).</span><span class="sxs-lookup"><span data-stu-id="3596e-150">First, download and install hello [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).</span></span> <span data-ttu-id="3596e-151">Majd letöltéséhez és telepítéséhez hello [64 bites Azure Active Directory-modul Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span><span class="sxs-lookup"><span data-stu-id="3596e-151">Then download and install hello [64-bit Azure Active Directory module for Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span></span>

<span data-ttu-id="3596e-152">Hello PowerShell-modul telepítése után nyissa meg a Powershellt, és csatlakozzon a tooyour B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="3596e-152">After you install hello PowerShell module, open PowerShell and connect tooyour B2C tenant.</span></span> <span data-ttu-id="3596e-153">Futtatása után `Get-Credential`, akkor a rendszer bekéri a felhasználói nevet és jelszót, Enter hello felhasználónév és a B2C bérlő rendszergazdai fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="3596e-153">After you run `Get-Credential`, you will be prompted for a user name and password, Enter hello user name and password of your B2C tenant administrator account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3596e-154">A B2C toouse bérlői rendszergazdai fiókot, amely kell **helyi** toohello B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="3596e-154">You need toouse a B2C tenant administrator account that is **local** toohello B2C tenant.</span></span> <span data-ttu-id="3596e-155">Ezek a fiókok néznek ki: myusername@myb2ctenant.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="3596e-155">These accounts look like this: myusername@myb2ctenant.onmicrosoft.com.</span></span>

```powershell
Connect-MsolService
```

<span data-ttu-id="3596e-156">Most hello fogjuk használni **Alkalmazásazonosító** hello parancsfájlban tooassign hello alkalmazás hello felhasználói fiók rendszergazdai szerepkör, amely lehetővé teszi az toodelete felhasználók alatt.</span><span class="sxs-lookup"><span data-stu-id="3596e-156">Now we'll use hello **Application ID** in hello script below tooassign hello application hello user account administrator role which will allow it toodelete users.</span></span> <span data-ttu-id="3596e-157">Ezek a szerepkörök jól ismert azonosítók rendelkezik, így a toodo szüksége van bemeneti a **Alkalmazásazonosító** hello parancsfájlban az alábbi.</span><span class="sxs-lookup"><span data-stu-id="3596e-157">These roles have well-known identifiers, so all you need toodo is input your **Application ID** in hello script below.</span></span>

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

<span data-ttu-id="3596e-158">Az alkalmazás most már rendelkezik a B2C-bérlő felhasználóit engedélyek toodelete is.</span><span class="sxs-lookup"><span data-stu-id="3596e-158">Your application now also has permissions toodelete users from your B2C tenant.</span></span>

## <a name="download-configure-and-build-hello-sample-code"></a><span data-ttu-id="3596e-159">Töltse le, konfigurálása, és állítsa be hello mintakód</span><span class="sxs-lookup"><span data-stu-id="3596e-159">Download, configure, and build hello sample code</span></span>
<span data-ttu-id="3596e-160">Először hello mintakód letöltése, és lekérése is fusson.</span><span class="sxs-lookup"><span data-stu-id="3596e-160">First, download hello sample code and get it running.</span></span> <span data-ttu-id="3596e-161">Majd most elindítjuk azt részletes bemutatása.</span><span class="sxs-lookup"><span data-stu-id="3596e-161">Then we will take a closer look at it.</span></span>  <span data-ttu-id="3596e-162">Is [.zip-fájlként hello mintakód letöltése](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="3596e-162">You can [download hello sample code as a .zip file](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span></span> <span data-ttu-id="3596e-163">Akkor is klónozhatja egy olyan könyvtárba, az Ön által választott:</span><span class="sxs-lookup"><span data-stu-id="3596e-163">You can also clone it into a directory of your choice:</span></span>

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

<span data-ttu-id="3596e-164">Nyissa meg hello `B2CGraphClient\B2CGraphClient.sln` Visual Studio-megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="3596e-164">Open hello `B2CGraphClient\B2CGraphClient.sln` Visual Studio solution in Visual Studio.</span></span> <span data-ttu-id="3596e-165">A hello `B2CGraphClient` projektet, nyissa meg hello fájl `App.config`.</span><span class="sxs-lookup"><span data-stu-id="3596e-165">In hello `B2CGraphClient` project, open hello file `App.config`.</span></span> <span data-ttu-id="3596e-166">Hello három alkalmazásbeállítást cserélje le a saját értékeit:</span><span class="sxs-lookup"><span data-stu-id="3596e-166">Replace hello three app settings with your own values:</span></span>

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{hello ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{hello Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="3596e-167">Kattintson a jobb gombbal a hello `B2CGraphClient` megoldás és a rebuild hello minta.</span><span class="sxs-lookup"><span data-stu-id="3596e-167">Next, right-click on hello `B2CGraphClient` solution and rebuild hello sample.</span></span> <span data-ttu-id="3596e-168">Ha sikeres, most rendelkeznie kell egy `B2C.exe` található végrehajtható fájl `B2CGraphClient\bin\Debug`.</span><span class="sxs-lookup"><span data-stu-id="3596e-168">If you are successful, you should now have a `B2C.exe` executable file located in `B2CGraphClient\bin\Debug`.</span></span>

## <a name="build-user-crud-operations-by-using-hello-graph-api"></a><span data-ttu-id="3596e-169">Felhasználói CRUD műveletek build hello Graph API használatával</span><span class="sxs-lookup"><span data-stu-id="3596e-169">Build user CRUD operations by using hello Graph API</span></span>
<span data-ttu-id="3596e-170">toouse hello B2CGraphClient, nyisson meg egy `cmd` Windows parancsot a parancssorba, és módosítsa a könyvtárat toohello `Debug` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3596e-170">toouse hello B2CGraphClient, open a `cmd` Windows command prompt and change your directory toohello `Debug` directory.</span></span> <span data-ttu-id="3596e-171">Futtassa a hello `B2C Help` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3596e-171">Then run hello `B2C Help` command.</span></span>

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

<span data-ttu-id="3596e-172">Ez megjeleníti minden egyes parancsnál rövid leírása.</span><span class="sxs-lookup"><span data-stu-id="3596e-172">This will display a brief description of each command.</span></span> <span data-ttu-id="3596e-173">Minden alkalommal, amikor aktiválják az alábbi parancsok egyikét `B2CGraphClient` egy kérelem toohello Azure AD Graph API segítségével.</span><span class="sxs-lookup"><span data-stu-id="3596e-173">Each time you invoke one of these commands, `B2CGraphClient` makes a request toohello Azure AD Graph API.</span></span>

### <a name="get-an-access-token"></a><span data-ttu-id="3596e-174">Hozzáférési jogkivonat lekérése</span><span class="sxs-lookup"><span data-stu-id="3596e-174">Get an access token</span></span>
<span data-ttu-id="3596e-175">A kérelem toohello Graph API hitelesítési olyan hozzáférési jogkivonatot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3596e-175">Any request toohello Graph API requires an access token for authentication.</span></span> <span data-ttu-id="3596e-176">`B2CGraphClient`felhasználási hello nyílt forráskódú Active Directory Authentication Library (ADAL) toohelp jogkivonatot szerezni.</span><span class="sxs-lookup"><span data-stu-id="3596e-176">`B2CGraphClient` uses hello open-source Active Directory Authentication Library (ADAL) toohelp acquire access tokens.</span></span> <span data-ttu-id="3596e-177">ADAL egyszerűbbé teszi a token beszerzési biztosító egyszerű API-t, és néhány fontos részleteket, például a gyorsítótár hozzáférési jogkivonatok figyelembe vételével.</span><span class="sxs-lookup"><span data-stu-id="3596e-177">ADAL makes token acquisition easier by providing a simple API and taking care of some important details, such as caching access tokens.</span></span> <span data-ttu-id="3596e-178">Toouse ADAL tooget jogkivonatokat, azonban nincs.</span><span class="sxs-lookup"><span data-stu-id="3596e-178">You don't have toouse ADAL tooget tokens, though.</span></span> <span data-ttu-id="3596e-179">Jogkivonatok HTTP-kérelmek létrehozásával is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="3596e-179">You can also get tokens by crafting HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="3596e-180">A fenti sorrendben toocommunicate hello Graph API-val rendelkező ADAL v2 használja.</span><span class="sxs-lookup"><span data-stu-id="3596e-180">This code sample uses ADAL v2 in order toocommunicate with hello Graph API.</span></span>  <span data-ttu-id="3596e-181">Az order tooget hozzáférési jogkivonatok használható az Azure AD Graph API hello ADAL v2 és v3 kell használnia.</span><span class="sxs-lookup"><span data-stu-id="3596e-181">You must use ADAL v2 or v3 in order tooget access tokens which can be used with hello Azure AD Graph API.</span></span>
> 
> 

<span data-ttu-id="3596e-182">Ha `B2CGraphClient` fut, létrehoz egy példánya hello `B2CGraphClient` osztály.</span><span class="sxs-lookup"><span data-stu-id="3596e-182">When `B2CGraphClient` runs, it creates an instance of hello `B2CGraphClient` class.</span></span> <span data-ttu-id="3596e-183">Ez az osztály konstruktorában hello beállítása az ADAL-hitelesítés állványok:</span><span class="sxs-lookup"><span data-stu-id="3596e-183">hello constructor for this class sets up an ADAL authentication scaffolding:</span></span>

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // hello client_id, client_secret, and tenant are provided in Program.cs, which pulls hello values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // hello AuthenticationContext is ADAL's primary class, in which you indicate hello tenant toouse.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // hello ClientCredential is where you pass in your client_id and client_secret, which are
    // provided tooAzure AD in order tooreceive an access_token by using hello app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

<span data-ttu-id="3596e-184">Hello fogjuk használni `B2C Get-User` példaként parancsot.</span><span class="sxs-lookup"><span data-stu-id="3596e-184">We'll use hello `B2C Get-User` command as an example.</span></span> <span data-ttu-id="3596e-185">Ha `B2C Get-User` nélkül semmilyen további bemenetek, hello CLI hívások hello meghívták `B2CGraphClient.GetAllUsers(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="3596e-185">When `B2C Get-User` is invoked without any additional inputs, hello CLI calls hello `B2CGraphClient.GetAllUsers(...)` method.</span></span> <span data-ttu-id="3596e-186">Ez a metódus meghívja `B2CGraphClient.SendGraphGetRequest(...)`, amely elküld egy HTTP GET kérést toohello Graph API-val.</span><span class="sxs-lookup"><span data-stu-id="3596e-186">This method calls `B2CGraphClient.SendGraphGetRequest(...)`, which submits an HTTP GET request toohello Graph API.</span></span> <span data-ttu-id="3596e-187">Mielőtt `B2CGraphClient.SendGraphGetRequest(...)` küld hello GET kérés, először nyer access token ADAL használatával:</span><span class="sxs-lookup"><span data-stu-id="3596e-187">Before `B2CGraphClient.SendGraphGetRequest(...)` sends hello GET request, it first gets an access token by using ADAL:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL tooacquire a token by using hello app's identity (hello credential)
    // hello first parameter is hello resource we want an access_token for; in this case, hello Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

<span data-ttu-id="3596e-188">Kaphat access token hello Graph API-t hívó hello ADAL által `AuthenticationContext.AcquireToken(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="3596e-188">You can get an access token for hello Graph API by calling hello ADAL `AuthenticationContext.AcquireToken(...)` method.</span></span> <span data-ttu-id="3596e-189">Adal-t adja vissza egy `access_token` , amely hello alkalmazás azonosítóját jelöli.</span><span class="sxs-lookup"><span data-stu-id="3596e-189">ADAL then returns an `access_token` that represents hello application's identity.</span></span>

### <a name="read-users"></a><span data-ttu-id="3596e-190">Olvassa el a felhasználók</span><span class="sxs-lookup"><span data-stu-id="3596e-190">Read users</span></span>
<span data-ttu-id="3596e-191">Ha szeretné, hogy tooget azoknak a felhasználóknak, vagy egy adott felhasználó beszerezni hello Graph API-val, HTTP küldhet `GET` toohello kérelem `/users` végpont.</span><span class="sxs-lookup"><span data-stu-id="3596e-191">When you want tooget a list of users or get a particular user from hello Graph API, you can send an HTTP `GET` request toohello `/users` endpoint.</span></span> <span data-ttu-id="3596e-192">Egy bérlő hello felhasználói kérelem így néz ki:</span><span class="sxs-lookup"><span data-stu-id="3596e-192">A request for all of hello users in a tenant looks like this:</span></span>

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="3596e-193">toosee a kérelem futtatásához:</span><span class="sxs-lookup"><span data-stu-id="3596e-193">toosee this request, run:</span></span>

 ```
 > B2C Get-User
 ```

<span data-ttu-id="3596e-194">Két fontos részek toonote van:</span><span class="sxs-lookup"><span data-stu-id="3596e-194">There are two important things toonote:</span></span>

* <span data-ttu-id="3596e-195">hello ADAL keresztül megszerzett jogkivonattal hozzáadott toohello `Authorization` hello segítségével fejléc `Bearer` séma.</span><span class="sxs-lookup"><span data-stu-id="3596e-195">hello access token acquired via ADAL is added toohello `Authorization` header by using hello `Bearer` scheme.</span></span>
* <span data-ttu-id="3596e-196">A B2C bérlőre, kell használnia hello lekérdezésparaméter `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="3596e-196">For B2C tenants, you must use hello query parameter `api-version=1.6`.</span></span>

<span data-ttu-id="3596e-197">Ezen adatok mindegyikét kezeli, hello `B2CGraphClient.SendGraphGetRequest(...)` módszert:</span><span class="sxs-lookup"><span data-stu-id="3596e-197">Both of these details are handled in hello `B2CGraphClient.SendGraphGetRequest(...)` method:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure toouse hello 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append hello access token for hello Graph API toohello Authorization header of hello request by using hello Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a><span data-ttu-id="3596e-198">Felhasználói fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="3596e-198">Create consumer user accounts</span></span>
<span data-ttu-id="3596e-199">A B2C-bérlő felhasználói fiókokat hoz létre, amikor HTTP küldhet `POST` toohello kérelem `/users` végpont:</span><span class="sxs-lookup"><span data-stu-id="3596e-199">When you create user accounts in your B2C tenant, you can send an HTTP `POST` request toohello `/users` endpoint:</span></span>

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required toocreate consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier hello user uses toosign in toohello account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set too'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying toohello end user
    "mailNickname": "joec",                        // an email alias for hello user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set toofalse
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

<span data-ttu-id="3596e-200">Ezeket a tulajdonságokat a kérésben többsége szükséges toocreate identitásrendszerében a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="3596e-200">Most of these properties in this request are required toocreate consumer users.</span></span> <span data-ttu-id="3596e-201">több, kattintson a toolearn [Itt](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span><span class="sxs-lookup"><span data-stu-id="3596e-201">toolearn more, click [here](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span></span> <span data-ttu-id="3596e-202">Vegye figyelembe, hogy hello `//` illusztrációs megjelent megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="3596e-202">Note that hello `//` comments have been included for illustration.</span></span> <span data-ttu-id="3596e-203">Ne szerepeljen az azokat a tényleges kérelmet.</span><span class="sxs-lookup"><span data-stu-id="3596e-203">Do not include them in an actual request.</span></span>

<span data-ttu-id="3596e-204">toosee hello kérelem, hello a következő parancsok egyikét futtatja:</span><span class="sxs-lookup"><span data-stu-id="3596e-204">toosee hello request, run one of hello following commands:</span></span>

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

<span data-ttu-id="3596e-205">Hello `Create-User` parancs fogadja bemeneti paraméterként egy .JSON kiterjesztésű fájlt.</span><span class="sxs-lookup"><span data-stu-id="3596e-205">hello `Create-User` command takes a .json file as an input parameter.</span></span> <span data-ttu-id="3596e-206">Ez tartalmazza a user objektum JSON-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3596e-206">This contains a JSON representation of a user object.</span></span> <span data-ttu-id="3596e-207">Nincsenek két minta .JSON kiterjesztésű fájlok hello mintakód: `usertemplate-email.json` és `usertemplate-username.json`.</span><span class="sxs-lookup"><span data-stu-id="3596e-207">There are two sample .json files in hello sample code: `usertemplate-email.json` and `usertemplate-username.json`.</span></span> <span data-ttu-id="3596e-208">Ezek a fájlok toosuit módosíthatja az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="3596e-208">You can modify these files toosuit your needs.</span></span> <span data-ttu-id="3596e-209">Ezenkívül toohello szükséges a fenti mezők, több választható mezőket, használható tartalmazza ezeket a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="3596e-209">In addition toohello required fields above, several optional fields that you can use are included in these files.</span></span> <span data-ttu-id="3596e-210">Hello opcionális mezők értékét a részletek megtalálhatók a hello [Azure AD Graph API entitáshivatkozás](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span><span class="sxs-lookup"><span data-stu-id="3596e-210">Details on hello optional fields can be found in hello [Azure AD Graph API entity reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span></span>

<span data-ttu-id="3596e-211">Láthatja, hogyan hello POST-kérelmet a összeállított `B2CGraphClient.SendGraphPostRequest(...)`.</span><span class="sxs-lookup"><span data-stu-id="3596e-211">You can see how hello POST request is constructed in `B2CGraphClient.SendGraphPostRequest(...)`.</span></span>

* <span data-ttu-id="3596e-212">Az access token toohello csatol `Authorization` hello kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="3596e-212">It attaches an access token toohello `Authorization` header of hello request.</span></span>
* <span data-ttu-id="3596e-213">Állítja a `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="3596e-213">It sets `api-version=1.6`.</span></span>
* <span data-ttu-id="3596e-214">Ez magában foglalja hello JSON felhasználói objektum hello kérelem hello törzsében.</span><span class="sxs-lookup"><span data-stu-id="3596e-214">It includes hello JSON user object in hello body of hello request.</span></span>

> [!NOTE]
> <span data-ttu-id="3596e-215">Ha hello fiókok kívánt toomigrate ki egy meglévő felhasználó rendelkezik mint hello alacsonyabb jelszó erőssége [erős jelszó erőssége kényszeríti ki az Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), letilthatja a hello erős jelszó megkövetelésének hello használatával`DisableStrongPassword`hello érték `passwordPolicies` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3596e-215">If hello accounts that you want toomigrate from an existing user store has lower password strength than hello [strong password strength enforced by Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), you can disable hello strong password requirement using hello `DisableStrongPassword` value in hello `passwordPolicies` property.</span></span> <span data-ttu-id="3596e-216">Például módosíthatja hello az alábbiak szerint fent megadott felhasználói kérelem létrehozása: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span><span class="sxs-lookup"><span data-stu-id="3596e-216">For instance, you can modify hello create user request provided above as follows: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span></span>
> 
> 

### <a name="update-consumer-user-accounts"></a><span data-ttu-id="3596e-217">Felhasználói fiókok frissítése</span><span class="sxs-lookup"><span data-stu-id="3596e-217">Update consumer user accounts</span></span>
<span data-ttu-id="3596e-218">Felhasználói objektumok frissítésekor hello érték. a fiókot használja, amelyet toocreate felhasználói objektumok hasonló toohello.</span><span class="sxs-lookup"><span data-stu-id="3596e-218">When you update user objects, hello process is similar toohello one you use toocreate user objects.</span></span> <span data-ttu-id="3596e-219">Ez a folyamat hello HTTP használja, de `PATCH` módszert:</span><span class="sxs-lookup"><span data-stu-id="3596e-219">But this process uses hello HTTP `PATCH` method:</span></span>

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only hello user's displayName
}
```

<span data-ttu-id="3596e-220">Próbálja meg a felhasználó tooupdate azáltal, hogy frissíti a JSON-fájlokat az új adatokat.</span><span class="sxs-lookup"><span data-stu-id="3596e-220">Try tooupdate a user by updating your JSON files with new data.</span></span> <span data-ttu-id="3596e-221">Ezután `B2CGraphClient` toorun, ezek a parancsok egyikét:</span><span class="sxs-lookup"><span data-stu-id="3596e-221">You can then use `B2CGraphClient` toorun one of these commands:</span></span>

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

<span data-ttu-id="3596e-222">Vizsgálja meg a hello `B2CGraphClient.SendGraphPatchRequest(...)` kapcsolatos részletes tudnivalókért metódus toosend ehhez a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="3596e-222">Inspect hello `B2CGraphClient.SendGraphPatchRequest(...)` method for details on how toosend this request.</span></span>

### <a name="search-users"></a><span data-ttu-id="3596e-223">Felhasználók keresése</span><span class="sxs-lookup"><span data-stu-id="3596e-223">Search users</span></span>
<span data-ttu-id="3596e-224">Felhasználók B2C-bérlőben lévő több módon is kereshet.</span><span class="sxs-lookup"><span data-stu-id="3596e-224">You can search for users in your B2C tenant in a couple of ways.</span></span> <span data-ttu-id="3596e-225">Egy hello felhasználói objektum azonosítója vagy két hello felhasználói bejelentkezési azonosítót használ (azaz hello `signInNames` tulajdonság).</span><span class="sxs-lookup"><span data-stu-id="3596e-225">One, using hello user's object ID or two, using hello user's sign-in identifer (i.e., hello `signInNames` property).</span></span>

<span data-ttu-id="3596e-226">Futtassa a következő parancsok toosearch egy adott felhasználó hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="3596e-226">Run one of hello following commands toosearch for a specific user:</span></span>

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

<span data-ttu-id="3596e-227">Íme néhány példa:</span><span class="sxs-lookup"><span data-stu-id="3596e-227">Here are a couple of examples:</span></span>

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a><span data-ttu-id="3596e-228">Felhasználók törlése</span><span class="sxs-lookup"><span data-stu-id="3596e-228">Delete users</span></span>
<span data-ttu-id="3596e-229">felhasználó törléséhez hello folyamat egyszerű.</span><span class="sxs-lookup"><span data-stu-id="3596e-229">hello process for deleting a user is straightforward.</span></span> <span data-ttu-id="3596e-230">Használjon hello HTTP `DELETE` metódus és szerkezet hello URL-cím elé hello javítsa ki az Objektumazonosító:</span><span class="sxs-lookup"><span data-stu-id="3596e-230">Use hello HTTP `DELETE` method and construct hello URL with hello correct object ID:</span></span>

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="3596e-231">toosee például adja meg a parancs és a nézet hello törlési kérelem, amely nyomtatott toohello konzol:</span><span class="sxs-lookup"><span data-stu-id="3596e-231">toosee an example, enter this command and view hello delete request that is printed toohello console:</span></span>

```
> B2C Delete-User <object-id-of-user>
```

<span data-ttu-id="3596e-232">Vizsgálja meg a hello `B2CGraphClient.SendGraphDeleteRequest(...)` kapcsolatos részletes tudnivalókért metódus toosend ehhez a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="3596e-232">Inspect hello `B2CGraphClient.SendGraphDeleteRequest(...)` method for details on how toosend this request.</span></span>

<span data-ttu-id="3596e-233">Hello Azure AD Graph API számos más műveletet toouser felügyeleti hozzáadását végezheti el.</span><span class="sxs-lookup"><span data-stu-id="3596e-233">You can perform many other actions with hello Azure AD Graph API in addition toouser management.</span></span> <span data-ttu-id="3596e-234">A [Azure AD Graph API-referencia](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) minden művelet együtt minta kérelmek részleteit.</span><span class="sxs-lookup"><span data-stu-id="3596e-234">The [Azure AD Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) provides details on each action, along with sample requests.</span></span>

## <a name="use-custom-attributes"></a><span data-ttu-id="3596e-235">Egyéni attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="3596e-235">Use custom attributes</span></span>
<span data-ttu-id="3596e-236">A legtöbb felhasználói alkalmazások kell toostore valamilyen egyéni felhasználói profillal kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="3596e-236">Most consumer applications need toostore some type of custom user profile information.</span></span> <span data-ttu-id="3596e-237">Ezt megteheti például toodefine B2C-bérlőben egyéni attribútumot.</span><span class="sxs-lookup"><span data-stu-id="3596e-237">One way you can do this is toodefine a custom attribute in your B2C tenant.</span></span> <span data-ttu-id="3596e-238">Ezután is kezelheti, hogy attribútum hello ugyanúgy más tulajdonságok úgy kezelje, a user objektum.</span><span class="sxs-lookup"><span data-stu-id="3596e-238">You can then treat that attribute hello same way you treat any other property on a user object.</span></span> <span data-ttu-id="3596e-239">Frissítheti az hello attribútum delete hello attribútum hello attribútum lekérdezés küldése hello attribútum bejelentkezési jogkivonatokat, és több jogcímként.</span><span class="sxs-lookup"><span data-stu-id="3596e-239">You can update hello attribute, delete hello attribute, query by hello attribute, send hello attribute as a claim in sign-in tokens, and more.</span></span>

<span data-ttu-id="3596e-240">a B2C bérlőre, az egyéni attribútum toodefine lásd: hello [B2C egyéni attribútumhivatkozás](active-directory-b2c-reference-custom-attr.md).</span><span class="sxs-lookup"><span data-stu-id="3596e-240">toodefine a custom attribute in your B2C tenant, see hello [B2C custom attribute reference](active-directory-b2c-reference-custom-attr.md).</span></span>

<span data-ttu-id="3596e-241">Megtekintheti a B2C-bérlő segítségével definiált egyéni attribútumok hello `B2CGraphClient`:</span><span class="sxs-lookup"><span data-stu-id="3596e-241">You can view hello custom attributes defined in your B2C tenant by using `B2CGraphClient`:</span></span>

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

<span data-ttu-id="3596e-242">hello kimeneti ezeket a funkciókat, mint felfedi hello összes egyéni attribútumot, részleteit:</span><span class="sxs-lookup"><span data-stu-id="3596e-242">hello output of these functions reveals hello details of each custom attribute, such as:</span></span>

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

<span data-ttu-id="3596e-243">Használhatja a teljes név, például hello `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, a felhasználói objektumok tulajdonságainál.</span><span class="sxs-lookup"><span data-stu-id="3596e-243">You can use hello full name, such as `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, as a property on your user objects.</span></span>  <span data-ttu-id="3596e-244">Frissítse a .JSON kiterjesztésű fájlt hello új tulajdonság és hello tulajdonság értékét, és futtassa:</span><span class="sxs-lookup"><span data-stu-id="3596e-244">Update your .json file with hello new property and a value for hello property, and then run:</span></span>

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

<span data-ttu-id="3596e-245">A `B2CGraphClient`, programozott módon kezelje a B2C bérlő felhasználók szolgáltatás-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="3596e-245">By using `B2CGraphClient`, you have a service application that can manage your B2C tenant users programmatically.</span></span> <span data-ttu-id="3596e-246">`B2CGraphClient`saját alkalmazás identitását tooauthenticate toohello Azure AD Graph API-t használ.</span><span class="sxs-lookup"><span data-stu-id="3596e-246">`B2CGraphClient` uses its own application identity tooauthenticate toohello Azure AD Graph API.</span></span> <span data-ttu-id="3596e-247">Egy ügyfélkulcsot a jogkivonatok is megkapja.</span><span class="sxs-lookup"><span data-stu-id="3596e-247">It also acquires tokens by using a client secret.</span></span> <span data-ttu-id="3596e-248">Mivel használhatja ezt a funkciót az alkalmazásba, ne felejtse néhány fő szempontot B2C-alkalmazásokhoz:</span><span class="sxs-lookup"><span data-stu-id="3596e-248">As you incorporate this functionality into your application, remember a few key points for B2C apps:</span></span>

* <span data-ttu-id="3596e-249">Toogrant hello alkalmazás hello megfelelő engedélyekkel a hello bérlői van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3596e-249">You need toogrant hello application hello proper permissions in hello tenant.</span></span>
* <span data-ttu-id="3596e-250">Most kell toouse adal-t (nem MSAL) tooget hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="3596e-250">For now, you need toouse ADAL (not MSAL) tooget access tokens.</span></span> <span data-ttu-id="3596e-251">(Ön is is küldhet közvetlen, szalagtár használata nélkül.)</span><span class="sxs-lookup"><span data-stu-id="3596e-251">(You can also send protocol messages directly, without using a library.)</span></span>
* <span data-ttu-id="3596e-252">A Graph API hello hívásakor használható `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="3596e-252">When you call hello Graph API, use `api-version=1.6`.</span></span>
* <span data-ttu-id="3596e-253">Amikor létrehozása és frissítése identitásrendszerében a felhasználók, néhány tulajdonságok szükségesek, fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="3596e-253">When you create and update consumer users, a few properties are required, as described above.</span></span>

<span data-ttu-id="3596e-254">Ha bármilyen kérdése vagy hello Graph API segítségével szeretné tooperform műveletek kéréseinek van a B2C-bérlő megjegyzést szóljon ebben a cikkben, vagy probléma fájlt hello GitHub kód a minta-tárházban.</span><span class="sxs-lookup"><span data-stu-id="3596e-254">If you have any questions or requests for actions you would like tooperform by using hello Graph API on your B2C tenant, leave a comment on this article or file an issue in hello GitHub code sample repository.</span></span>

