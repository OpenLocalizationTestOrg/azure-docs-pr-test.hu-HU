---
title: "a helyszíni adatátjáró aaaInstall |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és konfiguráljon egy a helyszíni adatok átjárót."
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
ms.date: 08/22/2017
ms.author: owend
ms.openlocfilehash: e2878bf765c82910d452ae2cdd9264a343ec1990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a><span data-ttu-id="cc38a-103">Telepítse és konfigurálja a helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="cc38a-103">Install and configure an on-premises data gateway</span></span>
<span data-ttu-id="cc38a-104">Egy a helyszíni adatok átjáróra szükség, ha az egy vagy több Azure Analysis Services-kiszolgáló azonos hello régió tooon helyszíni adatforrások csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="cc38a-104">An on-premises data gateway is required when one or more Azure Analysis Services servers in hello same region connect tooon-premises data sources.</span></span> <span data-ttu-id="cc38a-105">toolearn hello átjáró kapcsolatos további információkért lásd: [helyszíni adatátjáró](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="cc38a-105">toolearn more about hello gateway, see [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc38a-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cc38a-106">Prerequisites</span></span>
<span data-ttu-id="cc38a-107">**Minimumkövetelmények:**</span><span class="sxs-lookup"><span data-stu-id="cc38a-107">**Minimum Requirements:**</span></span>

* <span data-ttu-id="cc38a-108">.NET 4.5 keretrendszer</span><span class="sxs-lookup"><span data-stu-id="cc38a-108">.NET 4.5 Framework</span></span>
* <span data-ttu-id="cc38a-109">64 bites Windows 7 vagy Windows Server 2008 R2 (vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="cc38a-109">64-bit version of Windows 7 / Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="cc38a-110">**Ajánlott:**</span><span class="sxs-lookup"><span data-stu-id="cc38a-110">**Recommended:**</span></span>

* <span data-ttu-id="cc38a-111">8 mag Processzor</span><span class="sxs-lookup"><span data-stu-id="cc38a-111">8 Core CPU</span></span>
* <span data-ttu-id="cc38a-112">8 GB memória</span><span class="sxs-lookup"><span data-stu-id="cc38a-112">8 GB Memory</span></span>
* <span data-ttu-id="cc38a-113">64 bites Windows 2012 R2 (vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="cc38a-113">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="cc38a-114">**Fontos tudnivalók találhatók:**</span><span class="sxs-lookup"><span data-stu-id="cc38a-114">**Important considerations:**</span></span>

* <span data-ttu-id="cc38a-115">A telepítés során, ha az átjáró regisztrálása az Azure-ral hello alapértelmezett terület előfizetési van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="cc38a-115">During setup, when registering your gateway with Azure, hello default region for your subscription is selected.</span></span> <span data-ttu-id="cc38a-116">Lehetősége van egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="cc38a-116">You can choose a different region.</span></span> <span data-ttu-id="cc38a-117">Ha egynél több régióban kiszolgálóval rendelkezik, telepítenie kell egy átjáró mindegyik régióhoz.</span><span class="sxs-lookup"><span data-stu-id="cc38a-117">If you have servers in more than one region, you must install a gateway for each region.</span></span> 
* <span data-ttu-id="cc38a-118">hello átjáró nem telepíthető tartományvezérlőre.</span><span class="sxs-lookup"><span data-stu-id="cc38a-118">hello gateway cannot be installed on a domain controller.</span></span>
* <span data-ttu-id="cc38a-119">Csak egy átjáró telepíthető egy számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cc38a-119">Only one gateway can be installed on a single computer.</span></span>
* <span data-ttu-id="cc38a-120">Hello átjáró telepíthető olyan számítógépre, amely továbbra is megtalálható, és nem halad toosleep.</span><span class="sxs-lookup"><span data-stu-id="cc38a-120">Install hello gateway on a computer that remains on and does not go toosleep.</span></span>
* <span data-ttu-id="cc38a-121">Ne telepítse a számítógép vezeték nélkül csatlakozó tooyour hálózat hello átjárót.</span><span class="sxs-lookup"><span data-stu-id="cc38a-121">Do not install hello gateway on a computer wirelessly connected tooyour network.</span></span> <span data-ttu-id="cc38a-122">Teljesítmény is lehet csökken.</span><span class="sxs-lookup"><span data-stu-id="cc38a-122">Performance can be diminished.</span></span>


## <span data-ttu-id="cc38a-123"><a name="download"></a>Letöltése</span><span class="sxs-lookup"><span data-stu-id="cc38a-123"><a name="download"></a>Download</span></span>
 [<span data-ttu-id="cc38a-124">Töltse le a hello átjáró</span><span class="sxs-lookup"><span data-stu-id="cc38a-124">Download hello gateway</span></span>](https://aka.ms/azureasgateway)

## <span data-ttu-id="cc38a-125"><a name="install"></a>Telepítése</span><span class="sxs-lookup"><span data-stu-id="cc38a-125"><a name="install"></a>Install</span></span>

1. <span data-ttu-id="cc38a-126">Futtassa a telepítőt.</span><span class="sxs-lookup"><span data-stu-id="cc38a-126">Run setup.</span></span>

2. <span data-ttu-id="cc38a-127">Jelöljön ki egy helyet, hello fogadnia, és kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="cc38a-127">Select a location, accept hello terms, and then click **Install**.</span></span>

   ![Telepítse a helyét és a licencszerződés feltételeit](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. <span data-ttu-id="cc38a-129">Válassza ki **(ajánlott) a helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="cc38a-129">Select **On-premises data gateway (recommended)**.</span></span> <span data-ttu-id="cc38a-130">Az Azure Analysis Services nem támogatja a személyes módban.</span><span class="sxs-lookup"><span data-stu-id="cc38a-130">Azure Analysis Services does not support personal mode.</span></span>

   ![Válassza ki az átjáró típusa](media/analysis-services-gateway-install/aas-gateway-installer-shared.png)

4. <span data-ttu-id="cc38a-132">Adjon meg egy fiókot toosign tooAzure.</span><span class="sxs-lookup"><span data-stu-id="cc38a-132">Enter an account toosign in tooAzure.</span></span> <span data-ttu-id="cc38a-133">hello fióknak kell lennie a bérlő Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="cc38a-133">hello account must be in your tenant's Azure Active Directory.</span></span> <span data-ttu-id="cc38a-134">Ez a fiók hello átjáró rendszergazdája szolgál.</span><span class="sxs-lookup"><span data-stu-id="cc38a-134">This account is used for hello gateway administrator.</span></span> 

   ![Adjon meg egy fiókot toosign tooAzure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > <span data-ttu-id="cc38a-136">Ha egy tartományi fiókkal jelentkezik, akkor lesz leképezve tooyour szervezeti fiókkal az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="cc38a-136">If you sign in with a domain account, it will be mapped tooyour organizational account in Azure AD.</span></span> <span data-ttu-id="cc38a-137">A szervezeti fiókjával hello hello átjáró rendszergazdája lesz.</span><span class="sxs-lookup"><span data-stu-id="cc38a-137">Your organizational account will be used as hello hello gateway administrator.</span></span>

## <span data-ttu-id="cc38a-138"><a name="register"></a>Regisztráció</span><span class="sxs-lookup"><span data-stu-id="cc38a-138"><a name="register"></a>Register</span></span>
<span data-ttu-id="cc38a-139">A sorrend toocreate egy átjáró-erőforráshoz az Azure-ban regisztrálnia kell a hello helyi példány hello átjáró Felhőszolgáltatáshoz telepítette.</span><span class="sxs-lookup"><span data-stu-id="cc38a-139">In order toocreate a gateway resource in Azure, you must register hello local instance you installed with hello Gateway Cloud Service.</span></span> 

1.  <span data-ttu-id="cc38a-140">Válassza ki **ezen a számítógépen egy új átjáró regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="cc38a-140">Select **Register a new gateway on this computer**.</span></span>

    ![Regisztráljon](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. <span data-ttu-id="cc38a-142">Írja be az átjáró nevét és a helyreállítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cc38a-142">Type a name and recovery key for your gateway.</span></span> <span data-ttu-id="cc38a-143">Hello átjáró alapértelmezés szerint az előfizetés alapértelmezett régió használja.</span><span class="sxs-lookup"><span data-stu-id="cc38a-143">By default, hello gateway uses your subscription's default region.</span></span> <span data-ttu-id="cc38a-144">Ha egy másik régióban tooselect van szüksége, válassza ki a **régió módosítása**.</span><span class="sxs-lookup"><span data-stu-id="cc38a-144">If you need tooselect a different region, select **Change Region**.</span></span>

   ![Regisztráljon](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <span data-ttu-id="cc38a-146"><a name="create-resource"></a>Hozzon létre egy Azure átjáró erőforrást</span><span class="sxs-lookup"><span data-stu-id="cc38a-146"><a name="create-resource"></a>Create an Azure gateway resource</span></span>
<span data-ttu-id="cc38a-147">Miután telepíteni és regisztrálni az átjárót, az Azure-előfizetéshez toocreate egy átjáró-erőforráshoz kell.</span><span class="sxs-lookup"><span data-stu-id="cc38a-147">After you've installed and registered your gateway, you need toocreate a gateway resource in your Azure subscription.</span></span> <span data-ttu-id="cc38a-148">Jelentkezzen be a hello tooAzure azonos hello átjáró regisztrálásakor használt fiókkal.</span><span class="sxs-lookup"><span data-stu-id="cc38a-148">Sign in tooAzure with hello same account you used when registering hello gateway.</span></span>

1. <span data-ttu-id="cc38a-149">Az Azure portálon kattintson **hozzon létre egy új** > **vállalati integrációs** > **helyszíni adatátjáró** > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cc38a-149">In Azure portal, click **Create a new service** > **Enterprise Integration** > **On-premises data gateway** > **Create**.</span></span>

   ![Hozzon létre egy átjáró erőforrást](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. <span data-ttu-id="cc38a-151">A **kapcsolat átjáró létrehozása**, adja meg ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="cc38a-151">In **Create connection gateway**, enter these settings:</span></span>

    * <span data-ttu-id="cc38a-152">**Név**: Adjon meg egy nevet az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="cc38a-152">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="cc38a-153">**Előfizetés**: Válasszon hello Azure-előfizetés tooassociate az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="cc38a-153">**Subscription**: Select hello Azure subscription tooassociate with your gateway resource.</span></span> 
    <span data-ttu-id="cc38a-154">Ehhez az előfizetéshez kell hello a kiszolgálók ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="cc38a-154">This subscription should be hello same subscription your servers are in.</span></span>
   
      <span data-ttu-id="cc38a-155">hello alapértelmezett előfizetés hello Azure-fiókot, amelyet a toosign használt alapul.</span><span class="sxs-lookup"><span data-stu-id="cc38a-155">hello default subscription is based on hello Azure account that you used toosign in.</span></span>

    * <span data-ttu-id="cc38a-156">**Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy válasszon ki egy már meglévőt.</span><span class="sxs-lookup"><span data-stu-id="cc38a-156">**Resource group**: Create a resource group or select an existing resource group.</span></span>

    * <span data-ttu-id="cc38a-157">**Hely**: Select hello régió regisztrálta az átjáró található.</span><span class="sxs-lookup"><span data-stu-id="cc38a-157">**Location**: Select hello region you registered your gateway in.</span></span>

    * <span data-ttu-id="cc38a-158">**Telepítési neve**: Ha nincs kiválasztva, akkor az átjáró telepítésének, jelölje be hello átjáró regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="cc38a-158">**Installation Name**: If your gateway installation isn't already selected, select hello gateway registered.</span></span> 

    <span data-ttu-id="cc38a-159">Amikor elkészült, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cc38a-159">When you're done, click **Create**.</span></span>

## <span data-ttu-id="cc38a-160"><a name="connect-servers"></a>Csatlakoztassa az kiszolgálók toohello átjáró erőforrást</span><span class="sxs-lookup"><span data-stu-id="cc38a-160"><a name="connect-servers"></a>Connect servers toohello gateway resource</span></span>

1. <span data-ttu-id="cc38a-161">Kattintson az Azure Analysis Services-kiszolgáló áttekintése **helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="cc38a-161">In your Azure Analysis Services server overview, click **On-Premises Data Gateway**.</span></span>

   ![Csatlakozás kiszolgálóhoz toogateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. <span data-ttu-id="cc38a-163">A **válasszon egy helyszíni Data Gateway tooconnect**, jelölje be az átjáró-erőforráshoz, és kattintson a **csatlakozás a kiválasztott átjáróeszközön**.</span><span class="sxs-lookup"><span data-stu-id="cc38a-163">In **Pick an On-Premises Data Gateway tooconnect**, select your gateway resource, and then click **Connect selected gateway**.</span></span>

   ![Csatlakozás kiszolgálóhoz toogateway erőforrás](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > <span data-ttu-id="cc38a-165">Az átjáró hello lista nem jelenik meg, ha a kiszolgáló nincs valószínűleg az hello és hello átjáró regisztrálásakor megadott hello régió ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="cc38a-165">If your gateway does not appear in hello list, your server is likely not in hello same region as hello region you specified when registering hello gateway.</span></span> 

<span data-ttu-id="cc38a-166">Ennyi az egész.</span><span class="sxs-lookup"><span data-stu-id="cc38a-166">That's it.</span></span> <span data-ttu-id="cc38a-167">Ha tooopen portokat kell vagy hibaelhárításra tegye, lehet, hogy toocheck kimenő [helyszíni adatátjáró](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="cc38a-167">If you need tooopen ports or do any troubleshooting, be sure toocheck out [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc38a-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc38a-168">Next steps</span></span>
* [<span data-ttu-id="cc38a-169">Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="cc38a-169">Manage Analysis Services</span></span>](analysis-services-manage.md)   
* [<span data-ttu-id="cc38a-170">Adatok beolvasása az Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="cc38a-170">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)
