---
title: "aaaManage adatbázis-szerepkörök és a felhasználók az Azure Analysis Services |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage adatbázis-szerepkörök és a felhasználók az Azure Analysis Services kiszolgálón."
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
ms.openlocfilehash: 2ad069a6bcce11bc43347625cb32ec400d48af18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-database-roles-and-users"></a><span data-ttu-id="b41b7-103">Adatbázis-szerepkörök és a felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="b41b7-103">Manage database roles and users</span></span>

<span data-ttu-id="b41b7-104">Hello modell adatbázis szintjén a minden felhasználó tooa szerepkörhöz kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="b41b7-104">At hello model database level, all users must belong tooa role.</span></span> <span data-ttu-id="b41b7-105">Szerepkörök definiálása hello modellű adatbázisnál adott engedéllyel rendelkező felhasználók.</span><span class="sxs-lookup"><span data-stu-id="b41b7-105">Roles define users with particular permissions for hello model database.</span></span> <span data-ttu-id="b41b7-106">Minden felhasználót vagy biztonsági csoportot hozzáadni tooa szerepkörnek rendelkeznie kell egy fiókot az Azure AD-bérlő hello hello kiszolgálóként ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b41b7-106">Any user or security group added tooa role must have an account in an Azure AD tenant in hello same subscription as hello server.</span></span>

<span data-ttu-id="b41b7-107">Szerepkörök definiálása hogyan hello eszköz, amellyel másik attól függően, de hello hatása van hello azonos.</span><span class="sxs-lookup"><span data-stu-id="b41b7-107">How you define roles is different depending on hello tool you use, but hello effect is hello same.</span></span>

<span data-ttu-id="b41b7-108">Szerepkör-engedélyek a következők:</span><span class="sxs-lookup"><span data-stu-id="b41b7-108">Role permissions include:</span></span>
*  <span data-ttu-id="b41b7-109">**Rendszergazda** -felhasználók hello adatbázis teljes körű engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b41b7-109">**Administrator** - Users have full permissions for hello database.</span></span> <span data-ttu-id="b41b7-110">Adatbázis-szerepkörök rendszergazdai jogosultságokkal rendelkező rendszergazdáinak eltérnek.</span><span class="sxs-lookup"><span data-stu-id="b41b7-110">Database roles with Administrator permissions are different from server administrators.</span></span>
*  <span data-ttu-id="b41b7-111">**Folyamat** – a felhasználók kapcsolódhatnak tooand hello adatbázis folyamat műveleteket, és elemezni a modell adatbázis adatai.</span><span class="sxs-lookup"><span data-stu-id="b41b7-111">**Process** - Users can connect tooand perform process operations on hello database, and analyze model database data.</span></span>
*  <span data-ttu-id="b41b7-112">**Olvasási** -felhasználók használhatják az ügyfél alkalmazás tooconnect tooand modell adatbázis elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="b41b7-112">**Read** -  Users can use a client application tooconnect tooand analyze model database data.</span></span>

<span data-ttu-id="b41b7-113">Amikor létrehoz egy táblázatos modell projektet, szerepkörök létrehozása és felhasználók vagy csoportok toothose szerepkörök hozzáadása a szerepkörkezelővel SSDT-ben.</span><span class="sxs-lookup"><span data-stu-id="b41b7-113">When creating a tabular model project, you create roles and add users or groups toothose roles by using Role Manager in SSDT.</span></span> <span data-ttu-id="b41b7-114">Amikor telepített tooa kiszolgáló, az SSMS, [Analysis Services PowerShell-parancsmagok](https://msdn.microsoft.com/library/hh758425.aspx), vagy [táblázatos modell Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) tooadd, vagy távolítsa el a szerepkörök és a felhasználói tagjai.</span><span class="sxs-lookup"><span data-stu-id="b41b7-114">When deployed tooa server, you use SSMS, [Analysis Services PowerShell cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), or [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) tooadd or remove roles and user members.</span></span>

## <a name="tooadd-or-manage-roles-and-users-in-ssdt"></a><span data-ttu-id="b41b7-115">tooadd vagy szerepkörök és az SSDT felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="b41b7-115">tooadd or manage roles and users in SSDT</span></span>  
  
1.  <span data-ttu-id="b41b7-116">Az SSDT > **táblázatos modell Explorer**, kattintson a jobb gombbal **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="b41b7-116">In SSDT > **Tabular Model Explorer**, right-click **Roles**.</span></span>  
  
2.  <span data-ttu-id="b41b7-117">A **Szerepkörkezelőben** kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="b41b7-117">In **Role Manager**, click **New**.</span></span>  
  
3.  <span data-ttu-id="b41b7-118">Írja be a hello szerepkör nevét.</span><span class="sxs-lookup"><span data-stu-id="b41b7-118">Type a name for hello role.</span></span>  
  
     <span data-ttu-id="b41b7-119">Alapértelmezés szerint hello alapértelmezett szerepkör nevét hello Növekményesen számozott minden új szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="b41b7-119">By default, hello name of hello default role is incrementally numbered for each new role.</span></span> <span data-ttu-id="b41b7-120">Javasoljuk, adjon meg egy nevet, amely egyértelműen azonosítja a hello tag típusa, például a pénzügyi menedzserek vagy az emberi erőforrások szakemberei.</span><span class="sxs-lookup"><span data-stu-id="b41b7-120">It's recommended you type a name that clearly identifies hello member type, for example, Finance Managers or Human Resources Specialists.</span></span>  
  
4.  <span data-ttu-id="b41b7-121">Válasszon egyet az alábbi engedélyek használata hello:</span><span class="sxs-lookup"><span data-stu-id="b41b7-121">Select one of hello following permissions:</span></span>  
  
    |<span data-ttu-id="b41b7-122">Engedély</span><span class="sxs-lookup"><span data-stu-id="b41b7-122">Permission</span></span>|<span data-ttu-id="b41b7-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="b41b7-123">Description</span></span>|  
    |----------------|-----------------|  
    |<span data-ttu-id="b41b7-124">**Egyik sem**</span><span class="sxs-lookup"><span data-stu-id="b41b7-124">**None**</span></span>|<span data-ttu-id="b41b7-125">A tagok hello modellsémát nem módosítható, és nem tudja lekérdezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b41b7-125">Members cannot modify hello model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="b41b7-126">**Olvasás**</span><span class="sxs-lookup"><span data-stu-id="b41b7-126">**Read**</span></span>|<span data-ttu-id="b41b7-127">Tagok lekérdezheti adatokat (sorszűrőket alapján), de nem módosíthatja a hello modellsémát.</span><span class="sxs-lookup"><span data-stu-id="b41b7-127">Members can query data (based on row filters) but cannot modify hello model schema.</span></span>|  
    |<span data-ttu-id="b41b7-128">**Olvasás és a folyamat**</span><span class="sxs-lookup"><span data-stu-id="b41b7-128">**Read and Process**</span></span>|<span data-ttu-id="b41b7-129">Tagok lekérdezheti adatokat (a sorszintű szűrők) és futtatási folyamat és a folyamat minden műveleteket, de nem módosítható a hello modellsémát.</span><span class="sxs-lookup"><span data-stu-id="b41b7-129">Members can query data (based on row-level filters) and run Process and Process All operations, but cannot modify hello model schema.</span></span>|  
    |<span data-ttu-id="b41b7-130">**Folyamat**</span><span class="sxs-lookup"><span data-stu-id="b41b7-130">**Process**</span></span>|<span data-ttu-id="b41b7-131">Tagok folyamat és a folyamat minden műveletek is futtatható.</span><span class="sxs-lookup"><span data-stu-id="b41b7-131">Members can run Process and Process All operations.</span></span> <span data-ttu-id="b41b7-132">Hello modellsémát nem módosítható és nem tudja lekérdezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b41b7-132">Cannot modify hello model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="b41b7-133">**Rendszergazda**</span><span class="sxs-lookup"><span data-stu-id="b41b7-133">**Administrator**</span></span>|<span data-ttu-id="b41b7-134">Tagok hello modellsémát módosíthatja, és minden adat lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="b41b7-134">Members can modify hello model schema and query all data.</span></span>|   
  
5.  <span data-ttu-id="b41b7-135">Ha hello szerepkör létrehozása rendelkezik olvasási vagy olvasási és a folyamat engedélyt adhat hozzá sorszűrőket DAX-képlet használatával.</span><span class="sxs-lookup"><span data-stu-id="b41b7-135">If hello role you are creating has Read or Read and Process permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="b41b7-136">Kattintson a hello **sorszűrőket** lapra, majd egy táblázatban, majd kattintson a hello **DAX-szűrő** mezőben, és írja be egy DAX-képletet.</span><span class="sxs-lookup"><span data-stu-id="b41b7-136">Click hello **Row Filters** tab, then select a table, then click hello **DAX Filter** field, and then type a DAX formula.</span></span>
  
6.  <span data-ttu-id="b41b7-137">Kattintson a **tagok** > **vegye fel a külső**.</span><span class="sxs-lookup"><span data-stu-id="b41b7-137">Click **Members** > **Add External**.</span></span>  
  
8.  <span data-ttu-id="b41b7-138">A **külső tag hozzáadása**, adjon meg felhasználókat vagy csoportokat az Azure AD-bérlőben e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="b41b7-138">In **Add External Member**, enter users or groups in your tenant Azure AD by email address.</span></span> <span data-ttu-id="b41b7-139">Kattintson az OK gombra, és zárja be a szerepkör-kezelőt, szerepkörök és a szerepkör tagjai jelennek meg a táblázatos modell Explorer.</span><span class="sxs-lookup"><span data-stu-id="b41b7-139">After you click OK and close Role Manager, roles and role members appear in Tabular Model Explorer.</span></span> 
 
     ![Szerepkörök és a táblázatos modell Explorer felhasználók](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. <span data-ttu-id="b41b7-141">Tooyour Azure Analysis Services-kiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="b41b7-141">Deploy tooyour Azure Analysis Services server.</span></span>


## <a name="tooadd-or-manage-roles-and-users-in-ssms"></a><span data-ttu-id="b41b7-142">tooadd vagy szerepkörök és a szolgáltatáshoz az ssms felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="b41b7-142">tooadd or manage roles and users in SSMS</span></span>
<span data-ttu-id="b41b7-143">szerepkörök és a felhasználók tooa tooadd telepített modellű adatbázisnál, kiszolgálói rendszergazdaként vagy a rendszergazdai jogosultságokkal rendelkező adatbázis-szerepkör már csatlakoztatott toohello kiszolgálón kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b41b7-143">tooadd roles and users tooa deployed model database, you must be connected toohello server as a Server administrator or already in a database role with administrator permissions.</span></span>

1. <span data-ttu-id="b41b7-144">Az objektum Exporer, kattintson a jobb gombbal **szerepkörök** > **új szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="b41b7-144">In Object Exporer, right-click **Roles** > **New Role**.</span></span>

2. <span data-ttu-id="b41b7-145">A **szerepkör létrehozása**, írja be a szerepkör nevét és leírását.</span><span class="sxs-lookup"><span data-stu-id="b41b7-145">In **Create Role**, enter a role name and description.</span></span>

3. <span data-ttu-id="b41b7-146">Jelöljön ki egy engedélyt.</span><span class="sxs-lookup"><span data-stu-id="b41b7-146">Select a permission.</span></span>
   |<span data-ttu-id="b41b7-147">Engedély</span><span class="sxs-lookup"><span data-stu-id="b41b7-147">Permission</span></span>|<span data-ttu-id="b41b7-148">Leírás</span><span class="sxs-lookup"><span data-stu-id="b41b7-148">Description</span></span>|  
   |----------------|-----------------|  
   |<span data-ttu-id="b41b7-149">**Teljes hozzáférés (rendszergazda)**</span><span class="sxs-lookup"><span data-stu-id="b41b7-149">**Full control (Administrator)**</span></span>|<span data-ttu-id="b41b7-150">Tagjai módosíthatják hello modellsémát, feldolgozásához, és lekérdezheti az összes adatot.</span><span class="sxs-lookup"><span data-stu-id="b41b7-150">Members can modify hello model schema, process, and can query all data.</span></span>| 
   |<span data-ttu-id="b41b7-151">**Folyamat adatbázis**</span><span class="sxs-lookup"><span data-stu-id="b41b7-151">**Process database**</span></span>|<span data-ttu-id="b41b7-152">Tagok folyamat és a folyamat minden műveletek is futtatható.</span><span class="sxs-lookup"><span data-stu-id="b41b7-152">Members can run Process and Process All operations.</span></span> <span data-ttu-id="b41b7-153">Hello modellsémát nem módosítható és nem tudja lekérdezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b41b7-153">Cannot modify hello model schema and cannot query data.</span></span>|  
   |<span data-ttu-id="b41b7-154">**Olvasás**</span><span class="sxs-lookup"><span data-stu-id="b41b7-154">**Read**</span></span>|<span data-ttu-id="b41b7-155">Tagok lekérdezheti adatokat (sorszűrőket alapján), de nem módosíthatja a hello modellsémát.</span><span class="sxs-lookup"><span data-stu-id="b41b7-155">Members can query data (based on row filters) but cannot modify hello model schema.</span></span>|  
  
4. <span data-ttu-id="b41b7-156">Kattintson a **tagsági**, majd írjon be egy felhasználót vagy csoportot az Azure AD bérlőhöz e-mail cím alapján.</span><span class="sxs-lookup"><span data-stu-id="b41b7-156">Click **Membership**, then enter a user or group in your tenant Azure AD by email address.</span></span>

     ![Felhasználó hozzáadása](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. <span data-ttu-id="b41b7-158">Ha hello szerepkör létrehozásakor olvasási engedéllyel rendelkezik, egy DAX-képlet használatával adhat hozzá a sor szűrők.</span><span class="sxs-lookup"><span data-stu-id="b41b7-158">If hello role you are creating has Read permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="b41b7-159">Kattintson a **sorszűrőket**, válasszon ki egy táblát, és írja be egy DAX-képlet hello **DAX-szűrő** mező.</span><span class="sxs-lookup"><span data-stu-id="b41b7-159">Click **Row Filters**, select a table, and then type a DAX formula in hello **DAX Filter** field.</span></span> 

## <a name="tooadd-roles-and-users-by-using-a-tmsl-script"></a><span data-ttu-id="b41b7-160">tooadd szerepkörök és a felhasználók TMSL parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="b41b7-160">tooadd roles and users by using a TMSL script</span></span>
<span data-ttu-id="b41b7-161">Egy TMSL parancsprogrammal hello XMLA ablakban szolgáltatáshoz az ssms vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b41b7-161">You can run a TMSL script in hello XMLA window in SSMS or by using PowerShell.</span></span> <span data-ttu-id="b41b7-162">Használjon hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) parancs és hello [szerepkörök](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) objektum.</span><span class="sxs-lookup"><span data-stu-id="b41b7-162">Use hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) command and hello [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) object.</span></span>

<span data-ttu-id="b41b7-163">**Mintaparancsfájl TMSL**</span><span class="sxs-lookup"><span data-stu-id="b41b7-163">**Sample TMSL script**</span></span>

<span data-ttu-id="b41b7-164">Ez a példa egy B2B külső felhasználó vagy csoport kerülnek toohello elemző szerepkör hello SalesBI adatbázis olvasási engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="b41b7-164">In this sample, a B2B external user and a group are added toohello Analyst role with Read permissions for hello SalesBI database.</span></span> <span data-ttu-id="b41b7-165">Mindkét hello külső felhasználó, és az Azure AD ugyanannak a bérlőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b41b7-165">Both hello external user and group must be in same tenant Azure AD.</span></span>

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users tooquery hello model",
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

## <a name="tooadd-roles-and-users-by-using-powershell"></a><span data-ttu-id="b41b7-166">tooadd szerepkörök és a felhasználók a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b41b7-166">tooadd roles and users by using PowerShell</span></span>
<span data-ttu-id="b41b7-167">Hello [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) modul adja meg a tevékenység-specifikus adatbázis felügyeleti parancsmagok és hello általános célú Invoke-ASCmd parancsmag, amely egy táblázatos modell Scripting Language (TMSL) lekérdezés vagy parancsfájl fogadja.</span><span class="sxs-lookup"><span data-stu-id="b41b7-167">hello [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) module provides task-specific database management cmdlets and hello general-purpose Invoke-ASCmd cmdlet that accepts a Tabular Model Scripting Language (TMSL) query or script.</span></span> <span data-ttu-id="b41b7-168">a következő parancsmagok hello használt adatbázis-szerepkörök és felhasználók kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b41b7-168">hello following cmdlets are used for managing database roles and users.</span></span>
  
|<span data-ttu-id="b41b7-169">Parancsmag</span><span class="sxs-lookup"><span data-stu-id="b41b7-169">Cmdlet</span></span>|<span data-ttu-id="b41b7-170">Leírás</span><span class="sxs-lookup"><span data-stu-id="b41b7-170">Description</span></span>|
|------------|-----------------| 
|[<span data-ttu-id="b41b7-171">Adja hozzá RoleMember</span><span class="sxs-lookup"><span data-stu-id="b41b7-171">Add-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510167.aspx)|<span data-ttu-id="b41b7-172">Egy tag tooa adatbázis-szerepkör hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="b41b7-172">Add a member tooa database role.</span></span>| 
|[<span data-ttu-id="b41b7-173">Remove-RoleMember</span><span class="sxs-lookup"><span data-stu-id="b41b7-173">Remove-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510173.aspx)|<span data-ttu-id="b41b7-174">Tag eltávolítása egy adatbázis-szerepkör.</span><span class="sxs-lookup"><span data-stu-id="b41b7-174">Remove a member from a database role.</span></span>|   
|[<span data-ttu-id="b41b7-175">Invoke-ASCmd</span><span class="sxs-lookup"><span data-stu-id="b41b7-175">Invoke-ASCmd</span></span>](https://msdn.microsoft.com/library/hh479579.aspx)|<span data-ttu-id="b41b7-176">Hajtsa végre a TMSL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b41b7-176">Execute a TMSL script.</span></span>|

## <a name="row-filters"></a><span data-ttu-id="b41b7-177">Sorszűrőket</span><span class="sxs-lookup"><span data-stu-id="b41b7-177">Row filters</span></span>  
<span data-ttu-id="b41b7-178">Sorszűrőket határozza meg, mely egy tábla sorainak lekérdezhetők, egy adott szerepkör tagjai.</span><span class="sxs-lookup"><span data-stu-id="b41b7-178">Row filters define which rows in a table can be queried by members of a particular role.</span></span> <span data-ttu-id="b41b7-179">Sorszűrőket DAX-képlet használatával definiáltak a modellben minden táblához.</span><span class="sxs-lookup"><span data-stu-id="b41b7-179">Row filters are defined for each table in a model by using DAX formulas.</span></span>  
  
<span data-ttu-id="b41b7-180">Sorszűrőket csak rendelkező olvasási és olvasási szerepkörökhöz adható meg és folyamat-engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="b41b7-180">Row filters can be defined only for roles with Read and Read and Process permissions.</span></span> <span data-ttu-id="b41b7-181">Alapértelmezés szerint ha egy adott tábla nincs definiálva a Sorszűrő tagok lekérdezheti hello tábla összes sorát kivéve, ha egy másik táblából keresztszűrési vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b41b7-181">By default, if a row filter is not defined for a particular table, members  can query all rows in hello table unless cross-filtering applies from another table.</span></span>
  
 <span data-ttu-id="b41b7-182">Sorszűrőket kell a DAX-képlet, amely ki kell értékelnie minden tooa igaz vagy hamis értéket, toodefine hello azon sorait, amelyek az adott adott szerepkör tagjai kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="b41b7-182">Row filters require a DAX formula, which must evaluate tooa TRUE/FALSE value, toodefine hello rows that can be queried by members of that particular role.</span></span> <span data-ttu-id="b41b7-183">Hello DAX-képlet nem szereplő sorok nem kérdezhető le.</span><span class="sxs-lookup"><span data-stu-id="b41b7-183">Rows not included in hello DAX formula cannot be queried.</span></span> <span data-ttu-id="b41b7-184">Például hello ügyfelek tábla a következő szűrők sorkifejezésen hello *ügyfelek [Ország] = "USA" =*, hello értékesítési szerepkör tagjai csak ügyfelek hello USA tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="b41b7-184">For example, hello Customers table with hello following row filters expression, *=Customers [Country] = “USA”*, members of hello Sales role can only see customers in hello USA.</span></span>  
  
<span data-ttu-id="b41b7-185">Sorszűrőket alkalmazása toohello megadott sorok és a kapcsolódó sorok.</span><span class="sxs-lookup"><span data-stu-id="b41b7-185">Row filters apply toohello specified rows and related rows.</span></span> <span data-ttu-id="b41b7-186">Ha egy táblában több kapcsolatnak is tagja, szűrők hello kapcsolat aktív biztonsági lesznek alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="b41b7-186">When a table has multiple relationships, filters apply security for hello relationship that is active.</span></span> <span data-ttu-id="b41b7-187">Sorszűrőket az egyéb kapcsolódó tábla, például meghatározott sor szûrõkkel vannak átfedő:</span><span class="sxs-lookup"><span data-stu-id="b41b7-187">Row filters are intersected with other row filers defined for related tables, for example:</span></span>  
  
|<span data-ttu-id="b41b7-188">Tábla</span><span class="sxs-lookup"><span data-stu-id="b41b7-188">Table</span></span>|<span data-ttu-id="b41b7-189">DAX-kifejezés</span><span class="sxs-lookup"><span data-stu-id="b41b7-189">DAX expression</span></span>|  
|-----------|--------------------|  
|<span data-ttu-id="b41b7-190">Régió</span><span class="sxs-lookup"><span data-stu-id="b41b7-190">Region</span></span>|<span data-ttu-id="b41b7-191">= Régió [Ország] = "USA"</span><span class="sxs-lookup"><span data-stu-id="b41b7-191">=Region[Country]=”USA”</span></span>|  
|<span data-ttu-id="b41b7-192">ProductCategory</span><span class="sxs-lookup"><span data-stu-id="b41b7-192">ProductCategory</span></span>|<span data-ttu-id="b41b7-193">= ProductCategory [Name] = "Kerékpárt"</span><span class="sxs-lookup"><span data-stu-id="b41b7-193">=ProductCategory[Name]=”Bicycles”</span></span>|  
|<span data-ttu-id="b41b7-194">Tranzakciók</span><span class="sxs-lookup"><span data-stu-id="b41b7-194">Transactions</span></span>|<span data-ttu-id="b41b7-195">= Tranzakciók [év] = 2016</span><span class="sxs-lookup"><span data-stu-id="b41b7-195">=Transactions[Year]=2016</span></span>|  
  
 <span data-ttu-id="b41b7-196">hello nettó hatása tagok sornyi adatot, ahol hello ügyfél hello USA, hello termékkategória kerékpárt, és hello év 2016 lekérdezheti.</span><span class="sxs-lookup"><span data-stu-id="b41b7-196">hello net effect is members can query rows of data where hello customer is in hello USA, hello product category is bicycles, and hello year is 2016.</span></span> <span data-ttu-id="b41b7-197">Felhasználók kívül hello USA tranzakciók, amelyek nincsenek kerékpárt vagy tranzakciók nem 2016 kivéve, ha egy másik szerepkör, amely engedélyt ad a tranzakciók nem tudja lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="b41b7-197">Users cannot query transactions outside of hello USA, transactions that are not bicycles, or transactions not in 2016 unless they are a member of another role that grants these permissions.</span></span>
  
 <span data-ttu-id="b41b7-198">Hello szűrővel *=FALSE()*, toodeny hozzáférés tooall sorainak teljes táblát.</span><span class="sxs-lookup"><span data-stu-id="b41b7-198">You can use hello filter, *=FALSE()*, toodeny access tooall rows for an entire table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b41b7-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b41b7-199">Next steps</span></span>
  <span data-ttu-id="b41b7-200">[Kiszolgáló-rendszergazdák kezelése](analysis-services-server-admins.md) </span><span class="sxs-lookup"><span data-stu-id="b41b7-200">[Manage server administrators](analysis-services-server-admins.md) </span></span>  
  [<span data-ttu-id="b41b7-201">A PowerShell segítségével az Azure Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="b41b7-201">Manage Azure Analysis Services with PowerShell</span></span>](analysis-services-powershell.md)  
  [<span data-ttu-id="b41b7-202">Táblázatos modell Scripting (TMSL) nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="b41b7-202">Tabular Model Scripting Language (TMSL) Reference</span></span>](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

