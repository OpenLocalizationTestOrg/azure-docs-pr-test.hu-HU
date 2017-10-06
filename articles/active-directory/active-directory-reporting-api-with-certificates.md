---
title: "tanúsítványok az Azure AD jelentéskészítési API aaaGet használatával végzett hello |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello tanúsítvány hitelesítő adatok tooget származó adatokkal könyvtárak felhasználói beavatkozás nélkül az Azure AD jelentéskészítési API."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2017
ms.author: ramical
ms.openlocfilehash: 00ddfaefe32ea6ae48f276c974a17ddcf84f7894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-data-using-hello-azure-ad-reporting-api-with-certificates"></a><span data-ttu-id="cb912-103">Adatok és tanúsítványok hello Azure AD jelentéskészítési API segítségével.</span><span class="sxs-lookup"><span data-stu-id="cb912-103">Get data using hello Azure AD Reporting API with certificates</span></span>
<span data-ttu-id="cb912-104">A cikk ismerteti, hogyan toouse hello tanúsítvány hitelesítő adatok tooget származó adatokkal könyvtárak felhasználói beavatkozás nélkül az Azure AD jelentéskészítési API.</span><span class="sxs-lookup"><span data-stu-id="cb912-104">This article discusses how toouse hello Azure AD Reporting API with certificate credentials tooget data from directories without user intervention.</span></span> 

## <a name="use-hello-azure-ad-reporting-api"></a><span data-ttu-id="cb912-105">Használja az Azure AD jelentéskészítési API hello</span><span class="sxs-lookup"><span data-stu-id="cb912-105">Use hello Azure AD Reporting API</span></span> 
<span data-ttu-id="cb912-106">Azure AD jelentéskészítési API szükséges, végezze el a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="cb912-106">Azure AD Reporting API requires that you complete hello following steps:</span></span>
 *  <span data-ttu-id="cb912-107">Az előfeltételek telepítése</span><span class="sxs-lookup"><span data-stu-id="cb912-107">Install prerequisites</span></span>
 *  <span data-ttu-id="cb912-108">Az alkalmazás hello tanúsítvány beállítása</span><span class="sxs-lookup"><span data-stu-id="cb912-108">Set hello certificate in your app</span></span>
 *  <span data-ttu-id="cb912-109">Hozzáférési jogkivonat lekérése</span><span class="sxs-lookup"><span data-stu-id="cb912-109">Get an access token</span></span>
 *  <span data-ttu-id="cb912-110">Hello access token toocall hello Graph API használata</span><span class="sxs-lookup"><span data-stu-id="cb912-110">Use hello access token toocall hello Graph API</span></span>

<span data-ttu-id="cb912-111">További információ a forráskódról: [A Report API modul használata](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils).</span><span class="sxs-lookup"><span data-stu-id="cb912-111">For information about source code, see [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils).</span></span> 

### <a name="install-prerequisites"></a><span data-ttu-id="cb912-112">Az előfeltételek telepítése</span><span class="sxs-lookup"><span data-stu-id="cb912-112">Install prerequisites</span></span>
<span data-ttu-id="cb912-113">Azure AD PowerShell V2 toohave és AzureADUtils modul telepítve lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="cb912-113">You will need toohave Azure AD PowerShell V2 and AzureADUtils module installed.</span></span>

1. <span data-ttu-id="cb912-114">Töltse le és telepítse az Azure AD Powershell V2 hello található utasítások segítségével: a következő [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="cb912-114">Download and install Azure AD Powershell V2, following hello instructions at [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).</span></span>
2. <span data-ttu-id="cb912-115">Töltse le a hello Azure AD Utils modulnak a [AzureAD/azure-Active Directoryban-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span><span class="sxs-lookup"><span data-stu-id="cb912-115">Download hello Azure AD Utils module from [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1).</span></span> 
  <span data-ttu-id="cb912-116">Ez a modul számos segédprogramként használható parancsmagot biztosít, többek között:</span><span class="sxs-lookup"><span data-stu-id="cb912-116">This module provides several utility cmdlets including:</span></span>
   * <span data-ttu-id="cb912-117">az adal TÁRAT használó Nuget hello legújabb verziója</span><span class="sxs-lookup"><span data-stu-id="cb912-117">hello latest version of ADAL using Nuget</span></span>
   * <span data-ttu-id="cb912-118">a felhasználó, alkalmazáskulcsok és tanúsítványok jogkivonatainak elérését az ADAL használatával,</span><span class="sxs-lookup"><span data-stu-id="cb912-118">Access tokens from user, application keys, and certificates using ADAL</span></span>
   * <span data-ttu-id="cb912-119">a lapokra bontott eredményeket kezelő Graph API-t.</span><span class="sxs-lookup"><span data-stu-id="cb912-119">Graph API handling paged results</span></span>

<span data-ttu-id="cb912-120">**tooinstall hello Azure AD Utils modul:**</span><span class="sxs-lookup"><span data-stu-id="cb912-120">**tooinstall hello Azure AD Utils module:**</span></span>

1. <span data-ttu-id="cb912-121">Hozzon létre egy könyvtárat toosave hello segédprogramok modult (például c:\azureAD), és töltse le a hello modul a Githubról.</span><span class="sxs-lookup"><span data-stu-id="cb912-121">Create a directory toosave hello utilities module (for example, c:\azureAD) and download hello module from GitHub.</span></span>
2. <span data-ttu-id="cb912-122">Nyisson meg egy PowerShell-munkamenetet, és válassza az imént létrehozott toohello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="cb912-122">Open a PowerShell session, and go toohello directory you just created.</span></span> 
3. <span data-ttu-id="cb912-123">Hello modul importálásához, majd telepítse hello PowerShell modul elérési útján hello Install-AzureADUtilsModule parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="cb912-123">Import hello module, and install it in hello PowerShell module path using hello Install-AzureADUtilsModule cmdlet.</span></span> 

<span data-ttu-id="cb912-124">hello munkamenet toothis hasonló képernyő kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="cb912-124">hello session should look similar toothis screen:</span></span>

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-hello-certificate-in-your-app"></a><span data-ttu-id="cb912-126">Az alkalmazás hello tanúsítvány beállítása</span><span class="sxs-lookup"><span data-stu-id="cb912-126">Set hello certificate in your app</span></span>
1. <span data-ttu-id="cb912-127">Ha már telepítette az alkalmazást, az objektum Azonosítóját beszerzése hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="cb912-127">If you already have an app, get its Object ID from hello Azure Portal.</span></span> 

  ![Azure Portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. <span data-ttu-id="cb912-129">Nyisson meg egy PowerShell-munkamenetet, és csatlakozzon a tooAzure AD Connect-AzureAD hello parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="cb912-129">Open a PowerShell session and connect tooAzure AD using hello Connect-AzureAD cmdlet.</span></span>

  ![Azure Portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. <span data-ttu-id="cb912-131">A AzureADUtils tooadd egy tanúsítvány credential tooit hello New-AzureADApplicationCertificateCredential parancsmag használható.</span><span class="sxs-lookup"><span data-stu-id="cb912-131">Use hello New-AzureADApplicationCertificateCredential cmdlet from AzureADUtils tooadd a certificate credential tooit.</span></span> 

>[!Note]
><span data-ttu-id="cb912-132">Meg kell tooprovide hello alkalmazás Objektumazonosító, korábban rögzített, valamint tanúsítványobjektum hello (beolvasása a használatával hello Cert: meghajtón).</span><span class="sxs-lookup"><span data-stu-id="cb912-132">You need tooprovide hello application Object ID that you captured earlier, as well as hello certificate object (get this using hello Cert: drive).</span></span>
>


  ![Azure Portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a><span data-ttu-id="cb912-134">Hozzáférési jogkivonat lekérése</span><span class="sxs-lookup"><span data-stu-id="cb912-134">Get an access token</span></span>

<span data-ttu-id="cb912-135">tooget hozzáférési tokent, AzureADUtils hello Get-AzureADGraphAPIAccessTokenFromCert parancsmag használható.</span><span class="sxs-lookup"><span data-stu-id="cb912-135">tooget an access token, use hello Get-AzureADGraphAPIAccessTokenFromCert cmdlet from AzureADUtils.</span></span> 

>[!NOTE]
><span data-ttu-id="cb912-136">Toouse hello Alkalmazásazonosító helyett hello hello utolsó szakaszban használt Objektumazonosító van szüksége.</span><span class="sxs-lookup"><span data-stu-id="cb912-136">You need toouse hello Application ID instead of hello Object ID that you used in hello last section.</span></span>
>

 ![Azure Portal](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-hello-access-token-toocall-hello-graph-api"></a><span data-ttu-id="cb912-138">Hello access token toocall hello Graph API használata</span><span class="sxs-lookup"><span data-stu-id="cb912-138">Use hello access token toocall hello Graph API</span></span>

<span data-ttu-id="cb912-139">Most hello parancsfájlt is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="cb912-139">Now you can create hello script.</span></span> <span data-ttu-id="cb912-140">Alább látható egy példa, hello AzureADUtils hello Invoke-AzureADGraphAPIQuery parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="cb912-140">Below is an example using hello Invoke-AzureADGraphAPIQuery cmdlet from hello AzureADUtils.</span></span> <span data-ttu-id="cb912-141">Ez a parancsmag több lapozható eredmények kezeli, és ezután elküldi az adott eredmények toohello PowerShell kimenetátirányításának.</span><span class="sxs-lookup"><span data-stu-id="cb912-141">This cmdlet handles multi-paged results, and then sends those results toohello PowerShell pipeline.</span></span> 

 ![Azure Portal](./media/active-directory-report-api-with-certificates/script-completed.png)

<span data-ttu-id="cb912-143">Most már készen áll a tooexport tooa CSV áll, és mentse a tooa SIEM-rendszerben.</span><span class="sxs-lookup"><span data-stu-id="cb912-143">You are now ready tooexport tooa CSV and save tooa SIEM system.</span></span> <span data-ttu-id="cb912-144">Akkor is futtathatja a parancsfájl egy ütemezett feladat tooget az Azure AD-adatok a a bérlőhöz rendszeres időközönként hello forráskód toostore alkalmazás kulcsok nélkül.</span><span class="sxs-lookup"><span data-stu-id="cb912-144">You can also wrap your script in a scheduled task tooget Azure AD data from your tenant periodically without having toostore application keys in hello source code.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="cb912-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb912-145">Next steps</span></span>
[<span data-ttu-id="cb912-146">Azure-identitás felügyeleti hello alapjai</span><span class="sxs-lookup"><span data-stu-id="cb912-146">hello fundamentals of Azure identity management</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>



