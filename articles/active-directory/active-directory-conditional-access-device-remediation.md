---
title: "Innen nem érheti el az Azure Portalon Windows-eszközről| Microsoft Docs"
description: "Ismerje meg, honnan származik az „Innen nem érheti el” üzenet, és mely tényezőknek az ellenőrzésével előzheti meg, hogy belefusson."
services: active-directory
keywords: "eszközalapú feltételes hozzáférés, eszközregisztráció, eszközregisztráció engedélyezése, eszközregisztráció és MDM"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 16543c7bb7b6b236dcc24093c9963bc218ca1fa6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a><span data-ttu-id="59166-104">Innen nem érheti el Windows-eszközről</span><span class="sxs-lookup"><span data-stu-id="59166-104">You can't get there from here on a Windows device</span></span>

<span data-ttu-id="59166-105">Ha például a szervezet SharePoint Online intranetéhez próbál hozzáférni, megjelenhet egy oldal a következő üzenettel: *Innen nem érheti el*.</span><span class="sxs-lookup"><span data-stu-id="59166-105">During an attempt to, for example, access your organization's SharePoint Online intranet you might run into a page that states that *you can't get there from here*.</span></span> <span data-ttu-id="59166-106">Az oldal azért jelenik meg, mert a rendszergazda olyan hozzáférési szabályzatot állított be, amely bizonyos feltételek szerint megakadályozza a vállalat erőforrásaihoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="59166-106">You are seeing this page, because your administrator has configured a conditional access policy that prevents access to your organization's resources under certain conditions.</span></span> <span data-ttu-id="59166-107">Bár lehetséges, hogy a probléma megoldásához végül az ügyfélszolgálathoz vagy a rendszergazdához kell fordulni, érdemes először megpróbálkozni néhány lépéssel.</span><span class="sxs-lookup"><span data-stu-id="59166-107">While it might be necessary to contact helpdesk or your administrator to get this problem solved, there are a few things you can try out yourself, first.</span></span>

<span data-ttu-id="59166-108">Ha **Windows**-eszközt használ, ellenőrizze a következőket:</span><span class="sxs-lookup"><span data-stu-id="59166-108">If you are using a **Windows** device, you should check the following:</span></span>

- <span data-ttu-id="59166-109">Támogatott böngészőt használ?</span><span class="sxs-lookup"><span data-stu-id="59166-109">Are you using a supported browser?</span></span>

- <span data-ttu-id="59166-110">A Windows támogatott verzióját futtatja az eszközön?</span><span class="sxs-lookup"><span data-stu-id="59166-110">Are you running a supported version of Windows on your device?</span></span>

- <span data-ttu-id="59166-111">Az eszköz megfelelő?</span><span class="sxs-lookup"><span data-stu-id="59166-111">Is your device compliant?</span></span>






## <a name="supported-browser"></a><span data-ttu-id="59166-112">Támogatott böngésző</span><span class="sxs-lookup"><span data-stu-id="59166-112">Supported browser</span></span>

<span data-ttu-id="59166-113">Ha a rendszergazda feltételes hozzáférési szabályzatot állított be, akkor csak támogatott böngészővel férhet hozzá a szervezet erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="59166-113">If your administrator has configured a conditional access policy, you can only access your organization's resources by using a supported browser.</span></span> <span data-ttu-id="59166-114">Windows-eszközön csak az **Internet Explorer** és az **Edge** támogatott.</span><span class="sxs-lookup"><span data-stu-id="59166-114">On a Windows device, only **Internet Explorer** and **Edge** are supported.</span></span>

<span data-ttu-id="59166-115">Ha egy erőforráshoz egy nem támogatott böngésző miatt nem férhet hozzá, azt könnyen megállapíthatja a hibaoldal részletek szakaszának megtekintésével:</span><span class="sxs-lookup"><span data-stu-id="59166-115">You can easily identify whether you can't access a resource due to an unsupported browser by looking at the details section of the error page:</span></span>

<span data-ttu-id="59166-116">![„Innen nem érheti el” üzenetek nem támogatott böngészők esetén](./media/active-directory-conditional-access-device-remediation/02.png "Forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="59166-116">!["You can't get there from here" message for unsupported browsers](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")</span></span>

<span data-ttu-id="59166-117">Az egyetlen javítási megoldás egy olyan böngésző használata, amelyet az alkalmazás támogat az adott eszközplatformon.</span><span class="sxs-lookup"><span data-stu-id="59166-117">The only remediation is to use a browser that the application supports for your device platform.</span></span> <span data-ttu-id="59166-118">A támogatott böngészők teljes listáját a [támogatott böngészők](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies) című témakörben találja.</span><span class="sxs-lookup"><span data-stu-id="59166-118">For a complete list of supported browsers, see [supported browsers](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span></span>  


## <a name="supported-versions-of-windows"></a><span data-ttu-id="59166-119">A Windows támogatott verziói</span><span class="sxs-lookup"><span data-stu-id="59166-119">Supported versions of Windows</span></span>

<span data-ttu-id="59166-120">A következő feltételeknek kell teljesülnie az eszközön futó Windows operációs rendszer esetében:</span><span class="sxs-lookup"><span data-stu-id="59166-120">The following must be true about the Windows operating system on your device:</span></span> 

- <span data-ttu-id="59166-121">Ha eszközén Windows asztali operációs rendszert használ, annak verziója Windows 7 vagy újabb kell, hogy legyen.</span><span class="sxs-lookup"><span data-stu-id="59166-121">If you are running a Windows desktop operating system on your device, it needs to be Windows 7 or later.</span></span>
- <span data-ttu-id="59166-122">Ha eszközén Windows kiszolgálói operációs rendszert használ, annak verziója Windows Server 2008 R2 vagy újabb kell, hogy legyen.</span><span class="sxs-lookup"><span data-stu-id="59166-122">If you are running a Windows server operating system on your device, it needs to be Windows Server 2008 R2 or later.</span></span> 


## <a name="compliant-device"></a><span data-ttu-id="59166-123">Megfelelő eszköz</span><span class="sxs-lookup"><span data-stu-id="59166-123">Compliant device</span></span>

<span data-ttu-id="59166-124">Lehetséges, hogy a rendszergazda olyan feltételes hozzáférési szabályzatot állított be, amely a vállalat erőforrásaihoz való hozzáférést csak megfelelő eszközökről engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="59166-124">Your administrator might have configured a conditional access policy that allows access to your organization's resources only from compliant devices.</span></span> <span data-ttu-id="59166-125">A megfeleléshez az eszköznek csatlakoznia kell a helyszíni Active Directoryhez vagy az Azure Active Directoryhez.</span><span class="sxs-lookup"><span data-stu-id="59166-125">To be compliant, your device must be either joined to your on-premises Active Directory or joined to your Azure Active Directory.</span></span>

<span data-ttu-id="59166-126">Ha egy erőforráshoz egy nem megfelelő eszköz miatt nem férhet hozzá, azt könnyedén megállapíthatja a hibaoldal részletek szakaszának megtekintésével:</span><span class="sxs-lookup"><span data-stu-id="59166-126">You can easily identify whether you can't access a resource due to a device that is not compliant by looking at the details section of the error page:</span></span>
 
<span data-ttu-id="59166-127">![„Innen nem érheti el” üzenetek nem regisztrált eszközök esetén](./media/active-directory-conditional-access-device-remediation/01.png "Forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="59166-127">!["You can't get there from here" messages for unregistered devices](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")</span></span>


### <a name="is-your-device-joined-to-an-on-premises-active-directory"></a><span data-ttu-id="59166-128">Az eszköz egy helyszíni Active Directoryhez csatlakozik?</span><span class="sxs-lookup"><span data-stu-id="59166-128">Is your device joined to an on-premises Active Directory?</span></span>

<span data-ttu-id="59166-129">**Ha az eszköz egy helyszíni Active Directoryhez csatlakozik a szervezeten belül:**</span><span class="sxs-lookup"><span data-stu-id="59166-129">**If your device is joined to an on-premises Active Directory in your organization:**</span></span>

1. <span data-ttu-id="59166-130">Győződjön meg róla, hogy a munkahelyi fiókot (Active Directory-fiókját) használva lépett be a Windowsba.</span><span class="sxs-lookup"><span data-stu-id="59166-130">Make sure that you sign in to Windows by using your work account (your Active Directory account).</span></span>
2. <span data-ttu-id="59166-131">Csatlakozzon a vállalati hálózathoz virtuális magánhálózaton (VPN) vagy DirectAccessen keresztül.</span><span class="sxs-lookup"><span data-stu-id="59166-131">Connect to your corporate network via a virtual private network (VPN) or DirectAccess.</span></span>
3. <span data-ttu-id="59166-132">Miután csatlakozott, zárolja a Windows-munkamenetet a Windows-gomb és az L billentyű lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="59166-132">After you are connected, press the Windows logo key + the L key to lock your Windows session.</span></span>
4. <span data-ttu-id="59166-133">Oldja fel a Windows-munkamenet zárolását a munkahelyi fiókjához tartozó hitelesítő adatok beírásával.</span><span class="sxs-lookup"><span data-stu-id="59166-133">Unlock your Windows session by entering your work account credentials.</span></span>
5. <span data-ttu-id="59166-134">Várjon egy percet, majd próbálja meg újból elérni az alkalmazást vagy a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="59166-134">Wait for a minute, and then try again to access the application or service.</span></span>
6. <span data-ttu-id="59166-135">Ha ugyanazt a lapot látja, kattintson a **További részletek** hivatkozásra, és az ott található információt adja át a rendszergazdának.</span><span class="sxs-lookup"><span data-stu-id="59166-135">If you see the same page, click the **More details** link, and then contact your administrator with the details.</span></span>


### <a name="is-your-device-not-joined-to-an-on-premises-active-directory"></a><span data-ttu-id="59166-136">Az eszköz nem csatlakozik egy helyszíni Active Directoryhez?</span><span class="sxs-lookup"><span data-stu-id="59166-136">Is your device not joined to an on-premises Active Directory?</span></span>

<span data-ttu-id="59166-137">Ha az eszköze nem csatlakozik egy helyszíni Active Directoryhez, és Windows 10 rendszert futtat, két lehetőség áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="59166-137">If your device is not joined to an on-premises Active Directory and runs Windows 10, you have two options:</span></span>

* <span data-ttu-id="59166-138">Futtassa az Azure AD Joint</span><span class="sxs-lookup"><span data-stu-id="59166-138">Run Azure AD Join</span></span>
* <span data-ttu-id="59166-139">Adja hozzá a munkahelyi vagy iskolai fiókját a Windowshoz</span><span class="sxs-lookup"><span data-stu-id="59166-139">Add your work or school account to Windows</span></span>

<span data-ttu-id="59166-140">További információt a két megoldás közötti különbségekről itt talál: [Windows 10-es eszközök használata a munkahelyen ](active-directory-azureadjoin-windows10-devices.md).</span><span class="sxs-lookup"><span data-stu-id="59166-140">For information about how these options are different, see [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md).</span></span>  
<span data-ttu-id="59166-141">Ha az eszköz:</span><span class="sxs-lookup"><span data-stu-id="59166-141">If your device:</span></span>

- <span data-ttu-id="59166-142">A szervezethez tartozik, futtassa az Azure AD Joint.</span><span class="sxs-lookup"><span data-stu-id="59166-142">Belongs to your organization, you should run Azure AD Join.</span></span>
- <span data-ttu-id="59166-143">Egy személyes eszköz vagy Windows-telefon, adja hozzá a munkahelyi vagy iskolai fiókját a Windowshoz</span><span class="sxs-lookup"><span data-stu-id="59166-143">Is a personal device or a Windows phone, you should add your work or school account to Windows</span></span> 



#### <a name="azure-ad-join-on-windows-10"></a><span data-ttu-id="59166-144">Azure AD Join a Windows 10 rendszeren</span><span class="sxs-lookup"><span data-stu-id="59166-144">Azure AD Join on Windows 10</span></span>

<span data-ttu-id="59166-145">Az eszköz Azure AD-hez való csatlakoztatásának lépései az eszközön futó Windows 10 verziójához vannak kötve.</span><span class="sxs-lookup"><span data-stu-id="59166-145">The steps to join your device to Azure AD are tied the version of Windows 10 you are running on it.</span></span> <span data-ttu-id="59166-146">A Windows 10 operációs rendszer verziójának megállapításához futtassa a **winver** parancsot:</span><span class="sxs-lookup"><span data-stu-id="59166-146">To determine the version of your Windows 10 operating system, run the **winver** command:</span></span> 

![Windows-verzió](./media/active-directory-conditional-access-device-remediation/03.png )


<span data-ttu-id="59166-148">**Windows 10 évfordulós frissítés (1607-es verzió):**</span><span class="sxs-lookup"><span data-stu-id="59166-148">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="59166-149">Nyissa meg a **Gépházat**.</span><span class="sxs-lookup"><span data-stu-id="59166-149">Open the **Settings** app.</span></span>
2. <span data-ttu-id="59166-150">Kattintson a **Fiókok** > **Hozzáférés munkahelyi vagy iskolai rendszerhez** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-150">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="59166-151">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="59166-151">Click **Connect**.</span></span>
4. <span data-ttu-id="59166-152">Kattintson az **Eszköz csatlakoztatása az Azure AD-hez** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-152">Click **Join this device to Azure AD**.</span></span>
5. <span data-ttu-id="59166-153">Hitelesítse magát szervezeténél, biztosítson többtényezős hitelesítési adatokat, ha szükséges, majd kövesse a bemutatott lépéseket.</span><span class="sxs-lookup"><span data-stu-id="59166-153">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
6. <span data-ttu-id="59166-154">Jelentkezzen ki, majd jelentkezzen be a munkahelyi fiókjával.</span><span class="sxs-lookup"><span data-stu-id="59166-154">Sign out, and then sign in with your work account.</span></span>
7. <span data-ttu-id="59166-155">Próbálja meg újból elérni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="59166-155">Try again to access the application.</span></span>

<span data-ttu-id="59166-156">**Windows 10, 2015 novemberi frissítés (1511-es verzió):**</span><span class="sxs-lookup"><span data-stu-id="59166-156">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="59166-157">Nyissa meg a **Gépházat**.</span><span class="sxs-lookup"><span data-stu-id="59166-157">Open the **Settings** app.</span></span>
2. <span data-ttu-id="59166-158">Kattintson a **Rendszer** > **Névjegy** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-158">Click **System** > **About**.</span></span>
3. <span data-ttu-id="59166-159">Kattintson a **Csatlakozás az Azure AD-hez** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-159">Click **Join Azure AD**.</span></span>
4. <span data-ttu-id="59166-160">Hitelesítse magát szervezeténél, biztosítson többtényezős hitelesítési adatokat, ha szükséges, majd kövesse a bemutatott lépéseket.</span><span class="sxs-lookup"><span data-stu-id="59166-160">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="59166-161">Jelentkezzen ki, majd jelentkezzen be a munkahelyi fiókjával (Azure AD-fiókjával).</span><span class="sxs-lookup"><span data-stu-id="59166-161">Sign out, and then sign in with your work account (your Azure AD account).</span></span>
6. <span data-ttu-id="59166-162">Próbálja meg újból elérni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="59166-162">Try again to access the application.</span></span>


#### <a name="workplace-join-on-windows-81"></a><span data-ttu-id="59166-163">Munkahelyi csatlakoztatás Windows 8.1 rendszeren</span><span class="sxs-lookup"><span data-stu-id="59166-163">Workplace Join on Windows 8.1</span></span>

<span data-ttu-id="59166-164">Ha az eszköze nincs tartományhoz csatlakoztatva és Windows 8.1 rendszert futtat, munkahelyi csatlakoztatást hajthat végre, és regisztrálhat a Microsoft Intune-ba a következők elvégzésével:</span><span class="sxs-lookup"><span data-stu-id="59166-164">If your device is not domain-joined and runs Windows 8.1, to do a Workplace Join and enroll in Microsoft Intune, do the following steps:</span></span>

1. <span data-ttu-id="59166-165">Nyissa meg a **Gépházat**.</span><span class="sxs-lookup"><span data-stu-id="59166-165">Open **PC Settings**.</span></span>
2. <span data-ttu-id="59166-166">Kattintson a **Hálózat** > **Munkahely** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-166">Click **Network** > **Workplace**.</span></span>
3. <span data-ttu-id="59166-167">Kattintson a **Csatlakozás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="59166-167">Click **Join**.</span></span>
4. <span data-ttu-id="59166-168">Hitelesítse magát szervezeténél, biztosítson többtényezős hitelesítési adatokat, ha szükséges, majd kövesse a bemutatott lépéseket.</span><span class="sxs-lookup"><span data-stu-id="59166-168">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="59166-169">Kattintson a **Bekapcsolás** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-169">Click **Turn on**.</span></span>
6. <span data-ttu-id="59166-170">Próbálja meg újból elérni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="59166-170">Try again to access the application.</span></span>



#### <a name="add-your-work-or-school-account-to-windows"></a><span data-ttu-id="59166-171">Adja hozzá a munkahelyi vagy iskolai fiókját a Windowshoz</span><span class="sxs-lookup"><span data-stu-id="59166-171">Add your work or school account to Windows</span></span> 


<span data-ttu-id="59166-172">**Windows 10 évfordulós frissítés (1607-es verzió):**</span><span class="sxs-lookup"><span data-stu-id="59166-172">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="59166-173">Nyissa meg a **Gépházat**.</span><span class="sxs-lookup"><span data-stu-id="59166-173">Open the **Settings** app.</span></span>
2. <span data-ttu-id="59166-174">Kattintson a **Fiókok** > **Hozzáférés munkahelyi vagy iskolai rendszerhez** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-174">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="59166-175">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="59166-175">Click **Connect**.</span></span>
4. <span data-ttu-id="59166-176">Hitelesítse magát szervezeténél, biztosítson többtényezős hitelesítési adatokat, ha szükséges, majd kövesse a bemutatott lépéseket.</span><span class="sxs-lookup"><span data-stu-id="59166-176">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="59166-177">Próbálja meg újból elérni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="59166-177">Try again to access the application.</span></span>


<span data-ttu-id="59166-178">**Windows 10, 2015 novemberi frissítés (1511-es verzió):**</span><span class="sxs-lookup"><span data-stu-id="59166-178">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="59166-179">Nyissa meg a **Gépházat**.</span><span class="sxs-lookup"><span data-stu-id="59166-179">Open the **Settings** app.</span></span>
2. <span data-ttu-id="59166-180">Kattintson a **Fiókok** > **Saját fiókok** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-180">Click **Accounts** > **Your accounts**.</span></span>
3. <span data-ttu-id="59166-181">Kattintson a **Munkahelyi vagy iskolai fiók beállítása** elemre.</span><span class="sxs-lookup"><span data-stu-id="59166-181">Click **Add work or school account**.</span></span>
4. <span data-ttu-id="59166-182">Hitelesítse magát szervezeténél, biztosítson többtényezős hitelesítési adatokat, ha szükséges, majd kövesse a bemutatott lépéseket.</span><span class="sxs-lookup"><span data-stu-id="59166-182">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="59166-183">Próbálja meg újból elérni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="59166-183">Try again to access the application.</span></span>





## <a name="next-steps"></a><span data-ttu-id="59166-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59166-184">Next steps</span></span>
[<span data-ttu-id="59166-185">Azure Active Directory feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="59166-185">Azure Active Directory conditional access</span></span>](active-directory-conditional-access.md)

