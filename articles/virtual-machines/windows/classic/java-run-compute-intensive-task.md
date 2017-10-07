---
title: "a virtuális gép aaaCompute igényű Java-alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Azure virtuális gépen futó számítási igényű Java-alkalmazás, amely egy másik Java-alkalmazás figyelhető-e."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 02a198802a8d78bd444cd5a9197a78cb94f48e3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-compute-intensive-task-in-java-on-a-virtual-machine"></a><span data-ttu-id="6b319-103">Hogyan toorun számítási igényű feladat a Java virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="6b319-103">How toorun a compute-intensive task in Java on a virtual machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="6b319-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6b319-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6b319-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="6b319-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="6b319-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="6b319-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="6b319-107">Az Azure-ral a virtuális gép toohandle számítási igénybe vevő feladatokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6b319-107">With Azure, you can use a virtual machine toohandle compute-intensive tasks.</span></span> <span data-ttu-id="6b319-108">Például egy virtuális gép feladatokat és eredmények tooclient gépek vagy mobilalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="6b319-108">For example, a virtual machine can handle tasks and deliver results tooclient machines or mobile applications.</span></span> <span data-ttu-id="6b319-109">A cikk elolvasása után fog megértéséhez hogyan toocreate futtató számítási igényű Java-alkalmazások, amelyek virtuális gép egy másik Java-alkalmazás figyelhető-e.</span><span class="sxs-lookup"><span data-stu-id="6b319-109">After reading this article, you will have an understanding of how toocreate a virtual machine that runs a compute-intensive Java application that can be monitored by another Java application.</span></span>

<span data-ttu-id="6b319-110">Ez az oktatóanyag feltételezi, hogy tudja, hogyan toocreate Java konzol alkalmazások, könyvtárak tooyour Java-alkalmazások importálhatja, és hozhat létre egy Java-archívum (JAR).</span><span class="sxs-lookup"><span data-stu-id="6b319-110">This tutorial assumes you know how toocreate Java console applications, can import libraries tooyour Java application, and can generate a Java archive (JAR).</span></span> <span data-ttu-id="6b319-111">Nem ismeri a Microsoft Azure feltételezi.</span><span class="sxs-lookup"><span data-stu-id="6b319-111">No knowledge of Microsoft Azure is assumed.</span></span>

<span data-ttu-id="6b319-112">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="6b319-112">You will learn:</span></span>

* <span data-ttu-id="6b319-113">Hogyan toocreate egy Java fejlesztői készlet (JDK) rendelkező virtuális gép már telepítve.</span><span class="sxs-lookup"><span data-stu-id="6b319-113">How toocreate a virtual machine with a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="6b319-114">Hogyan tooremotely tooyour virtuálisgép jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="6b319-114">How tooremotely log in tooyour virtual machine.</span></span>
* <span data-ttu-id="6b319-115">Hogyan toocreate egy service bus névtér.</span><span class="sxs-lookup"><span data-stu-id="6b319-115">How toocreate a service bus namespace.</span></span>
* <span data-ttu-id="6b319-116">Hogyan toocreate Java-alkalmazások számítás-igényes feladattá hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="6b319-116">How toocreate a Java application that performs a compute-intensive task.</span></span>
* <span data-ttu-id="6b319-117">Hogyan toocreate egy Java-alkalmazás, amely figyeli a hello hello számítási igényű feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="6b319-117">How toocreate a Java application that monitors hello progress of hello compute-intensive task.</span></span>
* <span data-ttu-id="6b319-118">Hogyan toorun hello Java-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="6b319-118">How toorun hello Java applications.</span></span>
* <span data-ttu-id="6b319-119">Hogyan toostop hello Java-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="6b319-119">How toostop hello Java applications.</span></span>

<span data-ttu-id="6b319-120">Ez az oktatóanyag hello számítási-igényes tevékenység hello Traveling értékesítők probléma fogja használni.</span><span class="sxs-lookup"><span data-stu-id="6b319-120">This tutorial will use hello Traveling Salesman Problem for hello compute-intensive task.</span></span> <span data-ttu-id="6b319-121">hello az alábbiakban látható egy példa hello Java application futó hello számítási-igényes tevékenység.</span><span class="sxs-lookup"><span data-stu-id="6b319-121">hello following is an example of hello Java application running hello compute-intensive task.</span></span>

![Traveling értékesítők probléma solver][solver_output]

<span data-ttu-id="6b319-123">hello az alábbiakban látható egy példa hello Java alkalmazás figyelési hello számítási igényű feladat.</span><span class="sxs-lookup"><span data-stu-id="6b319-123">hello following is an example of hello Java application monitoring hello compute-intensive task.</span></span>

![Traveling értékesítők probléma ügyfél][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a><span data-ttu-id="6b319-125">a virtuális gépek toocreate</span><span class="sxs-lookup"><span data-stu-id="6b319-125">toocreate a virtual machine</span></span>
1. <span data-ttu-id="6b319-126">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="6b319-126">Log in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="6b319-127">Kattintson a **új**, kattintson a **számítási**, kattintson a **virtuális gép**, és kattintson a **a gyűjtemény**.</span><span class="sxs-lookup"><span data-stu-id="6b319-127">Click **New**, click **Compute**, click **Virtual machine**, and then click **From Gallery**.</span></span>
3. <span data-ttu-id="6b319-128">A hello **virtuális gép lemezképét válasszon** párbeszédpanelen jelölje ki **JDK 7 a Windows Server 2012**.</span><span class="sxs-lookup"><span data-stu-id="6b319-128">In hello **Virtual machine image select** dialog box, select **JDK 7 Windows Server 2012**.</span></span>
   <span data-ttu-id="6b319-129">Vegye figyelembe, hogy **JDK 6 Windows Server 2012** áll rendelkezésre, amennyiben örökölt alkalmazásokat, amelyek még nem kész toorun JDK 7.</span><span class="sxs-lookup"><span data-stu-id="6b319-129">Note that **JDK 6 Windows Server 2012** is available in case you have legacy applications that are not yet ready toorun in JDK 7.</span></span>
4. <span data-ttu-id="6b319-130">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b319-130">Click **Next**.</span></span>
5. <span data-ttu-id="6b319-131">A hello **virtuálisgép-konfiguráció** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="6b319-131">In hello **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="6b319-132">Adja meg a hello virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="6b319-132">Specify a name for hello virtual machine.</span></span>
   2. <span data-ttu-id="6b319-133">Adja meg a hello mérete toouse hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="6b319-133">Specify hello size toouse for hello virtual machine.</span></span>
   3. <span data-ttu-id="6b319-134">Adja meg egy nevet a hello rendszergazda hello **felhasználónév** mező.</span><span class="sxs-lookup"><span data-stu-id="6b319-134">Enter a name for hello administrator in hello **User Name** field.</span></span> <span data-ttu-id="6b319-135">Ezt a nevet és hello jelszót következő módba lép, használhatja őket toohello virtuális gép távoli bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="6b319-135">Remember this name and hello password you will enter next, you will use them when you remotely log in toohello virtual machine.</span></span>
   4. <span data-ttu-id="6b319-136">Adjon meg egy jelszót hello **új jelszó** mezőben, majd adja meg újra a hello **megerősítése** mező.</span><span class="sxs-lookup"><span data-stu-id="6b319-136">Enter a password in hello **New password** field, and re-enter it in hello **Confirm** field.</span></span> <span data-ttu-id="6b319-137">Ez a hello rendszergazdai fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6b319-137">This is hello Administrator account password.</span></span>
   5. <span data-ttu-id="6b319-138">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b319-138">Click **Next**.</span></span>
6. <span data-ttu-id="6b319-139">Hello a következő **virtuálisgép-konfiguráció** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="6b319-139">In hello next **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="6b319-140">A **felhőalapú szolgáltatás**, hello alapértelmezett **hozzon létre egy új felhőalapú szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="6b319-140">For **Cloud service**, use hello default **Create a new cloud service**.</span></span>
   2. <span data-ttu-id="6b319-141">a következő hello **felhőalapú szolgáltatás DNS-név** cloudapp.net belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6b319-141">hello value for **Cloud service DNS name** must be unique across cloudapp.net.</span></span> <span data-ttu-id="6b319-142">Szükség esetén módosítsa ezt az értéket úgy, hogy Azure azt jelzi, hogy egyedi.</span><span class="sxs-lookup"><span data-stu-id="6b319-142">If needed, modify this value so that Azure indicates it is unique.</span></span>
   3. <span data-ttu-id="6b319-143">Adjon meg egy régiót, az affinitáscsoport vagy a virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="6b319-143">Specify a region, affinity group, or virtual network.</span></span> <span data-ttu-id="6b319-144">Ez az oktatóanyag céljából, adjon meg egy régiót például **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="6b319-144">For purposes of this tutorial, specify a region such as **West US**.</span></span>
   4. <span data-ttu-id="6b319-145">A **Tárfiók**, jelölje be **automatikusan létrehozott tárfiók használata**.</span><span class="sxs-lookup"><span data-stu-id="6b319-145">For **Storage Account**, select **Use an automatically generated storage account**.</span></span>
   5. <span data-ttu-id="6b319-146">A **rendelkezésre állási csoport**, jelölje be **(nincs)**.</span><span class="sxs-lookup"><span data-stu-id="6b319-146">For **Availability Set**, select **(None)**.</span></span>
   6. <span data-ttu-id="6b319-147">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b319-147">Click **Next**.</span></span>
7. <span data-ttu-id="6b319-148">A végső hello **virtuálisgép-konfiguráció** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="6b319-148">In hello final **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="6b319-149">Fogadja el a hello alapértelmezett végpont bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="6b319-149">Accept hello default endpoint entries.</span></span>
   2. <span data-ttu-id="6b319-150">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b319-150">Click **Complete**.</span></span>

## <a name="tooremotely-log-in-tooyour-virtual-machine"></a><span data-ttu-id="6b319-151">a virtuális gép tooyour tooremotely napló</span><span class="sxs-lookup"><span data-stu-id="6b319-151">tooremotely log in tooyour virtual machine</span></span>
1. <span data-ttu-id="6b319-152">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="6b319-152">Log on toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="6b319-153">Kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="6b319-153">Click **Virtual machines**.</span></span>
3. <span data-ttu-id="6b319-154">Kattintson a kívánt toolog a hello virtuális gép hello nevére.</span><span class="sxs-lookup"><span data-stu-id="6b319-154">Click hello name of hello virtual machine that you want toolog in to.</span></span>
4. <span data-ttu-id="6b319-155">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6b319-155">Click **Connect**.</span></span>
5. <span data-ttu-id="6b319-156">Válaszoljon toohello kérni fogja a szükséges tooconnect toohello virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="6b319-156">Respond toohello prompts as needed tooconnect toohello virtual machine.</span></span> <span data-ttu-id="6b319-157">Amikor a rendszer kéri, hello rendszergazda felhasználónevét és jelszavát, hello virtuális gép létrehozásakor adott hello értékeket használja.</span><span class="sxs-lookup"><span data-stu-id="6b319-157">When prompted for hello administrator name and password, use hello values that you provided when you created hello virtual machine.</span></span>

<span data-ttu-id="6b319-158">Vegye figyelembe, hogy hello Azure Service Bus-funkciókat igényli hello Baltimore CyberTrust legfelső szintű tanúsítvány toobe a JRE részeként telepített **cacerts** tárolja.</span><span class="sxs-lookup"><span data-stu-id="6b319-158">Note that hello Azure Service Bus functionality requires hello Baltimore CyberTrust Root certificate toobe installed as part of your JRE's **cacerts** store.</span></span> <span data-ttu-id="6b319-159">Ez a tanúsítvány automatikusan hello Java-futtatókörnyezet (JRE) ebben az oktatóanyagban használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6b319-159">This certificate is automatically included in hello Java Runtime Environment (JRE) used by this tutorial.</span></span> <span data-ttu-id="6b319-160">Ha nincs ezt a tanúsítványt a a JRE **cacerts** tárolja, lásd: [hozzáadása a Java-hitelesítésszolgáltató tanúsítványtároló tanúsítvány toohello] [ add_ca_cert] hozzáadásával kapcsolatos (valamint információ a cacerts tárolójában hello tanúsítványok megtekintése).</span><span class="sxs-lookup"><span data-stu-id="6b319-160">If you do not have this certificate in your JRE **cacerts** store, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert] for information on adding it (as well as information on viewing hello certificates in your cacerts store).</span></span>

## <a name="how-toocreate-a-service-bus-namespace"></a><span data-ttu-id="6b319-161">Hogyan toocreate egy service bus névtér</span><span class="sxs-lookup"><span data-stu-id="6b319-161">How toocreate a service bus namespace</span></span>
<span data-ttu-id="6b319-162">a Service Bus használatával toobegin várólisták az Azure-ban, először létre kell hoznia egy szolgáltatásnévteret.</span><span class="sxs-lookup"><span data-stu-id="6b319-162">toobegin using Service Bus queues in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="6b319-163">A névtér egy hatókörkezelési tárolót biztosít az alkalmazáson belül a Service Bus erőforrásainak kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="6b319-163">A service namespace provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="6b319-164">a névtér toocreate:</span><span class="sxs-lookup"><span data-stu-id="6b319-164">toocreate a service namespace:</span></span>

1. <span data-ttu-id="6b319-165">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="6b319-165">Log on toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="6b319-166">A klasszikus Azure portálon hello hello bal alsó navigációs ablaktábláján kattintson **Service Bus, a hozzáférés-vezérlés és a gyorsítótár**.</span><span class="sxs-lookup"><span data-stu-id="6b319-166">In hello lower-left navigation pane of hello Azure classic portal, click **Service Bus, Access Control & Caching**.</span></span>
3. <span data-ttu-id="6b319-167">A klasszikus Azure portálon hello hello bal felső ablaktábláján kattintson hello **Service Bus** csomópont, és kattintson a hello **új** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b319-167">In hello upper-left pane of hello Azure classic portal, click hello **Service Bus** node, and then click hello **New** button.</span></span>  
   <span data-ttu-id="6b319-168">![Service Bus csomópont képernyőképe][svc_bus_node]</span><span class="sxs-lookup"><span data-stu-id="6b319-168">![Service Bus Node screenshot][svc_bus_node]</span></span>
4. <span data-ttu-id="6b319-169">A hello **hozzon létre egy új szolgáltatás Namespace** párbeszédpanelen adja meg egy **Namespace**, és toomake meg arról, hogy a rendszer egyedi, kattintson a **ellenőrizze a rendelkezésre állási** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b319-169">In hello **Create a new Service Namespace** dialog box, enter a **Namespace**, and then toomake sure that it is unique, click the **Check Availability** button.</span></span>  
   <span data-ttu-id="6b319-170">![Hozzon létre egy új Namespace képernyőképe][create_namespace]</span><span class="sxs-lookup"><span data-stu-id="6b319-170">![Create a New Namespace screenshot][create_namespace]</span></span>
5. <span data-ttu-id="6b319-171">Miután meggyőződött arról, hogy hello névtér neve elérhető, válassza ki az országot vagy régiót, amelyben a névtér üzemeltetve lesz, és kattintson a hello **létrehozása Namespace** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b319-171">After making sure hello namespace name is available, choose the country or region in which your namespace should be hosted, and then click hello **Create Namespace** button.</span></span>  
   
   <span data-ttu-id="6b319-172">hello létrehozott névtér hello a klasszikus Azure portálon megjelenik, és egy rövid ideig tooactivate vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="6b319-172">hello namespace you created will then appear in hello Azure classic portal and takes a moment tooactivate.</span></span> <span data-ttu-id="6b319-173">Várjon, amíg a hello állapota **aktív** a hello következő lépés elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="6b319-173">Wait until hello status is **Active** before continuing with hello next step.</span></span>

## <a name="obtain-hello-default-management-credentials-for-hello-namespace"></a><span data-ttu-id="6b319-174">Hello alapértelmezett felügyeleti hitelesítő adatok beszerzése hello névtér</span><span class="sxs-lookup"><span data-stu-id="6b319-174">Obtain hello Default Management Credentials for hello namespace</span></span>
<span data-ttu-id="6b319-175">A sorrend tooperform felügyeleti műveletek, például létrehozhat egy várólista hello új névtéren tooobtain hello felügyeleti hitelesítő adatokat a névtér kell.</span><span class="sxs-lookup"><span data-stu-id="6b319-175">In order tooperform management operations, such as creating a queue, on hello new namespace, you need tooobtain hello management credentials for the namespace.</span></span>

1. <span data-ttu-id="6b319-176">Hello bal oldali navigációs panelen, kattintson a hello **Service Bus** csomópontra hello az elérhető névterek listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6b319-176">In hello left navigation pane, click hello **Service Bus** node to display hello list of available namespaces.</span></span>
   <span data-ttu-id="6b319-177">![Elérhető névterek képernyőképe][avail_namespaces]</span><span class="sxs-lookup"><span data-stu-id="6b319-177">![Available Namespaces screenshot][avail_namespaces]</span></span>
2. <span data-ttu-id="6b319-178">Válassza ki a most létrehozott hello listán látható hello névteret.</span><span class="sxs-lookup"><span data-stu-id="6b319-178">Select hello namespace you just created from hello list shown.</span></span>
   <span data-ttu-id="6b319-179">![Namespace lista képernyőképe][namespace_list]</span><span class="sxs-lookup"><span data-stu-id="6b319-179">![Namespace List screenshot][namespace_list]</span></span>
3. <span data-ttu-id="6b319-180">jobb oldali hello **tulajdonságok** ablaktábla listázza az új névtéren hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="6b319-180">hello right-hand **Properties** pane lists hello properties for the new namespace.</span></span>
   <span data-ttu-id="6b319-181">![Képernyőfelvétel a Tulajdonságok panelen][properties_pane]</span><span class="sxs-lookup"><span data-stu-id="6b319-181">![Properties Pane screenshot][properties_pane]</span></span>
4. <span data-ttu-id="6b319-182">Hello **alapértelmezett kulcs** van-e rejtve.</span><span class="sxs-lookup"><span data-stu-id="6b319-182">hello **Default Key** is hidden.</span></span> <span data-ttu-id="6b319-183">Kattintson a hello **nézet** gomb toodisplay hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="6b319-183">Click hello **View** button toodisplay hello security credentials.</span></span>
   <span data-ttu-id="6b319-184">![Alapértelmezett kulcs képernyőképe][default_key]</span><span class="sxs-lookup"><span data-stu-id="6b319-184">![Default Key screenshot][default_key]</span></span>
5. <span data-ttu-id="6b319-185">Jegyezze fel a hello **alapértelmezett kibocsátó** és hello **alapértelmezett kulcs** módon használja, akkor ez az információ alább tooperform műveletek a névteret.</span><span class="sxs-lookup"><span data-stu-id="6b319-185">Make a note of hello **Default Issuer** and hello **Default Key** as you will use this information below tooperform operations with the namespace.</span></span>

## <a name="how-toocreate-a-java-application-that-performs-a-compute-intensive-task"></a><span data-ttu-id="6b319-186">Hogyan toocreate számítási-igényes feladattá végző Java-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="6b319-186">How toocreate a Java application that performs a compute-intensive task</span></span>
1. <span data-ttu-id="6b319-187">A fejlesztői számítógépén (amelyhez nem tartozik toobe hello virtuális gép, amelyet hozott létre), letöltési hello [Javához készült Azure SDK](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="6b319-187">On your development machine (which does not have toobe hello virtual machine that you created), download hello [Azure SDK for Java](https://azure.microsoft.com/develop/java/).</span></span>
2. <span data-ttu-id="6b319-188">Hozzon létre egy Java konzolalkalmazást, ez a szakasz végén hello hello példa kód használatával.</span><span class="sxs-lookup"><span data-stu-id="6b319-188">Create a Java console application using hello example code at hello end of this section.</span></span> <span data-ttu-id="6b319-189">Ebben az oktatóanyagban fogjuk használni **TSPSolver.java** hello Java fájlnév.</span><span class="sxs-lookup"><span data-stu-id="6b319-189">In this tutorial, we'll use **TSPSolver.java** as hello Java file name.</span></span> <span data-ttu-id="6b319-190">Hello módosítása **a\_szolgáltatás\_bus\_névtér**, **a\_szolgáltatás\_bus\_tulajdonos**, és **a\_szolgáltatás\_bus\_kulcs** toouse helyőrzőket a service bus **névtér**, **alapértelmezett kibocsátó** és  **Alapértelmezett kulcs** értékei, illetve.</span><span class="sxs-lookup"><span data-stu-id="6b319-190">Modify hello **your\_service\_bus\_namespace**, **your\_service\_bus\_owner**, and **your\_service\_bus\_key** placeholders toouse your service bus **namespace**, **Default Issuer** and **Default Key** values, respectively.</span></span>
3. <span data-ttu-id="6b319-191">Után kódolási, exportálás hello tooa futtatható Java archívumából (JAR), és a csomag hello szükséges hello a szalagtárak JAR jön létre.</span><span class="sxs-lookup"><span data-stu-id="6b319-191">After coding, export hello application tooa runnable Java archive (JAR), and package hello required libraries into hello generated JAR.</span></span> <span data-ttu-id="6b319-192">Ebben az oktatóanyagban fogjuk használni **TSPSolver.jar** generált hello JAR neveként.</span><span class="sxs-lookup"><span data-stu-id="6b319-192">In this tutorial, we'll use **TSPSolver.jar** as hello generated JAR name.</span></span>

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often tooprovide an update toohello console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as hello default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing toooccur other than creating hello queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing toooccur other than deleting hello queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume hello value passed in is hello number of cities toosolve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-toocreate-a-java-application-that-monitors-hello-progress-of-hello-compute-intensive-task"></a><span data-ttu-id="6b319-193">Hogyan toocreate egy Java-alkalmazás, amely figyeli a hello hello számítási-igényes tevékenység végrehajtási állapota</span><span class="sxs-lookup"><span data-stu-id="6b319-193">How toocreate a Java application that monitors hello progress of hello compute-intensive task</span></span>
1. <span data-ttu-id="6b319-194">A fejlesztői számítógépén ez a szakasz végén hello hello példakódot használatával Java Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6b319-194">On your development machine, create a Java console application using hello example code at hello end of this section.</span></span> <span data-ttu-id="6b319-195">Ebben az oktatóanyagban fogjuk használni **TSPClient.java** hello Java fájlnév.</span><span class="sxs-lookup"><span data-stu-id="6b319-195">In this tutorial, we'll use **TSPClient.java** as hello Java file name.</span></span> <span data-ttu-id="6b319-196">Ahogy korábban is látható, módosítsa a hello **a\_szolgáltatás\_bus\_névtér**, **a\_szolgáltatás\_bus\_tulajdonos**, és **a\_szolgáltatás\_bus\_kulcs** toouse helyőrzőket a service bus **névtér**, **alapértelmezett kibocsátó**és **alapértelmezett kulcs** értékei, illetve.</span><span class="sxs-lookup"><span data-stu-id="6b319-196">As shown earlier, modify hello **your\_service\_bus\_namespace**, **your\_service\_bus\_owner**, and **your\_service\_bus\_key** placeholders toouse your service bus **namespace**, **Default Issuer** and **Default Key** values, respectively.</span></span>
2. <span data-ttu-id="6b319-197">Hello alkalmazás tooa exportálás futtatható JAR, és a csomag hello szükséges hello a szalagtárak JAR jön létre.</span><span class="sxs-lookup"><span data-stu-id="6b319-197">Export hello application tooa runnable JAR, and package hello required libraries into hello generated JAR.</span></span> <span data-ttu-id="6b319-198">Ebben az oktatóanyagban fogjuk használni **TSPClient.jar** generált hello JAR neveként.</span><span class="sxs-lookup"><span data-stu-id="6b319-198">In this tutorial, we'll use **TSPClient.jar** as hello generated JAR name.</span></span>

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as hello default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display hello queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing toooccur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // hello queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-toorun-hello-java-applications"></a><span data-ttu-id="6b319-199">Hogyan toorun hello Java-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="6b319-199">How toorun hello Java applications</span></span>
<span data-ttu-id="6b319-200">Hello számítási igényű alkalmazás futtatásához, első toocreate hello várólistát, majd toosolve hello utazás Saleseman probléma, amely felveszi hello aktuális legjobb útvonalat toohello service bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="6b319-200">Run hello compute-intensive application, first toocreate hello queue, then toosolve hello Traveling Saleseman Problem, which will add hello current best route toohello service bus queue.</span></span> <span data-ttu-id="6b319-201">Hello közben számítási igényű alkalmazásra az fut (vagy ezek után), futtatási hello ügyfél toodisplay eredmények hello service bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="6b319-201">While hello compute-intensive application is running (or afterwards), run hello client toodisplay results from hello service bus queue.</span></span>

### <a name="toorun-hello-compute-intensive-application"></a><span data-ttu-id="6b319-202">toorun hello számítási igényű alkalmazás</span><span class="sxs-lookup"><span data-stu-id="6b319-202">toorun hello compute-intensive application</span></span>
1. <span data-ttu-id="6b319-203">Jelentkezzen be a virtuális gép tooyour.</span><span class="sxs-lookup"><span data-stu-id="6b319-203">Log on tooyour virtual machine.</span></span>
2. <span data-ttu-id="6b319-204">Hozzon létre egy mappát, ahol az alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="6b319-204">Create a folder where you will run your application.</span></span> <span data-ttu-id="6b319-205">Például **c:\TSP**.</span><span class="sxs-lookup"><span data-stu-id="6b319-205">For example, **c:\TSP**.</span></span>
3. <span data-ttu-id="6b319-206">Másolás **TSPSolver.jar** túl**c:\TSP**,</span><span class="sxs-lookup"><span data-stu-id="6b319-206">Copy **TSPSolver.jar** too**c:\TSP**,</span></span>
4. <span data-ttu-id="6b319-207">Hozzon létre egy fájlt **c:\TSP\cities.txt** a tartalom a következő hello.</span><span class="sxs-lookup"><span data-stu-id="6b319-207">Create a file named **c:\TSP\cities.txt** with hello following contents.</span></span>
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. <span data-ttu-id="6b319-208">Egy parancssorban módosítsa a könyvtárak tooc:\TSP.</span><span class="sxs-lookup"><span data-stu-id="6b319-208">At a command prompt, change directories tooc:\TSP.</span></span>
6. <span data-ttu-id="6b319-209">Győződjön meg arról, hello JRE bin mappájában megtalálható-e hello PATH környezeti változóhoz.</span><span class="sxs-lookup"><span data-stu-id="6b319-209">Ensure hello JRE's bin folder is in hello PATH environment variable.</span></span>
7. <span data-ttu-id="6b319-210">Hello Szolgáltató solver kombinációinak futtatása előtt kell toocreate hello service bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="6b319-210">You'll need toocreate hello service bus queue before you run hello TSP solver permutations.</span></span> <span data-ttu-id="6b319-211">Futtassa a következő parancs toocreate hello service bus-üzenetsorba hello.</span><span class="sxs-lookup"><span data-stu-id="6b319-211">Run hello following command toocreate hello service bus queue.</span></span>
   
        java -jar TSPSolver.jar createqueue
8. <span data-ttu-id="6b319-212">Most, hogy hello várólista létrehozása hello Szolgáltató solver kombinációinak is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="6b319-212">Now that hello queue is created, you can run hello TSP solver permutations.</span></span> <span data-ttu-id="6b319-213">Például futtassa a következő parancs toorun hello solver 8 városokat hello.</span><span class="sxs-lookup"><span data-stu-id="6b319-213">For example, run hello following command toorun hello solver for 8 cities.</span></span>
   
        java -jar TSPSolver.jar 8
   
   <span data-ttu-id="6b319-214">Ha nem adja meg egy számot, akkor a 10 városokat fog futni.</span><span class="sxs-lookup"><span data-stu-id="6b319-214">If you don't specify a number, it will run for 10 cities.</span></span> <span data-ttu-id="6b319-215">Hello solver aktuális legrövidebb útvonalak talál, mivel azt hozzáadja azokat toohello várólista.</span><span class="sxs-lookup"><span data-stu-id="6b319-215">As hello solver finds current shortest routes, it will add them toohello queue.</span></span>

> [!NOTE]
> <span data-ttu-id="6b319-216">nagyobb hello hello száma, hogy megadja, hello hosszabb hello solver fog futni.</span><span class="sxs-lookup"><span data-stu-id="6b319-216">hello larger hello number that you specify, hello longer hello solver will run.</span></span> <span data-ttu-id="6b319-217">Például futtató 14 városokat eltarthat néhány percig, és fut, amely 15 várost több óráig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="6b319-217">For example, running for 14 cities could take several minutes, and running for 15 cities could take several hours.</span></span> <span data-ttu-id="6b319-218">Növelése too16 vagy több város tartozik (idővel hét, hónap és év) futásidejű napon eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="6b319-218">Increasing too16 or more cities could result in days of runtime (eventually weeks, months, and years).</span></span> <span data-ttu-id="6b319-219">Ez a gyors növekedése toohello hello városokat növekszik hello száma hello Solver kiértékelése kombinációinak száma miatt.</span><span class="sxs-lookup"><span data-stu-id="6b319-219">This is due toohello rapid increase in hello number of permutations evaluated by hello solver as hello number of cities increases.</span></span>
> 
> 

### <a name="how-toorun-hello-monitoring-client-application"></a><span data-ttu-id="6b319-220">Hogyan toorun hello figyelési ügyfélalkalmazás</span><span class="sxs-lookup"><span data-stu-id="6b319-220">How toorun hello monitoring client application</span></span>
1. <span data-ttu-id="6b319-221">Jelentkezzen be tooyour gép hello ügyfélalkalmazást futtató lesz.</span><span class="sxs-lookup"><span data-stu-id="6b319-221">Log on tooyour machine where you will run hello client application.</span></span> <span data-ttu-id="6b319-222">Ez nem szükséges toobe hello egyazon számítógépen futó hello **TSPSolver** alkalmazás, bár lehet.</span><span class="sxs-lookup"><span data-stu-id="6b319-222">This does not need toobe hello same machine running hello **TSPSolver** application, although it can be.</span></span>
2. <span data-ttu-id="6b319-223">Hozzon létre egy mappát, ahol az alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="6b319-223">Create a folder where you will run your application.</span></span> <span data-ttu-id="6b319-224">Például **c:\TSP**.</span><span class="sxs-lookup"><span data-stu-id="6b319-224">For example, **c:\TSP**.</span></span>
3. <span data-ttu-id="6b319-225">Másolás **TSPClient.jar** túl**c:\TSP**,</span><span class="sxs-lookup"><span data-stu-id="6b319-225">Copy **TSPClient.jar** too**c:\TSP**,</span></span>
4. <span data-ttu-id="6b319-226">Győződjön meg arról, hello JRE bin mappájában megtalálható-e hello PATH környezeti változóhoz.</span><span class="sxs-lookup"><span data-stu-id="6b319-226">Ensure hello JRE's bin folder is in hello PATH environment variable.</span></span>
5. <span data-ttu-id="6b319-227">Egy parancssorban módosítsa a könyvtárak tooc:\TSP.</span><span class="sxs-lookup"><span data-stu-id="6b319-227">At a command prompt, change directories tooc:\TSP.</span></span>
6. <span data-ttu-id="6b319-228">Futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="6b319-228">Run hello following command.</span></span>
   
        java -jar TSPClient.jar
   
    <span data-ttu-id="6b319-229">Szükség esetén adja meg percben toosleep Between hello várólista, a parancssori argumentum történő ellenőrzése hello számát.</span><span class="sxs-lookup"><span data-stu-id="6b319-229">Optionally, specify hello number of minutes toosleep in between checking hello queue, by passing in a command-line argument.</span></span> <span data-ttu-id="6b319-230">hello alapértelmezett alvó időszak hello várólista ellenőrzése a következő: 3 percenként, amely akkor használatos, ha nincs parancssori argumentum túl átadása**TSPClient**.</span><span class="sxs-lookup"><span data-stu-id="6b319-230">hello default sleep period for checking hello queue is 3 minutes, which is used if no command-line argument is passed too**TSPClient**.</span></span> <span data-ttu-id="6b319-231">Ha toouse egy másik értéket hello alvó időtartam alatt, például egy percet, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="6b319-231">If you want toouse a different value for hello sleep interval, for example, one minute, run hello following command.</span></span>
   
        java -jar TSPClient.jar 1
   
    <span data-ttu-id="6b319-232">hello ügyfél fog futni, amíg azt egy üzenetet várólista a "Teljes".</span><span class="sxs-lookup"><span data-stu-id="6b319-232">hello client will run until it sees a queue message of "Complete".</span></span> <span data-ttu-id="6b319-233">Ne feledje, hogy ha több példányban hello solver hello ügyfél futtatása nélkül futtatja, előfordulhat, hogy toorun hello ügyfél többször toocompletely üres hello várólista.</span><span class="sxs-lookup"><span data-stu-id="6b319-233">Note that if you run multiple occurrences of hello solver without running hello client, you may need toorun hello client multiple times toocompletely empty hello queue.</span></span> <span data-ttu-id="6b319-234">Másik lehetőségként hello várólista törlése, és akkor hozza létre újra.</span><span class="sxs-lookup"><span data-stu-id="6b319-234">Alternatively, you can delete hello queue and then create it again.</span></span> <span data-ttu-id="6b319-235">toodelete hello várólista, futtassa a következő hello **TSPSolver** (nem **TSPClient**) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6b319-235">toodelete hello queue, run hello following **TSPSolver** (not **TSPClient**)  command.</span></span>
   
        java -jar TSPSolver.jar deletequeue
   
    <span data-ttu-id="6b319-236">hello solver fog futni, amíg be nem fejezi megvizsgálta az összes útvonal.</span><span class="sxs-lookup"><span data-stu-id="6b319-236">hello solver will run until it finishes examining all routes.</span></span>

## <a name="how-toostop-hello-java-applications"></a><span data-ttu-id="6b319-237">Hogyan toostop hello Java-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="6b319-237">How toostop hello Java applications</span></span>
<span data-ttu-id="6b319-238">Hello solver és ügyfélalkalmazások lenyomhatja **Ctrl + C** tooexit, ha azt szeretné, hogy tooend előzetes toonormal befejezését.</span><span class="sxs-lookup"><span data-stu-id="6b319-238">For both hello solver and client applications, you can press **Ctrl+C** tooexit if you want tooend prior toonormal completion.</span></span>

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
