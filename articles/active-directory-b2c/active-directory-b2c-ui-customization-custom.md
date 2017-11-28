---
title: "Egyéni házirendek – az Azure AD B2C használatával a felhasználói felület testreszabása |} Microsoft Docs"
description: "További tudnivalók a felhasználói felület (UI) testreszabása, miközben egyéni házirendekkel használhatja az Azure AD B2C."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: d5a3c0a323b31696d39e3d2b36317dec3a2337d7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a><span data-ttu-id="fa97a-103">Az Azure Active Directory B2C: Egyéni házirendek konfigurálása a felhasználói felület testreszabása</span><span class="sxs-lookup"><span data-stu-id="fa97a-103">Azure Active Directory B2C: Configure UI customization in a custom policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="fa97a-104">Ez a cikk befejezése után fog regisztráció és bejelentkezés egyéni házirendet a márka és megjelenését.</span><span class="sxs-lookup"><span data-stu-id="fa97a-104">After you complete this article, you will have a sign-up and sign-in custom policy with your brand and appearance.</span></span> <span data-ttu-id="fa97a-105">Azure Active Directory B2C (az Azure AD B2C), kap a HTML- és CSS tartalom bemutatott szinte teljes hozzáférés a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="fa97a-105">With Azure Active Directory B2C (Azure AD B2C), you get nearly full control of the HTML and CSS content that's presented to users.</span></span> <span data-ttu-id="fa97a-106">Egyéni házirend használata esetén konfigurálnia felhasználói felület testreszabásával XML-vezérlők használata az Azure portálon helyett.</span><span class="sxs-lookup"><span data-stu-id="fa97a-106">When you use a custom policy, you configure UI customization in XML instead of using controls in the Azure portal.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fa97a-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fa97a-107">Prerequisites</span></span>

<span data-ttu-id="fa97a-108">Mielőtt elkezdené, fejezze be a [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="fa97a-108">Before you begin, complete [Getting started with custom policies](active-directory-b2c-get-started-custom.md).</span></span> <span data-ttu-id="fa97a-109">A regisztrációt, és jelentkezzen be helyi fiókok működő egyéni házirendet kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="fa97a-109">You should have a working custom policy for sign-up and sign-in with local accounts.</span></span>

## <a name="page-ui-customization"></a><span data-ttu-id="fa97a-110">Lap felhasználói felületének testreszabása</span><span class="sxs-lookup"><span data-stu-id="fa97a-110">Page UI customization</span></span>

<span data-ttu-id="fa97a-111">A lap felhasználói felületének testreszabása szolgáltatás segítségével testre szabhatja a bármely egyéni házirend megjelenését és működését.</span><span class="sxs-lookup"><span data-stu-id="fa97a-111">By using the page UI customization feature, you can customize the look and feel of any custom policy.</span></span> <span data-ttu-id="fa97a-112">Kezelheti a márka és egységes megjelenést az alkalmazás és az Azure AD B2C között is.</span><span class="sxs-lookup"><span data-stu-id="fa97a-112">You can also maintain brand and visual consistency between your application and Azure AD B2C.</span></span>

<span data-ttu-id="fa97a-113">Hogyan működik ez: Azure AD B2C az ügyfél böngészőjében kód fut, és a modern megközelítés nevű [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="fa97a-113">Here's how it works: Azure AD B2C runs code in your customer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span></span> <span data-ttu-id="fa97a-114">Első lépésként ad meg URL-címet az egyéni házirendekben a testreszabott HTML-tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="fa97a-114">First, you specify a URL in the custom policy with customized HTML content.</span></span> <span data-ttu-id="fa97a-115">Az Azure AD B2C egyesíti a HTML-tartalmakat, be töltve az URL-címről, majd a oldalát jeleníti meg, hogy az ügyfél felhasználói felületének elemmel.</span><span class="sxs-lookup"><span data-stu-id="fa97a-115">Azure AD B2C merges UI elements with the HTML content that's loaded from your URL and then displays the page to the customer.</span></span>

## <a name="create-your-html5-content"></a><span data-ttu-id="fa97a-116">A HTML5 tartalmának létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa97a-116">Create your HTML5 content</span></span>

<span data-ttu-id="fa97a-117">Hozzon létre HTML tartalom a termék márkáját nevű cím.</span><span class="sxs-lookup"><span data-stu-id="fa97a-117">Create HTML content with your product's brand name in the title.</span></span>

1. <span data-ttu-id="fa97a-118">Másolja az alábbi HTML-részlet.</span><span class="sxs-lookup"><span data-stu-id="fa97a-118">Copy the following HTML snippet.</span></span> <span data-ttu-id="fa97a-119">Szabályos üres elem a HTML5 nevű  *\<div id = "api"\>\</div\>*  belül található a  *\<törzs\>*  címkék.</span><span class="sxs-lookup"><span data-stu-id="fa97a-119">It is well-formed HTML5 with an empty element called *\<div id="api"\>\</div\>* located within the *\<body\>* tags.</span></span> <span data-ttu-id="fa97a-120">Ez az elem azt jelzi, ahol az Azure AD B2C tartalmat beszúrni.</span><span class="sxs-lookup"><span data-stu-id="fa97a-120">This element indicates where Azure AD B2C content is to be inserted.</span></span>

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

   >[!NOTE]
   ><span data-ttu-id="fa97a-121">Biztonsági okokból a JavaScript használatát jelenleg le van tiltva a testreszabáshoz.</span><span class="sxs-lookup"><span data-stu-id="fa97a-121">For security reasons, the use of JavaScript is currently blocked for customization.</span></span>

2. <span data-ttu-id="fa97a-122">Illessze be a másolt részlet egy szövegszerkesztőben, és mentse a fájlt *testreszabása ui.html*.</span><span class="sxs-lookup"><span data-stu-id="fa97a-122">Paste the copied snippet in a text editor, and then save the file as *customize-ui.html*.</span></span>

## <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="fa97a-123">Egy Azure Blob storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa97a-123">Create an Azure Blob storage account</span></span>

>[!NOTE]
> <span data-ttu-id="fa97a-124">Ebben a cikkben a Azure Blob Storage tárolót a tartalom tárolására szolgáló használjuk.</span><span class="sxs-lookup"><span data-stu-id="fa97a-124">In this article, we use Azure Blob storage to host our content.</span></span> <span data-ttu-id="fa97a-125">Választhat, hogy a tartalmát egy webkiszolgálón, de meg kell [CORS engedélyezése a webkiszolgálón](https://enable-cors.org/server.html).</span><span class="sxs-lookup"><span data-stu-id="fa97a-125">You can choose to host your content on a web server, but you must [enable CORS on your web server](https://enable-cors.org/server.html).</span></span>

<span data-ttu-id="fa97a-126">A HTML-tartalmakat, a Blob Storage tárolóban tárolni, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fa97a-126">To host this HTML content in Blob storage, do the following:</span></span>

1. <span data-ttu-id="fa97a-127">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa97a-127">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fa97a-128">Az a **Hub** menüjében válassza **új** > **tárolási** > **tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-128">On the **Hub** menu, select **New** > **Storage** > **Storage account**.</span></span>
3. <span data-ttu-id="fa97a-129">Adjon meg egy egyedi **neve** a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="fa97a-129">Enter a unique **Name** for your storage account.</span></span>
4. <span data-ttu-id="fa97a-130">**Telepítési modell** maradjanak **erőforrás-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-130">**Deployment model** can remain **Resource Manager**.</span></span>
5. <span data-ttu-id="fa97a-131">Változás **fiók típusa** való **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-131">Change **Account Kind** to **Blob storage**.</span></span>
6. <span data-ttu-id="fa97a-132">**Teljesítmény** maradjanak **szabványos**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-132">**Performance** can remain **Standard**.</span></span>
7. <span data-ttu-id="fa97a-133">**Replikációs** maradjanak **RA-GRS**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-133">**Replication** can remain **RA-GRS**.</span></span>
8. <span data-ttu-id="fa97a-134">**Hozzáférési szint** maradjanak **gyakran használt adatok**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-134">**Access tier** can remain **Hot**.</span></span>
9. <span data-ttu-id="fa97a-135">**Storage szolgáltatás titkosítási** maradjanak **letiltott**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-135">**Storage service encryption** can remain **Disabled**.</span></span>
10. <span data-ttu-id="fa97a-136">Válassza ki a **előfizetés** a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="fa97a-136">Select a **Subscription** for your storage account.</span></span>
11. <span data-ttu-id="fa97a-137">Hozzon létre egy **erőforráscsoport** vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="fa97a-137">Create a **Resource group** or select an existing one.</span></span>
12. <span data-ttu-id="fa97a-138">Válassza ki a **földrajzi hely** a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="fa97a-138">Select the **Geographic location** for your storage account.</span></span>
13. <span data-ttu-id="fa97a-139">Kattintson a **Létrehozás** parancsra a tárfiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="fa97a-139">Click **Create** to create the storage account.</span></span>  
    <span data-ttu-id="fa97a-140">A telepítés befejezése után a **tárfiók** panel nyílik meg automatikusan.</span><span class="sxs-lookup"><span data-stu-id="fa97a-140">After the deployment is completed, the **Storage account** blade opens automatically.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="fa97a-141">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa97a-141">Create a container</span></span>

<span data-ttu-id="fa97a-142">A Blob-tároló egy nyilvános tároló létrehozásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fa97a-142">To create a public container in Blob storage, do the following:</span></span>

1. <span data-ttu-id="fa97a-143">Kattintson a **áttekintése** fülre.</span><span class="sxs-lookup"><span data-stu-id="fa97a-143">Click the **Overview** tab.</span></span>
2. <span data-ttu-id="fa97a-144">Kattintson a **tároló**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-144">Click **Container**.</span></span>
3. <span data-ttu-id="fa97a-145">A **neve**, típus **$root**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-145">For **Name**, type **$root**.</span></span>
4. <span data-ttu-id="fa97a-146">Állítsa be **hozzáférési típus** való **Blob**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-146">Set **Access type** to **Blob**.</span></span>
5. <span data-ttu-id="fa97a-147">Kattintson a **$root** az új tároló megnyitása.</span><span class="sxs-lookup"><span data-stu-id="fa97a-147">Click **$root** to open the new container.</span></span>
6. <span data-ttu-id="fa97a-148">Kattintson a **Feltöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="fa97a-148">Click **Upload**.</span></span>
7. <span data-ttu-id="fa97a-149">Kattintson a Tovább gombra a mappa ikon **válasszon ki egy fájlt**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-149">Click the folder icon next to **Select a file**.</span></span>
8. <span data-ttu-id="fa97a-150">Ugrás a **testreszabása ui.html**, amely amelyet előzőleg létrehozott a [lap felhasználói felületének testreszabása](#the-page-ui-customization-feature) szakasz.</span><span class="sxs-lookup"><span data-stu-id="fa97a-150">Go to **customize-ui.html**, which you created earlier in the [Page UI customization](#the-page-ui-customization-feature) section.</span></span>
9. <span data-ttu-id="fa97a-151">Kattintson a **Feltöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="fa97a-151">Click **Upload**.</span></span>
10. <span data-ttu-id="fa97a-152">Válassza ki a feltöltött testreszabás-ui.html blob.</span><span class="sxs-lookup"><span data-stu-id="fa97a-152">Select the customize-ui.html blob that you uploaded.</span></span>
11. <span data-ttu-id="fa97a-153">A **URL-cím**, kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-153">Next to **URL**, click **Copy**.</span></span>
12. <span data-ttu-id="fa97a-154">A böngészőben beilleszteni a másolt URL-címet, és nyissa meg a helyhez.</span><span class="sxs-lookup"><span data-stu-id="fa97a-154">In a browser, paste the copied URL, and go to the site.</span></span> <span data-ttu-id="fa97a-155">Ha a hely nem érhető el, győződjön meg arról, hogy a tároló hozzáférési típus értéke **blob**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-155">If the site is inaccessible, make sure the container access type is set to **blob**.</span></span>

## <a name="configure-cors"></a><span data-ttu-id="fa97a-156">A CORS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fa97a-156">Configure CORS</span></span>

<span data-ttu-id="fa97a-157">Konfigurálása a Blob storage eltérő eredetű erőforrások megosztása a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fa97a-157">Configure Blob storage for Cross-Origin Resource Sharing by doing the following:</span></span>

>[!NOTE]
><span data-ttu-id="fa97a-158">Szeretné, hogy a minta HTML és CSS tartalom használatával a felhasználói felület testreszabása a szolgáltatás kipróbálásához?</span><span class="sxs-lookup"><span data-stu-id="fa97a-158">Want to try out the UI customization feature by using our sample HTML and CSS content?</span></span> <span data-ttu-id="fa97a-159">Mellékelt [egy egyszerű segédeszköze](active-directory-b2c-reference-ui-customization-helper-tool.md) , amely feltölti és konfigurálja a minta tartalmat a Blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="fa97a-159">We've provided [a simple helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) that uploads and configures our sample content on your Blob storage account.</span></span> <span data-ttu-id="fa97a-160">Ha az eszköz használatához ugorjon előre [módosítsa a regisztráció vagy bejelentkezés egyéni házirendet](#modify-your-sign-up-or-sign-in-custom-policy).</span><span class="sxs-lookup"><span data-stu-id="fa97a-160">If you use the tool, skip ahead to [Modify your sign-up or sign-in custom policy](#modify-your-sign-up-or-sign-in-custom-policy).</span></span>

1. <span data-ttu-id="fa97a-161">Az a **tárolási** panel alatt **beállítások**, nyissa meg **CORS**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-161">On the **Storage** blade, under **Settings**, open **CORS**.</span></span>
2. <span data-ttu-id="fa97a-162">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="fa97a-162">Click **Add**.</span></span>
3. <span data-ttu-id="fa97a-163">A **engedélyezett eredeteket**, írja be egy csillag (\*).</span><span class="sxs-lookup"><span data-stu-id="fa97a-163">For **Allowed origins**, type an asterisk (\*).</span></span>
4. <span data-ttu-id="fa97a-164">Az a **engedélyezett műveletek** legördülő listából válassza ki, válassza ki mindkét **beolvasása** és **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-164">In the **Allowed verbs** drop-down list, select both **GET** and **OPTIONS**.</span></span>
5. <span data-ttu-id="fa97a-165">A **engedélyezett fejlécek**, írja be egy csillag (\*).</span><span class="sxs-lookup"><span data-stu-id="fa97a-165">For **Allowed headers**, type an asterisk (\*).</span></span>
6. <span data-ttu-id="fa97a-166">A **fejlécek kitett**, írja be egy csillag (\*).</span><span class="sxs-lookup"><span data-stu-id="fa97a-166">For **Exposed headers**, type an asterisk (\*).</span></span>
7. <span data-ttu-id="fa97a-167">A **maximális élettartama (másodperc)**, típus **200**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-167">For **Maximum age (seconds)**, type **200**.</span></span>
8. <span data-ttu-id="fa97a-168">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="fa97a-168">Click **Add**.</span></span>

## <a name="test-cors"></a><span data-ttu-id="fa97a-169">A CORS tesztelése</span><span class="sxs-lookup"><span data-stu-id="fa97a-169">Test CORS</span></span>

<span data-ttu-id="fa97a-170">Ellenőrizze, hogy készen áll a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fa97a-170">Validate that you're ready by doing the following:</span></span>

1. <span data-ttu-id="fa97a-171">Lépjen a [teszt-cors.org](http://test-cors.org/) webhelyen, majd illessze be az URL-címet a **távoli URL-cím** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="fa97a-171">Go to the [test-cors.org](http://test-cors.org/) website, and then paste the URL in the **Remote URL** box.</span></span>
2. <span data-ttu-id="fa97a-172">Kattintson a **kérés küldése**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-172">Click **Send Request**.</span></span>  
    <span data-ttu-id="fa97a-173">Ha hibaüzenetet kap, ellenőrizze, hogy a [CORS-beállítások](#configure-cors) helyes-e.</span><span class="sxs-lookup"><span data-stu-id="fa97a-173">If you receive an error, make sure that your [CORS settings](#configure-cors) are correct.</span></span> <span data-ttu-id="fa97a-174">Is szükség lehet a böngésző gyorsítótárat kiürítheti, vagy egy InPrivate-böngészés munkamenet megnyitásához nyomja le a Ctrl + Shift + P.</span><span class="sxs-lookup"><span data-stu-id="fa97a-174">You might also need to clear your browser cache or open an in-private browsing session by pressing Ctrl+Shift+P.</span></span>

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a><span data-ttu-id="fa97a-175">A regisztráció vagy bejelentkezés egyéni házirend módosítása</span><span class="sxs-lookup"><span data-stu-id="fa97a-175">Modify your sign-up or sign-in custom policy</span></span>

<span data-ttu-id="fa97a-176">A legfelső szintű alatt  *\<TrustFrameworkPolicy\>*  címke, keresse meg  *\<BuildingBlocks\>*  címke.</span><span class="sxs-lookup"><span data-stu-id="fa97a-176">Under the top-level *\<TrustFrameworkPolicy\>* tag, you should find *\<BuildingBlocks\>* tag.</span></span> <span data-ttu-id="fa97a-177">Belül a  *\<BuildingBlocks\>*  címkék, vegye fel a  *\<ContentDefinitions\>*  címke a következő példa másolásával.</span><span class="sxs-lookup"><span data-stu-id="fa97a-177">Within the *\<BuildingBlocks\>* tags, add a *\<ContentDefinitions\>* tag by copying the following example.</span></span> <span data-ttu-id="fa97a-178">Cserélje le *your_storage_account* a tárfiók nevével.</span><span class="sxs-lookup"><span data-stu-id="fa97a-178">Replace *your_storage_account* with the name of your storage account.</span></span>

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a><span data-ttu-id="fa97a-179">Töltse fel a frissített egyéni házirend</span><span class="sxs-lookup"><span data-stu-id="fa97a-179">Upload your updated custom policy</span></span>

1. <span data-ttu-id="fa97a-180">Az a [Azure-portálon](https://portal.azure.com), [átváltani a környezetében az Azure AD B2C-bérlő](active-directory-b2c-navigate-to-b2c-context.md), majd nyissa meg a **az Azure AD B2C** panelen.</span><span class="sxs-lookup"><span data-stu-id="fa97a-180">In the [Azure portal](https://portal.azure.com), [switch into the context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then open the **Azure AD B2C** blade.</span></span>
2. <span data-ttu-id="fa97a-181">Kattintson a **házirend**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-181">Click **All Policies**.</span></span>
3. <span data-ttu-id="fa97a-182">Kattintson a **házirend feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-182">Click **Upload Policy**.</span></span>
4. <span data-ttu-id="fa97a-183">Töltse fel `SignUpOrSignin.xml` rendelkező a  *\<ContentDefinitions\>*  címke korábban hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="fa97a-183">Upload `SignUpOrSignin.xml` with the *\<ContentDefinitions\>* tag that you added previously.</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="fa97a-184">Az egyéni házirend tesztelése **futtatása most**</span><span class="sxs-lookup"><span data-stu-id="fa97a-184">Test the custom policy by using **Run now**</span></span>

1. <span data-ttu-id="fa97a-185">Az a **az Azure AD B2C** paneljén lépjen **összes házirendek**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-185">On the **Azure AD B2C** blade, go to **All polices**.</span></span>
2. <span data-ttu-id="fa97a-186">Válassza ki az egyéni házirendet, feltöltött, majd kattintson a **futtatása most** gombra.</span><span class="sxs-lookup"><span data-stu-id="fa97a-186">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
3. <span data-ttu-id="fa97a-187">Az e-mail címükkel iratkozhat fel kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fa97a-187">You should be able to sign up by using an email address.</span></span>

## <a name="reference"></a><span data-ttu-id="fa97a-188">Referencia</span><span class="sxs-lookup"><span data-stu-id="fa97a-188">Reference</span></span>

<span data-ttu-id="fa97a-189">Minta sablonok a felhasználói felület testreszabásával itt található:</span><span class="sxs-lookup"><span data-stu-id="fa97a-189">You can find sample templates for UI customization here:</span></span>

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

<span data-ttu-id="fa97a-190">A sample_templates/wingtip mappa a következő HTML-fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="fa97a-190">The sample_templates/wingtip folder contains the following HTML files:</span></span>

| <span data-ttu-id="fa97a-191">HTML5-sablon</span><span class="sxs-lookup"><span data-stu-id="fa97a-191">HTML5 template</span></span> | <span data-ttu-id="fa97a-192">Leírás</span><span class="sxs-lookup"><span data-stu-id="fa97a-192">Description</span></span> |
|----------------|-------------|
| <span data-ttu-id="fa97a-193">*phonefactor.HTML*</span><span class="sxs-lookup"><span data-stu-id="fa97a-193">*phonefactor.html*</span></span> | <span data-ttu-id="fa97a-194">Használja ezt a fájlt egy sablont a többtényezős hitelesítés lap.</span><span class="sxs-lookup"><span data-stu-id="fa97a-194">Use this file as a template for a multi-factor authentication page.</span></span> |
| <span data-ttu-id="fa97a-195">*ResetPassword.HTML*</span><span class="sxs-lookup"><span data-stu-id="fa97a-195">*resetpassword.html*</span></span> | <span data-ttu-id="fa97a-196">Használja ezt a fájlt a sablont egy elfelejtett jelszó lap.</span><span class="sxs-lookup"><span data-stu-id="fa97a-196">Use this file as a template for a forgot password page.</span></span> |
| <span data-ttu-id="fa97a-197">*selfasserted.HTML*</span><span class="sxs-lookup"><span data-stu-id="fa97a-197">*selfasserted.html*</span></span> | <span data-ttu-id="fa97a-198">Használja ezt a fájlt egy közösségi fiók regisztrációs oldalon, a helyi fiók előfizetéshez vagy egy helyi fiók bejelentkezési oldalának sablont.</span><span class="sxs-lookup"><span data-stu-id="fa97a-198">Use this file as a template for a social account sign-up page, a local account sign-up page, or a local account sign-in page.</span></span> |
| <span data-ttu-id="fa97a-199">*Unified.HTML*</span><span class="sxs-lookup"><span data-stu-id="fa97a-199">*unified.html*</span></span> | <span data-ttu-id="fa97a-200">Használja ezt a fájlt egy egységes regisztráció vagy bejelentkezés lap sablont.</span><span class="sxs-lookup"><span data-stu-id="fa97a-200">Use this file as a template for a unified sign-up or sign-in page.</span></span> |
| <span data-ttu-id="fa97a-201">*updateprofile.HTML*</span><span class="sxs-lookup"><span data-stu-id="fa97a-201">*updateprofile.html*</span></span> | <span data-ttu-id="fa97a-202">Használja ezt a fájlt egy profil frissítés lap sablont.</span><span class="sxs-lookup"><span data-stu-id="fa97a-202">Use this file as a template for a profile update page.</span></span> |

<span data-ttu-id="fa97a-203">Az a [módosítása a regisztráció vagy bejelentkezés egyéni házirend szakasz](#modify-your-sign-up-or-sign-in-custom-policy), konfigurálta a tartalom definíciója `api.idpselections`.</span><span class="sxs-lookup"><span data-stu-id="fa97a-203">In the [Modify your sign-up or sign-in custom policy section](#modify-your-sign-up-or-sign-in-custom-policy), you configured the content definition for `api.idpselections`.</span></span> <span data-ttu-id="fa97a-204">A tartalom teljes készletének meghatározása az azonosítókat, és azok leírásait tartalmazza az Azure AD B2C identitás élmény keretében felismeri az alábbi táblázatban vannak:</span><span class="sxs-lookup"><span data-stu-id="fa97a-204">The full set of content definition IDs that are recognized by the Azure AD B2C identity experience framework and their descriptions are in the following table:</span></span>

| <span data-ttu-id="fa97a-205">Tartalmi azonosító</span><span class="sxs-lookup"><span data-stu-id="fa97a-205">Content definition ID</span></span> | <span data-ttu-id="fa97a-206">Leírás</span><span class="sxs-lookup"><span data-stu-id="fa97a-206">Description</span></span> | 
|-----------------------|-------------|
| <span data-ttu-id="fa97a-207">*API.Error*</span><span class="sxs-lookup"><span data-stu-id="fa97a-207">*api.error*</span></span> | <span data-ttu-id="fa97a-208">**Hibalap**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-208">**Error page**.</span></span> <span data-ttu-id="fa97a-209">Ezen a lapon megjelenik, ha kivétel, vagy hiba történt.</span><span class="sxs-lookup"><span data-stu-id="fa97a-209">This page is displayed when an exception or an error is encountered.</span></span> |
| <span data-ttu-id="fa97a-210">*API.idpselections*</span><span class="sxs-lookup"><span data-stu-id="fa97a-210">*api.idpselections*</span></span> | <span data-ttu-id="fa97a-211">**Identitás-szolgáltató kiválasztása lapon**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-211">**Identity provider selection page**.</span></span> <span data-ttu-id="fa97a-212">Ezen a lapon a bejelentkezés során a felhasználó választhat az identitás-szolgáltatóktól listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fa97a-212">This page contains a list of identity providers that the user can choose from during sign-in.</span></span> <span data-ttu-id="fa97a-213">Ezek a beállítások esetén a vállalati identitás-szolgáltatóktól, például a Facebookhoz és a Google + közösségi Identitásszolgáltatók, vagy helyi fiók.</span><span class="sxs-lookup"><span data-stu-id="fa97a-213">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="fa97a-214">*API.idpselections.Signup*</span><span class="sxs-lookup"><span data-stu-id="fa97a-214">*api.idpselections.signup*</span></span> | <span data-ttu-id="fa97a-215">**Identity provider adatgyűjtésre vonatkozó felhasználói előfizetési**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-215">**Identity provider selection for sign-up**.</span></span> <span data-ttu-id="fa97a-216">Ezen a lapon a regisztráció során a felhasználó választhat az identitás-szolgáltatóktól listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fa97a-216">This page contains a list of identity providers that the user can choose from during sign-up.</span></span> <span data-ttu-id="fa97a-217">Ezek a beállítások esetén a vállalati identitás-szolgáltatóktól, például a Facebookhoz és a Google + közösségi Identitásszolgáltatók, vagy helyi fiók.</span><span class="sxs-lookup"><span data-stu-id="fa97a-217">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="fa97a-218">*API.localaccountpasswordreset*</span><span class="sxs-lookup"><span data-stu-id="fa97a-218">*api.localaccountpasswordreset*</span></span> | <span data-ttu-id="fa97a-219">**Elfelejtett jelszó lap**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-219">**Forgot password page**.</span></span> <span data-ttu-id="fa97a-220">Ez a lap tartalmaz egy képernyőn a felhasználó által végrehajtandó kezdeményezheti a jelszó alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="fa97a-220">This page contains a form that the user must complete to initiate a password reset.</span></span>  |
| <span data-ttu-id="fa97a-221">*API.localaccountsignin*</span><span class="sxs-lookup"><span data-stu-id="fa97a-221">*api.localaccountsignin*</span></span> | <span data-ttu-id="fa97a-222">**Helyi fiók bejelentkezési oldalának**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-222">**Local account sign-in page**.</span></span> <span data-ttu-id="fa97a-223">Ez a lap tartalmaz egy helyi fiók, az e-mail címet vagy egy felhasználónevet alapuló bejelentkezést a bejelentkezési űrlap.</span><span class="sxs-lookup"><span data-stu-id="fa97a-223">This page contains a sign-in form for signing in with a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="fa97a-224">Az űrlap egy bemeneti szövegmező és a jelszó mező tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="fa97a-224">The form can contain a text input box and password entry box.</span></span> |
| <span data-ttu-id="fa97a-225">*API.localaccountsignup*</span><span class="sxs-lookup"><span data-stu-id="fa97a-225">*api.localaccountsignup*</span></span> | <span data-ttu-id="fa97a-226">**Helyi fiók előfizetéshez**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-226">**Local account sign-up page**.</span></span> <span data-ttu-id="fa97a-227">Ezen a lapon található regisztrációs űrlap iratkozik fel egy helyi fiók, amely egy e-mail címet vagy egy felhasználónevet alapul.</span><span class="sxs-lookup"><span data-stu-id="fa97a-227">This page contains a sign-up form for signing up for a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="fa97a-228">Az űrlap különböző bemeneti vezérlők, például szöveg beviteli mezőt, a jelszó mező, választógomb, egyetlen legördülő listák és a többszörös kiválasztási jelölőnégyzetek is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="fa97a-228">The form can contain various input controls, such as a text input box, a password entry box, a radio button, single-select drop-down boxes, and multi-select check boxes.</span></span> |
| <span data-ttu-id="fa97a-229">*API.phonefactor*</span><span class="sxs-lookup"><span data-stu-id="fa97a-229">*api.phonefactor*</span></span> | <span data-ttu-id="fa97a-230">**Többtényezős hitelesítés lap**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-230">**Multi-factor authentication page**.</span></span> <span data-ttu-id="fa97a-231">Ezen a lapon, a felhasználók a telefonszámok (segítségével ellenőrizheti szöveges vagy hangos) regisztráció vagy bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="fa97a-231">On this page, users can verify their phone numbers (by using text or voice) during sign-up or sign-in.</span></span> |
| <span data-ttu-id="fa97a-232">*API.selfasserted*</span><span class="sxs-lookup"><span data-stu-id="fa97a-232">*api.selfasserted*</span></span> | <span data-ttu-id="fa97a-233">**Közösségi fiók bejelentkezési oldalának**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-233">**Social account sign-up page**.</span></span> <span data-ttu-id="fa97a-234">Ezen a lapon, hogy a felhasználók kell végeznie, amikor azok az egy meglévő fiókkal a Facebook-on vagy a Google + például közösségi identitásszolgáltató regisztráljon regisztrációs űrlap tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fa97a-234">This page contains a sign-up form that users must complete when they sign up by using an existing account from a social identity provider such as Facebook or Google+.</span></span> <span data-ttu-id="fa97a-235">Ezen a lapon hasonlít az előző közösségi fiók regisztrációs oldalon, a jelszó számbeviteli mezők kivételével.</span><span class="sxs-lookup"><span data-stu-id="fa97a-235">This page is similar to the preceding social account sign-up page, except for the password entry fields.</span></span> |
| <span data-ttu-id="fa97a-236">*API.selfasserted.profileupdate*</span><span class="sxs-lookup"><span data-stu-id="fa97a-236">*api.selfasserted.profileupdate*</span></span> | <span data-ttu-id="fa97a-237">**Profil update lapon**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-237">**Profile update page**.</span></span> <span data-ttu-id="fa97a-238">Ezen a lapon, hogy a felhasználók használhatják a profil frissítése űrlap tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fa97a-238">This page contains a form that users can use to update their profile.</span></span> <span data-ttu-id="fa97a-239">Ezen a lapon hasonlít a közösségi fiók regisztrációs oldalon, a jelszó számbeviteli mezők kivételével.</span><span class="sxs-lookup"><span data-stu-id="fa97a-239">This page is similar to the social account sign-up page, except for the password entry fields.</span></span> |
| <span data-ttu-id="fa97a-240">*API.signuporsignin*</span><span class="sxs-lookup"><span data-stu-id="fa97a-240">*api.signuporsignin*</span></span> | <span data-ttu-id="fa97a-241">**Egyesített előfizetési vagy a bejelentkezési oldal**.</span><span class="sxs-lookup"><span data-stu-id="fa97a-241">**Unified sign-up or sign-in page**.</span></span> <span data-ttu-id="fa97a-242">Ezen a lapon a regisztráció és bejelentkezés a felhasználók, akik vállalati identitás-szolgáltatóktól, például a Facebook-on vagy a Google + és helyi fiókok közösségi Identitásszolgáltatók kezeli.</span><span class="sxs-lookup"><span data-stu-id="fa97a-242">This page handles both the sign-up and sign-in of users, who can use enterprise identity providers, social identity providers such as Facebook or Google+, or local accounts.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="fa97a-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fa97a-243">Next steps</span></span>

<span data-ttu-id="fa97a-244">További információ a testre szabható felhasználói felületi elemeket: [beépített házirendek felhasználói felület testreszabása a referencia-útmutató](active-directory-b2c-reference-ui-customization.md).</span><span class="sxs-lookup"><span data-stu-id="fa97a-244">For additional information about UI elements that can be customized, see [reference guide for UI customization for built-in policies](active-directory-b2c-reference-ui-customization.md).</span></span>
