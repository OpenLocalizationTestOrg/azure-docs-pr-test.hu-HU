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
ms.openlocfilehash: 6f00995e54c9f9ef27cc51e38f3de07cd5817cc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a><span data-ttu-id="7a158-103">Az Azure Active Directory B2C: Egyéni házirendek konfigurálása a felhasználói felület testreszabása</span><span class="sxs-lookup"><span data-stu-id="7a158-103">Azure Active Directory B2C: Configure UI customization in a custom policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="7a158-104">Ez a cikk befejezése után fog regisztráció és bejelentkezés egyéni házirendet a márka és megjelenését.</span><span class="sxs-lookup"><span data-stu-id="7a158-104">After you complete this article, you will have a sign-up and sign-in custom policy with your brand and appearance.</span></span> <span data-ttu-id="7a158-105">Azure Active Directory B2C (az Azure AD B2C), kapott szinte teljes körű hozzáférést engedélyezzenek hello HTML- és CSS tartalom, amely toousers bemutatott.</span><span class="sxs-lookup"><span data-stu-id="7a158-105">With Azure Active Directory B2C (Azure AD B2C), you get nearly full control of hello HTML and CSS content that's presented toousers.</span></span> <span data-ttu-id="7a158-106">Egyéni házirend használata esetén konfigurálnia felhasználói felület testreszabásával XML-vezérlők használata az Azure-portálon hello helyett.</span><span class="sxs-lookup"><span data-stu-id="7a158-106">When you use a custom policy, you configure UI customization in XML instead of using controls in hello Azure portal.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7a158-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7a158-107">Prerequisites</span></span>

<span data-ttu-id="7a158-108">Mielőtt elkezdené, fejezze be a [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="7a158-108">Before you begin, complete [Getting started with custom policies](active-directory-b2c-get-started-custom.md).</span></span> <span data-ttu-id="7a158-109">A regisztrációt, és jelentkezzen be helyi fiókok működő egyéni házirendet kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7a158-109">You should have a working custom policy for sign-up and sign-in with local accounts.</span></span>

## <a name="page-ui-customization"></a><span data-ttu-id="7a158-110">Lap felhasználói felületének testreszabása</span><span class="sxs-lookup"><span data-stu-id="7a158-110">Page UI customization</span></span>

<span data-ttu-id="7a158-111">Hello lap felhasználói felületének testreszabása szolgáltatás segítségével testre szabhatja a hello megjelenésének és arculatának bármilyen egyéni házirendet.</span><span class="sxs-lookup"><span data-stu-id="7a158-111">By using hello page UI customization feature, you can customize hello look and feel of any custom policy.</span></span> <span data-ttu-id="7a158-112">Kezelheti a márka és egységes megjelenést az alkalmazás és az Azure AD B2C között is.</span><span class="sxs-lookup"><span data-stu-id="7a158-112">You can also maintain brand and visual consistency between your application and Azure AD B2C.</span></span>

<span data-ttu-id="7a158-113">Hogyan működik ez: Azure AD B2C az ügyfél böngészőjében kód fut, és a modern megközelítés nevű [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="7a158-113">Here's how it works: Azure AD B2C runs code in your customer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span></span> <span data-ttu-id="7a158-114">Első lépésként ad meg URL-címet hello egyéni házirendekben a testreszabott HTML-tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="7a158-114">First, you specify a URL in hello custom policy with customized HTML content.</span></span> <span data-ttu-id="7a158-115">Az Azure AD B2C egyesíti a HTML-tartalmakat, amely be töltve az URL-címről, és megjeleníti hello lap toohello ügyfél hello felhasználói felületi elemei.</span><span class="sxs-lookup"><span data-stu-id="7a158-115">Azure AD B2C merges UI elements with hello HTML content that's loaded from your URL and then displays hello page toohello customer.</span></span>

## <a name="create-your-html5-content"></a><span data-ttu-id="7a158-116">A HTML5 tartalmának létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a158-116">Create your HTML5 content</span></span>

<span data-ttu-id="7a158-117">Hozzon létre HTML tartalom a termék márkáját nevű hello cím.</span><span class="sxs-lookup"><span data-stu-id="7a158-117">Create HTML content with your product's brand name in hello title.</span></span>

1. <span data-ttu-id="7a158-118">A következő HTML részlet hello másolja.</span><span class="sxs-lookup"><span data-stu-id="7a158-118">Copy hello following HTML snippet.</span></span> <span data-ttu-id="7a158-119">Szabályos üres elem a HTML5 nevű  *\<div id = "api"\>\</div\>*  hello belül található  *\<törzs\>*  címkék.</span><span class="sxs-lookup"><span data-stu-id="7a158-119">It is well-formed HTML5 with an empty element called *\<div id="api"\>\</div\>* located within hello *\<body\>* tags.</span></span> <span data-ttu-id="7a158-120">Ez az elem azt jelzi, ha az Azure AD B2C tartalom toobe beszúrni.</span><span class="sxs-lookup"><span data-stu-id="7a158-120">This element indicates where Azure AD B2C content is toobe inserted.</span></span>

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
   ><span data-ttu-id="7a158-121">Biztonsági okokból hello JavaScript jelenleg blokkolva van a Testreszabás.</span><span class="sxs-lookup"><span data-stu-id="7a158-121">For security reasons, hello use of JavaScript is currently blocked for customization.</span></span>

2. <span data-ttu-id="7a158-122">Illessze be a másolt hello részlet egy szövegszerkesztőben, és mentse a fájlt hello *testreszabása ui.html*.</span><span class="sxs-lookup"><span data-stu-id="7a158-122">Paste hello copied snippet in a text editor, and then save hello file as *customize-ui.html*.</span></span>

## <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="7a158-123">Egy Azure Blob storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a158-123">Create an Azure Blob storage account</span></span>

>[!NOTE]
> <span data-ttu-id="7a158-124">Ebben a cikkben használjuk Azure Blob storage toohost tartalmat.</span><span class="sxs-lookup"><span data-stu-id="7a158-124">In this article, we use Azure Blob storage toohost our content.</span></span> <span data-ttu-id="7a158-125">Kiválaszthatja toohost a tartalmat a webkiszolgálón, de meg kell [CORS engedélyezése a webkiszolgálón](https://enable-cors.org/server.html).</span><span class="sxs-lookup"><span data-stu-id="7a158-125">You can choose toohost your content on a web server, but you must [enable CORS on your web server](https://enable-cors.org/server.html).</span></span>

<span data-ttu-id="7a158-126">toohost hello a Blob Storage tárolóban, a HTML-tartalmakat, a következő:</span><span class="sxs-lookup"><span data-stu-id="7a158-126">toohost this HTML content in Blob storage, do hello following:</span></span>

1. <span data-ttu-id="7a158-127">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7a158-127">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7a158-128">A hello **Hub** menüjében válassza **új** > **tárolási** > **tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="7a158-128">On hello **Hub** menu, select **New** > **Storage** > **Storage account**.</span></span>
3. <span data-ttu-id="7a158-129">Adjon meg egy egyedi **neve** a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7a158-129">Enter a unique **Name** for your storage account.</span></span>
4. <span data-ttu-id="7a158-130">**Telepítési modell** maradjanak **erőforrás-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="7a158-130">**Deployment model** can remain **Resource Manager**.</span></span>
5. <span data-ttu-id="7a158-131">Változás **fiók típusa** túl**Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="7a158-131">Change **Account Kind** too**Blob storage**.</span></span>
6. <span data-ttu-id="7a158-132">**Teljesítmény** maradjanak **szabványos**.</span><span class="sxs-lookup"><span data-stu-id="7a158-132">**Performance** can remain **Standard**.</span></span>
7. <span data-ttu-id="7a158-133">**Replikációs** maradjanak **RA-GRS**.</span><span class="sxs-lookup"><span data-stu-id="7a158-133">**Replication** can remain **RA-GRS**.</span></span>
8. <span data-ttu-id="7a158-134">**Hozzáférési szint** maradjanak **gyakran használt adatok**.</span><span class="sxs-lookup"><span data-stu-id="7a158-134">**Access tier** can remain **Hot**.</span></span>
9. <span data-ttu-id="7a158-135">**Storage szolgáltatás titkosítási** maradjanak **letiltott**.</span><span class="sxs-lookup"><span data-stu-id="7a158-135">**Storage service encryption** can remain **Disabled**.</span></span>
10. <span data-ttu-id="7a158-136">Válassza ki a **előfizetés** a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7a158-136">Select a **Subscription** for your storage account.</span></span>
11. <span data-ttu-id="7a158-137">Hozzon létre egy **erőforráscsoport** vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="7a158-137">Create a **Resource group** or select an existing one.</span></span>
12. <span data-ttu-id="7a158-138">Jelölje be hello **földrajzi hely** a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7a158-138">Select hello **Geographic location** for your storage account.</span></span>
13. <span data-ttu-id="7a158-139">Kattintson a **létrehozása** toocreate hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7a158-139">Click **Create** toocreate hello storage account.</span></span>  
    <span data-ttu-id="7a158-140">Hello központi telepítés befejezése után hello **tárfiók** panel nyílik meg automatikusan.</span><span class="sxs-lookup"><span data-stu-id="7a158-140">After hello deployment is completed, hello **Storage account** blade opens automatically.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="7a158-141">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a158-141">Create a container</span></span>

<span data-ttu-id="7a158-142">a Blob Storage tárolóban, nyilvános tárolókban toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7a158-142">toocreate a public container in Blob storage, do hello following:</span></span>

1. <span data-ttu-id="7a158-143">Kattintson a hello **áttekintése** fülre.</span><span class="sxs-lookup"><span data-stu-id="7a158-143">Click hello **Overview** tab.</span></span>
2. <span data-ttu-id="7a158-144">Kattintson a **tároló**.</span><span class="sxs-lookup"><span data-stu-id="7a158-144">Click **Container**.</span></span>
3. <span data-ttu-id="7a158-145">A **neve**, típus **$root**.</span><span class="sxs-lookup"><span data-stu-id="7a158-145">For **Name**, type **$root**.</span></span>
4. <span data-ttu-id="7a158-146">Állítsa be **hozzáférési típus** túl**Blob**.</span><span class="sxs-lookup"><span data-stu-id="7a158-146">Set **Access type** too**Blob**.</span></span>
5. <span data-ttu-id="7a158-147">Kattintson a **$root** tooopen hello új tároló.</span><span class="sxs-lookup"><span data-stu-id="7a158-147">Click **$root** tooopen hello new container.</span></span>
6. <span data-ttu-id="7a158-148">Kattintson a **Feltöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a158-148">Click **Upload**.</span></span>
7. <span data-ttu-id="7a158-149">Kattintson a hello ikonja tovább túl**válasszon ki egy fájlt**.</span><span class="sxs-lookup"><span data-stu-id="7a158-149">Click hello folder icon next too**Select a file**.</span></span>
8. <span data-ttu-id="7a158-150">Nyissa meg túl**testreszabása ui.html**, amely hello korábban létrehozott [lap felhasználói felületének testreszabása](#the-page-ui-customization-feature) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7a158-150">Go too**customize-ui.html**, which you created earlier in hello [Page UI customization](#the-page-ui-customization-feature) section.</span></span>
9. <span data-ttu-id="7a158-151">Kattintson a **Feltöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a158-151">Click **Upload**.</span></span>
10. <span data-ttu-id="7a158-152">Válassza ki a feltöltött hello testreszabása ui.html blob.</span><span class="sxs-lookup"><span data-stu-id="7a158-152">Select hello customize-ui.html blob that you uploaded.</span></span>
11. <span data-ttu-id="7a158-153">Következő túl**URL-cím**, kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="7a158-153">Next too**URL**, click **Copy**.</span></span>
12. <span data-ttu-id="7a158-154">A böngészőben illessze be a másolt hello URL-címet, és toohello található.</span><span class="sxs-lookup"><span data-stu-id="7a158-154">In a browser, paste hello copied URL, and go toohello site.</span></span> <span data-ttu-id="7a158-155">Hello hely nem érhető el, ha állítson hello tároló hozzáférés típusa túl**blob**.</span><span class="sxs-lookup"><span data-stu-id="7a158-155">If hello site is inaccessible, make sure hello container access type is set too**blob**.</span></span>

## <a name="configure-cors"></a><span data-ttu-id="7a158-156">A CORS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a158-156">Configure CORS</span></span>

<span data-ttu-id="7a158-157">A Blob storage konfigurálásához az eltérő eredetű erőforrások megosztása hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="7a158-157">Configure Blob storage for Cross-Origin Resource Sharing by doing hello following:</span></span>

>[!NOTE]
><span data-ttu-id="7a158-158">Szeretné, hogy ki hello felhasználói felületi testreszabási szolgáltatás tootry minta HTML- és CSS tartalmat használatával?</span><span class="sxs-lookup"><span data-stu-id="7a158-158">Want tootry out hello UI customization feature by using our sample HTML and CSS content?</span></span> <span data-ttu-id="7a158-159">Mellékelt [egy egyszerű segédeszköze](active-directory-b2c-reference-ui-customization-helper-tool.md) , amely feltölti és konfigurálja a minta tartalmat a Blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="7a158-159">We've provided [a simple helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) that uploads and configures our sample content on your Blob storage account.</span></span> <span data-ttu-id="7a158-160">Ha hello eszközt használja, hagyja ki azokat, amelyek túl[módosítsa a regisztráció vagy bejelentkezés egyéni házirendet](#modify-your-sign-up-or-sign-in-custom-policy).</span><span class="sxs-lookup"><span data-stu-id="7a158-160">If you use hello tool, skip ahead too[Modify your sign-up or sign-in custom policy](#modify-your-sign-up-or-sign-in-custom-policy).</span></span>

1. <span data-ttu-id="7a158-161">A hello **tárolási** panel alatt **beállítások**, nyissa meg **CORS**.</span><span class="sxs-lookup"><span data-stu-id="7a158-161">On hello **Storage** blade, under **Settings**, open **CORS**.</span></span>
2. <span data-ttu-id="7a158-162">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="7a158-162">Click **Add**.</span></span>
3. <span data-ttu-id="7a158-163">A **engedélyezett eredeteket**, írja be egy csillag (\*).</span><span class="sxs-lookup"><span data-stu-id="7a158-163">For **Allowed origins**, type an asterisk (\*).</span></span>
4. <span data-ttu-id="7a158-164">A hello **engedélyezett műveletek** legördülő listából válassza ki, válassza ki mindkét **beolvasása** és **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7a158-164">In hello **Allowed verbs** drop-down list, select both **GET** and **OPTIONS**.</span></span>
5. <span data-ttu-id="7a158-165">A **engedélyezett fejlécek**, írja be egy csillag (\*).</span><span class="sxs-lookup"><span data-stu-id="7a158-165">For **Allowed headers**, type an asterisk (\*).</span></span>
6. <span data-ttu-id="7a158-166">A **fejlécek kitett**, írja be egy csillag (\*).</span><span class="sxs-lookup"><span data-stu-id="7a158-166">For **Exposed headers**, type an asterisk (\*).</span></span>
7. <span data-ttu-id="7a158-167">A **maximális élettartama (másodperc)**, típus **200**.</span><span class="sxs-lookup"><span data-stu-id="7a158-167">For **Maximum age (seconds)**, type **200**.</span></span>
8. <span data-ttu-id="7a158-168">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="7a158-168">Click **Add**.</span></span>

## <a name="test-cors"></a><span data-ttu-id="7a158-169">A CORS tesztelése</span><span class="sxs-lookup"><span data-stu-id="7a158-169">Test CORS</span></span>

<span data-ttu-id="7a158-170">Ellenőrizze, hogy készen áll hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="7a158-170">Validate that you're ready by doing hello following:</span></span>

1. <span data-ttu-id="7a158-171">Toohello Ugrás [teszt-cors.org](http://test-cors.org/) webhelyet, és beillesztés hello URL-címet hello **távoli URL-cím** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="7a158-171">Go toohello [test-cors.org](http://test-cors.org/) website, and then paste hello URL in hello **Remote URL** box.</span></span>
2. <span data-ttu-id="7a158-172">Kattintson a **kérés küldése**.</span><span class="sxs-lookup"><span data-stu-id="7a158-172">Click **Send Request**.</span></span>  
    <span data-ttu-id="7a158-173">Ha hibaüzenetet kap, ellenőrizze, hogy a [CORS-beállítások](#configure-cors) helyes-e.</span><span class="sxs-lookup"><span data-stu-id="7a158-173">If you receive an error, make sure that your [CORS settings](#configure-cors) are correct.</span></span> <span data-ttu-id="7a158-174">Előfordulhat, hogy tooclear kell a gyorsítótárban, vagy nyisson meg egy InPrivate-böngészés munkamenet Ctrl + Shift + P lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="7a158-174">You might also need tooclear your browser cache or open an in-private browsing session by pressing Ctrl+Shift+P.</span></span>

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a><span data-ttu-id="7a158-175">A regisztráció vagy bejelentkezés egyéni házirend módosítása</span><span class="sxs-lookup"><span data-stu-id="7a158-175">Modify your sign-up or sign-in custom policy</span></span>

<span data-ttu-id="7a158-176">A legfelső szintű hello  *\<TrustFrameworkPolicy\>*  címke, keresse meg  *\<BuildingBlocks\>*  címke.</span><span class="sxs-lookup"><span data-stu-id="7a158-176">Under hello top-level *\<TrustFrameworkPolicy\>* tag, you should find *\<BuildingBlocks\>* tag.</span></span> <span data-ttu-id="7a158-177">Hello belül  *\<BuildingBlocks\>*  címkék, vegye fel a  *\<ContentDefinitions\>*  címke a következő példa hello másolásával.</span><span class="sxs-lookup"><span data-stu-id="7a158-177">Within hello *\<BuildingBlocks\>* tags, add a *\<ContentDefinitions\>* tag by copying hello following example.</span></span> <span data-ttu-id="7a158-178">Cserélje le *your_storage_account* hello a tárfiók nevére.</span><span class="sxs-lookup"><span data-stu-id="7a158-178">Replace *your_storage_account* with hello name of your storage account.</span></span>

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a><span data-ttu-id="7a158-179">Töltse fel a frissített egyéni házirend</span><span class="sxs-lookup"><span data-stu-id="7a158-179">Upload your updated custom policy</span></span>

1. <span data-ttu-id="7a158-180">A hello [Azure-portálon](https://portal.azure.com), [hello keretében az Azure AD B2C-bérlő átváltani](active-directory-b2c-navigate-to-b2c-context.md), majd nyissa meg a hello **az Azure AD B2C** panelen.</span><span class="sxs-lookup"><span data-stu-id="7a158-180">In hello [Azure portal](https://portal.azure.com), [switch into hello context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then open hello **Azure AD B2C** blade.</span></span>
2. <span data-ttu-id="7a158-181">Kattintson a **házirend**.</span><span class="sxs-lookup"><span data-stu-id="7a158-181">Click **All Policies**.</span></span>
3. <span data-ttu-id="7a158-182">Kattintson a **házirend feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="7a158-182">Click **Upload Policy**.</span></span>
4. <span data-ttu-id="7a158-183">Töltse fel `SignUpOrSignin.xml` a hello  *\<ContentDefinitions\>*  címke korábban hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="7a158-183">Upload `SignUpOrSignin.xml` with hello *\<ContentDefinitions\>* tag that you added previously.</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="7a158-184">Tesztelése hello egyéni házirend használatával **futtatása most**</span><span class="sxs-lookup"><span data-stu-id="7a158-184">Test hello custom policy by using **Run now**</span></span>

1. <span data-ttu-id="7a158-185">A hello **az Azure AD B2C** panelen, lépjen túl**összes házirendek**.</span><span class="sxs-lookup"><span data-stu-id="7a158-185">On hello **Azure AD B2C** blade, go too**All polices**.</span></span>
2. <span data-ttu-id="7a158-186">Válassza ki a feltöltött hello egyéni házirendet, majd kattintson a hello **futtatása most** gombra.</span><span class="sxs-lookup"><span data-stu-id="7a158-186">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
3. <span data-ttu-id="7a158-187">Meg kell képes toosign fel e-mail cím használatával.</span><span class="sxs-lookup"><span data-stu-id="7a158-187">You should be able toosign up by using an email address.</span></span>

## <a name="reference"></a><span data-ttu-id="7a158-188">Referencia</span><span class="sxs-lookup"><span data-stu-id="7a158-188">Reference</span></span>

<span data-ttu-id="7a158-189">Minta sablonok a felhasználói felület testreszabásával itt található:</span><span class="sxs-lookup"><span data-stu-id="7a158-189">You can find sample templates for UI customization here:</span></span>

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

<span data-ttu-id="7a158-190">hello sample_templates/wingtip mappa a következő HTML-fájlok hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7a158-190">hello sample_templates/wingtip folder contains hello following HTML files:</span></span>

| <span data-ttu-id="7a158-191">HTML5-sablon</span><span class="sxs-lookup"><span data-stu-id="7a158-191">HTML5 template</span></span> | <span data-ttu-id="7a158-192">Leírás</span><span class="sxs-lookup"><span data-stu-id="7a158-192">Description</span></span> |
|----------------|-------------|
| <span data-ttu-id="7a158-193">*phonefactor.HTML*</span><span class="sxs-lookup"><span data-stu-id="7a158-193">*phonefactor.html*</span></span> | <span data-ttu-id="7a158-194">Használja ezt a fájlt egy sablont a többtényezős hitelesítés lap.</span><span class="sxs-lookup"><span data-stu-id="7a158-194">Use this file as a template for a multi-factor authentication page.</span></span> |
| <span data-ttu-id="7a158-195">*ResetPassword.HTML*</span><span class="sxs-lookup"><span data-stu-id="7a158-195">*resetpassword.html*</span></span> | <span data-ttu-id="7a158-196">Használja ezt a fájlt a sablont egy elfelejtett jelszó lap.</span><span class="sxs-lookup"><span data-stu-id="7a158-196">Use this file as a template for a forgot password page.</span></span> |
| <span data-ttu-id="7a158-197">*selfasserted.HTML*</span><span class="sxs-lookup"><span data-stu-id="7a158-197">*selfasserted.html*</span></span> | <span data-ttu-id="7a158-198">Használja ezt a fájlt egy közösségi fiók regisztrációs oldalon, a helyi fiók előfizetéshez vagy egy helyi fiók bejelentkezési oldalának sablont.</span><span class="sxs-lookup"><span data-stu-id="7a158-198">Use this file as a template for a social account sign-up page, a local account sign-up page, or a local account sign-in page.</span></span> |
| <span data-ttu-id="7a158-199">*Unified.HTML*</span><span class="sxs-lookup"><span data-stu-id="7a158-199">*unified.html*</span></span> | <span data-ttu-id="7a158-200">Használja ezt a fájlt egy egységes regisztráció vagy bejelentkezés lap sablont.</span><span class="sxs-lookup"><span data-stu-id="7a158-200">Use this file as a template for a unified sign-up or sign-in page.</span></span> |
| <span data-ttu-id="7a158-201">*updateprofile.HTML*</span><span class="sxs-lookup"><span data-stu-id="7a158-201">*updateprofile.html*</span></span> | <span data-ttu-id="7a158-202">Használja ezt a fájlt egy profil frissítés lap sablont.</span><span class="sxs-lookup"><span data-stu-id="7a158-202">Use this file as a template for a profile update page.</span></span> |

<span data-ttu-id="7a158-203">A hello [módosítása a regisztráció vagy bejelentkezés egyéni házirend szakasz](#modify-your-sign-up-or-sign-in-custom-policy), konfigurálta a tartalom definíciója hello `api.idpselections`.</span><span class="sxs-lookup"><span data-stu-id="7a158-203">In hello [Modify your sign-up or sign-in custom policy section](#modify-your-sign-up-or-sign-in-custom-policy), you configured hello content definition for `api.idpselections`.</span></span> <span data-ttu-id="7a158-204">hello teljes készletét tartalom definíció azonosítóját, amely felismeri a hello Azure AD B2C identitás élmény keretrendszer és azok leírásait tartalmazza a következő táblázat hello szerepelnek:</span><span class="sxs-lookup"><span data-stu-id="7a158-204">hello full set of content definition IDs that are recognized by hello Azure AD B2C identity experience framework and their descriptions are in hello following table:</span></span>

| <span data-ttu-id="7a158-205">Tartalmi azonosító</span><span class="sxs-lookup"><span data-stu-id="7a158-205">Content definition ID</span></span> | <span data-ttu-id="7a158-206">Leírás</span><span class="sxs-lookup"><span data-stu-id="7a158-206">Description</span></span> | 
|-----------------------|-------------|
| <span data-ttu-id="7a158-207">*API.Error*</span><span class="sxs-lookup"><span data-stu-id="7a158-207">*api.error*</span></span> | <span data-ttu-id="7a158-208">**Hibalap**.</span><span class="sxs-lookup"><span data-stu-id="7a158-208">**Error page**.</span></span> <span data-ttu-id="7a158-209">Ezen a lapon megjelenik, ha kivétel, vagy hiba történt.</span><span class="sxs-lookup"><span data-stu-id="7a158-209">This page is displayed when an exception or an error is encountered.</span></span> |
| <span data-ttu-id="7a158-210">*API.idpselections*</span><span class="sxs-lookup"><span data-stu-id="7a158-210">*api.idpselections*</span></span> | <span data-ttu-id="7a158-211">**Identitás-szolgáltató kiválasztása lapon**.</span><span class="sxs-lookup"><span data-stu-id="7a158-211">**Identity provider selection page**.</span></span> <span data-ttu-id="7a158-212">Ezen a lapon hello felhasználó-szolgáltatók közül választhatnak során bejelentkezési identitás listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7a158-212">This page contains a list of identity providers that hello user can choose from during sign-in.</span></span> <span data-ttu-id="7a158-213">Ezek a beállítások esetén a vállalati identitás-szolgáltatóktól, például a Facebookhoz és a Google + közösségi Identitásszolgáltatók, vagy helyi fiók.</span><span class="sxs-lookup"><span data-stu-id="7a158-213">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="7a158-214">*API.idpselections.Signup*</span><span class="sxs-lookup"><span data-stu-id="7a158-214">*api.idpselections.signup*</span></span> | <span data-ttu-id="7a158-215">**Identity provider adatgyűjtésre vonatkozó felhasználói előfizetési**.</span><span class="sxs-lookup"><span data-stu-id="7a158-215">**Identity provider selection for sign-up**.</span></span> <span data-ttu-id="7a158-216">Ezen a lapon hello felhasználó-szolgáltatók közül választhatnak regisztráció során identitás listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7a158-216">This page contains a list of identity providers that hello user can choose from during sign-up.</span></span> <span data-ttu-id="7a158-217">Ezek a beállítások esetén a vállalati identitás-szolgáltatóktól, például a Facebookhoz és a Google + közösségi Identitásszolgáltatók, vagy helyi fiók.</span><span class="sxs-lookup"><span data-stu-id="7a158-217">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="7a158-218">*API.localaccountpasswordreset*</span><span class="sxs-lookup"><span data-stu-id="7a158-218">*api.localaccountpasswordreset*</span></span> | <span data-ttu-id="7a158-219">**Elfelejtett jelszó lap**.</span><span class="sxs-lookup"><span data-stu-id="7a158-219">**Forgot password page**.</span></span> <span data-ttu-id="7a158-220">Ez a lap tartalmaz egy űrlap hello felhasználó jelszó-létrehozási tooinitiate kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="7a158-220">This page contains a form that hello user must complete tooinitiate a password reset.</span></span>  |
| <span data-ttu-id="7a158-221">*API.localaccountsignin*</span><span class="sxs-lookup"><span data-stu-id="7a158-221">*api.localaccountsignin*</span></span> | <span data-ttu-id="7a158-222">**Helyi fiók bejelentkezési oldalának**.</span><span class="sxs-lookup"><span data-stu-id="7a158-222">**Local account sign-in page**.</span></span> <span data-ttu-id="7a158-223">Ez a lap tartalmaz egy helyi fiók, az e-mail címet vagy egy felhasználónevet alapuló bejelentkezést a bejelentkezési űrlap.</span><span class="sxs-lookup"><span data-stu-id="7a158-223">This page contains a sign-in form for signing in with a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="7a158-224">hello űrlap is tartalmazhat, egy szöveges beviteli mezőbe, majd a jelszó mező.</span><span class="sxs-lookup"><span data-stu-id="7a158-224">hello form can contain a text input box and password entry box.</span></span> |
| <span data-ttu-id="7a158-225">*API.localaccountsignup*</span><span class="sxs-lookup"><span data-stu-id="7a158-225">*api.localaccountsignup*</span></span> | <span data-ttu-id="7a158-226">**Helyi fiók előfizetéshez**.</span><span class="sxs-lookup"><span data-stu-id="7a158-226">**Local account sign-up page**.</span></span> <span data-ttu-id="7a158-227">Ezen a lapon található regisztrációs űrlap iratkozik fel egy helyi fiók, amely egy e-mail címet vagy egy felhasználónevet alapul.</span><span class="sxs-lookup"><span data-stu-id="7a158-227">This page contains a sign-up form for signing up for a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="7a158-228">hello űrlap különböző bemeneti vezérlők, például szöveg beviteli mezőt, a jelszó mező, választógomb, egyetlen legördülő listák és a többszörös kiválasztási jelölőnégyzetek is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7a158-228">hello form can contain various input controls, such as a text input box, a password entry box, a radio button, single-select drop-down boxes, and multi-select check boxes.</span></span> |
| <span data-ttu-id="7a158-229">*API.phonefactor*</span><span class="sxs-lookup"><span data-stu-id="7a158-229">*api.phonefactor*</span></span> | <span data-ttu-id="7a158-230">**Többtényezős hitelesítés lap**.</span><span class="sxs-lookup"><span data-stu-id="7a158-230">**Multi-factor authentication page**.</span></span> <span data-ttu-id="7a158-231">Ezen a lapon, a felhasználók a telefonszámok (segítségével ellenőrizheti szöveges vagy hangos) regisztráció vagy bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="7a158-231">On this page, users can verify their phone numbers (by using text or voice) during sign-up or sign-in.</span></span> |
| <span data-ttu-id="7a158-232">*API.selfasserted*</span><span class="sxs-lookup"><span data-stu-id="7a158-232">*api.selfasserted*</span></span> | <span data-ttu-id="7a158-233">**Közösségi fiók bejelentkezési oldalának**.</span><span class="sxs-lookup"><span data-stu-id="7a158-233">**Social account sign-up page**.</span></span> <span data-ttu-id="7a158-234">Ezen a lapon, hogy a felhasználók kell végeznie, amikor azok az egy meglévő fiókkal a Facebook-on vagy a Google + például közösségi identitásszolgáltató regisztráljon regisztrációs űrlap tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7a158-234">This page contains a sign-up form that users must complete when they sign up by using an existing account from a social identity provider such as Facebook or Google+.</span></span> <span data-ttu-id="7a158-235">Ezen a lapon a hasonló toohello közösségi fiók előfizetéshez megelőző hello jelszó számbeviteli mezők kivételével.</span><span class="sxs-lookup"><span data-stu-id="7a158-235">This page is similar toohello preceding social account sign-up page, except for hello password entry fields.</span></span> |
| <span data-ttu-id="7a158-236">*API.selfasserted.profileupdate*</span><span class="sxs-lookup"><span data-stu-id="7a158-236">*api.selfasserted.profileupdate*</span></span> | <span data-ttu-id="7a158-237">**Profil update lapon**.</span><span class="sxs-lookup"><span data-stu-id="7a158-237">**Profile update page**.</span></span> <span data-ttu-id="7a158-238">Ezen a lapon, hogy felhasználók használhatják tooupdate a profilját űrlap tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7a158-238">This page contains a form that users can use tooupdate their profile.</span></span> <span data-ttu-id="7a158-239">Ezen a lapon a hasonló toohello közösségi fiók bejelentkezési lapon hello jelszó számbeviteli mezők kivételével.</span><span class="sxs-lookup"><span data-stu-id="7a158-239">This page is similar toohello social account sign-up page, except for hello password entry fields.</span></span> |
| <span data-ttu-id="7a158-240">*API.signuporsignin*</span><span class="sxs-lookup"><span data-stu-id="7a158-240">*api.signuporsignin*</span></span> | <span data-ttu-id="7a158-241">**Egyesített előfizetési vagy a bejelentkezési oldal**.</span><span class="sxs-lookup"><span data-stu-id="7a158-241">**Unified sign-up or sign-in page**.</span></span> <span data-ttu-id="7a158-242">Ezen a lapon mindkét hello előfizetési és jelentkezzen be a felhasználók, akik vállalati identitás-szolgáltatóktól, például a Facebook-on vagy a Google + és helyi fiókok közösségi Identitásszolgáltatók kezeli.</span><span class="sxs-lookup"><span data-stu-id="7a158-242">This page handles both hello sign-up and sign-in of users, who can use enterprise identity providers, social identity providers such as Facebook or Google+, or local accounts.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="7a158-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a158-243">Next steps</span></span>

<span data-ttu-id="7a158-244">További információ a testre szabható felhasználói felületi elemeket: [beépített házirendek felhasználói felület testreszabása a referencia-útmutató](active-directory-b2c-reference-ui-customization.md).</span><span class="sxs-lookup"><span data-stu-id="7a158-244">For additional information about UI elements that can be customized, see [reference guide for UI customization for built-in policies](active-directory-b2c-reference-ui-customization.md).</span></span>
