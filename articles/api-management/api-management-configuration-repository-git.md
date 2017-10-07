---
title: "aaaConfigure a Git - Azure használatával API Management szolgáltatáshoz |} Microsoft Docs"
description: "Megtudhatja, hogyan toosave és a Git használatával az API Management-konfigurációjának beállítása."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: mattfarm
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: ef7d4c18f2ea3f5c9b86403349a83aef240f979b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosave-and-configure-your-api-management-service-configuration-using-git"></a><span data-ttu-id="95eb0-103">Hogyan toosave és a Git használatával az API Management-konfigurációjának beállítása</span><span class="sxs-lookup"><span data-stu-id="95eb0-103">How toosave and configure your API Management service configuration using Git</span></span>
> 
> 

<span data-ttu-id="95eb0-104">Minden API Management szolgáltatáspéldány hello konfigurációs és a metaadatok hello szolgáltatáspéldány kapcsolatos információkat tartalmazó konfigurációs adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="95eb0-104">Each API Management service instance maintains a configuration database that contains information about hello configuration and metadata for hello service instance.</span></span> <span data-ttu-id="95eb0-105">Módosítások elvégzéséhez toohello szolgáltatáspéldány módosításával hello publisher portált a megfelelő értéket, PowerShell-parancsmag használatával, vagy a REST API-hívással.</span><span class="sxs-lookup"><span data-stu-id="95eb0-105">Changes can be made toohello service instance by changing a setting in hello publisher portal, using a PowerShell cmdlet, or making a REST API call.</span></span> <span data-ttu-id="95eb0-106">Ezenkívül toothese módszerek is kezelhet, a példány konfigurációjának Git használatával, a szolgáltatás felügyeleti lehetőségeket, mint engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="95eb0-106">In addition toothese methods, you can also manage your service instance configuration using Git, enabling service management scenarios such as:</span></span>

* <span data-ttu-id="95eb0-107">Konfigurációs rendszerverzió - töltse le, és tárolja a szolgáltatás konfigurációs különböző verziói</span><span class="sxs-lookup"><span data-stu-id="95eb0-107">Configuration versioning - download and store different versions of your service configuration</span></span>
* <span data-ttu-id="95eb0-108">Konfigurációs módosítások tömeges - módosítsa a szolgáltatás konfigurációs toomultiple részei a helyi tárház és hello módosítások hátsó toohello server integrálható egy műveletet</span><span class="sxs-lookup"><span data-stu-id="95eb0-108">Bulk configuration changes - make changes toomultiple parts of your service configuration in your local repository and integrate hello changes back toohello server with a single operation</span></span>
* <span data-ttu-id="95eb0-109">Jól ismert Git toolchain és munkafolyamat - hello Git tooling és, amelyek már ismeri a munkafolyamatok használni</span><span class="sxs-lookup"><span data-stu-id="95eb0-109">Familiar Git toolchain and workflow - use hello Git tooling and workflows that you are already familiar with</span></span>

<span data-ttu-id="95eb0-110">a következő diagram hello az API Management szolgáltatáspéldány hello különböző módokon tooconfigure áttekintését mutatja.</span><span class="sxs-lookup"><span data-stu-id="95eb0-110">hello following diagram shows an overview of hello different ways tooconfigure your API Management service instance.</span></span>

![Git konfigurálása][api-management-git-configure]

<span data-ttu-id="95eb0-112">Ha módosít tooyour szolgáltatás hello publisher portál, a PowerShell-parancsmagok vagy a hello REST API használatával, a szolgáltatás konfigurációs adatbázist hello kezelt `https://{name}.management.azure-api.net` végpont, hello diagram hello jobb oldalán látható módon.</span><span class="sxs-lookup"><span data-stu-id="95eb0-112">When you make changes tooyour service using hello publisher portal, PowerShell cmdlets, or hello REST API, you are managing your service configuration database using hello `https://{name}.management.azure-api.net` endpoint, as shown on hello right side of hello diagram.</span></span> <span data-ttu-id="95eb0-113">hello bal oldalán található hello diagram bemutatja, hogyan kezelheti a Git használatával konfigurációjának és a szolgáltatás Git-tárházban található `https://{name}.scm.azure-api.net`.</span><span class="sxs-lookup"><span data-stu-id="95eb0-113">hello left side of hello diagram illustrates how you can manage your service configuration using Git and Git repository for your service located at `https://{name}.scm.azure-api.net`.</span></span>

<span data-ttu-id="95eb0-114">a lépéseket követve hello nyújt áttekintést a Git használatával az API Management szolgáltatáspéldány kezelése.</span><span class="sxs-lookup"><span data-stu-id="95eb0-114">hello following steps provide an overview of managing your API Management service instance using Git.</span></span>

1. <span data-ttu-id="95eb0-115">A szolgáltatás a Git-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="95eb0-115">Access Git configuration in your service</span></span>
2. <span data-ttu-id="95eb0-116">A szolgáltatás konfigurációs adatbázis tooyour Git-tárház mentése</span><span class="sxs-lookup"><span data-stu-id="95eb0-116">Save your service configuration database tooyour Git repository</span></span>
3. <span data-ttu-id="95eb0-117">Hello Git tárház tooyour helyi gép klónozása</span><span class="sxs-lookup"><span data-stu-id="95eb0-117">Clone hello Git repo tooyour local machine</span></span>
4. <span data-ttu-id="95eb0-118">Hello legújabb tárház tooyour helyi számítógép le, és a véglegesítési és leküldéses módosítások hátsó tooyour tárház lekéréses</span><span class="sxs-lookup"><span data-stu-id="95eb0-118">Pull hello latest repo down tooyour local machine, and commit and push changes back tooyour repo</span></span>
5. <span data-ttu-id="95eb0-119">A tárházból hello módosítások üzembe helyezés a szolgáltatás konfigurációs adatbázis</span><span class="sxs-lookup"><span data-stu-id="95eb0-119">Deploy hello changes from your repo into your service configuration database</span></span>

<span data-ttu-id="95eb0-120">Ez a cikk ismerteti, hogyan tooenable Git toomanage használ a szolgáltatás konfigurációját, és az nyújt a hello fájlok és mappák hello Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="95eb0-120">This article describes how tooenable and use Git toomanage your service configuration and provides a reference for hello files and folders in hello Git repository.</span></span>

## <a name="access-git-configuration-in-your-service"></a><span data-ttu-id="95eb0-121">A szolgáltatás a Git-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="95eb0-121">Access Git configuration in your service</span></span>
<span data-ttu-id="95eb0-122">Gyorsan megtekintheti a Git-konfiguráció állapota hello hello publisher portál jobb felső sarkában hello hello Git ikon megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="95eb0-122">You can quickly view hello status of your Git configuration by viewing hello Git icon in hello upper-right corner of hello publisher portal.</span></span> <span data-ttu-id="95eb0-123">Ebben a példában hello állapotüzenet azt jelzi, hogy vannak-e a nem mentett módosítások toohello tárház.</span><span class="sxs-lookup"><span data-stu-id="95eb0-123">In this example, hello status message indicates that there are unsaved changes toohello repository.</span></span> <span data-ttu-id="95eb0-124">Ennek az az oka hello API-kezelés szolgáltatás konfigurációs adatbázis van még nincsenek mentve toohello tárházba.</span><span class="sxs-lookup"><span data-stu-id="95eb0-124">This is because hello API Management service configuration database has not yet been saved toohello repository.</span></span>

![Git állapota][api-management-git-icon-enable]

<span data-ttu-id="95eb0-126">tooview és a Git konfigurációs beállításait, hello Git ikonra, vagy kattintson a hello **biztonsági** menüt, és keresse meg a toohello **konfigurációs tárház** fülre.</span><span class="sxs-lookup"><span data-stu-id="95eb0-126">tooview and configure your Git configuration settings, you can either click hello Git icon, or click hello **Security** menu and navigate toohello **Configuration repository** tab.</span></span>

![GIT engedélyezése][api-management-enable-git]

> [!IMPORTANT]
> <span data-ttu-id="95eb0-128">Bármely titkos kulcsok nem definiált tulajdonságok hello tárházban fogja tárolni, és akkor is marad, amíg az előzmények tiltsa le, és engedélyezze újra a Git-hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="95eb0-128">Any secrets that are not defined as properties will be stored in hello repository and will remain in its history until you disable and re-enable Git access.</span></span> <span data-ttu-id="95eb0-129">Tulajdonságok adjon meg egy biztonságos hely toomanage állandó karakterlánc-értékek titkos adatait, beleértve a összes API konfigurálása és házirendek, így nem kell toostore közvetlenül a házirend-utasításoknál őket.</span><span class="sxs-lookup"><span data-stu-id="95eb0-129">Properties provide a secure place toomanage constant string values, including secrets, across all API configuration and policies, so you don't have toostore them directly in your policy statements.</span></span> <span data-ttu-id="95eb0-130">További információkért lásd: [hogyan toouse tulajdonságokat az Azure API-felügyeleti házirendek](api-management-howto-properties.md).</span><span class="sxs-lookup"><span data-stu-id="95eb0-130">For more information, see [How toouse properties in Azure API Management policies](api-management-howto-properties.md).</span></span>
> 
> 

<span data-ttu-id="95eb0-131">Az engedélyezés vagy letiltás hello REST API-t használó Git hozzáférés információkért lásd: [engedélyezheti vagy tilthatja le a REST API hello Git-hozzáférés](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span><span class="sxs-lookup"><span data-stu-id="95eb0-131">For information on enabling or disabling Git access using hello REST API, see [Enable or disable Git access using hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span></span>

## <a name="toosave-hello-service-configuration-toohello-git-repository"></a><span data-ttu-id="95eb0-132">toosave hello szolgáltatás konfigurációs toohello Git-tárház</span><span class="sxs-lookup"><span data-stu-id="95eb0-132">toosave hello service configuration toohello Git repository</span></span>
<span data-ttu-id="95eb0-133">hello első lépéseket, mielőtt hello tárház klónozása toosave hello aktuális állapot hello szolgáltatás konfigurációs toohello tárház.</span><span class="sxs-lookup"><span data-stu-id="95eb0-133">hello first step before cloning hello repository is toosave hello current state of hello service configuration toohello repository.</span></span> <span data-ttu-id="95eb0-134">Kattintson a **konfigurációs toorepository mentése**.</span><span class="sxs-lookup"><span data-stu-id="95eb0-134">Click **Save configuration toorepository**.</span></span>

![Konfiguráció mentése][api-management-save-configuration]

<span data-ttu-id="95eb0-136">Adja meg a kívánt módosításokat a hello visszaigazoló képernyő, és kattintson a **Ok** toosave.</span><span class="sxs-lookup"><span data-stu-id="95eb0-136">Make any desired changes on hello confirmation screen and click **Ok** toosave.</span></span>

![Konfiguráció mentése][api-management-save-configuration-confirm]

<span data-ttu-id="95eb0-138">Néhány másodpercen belül a program menti a hello konfigurációt, és hello konfiguráció állapota hello tárház megjelennek, például a hello dátum és idő hello utolsó konfigurációváltozás és a legutóbbi szinkronizálás hello hello szolgáltatás konfigurációja és hello között tárház.</span><span class="sxs-lookup"><span data-stu-id="95eb0-138">After a few moments hello configuration is saved, and hello configuration status of hello repository is displayed, including hello date and time of hello last configuration change and hello last synchronization between hello service configuration and hello repository.</span></span>

![Konfiguráció állapota][api-management-configuration-status]

<span data-ttu-id="95eb0-140">Miután hello konfigurációs toohello tárház menti, akkor klónozható.</span><span class="sxs-lookup"><span data-stu-id="95eb0-140">Once hello configuration is saved toohello repository, it can be cloned.</span></span>

<span data-ttu-id="95eb0-141">Információ a hello REST API használatával a művelet végrehajtása: [véglegesítési konfiguráció pillanatkép-hello REST API használatával](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span><span class="sxs-lookup"><span data-stu-id="95eb0-141">For information on performing this operation using hello REST API, see [Commit configuration snapshot using hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span></span>

## <a name="tooclone-hello-repository-tooyour-local-machine"></a><span data-ttu-id="95eb0-142">tooclone hello tárház tooyour helyi számítógép</span><span class="sxs-lookup"><span data-stu-id="95eb0-142">tooclone hello repository tooyour local machine</span></span>
<span data-ttu-id="95eb0-143">a tárház tooclone, kell hello URL-cím tooyour tárház, a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="95eb0-143">tooclone a repository, you need hello URL tooyour repository, a user name, and a password.</span></span> <span data-ttu-id="95eb0-144">hello felhasználói nevét és URL-cím megjelenített hello hello tetején **konfigurációs tárház** fülre.</span><span class="sxs-lookup"><span data-stu-id="95eb0-144">hello user name and URL are displayed near hello top of hello **Configuration repository** tab.</span></span>

![Git-klón][api-management-configuration-git-clone]

<span data-ttu-id="95eb0-146">hello jelszó jön létre hello hello alján **konfigurációs tárház** fülre.</span><span class="sxs-lookup"><span data-stu-id="95eb0-146">hello password is generated at hello bottom of hello **Configuration repository** tab.</span></span>

![Jelszó létrehozása][api-management-generate-password]

<span data-ttu-id="95eb0-148">toogenerate jelszavát, győződjön meg arról, hogy hello **lejárati** toohello szükséges a lejárati dátum és idő beállítása, és kattintson a **azonosító létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="95eb0-148">toogenerate a password, first ensure that hello **Expiry** is set toohello desired expiration date and time, and then click **Generate Token**.</span></span>

![Jelszó][api-management-password]

> [!IMPORTANT]
> <span data-ttu-id="95eb0-150">Jegyezze fel ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="95eb0-150">Make a note of this password.</span></span> <span data-ttu-id="95eb0-151">Ha hagyja a lap hello jelszó nem jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="95eb0-151">Once you leave this page hello password will not be displayed again.</span></span>
> 
> 

<span data-ttu-id="95eb0-152">az eszköz a következő példák használja a Git bash eszközt hello hello [Git for Windows](http://www.git-scm.com/downloads) , de bármely, a ismeri a Git-eszközt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="95eb0-152">hello following examples use hello Git Bash tool from [Git for Windows](http://www.git-scm.com/downloads) but you can use any Git tool that you are familiar with.</span></span>

<span data-ttu-id="95eb0-153">Nyissa meg a Git eszköz hello kívánt mappa, és futtassa a következő parancs tooclone hello git tárház tooyour helyi számítógép, hello publisher portál által szolgáltatott hello paranccsal hello.</span><span class="sxs-lookup"><span data-stu-id="95eb0-153">Open your Git tool in hello desired folder and run hello following command tooclone hello git repository tooyour local machine, using hello command provided by hello publisher portal.</span></span>

```
git clone https://bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="95eb0-154">Hello felhasználónevet és jelszót adjon meg.</span><span class="sxs-lookup"><span data-stu-id="95eb0-154">Provide hello user name and password when prompted.</span></span>

<span data-ttu-id="95eb0-155">Ha bármilyen hiba jelentkezik, próbálja meg módosítani a `git clone` parancs tooinclude hello felhasználónevét és jelszavát, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="95eb0-155">If you receive any errors, try modifying your `git clone` command tooinclude hello user name and password, as shown in hello following example.</span></span>

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="95eb0-156">Ez biztosítja, hogy a hiba, ha próbálja hello parancs része hello jelszó kódolás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="95eb0-156">If this provides an error, try URL encoding hello password portion of hello command.</span></span> <span data-ttu-id="95eb0-157">Egy gyorsan toodo ez tooopen Visual Studio, és probléma hello következő parancsot a hello **parancsablakban**.</span><span class="sxs-lookup"><span data-stu-id="95eb0-157">One quick way toodo this is tooopen Visual Studio, and issue hello following command in hello **Immediate Window**.</span></span> <span data-ttu-id="95eb0-158">tooopen hello **parancsablakban**, nyissa meg a bármely megoldás vagy a projektet a Visual Studio (vagy hozzon létre egy új üres konzolalkalmazást), és válassza a **Windows**, **Immediate** a Hello **Debug** menü.</span><span class="sxs-lookup"><span data-stu-id="95eb0-158">tooopen hello **Immediate Window**, open any solution or project in Visual Studio (or create a new empty console application), and choose **Windows**, **Immediate** from hello **Debug** menu.</span></span>

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

<span data-ttu-id="95eb0-159">A felhasználó nevét és a tárház hely tooconstruct hello git parancs együtt kódolású hello jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="95eb0-159">Use hello encoded password along with your user name and repository location tooconstruct hello git command.</span></span>

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="95eb0-160">Miután hello tárház klónozták tekintheti meg és a helyi rendszer használható.</span><span class="sxs-lookup"><span data-stu-id="95eb0-160">Once hello repository is cloned you can view and work with it in your local file system.</span></span> <span data-ttu-id="95eb0-161">További információkért lásd: [fájl- és helyi Git-tárház hivatkozás szerkezeti](#file-and-folder-structure-reference-of-local-git-repository).</span><span class="sxs-lookup"><span data-stu-id="95eb0-161">For more information, see [File and folder structure reference of local Git repository](#file-and-folder-structure-reference-of-local-git-repository).</span></span>

## <a name="tooupdate-your-local-repository-with-hello-most-current-service-instance-configuration"></a><span data-ttu-id="95eb0-162">tooupdate a helyi-tárház hello legfrissebb példányának konfigurációja</span><span class="sxs-lookup"><span data-stu-id="95eb0-162">tooupdate your local repository with hello most current service instance configuration</span></span>
<span data-ttu-id="95eb0-163">Ha módosítások tooyour API Management service-példány hello publisher portálon vagy a hello REST API használatával, a helyi tárház hello legújabb módosításokkal frissítése előtt mentenie kell a módosítások toohello tárház.</span><span class="sxs-lookup"><span data-stu-id="95eb0-163">If you make changes tooyour API Management service instance in hello publisher portal or using hello REST API, you must save these changes toohello repository before you can update your local repository with hello latest changes.</span></span> <span data-ttu-id="95eb0-164">toodo, kattintson **konfigurációs toorepository mentése** a hello **konfigurációs tárház** hello publisher portál lapot, és hogyan adhat ki a következő parancs a helyi tárházban hello.</span><span class="sxs-lookup"><span data-stu-id="95eb0-164">toodo this, click **Save configuration toorepository** on hello **Configuration repository** tab in hello publisher portal, and then issue hello following command in your local repository.</span></span>

```
git pull
```

<span data-ttu-id="95eb0-165">Futtatása előtt `git pull` érdekében, hogy hello mappában található a helyi tárház.</span><span class="sxs-lookup"><span data-stu-id="95eb0-165">Before running `git pull` ensure that you are in hello folder for your local repository.</span></span> <span data-ttu-id="95eb0-166">Ha befejezte a hello `git clone` parancsot, majd meg kell változtatnia hello directory tooyour tárház hello következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="95eb0-166">If you have just completed hello `git clone` command, then you must change hello directory tooyour repo by running a command like hello following.</span></span>

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="toopush-changes-from-your-local-repo-toohello-server-repo"></a><span data-ttu-id="95eb0-167">a helyi tárház toohello server tárházból toopush módosítások</span><span class="sxs-lookup"><span data-stu-id="95eb0-167">toopush changes from your local repo toohello server repo</span></span>
<span data-ttu-id="95eb0-168">toopush módosítja a helyi tárház toohello server tárházban lévő, a változtatások véglegesítése a határidő és majd küldje le őket toohello server tárház kell.</span><span class="sxs-lookup"><span data-stu-id="95eb0-168">toopush changes from your local repository toohello server repository, you must commit your changes and then push them toohello server repository.</span></span> <span data-ttu-id="95eb0-169">toocommit a módosításokat, a Git parancs eszköz, a helyi tárház, és a következő parancsok probléma hello kapcsoló toohello könyvtár megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="95eb0-169">toocommit your changes, open your Git command tool, switch toohello directory of your local repository, and issue hello following commands.</span></span>

```
git add --all
git commit -m "Description of your changes"
```

<span data-ttu-id="95eb0-170">összes hello véglegesíti toohello kiszolgálón, futtassa toopush hello a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="95eb0-170">toopush all of hello commits toohello server, run hello following command.</span></span>

```
git push
```

## <a name="toodeploy-any-service-configuration-changes-toohello-api-management-service-instance"></a><span data-ttu-id="95eb0-171">toodeploy bármilyen konfigurációs módosításokat toohello API-kezelés szolgáltatás szolgáltatáspéldány</span><span class="sxs-lookup"><span data-stu-id="95eb0-171">toodeploy any service configuration changes toohello API Management service instance</span></span>
<span data-ttu-id="95eb0-172">Ha a helyi módosítások véglegesítése és megnyomott toohello server tárház, telepíthetők tooyour API Management service-példány.</span><span class="sxs-lookup"><span data-stu-id="95eb0-172">Once your local changes are committed and pushed toohello server repository, you can deploy them tooyour API Management service instance.</span></span>

![Üzembe helyezés][api-management-configuration-deploy]

<span data-ttu-id="95eb0-174">Információ a hello REST API használatával a művelet végrehajtása: [telepítése Git változik hello REST API használatával tooconfiguration adatbázis](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span><span class="sxs-lookup"><span data-stu-id="95eb0-174">For information on performing this operation using hello REST API, see [Deploy Git changes tooconfiguration database using hello REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span></span>

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a><span data-ttu-id="95eb0-175">Fájl- és helyi Git-tárház szerkezete-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="95eb0-175">File and folder structure reference of local Git repository</span></span>
<span data-ttu-id="95eb0-176">hello fájlok és mappák hello helyi git-tárházban hello konfigurációs információkat tartalmaznak hello szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="95eb0-176">hello files and folders in hello local git repository contain hello configuration information about hello service instance.</span></span>

| <span data-ttu-id="95eb0-177">Elem</span><span class="sxs-lookup"><span data-stu-id="95eb0-177">Item</span></span> | <span data-ttu-id="95eb0-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="95eb0-178">Description</span></span> |
| --- | --- |
| <span data-ttu-id="95eb0-179">Api-felügyeleti gyökérmappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-179">root api-management folder</span></span> |<span data-ttu-id="95eb0-180">Hello szolgáltatáspéldány legfelső szintű konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="95eb0-180">Contains top-level configuration for hello service instance</span></span> |
| <span data-ttu-id="95eb0-181">API-k mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-181">apis folder</span></span> |<span data-ttu-id="95eb0-182">A következő szolgáltatáspéldányba hello hello API-k hello konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="95eb0-182">Contains hello configuration for hello apis in hello service instance</span></span> |
| <span data-ttu-id="95eb0-183">csoportok mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-183">groups folder</span></span> |<span data-ttu-id="95eb0-184">A következő szolgáltatáspéldányba hello hello csoportok hello konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="95eb0-184">Contains hello configuration for hello groups in hello service instance</span></span> |
| <span data-ttu-id="95eb0-185">házirend mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-185">policies folder</span></span> |<span data-ttu-id="95eb0-186">Tartalmazza a hello házirendek hello szolgáltatáspéldány</span><span class="sxs-lookup"><span data-stu-id="95eb0-186">Contains hello policies in hello service instance</span></span> |
| <span data-ttu-id="95eb0-187">portalStyles mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-187">portalStyles folder</span></span> |<span data-ttu-id="95eb0-188">A szolgáltatáspéldány hello hello developer portálon testreszabások hello konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="95eb0-188">Contains hello configuration for hello developer portal customizations in hello service instance</span></span> |
| <span data-ttu-id="95eb0-189">termékek mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-189">products folder</span></span> |<span data-ttu-id="95eb0-190">A következő szolgáltatáspéldányba hello hello termékek hello konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="95eb0-190">Contains hello configuration for hello products in hello service instance</span></span> |
| <span data-ttu-id="95eb0-191">sablonok mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-191">templates folder</span></span> |<span data-ttu-id="95eb0-192">A következő szolgáltatáspéldányba hello hello e-mail sablonok hello konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="95eb0-192">Contains hello configuration for hello email templates in hello service instance</span></span> |

<span data-ttu-id="95eb0-193">Minden mappa tartalmazhat egy vagy több fájlt, és bizonyos esetekben egy vagy több mappát, például egy mappát az egyes API-t, a termék vagy a csoport.</span><span class="sxs-lookup"><span data-stu-id="95eb0-193">Each folder can contain one or more files, and in some cases one or more folders, for example a folder for each API, product, or group.</span></span> <span data-ttu-id="95eb0-194">hello minden mappában található fájlok adott hello mappa neve szerint hello entitástípushoz.</span><span class="sxs-lookup"><span data-stu-id="95eb0-194">hello files within each folder are specific for hello entity type described by hello folder name.</span></span>

| <span data-ttu-id="95eb0-195">Fájltípus</span><span class="sxs-lookup"><span data-stu-id="95eb0-195">File type</span></span> | <span data-ttu-id="95eb0-196">Cél</span><span class="sxs-lookup"><span data-stu-id="95eb0-196">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="95eb0-197">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="95eb0-197">json</span></span> |<span data-ttu-id="95eb0-198">Hello megfelelő entitás konfigurációs adatait</span><span class="sxs-lookup"><span data-stu-id="95eb0-198">Configuration information about hello respective entity</span></span> |
| <span data-ttu-id="95eb0-199">HTML</span><span class="sxs-lookup"><span data-stu-id="95eb0-199">html</span></span> |<span data-ttu-id="95eb0-200">Hello entitás, gyakran hello developer portálon megjelenő leírást</span><span class="sxs-lookup"><span data-stu-id="95eb0-200">Descriptions about hello entity, often displayed in hello developer portal</span></span> |
| <span data-ttu-id="95eb0-201">xml</span><span class="sxs-lookup"><span data-stu-id="95eb0-201">xml</span></span> |<span data-ttu-id="95eb0-202">Házirend-utasítások</span><span class="sxs-lookup"><span data-stu-id="95eb0-202">Policy statements</span></span> |
| <span data-ttu-id="95eb0-203">CSS</span><span class="sxs-lookup"><span data-stu-id="95eb0-203">css</span></span> |<span data-ttu-id="95eb0-204">A fejlesztői portálon testreszabáshoz stíluslapok</span><span class="sxs-lookup"><span data-stu-id="95eb0-204">Style sheets for developer portal customization</span></span> |

<span data-ttu-id="95eb0-205">Ezeket a fájlokat létrehozása, törlése, szerkeszthető, és a helyi fájlrendszerben felügyelt, és hello módosítások telepített hátsó toohello az API Management szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="95eb0-205">These files can be created, deleted, edited, and managed on your local file system, and hello changes deployed back toohello your API Management service instance.</span></span>

> [!NOTE]
> <span data-ttu-id="95eb0-206">hello következő entitások nem hello Git-tárházban szereplő és nem lehet konfigurálni a Git használatával.</span><span class="sxs-lookup"><span data-stu-id="95eb0-206">hello following entities are not contained in hello Git repository and cannot be configured using Git.</span></span>
> 
> * <span data-ttu-id="95eb0-207">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="95eb0-207">Users</span></span>
> * <span data-ttu-id="95eb0-208">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="95eb0-208">Subscriptions</span></span>
> * <span data-ttu-id="95eb0-209">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="95eb0-209">Properties</span></span>
> * <span data-ttu-id="95eb0-210">Fejlesztői portálon entitások eltérő stílusok</span><span class="sxs-lookup"><span data-stu-id="95eb0-210">Developer portal entities other than styles</span></span>
> 
> 

### <a name="root-api-management-folder"></a><span data-ttu-id="95eb0-211">Api-felügyeleti gyökérmappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-211">Root api-management folder</span></span>
<span data-ttu-id="95eb0-212">hello legfelső szintű `api-management` a mappa tartalmaz egy `configuration.json` hello szolgáltatáspéldány formátuma a következő hello a legfelső szintű információkat tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="95eb0-212">hello root `api-management` folder contains a `configuration.json` file that contains top-level information about hello service instance in hello following format.</span></span>

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": ""
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

<span data-ttu-id="95eb0-213">első négy beállítások hello (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, és `UserRegistrationTermsConsentRequired`) hozzárendelését követően beállítások hello toohello **identitások** hello lapján **biztonsági** szakasz.</span><span class="sxs-lookup"><span data-stu-id="95eb0-213">hello first four settings (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, and `UserRegistrationTermsConsentRequired`) map toohello following settings on hello **Identities** tab in hello **Security** section.</span></span>

| <span data-ttu-id="95eb0-214">Azonosító beállítása</span><span class="sxs-lookup"><span data-stu-id="95eb0-214">Identity setting</span></span> | <span data-ttu-id="95eb0-215">A Maps túl</span><span class="sxs-lookup"><span data-stu-id="95eb0-215">Maps too</span></span>|
| --- | --- |
| <span data-ttu-id="95eb0-216">RegistrationEnabled</span><span class="sxs-lookup"><span data-stu-id="95eb0-216">RegistrationEnabled</span></span> |<span data-ttu-id="95eb0-217">**A névtelen felhasználók toosign oldal átirányítási** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="95eb0-217">**Redirect anonymous users toosign-in page** checkbox</span></span> |
| <span data-ttu-id="95eb0-218">UserRegistrationTerms</span><span class="sxs-lookup"><span data-stu-id="95eb0-218">UserRegistrationTerms</span></span> |<span data-ttu-id="95eb0-219">**Használati feltételek a felhasználói regisztráció** szövegmező</span><span class="sxs-lookup"><span data-stu-id="95eb0-219">**Terms of use on user signup** textbox</span></span> |
| <span data-ttu-id="95eb0-220">UserRegistrationTermsEnabled</span><span class="sxs-lookup"><span data-stu-id="95eb0-220">UserRegistrationTermsEnabled</span></span> |<span data-ttu-id="95eb0-221">**Használati feltételek megjelenítése a bejelentkezési lapon** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="95eb0-221">**Show terms of use on signup page** checkbox</span></span> |
| <span data-ttu-id="95eb0-222">UserRegistrationTermsConsentRequired</span><span class="sxs-lookup"><span data-stu-id="95eb0-222">UserRegistrationTermsConsentRequired</span></span> |<span data-ttu-id="95eb0-223">**Hozzájárulás szükséges** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="95eb0-223">**Require consent** checkbox</span></span> |

![Identitás][api-management-identity-settings]

<span data-ttu-id="95eb0-225">hello a következő négy beállítások (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, és `DelegationValidationKey`) hozzárendelését követően beállítások hello toohello **delegálás** hello lapján **biztonsági** szakasz.</span><span class="sxs-lookup"><span data-stu-id="95eb0-225">hello next four settings (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, and `DelegationValidationKey`) map toohello following settings on hello **Delegation** tab in hello **Security** section.</span></span>

| <span data-ttu-id="95eb0-226">Delegálási beállítás</span><span class="sxs-lookup"><span data-stu-id="95eb0-226">Delegation setting</span></span> | <span data-ttu-id="95eb0-227">A Maps túl</span><span class="sxs-lookup"><span data-stu-id="95eb0-227">Maps too</span></span>|
| --- | --- |
| <span data-ttu-id="95eb0-228">DelegationEnabled</span><span class="sxs-lookup"><span data-stu-id="95eb0-228">DelegationEnabled</span></span> |<span data-ttu-id="95eb0-229">**Delegált bejelentkezési és regisztrációs** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="95eb0-229">**Delegate sign-in & sign-up** checkbox</span></span> |
| <span data-ttu-id="95eb0-230">DelegationUrl</span><span class="sxs-lookup"><span data-stu-id="95eb0-230">DelegationUrl</span></span> |<span data-ttu-id="95eb0-231">**Delegálás végponti URL-cím** szövegmező</span><span class="sxs-lookup"><span data-stu-id="95eb0-231">**Delegation endpoint URL** textbox</span></span> |
| <span data-ttu-id="95eb0-232">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="95eb0-232">DelegatedSubscriptionEnabled</span></span> |<span data-ttu-id="95eb0-233">**Termék-előfizetéshez delegálása** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="95eb0-233">**Delegate product subscription** checkbox</span></span> |
| <span data-ttu-id="95eb0-234">DelegationValidationKey</span><span class="sxs-lookup"><span data-stu-id="95eb0-234">DelegationValidationKey</span></span> |<span data-ttu-id="95eb0-235">**Érvényesítési kulcs delegálása** szövegmező</span><span class="sxs-lookup"><span data-stu-id="95eb0-235">**Delegate Validation Key** textbox</span></span> |

![A delegálási beállításokat][api-management-delegation-settings]

<span data-ttu-id="95eb0-237">végső beállítás hello `$ref-policy`, toohello globális utasítások házirendfájl hello szolgáltatáspéldány rendeli.</span><span class="sxs-lookup"><span data-stu-id="95eb0-237">hello final setting, `$ref-policy`, maps toohello global policy statements file for hello service instance.</span></span>

### <a name="apis-folder"></a><span data-ttu-id="95eb0-238">API-k mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-238">apis folder</span></span>
<span data-ttu-id="95eb0-239">Hello `apis` mappa tartalmaz egy mappát az egyes API hello szolgáltatást a példányon, amely tartalmazza a következő elemek hello.</span><span class="sxs-lookup"><span data-stu-id="95eb0-239">hello `apis` folder contains a folder for each API in hello service instance which contains hello following items.</span></span>

* <span data-ttu-id="95eb0-240">`apis\<api name>\configuration.json`-Ez hello API hello konfigurációját és hello háttérkiszolgáló URL-címe és hello műveletek tartalmaz információkat.</span><span class="sxs-lookup"><span data-stu-id="95eb0-240">`apis\<api name>\configuration.json` - this is hello configuration for hello API and contains information about hello backend service URL and hello operations.</span></span> <span data-ttu-id="95eb0-241">Ez a hello ugyanazokat az információkat, amely akkor tér vissza, ha toocall [egy meghatározott API beszerzésének](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) rendelkező `export=true` a `application/json` formátumban.</span><span class="sxs-lookup"><span data-stu-id="95eb0-241">This is hello same information that would be returned if you were toocall [Get a specific API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) with `export=true` in `application/json` format.</span></span>
* <span data-ttu-id="95eb0-242">`apis\<api name>\api.description.html`-Ez hello hello API leírása és felel meg a toohello `description` hello tulajdonságának [API entitás](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="95eb0-242">`apis\<api name>\api.description.html` - this is hello description of hello API and corresponds toohello `description` property of hello [API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span></span>
* <span data-ttu-id="95eb0-243">`apis\<api name>\operations\`– Ez a mappa tartalmaz `<operation name>.description.html` fájlok, amelyek kapcsolódnak hello API toohello műveletei.</span><span class="sxs-lookup"><span data-stu-id="95eb0-243">`apis\<api name>\operations\` - this folder contains `<operation name>.description.html` files that map toohello operations in hello API.</span></span> <span data-ttu-id="95eb0-244">Minden fájl tartalmazza a hello API, amely a beléptetést toohello egyetlen műveletben hello leírása `description` hello tulajdonságának [művelet entitás](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) a hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="95eb0-244">Each file contains hello description of a single operation in hello API which maps toohello `description` property of hello [operation entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in hello REST API.</span></span>

### <a name="groups-folder"></a><span data-ttu-id="95eb0-245">csoportok mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-245">groups folder</span></span>
<span data-ttu-id="95eb0-246">Hello `groups` mappa tartalmaz egy mappát az egyes hello szolgáltatáspéldány definiálva.</span><span class="sxs-lookup"><span data-stu-id="95eb0-246">hello `groups` folder contains a folder for each group defined in hello service instance.</span></span>

* <span data-ttu-id="95eb0-247">`groups\<group name>\configuration.json`-hello csoport hello beállítani.</span><span class="sxs-lookup"><span data-stu-id="95eb0-247">`groups\<group name>\configuration.json` - this is hello configuration for hello group.</span></span> <span data-ttu-id="95eb0-248">Ez a hello ugyanazokat az információkat, amely akkor tér vissza, ha toocall hello [egy adott csoport lekérése](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) műveletet.</span><span class="sxs-lookup"><span data-stu-id="95eb0-248">This is hello same information that would be returned if you were toocall hello [Get a specific group](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operation.</span></span>
* <span data-ttu-id="95eb0-249">`groups\<group name>\description.html`-Ez hello csoport hello leírása és felel meg a toohello `description` hello tulajdonságának [entitás csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="95eb0-249">`groups\<group name>\description.html` - this is hello description of hello group and corresponds toohello `description` property of hello [group entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span></span>

### <a name="policies-folder"></a><span data-ttu-id="95eb0-250">házirend mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-250">policies folder</span></span>
<span data-ttu-id="95eb0-251">Hello `policies` mappa hello házirend-utasításoknál a szolgáltatáspéldány tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="95eb0-251">hello `policies` folder contains hello policy statements for your service instance.</span></span>

* <span data-ttu-id="95eb0-252">`policies\global.xml`-a szolgáltatáspéldány globális hatókörben meghatározott házirendek szerint tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="95eb0-252">`policies\global.xml` - contains policies defined at global scope for your service instance.</span></span>
* <span data-ttu-id="95eb0-253">`policies\apis\<api name>\`-Ha bármely API hatókörből meghatározott házirendek szerint, akkor ebben a mappában találhatók.</span><span class="sxs-lookup"><span data-stu-id="95eb0-253">`policies\apis\<api name>\` - if you have any policies defined at API scope, they are contained in this folder.</span></span>
* <span data-ttu-id="95eb0-254">`policies\apis\<api name>\<operation name>\`mappa - Ha minden házirendet a hatókör művelet van megadva, akkor az a mappában található `<operation name>.xml` fájlok, amelyek kapcsolódnak a házirend-utasításoknál toohello minden művelethez.</span><span class="sxs-lookup"><span data-stu-id="95eb0-254">`policies\apis\<api name>\<operation name>\` folder - if you have any policies defined at operation scope, they are contained in this folder in `<operation name>.xml` files that map toohello policy statements for each operation.</span></span>
* <span data-ttu-id="95eb0-255">`policies\products\`-Ha minden házirendet a termék hatókörben van megadva, ezt tartalmazza ezt a mappát, amelybe `<product name>.xml` fájlok, amelyek az egyes termékek toohello házirend-utasításoknál kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="95eb0-255">`policies\products\` - if you have any policies defined at product scope, they are contained in this folder, which contains `<product name>.xml` files that map toohello policy statements for each product.</span></span>

### <a name="portalstyles-folder"></a><span data-ttu-id="95eb0-256">portalStyles mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-256">portalStyles folder</span></span>
<span data-ttu-id="95eb0-257">Hello `portalStyles` mappa tartalmazza a konfigurációs és stílus lapok developer portálon testreszabni hello szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="95eb0-257">hello `portalStyles` folder contains configuration and style sheets for developer portal customizations for hello service instance.</span></span>

* <span data-ttu-id="95eb0-258">`portalStyles\configuration.json`-hello fejlesztői portál által használt hello stíluslapok hello nevét tartalmazza</span><span class="sxs-lookup"><span data-stu-id="95eb0-258">`portalStyles\configuration.json` - contains hello names of hello style sheets used by hello developer portal</span></span>
* <span data-ttu-id="95eb0-259">`portalStyles\<style name>.css`-minden `<style name>.css` fájl tartalmazza a stílusokat hello fejlesztői portálján (`Preview.css` és `Production.css` alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="95eb0-259">`portalStyles\<style name>.css` - each `<style name>.css` file contains styles for hello developer portal (`Preview.css` and `Production.css` by default).</span></span>

### <a name="products-folder"></a><span data-ttu-id="95eb0-260">termékek mappa</span><span class="sxs-lookup"><span data-stu-id="95eb0-260">products folder</span></span>
<span data-ttu-id="95eb0-261">Hello `products` mappa tartalmaz egy mappát az egyes termékek hello szolgáltatáspéldány definiálva.</span><span class="sxs-lookup"><span data-stu-id="95eb0-261">hello `products` folder contains a folder for each product defined in hello service instance.</span></span>

* <span data-ttu-id="95eb0-262">`products\<product name>\configuration.json`-hello termék hello beállítani.</span><span class="sxs-lookup"><span data-stu-id="95eb0-262">`products\<product name>\configuration.json` - this is hello configuration for hello product.</span></span> <span data-ttu-id="95eb0-263">Ez a hello ugyanazokat az információkat, amely akkor tér vissza, ha toocall hello [beolvasni a termék](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) műveletet.</span><span class="sxs-lookup"><span data-stu-id="95eb0-263">This is hello same information that would be returned if you were toocall hello [Get a specific product](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operation.</span></span>
* <span data-ttu-id="95eb0-264">`products\<product name>\product.description.html`-Ez hello termék hello leírása és toohello megfelel `description` hello tulajdonságának [termék entitás](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) a hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="95eb0-264">`products\<product name>\product.description.html` - this is hello description of hello product and corresponds toohello `description` property of hello [product entity](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in hello REST API.</span></span>

### <a name="templates"></a><span data-ttu-id="95eb0-265">sablonok</span><span class="sxs-lookup"><span data-stu-id="95eb0-265">templates</span></span>
<span data-ttu-id="95eb0-266">Hello `templates` mappa hello konfigurációját tartalmazza [e-mail sablonok](api-management-howto-configure-notifications.md) hello szolgáltatás példányának.</span><span class="sxs-lookup"><span data-stu-id="95eb0-266">hello `templates` folder contains configuration for hello [email templates](api-management-howto-configure-notifications.md) of hello service instance.</span></span>

* <span data-ttu-id="95eb0-267">`<template name>\configuration.json`-hello e-mail sablon hello beállítani.</span><span class="sxs-lookup"><span data-stu-id="95eb0-267">`<template name>\configuration.json` - this is hello configuration for hello email template.</span></span>
* <span data-ttu-id="95eb0-268">`<template name>\body.html`-Ez az hello hello e-mail sablon szövegtörzsét.</span><span class="sxs-lookup"><span data-stu-id="95eb0-268">`<template name>\body.html` - this is hello body of hello email template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95eb0-269">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95eb0-269">Next steps</span></span>
<span data-ttu-id="95eb0-270">Más módokon toomanage olvashat a szolgáltatáspéldány, lásd:</span><span class="sxs-lookup"><span data-stu-id="95eb0-270">For information on other ways toomanage your service instance, see:</span></span>

* <span data-ttu-id="95eb0-271">A szolgáltatáspéldány hello a következő PowerShell-parancsmagok használatával kezelése</span><span class="sxs-lookup"><span data-stu-id="95eb0-271">Manage your service instance using hello following PowerShell cmdlets</span></span>
  * [<span data-ttu-id="95eb0-272">Szolgáltatások üzembe helyezése – PowerShell-parancsmagok leírása</span><span class="sxs-lookup"><span data-stu-id="95eb0-272">Service deployment PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [<span data-ttu-id="95eb0-273">Szolgáltatás-felügyelet PowerShell parancsmag-referencia</span><span class="sxs-lookup"><span data-stu-id="95eb0-273">Service management PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* <span data-ttu-id="95eb0-274">A szolgáltatáspéldány hello publisher portálon kezelése</span><span class="sxs-lookup"><span data-stu-id="95eb0-274">Manage your service instance in hello publisher portal</span></span>
  * [<span data-ttu-id="95eb0-275">Az első API kezelése</span><span class="sxs-lookup"><span data-stu-id="95eb0-275">Manage your first API</span></span>](api-management-get-started.md)
* <span data-ttu-id="95eb0-276">A szolgáltatáspéldány hello REST API használatával kezelése</span><span class="sxs-lookup"><span data-stu-id="95eb0-276">Manage your service instance using hello REST API</span></span>
  * [<span data-ttu-id="95eb0-277">API Management REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="95eb0-277">API Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="95eb0-278">Áttekintő videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="95eb0-278">Watch a video overview</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Configuration-over-Git/player]
> 
> 

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




