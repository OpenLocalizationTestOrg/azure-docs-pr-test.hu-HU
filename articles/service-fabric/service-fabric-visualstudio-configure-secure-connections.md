---
title: "biztonságos Azure Service Fabric fürt kapcsolatok aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Visual Studio tooconfigure biztonságos kapcsolatok hello Azure Service Fabric-fürt által támogatott."
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: ed3e5043264cf026f74e24ca09b40ccc70086cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-connections-tooa-service-fabric-cluster-from-visual-studio"></a><span data-ttu-id="c04b6-103">Biztonságos kapcsolat tooa Service Fabric-fürt a Visual Studio konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c04b6-103">Configure secure connections tooa Service Fabric cluster from Visual Studio</span></span>
<span data-ttu-id="c04b6-104">Ismerje meg, hogyan toouse Visual Studio toosecurely elérni az Azure Service Fabric-fürt beállított hozzáférés-vezérlési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="c04b6-104">Learn how toouse Visual Studio toosecurely access an Azure Service Fabric cluster with access control policies configured.</span></span>

## <a name="cluster-connection-types"></a><span data-ttu-id="c04b6-105">Fürt-kapcsolat típusai</span><span class="sxs-lookup"><span data-stu-id="c04b6-105">Cluster connection types</span></span>
<span data-ttu-id="c04b6-106">Kétféle típusú kapcsolatok hello Azure Service Fabric-fürt által támogatott: **nem biztonságos** kapcsolatok és **tanúsítványalapú x509** biztonságos kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="c04b6-106">Two types of connections are supported by hello Azure Service Fabric cluster: **non-secure** connections and **x509 certificate-based** secure connections.</span></span> <span data-ttu-id="c04b6-107">(A Service Fabric-fürtök helyben tárolt, **Windows** és **dSTS** hitelesítések is támogatottak.) Hello fürt létrehozásakor rendelkezik tooconfigure hello fürt kapcsolattípus.</span><span class="sxs-lookup"><span data-stu-id="c04b6-107">(For Service Fabric clusters hosted on-premises, **Windows** and **dSTS** authentications are also supported.) You have tooconfigure hello cluster connection type when hello cluster is being created.</span></span> <span data-ttu-id="c04b6-108">A már létrehozott, hello kapcsolat típusa nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="c04b6-108">Once it's created, hello connection type can’t be changed.</span></span>

<span data-ttu-id="c04b6-109">hello Visual Studio Service Fabric-eszközök támogatása közzétételi csatlakozó tooa fürt összes hitelesítéstípussal.</span><span class="sxs-lookup"><span data-stu-id="c04b6-109">hello Visual Studio Service Fabric tools support all authentication types for connecting tooa cluster for publishing.</span></span> <span data-ttu-id="c04b6-110">Lásd: [a Service Fabric-fürt beállítása az Azure-portálon hello](service-fabric-cluster-creation-via-portal.md) útmutatást tooset biztonságos Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="c04b6-110">See [Setting up a Service Fabric cluster from hello Azure portal](service-fabric-cluster-creation-via-portal.md) for instructions on how tooset up a secure Service Fabric cluster.</span></span>

## <a name="configure-cluster-connections-in-publish-profiles"></a><span data-ttu-id="c04b6-111">Fürt kapcsolatok konfigurálása a profilok közzététele</span><span class="sxs-lookup"><span data-stu-id="c04b6-111">Configure cluster connections in publish profiles</span></span>
<span data-ttu-id="c04b6-112">Ha közzéteszi a Service Fabric-projektet, a Visual Studio, használja a hello **Fabric-alkalmazás közzététele** párbeszédpanel bezárásához toochoose egy Azure Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="c04b6-112">If you publish a Service Fabric project from Visual Studio, use hello **Publish Service Fabric Application** dialog box toochoose an Azure Service Fabric cluster.</span></span> <span data-ttu-id="c04b6-113">A **csatlakozási végpont**, jelölje be az előfizetéshez tartozó meglévő fürt.</span><span class="sxs-lookup"><span data-stu-id="c04b6-113">Under **Connection endpoint**, select an existing cluster under your subscription.</span></span>

![hello ** közzététele Service Fabric Application ** párbeszédpanel használt tooconfigure a Service Fabric kapcsolat.][publishdialog]

<span data-ttu-id="c04b6-115">Hello **Fabric-alkalmazás közzététele** párbeszédpanel automatikusan ellenőrzi az hello fürtkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="c04b6-115">hello **Publish Service Fabric Application** dialog box automatically validates hello cluster connection.</span></span> <span data-ttu-id="c04b6-116">Ha a rendszer kéri, jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="c04b6-116">If prompted, sign in tooyour Azure account.</span></span> <span data-ttu-id="c04b6-117">Ha az ellenőrzés eredménye akkor megfelelő, az azt jelenti, hogy a rendszer hello megfelelő tanúsítványok telepítve tooconnect toohello fürt biztonságosan, vagy a fürt nem biztonságos.</span><span class="sxs-lookup"><span data-stu-id="c04b6-117">If validation passes, it means that your system has hello correct certificates installed tooconnect toohello cluster securely, or your cluster is non-secure.</span></span> <span data-ttu-id="c04b6-118">Az érvényesítési hibák okozhatja hálózati probléma, vagy nem rendelkezik a rendszer megfelelően konfigurált tooconnect tooa biztonságos fürt.</span><span class="sxs-lookup"><span data-stu-id="c04b6-118">Validation failures can be caused by network issues or by not having your system correctly configured tooconnect tooa secure cluster.</span></span>

![hello ** közzététele Service Fabric alkalmazás ** párbeszédpanel ellenőrzi, hogy egy meglévő, megfelelően konfigurálva a Service Fabric-fürt kapcsolat.][selectsfcluster]

### <a name="tooconnect-tooa-secure-cluster"></a><span data-ttu-id="c04b6-120">tooconnect tooa biztonságos fürt</span><span class="sxs-lookup"><span data-stu-id="c04b6-120">tooconnect tooa secure cluster</span></span>
1. <span data-ttu-id="c04b6-121">Ellenőrizze, hogy a cél fürt bizalmi kapcsolatok hello hello ügyféltanúsítványok hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="c04b6-121">Make sure you can access one of hello client certificates that hello destination cluster trusts.</span></span> <span data-ttu-id="c04b6-122">hello tanúsítvány általában megosztott, személyes információcsere (.pfx) fájlként.</span><span class="sxs-lookup"><span data-stu-id="c04b6-122">hello certificate is usually shared as a Personal Information Exchange (.pfx) file.</span></span> <span data-ttu-id="c04b6-123">Lásd: [a Service Fabric-fürt beállítása az Azure-portálon hello](service-fabric-cluster-creation-via-portal.md) hogyan férhetnek hozzá a tooconfigure hello server toogrant tooa ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="c04b6-123">See [Setting up a Service Fabric cluster from hello Azure portal](service-fabric-cluster-creation-via-portal.md) for how tooconfigure hello server toogrant access tooa client.</span></span>
2. <span data-ttu-id="c04b6-124">Hello megbízható tanúsítvány telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c04b6-124">Install hello trusted certificate.</span></span> <span data-ttu-id="c04b6-125">toodo, kattintson duplán a hello .pfx fájlt, vagy hello PowerShell parancsfájl Import-PfxCertificate tooimport hello tanúsítványokat használnak.</span><span class="sxs-lookup"><span data-stu-id="c04b6-125">toodo this, double-click hello .pfx file, or use hello PowerShell script Import-PfxCertificate tooimport hello certificates.</span></span> <span data-ttu-id="c04b6-126">Hello tanúsítvány telepítése túl**Cert: \LocalMachine\My**.</span><span class="sxs-lookup"><span data-stu-id="c04b6-126">Install hello certificate too**Cert:\LocalMachine\My**.</span></span> <span data-ttu-id="c04b6-127">OK tooaccept hello tanúsítvány importálása során az összes alapértelmezett beállítást.</span><span class="sxs-lookup"><span data-stu-id="c04b6-127">It's OK tooaccept all default settings while importing hello certificate.</span></span>
3. <span data-ttu-id="c04b6-128">Válassza ki a hello **közzététel...**  hello helyi menüjének hello projekt tooopen hello parancs **Azure-alkalmazás közzététele** párbeszédpanel megnyitásához, majd jelölje be hello cél fürtnek.</span><span class="sxs-lookup"><span data-stu-id="c04b6-128">Choose hello **Publish...** command on hello shortcut menu of hello project tooopen hello **Publish Azure Application** dialog box and then select hello target cluster.</span></span> <span data-ttu-id="c04b6-129">hello eszköz automatikusan feloldja a hello kapcsolat, és menti hello biztonságos kapcsolatot hello paramétereiben közzététele profil.</span><span class="sxs-lookup"><span data-stu-id="c04b6-129">hello tool automatically resolves hello connection and saves hello secure connection parameters in hello publish profile.</span></span>
4. <span data-ttu-id="c04b6-130">Választható lehetőség: Hello szerkesztheti profil toospecify biztonságos fürtkapcsolat közzététele.</span><span class="sxs-lookup"><span data-stu-id="c04b6-130">Optional: You can edit hello publish profile toospecify a secure cluster connection.</span></span>
   
   <span data-ttu-id="c04b6-131">Manuálisan szerkesztése hello közzététele profil XML fájl toospecify hello tanúsítvány adatait, mert meg arról, hogy toonote hello tanúsítványtároló-nevet, tárolja a hely és a tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="c04b6-131">Since you're manually editing hello Publish Profile XML file toospecify hello certificate information, be sure toonote hello certificate store name, store location, and certificate thumbprint.</span></span> <span data-ttu-id="c04b6-132">Ezeket az értékeket hello tanúsítványok tárába nevet, és tárolóhelyére tooprovide lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="c04b6-132">You'll need tooprovide these values for hello certificate's store name and store location.</span></span> <span data-ttu-id="c04b6-133">Lásd: [hogyan: lekérése hello tanúsítvány ujjlenyomata](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="c04b6-133">See [How to: Retrieve hello Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) for more information.</span></span>
   
   <span data-ttu-id="c04b6-134">Használhatja a hello *ClusterConnectionParameters* paraméterek toospecify hello PowerShell paraméterek toouse toohello Service Fabric-fürt kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="c04b6-134">You can use hello *ClusterConnectionParameters* parameters toospecify hello PowerShell parameters toouse when connecting toohello Service Fabric cluster.</span></span> <span data-ttu-id="c04b6-135">Érvényes paraméterei bármilyen, amely hello Connect-ServiceFabricCluster parancsmag elfogadja.</span><span class="sxs-lookup"><span data-stu-id="c04b6-135">Valid parameters are any that are accepted by hello Connect-ServiceFabricCluster cmdlet.</span></span> <span data-ttu-id="c04b6-136">Lásd: [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) felhasználható paraméterek listáját.</span><span class="sxs-lookup"><span data-stu-id="c04b6-136">See [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) for a list of available parameters.</span></span>
   
   <span data-ttu-id="c04b6-137">Ha tooa távoli fürt még közzétételt, toospecify hello megfelelő paraméterekkel kell, hogy a fürt.</span><span class="sxs-lookup"><span data-stu-id="c04b6-137">If you’re publishing tooa remote cluster, you need toospecify hello appropriate parameters for that specific cluster.</span></span> <span data-ttu-id="c04b6-138">hello következő az összekötő tooa nem biztonságos fürt:</span><span class="sxs-lookup"><span data-stu-id="c04b6-138">hello following is an example of connecting tooa non-secure cluster:</span></span>
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   <span data-ttu-id="c04b6-139">Íme egy példa a kapcsolódó tooan x509, tanúsítvány-alapú biztonságos fürt:</span><span class="sxs-lookup"><span data-stu-id="c04b6-139">Here’s an example for connecting tooan x509 certificate-based secure cluster:</span></span>
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. <span data-ttu-id="c04b6-140">Bármely más szükséges beállításokat, például a frissítési paraméterek és az alkalmazás paraméter fájl helye, szerkesztheti, és tegye közzé az alkalmazást a hello **Fabric-alkalmazás közzététele** párbeszédpanel a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="c04b6-140">Edit any other necessary settings, such as upgrade parameters and Application Parameter file location, and then publish your application from hello **Publish Service Fabric Application** dialog box in Visual Studio.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c04b6-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c04b6-141">Next steps</span></span>
<span data-ttu-id="c04b6-142">Service Fabric-fürtök elérésével kapcsolatos további információkért lásd: [a fürt megjelenítése a Service Fabric Explorer használatával](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="c04b6-142">For more information about accessing Service Fabric clusters, see [Visualizing your cluster by using Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
