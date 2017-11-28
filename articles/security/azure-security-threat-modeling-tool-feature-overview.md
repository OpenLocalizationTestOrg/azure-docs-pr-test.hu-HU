---
title: "Azure fenyegetések modellezése eszköz - aaaMicrosoft |} Microsoft Docs"
description: "A fenyegetések modellezése eszköz hello elérhető hello szolgáltatásainak megismerése"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: f9ad5e623e7758063084cb7fc723c5735161a846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="threat-modeling-tool-feature-overview"></a><span data-ttu-id="b4de6-103">Fenyegetések modellezése eszköz funkcióinak áttekintése</span><span class="sxs-lookup"><span data-stu-id="b4de6-103">Threat Modeling Tool feature overview</span></span>

<span data-ttu-id="b4de6-104">Örülünk a modellezési igényeinek fenyegetés toouse hello fenyegetések modellezése eszköz választott!</span><span class="sxs-lookup"><span data-stu-id="b4de6-104">We are glad you chose toouse hello Threat Modeling Tool for your threat modeling needs!</span></span> <span data-ttu-id="b4de6-105">Ha még nem tette meg, keresse fel a  **[Ismerkedés a fenyegetések modellezése eszköz hello](./azure-security-threat-modeling-tool-getting-started.md)**  toolearn hello alapjait.</span><span class="sxs-lookup"><span data-stu-id="b4de6-105">If you haven’t done so, visit **[Getting Started with hello Threat Modeling Tool](./azure-security-threat-modeling-tool-getting-started.md)** toolearn hello basics.</span></span>

> <span data-ttu-id="b4de6-106">Az eszköz gyakran frissül, ezért ellenőrizze ezt gyakran toosee útmutató a legújabb szolgáltatásait és fejlesztéseit.</span><span class="sxs-lookup"><span data-stu-id="b4de6-106">Our tool is updated often, so check this guide often toosee our latest features and improvements.</span></span>

<span data-ttu-id="b4de6-107">Hello "Hozzon létre egy új modell" gombra kattintva megnyílik egy üres kezdőlapot, hasonló toohello kép alatt:</span><span class="sxs-lookup"><span data-stu-id="b4de6-107">Clicking on hello "Create a New Model" button opens a blank start page, similar toohello image below:</span></span>

![Üres Start lap](./media/azure-security-threat-modeling-tool/tmtstart.png)

<span data-ttu-id="b4de6-109">Hello fenyegetések modellezése használatával hozta létre a csapat a hello  **[bevezetés](./azure-security-threat-modeling-tool-getting-started.md)**  példában most kivétele ma összes hello szolgáltatásához hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="b4de6-109">Using hello threat model created by our team in hello **[Getting Started](./azure-security-threat-modeling-tool-getting-started.md)** example, let’s check out all hello features available in hello tool today.</span></span>

![Alapszintű fenyegetések modellezése](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a><span data-ttu-id="b4de6-111">Navigációs</span><span class="sxs-lookup"><span data-stu-id="b4de6-111">Navigation</span></span>

<span data-ttu-id="b4de6-112">Hello beépített szolgáltatásai ról, mielőtt ugorjunk keresztül hello fő összetevőkben hello eszközben található</span><span class="sxs-lookup"><span data-stu-id="b4de6-112">Before diving into hello built-in features, let’s go over hello main components found in hello tool</span></span>

### <a name="menu-items"></a><span data-ttu-id="b4de6-113">Menüelemek</span><span class="sxs-lookup"><span data-stu-id="b4de6-113">Menu items</span></span>

<span data-ttu-id="b4de6-114">hello élmény hasonló tooother Microsoft-termékek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4de6-114">hello experience should be similar tooother Microsoft products.</span></span> <span data-ttu-id="b4de6-115">Hozzuk, az hello legfelső szintű menüelemek keresztül:</span><span class="sxs-lookup"><span data-stu-id="b4de6-115">Let’s begin by going through hello top-level menu items:</span></span>

![Menüelemek](./media/azure-security-threat-modeling-tool/menuitems.png)

| <span data-ttu-id="b4de6-117">Címke</span><span class="sxs-lookup"><span data-stu-id="b4de6-117">Label</span></span>                               | <span data-ttu-id="b4de6-118">Részletek</span><span class="sxs-lookup"><span data-stu-id="b4de6-118">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="b4de6-119">**Fájl**</span><span class="sxs-lookup"><span data-stu-id="b4de6-119">**File**</span></span> | <ul><li><span data-ttu-id="b4de6-120">Nyissa meg, mentse és zárja be a fájlok</span><span class="sxs-lookup"><span data-stu-id="b4de6-120">Open, Save and Close Files</span></span></li><li><span data-ttu-id="b4de6-121">OneDrive In/Out bejelentkezési fiókok</span><span class="sxs-lookup"><span data-stu-id="b4de6-121">Sign In/Out of OneDrive accounts</span></span></li><li><span data-ttu-id="b4de6-122">Hivatkozásaival (nézet + Szerkesztés)</span><span class="sxs-lookup"><span data-stu-id="b4de6-122">Share Links (View + Edit)</span></span></li><li><span data-ttu-id="b4de6-123">Fájl adatainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="b4de6-123">View File Information</span></span></li><li><span data-ttu-id="b4de6-124">Új sablon tooExisting modellek alkalmazása</span><span class="sxs-lookup"><span data-stu-id="b4de6-124">Apply New Template tooExisting Models</span></span></li></ul> |
| <span data-ttu-id="b4de6-125">**Szerkesztése**</span><span class="sxs-lookup"><span data-stu-id="b4de6-125">**Edit**</span></span> | <span data-ttu-id="b4de6-126">Visszavonási vagy visszaállítási műveletek, valamint egy másolatát, Beillesztés és törlése</span><span class="sxs-lookup"><span data-stu-id="b4de6-126">Undo/redo actions, as well a copy, paste and delete</span></span> |
| <span data-ttu-id="b4de6-127">**Nézet**</span><span class="sxs-lookup"><span data-stu-id="b4de6-127">**View**</span></span> | <ul><li><span data-ttu-id="b4de6-128">Váltás a **elemzés** és **tervezési** nézetek</span><span class="sxs-lookup"><span data-stu-id="b4de6-128">Switch between **Analysis** and **Design** views</span></span></li><li><span data-ttu-id="b4de6-129">Nyissa meg a lezárt windows (e.g.stencils, tulajdonságokhoz és üzenetek)</span><span class="sxs-lookup"><span data-stu-id="b4de6-129">Open closed windows (e.g.stencils, element properties and messages)</span></span></li><li><span data-ttu-id="b4de6-130">Elrendezés toodefault beállításainak alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="b4de6-130">Reset layout toodefault settings</span></span></li></ul> |
| <span data-ttu-id="b4de6-131">**Diagram**</span><span class="sxs-lookup"><span data-stu-id="b4de6-131">**Diagram**</span></span> | <span data-ttu-id="b4de6-132">Diagramok hozzáadása vagy törlése és a "lap" diagramjainak navigálnak</span><span class="sxs-lookup"><span data-stu-id="b4de6-132">Add/Delete diagrams and navigate through “tabs” of diagrams</span></span> |
| <span data-ttu-id="b4de6-133">**Jelentések**</span><span class="sxs-lookup"><span data-stu-id="b4de6-133">**Reports**</span></span> | <span data-ttu-id="b4de6-134">HTML-jelentésekben tooshare létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4de6-134">Create HTML reports tooshare with others</span></span> |
| <span data-ttu-id="b4de6-135">**segítség**</span><span class="sxs-lookup"><span data-stu-id="b4de6-135">**Help**</span></span> | <span data-ttu-id="b4de6-136">Útmutatók toohelp hello eszközzel</span><span class="sxs-lookup"><span data-stu-id="b4de6-136">Guides toohelp you use hello tool</span></span> |

<span data-ttu-id="b4de6-137">hello ikonok hello legfelső szintű menük billentyűparancsainak is:</span><span class="sxs-lookup"><span data-stu-id="b4de6-137">hello icons are shortcuts for hello top-level menus:</span></span>

| <span data-ttu-id="b4de6-138">Ikon</span><span class="sxs-lookup"><span data-stu-id="b4de6-138">Icon</span></span>                               | <span data-ttu-id="b4de6-139">Részletek</span><span class="sxs-lookup"><span data-stu-id="b4de6-139">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="b4de6-140">**Nyissa meg**</span><span class="sxs-lookup"><span data-stu-id="b4de6-140">**Open**</span></span> | <span data-ttu-id="b4de6-141">Megnyit egy új fájlt</span><span class="sxs-lookup"><span data-stu-id="b4de6-141">Opens a new file</span></span> |
| <span data-ttu-id="b4de6-142">**Mentése**</span><span class="sxs-lookup"><span data-stu-id="b4de6-142">**Save**</span></span> | <span data-ttu-id="b4de6-143">Menti az aktuális fájl</span><span class="sxs-lookup"><span data-stu-id="b4de6-143">Saves current file</span></span> |
| <span data-ttu-id="b4de6-144">**Tervezési**</span><span class="sxs-lookup"><span data-stu-id="b4de6-144">**Design**</span></span> | <span data-ttu-id="b4de6-145">Tervező nézetben állapotba kerül, ahol modellek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4de6-145">Goes into design view, where you can create models</span></span> |
| <span data-ttu-id="b4de6-146">**Elemzés**</span><span class="sxs-lookup"><span data-stu-id="b4de6-146">**Analyze**</span></span> | <span data-ttu-id="b4de6-147">Azt mutatja be generált fenyegetések és azok tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="b4de6-147">Shows generated threats and their properties</span></span> |
| <span data-ttu-id="b4de6-148">**Diagram hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="b4de6-148">**Add Diagram**</span></span> | <span data-ttu-id="b4de6-149">Új diagram (hasonló toonew lapok az Excelben) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4de6-149">Adds new diagram (similar toonew tabs in Excel)</span></span> |
| <span data-ttu-id="b4de6-150">**Diagram törlése**</span><span class="sxs-lookup"><span data-stu-id="b4de6-150">**Delete Diagram**</span></span> | <span data-ttu-id="b4de6-151">Törli a jelenlegi diagramhoz</span><span class="sxs-lookup"><span data-stu-id="b4de6-151">Deletes current diagram</span></span> |
| <span data-ttu-id="b4de6-152">**Másolás, Kivágás és beillesztési**</span><span class="sxs-lookup"><span data-stu-id="b4de6-152">**Copy/Cut/Paste**</span></span> | <span data-ttu-id="b4de6-153">Másolatot/darabok/illeszti elemei</span><span class="sxs-lookup"><span data-stu-id="b4de6-153">Copies/cuts/pastes elements</span></span> |
| <span data-ttu-id="b4de6-154">**Visszavonási vagy visszaállítási**</span><span class="sxs-lookup"><span data-stu-id="b4de6-154">**Undo/Redo**</span></span> | <span data-ttu-id="b4de6-155">Műveletek visszavonja/megismétlése</span><span class="sxs-lookup"><span data-stu-id="b4de6-155">Undoes/redoes actions</span></span> |
| <span data-ttu-id="b4de6-156">**Nagyítás / kicsinyítés**</span><span class="sxs-lookup"><span data-stu-id="b4de6-156">**Zoom In/Zoom Out**</span></span> | <span data-ttu-id="b4de6-157">Mindkét hello diagram jobb nagyít</span><span class="sxs-lookup"><span data-stu-id="b4de6-157">Zooms in and out of hello diagram for a better view</span></span> |
| <span data-ttu-id="b4de6-158">**Visszajelzés**</span><span class="sxs-lookup"><span data-stu-id="b4de6-158">**Feedback**</span></span> | <span data-ttu-id="b4de6-159">Megnyitja a hello MSDN fórum</span><span class="sxs-lookup"><span data-stu-id="b4de6-159">Opens hello MSDN Forum</span></span> |

### <a name="canvas"></a><span data-ttu-id="b4de6-160">Vászonra</span><span class="sxs-lookup"><span data-stu-id="b4de6-160">Canvas</span></span>

<span data-ttu-id="b4de6-161">hello adhatja meg, ahol Ön áthúzása az elemeket.</span><span class="sxs-lookup"><span data-stu-id="b4de6-161">hello space where you drag and drop elements into.</span></span> <span data-ttu-id="b4de6-162">Fogd és vidd módszer hello leggyorsabb és leghatékonyabb módon toobuild modellek.</span><span class="sxs-lookup"><span data-stu-id="b4de6-162">Drag and drop is hello quickest and most efficient way toobuild models.</span></span> <span data-ttu-id="b4de6-163">Előfordulhat, hogy kattintson a jobb gombbal, és válassza hello menüpontot, amely hello elemek használata esetén általános verziói hozzáadja az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="b4de6-163">You may also right click and select from hello menu, which adds generic versions of hello elements you’re using, as shown below.</span></span>

#### <a name="dropping-hello-stencil-on-hello-canvas"></a><span data-ttu-id="b4de6-164">Hello rajzsablon eldobása hello vászonra</span><span class="sxs-lookup"><span data-stu-id="b4de6-164">Dropping hello stencil on hello canvas</span></span>

![Vászonra eldobási](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-hello-stencil"></a><span data-ttu-id="b4de6-166">Kattintson a hello rajzsablon</span><span class="sxs-lookup"><span data-stu-id="b4de6-166">Clicking on hello stencil</span></span>

![Elem tulajdonságai](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a><span data-ttu-id="b4de6-168">Rajzsablonok</span><span class="sxs-lookup"><span data-stu-id="b4de6-168">Stencils</span></span>

<span data-ttu-id="b4de6-169">Ha megtalálja az összes rajzsablonok elérhető toouse hello kiválasztott sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="b4de6-169">Where you can find all stencils available toouse based on hello template selected.</span></span> <span data-ttu-id="b4de6-170">Ha hello megfelelő elemek nem talál meg, próbálkozzon egy másik sablonnal, vagy módosítsa egy toofit igényeinek.</span><span class="sxs-lookup"><span data-stu-id="b4de6-170">If you can’t find hello right elements, try using another template, or modify one toofit your needs.</span></span> <span data-ttu-id="b4de6-171">Általában kell tudni toofind például hello néhányat a meglévők közül alábbi kategóriák kombinációja:</span><span class="sxs-lookup"><span data-stu-id="b4de6-171">Generally, you should be able toofind a combination of categories like hello ones below:</span></span>

| <span data-ttu-id="b4de6-172">Rajzsablon neve</span><span class="sxs-lookup"><span data-stu-id="b4de6-172">Stencil Name</span></span>                               | <span data-ttu-id="b4de6-173">Részletek</span><span class="sxs-lookup"><span data-stu-id="b4de6-173">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="b4de6-174">**Folyamat**</span><span class="sxs-lookup"><span data-stu-id="b4de6-174">**Process**</span></span> | <span data-ttu-id="b4de6-175">Alkalmazások, a böngésző beépülő modulok, szálak, virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b4de6-175">Applications, Browser Plugins, Threads, Virtual Machines</span></span> |
| <span data-ttu-id="b4de6-176">**Külső interaktor**</span><span class="sxs-lookup"><span data-stu-id="b4de6-176">**External Interactor**</span></span> | <span data-ttu-id="b4de6-177">Hitelesítésszolgáltatók és a böngészők felhasználói, a webes alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b4de6-177">Authentication Providers, Browsers, Users, Web Applications</span></span> |
| <span data-ttu-id="b4de6-178">**Adattár**</span><span class="sxs-lookup"><span data-stu-id="b4de6-178">**Data Store**</span></span> | <span data-ttu-id="b4de6-179">Gyorsítótár, tárolási, konfigurációs fájlok, adatbázisok, beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="b4de6-179">Cache, Storage, Configuration Files, Databases, Registry</span></span> |
| <span data-ttu-id="b4de6-180">**Adatfolyam**</span><span class="sxs-lookup"><span data-stu-id="b4de6-180">**Data Flow**</span></span> | <span data-ttu-id="b4de6-181">Bináris, ALPC, HTTP, HTTPS vagy a TLS/SSL, IOCTL, IPSec, Named Pipe, RPC/DCOM, SMB, UDP</span><span class="sxs-lookup"><span data-stu-id="b4de6-181">Binary, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, Named Pipe, RPC/DCOM, SMB, UDP</span></span> |
| <span data-ttu-id="b4de6-182">**Szegély/határ megbízhatóság**</span><span class="sxs-lookup"><span data-stu-id="b4de6-182">**Trust Line/Border Boundary**</span></span> | <span data-ttu-id="b4de6-183">Vállalati hálózatokhoz, a Internet, a gép, a védőfal, a felhasználó és a Kernel mód</span><span class="sxs-lookup"><span data-stu-id="b4de6-183">Corporate Networks, Internet, Machine, Sandbox, User/Kernel Mode</span></span> |

### <a name="notesmessages"></a><span data-ttu-id="b4de6-184">Megjegyzések/üzenetek</span><span class="sxs-lookup"><span data-stu-id="b4de6-184">Notes/Messages</span></span>

| <span data-ttu-id="b4de6-185">Összetevő</span><span class="sxs-lookup"><span data-stu-id="b4de6-185">Component</span></span>                               | <span data-ttu-id="b4de6-186">Részletek</span><span class="sxs-lookup"><span data-stu-id="b4de6-186">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="b4de6-187">**Üzenetek**</span><span class="sxs-lookup"><span data-stu-id="b4de6-187">**Messages**</span></span> | <span data-ttu-id="b4de6-188">Belső eszköz logika, amely értesíti a felhasználókat, ha nem sikerül, például az adatok elemei között zajló kommunikációról</span><span class="sxs-lookup"><span data-stu-id="b4de6-188">Internal tool logic that alerts users whenever there is an error, such as no data flows between elements</span></span> |
| <span data-ttu-id="b4de6-189">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="b4de6-189">**Notes**</span></span> | <span data-ttu-id="b4de6-190">Manuális megjegyzések mérnöki csapat által hozzáadott toohello fájl globálisan hello tervezési és a folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="b4de6-190">Manual notes added toohello file by engineering teams throughout hello design and review process</span></span> |

### <a name="element-properties"></a><span data-ttu-id="b4de6-191">Elem tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b4de6-191">Element properties</span></span>

<span data-ttu-id="b4de6-192">Ezek által kiválasztott hello elemek eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="b4de6-192">These vary by hello elements selected.</span></span> <span data-ttu-id="b4de6-193">Megbízhatósági határait kívül minden más elemet tartalmazhat 3 általános beállításokat:</span><span class="sxs-lookup"><span data-stu-id="b4de6-193">Apart from Trust Boundaries, all other elements contain 3 general selections:</span></span>

| <span data-ttu-id="b4de6-194">Elem tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4de6-194">Element Property</span></span>                               | <span data-ttu-id="b4de6-195">Részletek</span><span class="sxs-lookup"><span data-stu-id="b4de6-195">Details</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="b4de6-196">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="b4de6-196">**Name**</span></span> | <span data-ttu-id="b4de6-197">Hasznos, ha a folyamatok, tárolók, interactors és adatfolyamok toobe könnyen felismerhető elnevezése</span><span class="sxs-lookup"><span data-stu-id="b4de6-197">Useful for naming your processes, stores, interactors and flows toobe easily recognized</span></span> |
| <span data-ttu-id="b4de6-198">**Hatókörén kívül**</span><span class="sxs-lookup"><span data-stu-id="b4de6-198">**Out of Scope**</span></span> | <span data-ttu-id="b4de6-199">Választásakor hello elem használatban van-e kívül hello fenyegetés generációs mátrix (nem ajánlott)</span><span class="sxs-lookup"><span data-stu-id="b4de6-199">If selected, hello element is taken out of hello threat generation matrix (not recommended)</span></span> |
| <span data-ttu-id="b4de6-200">**Ezért a hatókörén kívül**</span><span class="sxs-lookup"><span data-stu-id="b4de6-200">**Reason for Out of Scope**</span></span> | <span data-ttu-id="b4de6-201">Indoklás mező toolet felhasználókkal, hogy része volt jelölve</span><span class="sxs-lookup"><span data-stu-id="b4de6-201">Justification field toolet users know why out of scope was selected</span></span> |

<span data-ttu-id="b4de6-202">Tulajdonságok elem kategóriákban módosult.</span><span class="sxs-lookup"><span data-stu-id="b4de6-202">Properties are changed under each element category.</span></span> <span data-ttu-id="b4de6-203">Kattintson az egyes elem tooinspect hello rendelkezésre álló lehetőségeket, vagy nyissa meg a további hello sablon toolearn.</span><span class="sxs-lookup"><span data-stu-id="b4de6-203">Click on each element tooinspect hello available options, or open hello template toolearn more.</span></span> <span data-ttu-id="b4de6-204">Folytassuk hello funkciók be.</span><span class="sxs-lookup"><span data-stu-id="b4de6-204">Let’s get into hello features.</span></span>

## <a name="welcome-screen"></a><span data-ttu-id="b4de6-205">Üdvözlőképernyő</span><span class="sxs-lookup"><span data-stu-id="b4de6-205">Welcome screen</span></span>

<span data-ttu-id="b4de6-206">hello üdvözlőképernyő hello elsőként hello alkalmazás megnyitásakor látni.</span><span class="sxs-lookup"><span data-stu-id="b4de6-206">hello welcome screen is hello first thing you see when you open hello app.</span></span>

### <a name="open-a-model"></a><span data-ttu-id="b4de6-207">Nyissa meg A modell</span><span class="sxs-lookup"><span data-stu-id="b4de6-207">Open A model</span></span>

<span data-ttu-id="b4de6-208">"Nyissa meg a modell" gomb fölött látható 2 rejtett beállítások: "Nyissa meg a számítógépen" és "Nyissa meg a onedrive-ról."</span><span class="sxs-lookup"><span data-stu-id="b4de6-208">Hovering over “Open a Model” button shows you 2 hidden options: “Open From this Computer” and “Open from OneDrive.”</span></span> <span data-ttu-id="b4de6-209">hello először nyitja meg a fájl megnyitása üdvözlő képernyőt, amíg a hello második végigvezeti hello bejelentkezési folyamat során a onedrive vállalati verzióhoz, így toopick mappákról és fájlokról, a sikeres hitelesítés után.</span><span class="sxs-lookup"><span data-stu-id="b4de6-209">hello first opens hello File Open screen, while hello second takes you through hello sign in process for OneDrive, allowing you toopick folders and files after a successful authentication.</span></span>

![Modell megnyitása](./media/azure-security-threat-modeling-tool/openmodel.png)

![Nyissa meg a számítógép vagy a onedrive vállalati verzió](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a><span data-ttu-id="b4de6-212">Visszajelzés, a javaslatok és a problémák</span><span class="sxs-lookup"><span data-stu-id="b4de6-212">Feedback, suggestions and issues</span></span>

<span data-ttu-id="b4de6-213">Ezzel a beállítással léphet vissza toohello MSDN fórumain SDL eszközök.</span><span class="sxs-lookup"><span data-stu-id="b4de6-213">Selecting this option will take you toohello MSDN Forums for SDL Tools.</span></span> <span data-ttu-id="b4de6-214">A kiváló módja toocheck kimenő mi mások véleményét az hello eszközt, beleértve a lehetséges megoldások és új ötleteket is.</span><span class="sxs-lookup"><span data-stu-id="b4de6-214">It’s a great way toocheck out what other people are saying about hello tool, including workarounds and new ideas.</span></span>

![Visszajelzés](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a><span data-ttu-id="b4de6-216">Tervező nézetben</span><span class="sxs-lookup"><span data-stu-id="b4de6-216">Design view</span></span>

<span data-ttu-id="b4de6-217">Amikor megnyitásához, vagy hozzon létre egy új modell, akkor megnyílik toohello tervezési nézetben.</span><span class="sxs-lookup"><span data-stu-id="b4de6-217">Whenever you open or create a new model, you’ll be taken toohello design view.</span></span>

### <a name="adding-elements"></a><span data-ttu-id="b4de6-218">Elemek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b4de6-218">Adding elements</span></span>

<span data-ttu-id="b4de6-219">Módon 2 tooadd elemek hello rácson:</span><span class="sxs-lookup"><span data-stu-id="b4de6-219">There are 2 ways tooadd elements on hello grid:</span></span>

- <span data-ttu-id="b4de6-220">**Adatértékmezők áthúzása** – hello kívánt elem toohello rács húzza hello elem tulajdonságai tooprovide további információt.</span><span class="sxs-lookup"><span data-stu-id="b4de6-220">**Drag and Drop** – drag hello desired element toohello grid, then use hello element properties tooprovide additional information.</span></span>
- <span data-ttu-id="b4de6-221">**Kattintson a jobb gombbal** – bármely részén kattintson a jobb gombbal hello rács és hello legördülő menüből válassza ki.</span><span class="sxs-lookup"><span data-stu-id="b4de6-221">**Right Click** – right click anywhere on hello grid and select from hello dropdown menu.</span></span> <span data-ttu-id="b4de6-222">Egy adott elem általános ábrázolása üdvözlő képernyőt fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="b4de6-222">A generic representation of that element will appear on hello screen.</span></span>

### <a name="connecting-elements"></a><span data-ttu-id="b4de6-223">Kapcsolódó elemek</span><span class="sxs-lookup"><span data-stu-id="b4de6-223">Connecting elements</span></span>

<span data-ttu-id="b4de6-224">Módon 2 tooconnect elemek hello eszközben:</span><span class="sxs-lookup"><span data-stu-id="b4de6-224">There are 2 ways tooconnect elements in hello tool:</span></span>

- <span data-ttu-id="b4de6-225">**Adatértékmezők áthúzása** – hello kívánt adatfolyamblokk toohello rács húzza, és csatlakoztassa mindkét ends toohello megfelelő elemeket.</span><span class="sxs-lookup"><span data-stu-id="b4de6-225">**Drag and Drop** – drag hello desired dataflow toohello grid, and connect both ends toohello appropriate elements.</span></span>
- <span data-ttu-id="b4de6-226">**Kattintson a + Shift** – hello első eleme (adatok küldése) parancsra, tartsa lenyomva a hello Shift billentyűt, majd jelölje be hello második elemének (az adatok fogadása).</span><span class="sxs-lookup"><span data-stu-id="b4de6-226">**Click + Shift** – click on hello first element (sending data), press and hold hello Shift key, then select hello second element (receiving data).</span></span> <span data-ttu-id="b4de6-227">Kattintson a jobb gombbal, és kattintson a "Csatlakozás".</span><span class="sxs-lookup"><span data-stu-id="b4de6-227">Right click, and select “Connect.”</span></span> <span data-ttu-id="b4de6-228">Ha egy kétirányú adatfolyam használata esetén hello sorrendje nem olyan fontos.</span><span class="sxs-lookup"><span data-stu-id="b4de6-228">If you’re using a bi-directional dataflow, hello order is not as important.</span></span>

### <a name="properties"></a><span data-ttu-id="b4de6-229">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="b4de6-229">Properties</span></span>

<span data-ttu-id="b4de6-230">Összes hello tulajdonság módosítható hello rajzsablonok helyezett hello ábra mutatja be.</span><span class="sxs-lookup"><span data-stu-id="b4de6-230">Shows all hello properties that can be modified on hello stencils placed in hello diagram.</span></span> <span data-ttu-id="b4de6-231">toosee hello tulajdonságait, kattintson hello rajzsablonon, és hello információ ennek megfelelően tölti fel.</span><span class="sxs-lookup"><span data-stu-id="b4de6-231">toosee hello properties, just click on hello stencil and hello information will be populated accordingly.</span></span> <span data-ttu-id="b4de6-232">hello az alábbi példában előtt és után egy "adatbázis" rajzsablon van húzott hello diagramja:</span><span class="sxs-lookup"><span data-stu-id="b4de6-232">hello example below shows before and after a "Database" stencil is dragged onto hello diagram:</span></span>

#### <a name="before"></a><span data-ttu-id="b4de6-233">Előtt</span><span class="sxs-lookup"><span data-stu-id="b4de6-233">Before</span></span>

![Előtt](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a><span data-ttu-id="b4de6-235">Után</span><span class="sxs-lookup"><span data-stu-id="b4de6-235">After</span></span>

![Után](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a><span data-ttu-id="b4de6-237">Üzenetek</span><span class="sxs-lookup"><span data-stu-id="b4de6-237">Messages</span></span>

<span data-ttu-id="b4de6-238">Ha a fenyegetések modellezése elfelejti tooconnect adatáramlás tooelements, hello üzenetablakban értesítést küld, tooact.</span><span class="sxs-lookup"><span data-stu-id="b4de6-238">If you create a threat model and forget tooconnect data flows tooelements, hello message window notifies you tooact.</span></span> <span data-ttu-id="b4de6-239">Kiválaszthatja a tooignore, vagy hajtsa végre hello utasításokat toofix hello probléma.</span><span class="sxs-lookup"><span data-stu-id="b4de6-239">You can choose tooignore it or follow hello instructions toofix hello issue.</span></span> 

![Üzenetek](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a><span data-ttu-id="b4de6-241">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b4de6-241">Notes</span></span>

<span data-ttu-id="b4de6-242">Váltás a lapok üzenetek tooNotes lehetővé teszi, hogy Ön tooadd megjegyzések tooyour diagram toocapture a gondolatait</span><span class="sxs-lookup"><span data-stu-id="b4de6-242">Switching tabs from Messages tooNotes allows you tooadd notes tooyour diagram toocapture all your thoughts</span></span>

## <a name="analysis-view"></a><span data-ttu-id="b4de6-243">Elemző nézet</span><span class="sxs-lookup"><span data-stu-id="b4de6-243">Analysis view</span></span>

<span data-ttu-id="b4de6-244">Miután befejezte a diagram felépítése, kapcsoló tooanalysis nézet keresztül fog toohello felső menüjében beállításokat, majd válassza a hello nagyítóüveg következő toohello paint paletta.</span><span class="sxs-lookup"><span data-stu-id="b4de6-244">Once you're done building your diagram, switch over tooanalysis view by going toohello top menu selections and choosing hello magnifying glass next toohello paint palette.</span></span>

![Elemző nézet](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a><span data-ttu-id="b4de6-246">Generált fenyegetés kiválasztása</span><span class="sxs-lookup"><span data-stu-id="b4de6-246">Generated threat selection</span></span>

<span data-ttu-id="b4de6-247">Amikor fenyegetést kattint, kihasználhatják a három egyedi funkciókat:</span><span class="sxs-lookup"><span data-stu-id="b4de6-247">When you click on a threat, you can leverage three unique functions:</span></span>

| <span data-ttu-id="b4de6-248">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4de6-248">Feature</span></span>                               | <span data-ttu-id="b4de6-249">Információ</span><span class="sxs-lookup"><span data-stu-id="b4de6-249">Info</span></span>      |
| --------------------------------------- | ------------ |
| <span data-ttu-id="b4de6-250">**Olvasási kijelző**</span><span class="sxs-lookup"><span data-stu-id="b4de6-250">**Read Indicator**</span></span> | <p><span data-ttu-id="b4de6-251">Fenyegetés most van megjelölve, olvassa el, amely segítségével könnyen nyomon követjük, hello elemek már a végrehajtás során</span><span class="sxs-lookup"><span data-stu-id="b4de6-251">Threat is now marked as read, which can easily help you keep track of hello items you already went through</span></span></p><p>![Olvasás/olvasatlan kijelző](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| <span data-ttu-id="b4de6-253">**Interakció fókusz**</span><span class="sxs-lookup"><span data-stu-id="b4de6-253">**Interaction Focus**</span></span> | <p><span data-ttu-id="b4de6-254">Hello diagram toothat fenyegetés tartozó kapcsolati ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="b4de6-254">Interaction in hello diagram belonging toothat threat is highlighted</span></span></p><p>![Interakció fókusz](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| <span data-ttu-id="b4de6-256">**Fenyegetés-tulajdonságok**</span><span class="sxs-lookup"><span data-stu-id="b4de6-256">**Threat Properties**</span></span> | <p><span data-ttu-id="b4de6-257">Hello fenyegetés további információt a telepítéskor hello fenyegetés tulajdonságai ablakban</span><span class="sxs-lookup"><span data-stu-id="b4de6-257">Additional information about hello threat is populated in hello threat properties window</span></span></p><p>![Fenyegetés-tulajdonságok](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a><span data-ttu-id="b4de6-259">Prioritás módosítása</span><span class="sxs-lookup"><span data-stu-id="b4de6-259">Priority change</span></span>

<span data-ttu-id="b4de6-260">Minden létrehozott fenyegetettség hello prioritási szintet is módosítása a színek toomake azt könnyen tooidentify magas, közepes és alacsony prioritás fenyegetéseket.</span><span class="sxs-lookup"><span data-stu-id="b4de6-260">Changing hello priority level of each generated threat also changes their colors toomake it easy tooidentify high, medium and low priority threats.</span></span>

![Prioritás módosítása](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a><span data-ttu-id="b4de6-262">Fenyegetés-tulajdonságok szerkeszthető mezők</span><span class="sxs-lookup"><span data-stu-id="b4de6-262">Threat properties editable fields</span></span>

<span data-ttu-id="b4de6-263">Hello a fenti kép látható, a felhasználók módosíthatják-e a hello eszköz által generált hello adatokat egy toocertain adatmezőket, indoklás is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b4de6-263">As seen in hello image above, users can change hello information generated by hello tool an also add information toocertain fields, such as justification.</span></span> <span data-ttu-id="b4de6-264">Ezek a mezők hello sablon által előállított, ha további információt szeretne az egyes fenyegetésekre, tehát javasolt toomake módosítások.</span><span class="sxs-lookup"><span data-stu-id="b4de6-264">These fields are generated by hello template, so if you need more information for each threat, you're encouraged toomake modifications.</span></span>

![Fenyegetés-tulajdonságok](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a><span data-ttu-id="b4de6-266">Jelentések</span><span class="sxs-lookup"><span data-stu-id="b4de6-266">Reports</span></span>

<span data-ttu-id="b4de6-267">Miután elkészült változó prioritások és frissítési hello állapotban vannak az egyes létrehozott fenyegetés, hello fájl mentéséhez, illetve nyomtassa ki a jelentés túl "jelentés" állapotra és majd "teljes jelentés létrehozása."</span><span class="sxs-lookup"><span data-stu-id="b4de6-267">Once you're done changing priorities and updating hello status of each generated threat, you can save hello file and/or print out a report by going too"Report" and then "Create Full Report."</span></span> <span data-ttu-id="b4de6-268">Meg kell adnia tooname hello jelentést, és ha így tesz, megjelenítheti toohello valami hasonló, az alábbi képen:</span><span class="sxs-lookup"><span data-stu-id="b4de6-268">You'll be asked tooname hello report, and once you do, you should see something similar toohello image below:</span></span>

![Jelentés](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a><span data-ttu-id="b4de6-270">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4de6-270">Next steps</span></span>

<span data-ttu-id="b4de6-271">hello közösségi sablonját toocontribute lépjen tooour  **[GitHub](https://github.com/Microsoft/threat-modeling-templates)**  lap.</span><span class="sxs-lookup"><span data-stu-id="b4de6-271">toocontribute a template for hello community, please go tooour **[GitHub](https://github.com/Microsoft/threat-modeling-templates)** page.</span></span> <span data-ttu-id="b4de6-272">**[Töltse le](https://aka.ms/tmtpreview)**  hello eszköz tooget neki még ma.</span><span class="sxs-lookup"><span data-stu-id="b4de6-272">**[Download](https://aka.ms/tmtpreview)** hello tool tooget started today.</span></span>
