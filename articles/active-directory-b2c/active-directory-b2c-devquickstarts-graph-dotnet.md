---
title: "Az Azure Active Directory B2C: Használja a Graph API |} Microsoft Docs"
description: "Hogyan hívhatja meg a Graph API-val egy B2C-bérlő automatizálja az Alkalmazásidentitás használatával."
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
ms.openlocfilehash: c838fcad21875c4f813159ad78d4c87129a40a86
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-use-the-graph-api"></a><span data-ttu-id="26bfd-103">Az Azure AD B2C: Használja a Graph API</span><span class="sxs-lookup"><span data-stu-id="26bfd-103">Azure AD B2C: Use the Graph API</span></span>
<span data-ttu-id="26bfd-104">Az Azure Active Directory (Azure AD) B2C-bérlők általában nagyon nagy.</span><span class="sxs-lookup"><span data-stu-id="26bfd-104">Azure Active Directory (Azure AD) B2C tenants tend to be very large.</span></span> <span data-ttu-id="26bfd-105">Ez azt jelenti, hogy több bérlő felügyeletének általános feladatai van szükség, programozott módon.</span><span class="sxs-lookup"><span data-stu-id="26bfd-105">This means that many common tenant management tasks need to be performed programmatically.</span></span> <span data-ttu-id="26bfd-106">Egy elsődleges példája felhasználók kezelése.</span><span class="sxs-lookup"><span data-stu-id="26bfd-106">A primary example is user management.</span></span> <span data-ttu-id="26bfd-107">Szükség lehet egy meglévő felhasználó tároló áttelepítése a B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="26bfd-107">You might need to migrate an existing user store to a B2C tenant.</span></span> <span data-ttu-id="26bfd-108">Érdemes lehet a saját oldalon felhasználói regisztráció és a háttérben Azure AD B2C-címtárban lévő felhasználói fiókokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="26bfd-108">You may want to host user registration on your own page and create user accounts in your Azure AD B2C directory behind the scenes.</span></span> <span data-ttu-id="26bfd-109">Ilyen típusú feladatok elvégzéséhez szükség van a szolgáltatásvezérlési létrehozása, olvasása, frissítése, és törölje a felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="26bfd-109">These types of tasks require the ability to create, read, update, and delete user accounts.</span></span> <span data-ttu-id="26bfd-110">Ezeket a feladatokat az Azure AD Graph API használatával teheti meg.</span><span class="sxs-lookup"><span data-stu-id="26bfd-110">You can do these tasks by using the Azure AD Graph API.</span></span>

<span data-ttu-id="26bfd-111">A B2C bérlőre nincsenek két elsődleges módja a Graph API-val folytatott kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="26bfd-111">For B2C tenants, there are two primary modes of communicating with the Graph API.</span></span>

* <span data-ttu-id="26bfd-112">Interaktív, egyszer futó feladatok akkor szerepét a B2C-bérlő a rendszergazdai fiók a feladatok végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="26bfd-112">For interactive, run-once tasks, you should act as an administrator account in the B2C tenant when you perform the tasks.</span></span> <span data-ttu-id="26bfd-113">Ebben a módban a rendszergazda számára, hogy a hitelesítő adatokkal jelentkezhetnek be, hogy a rendszergazda a Graph API bármely hívások végrehajtása előtt szükséges.</span><span class="sxs-lookup"><span data-stu-id="26bfd-113">This mode requires an administrator to sign in with credentials before that admin can perform any calls to the Graph API.</span></span>
* <span data-ttu-id="26bfd-114">Automatikus, folyamatos feladatokat valamilyen adja meg a felügyeleti feladatok végrehajtásához szükséges jogosultságokkal rendelkező szolgáltatásfiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="26bfd-114">For automated, continuous tasks, you should use some type of service account that you provide with the necessary privileges to perform management tasks.</span></span> <span data-ttu-id="26bfd-115">Az Azure AD meg ehhez az alkalmazásnak, és az Azure AD hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="26bfd-115">In Azure AD, you can do this by registering an application and authenticating to Azure AD.</span></span> <span data-ttu-id="26bfd-116">Ehhez használja az **Alkalmazásazonosító** használó a [OAuth 2.0 ügyfél hitelesítő adatai megadják](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="26bfd-116">This is done by using an **Application ID** that uses the [OAuth 2.0 client credentials grant](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api).</span></span> <span data-ttu-id="26bfd-117">Ebben az esetben az alkalmazás úgy működik, mint maga nem felhasználóként, a Graph API hívása.</span><span class="sxs-lookup"><span data-stu-id="26bfd-117">In this case, the application acts as itself, not as a user, to call the Graph API.</span></span>

<span data-ttu-id="26bfd-118">Ebben a cikkben mutatjuk be az automatikus használati eset végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="26bfd-118">In this article, we'll discuss how to perform the automated-use case.</span></span> <span data-ttu-id="26bfd-119">Annak bemutatásához, azt fogja összeállítása a .NET 4.5 `B2CGraphClient` , amely elvégzi a felhasználó létrehozása, olvasása, frissítése és Törlés (CRUD) típusú műveletek.</span><span class="sxs-lookup"><span data-stu-id="26bfd-119">To demonstrate, we'll build a .NET 4.5 `B2CGraphClient` that performs user create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="26bfd-120">Az ügyfél egy Windows parancssori felület (CLI), mely lehetővé teszi a különböző módszerek meghívására lesz.</span><span class="sxs-lookup"><span data-stu-id="26bfd-120">The client will have a Windows command-line interface (CLI) that allows you to invoke various methods.</span></span> <span data-ttu-id="26bfd-121">Azonban a kód írása a nem interaktív, automatikus módon viselkedik.</span><span class="sxs-lookup"><span data-stu-id="26bfd-121">However, the code is written to behave in a noninteractive, automated fashion.</span></span>

## <a name="get-an-azure-ad-b2c-tenant"></a><span data-ttu-id="26bfd-122">Az Azure AD B2C-bérlő beszerzése</span><span class="sxs-lookup"><span data-stu-id="26bfd-122">Get an Azure AD B2C tenant</span></span>
<span data-ttu-id="26bfd-123">Előtt hozzon létre az alkalmazások vagy felhasználók számára, vagy minden kommunikálni az Azure AD, szüksége lesz egy Azure AD B2C-bérlő és a bérlő egy globális rendszergazdai fiókját.</span><span class="sxs-lookup"><span data-stu-id="26bfd-123">Before you can create applications or users, or interact with Azure AD at all, you will need an Azure AD B2C tenant and a global administrator account in the tenant.</span></span> <span data-ttu-id="26bfd-124">Ha már, a bérlő nincs [Ismerkedés az Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="26bfd-124">If you don't have a tenant already, [get started with Azure AD B2C](active-directory-b2c-get-started.md).</span></span>

## <a name="register-your-application-in-your-tenant"></a><span data-ttu-id="26bfd-125">Az alkalmazás regisztrálása-bérlőben</span><span class="sxs-lookup"><span data-stu-id="26bfd-125">Register your application in your tenant</span></span>
<span data-ttu-id="26bfd-126">Miután a B2C-bérlő elkészült, az alkalmazás használatával regisztrálnia kell a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="26bfd-126">After you have a B2C tenant, you need to register your application via the [Azure Portal](https://portal.azure.com).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="26bfd-127">A Graph API használata a B2C bérlőre, szüksége lesz egy dedikált alkalmazás regisztrálása az általános *App regisztrációk* panel az Azure portálon **nem** az Azure AD B2C *alkalmazások* panelen.</span><span class="sxs-lookup"><span data-stu-id="26bfd-127">To use the Graph API with your B2C tenant, you will need to register a dedicated application by using the generic *App Registrations* blade in the Azure Portal, **NOT** Azure AD B2C's *Applications* blade.</span></span> <span data-ttu-id="26bfd-128">Nem használhat újra a már meglévő B2C alkalmazások az Azure AD B2C regisztrált *alkalmazások* panelen.</span><span class="sxs-lookup"><span data-stu-id="26bfd-128">You can't reuse the already-existing B2C applications that you registered in the Azure AD B2C's *Applications* blade.</span></span>

1. <span data-ttu-id="26bfd-129">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="26bfd-129">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="26bfd-130">Válassza ki az Azure AD B2C-bérlő fiókja kiválasztja az oldal jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="26bfd-130">Choose your Azure AD B2C tenant by selecting your account in the top right corner of the page.</span></span>
3. <span data-ttu-id="26bfd-131">A bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="26bfd-131">In the left-hand navigation pane, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
4. <span data-ttu-id="26bfd-132">Kövesse az utasításokat az új alkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="26bfd-132">Follow the prompts and create a new application.</span></span> 
    1. <span data-ttu-id="26bfd-133">Válassza ki **Web App / API** az alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="26bfd-133">Select **Web App / API** as the Application Type.</span></span>    
    2. <span data-ttu-id="26bfd-134">Adjon meg **bármely átirányítási URI** (pl. https://B2CGraphAPI), mert nincs megfelelő ehhez a példához.</span><span class="sxs-lookup"><span data-stu-id="26bfd-134">Provide **any redirect URI** (e.g. https://B2CGraphAPI) as it's not relevant for this example.</span></span>  
5. <span data-ttu-id="26bfd-135">Az alkalmazás lesz most megjelenjenek az alkalmazások listájának kattintson rá az beszerzése a **Alkalmazásazonosító** (más néven Ügyfélazonosítót).</span><span class="sxs-lookup"><span data-stu-id="26bfd-135">The application will now show up in the list of applications, click on it to obtain the **Application ID** (also known as Client ID).</span></span> <span data-ttu-id="26bfd-136">Másolja, szüksége lehet rájuk egy későbbi szakasz ismerteti.</span><span class="sxs-lookup"><span data-stu-id="26bfd-136">Copy it as you'll need it in a later section.</span></span>
6. <span data-ttu-id="26bfd-137">A beállítások panelen kattintson a **kulcsok** , és adja hozzá egy új kulcsot (más néven ügyfélkulcs).</span><span class="sxs-lookup"><span data-stu-id="26bfd-137">In the Settings blade, click on **Keys** and add a new key (also known as client secret).</span></span> <span data-ttu-id="26bfd-138">Is másolja azt egy későbbi szakasz ismerteti a használatra.</span><span class="sxs-lookup"><span data-stu-id="26bfd-138">Also copy it for use in a later section.</span></span>

## <a name="configure-create-read-and-update-permissions-for-your-application"></a><span data-ttu-id="26bfd-139">Konfigurálása létrehozása, olvasása, és az alkalmazás engedélyeinek frissítése</span><span class="sxs-lookup"><span data-stu-id="26bfd-139">Configure create, read and update permissions for your application</span></span>
<span data-ttu-id="26bfd-140">Most szeretné beolvasni a szükséges engedélyekkel létrehozása, olvasása, frissítése és törlése a felhasználók számára az alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="26bfd-140">Now you need to configure your application to get all the required permissions to create, read, update and delete users.</span></span>

1. <span data-ttu-id="26bfd-141">A Folytatás az Azure-portál alkalmazás regisztrációk panelen válassza ki az alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="26bfd-141">Continuing in the Azure portal's App Registrations blade, select your application.</span></span>
2. <span data-ttu-id="26bfd-142">A beállítások panelen kattintson a **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="26bfd-142">In the Settings blade, click on **Required permissions**.</span></span>
3. <span data-ttu-id="26bfd-143">A szükséges engedélyekkel a panelen, kattintson a **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="26bfd-143">In the Required permissions blade, click on **Windows Azure Active Directory**.</span></span>
4. <span data-ttu-id="26bfd-144">A hozzáférés engedélyezése a panelen, jelölje ki a **címtáradatok olvasása és írása** engedélyt **Alkalmazásengedélyek** kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="26bfd-144">In the Enable Access  blade, select the **Read and write directory data** permission from **Application Permissions** and click **Save**.</span></span>
5. <span data-ttu-id="26bfd-145">Végezetül vissza a szükséges engedélyek panelen kattintson a a **engedélyt adjon** gombra.</span><span class="sxs-lookup"><span data-stu-id="26bfd-145">Finally, back in the Required permissions blade, click on the **Grant Permissions** button.</span></span>

<span data-ttu-id="26bfd-146">Most már rendelkezik egy alkalmazás, amely jogosult létrehozása, olvasása és frissítése a B2C-bérlő felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="26bfd-146">You now have an application that has permission to create, read and update users from your B2C tenant.</span></span>

## <a name="configure-delete-permissions-for-your-application"></a><span data-ttu-id="26bfd-147">Az alkalmazás törlése engedélyek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26bfd-147">Configure delete permissions for your application</span></span>
<span data-ttu-id="26bfd-148">Jelenleg a *címtáradatok olvasása és írása* engedély does **nem** közé tartozik például a felhasználók törlése a törlések ezt.</span><span class="sxs-lookup"><span data-stu-id="26bfd-148">Currently, the *Read and write directory data* permission does **NOT** include the ability to do any deletions such as deleting users.</span></span> <span data-ttu-id="26bfd-149">Ha szeretne adni az alkalmazás felhasználók törlésének lehetőségét, PowerShell érintő további lépések elvégzéséhez szüksége lesz, egyéb esetben ugorjon a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="26bfd-149">If you want to give your application the ability to delete users, you'll need to do these extra steps that involve PowerShell, otherwise, you can skip to the next section.</span></span>

<span data-ttu-id="26bfd-150">Első lépésként töltse le és telepítse a [Microsoft Online Services bejelentkezési segéd](http://go.microsoft.com/fwlink/?LinkID=286152).</span><span class="sxs-lookup"><span data-stu-id="26bfd-150">First, download and install the [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).</span></span> <span data-ttu-id="26bfd-151">Ezután töltse le és telepítse a [64 bites Azure Active Directory-modul Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span><span class="sxs-lookup"><span data-stu-id="26bfd-151">Then download and install the [64-bit Azure Active Directory module for Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).</span></span>

<span data-ttu-id="26bfd-152">A PowerShell-modul telepítése után nyissa meg a Powershellt, és csatlakozzon a B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="26bfd-152">After you install the PowerShell module, open PowerShell and connect to your B2C tenant.</span></span> <span data-ttu-id="26bfd-153">Futtatása után `Get-Credential`, is meg kell adnia egy felhasználónevet és jelszót, írja be a felhasználónevet és a B2C bérlő rendszergazdai fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="26bfd-153">After you run `Get-Credential`, you will be prompted for a user name and password, Enter the user name and password of your B2C tenant administrator account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="26bfd-154">Kell használnia a B2C bérlői rendszergazdai fiókot, amely **helyi** a B2C bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="26bfd-154">You need to use a B2C tenant administrator account that is **local** to the B2C tenant.</span></span> <span data-ttu-id="26bfd-155">Ezek a fiókok néznek ki: myusername@myb2ctenant.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="26bfd-155">These accounts look like this: myusername@myb2ctenant.onmicrosoft.com.</span></span>

```powershell
Connect-MsolService
```

<span data-ttu-id="26bfd-156">Most fogjuk használni a **Alkalmazásazonosító** a parancsfájlban az alábbi rendelje hozzá az alkalmazás a felhasználói fiók rendszergazdai szerepkört, amely lehetővé teszi, hogy a felhasználók törlése.</span><span class="sxs-lookup"><span data-stu-id="26bfd-156">Now we'll use the **Application ID** in the script below to assign the application the user account administrator role which will allow it to delete users.</span></span> <span data-ttu-id="26bfd-157">Ezeket a szerepköröket rendelkezik a jól ismert azonosítók, így az összes végre kell hajtani rendszer bemeneti a **Alkalmazásazonosító** az alábbi parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="26bfd-157">These roles have well-known identifiers, so all you need to do is input your **Application ID** in the script below.</span></span>

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

<span data-ttu-id="26bfd-158">Az alkalmazás most is jogosult felhasználók törlése a B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="26bfd-158">Your application now also has permissions to delete users from your B2C tenant.</span></span>

## <a name="download-configure-and-build-the-sample-code"></a><span data-ttu-id="26bfd-159">Töltse le, konfigurálása, és állítsa be a mintakód</span><span class="sxs-lookup"><span data-stu-id="26bfd-159">Download, configure, and build the sample code</span></span>
<span data-ttu-id="26bfd-160">Először letöltötte a mintakódot, és lekérése is fusson.</span><span class="sxs-lookup"><span data-stu-id="26bfd-160">First, download the sample code and get it running.</span></span> <span data-ttu-id="26bfd-161">Majd most elindítjuk azt részletes bemutatása.</span><span class="sxs-lookup"><span data-stu-id="26bfd-161">Then we will take a closer look at it.</span></span>  <span data-ttu-id="26bfd-162">Is [letöltötte a mintakódot .zip-fájlként](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="26bfd-162">You can [download the sample code as a .zip file](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip).</span></span> <span data-ttu-id="26bfd-163">Akkor is klónozhatja egy olyan könyvtárba, az Ön által választott:</span><span class="sxs-lookup"><span data-stu-id="26bfd-163">You can also clone it into a directory of your choice:</span></span>

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

<span data-ttu-id="26bfd-164">Nyissa meg a `B2CGraphClient\B2CGraphClient.sln` Visual Studio-megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="26bfd-164">Open the `B2CGraphClient\B2CGraphClient.sln` Visual Studio solution in Visual Studio.</span></span> <span data-ttu-id="26bfd-165">Az a `B2CGraphClient` projektre, nyissa meg a fájlt `App.config`.</span><span class="sxs-lookup"><span data-stu-id="26bfd-165">In the `B2CGraphClient` project, open the file `App.config`.</span></span> <span data-ttu-id="26bfd-166">A három alkalmazásbeállítást cserélje le a saját értékeit:</span><span class="sxs-lookup"><span data-stu-id="26bfd-166">Replace the three app settings with your own values:</span></span>

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="26bfd-167">A jobb gombbal a `B2CGraphClient` megoldás és a minta Újraépítés.</span><span class="sxs-lookup"><span data-stu-id="26bfd-167">Next, right-click on the `B2CGraphClient` solution and rebuild the sample.</span></span> <span data-ttu-id="26bfd-168">Ha sikeres, most rendelkeznie kell egy `B2C.exe` található végrehajtható fájl `B2CGraphClient\bin\Debug`.</span><span class="sxs-lookup"><span data-stu-id="26bfd-168">If you are successful, you should now have a `B2C.exe` executable file located in `B2CGraphClient\bin\Debug`.</span></span>

## <a name="build-user-crud-operations-by-using-the-graph-api"></a><span data-ttu-id="26bfd-169">A Graph API használatával hozhat létre felhasználói CRUD műveleteihez</span><span class="sxs-lookup"><span data-stu-id="26bfd-169">Build user CRUD operations by using the Graph API</span></span>
<span data-ttu-id="26bfd-170">A B2CGraphClient használatához nyisson meg egy `cmd` Windows parancsot a parancssorba, és módosítsa a könyvtárat a `Debug` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="26bfd-170">To use the B2CGraphClient, open a `cmd` Windows command prompt and change your directory to the `Debug` directory.</span></span> <span data-ttu-id="26bfd-171">Ezután futtassa a `B2C Help` parancsot.</span><span class="sxs-lookup"><span data-stu-id="26bfd-171">Then run the `B2C Help` command.</span></span>

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

<span data-ttu-id="26bfd-172">Ez megjeleníti minden egyes parancsnál rövid leírása.</span><span class="sxs-lookup"><span data-stu-id="26bfd-172">This will display a brief description of each command.</span></span> <span data-ttu-id="26bfd-173">Minden alkalommal, amikor aktiválják az alábbi parancsok egyikét `B2CGraphClient` egy kérést küld az Azure AD Graph API-t.</span><span class="sxs-lookup"><span data-stu-id="26bfd-173">Each time you invoke one of these commands, `B2CGraphClient` makes a request to the Azure AD Graph API.</span></span>

### <a name="get-an-access-token"></a><span data-ttu-id="26bfd-174">Hozzáférési jogkivonat lekérése</span><span class="sxs-lookup"><span data-stu-id="26bfd-174">Get an access token</span></span>
<span data-ttu-id="26bfd-175">Bármely kérelem a Graph API olyan hozzáférési jogkivonatot igényel a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="26bfd-175">Any request to the Graph API requires an access token for authentication.</span></span> <span data-ttu-id="26bfd-176">`B2CGraphClient`a hozzáférési jogkivonatok szerezni a nyílt forráskódú Active Directory Authentication Library (ADAL) használja.</span><span class="sxs-lookup"><span data-stu-id="26bfd-176">`B2CGraphClient` uses the open-source Active Directory Authentication Library (ADAL) to help acquire access tokens.</span></span> <span data-ttu-id="26bfd-177">ADAL egyszerűbbé teszi a token beszerzési biztosító egyszerű API-t, és néhány fontos részleteket, például a gyorsítótár hozzáférési jogkivonatok figyelembe vételével.</span><span class="sxs-lookup"><span data-stu-id="26bfd-177">ADAL makes token acquisition easier by providing a simple API and taking care of some important details, such as caching access tokens.</span></span> <span data-ttu-id="26bfd-178">Adal-t használó lekérni a jogkivonatokat, azonban nincs.</span><span class="sxs-lookup"><span data-stu-id="26bfd-178">You don't have to use ADAL to get tokens, though.</span></span> <span data-ttu-id="26bfd-179">Jogkivonatok HTTP-kérelmek létrehozásával is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="26bfd-179">You can also get tokens by crafting HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="26bfd-180">A fenti ADAL v2 ahhoz, hogy kommunikálhasson a Graph API-t használ.</span><span class="sxs-lookup"><span data-stu-id="26bfd-180">This code sample uses ADAL v2 in order to communicate with the Graph API.</span></span>  <span data-ttu-id="26bfd-181">Ahhoz, hogy az Azure AD Graph API-val használható hozzáférési jogkivonatok lekérésére ADAL v2 és v3 kell használnia.</span><span class="sxs-lookup"><span data-stu-id="26bfd-181">You must use ADAL v2 or v3 in order to get access tokens which can be used with the Azure AD Graph API.</span></span>
> 
> 

<span data-ttu-id="26bfd-182">Ha `B2CGraphClient` fut, létrehoz egy példányát a `B2CGraphClient` osztály.</span><span class="sxs-lookup"><span data-stu-id="26bfd-182">When `B2CGraphClient` runs, it creates an instance of the `B2CGraphClient` class.</span></span> <span data-ttu-id="26bfd-183">Ez az osztály konstruktorában állít be egy ADAL-hitelesítés állványok:</span><span class="sxs-lookup"><span data-stu-id="26bfd-183">The constructor for this class sets up an ADAL authentication scaffolding:</span></span>

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

<span data-ttu-id="26bfd-184">Fogjuk használni a `B2C Get-User` példaként parancsot.</span><span class="sxs-lookup"><span data-stu-id="26bfd-184">We'll use the `B2C Get-User` command as an example.</span></span> <span data-ttu-id="26bfd-185">Ha `B2C Get-User` minden további, a parancssori felület hívások bemeneti adatok nélkül meghívták a `B2CGraphClient.GetAllUsers(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="26bfd-185">When `B2C Get-User` is invoked without any additional inputs, the CLI calls the `B2CGraphClient.GetAllUsers(...)` method.</span></span> <span data-ttu-id="26bfd-186">Ez a metódus meghívja `B2CGraphClient.SendGraphGetRequest(...)`, amely elküld egy HTTP GET kérést a Graph API-val.</span><span class="sxs-lookup"><span data-stu-id="26bfd-186">This method calls `B2CGraphClient.SendGraphGetRequest(...)`, which submits an HTTP GET request to the Graph API.</span></span> <span data-ttu-id="26bfd-187">Mielőtt `B2CGraphClient.SendGraphGetRequest(...)` küld a GET kérelmek, először nyer access token ADAL használatával:</span><span class="sxs-lookup"><span data-stu-id="26bfd-187">Before `B2CGraphClient.SendGraphGetRequest(...)` sends the GET request, it first gets an access token by using ADAL:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

<span data-ttu-id="26bfd-188">Kaphat access token a Graph API hívása az ADAL `AuthenticationContext.AcquireToken(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="26bfd-188">You can get an access token for the Graph API by calling the ADAL `AuthenticationContext.AcquireToken(...)` method.</span></span> <span data-ttu-id="26bfd-189">Adal-t adja vissza egy `access_token` , amely jelzi, hogy az alkalmazás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="26bfd-189">ADAL then returns an `access_token` that represents the application's identity.</span></span>

### <a name="read-users"></a><span data-ttu-id="26bfd-190">Olvassa el a felhasználók</span><span class="sxs-lookup"><span data-stu-id="26bfd-190">Read users</span></span>
<span data-ttu-id="26bfd-191">Ha azt szeretné, a felhasználók listáját, vagy kérjen egy adott felhasználó a Graph API-val, HTTP küldhet `GET` elküldeni a kérelmet a `/users` végpont.</span><span class="sxs-lookup"><span data-stu-id="26bfd-191">When you want to get a list of users or get a particular user from the Graph API, you can send an HTTP `GET` request to the `/users` endpoint.</span></span> <span data-ttu-id="26bfd-192">A bérlő számára az összes kérelem így néz ki:</span><span class="sxs-lookup"><span data-stu-id="26bfd-192">A request for all of the users in a tenant looks like this:</span></span>

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="26bfd-193">A kérelem megtekintéséhez futtassa:</span><span class="sxs-lookup"><span data-stu-id="26bfd-193">To see this request, run:</span></span>

 ```
 > B2C Get-User
 ```

<span data-ttu-id="26bfd-194">Két fontos dolgot figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="26bfd-194">There are two important things to note:</span></span>

* <span data-ttu-id="26bfd-195">A hozzáférési jogkivonat ADAL keresztül szerzett hozzáadódik a `Authorization` fejléc használatával a `Bearer` séma.</span><span class="sxs-lookup"><span data-stu-id="26bfd-195">The access token acquired via ADAL is added to the `Authorization` header by using the `Bearer` scheme.</span></span>
* <span data-ttu-id="26bfd-196">A B2C bérlőre, kell használnia a következő lekérdezésparaméter `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="26bfd-196">For B2C tenants, you must use the query parameter `api-version=1.6`.</span></span>

<span data-ttu-id="26bfd-197">Ezen adatok mindegyikét kezeli a `B2CGraphClient.SendGraphGetRequest(...)` módszert:</span><span class="sxs-lookup"><span data-stu-id="26bfd-197">Both of these details are handled in the `B2CGraphClient.SendGraphGetRequest(...)` method:</span></span>

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a><span data-ttu-id="26bfd-198">Felhasználói fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="26bfd-198">Create consumer user accounts</span></span>
<span data-ttu-id="26bfd-199">A B2C-bérlő felhasználói fiókokat hoz létre, amikor HTTP küldhet `POST` elküldeni a kérelmet a `/users` végpont:</span><span class="sxs-lookup"><span data-stu-id="26bfd-199">When you create user accounts in your B2C tenant, you can send an HTTP `POST` request to the `/users` endpoint:</span></span>

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying to the end user
    "mailNickname": "joec",                        // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

<span data-ttu-id="26bfd-200">A legtöbb ezeket a tulajdonságokat a kérésben identitásrendszerében a felhasználók létrehozásához szükségesek.</span><span class="sxs-lookup"><span data-stu-id="26bfd-200">Most of these properties in this request are required to create consumer users.</span></span> <span data-ttu-id="26bfd-201">További tudnivalókért kattintson [Itt](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span><span class="sxs-lookup"><span data-stu-id="26bfd-201">To learn more, click [here](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser).</span></span> <span data-ttu-id="26bfd-202">Vegye figyelembe, hogy a `//` illusztrációs megjelent megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="26bfd-202">Note that the `//` comments have been included for illustration.</span></span> <span data-ttu-id="26bfd-203">Ne szerepeljen az azokat a tényleges kérelmet.</span><span class="sxs-lookup"><span data-stu-id="26bfd-203">Do not include them in an actual request.</span></span>

<span data-ttu-id="26bfd-204">A kérelem megtekintéséhez futtassa a következő parancsok egyikét:</span><span class="sxs-lookup"><span data-stu-id="26bfd-204">To see the request, run one of the following commands:</span></span>

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

<span data-ttu-id="26bfd-205">A `Create-User` parancs fogadja bemeneti paraméterként egy .JSON kiterjesztésű fájlt.</span><span class="sxs-lookup"><span data-stu-id="26bfd-205">The `Create-User` command takes a .json file as an input parameter.</span></span> <span data-ttu-id="26bfd-206">Ez tartalmazza a user objektum JSON-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="26bfd-206">This contains a JSON representation of a user object.</span></span> <span data-ttu-id="26bfd-207">Példakód két minta .JSON kiterjesztésű fájl van: `usertemplate-email.json` és `usertemplate-username.json`.</span><span class="sxs-lookup"><span data-stu-id="26bfd-207">There are two sample .json files in the sample code: `usertemplate-email.json` and `usertemplate-username.json`.</span></span> <span data-ttu-id="26bfd-208">Ezeket a fájlokat a saját igényeinek megfelelően módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="26bfd-208">You can modify these files to suit your needs.</span></span> <span data-ttu-id="26bfd-209">A kötelező mezőket a fenti mellett több választható mezőket, melyekkel ezeket a fájlokat szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="26bfd-209">In addition to the required fields above, several optional fields that you can use are included in these files.</span></span> <span data-ttu-id="26bfd-210">A választható mezőket a részletek megtalálhatók a [Azure AD Graph API entitáshivatkozás](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span><span class="sxs-lookup"><span data-stu-id="26bfd-210">Details on the optional fields can be found in the [Azure AD Graph API entity reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).</span></span>

<span data-ttu-id="26bfd-211">Láthatja, hogyan a POST-kérelmet a összeállított `B2CGraphClient.SendGraphPostRequest(...)`.</span><span class="sxs-lookup"><span data-stu-id="26bfd-211">You can see how the POST request is constructed in `B2CGraphClient.SendGraphPostRequest(...)`.</span></span>

* <span data-ttu-id="26bfd-212">A hozzáférési token csatol a `Authorization` a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="26bfd-212">It attaches an access token to the `Authorization` header of the request.</span></span>
* <span data-ttu-id="26bfd-213">Állítja a `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="26bfd-213">It sets `api-version=1.6`.</span></span>
* <span data-ttu-id="26bfd-214">Ez magában foglalja a JSON felhasználói objektum, a kérelem törzsében.</span><span class="sxs-lookup"><span data-stu-id="26bfd-214">It includes the JSON user object in the body of the request.</span></span>

> [!NOTE]
> <span data-ttu-id="26bfd-215">Ha az áttelepítendő ki egy meglévő felhasználói fiókokat alacsonyabb jelszó erőssége, mint a [kényszeríti ki az Azure AD B2C erős jelszó erőssége](https://msdn.microsoft.com/library/azure/jj943764.aspx), letilthatja az erős jelszó követelmény használatával a `DisableStrongPassword` értéket a `passwordPolicies` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="26bfd-215">If the accounts that you want to migrate from an existing user store has lower password strength than the [strong password strength enforced by Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), you can disable the strong password requirement using the `DisableStrongPassword` value in the `passwordPolicies` property.</span></span> <span data-ttu-id="26bfd-216">Például a következőképpen fent megadott felhasználói kérés módosíthatja: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span><span class="sxs-lookup"><span data-stu-id="26bfd-216">For instance, you can modify the create user request provided above as follows: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.</span></span>
> 
> 

### <a name="update-consumer-user-accounts"></a><span data-ttu-id="26bfd-217">Felhasználói fiókok frissítése</span><span class="sxs-lookup"><span data-stu-id="26bfd-217">Update consumer user accounts</span></span>
<span data-ttu-id="26bfd-218">Ha frissíti a felhasználói objektumok, a folyamat hasonlít a felhasználói objektumok létrehozására használhatja.</span><span class="sxs-lookup"><span data-stu-id="26bfd-218">When you update user objects, the process is similar to the one you use to create user objects.</span></span> <span data-ttu-id="26bfd-219">Ez a folyamat a HTTP Protokollt használ, de `PATCH` módszert:</span><span class="sxs-lookup"><span data-stu-id="26bfd-219">But this process uses the HTTP `PATCH` method:</span></span>

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only the user's displayName
}
```

<span data-ttu-id="26bfd-220">Próbálja meg frissíteni a felhasználó frissítése a JSON-fájlokat az új adatokat.</span><span class="sxs-lookup"><span data-stu-id="26bfd-220">Try to update a user by updating your JSON files with new data.</span></span> <span data-ttu-id="26bfd-221">Ezután `B2CGraphClient` az alábbi parancsok egyikét futtatja:</span><span class="sxs-lookup"><span data-stu-id="26bfd-221">You can then use `B2CGraphClient` to run one of these commands:</span></span>

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

<span data-ttu-id="26bfd-222">Vizsgálja meg a `B2CGraphClient.SendGraphPatchRequest(...)` metódus talál részletes információt a kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="26bfd-222">Inspect the `B2CGraphClient.SendGraphPatchRequest(...)` method for details on how to send this request.</span></span>

### <a name="search-users"></a><span data-ttu-id="26bfd-223">Felhasználók keresése</span><span class="sxs-lookup"><span data-stu-id="26bfd-223">Search users</span></span>
<span data-ttu-id="26bfd-224">Felhasználók B2C-bérlőben lévő több módon is kereshet.</span><span class="sxs-lookup"><span data-stu-id="26bfd-224">You can search for users in your B2C tenant in a couple of ways.</span></span> <span data-ttu-id="26bfd-225">Egy, a felhasználó azonosító vagy a felhasználó bejelentkezési azonosítót használja, a két objektum (azaz a `signInNames` tulajdonság).</span><span class="sxs-lookup"><span data-stu-id="26bfd-225">One, using the user's object ID or two, using the user's sign-in identifer (i.e., the `signInNames` property).</span></span>

<span data-ttu-id="26bfd-226">Egy adott felhasználó keresése a következő parancsok egyikét futtatja:</span><span class="sxs-lookup"><span data-stu-id="26bfd-226">Run one of the following commands to search for a specific user:</span></span>

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

<span data-ttu-id="26bfd-227">Íme néhány példa:</span><span class="sxs-lookup"><span data-stu-id="26bfd-227">Here are a couple of examples:</span></span>

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a><span data-ttu-id="26bfd-228">Felhasználók törlése</span><span class="sxs-lookup"><span data-stu-id="26bfd-228">Delete users</span></span>
<span data-ttu-id="26bfd-229">A felhasználó törlése történik egyszerű.</span><span class="sxs-lookup"><span data-stu-id="26bfd-229">The process for deleting a user is straightforward.</span></span> <span data-ttu-id="26bfd-230">A HTTP protokollal `DELETE` metódus és a szerkezetet az URL-cím a megfelelő objektumazonosító:</span><span class="sxs-lookup"><span data-stu-id="26bfd-230">Use the HTTP `DELETE` method and construct the URL with the correct object ID:</span></span>

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

<span data-ttu-id="26bfd-231">Példa megtekintéséhez írja be ezt a parancsot, és tekintse meg a törlési kérelmet, amelyet a program a konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="26bfd-231">To see an example, enter this command and view the delete request that is printed to the console:</span></span>

```
> B2C Delete-User <object-id-of-user>
```

<span data-ttu-id="26bfd-232">Vizsgálja meg a `B2CGraphClient.SendGraphDeleteRequest(...)` metódus talál részletes információt a kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="26bfd-232">Inspect the `B2CGraphClient.SendGraphDeleteRequest(...)` method for details on how to send this request.</span></span>

<span data-ttu-id="26bfd-233">Az Azure AD Graph API felhasználók kezelése mellett sok más műveletet végezheti el.</span><span class="sxs-lookup"><span data-stu-id="26bfd-233">You can perform many other actions with the Azure AD Graph API in addition to user management.</span></span> <span data-ttu-id="26bfd-234">A [Azure AD Graph API-referencia](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) minden művelet együtt minta kérelmek részleteit.</span><span class="sxs-lookup"><span data-stu-id="26bfd-234">The [Azure AD Graph API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) provides details on each action, along with sample requests.</span></span>

## <a name="use-custom-attributes"></a><span data-ttu-id="26bfd-235">Egyéni attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="26bfd-235">Use custom attributes</span></span>
<span data-ttu-id="26bfd-236">A legtöbb fogyasztói alkalmazásokat kell valamilyen egyéni felhasználói profil adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="26bfd-236">Most consumer applications need to store some type of custom user profile information.</span></span> <span data-ttu-id="26bfd-237">Ehhez egyik módja a B2C-bérlő ad meg egy egyedi attribútumot.</span><span class="sxs-lookup"><span data-stu-id="26bfd-237">One way you can do this is to define a custom attribute in your B2C tenant.</span></span> <span data-ttu-id="26bfd-238">Ez az attribútum úgy, ahogy bármely más tulajdonság úgy kezelje, a user objektum akkor kezelni.</span><span class="sxs-lookup"><span data-stu-id="26bfd-238">You can then treat that attribute the same way you treat any other property on a user object.</span></span> <span data-ttu-id="26bfd-239">Az attribútum, törli az attribútumot, az attribútum alapján, küldése az attribútum a bejelentkezési jogkivonatokat, és több jogcímként.</span><span class="sxs-lookup"><span data-stu-id="26bfd-239">You can update the attribute, delete the attribute, query by the attribute, send the attribute as a claim in sign-in tokens, and more.</span></span>

<span data-ttu-id="26bfd-240">A B2C-bérlő egyéni attribútumot megadásához tekintse meg a [B2C egyéni attribútumhivatkozás](active-directory-b2c-reference-custom-attr.md).</span><span class="sxs-lookup"><span data-stu-id="26bfd-240">To define a custom attribute in your B2C tenant, see the [B2C custom attribute reference](active-directory-b2c-reference-custom-attr.md).</span></span>

<span data-ttu-id="26bfd-241">Megtekintheti a használatával a B2C-bérlő definiált egyéni attribútumok `B2CGraphClient`:</span><span class="sxs-lookup"><span data-stu-id="26bfd-241">You can view the custom attributes defined in your B2C tenant by using `B2CGraphClient`:</span></span>

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

<span data-ttu-id="26bfd-242">A kimeneti ezeket a funkciókat, mint felfedi összes egyéni attribútumot, részleteit:</span><span class="sxs-lookup"><span data-stu-id="26bfd-242">The output of these functions reveals the details of each custom attribute, such as:</span></span>

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

<span data-ttu-id="26bfd-243">Használhatja a teljes nevet, például a `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, a felhasználói objektumok tulajdonságainál.</span><span class="sxs-lookup"><span data-stu-id="26bfd-243">You can use the full name, such as `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, as a property on your user objects.</span></span>  <span data-ttu-id="26bfd-244">Frissítse a .JSON kiterjesztésű fájlt az új tulajdonságot, és a tulajdonság értékét, és futtassa:</span><span class="sxs-lookup"><span data-stu-id="26bfd-244">Update your .json file with the new property and a value for the property, and then run:</span></span>

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

<span data-ttu-id="26bfd-245">A `B2CGraphClient`, programozott módon kezelje a B2C bérlő felhasználók szolgáltatás-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="26bfd-245">By using `B2CGraphClient`, you have a service application that can manage your B2C tenant users programmatically.</span></span> <span data-ttu-id="26bfd-246">`B2CGraphClient`saját alkalmazás identitását használja az Azure AD Graph API felé történő hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="26bfd-246">`B2CGraphClient` uses its own application identity to authenticate to the Azure AD Graph API.</span></span> <span data-ttu-id="26bfd-247">Egy ügyfélkulcsot a jogkivonatok is megkapja.</span><span class="sxs-lookup"><span data-stu-id="26bfd-247">It also acquires tokens by using a client secret.</span></span> <span data-ttu-id="26bfd-248">Mivel használhatja ezt a funkciót az alkalmazásba, ne felejtse néhány fő szempontot B2C-alkalmazásokhoz:</span><span class="sxs-lookup"><span data-stu-id="26bfd-248">As you incorporate this functionality into your application, remember a few key points for B2C apps:</span></span>

* <span data-ttu-id="26bfd-249">Adja meg az alkalmazás a bérlő a megfelelő engedélyeket kell.</span><span class="sxs-lookup"><span data-stu-id="26bfd-249">You need to grant the application the proper permissions in the tenant.</span></span>
* <span data-ttu-id="26bfd-250">Most kell használnia az adal-t (nem MSAL) jogkivonatot beolvasni.</span><span class="sxs-lookup"><span data-stu-id="26bfd-250">For now, you need to use ADAL (not MSAL) to get access tokens.</span></span> <span data-ttu-id="26bfd-251">(Ön is is küldhet közvetlen, szalagtár használata nélkül.)</span><span class="sxs-lookup"><span data-stu-id="26bfd-251">(You can also send protocol messages directly, without using a library.)</span></span>
* <span data-ttu-id="26bfd-252">A Graph API hívásakor használható `api-version=1.6`.</span><span class="sxs-lookup"><span data-stu-id="26bfd-252">When you call the Graph API, use `api-version=1.6`.</span></span>
* <span data-ttu-id="26bfd-253">Amikor létrehozása és frissítése identitásrendszerében a felhasználók, néhány tulajdonságok szükségesek, fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="26bfd-253">When you create and update consumer users, a few properties are required, as described above.</span></span>

<span data-ttu-id="26bfd-254">Ha bármilyen kérdése vagy a műveletek hajtsa végre a B2C-bérlő a Graph API segítségével szeretné, ez a cikk megjegyzést szóljon, vagy problémát fájlt a Githubon code minta tárházban.</span><span class="sxs-lookup"><span data-stu-id="26bfd-254">If you have any questions or requests for actions you would like to perform by using the Graph API on your B2C tenant, leave a comment on this article or file an issue in the GitHub code sample repository.</span></span>

