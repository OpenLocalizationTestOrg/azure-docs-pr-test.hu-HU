---
title: "Azure Service Fabric-fürt biztonságos kapcsolatok konfigurálása |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az Azure Service Fabric-fürt által támogatott biztonságos kapcsolatokat a Visual Studio használatával."
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
ms.openlocfilehash: dc07b2f38d6fd2de941ebbe99303f6e63cbf122d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a><span data-ttu-id="946e8-103">Biztonságos kapcsolat konfigurálása a Service Fabric-fürt a Visual Studio eszközből</span><span class="sxs-lookup"><span data-stu-id="946e8-103">Configure secure connections to a Service Fabric cluster from Visual Studio</span></span>
<span data-ttu-id="946e8-104">Útmutató: a Visual Studio használatával biztonságosan elérni az Azure Service Fabric-fürt beállított hozzáférés-vezérlési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="946e8-104">Learn how to use Visual Studio to securely access an Azure Service Fabric cluster with access control policies configured.</span></span>

## <a name="cluster-connection-types"></a><span data-ttu-id="946e8-105">Fürt-kapcsolat típusai</span><span class="sxs-lookup"><span data-stu-id="946e8-105">Cluster connection types</span></span>
<span data-ttu-id="946e8-106">Az Azure Service Fabric-fürt által támogatott két típusú kapcsolatok: **nem biztonságos** kapcsolatok és **tanúsítványalapú x509** kapcsolatok biztonságos.</span><span class="sxs-lookup"><span data-stu-id="946e8-106">Two types of connections are supported by the Azure Service Fabric cluster: **non-secure** connections and **x509 certificate-based** secure connections.</span></span> <span data-ttu-id="946e8-107">(A Service Fabric-fürtök helyben tárolt, **Windows** és **dSTS** hitelesítések is támogatottak.) Be kell állítania a fürt kapcsolódási típus, a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="946e8-107">(For Service Fabric clusters hosted on-premises, **Windows** and **dSTS** authentications are also supported.) You have to configure the cluster connection type when the cluster is being created.</span></span> <span data-ttu-id="946e8-108">A már létrehozott, a kapcsolat típusa nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="946e8-108">Once it's created, the connection type can’t be changed.</span></span>

<span data-ttu-id="946e8-109">A Visual Studio Service Fabric eszközök összes hitelesítéstípussal támogatása csatlakozni a közzétételt a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="946e8-109">The Visual Studio Service Fabric tools support all authentication types for connecting to a cluster for publishing.</span></span> <span data-ttu-id="946e8-110">Lásd: [a Service Fabric-fürt beállítása az Azure-portálról](service-fabric-cluster-creation-via-portal.md) biztonságos Service Fabric-fürt beállításával kapcsolatos utasításokat.</span><span class="sxs-lookup"><span data-stu-id="946e8-110">See [Setting up a Service Fabric cluster from the Azure portal](service-fabric-cluster-creation-via-portal.md) for instructions on how to set up a secure Service Fabric cluster.</span></span>

## <a name="configure-cluster-connections-in-publish-profiles"></a><span data-ttu-id="946e8-111">Fürt kapcsolatok konfigurálása a profilok közzététele</span><span class="sxs-lookup"><span data-stu-id="946e8-111">Configure cluster connections in publish profiles</span></span>
<span data-ttu-id="946e8-112">Ha közzéteszi a Service Fabric-projektet, a Visual Studio, a **Fabric-alkalmazás közzététele** párbeszédpanelen válassza ki az Azure Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="946e8-112">If you publish a Service Fabric project from Visual Studio, use the **Publish Service Fabric Application** dialog box to choose an Azure Service Fabric cluster.</span></span> <span data-ttu-id="946e8-113">A **csatlakozási végpont**, jelölje be az előfizetéshez tartozó meglévő fürt.</span><span class="sxs-lookup"><span data-stu-id="946e8-113">Under **Connection endpoint**, select an existing cluster under your subscription.</span></span>

![A ** közzététele Service Fabric Application ** párbeszédpanel a Service Fabric-kapcsolat konfigurálására szolgál.][publishdialog]

<span data-ttu-id="946e8-115">A **Fabric-alkalmazás közzététele** párbeszédpanel automatikusan ellenőrzi a fürt kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="946e8-115">The **Publish Service Fabric Application** dialog box automatically validates the cluster connection.</span></span> <span data-ttu-id="946e8-116">Ha a rendszer kéri, jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="946e8-116">If prompted, sign in to your Azure account.</span></span> <span data-ttu-id="946e8-117">Ha az ellenőrzés eredménye akkor megfelelő, az azt jelenti, hogy a rendszer rendelkezik a megfelelő tanúsítványok biztonságosan csatlakozzon a fürthöz telepítve, vagy a fürt nem biztonságos.</span><span class="sxs-lookup"><span data-stu-id="946e8-117">If validation passes, it means that your system has the correct certificates installed to connect to the cluster securely, or your cluster is non-secure.</span></span> <span data-ttu-id="946e8-118">Az érvényesítési hibák okozhatja hálózati probléma, vagy nem rendelkezik a rendszer kapcsolódik egy biztonságos fürthöz megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="946e8-118">Validation failures can be caused by network issues or by not having your system correctly configured to connect to a secure cluster.</span></span>

![A ** közzététele Service Fabric alkalmazás ** párbeszédpanel ellenőrzi, hogy egy meglévő, megfelelően konfigurálva a Service Fabric-fürt kapcsolat.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a><span data-ttu-id="946e8-120">A biztonságos fürthöz való kapcsolódás</span><span class="sxs-lookup"><span data-stu-id="946e8-120">To connect to a secure cluster</span></span>
1. <span data-ttu-id="946e8-121">Ellenőrizze, hogy a célfürt Megbízhatóságok ügyféltanúsítványok hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="946e8-121">Make sure you can access one of the client certificates that the destination cluster trusts.</span></span> <span data-ttu-id="946e8-122">A tanúsítvány általában megosztott, személyes információcsere (.pfx) fájlként.</span><span class="sxs-lookup"><span data-stu-id="946e8-122">The certificate is usually shared as a Personal Information Exchange (.pfx) file.</span></span> <span data-ttu-id="946e8-123">Lásd: [a Service Fabric-fürt beállítása az Azure-portálról](service-fabric-cluster-creation-via-portal.md) biztosítanak hozzáférést az ügyfél a kiszolgáló konfigurálásának.</span><span class="sxs-lookup"><span data-stu-id="946e8-123">See [Setting up a Service Fabric cluster from the Azure portal](service-fabric-cluster-creation-via-portal.md) for how to configure the server to grant access to a client.</span></span>
2. <span data-ttu-id="946e8-124">A megbízható tanúsítvány telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="946e8-124">Install the trusted certificate.</span></span> <span data-ttu-id="946e8-125">Ehhez kattintson duplán a .pfx-fájlra, vagy a PowerShell parancsfájl Import-PfxCertificate segítségével importálja a tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="946e8-125">To do this, double-click the .pfx file, or use the PowerShell script Import-PfxCertificate to import the certificates.</span></span> <span data-ttu-id="946e8-126">A tanúsítvány telepítése **Cert: \LocalMachine\My**.</span><span class="sxs-lookup"><span data-stu-id="946e8-126">Install the certificate to **Cert:\LocalMachine\My**.</span></span> <span data-ttu-id="946e8-127">Az OK gombra, fogadja el az összes alapértelmezett beállítást a tanúsítvány importálása során is.</span><span class="sxs-lookup"><span data-stu-id="946e8-127">It's OK to accept all default settings while importing the certificate.</span></span>
3. <span data-ttu-id="946e8-128">Válassza ki a **közzététele...**  parancs a helyi menüben a projekt megnyitása a **Azure-alkalmazás közzététele** párbeszédpanel megnyitásához, majd válassza ki a cél fürtnek.</span><span class="sxs-lookup"><span data-stu-id="946e8-128">Choose the **Publish...** command on the shortcut menu of the project to open the **Publish Azure Application** dialog box and then select the target cluster.</span></span> <span data-ttu-id="946e8-129">Az eszköz automatikusan oldja fel a kapcsolatot, és menti a közzétételi profil a biztonságos kapcsolat paramétereit.</span><span class="sxs-lookup"><span data-stu-id="946e8-129">The tool automatically resolves the connection and saves the secure connection parameters in the publish profile.</span></span>
4. <span data-ttu-id="946e8-130">Választható lehetőség: Szerkesztheti a közzétételi profilt, a fürt biztonságos kapcsolat megadásához.</span><span class="sxs-lookup"><span data-stu-id="946e8-130">Optional: You can edit the publish profile to specify a secure cluster connection.</span></span>
   
   <span data-ttu-id="946e8-131">Manuálisan szerkesztése az közzététele profil XML-fájlban adja meg a tanúsítvány adatait, jegyezze fel a tanúsítványtároló-nevet, mivel tárolja a helyét, és a tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="946e8-131">Since you're manually editing the Publish Profile XML file to specify the certificate information, be sure to note the certificate store name, store location, and certificate thumbprint.</span></span> <span data-ttu-id="946e8-132">Adja meg ezeket az értékeket a tanúsítványtároló neve, és helyezze a helyre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="946e8-132">You'll need to provide these values for the certificate's store name and store location.</span></span> <span data-ttu-id="946e8-133">Lásd: [Útmutató: a tanúsítvány ujjlenyomatának beolvasása](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="946e8-133">See [How to: Retrieve the Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) for more information.</span></span>
   
   <span data-ttu-id="946e8-134">Használhatja a *ClusterConnectionParameters* adja meg a PowerShell-paramétereket a Service Fabric-fürt történő csatlakozás során használandó paramétereket.</span><span class="sxs-lookup"><span data-stu-id="946e8-134">You can use the *ClusterConnectionParameters* parameters to specify the PowerShell parameters to use when connecting to the Service Fabric cluster.</span></span> <span data-ttu-id="946e8-135">Érvényes paraméterei bármilyen, amely elfogadja a Connect-ServiceFabricCluster parancsmag.</span><span class="sxs-lookup"><span data-stu-id="946e8-135">Valid parameters are any that are accepted by the Connect-ServiceFabricCluster cmdlet.</span></span> <span data-ttu-id="946e8-136">Lásd: [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) felhasználható paraméterek listáját.</span><span class="sxs-lookup"><span data-stu-id="946e8-136">See [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) for a list of available parameters.</span></span>
   
   <span data-ttu-id="946e8-137">Ha most közzétételt távoli fürtre, meg kell adnia a megfelelő paramétereket, hogy a fürt.</span><span class="sxs-lookup"><span data-stu-id="946e8-137">If you’re publishing to a remote cluster, you need to specify the appropriate parameters for that specific cluster.</span></span> <span data-ttu-id="946e8-138">A következő egy példa egy nem biztonságos fürthöz csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="946e8-138">The following is an example of connecting to a non-secure cluster:</span></span>
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   <span data-ttu-id="946e8-139">Íme egy példa egy x509 való kapcsolódáshoz biztonságos fürt tanúsítványalapú:</span><span class="sxs-lookup"><span data-stu-id="946e8-139">Here’s an example for connecting to an x509 certificate-based secure cluster:</span></span>
   
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
5. <span data-ttu-id="946e8-140">Bármely más szükséges beállításokat, például a frissítési paraméterek és az alkalmazás paraméter fájl helye, szerkesztheti, és tegye közzé az alkalmazást a **Fabric-alkalmazás közzététele** párbeszédpanel a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="946e8-140">Edit any other necessary settings, such as upgrade parameters and Application Parameter file location, and then publish your application from the **Publish Service Fabric Application** dialog box in Visual Studio.</span></span>

## <a name="next-steps"></a><span data-ttu-id="946e8-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="946e8-141">Next steps</span></span>
<span data-ttu-id="946e8-142">Service Fabric-fürtök elérésével kapcsolatos további információkért lásd: [a fürt megjelenítése a Service Fabric Explorer használatával](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="946e8-142">For more information about accessing Service Fabric clusters, see [Visualizing your cluster by using Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
