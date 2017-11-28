---
title: "Azure Active Directory-fürtöt aaaHPC Pack |} Microsoft Docs"
description: "Ismerje meg, hogyan toointegrate egy HPC Pack 2016 fürtön, az Azure Active Directoryval az Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
ms.openlocfilehash: 0806e544a468e27ca0567e18c55554811584fbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a><span data-ttu-id="3efc8-103">Az Azure-ban az Azure Active Directory egy HPC Pack fürt kezelése</span><span class="sxs-lookup"><span data-stu-id="3efc8-103">Manage an HPC Pack cluster in Azure using Azure Active Directory</span></span>
<span data-ttu-id="3efc8-104">[A Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) való integráció támogatja [Azure Active Directory](../../active-directory/index.md) (az Azure AD) a rendszergazdák, akik HPC Pack-fürt üzembe helyezése az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3efc8-104">[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) supports integration with [Azure Active Directory](../../active-directory/index.md) (Azure AD) for administrators who deploy an HPC Pack cluster in Azure.</span></span>



<span data-ttu-id="3efc8-105">Hajtsa végre a következő magas szintű feladatok hello a jelen cikk hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3efc8-105">Follow hello steps in this article for hello following high level tasks:</span></span> 
* <span data-ttu-id="3efc8-106">A HPC Pack fürt manuálisan integrálása az Azure AD-bérlő</span><span class="sxs-lookup"><span data-stu-id="3efc8-106">Manually integrate your HPC Pack cluster with your Azure AD tenant</span></span>
* <span data-ttu-id="3efc8-107">Kezelése és az Azure-ban a HPC Pack fürt feladatok ütemezése</span><span class="sxs-lookup"><span data-stu-id="3efc8-107">Manage and schedule jobs in your HPC Pack cluster in Azure</span></span> 

<span data-ttu-id="3efc8-108">Standard lépéseket toointegrate egy HPC Pack fürtmegoldás integrálása az Azure AD követi, más alkalmazásokhoz és szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="3efc8-108">Integrating an HPC Pack cluster solution with Azure AD follows standard steps toointegrate other applications and services.</span></span> <span data-ttu-id="3efc8-109">Ez a cikk feltételezi, hogy jártas alapszintű felhasználói kezelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="3efc8-109">This article assumes you are familiar with basic user management in Azure AD.</span></span> <span data-ttu-id="3efc8-110">További információk és háttér: hello [Azure Active Directory dokumentációjának](../../active-directory/index.md) és hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="3efc8-110">For more information and background, see hello [Azure Active Directory documentation](../../active-directory/index.md) and hello following section.</span></span>

## <a name="benefits-of-integration"></a><span data-ttu-id="3efc8-111">Integráció előnyei</span><span class="sxs-lookup"><span data-stu-id="3efc8-111">Benefits of integration</span></span>


<span data-ttu-id="3efc8-112">Azure Active Directory (Azure AD) egy olyan több-bérlős felhőalapú címtár- és identitáskezelési felügyeleti szolgáltatás, amely az egyszeri bejelentkezés (SSO) hozzáférést biztosít a toocloud megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="3efc8-112">Azure Active Directory (Azure AD) is a multi-tenant cloud-based directory and identity management service that provides single sign-on (SSO) access toocloud solutions.</span></span>

<span data-ttu-id="3efc8-113">Integráció az Azure ad-val HPC Pack fürt segítségére lehetnek hello a következő célok érdekében:</span><span class="sxs-lookup"><span data-stu-id="3efc8-113">Integration of an HPC Pack cluster with Azure AD can help you achieve hello following goals:</span></span>

* <span data-ttu-id="3efc8-114">Hello hagyományos Active Directory-tartományvezérlőhöz eltávolítása hello HPC Pack fürtről.</span><span class="sxs-lookup"><span data-stu-id="3efc8-114">Remove hello traditional Active Directory domain controller from hello HPC Pack cluster.</span></span> <span data-ttu-id="3efc8-115">Ez csökkentheti a hello fürt karbantartásának, ha ez nem szükséges a vállalatba, illetve számlázhasson hello telepítési folyamatának hello költségeket.</span><span class="sxs-lookup"><span data-stu-id="3efc8-115">This can help reduce hello costs of maintaining hello cluster if this is not necessary for your business, and speed-up hello deployment process.</span></span>
* <span data-ttu-id="3efc8-116">Kihasználhatja a következő előnyöket a Azure ad kerülnek hello:</span><span class="sxs-lookup"><span data-stu-id="3efc8-116">Leverage hello following benefits that are brought by Azure AD:</span></span>
    *   <span data-ttu-id="3efc8-117">Egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3efc8-117">Single sign-on</span></span> 
    *   <span data-ttu-id="3efc8-118">A helyi AD-azonosítójának használatával hello HPC Pack fürthöz az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3efc8-118">Using a local AD identity for hello HPC Pack cluster in Azure</span></span> 

    ![Az Azure Active Directory-környezet](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a><span data-ttu-id="3efc8-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3efc8-120">Prerequisites</span></span>
* <span data-ttu-id="3efc8-121">**Az Azure virtuális gépekre telepített HPC Pack 2016 fürt** - lépéseiért lásd: [HPC Pack 2016-fürt üzembe helyezése az Azure-ban](hpcpack-2016-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="3efc8-121">**HPC Pack 2016 cluster deployed in Azure virtual machines** - For steps, see [Deploy an HPC Pack 2016 cluster in Azure](hpcpack-2016-cluster.md).</span></span> <span data-ttu-id="3efc8-122">Hello átjárócsomópont hello DNS-nevét és hello hitelesítő adatait. Ez a cikk hello végrehajtásához a fürt rendszergazdája van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3efc8-122">You need hello DNS name of hello head node and hello credentials of a cluster administrator to complete hello steps in this article.</span></span>

  > [!NOTE]
  > <span data-ttu-id="3efc8-123">Az Azure Active Directory-integráció HPC Pack HPC Pack 2016 előtti verzióiban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="3efc8-123">Azure Active Directory integration is not supported in versions of HPC Pack before HPC Pack 2016.</span></span>



* <span data-ttu-id="3efc8-124">**Ügyfélszámítógép** -olyan Windows vagy Windows Server ügyfél túl futtatása HPC Pack ügyfél segédprogramok van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3efc8-124">**Client computer** - You need a Windows or Windows Server client computer too run HPC Pack client utilities.</span></span> <span data-ttu-id="3efc8-125">Ha csak szeretné toouse hello HPC Pack webes portál vagy a REST API toosubmit feladatokat, az Ön által választott bármely ügyfélszámítógép is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3efc8-125">If you only want toouse hello HPC Pack web portal or REST API toosubmit jobs, you can use any client computer of your choice.</span></span>

* <span data-ttu-id="3efc8-126">**HPC Pack ügyfél segédprogramok** -hello hello szabad telepítési csomag elérhető a Microsoft Download Center hello használó ügyfélszámítógép hello HPC Pack ügyfél segédprogramokat telepíthet.</span><span class="sxs-lookup"><span data-stu-id="3efc8-126">**HPC Pack client utilities** - Install hello HPC Pack client utilities on hello client computer, using hello free installation package available from hello Microsoft Download Center.</span></span>


## <a name="step-1-register-hello-hpc-cluster-server-with-your-azure-ad-tenant"></a><span data-ttu-id="3efc8-127">1. lépés: Regisztrálja hello HPC-fürt kiszolgálót az Azure AD-bérlő</span><span class="sxs-lookup"><span data-stu-id="3efc8-127">Step 1: Register hello HPC cluster server with your Azure AD tenant</span></span>
1. <span data-ttu-id="3efc8-128">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="3efc8-128">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="3efc8-129">Kattintson a **Active Directory** a bal oldali menü hello, és kattintson a kívánt címtár hello az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="3efc8-129">Click **Active Directory** in hello left menu, and then click hello desired directory in your subscription.</span></span> <span data-ttu-id="3efc8-130">Rendelkeznie kell engedéllyel tooaccess erőforrások hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="3efc8-130">You must have permission tooaccess resources in hello directory.</span></span>
3. <span data-ttu-id="3efc8-131">Kattintson a **felhasználók**, és győződjön meg arról, hogy a felhasználói fiókok már létrehozott vagy konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="3efc8-131">Click **Users**, and make sure there are user accounts already created or configured.</span></span>
4. <span data-ttu-id="3efc8-132">Kattintson a **alkalmazások** > **Hozzáadás**, és kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-132">Click **Applications** > **Add**, and then click **Add an application my organization is developing**.</span></span> <span data-ttu-id="3efc8-133">Adja meg a következő információ hello varázslóban hello:</span><span class="sxs-lookup"><span data-stu-id="3efc8-133">Enter hello following information in hello wizard:</span></span>
    * <span data-ttu-id="3efc8-134">**Név** -HPCPackClusterServer</span><span class="sxs-lookup"><span data-stu-id="3efc8-134">**Name** - HPCPackClusterServer</span></span>
    * <span data-ttu-id="3efc8-135">**Típus** – Itt adhatja meg **webes alkalmazáshoz és/vagy webes API-t**</span><span class="sxs-lookup"><span data-stu-id="3efc8-135">**Type** - Select **Web Application and/or Web API**</span></span>
    * <span data-ttu-id="3efc8-136">**Bejelentkezés URL-cím**– hello hello mintát, amely alapértelmezés szerint az alap URL`https://hpcserver`</span><span class="sxs-lookup"><span data-stu-id="3efc8-136">**Sign-on URL**- hello base URL for hello sample, which is by default `https://hpcserver`</span></span>
    * <span data-ttu-id="3efc8-137">**App ID URI** - `https://<Directory_name>/<application_name>`.</span><span class="sxs-lookup"><span data-stu-id="3efc8-137">**App ID URI** - `https://<Directory_name>/<application_name>`.</span></span> <span data-ttu-id="3efc8-138">Cserélje le `<Directory_name`> az Azure AD-bérlőn, például a teljes név hello `hpclocal.onmicrosoft.com`, és cserélje le `<application_name>` hello névvel, akkor a korábban kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="3efc8-138">Replace `<Directory_name`> with hello full name of your Azure AD tenant, for example, `hpclocal.onmicrosoft.com`, and replace `<application_name>` with hello name you chose previously.</span></span>

5. <span data-ttu-id="3efc8-139">Hello alkalmazás hozzáadása után kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-139">After hello app is added, click **Configure**.</span></span> <span data-ttu-id="3efc8-140">A következő tulajdonságok hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="3efc8-140">Configure hello following properties:</span></span>
    * <span data-ttu-id="3efc8-141">Válassza ki **Igen** a **alkalmazás több-bérlős**</span><span class="sxs-lookup"><span data-stu-id="3efc8-141">Select **Yes** for **Application is multi-tenant**</span></span>
    * <span data-ttu-id="3efc8-142">Válassza ki **Igen** a **felhasználói hozzárendelés szükséges tooaccess app**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-142">Select **Yes** for **User assignment required tooaccess app**.</span></span>

6. <span data-ttu-id="3efc8-143">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3efc8-143">Click **Save**.</span></span> <span data-ttu-id="3efc8-144">Mentés befejeztével kattintson **kezelése Manifest**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-144">When saving completes, click **Manage Manifest**.</span></span> <span data-ttu-id="3efc8-145">Ez a művelet az alkalmazás JavaScript object notation (JSON) jegyzékfájl tölti le.</span><span class="sxs-lookup"><span data-stu-id="3efc8-145">This action downloads your application’s manifest JavaScript object notation (JSON) file.</span></span> <span data-ttu-id="3efc8-146">Szerkesztés letöltött hello jegyzékfájl hello megkeresésével `appRoles` beállítása és alkalmazás-szerepkör a következő hello felvétele:</span><span class="sxs-lookup"><span data-stu-id="3efc8-146">Edit hello downloaded manifest by locating hello `appRoles` setting and adding hello following application role:</span></span>
    ```json
    "appRoles": [
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "displayName": "HpcAdminMirror",
        "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "description": "HpcAdminMirror",
        "value": "HpcAdminMirror"
        },
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "description": "HpcUsers",
        "displayName": "HpcUsers",
        "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "value": "HpcUsers"
        }
    ],
    ```
7. <span data-ttu-id="3efc8-147">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="3efc8-147">Save hello file.</span></span> <span data-ttu-id="3efc8-148">Hello portálon, kattintson a **kezelése Manifest** > **feltöltése Manifest**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-148">Then in hello portal, click **Manage Manifest** > **Upload Manifest**.</span></span> <span data-ttu-id="3efc8-149">Majd feltöltheti hello szerkesztett jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="3efc8-149">You can then upload hello edited manifest.</span></span>
8. <span data-ttu-id="3efc8-150">Kattintson a **felhasználók**, válasszon ki egy felhasználót, és kattintson a **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-150">Click **Users**, select a user, and then click **Assign**.</span></span> <span data-ttu-id="3efc8-151">Hello elérhető szerepkörök (HpcUsers vagy HpcAdminMirror) toohello felhasználó hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="3efc8-151">Assign one of hello available roles (HpcUsers or HpcAdminMirror) toohello user.</span></span> <span data-ttu-id="3efc8-152">Ismételje meg ezt a lépést hello directory további felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="3efc8-152">Repeat this step with additional users in hello directory.</span></span> <span data-ttu-id="3efc8-153">Háttér-információkat fürt felhasználók, lásd: [fürt felhasználók kezelése](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="3efc8-153">For background information about cluster users, see [Managing Cluster Users](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).</span></span>

   > [!NOTE] 
   > <span data-ttu-id="3efc8-154">toomanage felhasználók, azt javasoljuk, hello Azure Active Directory előnézeti panelen a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3efc8-154">toomanage users, we recommend using hello Azure Active Directory preview blade in hello [Azure portal](https://portal.azure.com).</span></span>
   >


## <a name="step-2-register-hello-hpc-cluster-client-with-your-azure-ad-tenant"></a><span data-ttu-id="3efc8-155">2. lépés: Az Azure AD-bérlő hello HPC-fürt ügyfél regisztrálása</span><span class="sxs-lookup"><span data-stu-id="3efc8-155">Step 2: Register hello HPC cluster client with your Azure AD tenant</span></span>

1. <span data-ttu-id="3efc8-156">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="3efc8-156">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="3efc8-157">Kattintson a **Active Directory** a bal oldali menü hello, és kattintson a kívánt címtár hello az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="3efc8-157">Click **Active Directory** in hello left menu, and then click hello desired directory in your subscription.</span></span> <span data-ttu-id="3efc8-158">Rendelkeznie kell engedéllyel tooaccess erőforrások hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="3efc8-158">You must have permission tooaccess resources in hello directory.</span></span>
3. <span data-ttu-id="3efc8-159">Kattintson a **alkalmazások** > **Hozzáadás**, és kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-159">Click **Applications** > **Add**, and then click **Add an application my organization is developing**.</span></span> <span data-ttu-id="3efc8-160">Adja meg a következő információ hello varázslóban hello:</span><span class="sxs-lookup"><span data-stu-id="3efc8-160">Enter hello following information in hello wizard:</span></span>

    * <span data-ttu-id="3efc8-161">**Név** -HPCPackClusterClient</span><span class="sxs-lookup"><span data-stu-id="3efc8-161">**Name** - HPCPackClusterClient</span></span>
    * <span data-ttu-id="3efc8-162">**Típus** – Itt adhatja meg **natív ügyfélalkalmazás**</span><span class="sxs-lookup"><span data-stu-id="3efc8-162">**Type** - Select **Native Client Application**</span></span>
    * <span data-ttu-id="3efc8-163">**Átirányítási URI** - `http://hpcclient`</span><span class="sxs-lookup"><span data-stu-id="3efc8-163">**Redirect URI** - `http://hpcclient`</span></span>

4. <span data-ttu-id="3efc8-164">Hello alkalmazás hozzáadása után kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-164">After hello app is added, click **Configure**.</span></span> <span data-ttu-id="3efc8-165">Másolás hello **ügyfél-azonosító** értékét, és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="3efc8-165">Copy hello **Client ID** value and save it.</span></span> <span data-ttu-id="3efc8-166">Később szüksége az alkalmazás konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="3efc8-166">You need this later when configuring your application.</span></span>

5. <span data-ttu-id="3efc8-167">A **engedélyek tooother alkalmazások**, kattintson a **alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-167">In **Permissions tooother applications**, click **Add Application**.</span></span> <span data-ttu-id="3efc8-168">Keressen, és adja hozzá a hello HpcPackClusterServer alkalmazás (1. lépésben létrehozott).</span><span class="sxs-lookup"><span data-stu-id="3efc8-168">Search and add hello  HpcPackClusterServer application (created in Step 1).</span></span>

6. <span data-ttu-id="3efc8-169">A hello **delegált engedélyek** legördülő menüből válassza **hozzáférés HpcClusterServer**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-169">In hello **Delegated Permissions** dropdown, select **Access HpcClusterServer**.</span></span> <span data-ttu-id="3efc8-170">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3efc8-170">Then click **Save**.</span></span>


## <a name="step-3-configure-hello-hpc-cluster"></a><span data-ttu-id="3efc8-171">3. lépés: Hello HPC-fürt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3efc8-171">Step 3: Configure hello HPC cluster</span></span>

1. <span data-ttu-id="3efc8-172">Csatlakoztassa az átjárócsomópont toohello HPC Pack 2016 az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3efc8-172">Connect toohello HPC Pack 2016 head node in Azure.</span></span>

2. <span data-ttu-id="3efc8-173">Indítsa el a HPC PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3efc8-173">Start HPC PowerShell.</span></span>

3. <span data-ttu-id="3efc8-174">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3efc8-174">Run hello following command:</span></span>

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    <span data-ttu-id="3efc8-175">Ha</span><span class="sxs-lookup"><span data-stu-id="3efc8-175">where</span></span>

    * <span data-ttu-id="3efc8-176">`AADTenant`Adja meg például a hello Azure AD bérlő neve`hpclocal.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="3efc8-176">`AADTenant` specifies hello Azure AD tenant name, such as `hpclocal.onmicrosoft.com`</span></span>
    * <span data-ttu-id="3efc8-177">`AADClientAppId`Adja meg a 2. lépésben létrehozott hello alkalmazás hello ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="3efc8-177">`AADClientAppId` specifies hello client ID for hello app created in Step 2.</span></span>

4. <span data-ttu-id="3efc8-178">Indítsa újra a hello HpcSchedulerStateful szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3efc8-178">Restart hello HpcSchedulerStateful service.</span></span>

    <span data-ttu-id="3efc8-179">Több központi csomóponttal rendelkező fürt esetén a következő PowerShell-parancsok hello átjárócsomópont tooswitch hello elsődleges replikán hello HpcSchedulerStateful szolgáltatás hello futtathatja:</span><span class="sxs-lookup"><span data-stu-id="3efc8-179">In a cluster with multiple head nodes, you can run hello following PowerShell commands on hello head node tooswitch hello primary replica for hello HpcSchedulerStateful service:</span></span>

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-hello-client"></a><span data-ttu-id="3efc8-180">4. lépés: Kezeléséhez, és küldje el a feladatok hello ügyfélről</span><span class="sxs-lookup"><span data-stu-id="3efc8-180">Step 4: Manage and submit jobs from hello client</span></span>

<span data-ttu-id="3efc8-181">tooinstall hello HPC Pack ügyfél segédprogramok a számítógépre letölthető Microsoft Download Center hello a HPC Pack 2016-telepítőfájlok (teljes telepítés).</span><span class="sxs-lookup"><span data-stu-id="3efc8-181">tooinstall hello HPC Pack client utilities on your computer, download the HPC Pack 2016 setup files (full installation) from hello Microsoft Download Center.</span></span> <span data-ttu-id="3efc8-182">Ha hello a telepítés megkezdéséhez válassza hello telepítési beállítással a hello **HPC Pack ügyfél segédprogramok**.</span><span class="sxs-lookup"><span data-stu-id="3efc8-182">When you begin hello installation, choose hello setup option for hello **HPC Pack client utilities**.</span></span>

<span data-ttu-id="3efc8-183">tooprepare hello ügyfélszámítógép telepítése során használt hello tanúsítvány [HPC-fürttelepítés](hpcpack-2016-cluster.md) hello ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="3efc8-183">tooprepare hello client computer, install hello certificate used during [HPC cluster setup](hpcpack-2016-cluster.md) on hello client computer.</span></span> <span data-ttu-id="3efc8-184">A szabványos Windows tanúsítvány felügyeleti eljárások tooinstall hello nyilvános tanúsítvány toohello **tanúsítványok – aktuális felhasználó** > **megbízható legfelső szintű hitelesítésszolgáltatók** tárolja.</span><span class="sxs-lookup"><span data-stu-id="3efc8-184">Use standard Windows certificate management procedures tooinstall hello public certificate toohello **Certificates – Current user** > **Trusted Root Certification Authorities** store.</span></span> 

<span data-ttu-id="3efc8-185">Mostantól hello HPC Pack parancsok futtatásához vagy hello HPC Pack feladat grafikus felhasználói felület toosubmit használja és fürt-feladatok kezelése hello Azure AD-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="3efc8-185">You can now run hello HPC Pack commands or use hello HPC Pack Job manager GUI toosubmit and manage cluster jobs by using hello Azure AD account.</span></span> <span data-ttu-id="3efc8-186">További feladatok küldésének beállításai: [nyújt HPC feladatok tooan HPC Pack fürtön, az Azure-ban](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="3efc8-186">For job submission options, see [Submit HPC jobs tooan HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).</span></span>

> [!NOTE]
> <span data-ttu-id="3efc8-187">Tooconnect toohello HPC Pack fürtöt az Azure-ban a hello első alkalommal meg, megjelenik egy felugró ablakokat.</span><span class="sxs-lookup"><span data-stu-id="3efc8-187">When you try tooconnect toohello HPC Pack cluster in Azure for hello first time, a popup windows appears.</span></span> <span data-ttu-id="3efc8-188">Adja meg az Azure AD hitelesítő adatait toolog a.</span><span class="sxs-lookup"><span data-stu-id="3efc8-188">Enter your Azure AD credentials toolog in.</span></span> <span data-ttu-id="3efc8-189">hello token gyorsítótárába.</span><span class="sxs-lookup"><span data-stu-id="3efc8-189">hello token is then cached.</span></span> <span data-ttu-id="3efc8-190">Az Azure használatát hello későbbi kapcsolatok toohello fürt token gyorsítótárazva, kivéve, ha a hitelesítés módosításai vagy gyorsítótárazott hello nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="3efc8-190">Later connections toohello cluster in Azure use hello cached token unless authentication changes or hello cached is cleared.</span></span>
>
  
<span data-ttu-id="3efc8-191">Például hello előző lépések végrehajtását követően alapján is kereshet feladatokat a helyi ügyfélről az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3efc8-191">For example, after completing hello previous steps, you can query for jobs from an on-premises client as follows:</span></span>

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a><span data-ttu-id="3efc8-192">Az Azure AD-integrációs feladat elküldése hasznos parancsmagjai</span><span class="sxs-lookup"><span data-stu-id="3efc8-192">Useful cmdlets for job submission with Azure AD integration</span></span> 

### <a name="manage-hello-local-token-cache"></a><span data-ttu-id="3efc8-193">Hello helyi jogkivonat gyorsítótára kezelése</span><span class="sxs-lookup"><span data-stu-id="3efc8-193">Manage hello local token cache</span></span>

<span data-ttu-id="3efc8-194">HPC Pack 2016 toomanage hello helyi jogkivonat gyorsítótára két új HPC PowerShell-parancsmagokat kínál.</span><span class="sxs-lookup"><span data-stu-id="3efc8-194">HPC Pack 2016 provides two new HPC PowerShell cmdlets toomanage hello local token cache.</span></span> <span data-ttu-id="3efc8-195">Ezek a parancsmagok hasznosak feladatok nem interaktív elküldése.</span><span class="sxs-lookup"><span data-stu-id="3efc8-195">These cmdlets are useful for submitting jobs non-interactively.</span></span> <span data-ttu-id="3efc8-196">Tekintse meg a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="3efc8-196">See hello following example:</span></span>

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-hello-credentials-for-submitting-jobs-using-hello-azure-ad-account"></a><span data-ttu-id="3efc8-197">Hello Azure AD-fiókot használó feladatok elküldésekor hello hitelesítő adatok beállítására</span><span class="sxs-lookup"><span data-stu-id="3efc8-197">Set hello credentials for submitting jobs using hello Azure AD account</span></span> 

<span data-ttu-id="3efc8-198">Egyes esetekben érdemes lehet toorun hello feladat hello HPC-fürt felhasználói (a tartományhoz csatlakoztatott HPC-fürtre, egy tartományi felhasználói futtató; a nem tartományhoz csatlakoztatott HPC-fürtre, futtassa egy helyi felhasználói hello átjárócsomópont).</span><span class="sxs-lookup"><span data-stu-id="3efc8-198">Sometimes, you may want toorun hello job under hello HPC cluster user (for a domain-joined HPC cluster, run as one domain user; for a non-domain-joined HPC cluster, run as one local user on hello head node).</span></span>

1. <span data-ttu-id="3efc8-199">A következő parancsok tooset hello hitelesítő adatok hello használata:</span><span class="sxs-lookup"><span data-stu-id="3efc8-199">Use hello following commands tooset hello credentials:</span></span>

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. <span data-ttu-id="3efc8-200">Majd elküldeni az alábbiak szerint hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="3efc8-200">Then submit hello job as follows.</span></span> <span data-ttu-id="3efc8-201">hello/feladat fut a $localUser hello számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="3efc8-201">hello job/task runs under $localUser on hello compute nodes.</span></span>

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   <span data-ttu-id="3efc8-202">Ha `–Credential` nincs megadva `Submit-HpcJob`, hello feladat vagy tevékenység fut egy helyi csatlakoztatott felhasználói módon hello Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="3efc8-202">If `–Credential` is not specified with `Submit-HpcJob`, hello job or task runs under a local mapped user as hello Azure AD account.</span></span> <span data-ttu-id="3efc8-203">(hello HPC-fürtben hoz létre egy helyi felhasználói azonos nevet, amint az Azure AD fiók toorun hello feladat hello hello.)</span><span class="sxs-lookup"><span data-stu-id="3efc8-203">(hello HPC cluster creates a local user with hello same name as hello Azure AD account toorun hello task.)</span></span>
    
3. <span data-ttu-id="3efc8-204">Határozza meg az Azure AD-fiókot hello kiterjesztett dátumát.</span><span class="sxs-lookup"><span data-stu-id="3efc8-204">Set extended data for hello Azure AD account.</span></span> <span data-ttu-id="3efc8-205">Ez akkor hasznos, ha egy MPI feladat futó Linux csomópontok hello Azure AD-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="3efc8-205">This is useful when running an MPI job on Linux nodes using hello Azure AD account.</span></span>

   * <span data-ttu-id="3efc8-206">Hello Azure AD-fiókot, maga a kibővített adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="3efc8-206">Set extended data for hello Azure AD account itself</span></span>

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * <span data-ttu-id="3efc8-207">Állítsa be a kibővített adatok és a HPC-fürt felhasználóként</span><span class="sxs-lookup"><span data-stu-id="3efc8-207">Set extended data and run as HPC cluster user</span></span>
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

