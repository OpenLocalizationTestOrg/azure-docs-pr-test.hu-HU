---
title: "a Power BI Embedded aaaRow szintű biztonság"
description: "A Power BI Embedded sorszintű biztonsággal kapcsolatos részletek"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 7936ade5-2c75-435b-8314-ea7ca815867a
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 384f78826ecc710cf8f101b251ae68b074f3e98b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="row-level-security-with-power-bi-embedded"></a><span data-ttu-id="b3967-103">Sorszintű biztonság a Power BI Embedded szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b3967-103">Row level security with Power BI Embedded</span></span>

<span data-ttu-id="b3967-104">Sorszintű biztonság (RLS) használt toorestrict felhasználói hozzáférés tooparticular adatok egy jelentést vagy adatkészletet, így a több különböző felhasználók toouse hello ugyanabban a jelentésben minden megnéz különböző adatainak közben lehet.</span><span class="sxs-lookup"><span data-stu-id="b3967-104">Row level security (RLS) can be used toorestrict user access tooparticular data within a report or dataset, allowing for multiple different users toouse hello same report while all seeing different data.</span></span> <span data-ttu-id="b3967-105">A Power BI Embedded mostantól támogatja az RLS konfigurált adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="b3967-105">Power BI Embedded now supports datasets configured with RLS.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

<span data-ttu-id="b3967-106">RLS rendelés tootake előnyeit fontos tisztában három fő alapelv; Felhasználók, szerepkörök és szabályok.</span><span class="sxs-lookup"><span data-stu-id="b3967-106">In order tootake advantage of RLS, it’s important you understand three main concepts; Users, Roles, and Rules.</span></span> <span data-ttu-id="b3967-107">Megtudhatja, hogy minden egyes részletes bemutatása:</span><span class="sxs-lookup"><span data-stu-id="b3967-107">Let’s take a closer look at each:</span></span>

<span data-ttu-id="b3967-108">**Felhasználók** – ezek hello tényleges végfelhasználók tekinti meg jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="b3967-108">**Users** –  These are hello actual end-users viewing reports.</span></span> <span data-ttu-id="b3967-109">A Power BI Embedded hello username tulajdonság egy alkalmazási jogkivonatot a felhasználók azonosítja.</span><span class="sxs-lookup"><span data-stu-id="b3967-109">In Power BI Embedded, users are identified by hello username property in an App Token.</span></span>

<span data-ttu-id="b3967-110">**Szerepkörök** – felhasználó tooroles tartozik.</span><span class="sxs-lookup"><span data-stu-id="b3967-110">**Roles** – Users belong tooroles.</span></span> <span data-ttu-id="b3967-111">Egy szerepkör szabályok tárolója, és képes neve például "Értékesítési igazgató" vagy "Értékesítési Rep".</span><span class="sxs-lookup"><span data-stu-id="b3967-111">A role is a container for rules and can be named something like “Sales Manager” or “Sales Rep”.</span></span> <span data-ttu-id="b3967-112">A Power BI Embedded felhasználók hello szerepkörök tulajdonságait egy alkalmazási jogkivonatot azonosítják.</span><span class="sxs-lookup"><span data-stu-id="b3967-112">In Power BI Embedded, users are identified by hello roles property in an App Token.</span></span>

<span data-ttu-id="b3967-113">**Szabályok** – szerepkörök rendelkezik szabályokat, és ezeket a szabályokat, hogy alkalmazva toobe toohello adatok hello tényleges szűrők.</span><span class="sxs-lookup"><span data-stu-id="b3967-113">**Rules** – Roles have rules, and those rules are hello actual filters that are going toobe applied toohello data.</span></span> <span data-ttu-id="b3967-114">Ennek oka lehet egyszerűen "ország = USA" vagy más sokkal több dinamikus.</span><span class="sxs-lookup"><span data-stu-id="b3967-114">This could be as simple as “Country = USA” or something much more dynamic.</span></span>

### <a name="example"></a><span data-ttu-id="b3967-115">Példa</span><span class="sxs-lookup"><span data-stu-id="b3967-115">Example</span></span>

<span data-ttu-id="b3967-116">Ez a cikk többi hello a szerzői RLS, és majd fel, amely egy beágyazott alkalmazásban például következőkhöz nyújtunk.</span><span class="sxs-lookup"><span data-stu-id="b3967-116">For hello rest of this article, we’ll provide an example of authoring RLS, and then consuming that within an embedded application.</span></span> <span data-ttu-id="b3967-117">A példában hello [kiskereskedelmi elemzési minta](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX-fájl.</span><span class="sxs-lookup"><span data-stu-id="b3967-117">Our example uses hello [Retail Analysis Sample](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX file.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

<span data-ttu-id="b3967-118">A kiskereskedelmi elemzési minta értékesítés összes hello tárolóinak egy adott kereskedelmi lánc jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b3967-118">Our Retail Analysis sample shows sales for all hello stores in a particular retail chain.</span></span> <span data-ttu-id="b3967-119">Nélkül RLS függetlenül attól, milyen körzeti manager jelentkezik be, és nézetek jelentés hello, akkor fogja látni hello ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3967-119">Without RLS, no matter which district manager signs in and views hello report, they’ll see hello same data.</span></span> <span data-ttu-id="b3967-120">Minden körzet manager csak kell megjelennie hello forgalmi kezelnek hello áruházak, és a toodo ez vezető felügyeleti megállapítása szerint, RLS is használhatók.</span><span class="sxs-lookup"><span data-stu-id="b3967-120">Senior management has determined each district manager should only see hello sales for hello stores they manage, and toodo this, we can use RLS.</span></span>

<span data-ttu-id="b3967-121">RLS állította össze a Power BI Desktopban.</span><span class="sxs-lookup"><span data-stu-id="b3967-121">RLS is authored in Power BI Desktop.</span></span> <span data-ttu-id="b3967-122">Megnyitásakor hello adathalmazt és a jelentést, azt átválthatja toosee hello séma toodiagram megtekintése:</span><span class="sxs-lookup"><span data-stu-id="b3967-122">When hello dataset and report are opened, we can switch toodiagram view toosee hello schema:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

<span data-ttu-id="b3967-123">Az alábbiakban néhány dolgot toonotice a sémát:</span><span class="sxs-lookup"><span data-stu-id="b3967-123">Here are a few things toonotice with this schema:</span></span>

* <span data-ttu-id="b3967-124">Az összes mérték, például **összes értékesítés**, hello tárolódnak **értékesítési** ténytábla.</span><span class="sxs-lookup"><span data-stu-id="b3967-124">All measures, like **Total Sales**, are stored in hello **Sales** fact table.</span></span>
* <span data-ttu-id="b3967-125">Nincsenek négy további kapcsolódó dimenziótáblák: **elem**, **idő**, **tároló**, és **körzeti**.</span><span class="sxs-lookup"><span data-stu-id="b3967-125">There are four additional related dimension tables: **Item**, **Time**, **Store**, and **District**.</span></span>
* <span data-ttu-id="b3967-126">hello nyilak hello kapcsolat sorok szűrők áramolhasson az egyik tábla tooanother hogyan jelzi.</span><span class="sxs-lookup"><span data-stu-id="b3967-126">hello arrows on hello relationship lines indicate which way filters can flow from one table tooanother.</span></span> <span data-ttu-id="b3967-127">Például, ha a szűrőt el van helyezve **idő [dátum]**, hello aktuális sémában azt fog csak szűrni le hello értékek **értékesítési** tábla.</span><span class="sxs-lookup"><span data-stu-id="b3967-127">For example, if a filter is placed on **Time[Date]**, in hello current schema it would only filter down values in hello **Sales** table.</span></span> <span data-ttu-id="b3967-128">Más táblák befolyásolhat a szűrő óta az összes hello nyilak hello kapcsolat sorok pont toohello értékesítési táblában, és nem azonnal.</span><span class="sxs-lookup"><span data-stu-id="b3967-128">No other tables would be affected by this filter since all of hello arrows on hello relationship lines point toohello sales table and not away.</span></span>
* <span data-ttu-id="b3967-129">Hello **körzeti** táblázat azt jelzi, aki hello manager minden körzet:</span><span class="sxs-lookup"><span data-stu-id="b3967-129">hello **District** table indicates who hello manager is for each district:</span></span>
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

<span data-ttu-id="b3967-130">A séma alapján, ha egy szűrő toohello érvénybe lépni **körzeti Manager** oszlopa hello körzeti tábla, és ha ez a szűrő hello a jelentés megtekintése hello felhasználói megegyezik, a szűrő is szűrheti le hello **tároló**és **értékesítési** táblák tooonly jeleníthetők meg, hogy adott körzeti manager.</span><span class="sxs-lookup"><span data-stu-id="b3967-130">Based on this schema, if we apply a filter toohello **District Manager** column in hello District table, and if that filter matches hello user viewing hello report, that filter will also filter down hello **Store** and **Sales** tables tooonly show data for that particular district manager.</span></span>

<span data-ttu-id="b3967-131">Itt módját:</span><span class="sxs-lookup"><span data-stu-id="b3967-131">Here’s how:</span></span>

1. <span data-ttu-id="b3967-132">Hello modellezési lapján, kattintson a **szerepkörök kezelése**.</span><span class="sxs-lookup"><span data-stu-id="b3967-132">On hello Modeling tab, click **Manage Roles**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. <span data-ttu-id="b3967-133">Hozzon létre egy új szerepkör neve **Manager**.</span><span class="sxs-lookup"><span data-stu-id="b3967-133">Create a new role called **Manager**.</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. <span data-ttu-id="b3967-134">A hello **körzeti** tábla adja meg a következő DAX-kifejezése hello: **[körzeti Manager] = USERNAME()**</span><span class="sxs-lookup"><span data-stu-id="b3967-134">In hello **District** table enter hello following DAX expression: **[District Manager] = USERNAME()**</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. <span data-ttu-id="b3967-135">toomake meg arról, hogy hello szabályok dolgoznak, hello **modellezési** lapra, majd **szerepkörök nézetként**, majd adja meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b3967-135">toomake sure hello rules are working, on hello **Modeling** tab, click **View as Roles**, and then enter hello following:</span></span>  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   <span data-ttu-id="b3967-136">hello jelentések most adatait jeleníti meg, ha be volt jelentkezve mint **Andrew Ma**.</span><span class="sxs-lookup"><span data-stu-id="b3967-136">hello reports will now show data as if you were signed in as **Andrew Ma**.</span></span>

<span data-ttu-id="b3967-137">Minden rekordot hello le alkalmazása hello szűrőt, akkor azt itt volt hello módon szűrheti **körzeti**, **tároló**, és **értékesítési** táblákat.</span><span class="sxs-lookup"><span data-stu-id="b3967-137">Applying hello filter, hello way we did here, will filter down all records in hello **District**, **Store**, and **Sales** tables.</span></span> <span data-ttu-id="b3967-138">Hello szűrés irányának a hello közötti kapcsolatok miatt azonban **értékesítési** és **idő**, **értékesítési** és **elem**, és **Elem** és **idő** táblák nem lesznek szűrve le.</span><span class="sxs-lookup"><span data-stu-id="b3967-138">However, because of hello filter direction on hello relationships between **Sales** and **Time**, **Sales** and **Item**, and **Item** and **Time** tables will not be filtered down.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

<span data-ttu-id="b3967-139">Amely ehhez a követelményhez ok lehet, azonban kezelők toosee elem, amelynek nem rendelkeznek bármely értékesítési nem szeretnénk, ha azt sikerült kapcsolja be a kétirányú keresztszűrés hello kapcsolat és a folyamat hello biztonsági szűrőhöz mindkét irányban.</span><span class="sxs-lookup"><span data-stu-id="b3967-139">That may be ok for this requirement, however, if we don’t want managers toosee items for which they don’t have any sales, we could turn on bidirectional cross-filtering for hello relationship and flow hello security filter in both directions.</span></span> <span data-ttu-id="b3967-140">Hello közötti kapcsolat szerkesztésével ehhez **értékesítési** és **elem**, ez például:</span><span class="sxs-lookup"><span data-stu-id="b3967-140">This can be done by editing hello relationship between **Sales** and **Item**, like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

<span data-ttu-id="b3967-141">Most, szűrők is is haladjanak hello értékesítési tábla toohello **elem** tábla:</span><span class="sxs-lookup"><span data-stu-id="b3967-141">Now, filters can also flow from hello Sales table toohello **Item** table:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

> [!NOTE]
> <span data-ttu-id="b3967-142">Az adatok használata a DirectQuery mód, tooenable kétirányú-szűrés e két beállítás kiválasztásával határokon lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="b3967-142">If you're using DirectQuery mode for your data, you will need tooenable bidirectional-cross filtering by selecting these two options:</span></span>

1. <span data-ttu-id="b3967-143">**Fájl** -> **lehetőségek és beállítások** -> **előzetes verziójú funkciók** -> **a kétirányú keresztszűrés engedélyezése DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="b3967-143">**File** -> **Options and Settings** -> **Preview Features** -> **Enable cross filtering in both directions for DirectQuery**.</span></span>
2. <span data-ttu-id="b3967-144">**Fájl** -> **lehetőségek és beállítások** -> **DirectQuery** -> **DirectQuerymódbannemkorlátozottmértékengedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="b3967-144">**File** -> **Options and Settings** -> **DirectQuery** -> **Allow unrestricted measure in DirectQuery mode**.</span></span>

<span data-ttu-id="b3967-145">További információk a kétirányú keresztszűrés, letöltési hello toolearn [kétirányú keresztszűrés a SQL Server Analysis Services 2016 és a Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) tanulmány.</span><span class="sxs-lookup"><span data-stu-id="b3967-145">toolearn more about bidirectional cross-filtering, download hello [Bidirectional cross-filtering in SQL Server Analysis Services 2016 and Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) whitepaper.</span></span>

<span data-ttu-id="b3967-146">Ez foglalja össze összes hello munka toobe a Power BI Desktop kész, de egy további munkákat, amelyet a kész toobe toomake hello RLS szabályok a Power BI Embedded munkahelyi meghatározott.</span><span class="sxs-lookup"><span data-stu-id="b3967-146">This wraps up all hello work that needs toobe done in Power BI Desktop, but there’s one more piece of work that needs toobe done toomake hello RLS rules we defined work in Power BI Embedded.</span></span> <span data-ttu-id="b3967-147">A felhasználók hitelesítése és az alkalmazás által engedélyezett, és alkalmazási jogkivonatok használt toogrant, amelyek felhasználói hozzáférés tooa adott Power BI Embedded jelentést készítenek.</span><span class="sxs-lookup"><span data-stu-id="b3967-147">Users are authenticated and authorized by your application and App tokens are used toogrant that user access tooa specific Power BI Embedded report.</span></span> <span data-ttu-id="b3967-148">A Power BI Embedded nem rendelkezik a felhasználó ki van információk.</span><span class="sxs-lookup"><span data-stu-id="b3967-148">Power BI Embedded doesn’t have any specific information on who your user is.</span></span> <span data-ttu-id="b3967-149">RLS toowork kell toopass néhány további környezet az alkalmazások jogkivonat részeként:</span><span class="sxs-lookup"><span data-stu-id="b3967-149">For RLS toowork, you’ll need toopass some additional context as part of your app token:</span></span>

* <span data-ttu-id="b3967-150">**felhasználónév** (opcionális) – használja az RLS Ez egy használható toohelp hello felhasználó azonosítása, RLS szabályok alkalmazásakor.</span><span class="sxs-lookup"><span data-stu-id="b3967-150">**username** (optional) – Used with RLS this is a string that can be used toohelp identify hello user when applying RLS rules.</span></span> <span data-ttu-id="b3967-151">Lásd a biztonság a Power BI Embedded sor segítségével:</span><span class="sxs-lookup"><span data-stu-id="b3967-151">See Using Row Level Security with Power BI Embedded</span></span>
* <span data-ttu-id="b3967-152">**szerepkörök** – hello szerepkörök tooselect tartalmazhat a sorszintű biztonság szabályok alkalmazásakor.</span><span class="sxs-lookup"><span data-stu-id="b3967-152">**roles** – A string containing hello roles tooselect when applying Row Level Security rules.</span></span> <span data-ttu-id="b3967-153">Ha több szerepkör, át kell őket, egy karakterlánc-tömbben.</span><span class="sxs-lookup"><span data-stu-id="b3967-153">If passing more than one role, they should be passed as a string array.</span></span>

<span data-ttu-id="b3967-154">Hello segítségével hoz létre hello token [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) metódust.</span><span class="sxs-lookup"><span data-stu-id="b3967-154">You create hello token by using hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) method.</span></span> <span data-ttu-id="b3967-155">Ha hello username tulajdonság meg adva, is át kell legalább egy értéket a szerepkört.</span><span class="sxs-lookup"><span data-stu-id="b3967-155">If hello username property is present, you must also pass at least one value in roles.</span></span>

<span data-ttu-id="b3967-156">Például megváltoztathatja hello EmbedSample.</span><span class="sxs-lookup"><span data-stu-id="b3967-156">For example, you could change hello EmbedSample.</span></span> <span data-ttu-id="b3967-157">A DashboardController sor 55 frissítése sikerült</span><span class="sxs-lookup"><span data-stu-id="b3967-157">DashboardController line 55 could be updated from</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

<span data-ttu-id="b3967-158">erre:</span><span class="sxs-lookup"><span data-stu-id="b3967-158">to</span></span>

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

<span data-ttu-id="b3967-159">hello teljes alkalmazási jogkivonatot fog kinéznie:</span><span class="sxs-lookup"><span data-stu-id="b3967-159">hello full app token will look something like this:</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

<span data-ttu-id="b3967-160">Most az összes hello darabokat talál együtt, ha valaki be az alkalmazás tooview jelentkezik ez a jelentés azokat csak lesz képes toosee hello adatokat, hogy azok csatlakozhatnának a toosee, meghatározása szerint a sorszintű biztonság.</span><span class="sxs-lookup"><span data-stu-id="b3967-160">Now, with all hello pieces together, when someone logs into our application tooview this report, they’ll only be able toosee hello data that they are allowed toosee, as defined by our row-level security.</span></span>

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a><span data-ttu-id="b3967-161">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b3967-161">See also</span></span>

[<span data-ttu-id="b3967-162">A sorszintű biztonságot (RLS) rendelkező Power</span><span class="sxs-lookup"><span data-stu-id="b3967-162">Row-level security (RLS) with Power</span></span>](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)  
[<span data-ttu-id="b3967-163">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="b3967-163">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="b3967-164">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="b3967-164">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="b3967-165">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="b3967-165">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="b3967-166">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="b3967-166">More questions?</span></span> [<span data-ttu-id="b3967-167">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="b3967-167">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

