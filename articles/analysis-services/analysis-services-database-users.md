---
title: "Adatbázis-szerepkörök és a felhasználók az Azure Analysis Services kezelése |} Microsoft Docs"
description: "Útmutató: az adatbázis-szerepkörök és az Analysis Services-kiszolgálóhoz, az Azure-ban a felhasználók kezelése."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: d0bc7d7514f111b4bbde33bd60ae21264bd797fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-database-roles-and-users"></a><span data-ttu-id="bdef2-103">Adatbázis-szerepkörök és a felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="bdef2-103">Manage database roles and users</span></span>

<span data-ttu-id="bdef2-104">A modell adatbázis szinten minden felhasználó egy szerepkörhöz kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="bdef2-104">At the model database level, all users must belong to a role.</span></span> <span data-ttu-id="bdef2-105">Szerepkörök definiálása a modelladatbázis adott jogosultságokkal rendelkező felhasználók.</span><span class="sxs-lookup"><span data-stu-id="bdef2-105">Roles define users with particular permissions for the model database.</span></span> <span data-ttu-id="bdef2-106">Bármely felhasználónak vagy biztonsági csoportot hozzáadni a szerepkörhöz ugyanazt az előfizetést, mint a kiszolgáló Azure AD-bérlő fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="bdef2-106">Any user or security group added to a role must have an account in an Azure AD tenant in the same subscription as the server.</span></span>

<span data-ttu-id="bdef2-107">Szerepkörök definiálása hogyan eltér attól függően, hogy a használt eszköz, azonban a hatás azonos.</span><span class="sxs-lookup"><span data-stu-id="bdef2-107">How you define roles is different depending on the tool you use, but the effect is the same.</span></span>

<span data-ttu-id="bdef2-108">Szerepkör-engedélyek a következők:</span><span class="sxs-lookup"><span data-stu-id="bdef2-108">Role permissions include:</span></span>
*  <span data-ttu-id="bdef2-109">**Rendszergazda** -felhasználók az adatbázis teljes körű engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bdef2-109">**Administrator** - Users have full permissions for the database.</span></span> <span data-ttu-id="bdef2-110">Adatbázis-szerepkörök rendszergazdai jogosultságokkal rendelkező rendszergazdáinak eltérnek.</span><span class="sxs-lookup"><span data-stu-id="bdef2-110">Database roles with Administrator permissions are different from server administrators.</span></span>
*  <span data-ttu-id="bdef2-111">**Folyamat** -felhasználók csatlakozhat és az adatbázis folyamat műveleteket, és modell adatbázis elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="bdef2-111">**Process** - Users can connect to and perform process operations on the database, and analyze model database data.</span></span>
*  <span data-ttu-id="bdef2-112">**Olvasási** -felhasználók ügyfélalkalmazás segítségével csatlakozhat, és elemezni a modell adatbázis adatai.</span><span class="sxs-lookup"><span data-stu-id="bdef2-112">**Read** -  Users can use a client application to connect to and analyze model database data.</span></span>

<span data-ttu-id="bdef2-113">Amikor létrehoz egy táblázatos modell projektet, szerepköröket hozhat létre, és SSDT szerepkör-kezelővel ezeket a szerepköröket felhasználók vagy csoportok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="bdef2-113">When creating a tabular model project, you create roles and add users or groups to those roles by using Role Manager in SSDT.</span></span> <span data-ttu-id="bdef2-114">Egy kiszolgáló telepítésekor használhatja SSMS, [Analysis Services PowerShell-parancsmagok](https://msdn.microsoft.com/library/hh758425.aspx), vagy [táblázatos modell Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) hozzáadása vagy eltávolítása a szerepkörök és a felhasználói tagjai.</span><span class="sxs-lookup"><span data-stu-id="bdef2-114">When deployed to a server, you use SSMS, [Analysis Services PowerShell cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), or [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) to add or remove roles and user members.</span></span>

## <a name="to-add-or-manage-roles-and-users-in-ssdt"></a><span data-ttu-id="bdef2-115">Adja hozzá vagy szerepkörök és az SSDT felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="bdef2-115">To add or manage roles and users in SSDT</span></span>  
  
1.  <span data-ttu-id="bdef2-116">Az SSDT > **táblázatos modell Explorer**, kattintson a jobb gombbal **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="bdef2-116">In SSDT > **Tabular Model Explorer**, right-click **Roles**.</span></span>  
  
2.  <span data-ttu-id="bdef2-117">A **Szerepkörkezelőben** kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="bdef2-117">In **Role Manager**, click **New**.</span></span>  
  
3.  <span data-ttu-id="bdef2-118">Írja be a szerepkör nevét.</span><span class="sxs-lookup"><span data-stu-id="bdef2-118">Type a name for the role.</span></span>  
  
     <span data-ttu-id="bdef2-119">Alapértelmezés szerint az alapértelmezett szerepkör neve Növekményesen számozott minden új szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="bdef2-119">By default, the name of the default role is incrementally numbered for each new role.</span></span> <span data-ttu-id="bdef2-120">Javasoljuk, adjon meg egy nevet, amely egyértelműen azonosítja a tag típusa, például a pénzügyi menedzserek vagy az emberi erőforrások szakemberei.</span><span class="sxs-lookup"><span data-stu-id="bdef2-120">It's recommended you type a name that clearly identifies the member type, for example, Finance Managers or Human Resources Specialists.</span></span>  
  
4.  <span data-ttu-id="bdef2-121">Válassza ki a következő jogosultságok egyikével rendelkeznek:</span><span class="sxs-lookup"><span data-stu-id="bdef2-121">Select one of the following permissions:</span></span>  
  
    |<span data-ttu-id="bdef2-122">Engedély</span><span class="sxs-lookup"><span data-stu-id="bdef2-122">Permission</span></span>|<span data-ttu-id="bdef2-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="bdef2-123">Description</span></span>|  
    |----------------|-----------------|  
    |<span data-ttu-id="bdef2-124">**Egyik sem**</span><span class="sxs-lookup"><span data-stu-id="bdef2-124">**None**</span></span>|<span data-ttu-id="bdef2-125">A tagok nem módosítható a modellsémát, és nem tudja lekérdezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="bdef2-125">Members cannot modify the model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="bdef2-126">**Olvasás**</span><span class="sxs-lookup"><span data-stu-id="bdef2-126">**Read**</span></span>|<span data-ttu-id="bdef2-127">Tagok lekérdezheti adatokat (sorszűrőket alapján), de nem módosíthatja a modellsémát.</span><span class="sxs-lookup"><span data-stu-id="bdef2-127">Members can query data (based on row filters) but cannot modify the model schema.</span></span>|  
    |<span data-ttu-id="bdef2-128">**Olvasás és a folyamat**</span><span class="sxs-lookup"><span data-stu-id="bdef2-128">**Read and Process**</span></span>|<span data-ttu-id="bdef2-129">Tagok lekérdezheti adatokat (a sorszintű szűrők) és futtatási folyamat és a folyamat minden műveleteket, de nem módosítható a modellsémát.</span><span class="sxs-lookup"><span data-stu-id="bdef2-129">Members can query data (based on row-level filters) and run Process and Process All operations, but cannot modify the model schema.</span></span>|  
    |<span data-ttu-id="bdef2-130">**Folyamat**</span><span class="sxs-lookup"><span data-stu-id="bdef2-130">**Process**</span></span>|<span data-ttu-id="bdef2-131">Tagok folyamat és a folyamat minden műveletek is futtatható.</span><span class="sxs-lookup"><span data-stu-id="bdef2-131">Members can run Process and Process All operations.</span></span> <span data-ttu-id="bdef2-132">A modell sémája nem módosítható és nem tudja lekérdezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="bdef2-132">Cannot modify the model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="bdef2-133">**Rendszergazda**</span><span class="sxs-lookup"><span data-stu-id="bdef2-133">**Administrator**</span></span>|<span data-ttu-id="bdef2-134">Tagok modellsémát módosíthatja, és minden adat lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="bdef2-134">Members can modify the model schema and query all data.</span></span>|   
  
5.  <span data-ttu-id="bdef2-135">Ha a szerepkör létrehozása rendelkezik olvasási vagy olvasási és a folyamat engedélyt adhat hozzá sorszűrőket DAX-képlet használatával.</span><span class="sxs-lookup"><span data-stu-id="bdef2-135">If the role you are creating has Read or Read and Process permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="bdef2-136">Kattintson a **sorszűrőket** lapra, majd válasszon egy táblázatot, majd kattintson a **DAX-szűrő** mezőben, és írja be egy DAX-képletet.</span><span class="sxs-lookup"><span data-stu-id="bdef2-136">Click the **Row Filters** tab, then select a table, then click the **DAX Filter** field, and then type a DAX formula.</span></span>
  
6.  <span data-ttu-id="bdef2-137">Kattintson a **tagok** > **vegye fel a külső**.</span><span class="sxs-lookup"><span data-stu-id="bdef2-137">Click **Members** > **Add External**.</span></span>  
  
8.  <span data-ttu-id="bdef2-138">A **külső tag hozzáadása**, adjon meg felhasználókat vagy csoportokat az Azure AD-bérlőben e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="bdef2-138">In **Add External Member**, enter users or groups in your tenant Azure AD by email address.</span></span> <span data-ttu-id="bdef2-139">Kattintson az OK gombra, és zárja be a szerepkör-kezelőt, szerepkörök és a szerepkör tagjai jelennek meg a táblázatos modell Explorer.</span><span class="sxs-lookup"><span data-stu-id="bdef2-139">After you click OK and close Role Manager, roles and role members appear in Tabular Model Explorer.</span></span> 
 
     ![Szerepkörök és a táblázatos modell Explorer felhasználók](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. <span data-ttu-id="bdef2-141">Telepítse az Azure Analysis Services-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="bdef2-141">Deploy to your Azure Analysis Services server.</span></span>


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a><span data-ttu-id="bdef2-142">Adja hozzá vagy szerepkörök és a szolgáltatáshoz az ssms felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="bdef2-142">To add or manage roles and users in SSMS</span></span>
<span data-ttu-id="bdef2-143">Szerepkörök és felhasználók felvétele egy telepített modellű adatbázisnál, akkor kapcsolódnia kell a kiszolgáló kiszolgálói rendszergazdaként vagy a rendszergazdai jogosultságokkal rendelkező adatbázis-szerepkör már.</span><span class="sxs-lookup"><span data-stu-id="bdef2-143">To add roles and users to a deployed model database, you must be connected to the server as a Server administrator or already in a database role with administrator permissions.</span></span>

1. <span data-ttu-id="bdef2-144">Az objektum Exporer, kattintson a jobb gombbal **szerepkörök** > **új szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="bdef2-144">In Object Exporer, right-click **Roles** > **New Role**.</span></span>

2. <span data-ttu-id="bdef2-145">A **szerepkör létrehozása**, írja be a szerepkör nevét és leírását.</span><span class="sxs-lookup"><span data-stu-id="bdef2-145">In **Create Role**, enter a role name and description.</span></span>

3. <span data-ttu-id="bdef2-146">Jelöljön ki egy engedélyt.</span><span class="sxs-lookup"><span data-stu-id="bdef2-146">Select a permission.</span></span>
   |<span data-ttu-id="bdef2-147">Engedély</span><span class="sxs-lookup"><span data-stu-id="bdef2-147">Permission</span></span>|<span data-ttu-id="bdef2-148">Leírás</span><span class="sxs-lookup"><span data-stu-id="bdef2-148">Description</span></span>|  
   |----------------|-----------------|  
   |<span data-ttu-id="bdef2-149">**Teljes hozzáférés (rendszergazda)**</span><span class="sxs-lookup"><span data-stu-id="bdef2-149">**Full control (Administrator)**</span></span>|<span data-ttu-id="bdef2-150">Tagjai módosíthatják a modellsémát feldolgozásához, és lekérdezheti az összes adatot.</span><span class="sxs-lookup"><span data-stu-id="bdef2-150">Members can modify the model schema, process, and can query all data.</span></span>| 
   |<span data-ttu-id="bdef2-151">**Folyamat adatbázis**</span><span class="sxs-lookup"><span data-stu-id="bdef2-151">**Process database**</span></span>|<span data-ttu-id="bdef2-152">Tagok folyamat és a folyamat minden műveletek is futtatható.</span><span class="sxs-lookup"><span data-stu-id="bdef2-152">Members can run Process and Process All operations.</span></span> <span data-ttu-id="bdef2-153">A modell sémája nem módosítható és nem tudja lekérdezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="bdef2-153">Cannot modify the model schema and cannot query data.</span></span>|  
   |<span data-ttu-id="bdef2-154">**Olvasás**</span><span class="sxs-lookup"><span data-stu-id="bdef2-154">**Read**</span></span>|<span data-ttu-id="bdef2-155">Tagok lekérdezheti adatokat (sorszűrőket alapján), de nem módosíthatja a modellsémát.</span><span class="sxs-lookup"><span data-stu-id="bdef2-155">Members can query data (based on row filters) but cannot modify the model schema.</span></span>|  
  
4. <span data-ttu-id="bdef2-156">Kattintson a **tagsági**, majd írjon be egy felhasználót vagy csoportot az Azure AD bérlőhöz e-mail cím alapján.</span><span class="sxs-lookup"><span data-stu-id="bdef2-156">Click **Membership**, then enter a user or group in your tenant Azure AD by email address.</span></span>

     ![Felhasználó hozzáadása](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. <span data-ttu-id="bdef2-158">Ha a létrehozni kívánt szerepkör olvasási engedéllyel rendelkezik, egy DAX-képlet használatával adhat hozzá a sor szűrők.</span><span class="sxs-lookup"><span data-stu-id="bdef2-158">If the role you are creating has Read permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="bdef2-159">Kattintson a **sorszűrőket**, válasszon ki egy táblát, és írja be a DAX-képlet a **DAX-szűrő** mező.</span><span class="sxs-lookup"><span data-stu-id="bdef2-159">Click **Row Filters**, select a table, and then type a DAX formula in the **DAX Filter** field.</span></span> 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a><span data-ttu-id="bdef2-160">Szerepkörök és felhasználók hozzáadása TMSL parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="bdef2-160">To add roles and users by using a TMSL script</span></span>
<span data-ttu-id="bdef2-161">Az XMLA-ablakban szolgáltatáshoz az ssms vagy a PowerShell használatával TMSL parancsfájlt is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="bdef2-161">You can run a TMSL script in the XMLA window in SSMS or by using PowerShell.</span></span> <span data-ttu-id="bdef2-162">Használja a [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) parancs és a [szerepkörök](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) objektum.</span><span class="sxs-lookup"><span data-stu-id="bdef2-162">Use the [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) command and the [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) object.</span></span>

<span data-ttu-id="bdef2-163">**Mintaparancsfájl TMSL**</span><span class="sxs-lookup"><span data-stu-id="bdef2-163">**Sample TMSL script**</span></span>

<span data-ttu-id="bdef2-164">Ez a példa egy B2B külső felhasználó vagy csoport kerülnek a SalesBI adatbázis olvasási engedéllyel rendelkező elemző szerepkörre.</span><span class="sxs-lookup"><span data-stu-id="bdef2-164">In this sample, a B2B external user and a group are added to the Analyst role with Read permissions for the SalesBI database.</span></span> <span data-ttu-id="bdef2-165">A külső felhasználó és a csoport az Azure AD ugyanannak a bérlőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bdef2-165">Both the external user and group must be in same tenant Azure AD.</span></span>

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="to-add-roles-and-users-by-using-powershell"></a><span data-ttu-id="bdef2-166">Szerepkörök és felhasználók hozzáadása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="bdef2-166">To add roles and users by using PowerShell</span></span>
<span data-ttu-id="bdef2-167">A [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) modul adja meg a tevékenység-specifikus adatbázis parancsmagokat és az általános célú Invoke-ASCmd parancsmag, amely egy táblázatos modell Scripting Language (TMSL) lekérdezés vagy parancsfájl fogadja.</span><span class="sxs-lookup"><span data-stu-id="bdef2-167">The [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) module provides task-specific database management cmdlets and the general-purpose Invoke-ASCmd cmdlet that accepts a Tabular Model Scripting Language (TMSL) query or script.</span></span> <span data-ttu-id="bdef2-168">A következő parancsmagok használhatók adatbázis-szerepkörök és felhasználók kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="bdef2-168">The following cmdlets are used for managing database roles and users.</span></span>
  
|<span data-ttu-id="bdef2-169">Parancsmag</span><span class="sxs-lookup"><span data-stu-id="bdef2-169">Cmdlet</span></span>|<span data-ttu-id="bdef2-170">Leírás</span><span class="sxs-lookup"><span data-stu-id="bdef2-170">Description</span></span>|
|------------|-----------------| 
|[<span data-ttu-id="bdef2-171">Adja hozzá RoleMember</span><span class="sxs-lookup"><span data-stu-id="bdef2-171">Add-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510167.aspx)|<span data-ttu-id="bdef2-172">Tag hozzáadása egy adatbázis-szerepkör.</span><span class="sxs-lookup"><span data-stu-id="bdef2-172">Add a member to a database role.</span></span>| 
|[<span data-ttu-id="bdef2-173">Remove-RoleMember</span><span class="sxs-lookup"><span data-stu-id="bdef2-173">Remove-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510173.aspx)|<span data-ttu-id="bdef2-174">Tag eltávolítása egy adatbázis-szerepkör.</span><span class="sxs-lookup"><span data-stu-id="bdef2-174">Remove a member from a database role.</span></span>|   
|[<span data-ttu-id="bdef2-175">Invoke-ASCmd</span><span class="sxs-lookup"><span data-stu-id="bdef2-175">Invoke-ASCmd</span></span>](https://msdn.microsoft.com/library/hh479579.aspx)|<span data-ttu-id="bdef2-176">Hajtsa végre a TMSL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="bdef2-176">Execute a TMSL script.</span></span>|

## <a name="row-filters"></a><span data-ttu-id="bdef2-177">Sorszűrőket</span><span class="sxs-lookup"><span data-stu-id="bdef2-177">Row filters</span></span>  
<span data-ttu-id="bdef2-178">Sorszűrőket határozza meg, mely egy tábla sorainak lekérdezhetők, egy adott szerepkör tagjai.</span><span class="sxs-lookup"><span data-stu-id="bdef2-178">Row filters define which rows in a table can be queried by members of a particular role.</span></span> <span data-ttu-id="bdef2-179">Sorszűrőket DAX-képlet használatával definiáltak a modellben minden táblához.</span><span class="sxs-lookup"><span data-stu-id="bdef2-179">Row filters are defined for each table in a model by using DAX formulas.</span></span>  
  
<span data-ttu-id="bdef2-180">Sorszűrőket csak rendelkező olvasási és olvasási szerepkörökhöz adható meg és folyamat-engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="bdef2-180">Row filters can be defined only for roles with Read and Read and Process permissions.</span></span> <span data-ttu-id="bdef2-181">Alapértelmezés szerint ha egy adott tábla nincs definiálva a Sorszűrő tagok lekérdezheti a tábla összes sorát kivéve, ha egy másik táblából keresztszűrési vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="bdef2-181">By default, if a row filter is not defined for a particular table, members  can query all rows in the table unless cross-filtering applies from another table.</span></span>
  
 <span data-ttu-id="bdef2-182">Sorszűrőket kell a DAX-képlet, amely ki kell értékelnie minden igaz vagy hamis értéket, adja meg a sorok, amelyekre ez a szerepkör tagjai által kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="bdef2-182">Row filters require a DAX formula, which must evaluate to a TRUE/FALSE value, to define the rows that can be queried by members of that particular role.</span></span> <span data-ttu-id="bdef2-183">A DAX-képlet nem szereplő sorok nem kérdezhető le.</span><span class="sxs-lookup"><span data-stu-id="bdef2-183">Rows not included in the DAX formula cannot be queried.</span></span> <span data-ttu-id="bdef2-184">Például a következő sort az ügyfelek tábla szűrők kifejezését *ügyfelek [Ország] = "USA" =*, az értékesítési szerepkör tagjai csak az USA ügyfelek tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="bdef2-184">For example, the Customers table with the following row filters expression, *=Customers [Country] = “USA”*, members of the Sales role can only see customers in the USA.</span></span>  
  
<span data-ttu-id="bdef2-185">A megadott sorok és a kapcsolódó sorok sorszűrőket alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="bdef2-185">Row filters apply to the specified rows and related rows.</span></span> <span data-ttu-id="bdef2-186">Egy táblázat több olyan kapcsolattal rendelkezik, szűrők a kapcsolat aktív biztonsági vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="bdef2-186">When a table has multiple relationships, filters apply security for the relationship that is active.</span></span> <span data-ttu-id="bdef2-187">Sorszűrőket az egyéb kapcsolódó tábla, például meghatározott sor szûrõkkel vannak átfedő:</span><span class="sxs-lookup"><span data-stu-id="bdef2-187">Row filters are intersected with other row filers defined for related tables, for example:</span></span>  
  
|<span data-ttu-id="bdef2-188">Tábla</span><span class="sxs-lookup"><span data-stu-id="bdef2-188">Table</span></span>|<span data-ttu-id="bdef2-189">DAX-kifejezés</span><span class="sxs-lookup"><span data-stu-id="bdef2-189">DAX expression</span></span>|  
|-----------|--------------------|  
|<span data-ttu-id="bdef2-190">Régió</span><span class="sxs-lookup"><span data-stu-id="bdef2-190">Region</span></span>|<span data-ttu-id="bdef2-191">= Régió [Ország] = "USA"</span><span class="sxs-lookup"><span data-stu-id="bdef2-191">=Region[Country]=”USA”</span></span>|  
|<span data-ttu-id="bdef2-192">ProductCategory</span><span class="sxs-lookup"><span data-stu-id="bdef2-192">ProductCategory</span></span>|<span data-ttu-id="bdef2-193">= ProductCategory [Name] = "Kerékpárt"</span><span class="sxs-lookup"><span data-stu-id="bdef2-193">=ProductCategory[Name]=”Bicycles”</span></span>|  
|<span data-ttu-id="bdef2-194">Tranzakciók</span><span class="sxs-lookup"><span data-stu-id="bdef2-194">Transactions</span></span>|<span data-ttu-id="bdef2-195">= Tranzakciók [év] = 2016</span><span class="sxs-lookup"><span data-stu-id="bdef2-195">=Transactions[Year]=2016</span></span>|  
  
 <span data-ttu-id="bdef2-196">A nettó hatása az tagok sornyi adatot, ha az ügyfél az USA, a termék kategóriáját kerékpárt, és az év 2016 lekérdezheti.</span><span class="sxs-lookup"><span data-stu-id="bdef2-196">The net effect is members can query rows of data where the customer is in the USA, the product category is bicycles, and the year is 2016.</span></span> <span data-ttu-id="bdef2-197">Felhasználók kívül az USA tranzakciók, amelyek nincsenek kerékpárt vagy tranzakciók nem 2016 kivéve, ha egy másik szerepkör, amely engedélyt ad a tranzakciók nem tudja lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="bdef2-197">Users cannot query transactions outside of the USA, transactions that are not bicycles, or transactions not in 2016 unless they are a member of another role that grants these permissions.</span></span>
  
 <span data-ttu-id="bdef2-198">A szűrővel *=FALSE()*, hogy megtagadja a hozzáférést a teljes táblázat összes sorát.</span><span class="sxs-lookup"><span data-stu-id="bdef2-198">You can use the filter, *=FALSE()*, to deny access to all rows for an entire table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bdef2-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bdef2-199">Next steps</span></span>
  <span data-ttu-id="bdef2-200">[Kiszolgáló-rendszergazdák kezelése](analysis-services-server-admins.md) </span><span class="sxs-lookup"><span data-stu-id="bdef2-200">[Manage server administrators](analysis-services-server-admins.md) </span></span>  
  [<span data-ttu-id="bdef2-201">A PowerShell segítségével az Azure Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="bdef2-201">Manage Azure Analysis Services with PowerShell</span></span>](analysis-services-powershell.md)  
  [<span data-ttu-id="bdef2-202">Táblázatos modell Scripting (TMSL) nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="bdef2-202">Tabular Model Scripting Language (TMSL) Reference</span></span>](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

