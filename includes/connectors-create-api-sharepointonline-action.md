<span data-ttu-id="ac8e7-101">Most, hogy egy eseményindítót, az idő toodo valami érdekes hello eseményindító által létrehozott hello adatokkal hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-101">Now that you have added a trigger, its time toodo something interesting with hello data that's generated by hello trigger.</span></span> <span data-ttu-id="ac8e7-102">Kövesse az alábbi lépéseket tooadd hello **SharePoint Online - fájl létrehozása** művelet.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-102">Follow these steps tooadd hello **SharePoint Online - Create file** action.</span></span> <span data-ttu-id="ac8e7-103">Ez a művelet létrehoz egy fájlt a SharePoint Online minden alkalommal hello új cikk eseményindító következik be.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-103">This action will create a file in SharePoint Online each time hello new item trigger fires.</span></span> 

<span data-ttu-id="ac8e7-104">tooconfigure hello Ez a művelet, szüksége lesz a következő információ tooprovide hello.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-104">tooconfigure hello this action, you will need tooprovide hello following information.</span></span> <span data-ttu-id="ac8e7-105">Mivel ezek az információk láthatja, hogy a rendszer az új fájl hello hello tulajdonságok bemeneteként hello eseményindító által létrehozott egyszerű toouse adatokat:</span><span class="sxs-lookup"><span data-stu-id="ac8e7-105">As you provide this information, you will notice that it is easy toouse data generated by hello trigger as input for some of hello properties for hello new file:</span></span>

| <span data-ttu-id="ac8e7-106">Fájl tulajdonság létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac8e7-106">Create file property</span></span> | <span data-ttu-id="ac8e7-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="ac8e7-107">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ac8e7-108">Webhely URL-címe</span><span class="sxs-lookup"><span data-stu-id="ac8e7-108">Site URL</span></span> |<span data-ttu-id="ac8e7-109">Ez a hello hello SharePoint Online helyet, ahol toocreate hello új fájl URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-109">This is hello URL of hello SharePoint Online site where you want toocreate hello new file.</span></span> <span data-ttu-id="ac8e7-110">Válassza ki a hello helyet hello-listáról.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-110">Select hello site from hello list presented.</span></span> |
| <span data-ttu-id="ac8e7-111">Mappa elérési útja</span><span class="sxs-lookup"><span data-stu-id="ac8e7-111">Folder path</span></span> |<span data-ttu-id="ac8e7-112">Ez az hello mappát a (hello hello előző lépésben kiválasztott webhely URL-címe) ahol hello új fájl kerül.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-112">This is hello folder (at hello Site URL selected in hello previous step) where hello new file will be placed.</span></span> <span data-ttu-id="ac8e7-113">Tallózással keresse meg és válassza ki hello mappát.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-113">Browse for and select hello folder.</span></span> |
| <span data-ttu-id="ac8e7-114">Fájlnév</span><span class="sxs-lookup"><span data-stu-id="ac8e7-114">File name</span></span> |<span data-ttu-id="ac8e7-115">Ez az hello név hello fájl létrehozása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-115">This is hello name of hello file being created.</span></span> |
| <span data-ttu-id="ac8e7-116">A fájl</span><span class="sxs-lookup"><span data-stu-id="ac8e7-116">File content</span></span> |<span data-ttu-id="ac8e7-117">a rendszer úgy tünteti toohello fájl hello tartalom.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-117">hello content that will be written toohello file.</span></span> |

1. <span data-ttu-id="ac8e7-118">Válassza ki **+ új lépés** tooadd hello művelet.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-118">Select **+ New step** tooadd hello action.</span></span>  
   <span data-ttu-id="ac8e7-119">![SharePoint online művelet kép 1](./media/connectors-create-api-sharepointonline/action-1.png)</span><span class="sxs-lookup"><span data-stu-id="ac8e7-119">![SharePoint online action image 1](./media/connectors-create-api-sharepointonline/action-1.png)</span></span>  
2. <span data-ttu-id="ac8e7-120">Jelölje be hello **művelet hozzáadása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-120">Select hello **Add an action** link.</span></span> <span data-ttu-id="ac8e7-121">A megnyíló hello keresőmezőbe ahol kereshet bármely művelet, tootake szeretné.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-121">This opens hello search box where you can search for any action you would like tootake.</span></span> <span data-ttu-id="ac8e7-122">Ebben a példában a SharePoint-műveletek végezhetők, iránt.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-122">For this example, SharePoint actions are of interest.</span></span>    
   <span data-ttu-id="ac8e7-123">![SharePoint online művelet kép 2](./media/connectors-create-api-sharepointonline/action-2.png)</span><span class="sxs-lookup"><span data-stu-id="ac8e7-123">![SharePoint online action image 2](./media/connectors-create-api-sharepointonline/action-2.png)</span></span>    
3. <span data-ttu-id="ac8e7-124">Adja meg *sharepoint* toosearch kapcsolódó tooSharePoint műveletek számára.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-124">Enter *sharepoint* toosearch for actions related tooSharePoint.</span></span>
4. <span data-ttu-id="ac8e7-125">Válassza ki **SharePoint Online - fájl létrehozása** művelet tootake hello szerint.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-125">Select **SharePoint Online - Create file** as hello action tootake.</span></span>   <span data-ttu-id="ac8e7-126">**Megjegyzés:**: akkor lesz felszólító tooauthorize a logic app tooaccess a SharePoint fiókot, ha nem hozott létre a kapcsolat tooSharePoint Online korábban.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-126">**Note**: you will be prompted tooauthorize your logic app tooaccess your SharePoint account if you have not created a connection tooSharePoint Online previously.</span></span>    
   <span data-ttu-id="ac8e7-127">![SharePoint online művelet kép 3](./media/connectors-create-api-sharepointonline/action-3.png)</span><span class="sxs-lookup"><span data-stu-id="ac8e7-127">![SharePoint online action image 3](./media/connectors-create-api-sharepointonline/action-3.png)</span></span>    
5. <span data-ttu-id="ac8e7-128">Hello **létrehozás fájl** megnyílik szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-128">hello **Create file** control opens.</span></span>   
   <span data-ttu-id="ac8e7-129">![SharePoint online művelet kép 4](./media/connectors-create-api-sharepointonline/action-4.png)</span><span class="sxs-lookup"><span data-stu-id="ac8e7-129">![SharePoint online action image 4](./media/connectors-create-api-sharepointonline/action-4.png)</span></span>     
6. <span data-ttu-id="ac8e7-130">Válassza ki **webhely URL-címe** és Tallózás toofind hello hely toocreate hello fájl helyének.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-130">Select **Site URL** and browse toofind hello site where you would like toocreate hello file.</span></span>     
   <span data-ttu-id="ac8e7-131">![SharePoint online művelet kép 5](./media/connectors-create-api-sharepointonline/action-5.png)</span><span class="sxs-lookup"><span data-stu-id="ac8e7-131">![SharePoint online action image 5](./media/connectors-create-api-sharepointonline/action-5.png)</span></span>  
7. <span data-ttu-id="ac8e7-132">Válassza ki **mappa elérési útja** és Tallózás toofind hello mappa ahol hello új fájl kerül.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-132">Select **Folder path** and browse toofind hello folder where hello new file will be placed.</span></span>  
   <span data-ttu-id="ac8e7-133">![SharePoint online művelet kép 6](./media/connectors-create-api-sharepointonline/action-6.png)</span><span class="sxs-lookup"><span data-stu-id="ac8e7-133">![SharePoint online action image 6](./media/connectors-create-api-sharepointonline/action-6.png)</span></span>  
8. <span data-ttu-id="ac8e7-134">Jelölje be hello **Fájlnév** szabályozza, és írja be a hello neve hello fájl toocreate kívánt.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-134">Select hello **File name** control and enter hello name of hello file you want toocreate.</span></span> <span data-ttu-id="ac8e7-135">Itt adhatja meg hello fájlnevet közvetlenül, vagy használhatja a korábban létrehozott hello eseményindító hello tulajdonságok valamelyikét.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-135">Here, you can either enter hello file name directly or you can use any of hello properties from hello trigger you created previously.</span></span> <span data-ttu-id="ac8e7-136">Ehhez a Tulajdonságok parancsra kattintva hello listája **kimeneteinek új elem létrehozásakor**.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-136">You do this by selecting properties from hello list of **Outputs from When a new item is created**.</span></span> <span data-ttu-id="ac8e7-137">A lista létrehozási csak megjelenítési, miután kiválasztotta a hello **Fájlnév** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-137">This list is only display after you select hello **File name** control.</span></span> <span data-ttu-id="ac8e7-138">A walkthough a kiválasztott ID (hello hello új listaelem azonosítója) hello által létrehozott hello fájl hello névként **SharePoint Online - fájl létrehozása** művelet.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-138">In this walkthough, I selected ID (hello ID of hello new list item) as hello name of hello file being created by hello **SharePoint Online - Create file** action.</span></span>    
   <span data-ttu-id="ac8e7-139">![SharePoint online művelet kép 7](./media/connectors-create-api-sharepointonline/action-7.png)</span><span class="sxs-lookup"><span data-stu-id="ac8e7-139">![SharePoint online action image 7](./media/connectors-create-api-sharepointonline/action-7.png)</span></span>  
9. <span data-ttu-id="ac8e7-140">Jelölje be hello **tartalom fájl** vezérlik, és adja meg a létrehozandó toohello fájl írt hello tartalmat.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-140">Select hello **File content** control and enter hello content that will be written toohello file that will be created.</span></span> <span data-ttu-id="ac8e7-141">Hello fájl tartalom figyelje meg, használhatja a korábban létrehozott hello eseményindító hello tulajdonságok valamelyikét.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-141">For hello file content, notice that you can use any of hello properties from hello trigger you created previously.</span></span> <span data-ttu-id="ac8e7-142">Egyszerűen válassza hello tulajdonságok hello-listáról.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-142">Simply select hello properties from hello list presented.</span></span> <span data-ttu-id="ac8e7-143">Másik lehetőségként megadhat hello **tartalom fájl** szöveg közvetlenül hello vezérlőbe.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-143">Alternatively, you can enter hello **File content** text directly into hello control.</span></span> <span data-ttu-id="ac8e7-144">Ebben a példában I kijelölt egyes tulajdonságok és területek és az egyes tulajdonságokhoz kötőjelet hozzá.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-144">In this example, I selected some properties and added spaces and a hyphen between each property.</span></span>        
   <span data-ttu-id="ac8e7-145">![SharePoint online művelet kép 8](./media/connectors-create-api-sharepointonline/action-8.png)</span><span class="sxs-lookup"><span data-stu-id="ac8e7-145">![SharePoint online action image 8](./media/connectors-create-api-sharepointonline/action-8.png)</span></span>  
10. <span data-ttu-id="ac8e7-146">Hello módosítások tooyour munkafolyamat mentése</span><span class="sxs-lookup"><span data-stu-id="ac8e7-146">Save hello changes tooyour workflow</span></span>  
11. <span data-ttu-id="ac8e7-147">Gratulálunk, most már rendelkezik egy teljesen működőképes logikai alkalmazás, amely lesz kiváltva, ha egy új listaelem tooa SharePoint Online listája.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-147">Congratulations, you now have a fully functional logic app that is triggered when a new item is added tooa SharePoint Online list.</span></span> <span data-ttu-id="ac8e7-148">hello alkalmazás majd létrehoz egy fájlt, az új listaelem hello hello tulajdonságok használatával.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-148">hello app will then create a file, using some of hello properties from hello new list item.</span></span>  <span data-ttu-id="ac8e7-149">Most tesztelheti, hozzon létre egy új elem hello SharePoint-listán.</span><span class="sxs-lookup"><span data-stu-id="ac8e7-149">You can now test it by creating a new item in hello SharePoint list.</span></span> 
