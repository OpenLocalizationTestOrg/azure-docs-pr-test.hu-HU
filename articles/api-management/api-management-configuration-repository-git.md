---
title: "Konfigurálja a használatával a Git - Azure API Management szolgáltatást |} Microsoft Docs"
description: "Ismerje meg, mentse, és a Git használatával az API Management-konfigurációjának beállítása."
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
ms.openlocfilehash: f5d6bb7ccbf15424e9940ccda2fac668a2af5a57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a><span data-ttu-id="3b13b-103">Mentse, és a Git használatával az API Management-konfigurációjának beállítása</span><span class="sxs-lookup"><span data-stu-id="3b13b-103">How to save and configure your API Management service configuration using Git</span></span>
> 
> 

<span data-ttu-id="3b13b-104">Minden API Management service-példány konfigurációs és a szolgáltatáspéldány metaadatok kapcsolatos adatokat tartalmazó konfigurációs adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="3b13b-104">Each API Management service instance maintains a configuration database that contains information about the configuration and metadata for the service instance.</span></span> <span data-ttu-id="3b13b-105">Is módosítható a szolgáltatáspéldány módosításával a közzétevő portált a megfelelő értéket, PowerShell-parancsmag használatával, vagy a REST API-hívással.</span><span class="sxs-lookup"><span data-stu-id="3b13b-105">Changes can be made to the service instance by changing a setting in the publisher portal, using a PowerShell cmdlet, or making a REST API call.</span></span> <span data-ttu-id="3b13b-106">Ezek a módszerek mellett is kezelhet, a példány konfigurációjának Git használatával, a szolgáltatás felügyeleti lehetőségeket, mint engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="3b13b-106">In addition to these methods, you can also manage your service instance configuration using Git, enabling service management scenarios such as:</span></span>

* <span data-ttu-id="3b13b-107">Konfigurációs rendszerverzió - töltse le, és tárolja a szolgáltatás konfigurációs különböző verziói</span><span class="sxs-lookup"><span data-stu-id="3b13b-107">Configuration versioning - download and store different versions of your service configuration</span></span>
* <span data-ttu-id="3b13b-108">Konfigurációs módosítások tömeges - módosítja a szolgáltatás konfigurációját a helyi tárházban több részét, és a módosítások a kiszolgáló integrálása egyetlen műveletben</span><span class="sxs-lookup"><span data-stu-id="3b13b-108">Bulk configuration changes - make changes to multiple parts of your service configuration in your local repository and integrate the changes back to the server with a single operation</span></span>
* <span data-ttu-id="3b13b-109">Munkafolyamat - és ismerős Git toolchain használja a Git tooling és, amelyek már ismeri a munkafolyamatok</span><span class="sxs-lookup"><span data-stu-id="3b13b-109">Familiar Git toolchain and workflow - use the Git tooling and workflows that you are already familiar with</span></span>

<span data-ttu-id="3b13b-110">Az alábbi ábra a különböző módokon konfigurálhatja az API Management szolgáltatáspéldány áttekintését mutatja.</span><span class="sxs-lookup"><span data-stu-id="3b13b-110">The following diagram shows an overview of the different ways to configure your API Management service instance.</span></span>

![Git konfigurálása][api-management-git-configure]

<span data-ttu-id="3b13b-112">Amikor módosítja a szolgáltatás a közzétevő portal, PowerShell-parancsmagokkal vagy a REST API-t használó, kezelt a szolgáltatás konfigurációs adatbázis használata a `https://{name}.management.azure-api.net` végpont, a diagram jobb oldalán látható módon.</span><span class="sxs-lookup"><span data-stu-id="3b13b-112">When you make changes to your service using the publisher portal, PowerShell cmdlets, or the REST API, you are managing your service configuration database using the `https://{name}.management.azure-api.net` endpoint, as shown on the right side of the diagram.</span></span> <span data-ttu-id="3b13b-113">Bal oldalán található a diagram bemutatja, hogyan kezelheti a Git használatával konfigurációjának és a szolgáltatás Git-tárházban található `https://{name}.scm.azure-api.net`.</span><span class="sxs-lookup"><span data-stu-id="3b13b-113">The left side of the diagram illustrates how you can manage your service configuration using Git and Git repository for your service located at `https://{name}.scm.azure-api.net`.</span></span>

<span data-ttu-id="3b13b-114">Az alábbi lépések nyújtanak a Git használatával az API Management szolgáltatáspéldány kezelését.</span><span class="sxs-lookup"><span data-stu-id="3b13b-114">The following steps provide an overview of managing your API Management service instance using Git.</span></span>

1. <span data-ttu-id="3b13b-115">A szolgáltatás a Git-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="3b13b-115">Access Git configuration in your service</span></span>
2. <span data-ttu-id="3b13b-116">A szolgáltatás konfigurációs adatbázis mentse a Git-tárház</span><span class="sxs-lookup"><span data-stu-id="3b13b-116">Save your service configuration database to your Git repository</span></span>
3. <span data-ttu-id="3b13b-117">A helyi számítógépen a Git-tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="3b13b-117">Clone the Git repo to your local machine</span></span>
4. <span data-ttu-id="3b13b-118">A legújabb tárház le a helyi számítógép és a véglegesítési és leküldéses módosításokat visszavonni a tárházban</span><span class="sxs-lookup"><span data-stu-id="3b13b-118">Pull the latest repo down to your local machine, and commit and push changes back to your repo</span></span>
5. <span data-ttu-id="3b13b-119">A módosítások a tárházból üzembe helyezés a szolgáltatás konfigurációs adatbázis</span><span class="sxs-lookup"><span data-stu-id="3b13b-119">Deploy the changes from your repo into your service configuration database</span></span>

<span data-ttu-id="3b13b-120">Ez a cikk ismerteti, hogyan lehet engedélyezése és a Git segítségével kezelheti a szolgáltatás konfigurációját, és nyújt a fájlok és mappák a Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="3b13b-120">This article describes how to enable and use Git to manage your service configuration and provides a reference for the files and folders in the Git repository.</span></span>

## <a name="access-git-configuration-in-your-service"></a><span data-ttu-id="3b13b-121">A szolgáltatás a Git-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="3b13b-121">Access Git configuration in your service</span></span>
<span data-ttu-id="3b13b-122">A közzétevő portál jobb felső sarkában a Git ikon megtekintésével gyorsan megtekintheti a Git konfigurációs állapotát.</span><span class="sxs-lookup"><span data-stu-id="3b13b-122">You can quickly view the status of your Git configuration by viewing the Git icon in the upper-right corner of the publisher portal.</span></span> <span data-ttu-id="3b13b-123">Ebben a példában az állapotüzenet azt jelzi, amely nincs vannak nem mentett módosításokat a tárházba.</span><span class="sxs-lookup"><span data-stu-id="3b13b-123">In this example, the status message indicates that there are unsaved changes to the repository.</span></span> <span data-ttu-id="3b13b-124">Ennek az az oka az API Management szolgáltatás konfigurációs adatbázis még nem lett mentve a tárházba.</span><span class="sxs-lookup"><span data-stu-id="3b13b-124">This is because the API Management service configuration database has not yet been saved to the repository.</span></span>

![Git állapota][api-management-git-icon-enable]

<span data-ttu-id="3b13b-126">Megtekintheti, és a Git konfigurációs beállításait, kattintson a Git ikonra, vagy kattintson a **biztonsági** menü, és keresse meg a **konfigurációs tárház** fülre.</span><span class="sxs-lookup"><span data-stu-id="3b13b-126">To view and configure your Git configuration settings, you can either click the Git icon, or click the **Security** menu and navigate to the **Configuration repository** tab.</span></span>

![GIT engedélyezése][api-management-enable-git]

> [!IMPORTANT]
> <span data-ttu-id="3b13b-128">Bármely kulcsait, tulajdonságait a tárházban fogja tárolni, és az előzmények marad, amíg meg nem definiált tiltsa le, és engedélyezze újra a Git-hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="3b13b-128">Any secrets that are not defined as properties will be stored in the repository and will remain in its history until you disable and re-enable Git access.</span></span> <span data-ttu-id="3b13b-129">Tulajdonságok adjon meg egy biztonságos helyen kezelhetők a állandó karakterlánc-értékek, többek között a titkos kulcsok, összes API konfigurálása és házirendek, így nem kell közvetlenül a házirend-utasításoknál letölthetők.</span><span class="sxs-lookup"><span data-stu-id="3b13b-129">Properties provide a secure place to manage constant string values, including secrets, across all API configuration and policies, so you don't have to store them directly in your policy statements.</span></span> <span data-ttu-id="3b13b-130">További információkért lásd: [tulajdonságok használata az Azure API-felügyeleti házirendek](api-management-howto-properties.md).</span><span class="sxs-lookup"><span data-stu-id="3b13b-130">For more information, see [How to use properties in Azure API Management policies](api-management-howto-properties.md).</span></span>
> 
> 

<span data-ttu-id="3b13b-131">Engedélyezésével vagy letiltásával Git hozzáférés a REST API használatával kapcsolatos tudnivalókért lásd: [engedélyezheti vagy letilthatja a REST API használatával Git hozzáférést](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span><span class="sxs-lookup"><span data-stu-id="3b13b-131">For information on enabling or disabling Git access using the REST API, see [Enable or disable Git access using the REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span></span>

## <a name="to-save-the-service-configuration-to-the-git-repository"></a><span data-ttu-id="3b13b-132">A szolgáltatáskonfiguráció a Git-tárházba mentéséhez</span><span class="sxs-lookup"><span data-stu-id="3b13b-132">To save the service configuration to the Git repository</span></span>
<span data-ttu-id="3b13b-133">Az első lépéseket, mielőtt a tárház klónozása, hogy a szolgáltatáskonfiguráció a jelenlegi állapotában a tárházba mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="3b13b-133">The first step before cloning the repository is to save the current state of the service configuration to the repository.</span></span> <span data-ttu-id="3b13b-134">Kattintson a **mentés konfigurációs tárházba**.</span><span class="sxs-lookup"><span data-stu-id="3b13b-134">Click **Save configuration to repository**.</span></span>

![Konfiguráció mentése][api-management-save-configuration]

<span data-ttu-id="3b13b-136">Adja meg a kívánt módosításokat a megerősítő képernyőn a, és kattintson a **Ok** mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="3b13b-136">Make any desired changes on the confirmation screen and click **Ok** to save.</span></span>

![Konfiguráció mentése][api-management-save-configuration-confirm]

<span data-ttu-id="3b13b-138">A konfiguráció mentése után néhány percet, és az állapotot a tárház jelenik meg, beleértve a dátum és idő az utolsó konfigurációváltozás és a szolgáltatás konfigurációját és a tárház között a legutóbbi szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="3b13b-138">After a few moments the configuration is saved, and the configuration status of the repository is displayed, including the date and time of the last configuration change and the last synchronization between the service configuration and the repository.</span></span>

![Konfiguráció állapota][api-management-configuration-status]

<span data-ttu-id="3b13b-140">Ha a tárház menti a konfigurációt, akkor klónozható.</span><span class="sxs-lookup"><span data-stu-id="3b13b-140">Once the configuration is saved to the repository, it can be cloned.</span></span>

<span data-ttu-id="3b13b-141">Információ a REST API használatával a művelet végrehajtása: [véglegesítési konfiguráció pillanatkép-REST API használatával](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span><span class="sxs-lookup"><span data-stu-id="3b13b-141">For information on performing this operation using the REST API, see [Commit configuration snapshot using the REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span></span>

## <a name="to-clone-the-repository-to-your-local-machine"></a><span data-ttu-id="3b13b-142">Klónozza a tárházat a helyi számítógép számára</span><span class="sxs-lookup"><span data-stu-id="3b13b-142">To clone the repository to your local machine</span></span>
<span data-ttu-id="3b13b-143">Klónozza a tárházat, kell az URL-cím a tárház, a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="3b13b-143">To clone a repository, you need the URL to your repository, a user name, and a password.</span></span> <span data-ttu-id="3b13b-144">A felhasználói nevet és az URL-cím jelennek meg az tetején a **konfigurációs tárház** fülre.</span><span class="sxs-lookup"><span data-stu-id="3b13b-144">The user name and URL are displayed near the top of the **Configuration repository** tab.</span></span>

![Git-klón][api-management-configuration-git-clone]

<span data-ttu-id="3b13b-146">A jelszó alján jön létre a **konfigurációs tárház** fülre.</span><span class="sxs-lookup"><span data-stu-id="3b13b-146">The password is generated at the bottom of the **Configuration repository** tab.</span></span>

![Jelszó létrehozása][api-management-generate-password]

<span data-ttu-id="3b13b-148">Olyan jelszót létrehozni, először győződjön meg róla, hogy a **lejárati** állítsa be a kívánt lejárati dátumát és idejét, és kattintson a **azonosító létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3b13b-148">To generate a password, first ensure that the **Expiry** is set to the desired expiration date and time, and then click **Generate Token**.</span></span>

![Jelszó][api-management-password]

> [!IMPORTANT]
> <span data-ttu-id="3b13b-150">Jegyezze fel ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="3b13b-150">Make a note of this password.</span></span> <span data-ttu-id="3b13b-151">Ha bezárja ezt az oldalt a jelszó nem jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="3b13b-151">Once you leave this page the password will not be displayed again.</span></span>
> 
> 

<span data-ttu-id="3b13b-152">Az alábbi példák a Git Bash eszközt használja [Git for Windows](http://www.git-scm.com/downloads) , de bármely, a ismeri a Git-eszközt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3b13b-152">The following examples use the Git Bash tool from [Git for Windows](http://www.git-scm.com/downloads) but you can use any Git tool that you are familiar with.</span></span>

<span data-ttu-id="3b13b-153">A Git eszköz nyissa meg a kívánt mappa, és futtassa a következő parancsot a helyi számítógépen, a közzétevő portál által szolgáltatott paranccsal git-tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="3b13b-153">Open your Git tool in the desired folder and run the following command to clone the git repository to your local machine, using the command provided by the publisher portal.</span></span>

```
git clone https://bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="3b13b-154">Adja meg a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="3b13b-154">Provide the user name and password when prompted.</span></span>

<span data-ttu-id="3b13b-155">Ha bármilyen hiba jelentkezik, próbálja meg módosítani a `git clone` parancs tartalmazza a felhasználónevet és jelszót, a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="3b13b-155">If you receive any errors, try modifying your `git clone` command to include the user name and password, as shown in the following example.</span></span>

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="3b13b-156">Ez biztosítja, hogy a hiba, ha próbálja meg a jelszót a parancs része kódolás URL-címet.</span><span class="sxs-lookup"><span data-stu-id="3b13b-156">If this provides an error, try URL encoding the password portion of the command.</span></span> <span data-ttu-id="3b13b-157">Nyissa meg a Visual Studio, és adja ki a következő parancsot a egy gyors módja, ha ehhez a **parancsablakban**.</span><span class="sxs-lookup"><span data-stu-id="3b13b-157">One quick way to do this is to open Visual Studio, and issue the following command in the **Immediate Window**.</span></span> <span data-ttu-id="3b13b-158">Megnyitásához a **parancsablakban**, nyissa meg a bármely megoldás vagy a projektet a Visual Studio (vagy hozzon létre egy új üres konzolalkalmazást), és válassza a **Windows**, **Immediate** a a **Debug** menü.</span><span class="sxs-lookup"><span data-stu-id="3b13b-158">To open the **Immediate Window**, open any solution or project in Visual Studio (or create a new empty console application), and choose **Windows**, **Immediate** from the **Debug** menu.</span></span>

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

<span data-ttu-id="3b13b-159">A kódolt jelszót használhatja a felhasználó nevét és a tárház helye a git parancs összeállításához.</span><span class="sxs-lookup"><span data-stu-id="3b13b-159">Use the encoded password along with your user name and repository location to construct the git command.</span></span>

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="3b13b-160">Ha a tárház klónozták tekintheti meg és a helyi rendszer használható.</span><span class="sxs-lookup"><span data-stu-id="3b13b-160">Once the repository is cloned you can view and work with it in your local file system.</span></span> <span data-ttu-id="3b13b-161">További információkért lásd: [fájl- és helyi Git-tárház hivatkozás szerkezeti](#file-and-folder-structure-reference-of-local-git-repository).</span><span class="sxs-lookup"><span data-stu-id="3b13b-161">For more information, see [File and folder structure reference of local Git repository](#file-and-folder-structure-reference-of-local-git-repository).</span></span>

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a><span data-ttu-id="3b13b-162">A helyi tárház frissítheti a legfrissebb példányának konfigurációja</span><span class="sxs-lookup"><span data-stu-id="3b13b-162">To update your local repository with the most current service instance configuration</span></span>
<span data-ttu-id="3b13b-163">Ha módosítja az API Management szolgáltatáspéldány publisher portálon vagy REST API használatával, mentenie kell ezeket a módosításokat a tárházba a helyi tárház legújabb módosításainak frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="3b13b-163">If you make changes to your API Management service instance in the publisher portal or using the REST API, you must save these changes to the repository before you can update your local repository with the latest changes.</span></span> <span data-ttu-id="3b13b-164">Ehhez kattintson **mentés konfigurációs tárházba** a a **konfigurációs tárház** a közzétevő portál lapot, és hogyan adhat ki az alábbi parancsot a helyi tárházba.</span><span class="sxs-lookup"><span data-stu-id="3b13b-164">To do this, click **Save configuration to repository** on the **Configuration repository** tab in the publisher portal, and then issue the following command in your local repository.</span></span>

```
git pull
```

<span data-ttu-id="3b13b-165">Futtatása előtt `git pull` érdekében, hogy a mappa a helyi tárház.</span><span class="sxs-lookup"><span data-stu-id="3b13b-165">Before running `git pull` ensure that you are in the folder for your local repository.</span></span> <span data-ttu-id="3b13b-166">Ha befejezte a `git clone` parancsot, majd módosítsa a könyvtárat a tárház a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="3b13b-166">If you have just completed the `git clone` command, then you must change the directory to your repo by running a command like the following.</span></span>

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a><span data-ttu-id="3b13b-167">Az a helyi tárház változásainak leküldése a kiszolgáló-tárház</span><span class="sxs-lookup"><span data-stu-id="3b13b-167">To push changes from your local repo to the server repo</span></span>
<span data-ttu-id="3b13b-168">A tárház változásainak leküldése a helyi tárházból a kiszolgáló, meg kell a változtatások véglegesítése a határidő, majd küldje le őket a kiszolgáló tárház.</span><span class="sxs-lookup"><span data-stu-id="3b13b-168">To push changes from your local repository to the server repository, you must commit your changes and then push them to the server repository.</span></span> <span data-ttu-id="3b13b-169">A módosítások véglegesítéséhez, a Git parancs eszköz, a helyi tárház directory váltani, és a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="3b13b-169">To commit your changes, open your Git command tool, switch to the directory of your local repository, and issue the following commands.</span></span>

```
git add --all
git commit -m "Description of your changes"
```

<span data-ttu-id="3b13b-170">Kiszolgálóra történő leküldésére összes a véglegesíti a, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="3b13b-170">To push all of the commits to the server, run the following command.</span></span>

```
git push
```

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a><span data-ttu-id="3b13b-171">A szolgáltatás konfigurációs változásokat az API Management szolgáltatáspéldány központi telepítése</span><span class="sxs-lookup"><span data-stu-id="3b13b-171">To deploy any service configuration changes to the API Management service instance</span></span>
<span data-ttu-id="3b13b-172">Ha a helyi módosításokat vannak, és a kiszolgáló tárházba leküldött, az API Management szolgáltatáspéldány telepíthetné őket.</span><span class="sxs-lookup"><span data-stu-id="3b13b-172">Once your local changes are committed and pushed to the server repository, you can deploy them to your API Management service instance.</span></span>

![Üzembe helyezés][api-management-configuration-deploy]

<span data-ttu-id="3b13b-174">Információ a REST API használatával a művelet végrehajtása: [telepítése Git vált, a REST API használatával konfigurációs adatbázis](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span><span class="sxs-lookup"><span data-stu-id="3b13b-174">For information on performing this operation using the REST API, see [Deploy Git changes to configuration database using the REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span></span>

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a><span data-ttu-id="3b13b-175">Fájl- és helyi Git-tárház szerkezete-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="3b13b-175">File and folder structure reference of local Git repository</span></span>
<span data-ttu-id="3b13b-176">A fájlok és mappák a helyi git-tárházban tartalmaz a szolgáltatáspéldány konfigurációs adatait.</span><span class="sxs-lookup"><span data-stu-id="3b13b-176">The files and folders in the local git repository contain the configuration information about the service instance.</span></span>

| <span data-ttu-id="3b13b-177">Elem</span><span class="sxs-lookup"><span data-stu-id="3b13b-177">Item</span></span> | <span data-ttu-id="3b13b-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="3b13b-178">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3b13b-179">Api-felügyeleti gyökérmappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-179">root api-management folder</span></span> |<span data-ttu-id="3b13b-180">A szolgáltatáspéldány legfelső szintű konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="3b13b-180">Contains top-level configuration for the service instance</span></span> |
| <span data-ttu-id="3b13b-181">API-k mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-181">apis folder</span></span> |<span data-ttu-id="3b13b-182">Az API-k a szolgáltatáspéldány a konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="3b13b-182">Contains the configuration for the apis in the service instance</span></span> |
| <span data-ttu-id="3b13b-183">csoportok mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-183">groups folder</span></span> |<span data-ttu-id="3b13b-184">A szolgáltatáspéldány a csoportok konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="3b13b-184">Contains the configuration for the groups in the service instance</span></span> |
| <span data-ttu-id="3b13b-185">házirend mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-185">policies folder</span></span> |<span data-ttu-id="3b13b-186">A szolgáltatáspéldány házirendjeinek tartalmazza</span><span class="sxs-lookup"><span data-stu-id="3b13b-186">Contains the policies in the service instance</span></span> |
| <span data-ttu-id="3b13b-187">portalStyles mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-187">portalStyles folder</span></span> |<span data-ttu-id="3b13b-188">A fejlesztői portálon a testreszabások a szolgáltatáspéldány a konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="3b13b-188">Contains the configuration for the developer portal customizations in the service instance</span></span> |
| <span data-ttu-id="3b13b-189">termékek mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-189">products folder</span></span> |<span data-ttu-id="3b13b-190">A szolgáltatáspéldány a termék konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="3b13b-190">Contains the configuration for the products in the service instance</span></span> |
| <span data-ttu-id="3b13b-191">sablonok mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-191">templates folder</span></span> |<span data-ttu-id="3b13b-192">Az e-mail sablonok a szolgáltatáspéldány a konfigurációját tartalmazza</span><span class="sxs-lookup"><span data-stu-id="3b13b-192">Contains the configuration for the email templates in the service instance</span></span> |

<span data-ttu-id="3b13b-193">Minden mappa tartalmazhat egy vagy több fájlt, és bizonyos esetekben egy vagy több mappát, például egy mappát az egyes API-t, a termék vagy a csoport.</span><span class="sxs-lookup"><span data-stu-id="3b13b-193">Each folder can contain one or more files, and in some cases one or more folders, for example a folder for each API, product, or group.</span></span> <span data-ttu-id="3b13b-194">A fájlok minden egyes mappákban lévő mappa nevével entitástípus vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="3b13b-194">The files within each folder are specific for the entity type described by the folder name.</span></span>

| <span data-ttu-id="3b13b-195">Fájltípus</span><span class="sxs-lookup"><span data-stu-id="3b13b-195">File type</span></span> | <span data-ttu-id="3b13b-196">Cél</span><span class="sxs-lookup"><span data-stu-id="3b13b-196">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="3b13b-197">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="3b13b-197">json</span></span> |<span data-ttu-id="3b13b-198">A megfelelő entitás konfigurációs adatait</span><span class="sxs-lookup"><span data-stu-id="3b13b-198">Configuration information about the respective entity</span></span> |
| <span data-ttu-id="3b13b-199">HTML</span><span class="sxs-lookup"><span data-stu-id="3b13b-199">html</span></span> |<span data-ttu-id="3b13b-200">Entitás, gyakran megjelenik a fejlesztői portálon leírása</span><span class="sxs-lookup"><span data-stu-id="3b13b-200">Descriptions about the entity, often displayed in the developer portal</span></span> |
| <span data-ttu-id="3b13b-201">xml</span><span class="sxs-lookup"><span data-stu-id="3b13b-201">xml</span></span> |<span data-ttu-id="3b13b-202">Házirend-utasítások</span><span class="sxs-lookup"><span data-stu-id="3b13b-202">Policy statements</span></span> |
| <span data-ttu-id="3b13b-203">CSS</span><span class="sxs-lookup"><span data-stu-id="3b13b-203">css</span></span> |<span data-ttu-id="3b13b-204">A fejlesztői portálon testreszabáshoz stíluslapok</span><span class="sxs-lookup"><span data-stu-id="3b13b-204">Style sheets for developer portal customization</span></span> |

<span data-ttu-id="3b13b-205">Ezeket a fájlokat létrehozása, törlése, szerkeszthető, és a helyi fájlrendszerben felügyelt, és a telepített módosítások a az API Management szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="3b13b-205">These files can be created, deleted, edited, and managed on your local file system, and the changes deployed back to the your API Management service instance.</span></span>

> [!NOTE]
> <span data-ttu-id="3b13b-206">A következő entitások nem találhatók a Git-tárházban, és nem lehet konfigurálni a Git használatával.</span><span class="sxs-lookup"><span data-stu-id="3b13b-206">The following entities are not contained in the Git repository and cannot be configured using Git.</span></span>
> 
> * <span data-ttu-id="3b13b-207">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="3b13b-207">Users</span></span>
> * <span data-ttu-id="3b13b-208">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="3b13b-208">Subscriptions</span></span>
> * <span data-ttu-id="3b13b-209">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="3b13b-209">Properties</span></span>
> * <span data-ttu-id="3b13b-210">Fejlesztői portálon entitások eltérő stílusok</span><span class="sxs-lookup"><span data-stu-id="3b13b-210">Developer portal entities other than styles</span></span>
> 
> 

### <a name="root-api-management-folder"></a><span data-ttu-id="3b13b-211">Api-felügyeleti gyökérmappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-211">Root api-management folder</span></span>
<span data-ttu-id="3b13b-212">A legfelső szintű `api-management` a mappa tartalmaz egy `configuration.json` a szolgáltatáspéldány, a következő formátumban legfelső szintű információkat tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="3b13b-212">The root `api-management` folder contains a `configuration.json` file that contains top-level information about the service instance in the following format.</span></span>

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

<span data-ttu-id="3b13b-213">Az első négy beállítások (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, és `UserRegistrationTermsConsentRequired`) leképezése a következő beállításokat, a a **identitások** lapra a **biztonsági** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3b13b-213">The first four settings (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, and `UserRegistrationTermsConsentRequired`) map to the following settings on the **Identities** tab in the **Security** section.</span></span>

| <span data-ttu-id="3b13b-214">Azonosító beállítása</span><span class="sxs-lookup"><span data-stu-id="3b13b-214">Identity setting</span></span> | <span data-ttu-id="3b13b-215">Van leképezve</span><span class="sxs-lookup"><span data-stu-id="3b13b-215">Maps to</span></span> |
| --- | --- |
| <span data-ttu-id="3b13b-216">RegistrationEnabled</span><span class="sxs-lookup"><span data-stu-id="3b13b-216">RegistrationEnabled</span></span> |<span data-ttu-id="3b13b-217">**A névtelen felhasználók átirányítása bejelentkezési oldal** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="3b13b-217">**Redirect anonymous users to sign-in page** checkbox</span></span> |
| <span data-ttu-id="3b13b-218">UserRegistrationTerms</span><span class="sxs-lookup"><span data-stu-id="3b13b-218">UserRegistrationTerms</span></span> |<span data-ttu-id="3b13b-219">**Használati feltételek a felhasználói regisztráció** szövegmező</span><span class="sxs-lookup"><span data-stu-id="3b13b-219">**Terms of use on user signup** textbox</span></span> |
| <span data-ttu-id="3b13b-220">UserRegistrationTermsEnabled</span><span class="sxs-lookup"><span data-stu-id="3b13b-220">UserRegistrationTermsEnabled</span></span> |<span data-ttu-id="3b13b-221">**Használati feltételek megjelenítése a bejelentkezési lapon** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="3b13b-221">**Show terms of use on signup page** checkbox</span></span> |
| <span data-ttu-id="3b13b-222">UserRegistrationTermsConsentRequired</span><span class="sxs-lookup"><span data-stu-id="3b13b-222">UserRegistrationTermsConsentRequired</span></span> |<span data-ttu-id="3b13b-223">**Hozzájárulás szükséges** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="3b13b-223">**Require consent** checkbox</span></span> |

![Identitás][api-management-identity-settings]

<span data-ttu-id="3b13b-225">A következő négy beállítások (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, és `DelegationValidationKey`) leképezése a következő beállításokat, a a **delegálás** lapra a **biztonsági** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3b13b-225">The next four settings (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, and `DelegationValidationKey`) map to the following settings on the **Delegation** tab in the **Security** section.</span></span>

| <span data-ttu-id="3b13b-226">Delegálási beállítás</span><span class="sxs-lookup"><span data-stu-id="3b13b-226">Delegation setting</span></span> | <span data-ttu-id="3b13b-227">Van leképezve</span><span class="sxs-lookup"><span data-stu-id="3b13b-227">Maps to</span></span> |
| --- | --- |
| <span data-ttu-id="3b13b-228">DelegationEnabled</span><span class="sxs-lookup"><span data-stu-id="3b13b-228">DelegationEnabled</span></span> |<span data-ttu-id="3b13b-229">**Delegált bejelentkezési és regisztrációs** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="3b13b-229">**Delegate sign-in & sign-up** checkbox</span></span> |
| <span data-ttu-id="3b13b-230">DelegationUrl</span><span class="sxs-lookup"><span data-stu-id="3b13b-230">DelegationUrl</span></span> |<span data-ttu-id="3b13b-231">**Delegálás végponti URL-cím** szövegmező</span><span class="sxs-lookup"><span data-stu-id="3b13b-231">**Delegation endpoint URL** textbox</span></span> |
| <span data-ttu-id="3b13b-232">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="3b13b-232">DelegatedSubscriptionEnabled</span></span> |<span data-ttu-id="3b13b-233">**Termék-előfizetéshez delegálása** jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="3b13b-233">**Delegate product subscription** checkbox</span></span> |
| <span data-ttu-id="3b13b-234">DelegationValidationKey</span><span class="sxs-lookup"><span data-stu-id="3b13b-234">DelegationValidationKey</span></span> |<span data-ttu-id="3b13b-235">**Érvényesítési kulcs delegálása** szövegmező</span><span class="sxs-lookup"><span data-stu-id="3b13b-235">**Delegate Validation Key** textbox</span></span> |

![A delegálási beállításokat][api-management-delegation-settings]

<span data-ttu-id="3b13b-237">A végső beállítás `$ref-policy`, a globális utasítások házirendfájl a szolgáltatáspéldány van leképezve.</span><span class="sxs-lookup"><span data-stu-id="3b13b-237">The final setting, `$ref-policy`, maps to the global policy statements file for the service instance.</span></span>

### <a name="apis-folder"></a><span data-ttu-id="3b13b-238">API-k mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-238">apis folder</span></span>
<span data-ttu-id="3b13b-239">A `apis` mappa tartalmaz egy mappát minden API-nak a szolgáltatáspéldány, amely a következő elemeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3b13b-239">The `apis` folder contains a folder for each API in the service instance which contains the following items.</span></span>

* <span data-ttu-id="3b13b-240">`apis\<api name>\configuration.json`– Ez a konfiguráció az API-hoz és a háttérkiszolgáló URL-címe és a műveletek tartalmaz információkat.</span><span class="sxs-lookup"><span data-stu-id="3b13b-240">`apis\<api name>\configuration.json` - this is the configuration for the API and contains information about the backend service URL and the operations.</span></span> <span data-ttu-id="3b13b-241">Ez az ugyanazokat az információkat, akkor adja vissza, ha hívása [egy meghatározott API beszerzésének](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) rendelkező `export=true` a `application/json` formátumban.</span><span class="sxs-lookup"><span data-stu-id="3b13b-241">This is the same information that would be returned if you were to call [Get a specific API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) with `export=true` in `application/json` format.</span></span>
* <span data-ttu-id="3b13b-242">`apis\<api name>\api.description.html`-Ez az API leírása, megfelel-e a `description` tulajdonsága a [API entitás](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="3b13b-242">`apis\<api name>\api.description.html` - this is the description of the API and corresponds to the `description` property of the [API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span></span>
* <span data-ttu-id="3b13b-243">`apis\<api name>\operations\`– Ez a mappa tartalmaz `<operation name>.description.html` fájlokat, a műveletek az API-ban van leképezve.</span><span class="sxs-lookup"><span data-stu-id="3b13b-243">`apis\<api name>\operations\` - this folder contains `<operation name>.description.html` files that map to the operations in the API.</span></span> <span data-ttu-id="3b13b-244">Minden fájl tartalmazza az API-ban, amely egyetlen műveletben leírása a `description` tulajdonsága a [művelet entitás](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) a REST API-ban.</span><span class="sxs-lookup"><span data-stu-id="3b13b-244">Each file contains the description of a single operation in the API which maps to the `description` property of the [operation entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in the REST API.</span></span>

### <a name="groups-folder"></a><span data-ttu-id="3b13b-245">csoportok mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-245">groups folder</span></span>
<span data-ttu-id="3b13b-246">A `groups` mappa tartalmaz egy mappát az egyes csoportokhoz, a szolgáltatáspéldány definiálva.</span><span class="sxs-lookup"><span data-stu-id="3b13b-246">The `groups` folder contains a folder for each group defined in the service instance.</span></span>

* <span data-ttu-id="3b13b-247">`groups\<group name>\configuration.json`-a csoport beállítani.</span><span class="sxs-lookup"><span data-stu-id="3b13b-247">`groups\<group name>\configuration.json` - this is the configuration for the group.</span></span> <span data-ttu-id="3b13b-248">Ez az ugyanazokat az információkat, akkor adja vissza, ha hívása a [egy adott csoport lekérése](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) műveletet.</span><span class="sxs-lookup"><span data-stu-id="3b13b-248">This is the same information that would be returned if you were to call the [Get a specific group](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operation.</span></span>
* <span data-ttu-id="3b13b-249">`groups\<group name>\description.html`– Ez a csoport leírását, és megfelel-e a `description` tulajdonsága a [entitás csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="3b13b-249">`groups\<group name>\description.html` - this is the description of the group and corresponds to the `description` property of the [group entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span></span>

### <a name="policies-folder"></a><span data-ttu-id="3b13b-250">házirend mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-250">policies folder</span></span>
<span data-ttu-id="3b13b-251">A `policies` mappa tartalmazza a szolgáltatáspéldány a házirend-utasításoknál.</span><span class="sxs-lookup"><span data-stu-id="3b13b-251">The `policies` folder contains the policy statements for your service instance.</span></span>

* <span data-ttu-id="3b13b-252">`policies\global.xml`-a szolgáltatáspéldány globális hatókörben meghatározott házirendek szerint tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3b13b-252">`policies\global.xml` - contains policies defined at global scope for your service instance.</span></span>
* <span data-ttu-id="3b13b-253">`policies\apis\<api name>\`-Ha bármely API hatókörből meghatározott házirendek szerint, akkor ebben a mappában találhatók.</span><span class="sxs-lookup"><span data-stu-id="3b13b-253">`policies\apis\<api name>\` - if you have any policies defined at API scope, they are contained in this folder.</span></span>
* <span data-ttu-id="3b13b-254">`policies\apis\<api name>\<operation name>\`mappa - Ha minden házirendet a hatókör művelet van megadva, akkor az a mappában található `<operation name>.xml` fájlok, amelyek a házirend-utasításoknál minden művelethez.</span><span class="sxs-lookup"><span data-stu-id="3b13b-254">`policies\apis\<api name>\<operation name>\` folder - if you have any policies defined at operation scope, they are contained in this folder in `<operation name>.xml` files that map to the policy statements for each operation.</span></span>
* <span data-ttu-id="3b13b-255">`policies\products\`-Ha minden házirendet a termék hatókörben van megadva, ezt tartalmazza ezt a mappát, amelybe `<product name>.xml` fájlokat, a házirend-utasításoknál az egyes termékek van leképezve.</span><span class="sxs-lookup"><span data-stu-id="3b13b-255">`policies\products\` - if you have any policies defined at product scope, they are contained in this folder, which contains `<product name>.xml` files that map to the policy statements for each product.</span></span>

### <a name="portalstyles-folder"></a><span data-ttu-id="3b13b-256">portalStyles mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-256">portalStyles folder</span></span>
<span data-ttu-id="3b13b-257">A `portalStyles` mappa tartalmazza a konfigurációs és stílus lapok developer portálon testreszabni a szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="3b13b-257">The `portalStyles` folder contains configuration and style sheets for developer portal customizations for the service instance.</span></span>

* <span data-ttu-id="3b13b-258">`portalStyles\configuration.json`-a stíluslapok, a fejlesztői portál által használt nevét tartalmazza,</span><span class="sxs-lookup"><span data-stu-id="3b13b-258">`portalStyles\configuration.json` - contains the names of the style sheets used by the developer portal</span></span>
* <span data-ttu-id="3b13b-259">`portalStyles\<style name>.css`-minden `<style name>.css` fájl tartalmazza a fejlesztői portálhoz stílusok (`Preview.css` és `Production.css` alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="3b13b-259">`portalStyles\<style name>.css` - each `<style name>.css` file contains styles for the developer portal (`Preview.css` and `Production.css` by default).</span></span>

### <a name="products-folder"></a><span data-ttu-id="3b13b-260">termékek mappa</span><span class="sxs-lookup"><span data-stu-id="3b13b-260">products folder</span></span>
<span data-ttu-id="3b13b-261">A `products` mappa tartalmaz egy mappát az egyes termékek, a szolgáltatáspéldány definiálva.</span><span class="sxs-lookup"><span data-stu-id="3b13b-261">The `products` folder contains a folder for each product defined in the service instance.</span></span>

* <span data-ttu-id="3b13b-262">`products\<product name>\configuration.json`-a a termék beállítani.</span><span class="sxs-lookup"><span data-stu-id="3b13b-262">`products\<product name>\configuration.json` - this is the configuration for the product.</span></span> <span data-ttu-id="3b13b-263">Ez az ugyanazokat az információkat, akkor adja vissza, ha hívása a [beolvasni a termék](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) műveletet.</span><span class="sxs-lookup"><span data-stu-id="3b13b-263">This is the same information that would be returned if you were to call the [Get a specific product](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operation.</span></span>
* <span data-ttu-id="3b13b-264">`products\<product name>\product.description.html`– Ez a termék, és megfelel-e a `description` tulajdonsága a [termék entitás](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) a REST API-ban.</span><span class="sxs-lookup"><span data-stu-id="3b13b-264">`products\<product name>\product.description.html` - this is the description of the product and corresponds to the `description` property of the [product entity](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in the REST API.</span></span>

### <a name="templates"></a><span data-ttu-id="3b13b-265">sablonok</span><span class="sxs-lookup"><span data-stu-id="3b13b-265">templates</span></span>
<span data-ttu-id="3b13b-266">A `templates` mappa konfigurációját tartalmazza a [e-mail sablonok](api-management-howto-configure-notifications.md) service-példány.</span><span class="sxs-lookup"><span data-stu-id="3b13b-266">The `templates` folder contains configuration for the [email templates](api-management-howto-configure-notifications.md) of the service instance.</span></span>

* <span data-ttu-id="3b13b-267">`<template name>\configuration.json`-az e-mail sablon beállítani.</span><span class="sxs-lookup"><span data-stu-id="3b13b-267">`<template name>\configuration.json` - this is the configuration for the email template.</span></span>
* <span data-ttu-id="3b13b-268">`<template name>\body.html`-Ez az az e-mail sablon szövegtörzsét.</span><span class="sxs-lookup"><span data-stu-id="3b13b-268">`<template name>\body.html` - this is the body of the email template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b13b-269">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3b13b-269">Next steps</span></span>
<span data-ttu-id="3b13b-270">Más módokon kezelheti a szolgáltatáspéldány információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="3b13b-270">For information on other ways to manage your service instance, see:</span></span>

* <span data-ttu-id="3b13b-271">A szolgáltatáspéldány, a következő PowerShell-parancsmagok használatával kezelheti.</span><span class="sxs-lookup"><span data-stu-id="3b13b-271">Manage your service instance using the following PowerShell cmdlets</span></span>
  * [<span data-ttu-id="3b13b-272">Szolgáltatások üzembe helyezése – PowerShell-parancsmagok leírása</span><span class="sxs-lookup"><span data-stu-id="3b13b-272">Service deployment PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [<span data-ttu-id="3b13b-273">Szolgáltatás-felügyelet PowerShell parancsmag-referencia</span><span class="sxs-lookup"><span data-stu-id="3b13b-273">Service management PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* <span data-ttu-id="3b13b-274">A szolgáltatáspéldány, a közzétevő portálon kezelése</span><span class="sxs-lookup"><span data-stu-id="3b13b-274">Manage your service instance in the publisher portal</span></span>
  * [<span data-ttu-id="3b13b-275">Az első API kezelése</span><span class="sxs-lookup"><span data-stu-id="3b13b-275">Manage your first API</span></span>](api-management-get-started.md)
* <span data-ttu-id="3b13b-276">A szolgáltatáspéldány, a REST API használatával kezelése</span><span class="sxs-lookup"><span data-stu-id="3b13b-276">Manage your service instance using the REST API</span></span>
  * [<span data-ttu-id="3b13b-277">API Management REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="3b13b-277">API Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="3b13b-278">Áttekintő videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="3b13b-278">Watch a video overview</span></span>
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




