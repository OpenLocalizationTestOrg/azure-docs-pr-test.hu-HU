---
title: "a StorSimple 8000 series aaaManage sávszélesség sablonok |} Microsoft Docs"
description: "Ismerteti, hogyan toomanage StorSimple sávszélesség sablonok, amelyek lehetővé teszik toocontrol sávszélesség-használat."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: ebcd1824d7bb9e4c235194c04edbfe8001a3e794
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-bandwidth-templates"></a><span data-ttu-id="b87a5-103">Használjon hello StorSimple Device Manager szolgáltatás toomanage StorSimple sávszélesség sablonokat.</span><span class="sxs-lookup"><span data-stu-id="b87a5-103">Use hello StorSimple Device Manager service toomanage StorSimple bandwidth templates</span></span>

## <a name="overview"></a><span data-ttu-id="b87a5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b87a5-104">Overview</span></span>

<span data-ttu-id="b87a5-105">A sávszélesség a sablonok segítségével tooconfigure a sávszélesség-használat több idő napi ütemezés tootier hello adatait hello StorSimple eszköz toohello felhő között.</span><span class="sxs-lookup"><span data-stu-id="b87a5-105">Bandwidth templates allow you tooconfigure network bandwidth usage across multiple time-of-day schedules tootier hello data from hello StorSimple device toohello cloud.</span></span>

<span data-ttu-id="b87a5-106">A sávszélesség-szabályozási ütemezések segítségével:</span><span class="sxs-lookup"><span data-stu-id="b87a5-106">With bandwidth throttling schedules you can:</span></span>

* <span data-ttu-id="b87a5-107">Adja meg a testreszabott sávszélesség ütemezést attól függően, hogy hello munkaterhelés hálózati módjait.</span><span class="sxs-lookup"><span data-stu-id="b87a5-107">Specify customized bandwidth schedules depending on hello workload network usages.</span></span>
* <span data-ttu-id="b87a5-108">Kezelésének központosítása, és több eszközön hello ütemezések könnyen és zökkenőmentes módon használja fel.</span><span class="sxs-lookup"><span data-stu-id="b87a5-108">Centralize management and reuse hello schedules across multiple devices in an easy and seamless manner.</span></span>

> [!NOTE]
> <span data-ttu-id="b87a5-109">Ez a funkció csak a StorSimple fizikai eszközök (modellek 8100 és 8600) és a StorSimple felhő készülékek (modellek 8010-es és a 8020-as modell) nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="b87a5-109">This feature is available only for StorSimple physical devices (models 8100 and 8600) and not for StorSimple Cloud Appliances (models 8010 and 8020).</span></span>


## <a name="hello-bandwidth-templates-blade"></a><span data-ttu-id="b87a5-110">hello sávszélesség sablonok panel</span><span class="sxs-lookup"><span data-stu-id="b87a5-110">hello Bandwidth templates blade</span></span>

<span data-ttu-id="b87a5-111">Hello **sávszélesség sablonok** panelen táblázatos formában a szolgáltatás az összes hello sávszélesség sablonok rendelkezik, és tartalmazza a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="b87a5-111">hello **Bandwidth templates** blade has all hello bandwidth templates for your service in a tabular format, and contains hello following information:</span></span>

* <span data-ttu-id="b87a5-112">**Név** – egyedi név, hozzárendelése toohello sávszélességsablon létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b87a5-112">**Name** – A unique name assigned toohello bandwidth template when it was created.</span></span>
* <span data-ttu-id="b87a5-113">**Ütemezés** – hello egy adott sávszélesség sablonban található ütemezések száma.</span><span class="sxs-lookup"><span data-stu-id="b87a5-113">**Schedule** – hello number of schedules contained in a given bandwidth template.</span></span>
* <span data-ttu-id="b87a5-114">**Által használt** – hello hello sávszélesség-sablonokkal kötetek számát.</span><span class="sxs-lookup"><span data-stu-id="b87a5-114">**Used by** – hello number of volumes using hello bandwidth templates.</span></span>

<span data-ttu-id="b87a5-115">Is tekinthet meg további információkat toohelp sávszélesség sablonok konfigurálása az:</span><span class="sxs-lookup"><span data-stu-id="b87a5-115">You can also find additional information toohelp configure bandwidth templates in:</span></span>

* [<span data-ttu-id="b87a5-116">Sávszélesség-sablonokkal kapcsolatos kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="b87a5-116">Questions and answers about bandwidth templates</span></span>](#questions-and-answers-about-bandwidth-templates)
* [<span data-ttu-id="b87a5-117">Gyakorlati tanácsok a sávszélesség-sablonok</span><span class="sxs-lookup"><span data-stu-id="b87a5-117">Best practices for bandwidth templates</span></span>](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a><span data-ttu-id="b87a5-118">Sávszélességsablon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b87a5-118">Add a bandwidth template</span></span>

<span data-ttu-id="b87a5-119">Hajtsa végre a következő lépéseket toocreate új sávszélességsablon hello.</span><span class="sxs-lookup"><span data-stu-id="b87a5-119">Perform hello following steps toocreate a new bandwidth template.</span></span>

#### <a name="tooadd-a-bandwidth-template"></a><span data-ttu-id="b87a5-120">tooadd sávszélességsablon</span><span class="sxs-lookup"><span data-stu-id="b87a5-120">tooadd a bandwidth template</span></span>

1. <span data-ttu-id="b87a5-121">Nyissa meg tooyour StorSimple Device Manager szolgáltatást, kattintson a **sávszélesség sablonok** majd **+ Hozzáadás sávszélességsablon**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-121">Go tooyour StorSimple Device Manager service, click **Bandwidth templates** and then click **+ Add Bandwidth template**.</span></span>

    ![Kattintson a + sávszélességsablon hozzáadása](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. <span data-ttu-id="b87a5-123">A hello **sávszélességsablon hozzáadása** panelen hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b87a5-123">In hello **Add bandwidth template** blade, do hello following steps:</span></span>
   
    1. <span data-ttu-id="b87a5-124">Adja meg a sávszélesség-sablon egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="b87a5-124">Specify a unique name for your bandwidth template.</span></span>
    2. <span data-ttu-id="b87a5-125">Sávszélesség ütemezés meghatározása.</span><span class="sxs-lookup"><span data-stu-id="b87a5-125">Define a bandwidth schedule.</span></span> <span data-ttu-id="b87a5-126">toocreate ütemezés szerint:</span><span class="sxs-lookup"><span data-stu-id="b87a5-126">toocreate a schedule:</span></span>
   
        1. <span data-ttu-id="b87a5-127">Hello legördülő listából válassza ki a hello **nap** hello hét hello az ütemezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="b87a5-127">From hello drop-down list, choose hello **Days** of hello week hello schedule is configured for.</span></span> <span data-ttu-id="b87a5-128">Kiválaszthatja, hogy több nap.</span><span class="sxs-lookup"><span data-stu-id="b87a5-128">You can select multiple days.</span></span>        
        
        2. <span data-ttu-id="b87a5-129">Adjon meg egy **kezdete** a _óó: pp_ formátumban.</span><span class="sxs-lookup"><span data-stu-id="b87a5-129">Enter a **Start Time** in _hh:mm_ format.</span></span> <span data-ttu-id="b87a5-130">Ez akkor, ha hello ütemezés megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="b87a5-130">This is when hello schedule will begin.</span></span>

        3. <span data-ttu-id="b87a5-131">Adjon meg egy **befejezésének** a _óó: pp_ formátumban.</span><span class="sxs-lookup"><span data-stu-id="b87a5-131">Enter an **End Time** in _hh:mm_ format.</span></span> <span data-ttu-id="b87a5-132">Ez akkor, ha hello ütemezés leáll.</span><span class="sxs-lookup"><span data-stu-id="b87a5-132">This is when hello schedule will stop.</span></span>
      
           > [!NOTE]
           > <span data-ttu-id="b87a5-133">Átfedő ütemezések nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="b87a5-133">Overlapping schedules are not allowed.</span></span> <span data-ttu-id="b87a5-134">Hello kezdési és befejezési időpontjai eredményezi az átfedő ütemezés, ha toothat hatása hiba üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b87a5-134">If hello start and end times will result in an overlapping schedule, you will see an error message toothat effect.</span></span>

        4. <span data-ttu-id="b87a5-135">Adja meg a hello **sávszélesség mértékének**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-135">Specify hello **Bandwidth Rate**.</span></span> <span data-ttu-id="b87a5-136">Ez a hello sávszélesség megabit / másodperc (Mbps), a StorSimple eszköz (feltöltések és letöltések) hello felhő érintő műveletek használják.</span><span class="sxs-lookup"><span data-stu-id="b87a5-136">This is hello bandwidth in Megabits per second (Mbps) used by your StorSimple device in operations involving hello cloud (both uploads and downloads).</span></span> <span data-ttu-id="b87a5-137">Adjon meg egy 1 és 1000 ennél a mezőnél közötti számot.</span><span class="sxs-lookup"><span data-stu-id="b87a5-137">Supply a number between 1 and 1,000 for this field.</span></span>

            ![Sávszélesség ütemezés megadása](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            <span data-ttu-id="b87a5-139">Ismételje meg a fent lépéseket toodefine hello egyszerre több ütemezés a sablon amíg végzett.</span><span class="sxs-lookup"><span data-stu-id="b87a5-139">Repeat hello above steps toodefine multiple schedules for your template until you are done.</span></span>

        5. <span data-ttu-id="b87a5-140">Kattintson a **Hozzáadás** toostart sávszélesség sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b87a5-140">Click **Add** toostart creating a bandwidth template.</span></span> <span data-ttu-id="b87a5-141">sávszélesség-sablonok listájának toohello létrehozott sablon hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b87a5-141">hello created template is added toohello list of bandwidth templates.</span></span>
      

## <a name="edit-a-bandwidth-template"></a><span data-ttu-id="b87a5-142">A sávszélesség-sablon szerkesztése</span><span class="sxs-lookup"><span data-stu-id="b87a5-142">Edit a bandwidth template</span></span>

<span data-ttu-id="b87a5-143">Hajtsa végre a következő lépéseket tooedit sávszélességsablon hello.</span><span class="sxs-lookup"><span data-stu-id="b87a5-143">Perform hello following steps tooedit a bandwidth template.</span></span>

### <a name="tooedit-a-bandwidth-template"></a><span data-ttu-id="b87a5-144">tooedit sávszélességsablon</span><span class="sxs-lookup"><span data-stu-id="b87a5-144">tooedit a bandwidth template</span></span>

1. <span data-ttu-id="b87a5-145">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **sávszélesség sablonok**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-145">Go tooyour StorSimple Device Manager service and click **Bandwidth templates**.</span></span>
2. <span data-ttu-id="b87a5-146">Sávszélesség-sablonok listájának hello válassza ki kívánja toodelete hello sablont.</span><span class="sxs-lookup"><span data-stu-id="b87a5-146">In hello list of bandwidth templates, select hello template you wish toodelete.</span></span> <span data-ttu-id="b87a5-147">Kattintson a jobb gombbal, és hello helyi menüből válassza ki a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-147">Right-click and from hello context menu, select **Delete**.</span></span>
3. <span data-ttu-id="b87a5-148">Amikor felszólítja a megerősítésre, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-148">When prompted for confirmation, click **OK**.</span></span> <span data-ttu-id="b87a5-149">Ez a hello sávszélességsablon törölni kell.</span><span class="sxs-lookup"><span data-stu-id="b87a5-149">This should delete hello bandwidth template.</span></span> 
4. <span data-ttu-id="b87a5-150">sávszélesség-sablonok listájának hello tooreflect hello törlés frissíti.</span><span class="sxs-lookup"><span data-stu-id="b87a5-150">hello list of bandwidth templates updates tooreflect hello deletion.</span></span>

> [!NOTE]
> <span data-ttu-id="b87a5-151">A módosítások nem menthetők, ha hello szerkeszt hello sávszélesség sablont, amely módosítja a meglévő ütemezés ütemezés átfedésben.</span><span class="sxs-lookup"><span data-stu-id="b87a5-151">You cannot save your changes if hello edited schedule overlaps with an existing schedule in hello bandwidth template that you are modifying.</span></span>

## <a name="delete-a-bandwidth-template"></a><span data-ttu-id="b87a5-152">Sávszélesség sablon törlése</span><span class="sxs-lookup"><span data-stu-id="b87a5-152">Delete a bandwidth template</span></span>

<span data-ttu-id="b87a5-153">Hajtsa végre a következő lépéseket toodelete sávszélességsablon hello.</span><span class="sxs-lookup"><span data-stu-id="b87a5-153">Perform hello following steps toodelete a bandwidth template.</span></span>

#### <a name="toodelete-a-bandwidth-template"></a><span data-ttu-id="b87a5-154">toodelete sávszélességsablon</span><span class="sxs-lookup"><span data-stu-id="b87a5-154">toodelete a bandwidth template</span></span>

1. <span data-ttu-id="b87a5-155">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **sávszélesség sablonok**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-155">Go tooyour StorSimple Device Manager service and click **Bandwidth templates**.</span></span>
2. <span data-ttu-id="b87a5-156">Sávszélesség-sablonok listájának hello válassza ki kívánja toodelete hello sablont.</span><span class="sxs-lookup"><span data-stu-id="b87a5-156">In hello list of bandwidth templates, select hello template you wish toodelete.</span></span> <span data-ttu-id="b87a5-157">Kattintson a jobb gombbal, és hello helyi menüből válassza a törlés.</span><span class="sxs-lookup"><span data-stu-id="b87a5-157">Right-click and from hello context menu, select Delete.</span></span>
3. <span data-ttu-id="b87a5-158">Amikor felszólítja a megerősítésre, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-158">When prompted for confirmation, click **OK**.</span></span> <span data-ttu-id="b87a5-159">Ez a hello sávszélességsablon törölni kell.</span><span class="sxs-lookup"><span data-stu-id="b87a5-159">This should delete hello bandwidth template.</span></span>
4. <span data-ttu-id="b87a5-160">sávszélesség-sablonok listájának hello tooreflect hello törlés frissíti.</span><span class="sxs-lookup"><span data-stu-id="b87a5-160">hello list of bandwidth templates updates tooreflect hello deletion.</span></span>

<span data-ttu-id="b87a5-161">Ha hello sablon bármely kötet(ek) használja, akkor nem engedélyezett toodelete azt.</span><span class="sxs-lookup"><span data-stu-id="b87a5-161">If hello template is in use by any volume(s), you will not be allowed toodelete it.</span></span> <span data-ttu-id="b87a5-162">Megjelenik egy hiba üzenet arról, hogy a sablon hello használatban van.</span><span class="sxs-lookup"><span data-stu-id="b87a5-162">You will see an error message indicating that hello template is in use.</span></span> <span data-ttu-id="b87a5-163">Egy hibaüzenet párbeszédpanelén jelenik meg tájékoztatja, hogy minden hello hivatkozások toohello sablon el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="b87a5-163">An error message dialog box will appear advising you that all hello references toohello template should be removed.</span></span>

<span data-ttu-id="b87a5-164">Minden hello hivatkozások toohello sablon törlése hello elérésével **Kötettárolók** lapon, és ezt a sablont használja, hogy használjon egy másik sablont, vagy egy egyéni vagy korlátlan sávszélességet hello kötettárolók módosítása a beállítás.</span><span class="sxs-lookup"><span data-stu-id="b87a5-164">You can delete all hello references toohello template by accessing hello **Volume Containers** page and modifying hello volume containers that use this template so that they use another template or use a custom or unlimited bandwidth setting.</span></span> <span data-ttu-id="b87a5-165">Hello minden hivatkozást el lettek távolítva, törölheti hello sablont.</span><span class="sxs-lookup"><span data-stu-id="b87a5-165">When all hello references have been removed, you can delete hello template.</span></span>

## <a name="use-a-default-bandwidth-template"></a><span data-ttu-id="b87a5-166">Sávszélesség alapértelmezett sablon használata</span><span class="sxs-lookup"><span data-stu-id="b87a5-166">Use a default bandwidth template</span></span>

<span data-ttu-id="b87a5-167">Alapértelmezett sávszélességsablon valósul meg, és használják kötettárolók alapértelmezett tooenforce sávszélesség vezérlők hello felhő való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="b87a5-167">A default bandwidth template is provided and is used by volume containers by default tooenforce bandwidth controls when accessing hello cloud.</span></span> <span data-ttu-id="b87a5-168">hello alapértelmezett sablont is szolgál a felhasználók számára, akik a saját sablonok létrehozása készen hivatkozásként.</span><span class="sxs-lookup"><span data-stu-id="b87a5-168">hello default template also serves as a ready reference for users who create their own templates.</span></span> <span data-ttu-id="b87a5-169">az alapértelmezett sablon hello részleteit a következők:</span><span class="sxs-lookup"><span data-stu-id="b87a5-169">hello details of this default template are:</span></span>

* <span data-ttu-id="b87a5-170">**Név** – a korlátlan éjszakák, és a hétvégéket</span><span class="sxs-lookup"><span data-stu-id="b87a5-170">**Name** – Unlimited nights and weekends</span></span>
* <span data-ttu-id="b87a5-171">**Ütemezés** – a hétfő tooFriday közötti 8 óra és délután 5 óra eszköz ideje 1 MB/s sávszélesség mértéke alkalmazó egy ütemezéssel.</span><span class="sxs-lookup"><span data-stu-id="b87a5-171">**Schedule** – A single schedule from Monday tooFriday that applies a bandwidth rate of 1 Mbps between 8 AM and 5 PM device time.</span></span> <span data-ttu-id="b87a5-172">hello sávszélesség hello hét hello hátralévő tooUnlimited van beállítva.</span><span class="sxs-lookup"><span data-stu-id="b87a5-172">hello bandwidth is set tooUnlimited for hello remainder of hello week.</span></span>

<span data-ttu-id="b87a5-173">hello alapértelmezett sablon szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="b87a5-173">hello default template can be edited.</span></span> <span data-ttu-id="b87a5-174">Ez a sablon (beleértve a szerkesztett verzióit) hello használata kulcsban követhető nyomon.</span><span class="sxs-lookup"><span data-stu-id="b87a5-174">hello usage of this template (including edited versions) is tracked.</span></span>

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a><span data-ttu-id="b87a5-175">Egy megadott időpontban elinduló napos sávszélesség sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="b87a5-175">Create an all-day bandwidth template that starts at a specified time</span></span>

<span data-ttu-id="b87a5-176">Hajtsa végre az ezen eljárás toocreate olyan ütemezés, amely elindítja a megadott időpontban, és minden nap futtatja.</span><span class="sxs-lookup"><span data-stu-id="b87a5-176">Follow this procedure toocreate a schedule that starts at a specified time and runs all day.</span></span> <span data-ttu-id="b87a5-177">Hello példában hello ütemezés a hello reggel 9 óra kezdődik, és amíg Reggel 9 hello következő reggel futtat.</span><span class="sxs-lookup"><span data-stu-id="b87a5-177">In hello example, hello schedule starts at 9 AM in hello morning and runs until 9 AM hello next morning.</span></span> <span data-ttu-id="b87a5-178">Fontos toonote, amely hello kezdő és befejező időpontok egy adott ütemezés is tartalmaznia kell hello azonos 24 órás ütemezhet, és nem terjedhetnek ki több nap.</span><span class="sxs-lookup"><span data-stu-id="b87a5-178">It's important toonote that hello start and end times for a given schedule must both be contained on hello same 24 hour schedule and cannot span multiple days.</span></span> <span data-ttu-id="b87a5-179">Ha módosítania kell a sávszélesség-sablonok, amelyek több nap több tooset, szüksége lesz toouse egyszerre több ütemezés (hello példa szerint).</span><span class="sxs-lookup"><span data-stu-id="b87a5-179">If you need tooset up bandwidth templates that span multiple days, you will need toouse multiple schedules (as shown in hello example).</span></span>

#### <a name="toocreate-an-all-day-bandwidth-template"></a><span data-ttu-id="b87a5-180">egy napos sávszélességsablon toocreate</span><span class="sxs-lookup"><span data-stu-id="b87a5-180">toocreate an all-day bandwidth template</span></span>

1. <span data-ttu-id="b87a5-181">Hozzon létre egy ütemezést, a hello reggel Reggel 9 és éjfél futtatja.</span><span class="sxs-lookup"><span data-stu-id="b87a5-181">Create a schedule that starts at 9 AM in hello morning and runs until midnight.</span></span>
2. <span data-ttu-id="b87a5-182">Adjon hozzá egy másik ütemezést.</span><span class="sxs-lookup"><span data-stu-id="b87a5-182">Add another schedule.</span></span> <span data-ttu-id="b87a5-183">Hello második ütemezés toorun éjfél konfigurálása csak a hello reggel 9 óra.</span><span class="sxs-lookup"><span data-stu-id="b87a5-183">Configure hello second schedule toorun from midnight until 9 AM in hello morning.</span></span>
3. <span data-ttu-id="b87a5-184">Hello sávszélesség sablon mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b87a5-184">Save hello bandwidth template.</span></span>

<span data-ttu-id="b87a5-185">hello összetett ütemezés majd indítsa el a egyszerre, és futtassa napos.</span><span class="sxs-lookup"><span data-stu-id="b87a5-185">hello composite schedule will then start at a time of your choosing and run all-day.</span></span>

## <a name="questions-and-answers-about-bandwidth-templates"></a><span data-ttu-id="b87a5-186">Sávszélesség-sablonokkal kapcsolatos kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="b87a5-186">Questions and answers about bandwidth templates</span></span>

<span data-ttu-id="b87a5-187">**A Q**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-187">**Q**.</span></span> <span data-ttu-id="b87a5-188">Toobandwidth vezérlők mi történik, amikor Between hello ütemezéseket?</span><span class="sxs-lookup"><span data-stu-id="b87a5-188">What happens toobandwidth controls when you are in between hello schedules?</span></span> <span data-ttu-id="b87a5-189">(Ütemezés befejeződött, és egy másikat még nem kezdődött meg.)</span><span class="sxs-lookup"><span data-stu-id="b87a5-189">(A schedule has ended and another one has not started yet.)</span></span>

<span data-ttu-id="b87a5-190">**A**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-190">**A**.</span></span> <span data-ttu-id="b87a5-191">Ilyen esetekben nem sávszélesség-vezérlés alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="b87a5-191">In such cases, no bandwidth controls will be employed.</span></span> <span data-ttu-id="b87a5-192">Ez azt jelenti, hogy hello eszközökön használható korlátlan sávszélesség, amikor adatokat toohello felhő rétegezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b87a5-192">This means that hello device can use unlimited bandwidth when tiering data toohello cloud.</span></span>

<span data-ttu-id="b87a5-193">**A Q**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-193">**Q**.</span></span> <span data-ttu-id="b87a5-194">Módosíthatja a sávszélesség-sablonok offline-eszközön?</span><span class="sxs-lookup"><span data-stu-id="b87a5-194">Can you modify bandwidth templates on an offline device?</span></span>

<span data-ttu-id="b87a5-195">**A**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-195">**A**.</span></span> <span data-ttu-id="b87a5-196">Nem fogja tudni toomodify sávszélesség sablonok a kötetek tárolók Ha hello megfelelő eszköz offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="b87a5-196">You will not be able toomodify bandwidth templates on volumes containers if hello corresponding device is offline.</span></span>

<span data-ttu-id="b87a5-197">**A Q**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-197">**Q**.</span></span> <span data-ttu-id="b87a5-198">Szerkesztheti hello társított kötetek offline módban. Ha a kötettároló társított sávszélességsablon?</span><span class="sxs-lookup"><span data-stu-id="b87a5-198">Can you edit a bandwidth template associated with a volume container when hello associated volumes are offline?</span></span>

<span data-ttu-id="b87a5-199">**A**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-199">**A**.</span></span> <span data-ttu-id="b87a5-200">A kötettároló, amelynek a kötetek offline módban társított sávszélességsablon módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="b87a5-200">You can modify a bandwidth template associated with a volume container whose volumes are offline.</span></span> <span data-ttu-id="b87a5-201">Vegye figyelembe, hogy kötetek offline módban, amikor nincsenek adatok fog helyezhető el a hello eszköz toohello felhőből.</span><span class="sxs-lookup"><span data-stu-id="b87a5-201">Note that when volumes are offline, no data will be tiered from hello device toohello cloud.</span></span>

<span data-ttu-id="b87a5-202">**A Q**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-202">**Q**.</span></span> <span data-ttu-id="b87a5-203">Törölheti alapértelmezett sablont?</span><span class="sxs-lookup"><span data-stu-id="b87a5-203">Can you delete a default template?</span></span>

<span data-ttu-id="b87a5-204">**A**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-204">**A**.</span></span> <span data-ttu-id="b87a5-205">Bár az alapértelmezett sablon törléséhez nincs egy jó ötlet toodo így.</span><span class="sxs-lookup"><span data-stu-id="b87a5-205">Although you can delete a default template, it is not a good idea toodo so.</span></span> <span data-ttu-id="b87a5-206">alapértelmezett sablon, többek között a szerkesztett verziók hello használata kulcsban követhető nyomon.</span><span class="sxs-lookup"><span data-stu-id="b87a5-206">hello usage of a default template, including edited versions, is tracked.</span></span> <span data-ttu-id="b87a5-207">nyomkövetési adatok hello elemzésnek és hello folyamán idő, használt tooimprove hello alapértelmezett sablont.</span><span class="sxs-lookup"><span data-stu-id="b87a5-207">hello tracking data is analyzed and over hello course of time, is used tooimprove hello default template.</span></span>

<span data-ttu-id="b87a5-208">**A Q**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-208">**Q**.</span></span> <span data-ttu-id="b87a5-209">Hogyan meg, meg, hogy a sávszélesség-sablonokat kell-e a módosított toobe?</span><span class="sxs-lookup"><span data-stu-id="b87a5-209">How do you determine that your bandwidth templates need toobe modified?</span></span>

<span data-ttu-id="b87a5-210">**A**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-210">**A**.</span></span> <span data-ttu-id="b87a5-211">Lelassul, vagy egy napon belül több alkalommal fogyasztja hello jelentkezik, hogy kell-e toomodify hello sávszélesség sablonok rendszer indításakor egyik hello hálózati jelent.</span><span class="sxs-lookup"><span data-stu-id="b87a5-211">One of hello signs that you need toomodify hello bandwidth templates is when you start seeing hello network slow down or choke multiple times in a day.</span></span> <span data-ttu-id="b87a5-212">Ez akkor fordul elő, ha figyelni hello tárolás és a használati hálózati hello i/o-teljesítmény- és hálózati átviteli diagramok megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="b87a5-212">If this happens, monitor hello storage and usage network by looking at hello I/O Performance and Network Throughput charts.</span></span>

<span data-ttu-id="b87a5-213">Hello hálózati átviteli sebességről hello időpontot azonosítása és hello kötettárolók mely hello a hálózati szűk következik be.</span><span class="sxs-lookup"><span data-stu-id="b87a5-213">From hello network throughput data, identify hello time of day and hello volume containers in which hello network bottleneck occurs.</span></span> <span data-ttu-id="b87a5-214">Ha ez történik, ha az adatok folyamatban van a rétegzett toohello felhő (beszerezni ezeket az információkat az összes kötet tárolókat i/o-teljesítmény az eszköz toocloud), akkor a kötettárolók társított toomodify hello sávszélesség sablonok kell.</span><span class="sxs-lookup"><span data-stu-id="b87a5-214">If this happens when data is being tiered toohello cloud (get this information from I/O performance for all volume containers for device toocloud), then you will need toomodify hello bandwidth templates associated with your volume containers.</span></span>

<span data-ttu-id="b87a5-215">Hello módosítása után sablonok használatban van, szüksége lesz toomonitor hello hálózati újra jelentős késések fordulnak elő.</span><span class="sxs-lookup"><span data-stu-id="b87a5-215">After hello modified templates are in use, you will need toomonitor hello network again for significant latencies.</span></span> <span data-ttu-id="b87a5-216">Ha ezek továbbra is létezik, akkor szüksége lesz toorevisit a sávszélesség-sablonokat.</span><span class="sxs-lookup"><span data-stu-id="b87a5-216">If these still exist, then you will need toorevisit your bandwidth templates.</span></span>

<span data-ttu-id="b87a5-217">**A Q**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-217">**Q**.</span></span> <span data-ttu-id="b87a5-218">Mi történik, ha az eszközön több kötettárolók ütemezi, hogy hozzon létre, de különböző korlátok vonatkoznak tooeach?</span><span class="sxs-lookup"><span data-stu-id="b87a5-218">What happens if multiple volume containers on my device have schedules that overlap but different limits apply tooeach?</span></span>

<span data-ttu-id="b87a5-219">**A**.</span><span class="sxs-lookup"><span data-stu-id="b87a5-219">**A**.</span></span> <span data-ttu-id="b87a5-220">Tegyük fel, hogy rendelkezik-e egy eszközön keresztül a 3 kötettárolók.</span><span class="sxs-lookup"><span data-stu-id="b87a5-220">Let's assume that you have a device with 3 volume containers.</span></span> <span data-ttu-id="b87a5-221">hello ezekhez a tárolókhoz teljesen kapcsolódó átfedésben vannak.</span><span class="sxs-lookup"><span data-stu-id="b87a5-221">hello schedules associated with these containers completely overlap.</span></span> <span data-ttu-id="b87a5-222">Az egyes tárolókban használt hello sávszélességkorlátok 15 MB/s, 5, 10 kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="b87a5-222">For each of these containers, hello bandwidth limits used are 5, 10, and 15 Mbps respectively.</span></span> <span data-ttu-id="b87a5-223">Ha i/o előforduló összes hello hello legalább hello 3 sávszélesség korlátja egy időben is alkalmazható, ezek a tárolók: Ebben az esetben az 5 MB/s, ezek i/o-kérelmek megosztás kimenő hello ugyanazon a várólistán.</span><span class="sxs-lookup"><span data-stu-id="b87a5-223">When I/O are occurring on all of these containers at hello same time, hello minimum of hello 3 bandwidth limits may be applied: in this case, 5 Mbps as these outgoing I/O requests share hello same queue.</span></span>

## <a name="best-practices-for-bandwidth-templates"></a><span data-ttu-id="b87a5-224">Gyakorlati tanácsok a sávszélesség-sablonok</span><span class="sxs-lookup"><span data-stu-id="b87a5-224">Best practices for bandwidth templates</span></span>

<span data-ttu-id="b87a5-225">Kövesse az alábbi gyakorlati tanácsok a StorSimple eszközhöz:</span><span class="sxs-lookup"><span data-stu-id="b87a5-225">Follow these best practices for your StorSimple device:</span></span>

* <span data-ttu-id="b87a5-226">Az eszköz tooenable változó sávszélesség-szabályozás hello hálózati átviteli hello eszköz különböző napszakokban hello konfigurálása sávszélesség sablonok.</span><span class="sxs-lookup"><span data-stu-id="b87a5-226">Configure bandwidth templates on your device tooenable variable throttling of hello network throughput by hello device at different times of hello day.</span></span> <span data-ttu-id="b87a5-227">A mentési ütemezések használata esetén a sávszélesség sablonok csúcsidőn hatékonyan használhatják fel a felhőműveletek további hálózati sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="b87a5-227">These bandwidth templates when used with backup schedules can effectively leverage additional network bandwidth for cloud operations during off-peak hours.</span></span>
* <span data-ttu-id="b87a5-228">Hello tényleges sávszélességre van szüksége az adott példány hello üzembe helyezési és hello szükséges helyreállítási idő célkitűzése (RTO) hello méretének kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="b87a5-228">Calculate hello actual bandwidth required for a particular deployment based on hello size of hello deployment and hello required recovery time objective (RTO).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b87a5-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b87a5-229">Next steps</span></span>

<span data-ttu-id="b87a5-230">További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b87a5-230">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

