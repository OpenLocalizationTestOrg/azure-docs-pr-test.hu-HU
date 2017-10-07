---
title: "aaaAuthenticating és a Power BI Embedded engedélyezése"
description: "Authenticating and authorizing with Power BI Embedded (Hitelesítés és engedélyezés a Power BI Embedded használatával)"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 483ca0499e8d03584e1151d3d278c0179d4b8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a><span data-ttu-id="76932-103">Authenticating and authorizing with Power BI Embedded (Hitelesítés és engedélyezés a Power BI Embedded használatával)</span><span class="sxs-lookup"><span data-stu-id="76932-103">Authenticating and authorizing with Power BI Embedded</span></span>

<span data-ttu-id="76932-104">használja a Power BI Embedded szolgáltatás hello **kulcsok** és **alkalmazási jogkivonatok** a hitelesítéshez és engedélyezéshez, explicit végfelhasználói hitelesítés helyett.</span><span class="sxs-lookup"><span data-stu-id="76932-104">hello Power BI Embedded service uses **Keys** and **App Tokens** for authentication and authorization, instead of explicit end-user authentication.</span></span> <span data-ttu-id="76932-105">Ebben a modellben az alkalmazás kezeli a hitelesítési és engedélyezési a végfelhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="76932-105">In this model, your application manages authentication and authorization for your end-users.</span></span> <span data-ttu-id="76932-106">Ha szükséges, az alkalmazás hoz létre, és arról, hogy a szolgáltatás toorender hello alkalmazási jogkivonatok hello kért jelentést küld.</span><span class="sxs-lookup"><span data-stu-id="76932-106">When necessary, your app creates and sends hello App Tokens that tells our service toorender hello requested report.</span></span> <span data-ttu-id="76932-107">Ez a kialakítás nincs szükség az alkalmazás toouse Azure Active Directory felhasználói hitelesítési és engedélyezési, bár továbbra is.</span><span class="sxs-lookup"><span data-stu-id="76932-107">This design doesn't require your app toouse Azure Active Directory for user authentication and authorization, although you still can.</span></span>

## <a name="two-ways-tooauthenticate"></a><span data-ttu-id="76932-108">Két módon tooauthenticate</span><span class="sxs-lookup"><span data-stu-id="76932-108">Two ways tooauthenticate</span></span>

<span data-ttu-id="76932-109">**Kulcs** -kulcsok az összes Power BI Embedded REST API-hívásokhoz használható.</span><span class="sxs-lookup"><span data-stu-id="76932-109">**Key** -  You can use keys for all Power BI Embedded REST API calls.</span></span> <span data-ttu-id="76932-110">hello kulcsok hello található **Azure-portálon** kattintva **összes beállítás** , majd **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="76932-110">hello keys can be found in hello **Azure portal** by clicking on **All settings** and then **Access keys**.</span></span> <span data-ttu-id="76932-111">A kulcs mindig kezelje, mintha egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="76932-111">Always treat your key as if it were a password.</span></span> <span data-ttu-id="76932-112">Ezek a kulcsok engedélyek toomake REST API-k hívható meg egy adott munkaterület-csoport rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="76932-112">These keys have permissions toomake any REST API call on a particular workspace collection.</span></span>

<span data-ttu-id="76932-113">a kulcsot egy REST-hívást toouse hello engedélyezési fejléc a következő hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="76932-113">toouse a key on a REST call, add hello following authorization header:</span></span>            

    Authorization: AppKey {your key}

<span data-ttu-id="76932-114">**Alkalmazási jogkivonatának** -alkalmazási jogkivonatok használt összes beágyazási kérelem.</span><span class="sxs-lookup"><span data-stu-id="76932-114">**App token** - App tokens are used for all embedding requests.</span></span> <span data-ttu-id="76932-115">Úgy is korlátozott futtatni tervezett toobe ügyféloldali fontosságúak tooa egyetlen jelentés és az ajánlott eljárás tooset lejárati időt.</span><span class="sxs-lookup"><span data-stu-id="76932-115">They’re designed toobe run client-side, so they're restricted tooa single report and it’s best practice tooset an expiration time.</span></span>

<span data-ttu-id="76932-116">Alkalmazás jogkivonatok jwt-t (JSON Web Token), amely alá van írva a kulcsok egyikével.</span><span class="sxs-lookup"><span data-stu-id="76932-116">App tokens are a JWT (JSON Web Token) that is signed by one of your keys.</span></span>

<span data-ttu-id="76932-117">Az alkalmazás jogkivonatában tartalmazhat hello jogcímek a következő:</span><span class="sxs-lookup"><span data-stu-id="76932-117">Your app token can contain hello following claims:</span></span>

| <span data-ttu-id="76932-118">Jogcím</span><span class="sxs-lookup"><span data-stu-id="76932-118">Claim</span></span> | <span data-ttu-id="76932-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="76932-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="76932-120">**ver**</span><span class="sxs-lookup"><span data-stu-id="76932-120">**ver**</span></span> |<span data-ttu-id="76932-121">hello alkalmazási jogkivonatának hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="76932-121">hello version of hello app token.</span></span> <span data-ttu-id="76932-122">0.2.0 hello aktuális verziója van.</span><span class="sxs-lookup"><span data-stu-id="76932-122">0.2.0 is hello current version.</span></span> |
| <span data-ttu-id="76932-123">**és**</span><span class="sxs-lookup"><span data-stu-id="76932-123">**aud**</span></span> |<span data-ttu-id="76932-124">hello szánt címzett hello jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="76932-124">hello intended recipient of hello token.</span></span> <span data-ttu-id="76932-125">A Power BI Embedded használható: "https://analysis.windows.net/powerbi/api".</span><span class="sxs-lookup"><span data-stu-id="76932-125">For Power BI Embedded use: “https://analysis.windows.net/powerbi/api”.</span></span> |
| <span data-ttu-id="76932-126">**iss**</span><span class="sxs-lookup"><span data-stu-id="76932-126">**iss**</span></span> |<span data-ttu-id="76932-127">Egy karakterlánc, amely hello alkalmazás hello jogkivonatot kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="76932-127">A string indicating hello application which issued hello token.</span></span> |
| <span data-ttu-id="76932-128">**típusa**</span><span class="sxs-lookup"><span data-stu-id="76932-128">**type**</span></span> |<span data-ttu-id="76932-129">alkalmazási jogkivonatának létrehozott hello típusa.</span><span class="sxs-lookup"><span data-stu-id="76932-129">hello type of app token which is being created.</span></span> <span data-ttu-id="76932-130">Aktuális hello csak támogatott típus **beágyazása**.</span><span class="sxs-lookup"><span data-stu-id="76932-130">Current hello only supported type is **embed**.</span></span> |
| <span data-ttu-id="76932-131">**Windows azonnali csatlakozás**</span><span class="sxs-lookup"><span data-stu-id="76932-131">**wcn**</span></span> |<span data-ttu-id="76932-132">Munkaterület gyűjtemény neve hello token küldése történik.</span><span class="sxs-lookup"><span data-stu-id="76932-132">Workspace collection name hello token is being issued for.</span></span> |
| <span data-ttu-id="76932-133">**belső Windows-adatbázis**</span><span class="sxs-lookup"><span data-stu-id="76932-133">**wid**</span></span> |<span data-ttu-id="76932-134">A munkaterület azonosítója hello token küldése történik.</span><span class="sxs-lookup"><span data-stu-id="76932-134">Workspace ID hello token is being issued for.</span></span> |
| <span data-ttu-id="76932-135">**a relatív azonosítók**</span><span class="sxs-lookup"><span data-stu-id="76932-135">**rid**</span></span> |<span data-ttu-id="76932-136">Jelentés azonosító hello token küldése történik.</span><span class="sxs-lookup"><span data-stu-id="76932-136">Report ID hello token is being issued for.</span></span> |
| <span data-ttu-id="76932-137">**felhasználónév** (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="76932-137">**username** (optional)</span></span> |<span data-ttu-id="76932-138">RLS használt, ez egy olyan karakterlánc, amely segítségével azonosítható hello felhasználói RLS szabályok alkalmazásakor.</span><span class="sxs-lookup"><span data-stu-id="76932-138">Used with RLS, this is a string that can help identify hello user when applying RLS rules.</span></span> |
| <span data-ttu-id="76932-139">**szerepkörök** (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="76932-139">**roles** (optional)</span></span> |<span data-ttu-id="76932-140">Egy karakterlánc hello szerepkörök tooselect sorszintű biztonság szabályok alkalmazásakor.</span><span class="sxs-lookup"><span data-stu-id="76932-140">A string containing hello roles tooselect when applying Row Level Security rules.</span></span> <span data-ttu-id="76932-141">Ha több szerepkör, át kell őket, egy karakterlánc-tömbben.</span><span class="sxs-lookup"><span data-stu-id="76932-141">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="76932-142">**Szolgáltatáskapcsolódási pont** (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="76932-142">**scp** (optional)</span></span> |<span data-ttu-id="76932-143">Hello engedélyek hatókörök tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="76932-143">A string containing hello permissions scopes.</span></span> <span data-ttu-id="76932-144">Ha több szerepkör, át kell őket, egy karakterlánc-tömbben.</span><span class="sxs-lookup"><span data-stu-id="76932-144">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="76932-145">**Exp** (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="76932-145">**exp** (optional)</span></span> |<span data-ttu-id="76932-146">Azt jelzi, hogy mely hello a jogkivonat lejár hello idő.</span><span class="sxs-lookup"><span data-stu-id="76932-146">Indicates hello time in which hello token will expire.</span></span> <span data-ttu-id="76932-147">Ezek kell adhatók át a Unix időbélyegzőket.</span><span class="sxs-lookup"><span data-stu-id="76932-147">These should be passed in as Unix timestamps.</span></span> |
| <span data-ttu-id="76932-148">**NBF** (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="76932-148">**nbf** (optional)</span></span> |<span data-ttu-id="76932-149">Azt jelzi, hogy hello indításakor mely hello a jogkivonat érvényes alatt.</span><span class="sxs-lookup"><span data-stu-id="76932-149">Indicates hello time in which hello token starts being valid.</span></span> <span data-ttu-id="76932-150">Ezek kell adhatók át a Unix időbélyegzőket.</span><span class="sxs-lookup"><span data-stu-id="76932-150">These should be passed in as Unix timestamps.</span></span> |

<span data-ttu-id="76932-151">Egy minta app tokent fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="76932-151">A sample app token will look like this:</span></span>

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

<span data-ttu-id="76932-152">Amikor dekódolni, fog megjelenni ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="76932-152">When decoded, it will look something like this:</span></span>

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

<span data-ttu-id="76932-153">Nincsenek elérhető belül hello SDK-k, amelyek egyszerűbbé teszik apptokens létrehozása metódusok.</span><span class="sxs-lookup"><span data-stu-id="76932-153">There are methods available within hello SDKs that make creation of apptokens easier.</span></span> <span data-ttu-id="76932-154">Például a .NET-hez vessen egy pillantást hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) osztály és hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) módszerek.</span><span class="sxs-lookup"><span data-stu-id="76932-154">For example, for .NET you can look at hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) class and hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) methods.</span></span>

<span data-ttu-id="76932-155">A .NET SDK hello, olvassa el a túl[hatókörök](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span><span class="sxs-lookup"><span data-stu-id="76932-155">For hello .NET SDK, you can refer too[Scopes](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span></span>

## <a name="scopes"></a><span data-ttu-id="76932-156">Hatókörök</span><span class="sxs-lookup"><span data-stu-id="76932-156">Scopes</span></span>

<span data-ttu-id="76932-157">Beágyazási jogkivonatok használata esetén érdemes lehet toorestrict használatát hello erőforrásokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="76932-157">When using Embed tokens, you may want toorestrict usage of hello resources you give access to.</span></span> <span data-ttu-id="76932-158">Emiatt a hatókörbe tartozó engedélyek jogkivonatot is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="76932-158">For this reason, you can generate a token with scoped permissions.</span></span>

<span data-ttu-id="76932-159">Az alábbiakban hello hello elérhető hatókörök Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="76932-159">hello following are hello available scopes for Power BI Embedded.</span></span>

|<span data-ttu-id="76932-160">Hatókör</span><span class="sxs-lookup"><span data-stu-id="76932-160">Scope</span></span>|<span data-ttu-id="76932-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="76932-161">Description</span></span>|
|---|---|
|<span data-ttu-id="76932-162">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="76932-162">Dataset.Read</span></span>|<span data-ttu-id="76932-163">Engedélyt biztosít tooread hello megadott adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="76932-163">Provides permission tooread hello specified dataset.</span></span>|
|<span data-ttu-id="76932-164">Dataset.Write</span><span class="sxs-lookup"><span data-stu-id="76932-164">Dataset.Write</span></span>|<span data-ttu-id="76932-165">Itt megadott engedély toowrite toohello adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="76932-165">Provides permission toowrite toohello specified dataset.</span></span>|
|<span data-ttu-id="76932-166">Dataset.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="76932-166">Dataset.ReadWrite</span></span>|<span data-ttu-id="76932-167">Engedély tooread és írási toohello újrafordítja dataset biztosít.</span><span class="sxs-lookup"><span data-stu-id="76932-167">Provides permission tooread and write toohello specificed dataset.</span></span>|
|<span data-ttu-id="76932-168">Report.Read</span><span class="sxs-lookup"><span data-stu-id="76932-168">Report.Read</span></span>|<span data-ttu-id="76932-169">Engedélyt biztosít tooview hello megadott jelentés.</span><span class="sxs-lookup"><span data-stu-id="76932-169">Provides permission tooview hello specified report.</span></span>|
|<span data-ttu-id="76932-170">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="76932-170">Report.ReadWrite</span></span>|<span data-ttu-id="76932-171">Itt engedély tooview és Szerkesztés hello megadott jelentés.</span><span class="sxs-lookup"><span data-stu-id="76932-171">Provides permission tooview and edit hello specified report.</span></span>|
|<span data-ttu-id="76932-172">Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="76932-172">Workspace.Report.Create</span></span>|<span data-ttu-id="76932-173">Engedély toocreate biztosít egy új jelentés belül megadott hello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="76932-173">Provides permission toocreate a new report within hello specified workspace.</span></span>|
|<span data-ttu-id="76932-174">Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="76932-174">Workspace.Report.Copy</span></span>|<span data-ttu-id="76932-175">Itt engedély tooclone belül megadott hello meglévő jelentés munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="76932-175">Provides permission tooclone an existing report within hello specified workspace.</span></span>|

<span data-ttu-id="76932-176">Ekkor megadhatja, hogy több hatókör hello hatókörök hasonló hello között szóközt használatával.</span><span class="sxs-lookup"><span data-stu-id="76932-176">You can supply multiple scopes by using a space between hello scopes like hello following.</span></span>

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

<span data-ttu-id="76932-177">**Szükséges jogcímeket - hatókörök**</span><span class="sxs-lookup"><span data-stu-id="76932-177">**Required claims - scopes**</span></span>

<span data-ttu-id="76932-178">Szolgáltatáskapcsolódási pont: {scopesClaim} scopesClaim lehet egy karakterláncot vagy karakterláncok megjegyezni hello engedélyezett engedélyek tooworkspace erőforrások (jelentés, adatkészlet, stb.)</span><span class="sxs-lookup"><span data-stu-id="76932-178">scp: {scopesClaim} scopesClaim can be either a string or array of strings, noting hello allowed permissions tooworkspace resources (Report, Dataset, etc.)</span></span>

<span data-ttu-id="76932-179">A dekódolt jogkivonatot, hatókörök meghatározása az hasonló toohello következő lenne.</span><span class="sxs-lookup"><span data-stu-id="76932-179">A decoded token, with scopes defined, would look similar toohello following.</span></span>

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a><span data-ttu-id="76932-180">Műveletek és hatókörök</span><span class="sxs-lookup"><span data-stu-id="76932-180">Operations and scopes</span></span>

|<span data-ttu-id="76932-181">Művelet</span><span class="sxs-lookup"><span data-stu-id="76932-181">Operation</span></span>|<span data-ttu-id="76932-182">Tároló-erőforrás</span><span class="sxs-lookup"><span data-stu-id="76932-182">Target resource</span></span>|<span data-ttu-id="76932-183">Token engedélyek</span><span class="sxs-lookup"><span data-stu-id="76932-183">Token permissions</span></span>|
|---|---|---|
|<span data-ttu-id="76932-184">Adatkészlet alapján új jelentés létrehozása (a memóriában).</span><span class="sxs-lookup"><span data-stu-id="76932-184">Create (in-memory) a new report based on a dataset.</span></span>|<span data-ttu-id="76932-185">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="76932-185">Dataset</span></span>|<span data-ttu-id="76932-186">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="76932-186">Dataset.Read</span></span>|
|<span data-ttu-id="76932-187">(A memóriában) adatkészlet alapján új jelentés létrehozása és hello jelentés mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="76932-187">Create (in-memory) a new report based on a dataset and save hello report.</span></span>|<span data-ttu-id="76932-188">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="76932-188">Dataset</span></span>|<span data-ttu-id="76932-189">* Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="76932-189">* Dataset.Read</span></span><br><span data-ttu-id="76932-190">* Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="76932-190">* Workspace.Report.Create</span></span>|
|<span data-ttu-id="76932-191">Megtekintheti és (a memória) egy meglévő jelentés felfedezése és szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="76932-191">View and explore/edit (in-memory) an existing report.</span></span> <span data-ttu-id="76932-192">Report.Read Dataset.Read jelenti.</span><span class="sxs-lookup"><span data-stu-id="76932-192">Report.Read implies Dataset.Read.</span></span> <span data-ttu-id="76932-193">Ez nem lehetővé teszi a engedélyek toosave módosításokat.</span><span class="sxs-lookup"><span data-stu-id="76932-193">This will not allow permissions toosave edits.</span></span>|<span data-ttu-id="76932-194">Jelentés</span><span class="sxs-lookup"><span data-stu-id="76932-194">Report</span></span>|<span data-ttu-id="76932-195">Report.Read</span><span class="sxs-lookup"><span data-stu-id="76932-195">Report.Read</span></span>|
|<span data-ttu-id="76932-196">Módosítsa és mentse egy meglévő jelentést.</span><span class="sxs-lookup"><span data-stu-id="76932-196">Edit and save an existing report.</span></span>|<span data-ttu-id="76932-197">Jelentés</span><span class="sxs-lookup"><span data-stu-id="76932-197">Report</span></span>|<span data-ttu-id="76932-198">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="76932-198">Report.ReadWrite</span></span>|
|<span data-ttu-id="76932-199">A jelentés (Mentés másként) másolatának mentése.</span><span class="sxs-lookup"><span data-stu-id="76932-199">Save a copy of a report (Save As).</span></span>|<span data-ttu-id="76932-200">Jelentés</span><span class="sxs-lookup"><span data-stu-id="76932-200">Report</span></span>|<span data-ttu-id="76932-201">* Report.Read</span><span class="sxs-lookup"><span data-stu-id="76932-201">* Report.Read</span></span><br><span data-ttu-id="76932-202">* Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="76932-202">* Workspace.Report.Copy</span></span>|

## <a name="heres-how-hello-flow-works"></a><span data-ttu-id="76932-203">Íme a hello folyamat működése</span><span class="sxs-lookup"><span data-stu-id="76932-203">Here's how hello flow works</span></span>
1. <span data-ttu-id="76932-204">Másolja a hello API kulcsok tooyour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="76932-204">Copy hello API keys tooyour application.</span></span> <span data-ttu-id="76932-205">Hello kulcsok szerezhet be **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="76932-205">You can get hello keys in **Azure Portal**.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. <span data-ttu-id="76932-206">Token állításokat egy jogcímet, és rendelkezik a lejárati idő.</span><span class="sxs-lookup"><span data-stu-id="76932-206">Token asserts a claim and has an expiration time.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. <span data-ttu-id="76932-207">Token lekérdezi az API elérési kulcsainak aláírva.</span><span class="sxs-lookup"><span data-stu-id="76932-207">Token gets signed with an API access keys.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. <span data-ttu-id="76932-208">A felhasználó kéri tooview jelentést.</span><span class="sxs-lookup"><span data-stu-id="76932-208">User requests tooview a report.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. <span data-ttu-id="76932-209">Egy API-hozzáférési kulccsal rendelkező token érvényességét.</span><span class="sxs-lookup"><span data-stu-id="76932-209">Token is validated with an API access keys.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. <span data-ttu-id="76932-210">A Power BI Embedded küld egy jelentés toouser.</span><span class="sxs-lookup"><span data-stu-id="76932-210">Power BI Embedded sends a report toouser.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

<span data-ttu-id="76932-211">Után **Power BI Embedded** jelentésfelhasználó toohello küldi hello felhasználó megtekintheti a hello jelentést az egyéni alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="76932-211">After **Power BI Embedded** sends a report toohello user, hello user can view hello report in your custom app.</span></span> <span data-ttu-id="76932-212">Például, ha az importált hello [elemzése forgalmi adatok PBIX minta](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello minta webalkalmazás néz ki:</span><span class="sxs-lookup"><span data-stu-id="76932-212">For example, if you imported hello [Analyzing Sales Data PBIX sample](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a><span data-ttu-id="76932-213">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="76932-213">See Also</span></span>

[<span data-ttu-id="76932-214">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="76932-214">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="76932-215">Ismerkedés a Microsoft Power BI Embedded mintaverziójával</span><span class="sxs-lookup"><span data-stu-id="76932-215">Get started with Microsoft Power BI Embedded sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="76932-216">Microsoft Power BI Embedded gyakori helyzetek</span><span class="sxs-lookup"><span data-stu-id="76932-216">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="76932-217">Ismerkedés a Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="76932-217">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)  
[<span data-ttu-id="76932-218">A csharp nyelvű Power bi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="76932-218">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
<span data-ttu-id="76932-219">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="76932-219">More questions?</span></span> [<span data-ttu-id="76932-220">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="76932-220">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

