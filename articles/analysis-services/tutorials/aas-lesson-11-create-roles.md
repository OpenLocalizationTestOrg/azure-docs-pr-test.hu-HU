---
<span data-ttu-id="935a7-101">cím: aaa "Azure Analysis Services-oktatóanyag lecke 11: szerepkörök létrehozása |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate szerepköreinek hello Azure Analysis Services-oktatóanyag projekt.</span><span class="sxs-lookup"><span data-stu-id="935a7-101">title: aaa"Azure Analysis Services tutorial lesson 11: Create roles | Microsoft Docs" description: Describes how toocreate roles in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="935a7-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="935a7-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="935a7-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="935a7-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-11-create-roles"></a><span data-ttu-id="935a7-104">11. lecke: Szerepkörök létrehozása</span><span class="sxs-lookup"><span data-stu-id="935a7-104">Lesson 11: Create roles</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="935a7-105">Ebben a leckében szerepköröket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="935a7-105">In this lesson, you create roles.</span></span> <span data-ttu-id="935a7-106">Szerepkörök tartalmazzák modell adatbázis objektum és az adatok biztonsági hozzáférési tooonly korlátozza azoknak a felhasználóknak, akik szerepkör tagjai.</span><span class="sxs-lookup"><span data-stu-id="935a7-106">Roles provide model database object and data security by limiting access tooonly those users that are role members.</span></span> <span data-ttu-id="935a7-107">Minden szerepkör egyetlen engedéllyel van definiálva: Nincs, Olvasás, Olvasás és feldolgozás, Feldolgozás vagy Rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="935a7-107">Each role is defined with a single permission: None, Read, Read and Process, Process, or Administrator.</span></span> <span data-ttu-id="935a7-108">A szerepkörök a Szerepkörkezelő használatával definiálhatók a modell létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="935a7-108">Roles can be defined during model authoring by using Role Manager.</span></span> <span data-ttu-id="935a7-109">A modell üzembe helyezése után az SQL Server Management Studio (SSMS) használatával kezelheti a szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="935a7-109">After a model has been deployed, you can manage roles by using SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="935a7-110">több, lásd: toolearn [szerepkörök](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="935a7-110">toolearn more, see [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).</span></span>
  
> [!NOTE]  
> <span data-ttu-id="935a7-111">Szerepkörök létrehozása nem szükséges toocomplete van ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="935a7-111">Creating roles is not necessary toocomplete this tutorial.</span></span> <span data-ttu-id="935a7-112">Alapértelmezés szerint a is van jelenleg bejelentkezve hello fióknak rendszergazdai jogosultságai vannak a hello modellen.</span><span class="sxs-lookup"><span data-stu-id="935a7-112">By default, hello account you are currently logged in with has Administrator privileges on hello model.</span></span> <span data-ttu-id="935a7-113">Azonban a küldő ügyfél segítségével a szervezet toobrowse belüli más felhasználók kell legalább egy szerepkör létrehozása az olvasási engedélyek és azoknak a felhasználóknak felvenni.</span><span class="sxs-lookup"><span data-stu-id="935a7-113">However, for other users in your organization toobrowse by using a reporting client, you must create at least one role with Read permissions and add those users as members.</span></span>  
  
<span data-ttu-id="935a7-114">Három szerepkört kell létrehoznia:</span><span class="sxs-lookup"><span data-stu-id="935a7-114">You create three roles:</span></span>  
  
-   <span data-ttu-id="935a7-115">**Az értékesítési igazgató** – ehhez a szerepkörhöz felhasználók tartalmazhatnak a szervezetében, amelyekhez toohave olvasási engedélyt tooall modell objektumokat és adatokat.</span><span class="sxs-lookup"><span data-stu-id="935a7-115">**Sales Manager** – This role can include users in your organization for which you want toohave Read permission tooall model objects and data.</span></span>  
  
-   <span data-ttu-id="935a7-116">**Értékesítési elemző USA** – ehhez a szerepkörhöz felhasználók tartalmazhatnak a szervezet legyen csak toobe képes toobrowse adatok az Egyesült Államok hello toosales kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="935a7-116">**Sales Analyst US** – This role can include users in your organization for which you want only toobe able toobrowse data related toosales in hello United States.</span></span> <span data-ttu-id="935a7-117">Ehhez a szerepkörhöz használni egy DAX-képlet toodefine egy *sorainak szűrője*, amely korlátozza, hogy csak az Egyesült Államok hello tagok toobrowse adatokat.</span><span class="sxs-lookup"><span data-stu-id="935a7-117">For this role, you use a DAX formula toodefine a *Row Filter*, which restricts members toobrowse data only for hello United States.</span></span>  
  
-   <span data-ttu-id="935a7-118">**Rendszergazda** – ehhez a szerepkörhöz felhasználók legyen toohave rendszergazdai jogosultsággal, amely lehetővé teszi az korlátlan hozzáférést és engedélyeket tooperform felügyeleti feladatok hello modell adatbázison tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="935a7-118">**Administrator** – This role can include users for which you want toohave Administrator permission, which allows unlimited access and permissions tooperform administrative tasks on hello model database.</span></span>  
  
<span data-ttu-id="935a7-119">Mivel a szervezet felhasználói és csoportfiókokat a Windows egyedi, az adott szervezet toomembers fiókokat is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="935a7-119">Because Windows user and group accounts in your organization are unique, you can add accounts from your particular organization toomembers.</span></span> <span data-ttu-id="935a7-120">Azonban a jelen oktatóanyag esetében is üresen hello tagokat.</span><span class="sxs-lookup"><span data-stu-id="935a7-120">However, for this tutorial, you can also leave hello members blank.</span></span> <span data-ttu-id="935a7-121">Minden egyes szerepkör hello hatásának tesztelése későbbi részében lecke 12: elemzés az Excelben.</span><span class="sxs-lookup"><span data-stu-id="935a7-121">You test hello effect of each role later in Lesson 12: Analyze in Excel.</span></span>  
  
<span data-ttu-id="935a7-122">Becsült idő toocomplete Ez a lecke: **15 perc**</span><span class="sxs-lookup"><span data-stu-id="935a7-122">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="935a7-123">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="935a7-123">Prerequisites</span></span>  
<span data-ttu-id="935a7-124">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="935a7-124">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="935a7-125">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 10: partíciókat](../tutorials/aas-lesson-10-create-partitions.md).</span><span class="sxs-lookup"><span data-stu-id="935a7-125">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span>  
  
## <a name="create-roles"></a><span data-ttu-id="935a7-126">Szerepkörök létrehozása</span><span class="sxs-lookup"><span data-stu-id="935a7-126">Create roles</span></span>  
  
#### <a name="toocreate-a-sales-manager-user-role"></a><span data-ttu-id="935a7-127">toocreate értékesítési igazgató felhasználói szerepkör</span><span class="sxs-lookup"><span data-stu-id="935a7-127">toocreate a Sales Manager user role</span></span>  
  
1.  <span data-ttu-id="935a7-128">A Táblázatosmodell-tallózóban kattintson a jobb gombbal a **Szerepkörök** > **Szerepkörök** elemre.</span><span class="sxs-lookup"><span data-stu-id="935a7-128">In Tabular Model Explorer, right-click **Roles** > **Roles**.</span></span>  
  
2.  <span data-ttu-id="935a7-129">A Szerepkörkezelőben kattintson az **Új** elemre.</span><span class="sxs-lookup"><span data-stu-id="935a7-129">In Role Manager, click **New**.</span></span>  
  
3.  <span data-ttu-id="935a7-130">Kattintson az új szerepkör hello, majd a hello **neve** oszlop, hello szerepkör átnevezése túl**értékesítési igazgató**.</span><span class="sxs-lookup"><span data-stu-id="935a7-130">Click hello new role, and then in hello **Name** column, rename hello role too**Sales Manager**.</span></span>  
  
4.  <span data-ttu-id="935a7-131">A hello **engedélyek** oszlopban kattintson hello legördülő listára, és válassza a hello **olvasási** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="935a7-131">In hello **Permissions** column, click hello dropdown list, and then select hello **Read** permission.</span></span> 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  <span data-ttu-id="935a7-133">Választható lehetőség: Kattintson a hello **tagok** fülre, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="935a7-133">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="935a7-134">A hello **felhasználók vagy csoportok kiválasztása** párbeszédpanelen adja meg a Windows hello-felhasználók vagy csoportok a szervezete hello szerepkörben tooinclude kívánt.</span><span class="sxs-lookup"><span data-stu-id="935a7-134">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span>  
  
#### <a name="toocreate-a-sales-analyst-us-user-role"></a><span data-ttu-id="935a7-135">toocreate USA értékesítési elemzői felhasználói szerepkör</span><span class="sxs-lookup"><span data-stu-id="935a7-135">toocreate a Sales Analyst US user role</span></span>  
  
1.  <span data-ttu-id="935a7-136">A Szerepkörkezelőben kattintson az **Új** elemre.</span><span class="sxs-lookup"><span data-stu-id="935a7-136">In Role Manager, click **New**.</span></span>    
  
2.  <span data-ttu-id="935a7-137">Nevezze át a hello szerepkör túl**értékesítési elemző USA**.</span><span class="sxs-lookup"><span data-stu-id="935a7-137">Rename hello role too**Sales Analyst US**.</span></span>  
  
3.  <span data-ttu-id="935a7-138">Adjon **Olvasás** engedélyt a szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="935a7-138">Give this role **Read** permission.</span></span>  
  
4.  <span data-ttu-id="935a7-139">Hello sorszűrőket lapon, és majd a hello **DimGeography** csak, hello DAX-szűrő oszlop, tábla típusú hello képlet a következő:</span><span class="sxs-lookup"><span data-stu-id="935a7-139">Click hello Row Filters tab, and then for hello **DimGeography** table only, in hello DAX Filter column, type hello following formula:</span></span>  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    <span data-ttu-id="935a7-140">A Sorszűrőt képlet tooa logikai (igaz/hamis) értéket kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="935a7-140">A Row Filter formula must resolve tooa Boolean (TRUE/FALSE) value.</span></span> <span data-ttu-id="935a7-141">Ezzel a képlettel meg, hogy csak az "USA" hello ország régiókód érték rendelkező sor látható toohello felhasználókként szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="935a7-141">With this formula, you are specifying that only rows with hello Country Region Code value of “US” are visible toohello user.</span></span>  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  <span data-ttu-id="935a7-143">Választható lehetőség: Kattintson a hello **tagok** fülre, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="935a7-143">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="935a7-144">A hello **felhasználók vagy csoportok kiválasztása** párbeszédpanelen adja meg a Windows hello-felhasználók vagy csoportok a szervezete hello szerepkörben tooinclude kívánt.</span><span class="sxs-lookup"><span data-stu-id="935a7-144">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span>  
  
#### <a name="toocreate-an-administrator-user-role"></a><span data-ttu-id="935a7-145">egy rendszergazda felhasználói szerepkör toocreate</span><span class="sxs-lookup"><span data-stu-id="935a7-145">toocreate an Administrator user role</span></span>  
  
1.  <span data-ttu-id="935a7-146">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="935a7-146">Click **New**.</span></span>  
  
2.  <span data-ttu-id="935a7-147">Nevezze át a hello szerepkör túl**rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="935a7-147">Rename hello role too**Administrator**.</span></span>  
  
3.  <span data-ttu-id="935a7-148">Adjon **Rendszergazda** engedélyt a szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="935a7-148">Give this role **Administrator** permission.</span></span>  
  
4.  <span data-ttu-id="935a7-149">Választható lehetőség: Kattintson a hello **tagok** fülre, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="935a7-149">Optional: Click hello **Members** tab, and then click **Add**.</span></span> <span data-ttu-id="935a7-150">A hello **felhasználók vagy csoportok kiválasztása** párbeszédpanelen adja meg a Windows hello-felhasználók vagy csoportok a szervezete hello szerepkörben tooinclude kívánt.</span><span class="sxs-lookup"><span data-stu-id="935a7-150">In hello **Select Users or Groups** dialog box, enter hello Windows users or groups from your organization you want tooinclude in hello role.</span></span> 
  
  
## <a name="whats-next"></a><span data-ttu-id="935a7-151">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="935a7-151">What's next?</span></span>
<span data-ttu-id="935a7-152">[12. lecke: Elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md).</span><span class="sxs-lookup"><span data-stu-id="935a7-152">[Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>

  
  
