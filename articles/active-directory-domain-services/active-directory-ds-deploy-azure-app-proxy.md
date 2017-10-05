---
title: "Az Azure Active Directory tartományi szolgáltatások: Az Azure Active Directory-proxyt üzembe helyezni |} Microsoft Docs"
description: "Az Azure AD-alkalmazásproxy használata a felügyelt tartományok Azure Active Directory tartományi szolgáltatások"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: c158c67a82e12501386179e19bc75fd852d7e308
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="2b5f7-103">Központi telepítése az Azure AD-alkalmazásproxyval oldható meg az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="2b5f7-103">Deploy Azure AD Application Proxy on an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="2b5f7-104">Támogatja a távoli dolgozók által az interneten keresztül érhető el a helyszíni alkalmazások közzététele az Azure Active Directory (AD) alkalmazásproxy segítségével.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-104">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications to be accessed over the internet.</span></span> <span data-ttu-id="2b5f7-105">Az Azure AD tartományi szolgáltatásokat helyszíni Azure infrastruktúra-szolgáltatásokat futtató örökölt alkalmazások most növekedési-és-shift lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-105">With Azure AD Domain Services, you can now lift-and-shift legacy applications running on-premises to Azure Infrastructure Services.</span></span> <span data-ttu-id="2b5f7-106">Majd közzéteheti ezeket az alkalmazásokat az Azure AD alkalmazásproxy segítségével biztonságos távoli hozzáférés biztosítása a szervezetében lévő felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-106">You can then publish these applications using the Azure AD Application Proxy, to provide secure remote access to users in your organization.</span></span>

<span data-ttu-id="2b5f7-107">Ha most ismerkedik az Azure AD-alkalmazásproxy, kapcsolatos további Ez a szolgáltatás a következő cikkben: [hogyan biztosíthat biztonságos távoli hozzáférést a helyszíni alkalmazások](../active-directory/active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2b5f7-107">If you're new to the Azure AD Application Proxy, learn more about this feature with the following article: [How to provide secure remote access to on-premises applications](../active-directory/active-directory-application-proxy-get-started.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="2b5f7-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2b5f7-108">Before you begin</span></span>
<span data-ttu-id="2b5f7-109">A cikkben szereplő feladatok elvégzéséhez szüksége:</span><span class="sxs-lookup"><span data-stu-id="2b5f7-109">To perform the tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="2b5f7-110">Egy érvényes **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-110">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="2b5f7-111">Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-111">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="2b5f7-112">Egy **Azure AD alapszintű vagy Premium licenc** az Azure AD-alkalmazásproxy használatához szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-112">An **Azure AD Basic or Premium license** is required to use the Azure AD Application Proxy.</span></span>
4. <span data-ttu-id="2b5f7-113">**Azure AD tartományi szolgáltatások** az Azure AD-címtár engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-113">**Azure AD Domain Services** must be enabled for the Azure AD directory.</span></span> <span data-ttu-id="2b5f7-114">Ha még nem tette meg, az összes ismertetett feladatok végrehajtásával a [első lépések útmutató](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="2b5f7-114">If you haven't done so, follow all the tasks outlined in the [Getting Started guide](active-directory-ds-getting-started.md).</span></span>

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a><span data-ttu-id="2b5f7-115">1 - engedélyezése az Azure AD alkalmazásproxy feladatot az Azure AD-címtár</span><span class="sxs-lookup"><span data-stu-id="2b5f7-115">Task 1 - Enable Azure AD Application Proxy for your Azure AD directory</span></span>
<span data-ttu-id="2b5f7-116">Hajtsa végre az alábbi lépéseket az Azure AD-címtár Azure AD alkalmazásproxy engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-116">Perform the following steps to enable the Azure AD Application Proxy for your Azure AD directory.</span></span>

1. <span data-ttu-id="2b5f7-117">Jelentkezzen be rendszergazdaként a a [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2b5f7-117">Sign in as an administrator in the [Azure portal](http://portal.azure.com).</span></span>

2. <span data-ttu-id="2b5f7-118">Kattintson a **Azure Active Directory** elindítani a directory áttekintése.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-118">Click **Azure Active Directory** to bring up the directory overview.</span></span> <span data-ttu-id="2b5f7-119">Kattintson a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-119">Click **Enterprise applications**.</span></span>

    ![Válassza ki az Azure AD-címtár](./media/app-proxy/app-proxy-enable-start.png)
3. <span data-ttu-id="2b5f7-121">Kattintson a **alkalmazásproxy**.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-121">Click **Application proxy**.</span></span> <span data-ttu-id="2b5f7-122">Ha nem rendelkezik az Azure AD alapszintű vagy Azure AD Premium előfizetéssel, lásd: a próbaverziójának engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-122">If you do not have an Azure AD Basic or Azure AD Premium subscription, you see an option to enable a trial.</span></span> <span data-ttu-id="2b5f7-123">Váltás **alkalmazásproxy engedélyezése?** való **engedélyezése** kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-123">Toggle **Enable Application Proxy?** to **Enable** and click **Save**.</span></span>

    ![Alkalmazás Proxy engedélyezése](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. <span data-ttu-id="2b5f7-125">Az összekötő letöltéséhez kattintson a **összekötő** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-125">To download the connector, click the **Connector** button.</span></span>

    ![Összekötő letöltése](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. <span data-ttu-id="2b5f7-127">A letöltési oldalon fogadja el a licencfeltételeket és az adatvédelmi szerződést, majd kattintson a **letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-127">On the download page, accept the license terms and privacy agreement and click the **Download** button.</span></span>

    ![Erősítse meg a letöltési](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-to-deploy-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="2b5f7-129">2. feladat – kiépíteni a tartományhoz csatlakoztatott Windows Server rendszerű kiszolgálók az Azure AD alkalmazásproxy-összekötő üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="2b5f7-129">Task 2 - Provision domain-joined Windows servers to deploy the Azure AD Application Proxy connector</span></span>
<span data-ttu-id="2b5f7-130">A tartományhoz csatlakoztatott Windows Server virtuális gépek is telepíthető, amely az Azure AD alkalmazásproxy-összekötő van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-130">You need domain-joined Windows Server virtual machines on which you can install the Azure AD Application Proxy connector.</span></span> <span data-ttu-id="2b5f7-131">A közzétett alkalmazások, attól függően választhatja, hogy több kiszolgáló, amelyen telepítve van az összekötő telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-131">Depending on the applications being published, you may choose to provision multiple servers on which the connector is installed.</span></span> <span data-ttu-id="2b5f7-132">Központi telepítési lehetőséget ad nagyobb rendelkezésre állása, és segít nehezebb hitelesítési terhelés kezelésére.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-132">This deployment option gives you greater availability and helps handle heavier authentication loads.</span></span>

<span data-ttu-id="2b5f7-133">Az összekötő-kiszolgálókon ugyanazt a virtuális hálózatot (vagy a csatlakoztatott/társviszonyban virtuális hálózaton), amelyben engedélyezte az Azure AD tartományi szolgáltatások által kezelt tartomány kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-133">Provision the connector servers on the same virtual network (or a connected/peered virtual network), in which you have enabled your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="2b5f7-134">Ehhez hasonlóan az alkalmazások közzététele az alkalmazásproxy keresztül üzemeltető kiszolgálók telepítve kell lennie az Azure virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-134">Similarly, the servers hosting the applications you publish via the Application Proxy need to be installed on the same Azure virtual network.</span></span>

<span data-ttu-id="2b5f7-135">Összekötő kiszolgálók ellátását a című cikkben ismertetett feladatok végrehajtásával [egy Windows rendszerű virtuális gép csatlakoztatása felügyelt tartományhoz](active-directory-ds-admin-guide-join-windows-vm.md).</span><span class="sxs-lookup"><span data-stu-id="2b5f7-135">To provision connector servers, follow the tasks outlined in the article titled [Join a Windows virtual machine to a managed domain](active-directory-ds-admin-guide-join-windows-vm.md).</span></span>


## <a name="task-3---install-and-register-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="2b5f7-136">3. feladat – az Azure AD alkalmazásproxy-összekötő regisztrálása és telepítése</span><span class="sxs-lookup"><span data-stu-id="2b5f7-136">Task 3 - Install and register the Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="2b5f7-137">Korábban a Windows Server virtuális gép kiépítése, és felügyelt tartományhoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-137">Previously, you provisioned a Windows Server virtual machine and joined it to the managed domain.</span></span> <span data-ttu-id="2b5f7-138">Ebben a feladatban telepíti az Azure AD alkalmazásproxy-összekötő a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-138">In this task, you will install the Azure AD Application Proxy connector on this virtual machine.</span></span>

1. <span data-ttu-id="2b5f7-139">Az összekötő-telepítési csomag másolása a virtuális gép, amelyre telepíti az Azure AD-webalkalmazás-Proxy connector.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-139">Copy the connector installation package to the VM on which you install the Azure AD Web Application Proxy connector.</span></span>

2. <span data-ttu-id="2b5f7-140">Futtatás **AADApplicationProxyConnectorInstaller.exe** a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-140">Run **AADApplicationProxyConnectorInstaller.exe** on the virtual machine.</span></span> <span data-ttu-id="2b5f7-141">Fogadja el a szoftverlicenc-szerződést.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-141">Accept the software license terms.</span></span>

    ![A telepítés feltételek elfogadása](./media/app-proxy/app-proxy-install-connector-terms.png)
3. <span data-ttu-id="2b5f7-143">A telepítés során kéri az összekötő regisztrálására az Azure Active directory Alkalmazásproxyjával.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-143">During installation, you are prompted to register the connector with the Application Proxy of your Azure AD directory.</span></span>
    * <span data-ttu-id="2b5f7-144">Adja meg a **az Azure AD globális rendszergazda hitelesítő adatait**.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-144">Provide your **Azure AD global administrator credentials**.</span></span> <span data-ttu-id="2b5f7-145">A globális rendszergazdai bérlő adatai eltérhetnek a Microsoft Azure rendszerre vonatkozó hitelesítő adatoktól.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-145">Your global administrator tenant may be different from your Microsoft Azure credentials.</span></span>
    * <span data-ttu-id="2b5f7-146">A rendszergazdai fiók segítségével regisztrálhat az összekötő ugyanabban a könyvtárban, ahol az alkalmazásproxy engedélyezte kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-146">The administrator account used to register the connector must belong to the same directory where you enabled the Application Proxy service.</span></span> <span data-ttu-id="2b5f7-147">Például, ha a bérlő tartománya a contoso.com, a rendszergazdának kell lennie admin@contoso.com vagy más érvényes alias abban a tartományban.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-147">For example, if the tenant domain is contoso.com, the admin should be admin@contoso.com or any other valid alias on that domain.</span></span>
    * <span data-ttu-id="2b5f7-148">Ha az Internet Explorer fokozott biztonsági beállítások be van kapcsolva a kiszolgáló az összekötőt telepíti, a regisztrációs képernyő blokkolhatja.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-148">If IE Enhanced Security Configuration is turned on for the server where you are installing the connector, the registration screen might be blocked.</span></span> <span data-ttu-id="2b5f7-149">Engedélyezi a hozzáférést, kövesse az utasításokat a hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-149">To allow access, follow the instructions in the error message.</span></span> <span data-ttu-id="2b5f7-150">Győződjön meg arról, hogy az Internet Explorer fokozott biztonsági beállításai ki vannak kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-150">Make sure that Internet Explorer Enhanced Security is off.</span></span>
    * <span data-ttu-id="2b5f7-151">Ha az összekötő regisztrálása meghiúsul, tekintse meg a [Troubleshoot Application Proxy](../active-directory/active-directory-application-proxy-troubleshoot.md) (Alkalmazásproxy hibaelhárítása) című anyagot.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-151">If connector registration does not succeed, see [Troubleshoot Application Proxy](../active-directory/active-directory-application-proxy-troubleshoot.md).</span></span>

    ![Összekötő telepítve](./media/app-proxy/app-proxy-connector-installed.png)
4. <span data-ttu-id="2b5f7-153">Az összekötő biztosításához működik megfelelően, futtassa az Azure AD Application Proxy Connector hibaelhárító.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-153">To ensure the connector works properly, run the Azure AD Application Proxy Connector Troubleshooter.</span></span> <span data-ttu-id="2b5f7-154">Egy sikeres jelentésben a hibaelhárító futtatása után kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-154">You should see a successful report after running the troubleshooter.</span></span>

    ![Hibaelhárító sikeres](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. <span data-ttu-id="2b5f7-156">Az újonnan telepített összekötő az Azure AD-címtár Application proxy oldalon felsorolt kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-156">You should see the newly installed connector listed on the Application proxy page in your Azure AD directory.</span></span>

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> <span data-ttu-id="2b5f7-157">Összekötők telepítése több kiszolgálón hitelesítéséhez az Azure AD-alkalmazásproxy keresztül közzétett alkalmazás magas rendelkezésre állás biztosításához választhatja.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-157">You may choose to install connectors on multiple servers to guarantee high availability for authenticating applications published through the Azure AD Application Proxy.</span></span> <span data-ttu-id="2b5f7-158">Hajtsa végre az összekötő telepítéséhez más kiszolgálókon a felügyelt tartományhoz fent ismertetett lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-158">Perform the same steps listed above to install the connector on other servers joined to your managed domain.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="2b5f7-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b5f7-159">Next Steps</span></span>
<span data-ttu-id="2b5f7-160">Állítsa be az Azure AD-alkalmazásproxy, és azt az Azure AD tartományi szolgáltatások által kezelt tartomány integrálva.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-160">You have set up the Azure AD Application Proxy and integrated it with your Azure AD Domain Services managed domain.</span></span>

* <span data-ttu-id="2b5f7-161">**Az alkalmazások az Azure virtuális gépek áttelepítése:** növekedési-és-shift a felügyelt tartományra csatlakozott az alkalmazásait, a helyszíni kiszolgálók és az Azure virtuális gépek is.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-161">**Migrate your applications to Azure virtual machines:** You can lift-and-shift your applications from on-premises servers to Azure virtual machines joined to your managed domain.</span></span> <span data-ttu-id="2b5f7-162">Így segítséget nyújt az infrastruktúra költségek a helyszíni kiszolgálók futó selejtezni.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-162">Doing so helps you get rid of the infrastructure costs of running servers on-premises.</span></span>

* <span data-ttu-id="2b5f7-163">**Alkalmazások közzététele az Azure AD-alkalmazásproxy használatával:** közzététele az Azure AD-alkalmazásproxy használata a Azure virtuális gépek futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-163">**Publish applications using Azure AD Application Proxy:** Publish applications running on your Azure virtual machines using the Azure AD Application Proxy.</span></span> <span data-ttu-id="2b5f7-164">További információkért lásd: [az Azure AD-alkalmazásproxy használó alkalmazások közzététele](../active-directory/application-proxy-publish-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2b5f7-164">For more information, see [publish applications using Azure AD Application Proxy](../active-directory/application-proxy-publish-azure-portal.md)</span></span>


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="2b5f7-165">Központi telepítés Megjegyzés - közzététel integrált Windows-Hitelesítést (integrált Windows-hitelesítés) alkalmazások az Azure AD-alkalmazásproxy használatával</span><span class="sxs-lookup"><span data-stu-id="2b5f7-165">Deployment note - Publish IWA (Integrated Windows Authentication) applications using Azure AD Application Proxy</span></span>
<span data-ttu-id="2b5f7-166">Egyszeri bejelentkezés az alkalmazások által Application Proxy összekötők engedély integrált Windows-hitelesítéssel (IWA) segítségével megszemélyesíthet felhasználókat, küldésére és fogadására a nevében tokenek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-166">Enable single sign-on to your applications using Integrated Windows Authentication (IWA) by granting Application Proxy Connectors permission to impersonate users, and send and receive tokens on their behalf.</span></span> <span data-ttu-id="2b5f7-167">Kerberos által korlátozott delegálás (KCD) konfigurálása az összekötő és a felügyelt tartományra lévő erőforrások eléréséhez szükséges engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-167">Configure kerberos constrained delegation (KCD) for the connector to grant the required permissions to access resources on the managed domain.</span></span> <span data-ttu-id="2b5f7-168">Az erőforrás-alapú Kerberos által korlátozott Delegálás mechanizmust használni a felügyelt tartományok a biztonság fokozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-168">Use the resource-based KCD mechanism on managed domains for increased security.</span></span>


### <a name="enable-resource-based-kerberos-constrained-delegation-for-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="2b5f7-169">Erőforrás-alapú kerberos által korlátozott delegálás az Azure AD alkalmazásproxy-összekötő engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2b5f7-169">Enable resource-based kerberos constrained delegation for the Azure AD Application Proxy connector</span></span>
<span data-ttu-id="2b5f7-170">Az Azure Application Proxy connector konfigurálnia kell a kerberos által korlátozott delegálás (KCD), ezért esetében megszemélyesíthet felhasználókat a felügyelt tartományra.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-170">The Azure Application Proxy connector should be configured for kerberos constrained delegation (KCD), so it can impersonate users on the managed domain.</span></span> <span data-ttu-id="2b5f7-171">Az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz, a nem tartományi rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-171">On an Azure AD Domain Services managed domain, you do not have domain administrator privileges.</span></span> <span data-ttu-id="2b5f7-172">Ezért **hagyományos fiók szintű Kerberos által korlátozott Delegálás nem konfigurálható egy felügyelt tartomány**.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-172">Therefore, **traditional account-level KCD cannot be configured on a managed domain**.</span></span>

<span data-ttu-id="2b5f7-173">Erőforrás-alapú Kerberos által korlátozott Delegálás használata, ez a [cikk](active-directory-ds-enable-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="2b5f7-173">Use resource-based KCD as described in this [article](active-directory-ds-enable-kcd.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2b5f7-174">Felügyelheti a felügyelt tartományra AD PowerShell-parancsmagok használatával "AAD DC rendszergazdák" csoport tagjának lennie kell.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-174">You need to be a member of the 'AAD DC Administrators' group, to administer the managed domain using AD PowerShell cmdlets.</span></span>
>
>

<span data-ttu-id="2b5f7-175">A Get-ADComputer PowerShell-parancsmag használatával a számítógép, amelyen telepítve van az Azure AD alkalmazásproxy-összekötő beállításainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-175">Use the Get-ADComputer PowerShell cmdlet to retrieve the settings for the computer on which the Azure AD Application Proxy connector is installed.</span></span>
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

<span data-ttu-id="2b5f7-176">Ezt követően a Set-ADComputer parancsmag segítségével erőforrás-alapú az erőforrás-kiszolgáló Kerberos által korlátozott Delegálás beállítása.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-176">Thereafter, use the Set-ADComputer cmdlet to set up resource-based KCD for the resource server.</span></span>
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

<span data-ttu-id="2b5f7-177">Ha több alkalmazásproxy-összekötő a felügyelt tartomány telepítette, akkor erőforrás-alapú minden ilyen összekötő-példánynak a Kerberos által korlátozott Delegálás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2b5f7-177">If you have deployed multiple Application Proxy connectors on your managed domain, you need to configure resource-based KCD for each such connector instance.</span></span>


## <a name="related-content"></a><span data-ttu-id="2b5f7-178">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="2b5f7-178">Related Content</span></span>
* [<span data-ttu-id="2b5f7-179">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="2b5f7-179">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="2b5f7-180">Kerberos által korlátozott delegálás konfigurálása a felügyelt tartományhoz</span><span class="sxs-lookup"><span data-stu-id="2b5f7-180">Configure Kerberos Constrained Delegation on a managed domain</span></span>](active-directory-ds-enable-kcd.md)
* [<span data-ttu-id="2b5f7-181">A Kerberos általi korlátozott delegálás áttekintése</span><span class="sxs-lookup"><span data-stu-id="2b5f7-181">Kerberos Constrained Delegation Overview</span></span>](https://technet.microsoft.com/library/jj553400.aspx)
