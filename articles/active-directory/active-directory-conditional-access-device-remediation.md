---
title: "aaaYou nem érheti el a hello itt egy Windows-eszköz az Azure portálon |} Microsoft Docs"
description: "Ismerje meg, ahol nem get van helyről származik és mit ellenőrizheti, ezen a párbeszédpanelen rendszert futtató tooavoid."
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
ms.openlocfilehash: eda2aa10fbff5b3e515723219f61c1cb6f634e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a><span data-ttu-id="5b9b9-104">Innen nem érheti el Windows-eszközről</span><span class="sxs-lookup"><span data-stu-id="5b9b9-104">You can't get there from here on a Windows device</span></span>

<span data-ttu-id="5b9b9-105">Ha például a szervezet SharePoint Online intranetéhez próbál hozzáférni, megjelenhet egy oldal a következő üzenettel: *Innen nem érheti el*.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-105">During an attempt to, for example, access your organization's SharePoint Online intranet you might run into a page that states that *you can't get there from here*.</span></span> <span data-ttu-id="5b9b9-106">Ez a lap azért jelent meg, mert a rendszergazda által megadott egy feltételes hozzáférési szabályzatot, amely megakadályozza, hogy bizonyos feltételek hozzáférés tooyour szervezet erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-106">You are seeing this page, because your administrator has configured a conditional access policy that prevents access tooyour organization's resources under certain conditions.</span></span> <span data-ttu-id="5b9b9-107">Bár szükséges toocontact ügyfélszolgálathoz vagy a rendszergazda tooget, ez a probléma megoldódott, dolgot néhány próbálhatja ki saját kezűleg, először.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-107">While it might be necessary toocontact helpdesk or your administrator tooget this problem solved, there are a few things you can try out yourself, first.</span></span>

<span data-ttu-id="5b9b9-108">Ha használ egy **Windows** eszközt, jelölje be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5b9b9-108">If you are using a **Windows** device, you should check hello following:</span></span>

- <span data-ttu-id="5b9b9-109">Támogatott böngészőt használ?</span><span class="sxs-lookup"><span data-stu-id="5b9b9-109">Are you using a supported browser?</span></span>

- <span data-ttu-id="5b9b9-110">A Windows támogatott verzióját futtatja az eszközön?</span><span class="sxs-lookup"><span data-stu-id="5b9b9-110">Are you running a supported version of Windows on your device?</span></span>

- <span data-ttu-id="5b9b9-111">Az eszköz megfelelő?</span><span class="sxs-lookup"><span data-stu-id="5b9b9-111">Is your device compliant?</span></span>






## <a name="supported-browser"></a><span data-ttu-id="5b9b9-112">Támogatott böngésző</span><span class="sxs-lookup"><span data-stu-id="5b9b9-112">Supported browser</span></span>

<span data-ttu-id="5b9b9-113">Ha a rendszergazda feltételes hozzáférési szabályzatot állított be, akkor csak támogatott böngészővel férhet hozzá a szervezet erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-113">If your administrator has configured a conditional access policy, you can only access your organization's resources by using a supported browser.</span></span> <span data-ttu-id="5b9b9-114">Windows-eszközön csak az **Internet Explorer** és az **Edge** támogatott.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-114">On a Windows device, only **Internet Explorer** and **Edge** are supported.</span></span>

<span data-ttu-id="5b9b9-115">Egyszerűen azonosíthatja, hogy nem fér hozzá egy erőforrás miatt tooan nem támogatott böngészőt hello részletes adatait tartalmazó részben hello hibalap megtekintésével:</span><span class="sxs-lookup"><span data-stu-id="5b9b9-115">You can easily identify whether you can't access a resource due tooan unsupported browser by looking at hello details section of hello error page:</span></span>

<span data-ttu-id="5b9b9-116">![„Innen nem érheti el” üzenetek nem támogatott böngészők esetén](./media/active-directory-conditional-access-device-remediation/02.png "Forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="5b9b9-116">!["You can't get there from here" message for unsupported browsers](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")</span></span>

<span data-ttu-id="5b9b9-117">hello csak szervizelési toouse hello alkalmazás támogatja-e az eszközplatformnál böngészőt.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-117">hello only remediation is toouse a browser that hello application supports for your device platform.</span></span> <span data-ttu-id="5b9b9-118">A támogatott böngészők teljes listáját a [támogatott böngészők](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies) című témakörben találja.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-118">For a complete list of supported browsers, see [supported browsers](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span></span>  


## <a name="supported-versions-of-windows"></a><span data-ttu-id="5b9b9-119">A Windows támogatott verziói</span><span class="sxs-lookup"><span data-stu-id="5b9b9-119">Supported versions of Windows</span></span>

<span data-ttu-id="5b9b9-120">hello következő hello Windows operációs rendszer eszközére vonatkozó teljesülnie kell:</span><span class="sxs-lookup"><span data-stu-id="5b9b9-120">hello following must be true about hello Windows operating system on your device:</span></span> 

- <span data-ttu-id="5b9b9-121">Windows asztali operációs rendszert használ az eszközt, hogy szükséges-toobe Windows 7 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-121">If you are running a Windows desktop operating system on your device, it needs toobe Windows 7 or later.</span></span>
- <span data-ttu-id="5b9b9-122">Egy Windows server operációs rendszert használ az eszközt, hogy szükséges-e toobe Windows Server 2008 R2 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-122">If you are running a Windows server operating system on your device, it needs toobe Windows Server 2008 R2 or later.</span></span> 


## <a name="compliant-device"></a><span data-ttu-id="5b9b9-123">Megfelelő eszköz</span><span class="sxs-lookup"><span data-stu-id="5b9b9-123">Compliant device</span></span>

<span data-ttu-id="5b9b9-124">Előfordulhat, hogy a rendszergazda engedélyezte egy feltételes hozzáférési szabályzatot, amely lehetővé teszi, hogy csak az előírásoknak megfelelő eszközök hozzáférési tooyour szervezet erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-124">Your administrator might have configured a conditional access policy that allows access tooyour organization's resources only from compliant devices.</span></span> <span data-ttu-id="5b9b9-125">kompatibilis, az eszköznek meg kell mindkét illesztett tooyour toobe a helyszíni Active Directory vagy tooyour Azure Active Directory tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-125">toobe compliant, your device must be either joined tooyour on-premises Active Directory or joined tooyour Azure Active Directory.</span></span>

<span data-ttu-id="5b9b9-126">Könnyen azonosíthatja, hogy miatt tooa eszköz, amely nem kompatibilis hello részletes adatait tartalmazó részben hello hibalap megtekintésével erőforrás nem tud hozzáférni:</span><span class="sxs-lookup"><span data-stu-id="5b9b9-126">You can easily identify whether you can't access a resource due tooa device that is not compliant by looking at hello details section of hello error page:</span></span>
 
<span data-ttu-id="5b9b9-127">![„Innen nem érheti el” üzenetek nem regisztrált eszközök esetén](./media/active-directory-conditional-access-device-remediation/01.png "Forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="5b9b9-127">!["You can't get there from here" messages for unregistered devices](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")</span></span>


### <a name="is-your-device-joined-tooan-on-premises-active-directory"></a><span data-ttu-id="5b9b9-128">A csatlakoztatott eszköz tooan a helyszíni Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5b9b9-128">Is your device joined tooan on-premises Active Directory?</span></span>

<span data-ttu-id="5b9b9-129">**Ha az eszköz csatlakoztatva van tooan a helyszíni Active Directory a szervezetében:**</span><span class="sxs-lookup"><span data-stu-id="5b9b9-129">**If your device is joined tooan on-premises Active Directory in your organization:**</span></span>

1. <span data-ttu-id="5b9b9-130">Győződjön meg arról, hogy bejelentkezik tooWindows be munkahelyi fiókjával (az Active Directory-fiókot).</span><span class="sxs-lookup"><span data-stu-id="5b9b9-130">Make sure that you sign in tooWindows by using your work account (your Active Directory account).</span></span>
2. <span data-ttu-id="5b9b9-131">Csatlakozás tooyour a vállalati hálózathoz a DirectAccess vagy virtuális magánhálózati (VPN) keresztül.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-131">Connect tooyour corporate network via a virtual private network (VPN) or DirectAccess.</span></span>
3. <span data-ttu-id="5b9b9-132">Miután csatlakozott, nyomja le az hello Windows billentyű + hello L kulcs toolock a Windows-munkamenet.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-132">After you are connected, press hello Windows logo key + hello L key toolock your Windows session.</span></span>
4. <span data-ttu-id="5b9b9-133">Oldja fel a Windows-munkamenet zárolását a munkahelyi fiókjához tartozó hitelesítő adatok beírásával.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-133">Unlock your Windows session by entering your work account credentials.</span></span>
5. <span data-ttu-id="5b9b9-134">Várjon egy percet, és próbálja meg újból tooaccess hello alkalmazást vagy szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-134">Wait for a minute, and then try again tooaccess hello application or service.</span></span>
6. <span data-ttu-id="5b9b9-135">Ha látja hello azonos lapján kattintson a hello **további részleteket** hivatkozásra, és forduljon a rendszergazdához hello adatokkal.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-135">If you see hello same page, click hello **More details** link, and then contact your administrator with hello details.</span></span>


### <a name="is-your-device-not-joined-tooan-on-premises-active-directory"></a><span data-ttu-id="5b9b9-136">Az eszköz nincs tartományhoz csatlakoztatva tooan a helyszíni Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5b9b9-136">Is your device not joined tooan on-premises Active Directory?</span></span>

<span data-ttu-id="5b9b9-137">Ha az eszköz nincs tartományhoz csatlakoztatva tooan a helyszíni Active Directory és a Windows 10 rendszerű, két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="5b9b9-137">If your device is not joined tooan on-premises Active Directory and runs Windows 10, you have two options:</span></span>

* <span data-ttu-id="5b9b9-138">Futtassa az Azure AD Joint</span><span class="sxs-lookup"><span data-stu-id="5b9b9-138">Run Azure AD Join</span></span>
* <span data-ttu-id="5b9b9-139">Adja hozzá a munkahelyi vagy iskolai fiók tooWindows</span><span class="sxs-lookup"><span data-stu-id="5b9b9-139">Add your work or school account tooWindows</span></span>

<span data-ttu-id="5b9b9-140">További információt a két megoldás közötti különbségekről itt talál: [Windows 10-es eszközök használata a munkahelyen ](active-directory-azureadjoin-windows10-devices.md).</span><span class="sxs-lookup"><span data-stu-id="5b9b9-140">For information about how these options are different, see [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md).</span></span>  
<span data-ttu-id="5b9b9-141">Ha az eszköz:</span><span class="sxs-lookup"><span data-stu-id="5b9b9-141">If your device:</span></span>

- <span data-ttu-id="5b9b9-142">Tooyour szervezet tartozik, az Azure AD Join kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-142">Belongs tooyour organization, you should run Azure AD Join.</span></span>
- <span data-ttu-id="5b9b9-143">Személyes eszköz vagy Windows Phone-eszközön, adja hozzá a munkahelyi vagy iskolai fiók tooWindows kell</span><span class="sxs-lookup"><span data-stu-id="5b9b9-143">Is a personal device or a Windows phone, you should add your work or school account tooWindows</span></span> 



#### <a name="azure-ad-join-on-windows-10"></a><span data-ttu-id="5b9b9-144">Azure AD Join a Windows 10 rendszeren</span><span class="sxs-lookup"><span data-stu-id="5b9b9-144">Azure AD Join on Windows 10</span></span>

<span data-ttu-id="5b9b9-145">hello lépéseket toojoin az eszköz tooAzure AD vannak társítva hello futnak a kiszolgálón Windows 10-es verzióját.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-145">hello steps toojoin your device tooAzure AD are tied hello version of Windows 10 you are running on it.</span></span> <span data-ttu-id="5b9b9-146">a Windows 10 operációs rendszer hello toodetermine hello verziójának **winver** parancs:</span><span class="sxs-lookup"><span data-stu-id="5b9b9-146">toodetermine hello version of your Windows 10 operating system, run hello **winver** command:</span></span> 

![Windows-verzió](./media/active-directory-conditional-access-device-remediation/03.png )


<span data-ttu-id="5b9b9-148">**Windows 10 évfordulós frissítés (1607-es verzió):**</span><span class="sxs-lookup"><span data-stu-id="5b9b9-148">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="5b9b9-149">Nyissa meg hello **beállítások** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-149">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="5b9b9-150">Kattintson a **Fiókok** > **Hozzáférés munkahelyi vagy iskolai rendszerhez** elemre.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-150">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="5b9b9-151">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-151">Click **Connect**.</span></span>
4. <span data-ttu-id="5b9b9-152">Kattintson a **az eszköz tooAzure AD Join**.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-152">Click **Join this device tooAzure AD**.</span></span>
5. <span data-ttu-id="5b9b9-153">Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-153">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
6. <span data-ttu-id="5b9b9-154">Jelentkezzen ki, majd jelentkezzen be a munkahelyi fiókjával.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-154">Sign out, and then sign in with your work account.</span></span>
7. <span data-ttu-id="5b9b9-155">Próbálja meg újra tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-155">Try again tooaccess hello application.</span></span>

<span data-ttu-id="5b9b9-156">**Windows 10, 2015 novemberi frissítés (1511-es verzió):**</span><span class="sxs-lookup"><span data-stu-id="5b9b9-156">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="5b9b9-157">Nyissa meg hello **beállítások** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-157">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="5b9b9-158">Kattintson a **Rendszer** > **Névjegy** elemre.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-158">Click **System** > **About**.</span></span>
3. <span data-ttu-id="5b9b9-159">Kattintson a **Csatlakozás az Azure AD-hez** elemre.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-159">Click **Join Azure AD**.</span></span>
4. <span data-ttu-id="5b9b9-160">Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-160">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="5b9b9-161">Jelentkezzen ki, majd jelentkezzen be a munkahelyi fiókjával (Azure AD-fiókjával).</span><span class="sxs-lookup"><span data-stu-id="5b9b9-161">Sign out, and then sign in with your work account (your Azure AD account).</span></span>
6. <span data-ttu-id="5b9b9-162">Próbálja meg újra tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-162">Try again tooaccess hello application.</span></span>


#### <a name="workplace-join-on-windows-81"></a><span data-ttu-id="5b9b9-163">Munkahelyi csatlakoztatás Windows 8.1 rendszeren</span><span class="sxs-lookup"><span data-stu-id="5b9b9-163">Workplace Join on Windows 8.1</span></span>

<span data-ttu-id="5b9b9-164">Az eszköz nincs tartományhoz és a munkahelyi csatlakoztatás Windows 8.1, toodo fut, és a Microsoft Intune-beli regisztrálásakor, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5b9b9-164">If your device is not domain-joined and runs Windows 8.1, toodo a Workplace Join and enroll in Microsoft Intune, do hello following steps:</span></span>

1. <span data-ttu-id="5b9b9-165">Nyissa meg a **Gépházat**.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-165">Open **PC Settings**.</span></span>
2. <span data-ttu-id="5b9b9-166">Kattintson a **Hálózat** > **Munkahely** elemre.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-166">Click **Network** > **Workplace**.</span></span>
3. <span data-ttu-id="5b9b9-167">Kattintson a **Csatlakozás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-167">Click **Join**.</span></span>
4. <span data-ttu-id="5b9b9-168">Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-168">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="5b9b9-169">Kattintson a **Bekapcsolás** elemre.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-169">Click **Turn on**.</span></span>
6. <span data-ttu-id="5b9b9-170">Próbálja meg újra tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-170">Try again tooaccess hello application.</span></span>



#### <a name="add-your-work-or-school-account-toowindows"></a><span data-ttu-id="5b9b9-171">Adja hozzá a munkahelyi vagy iskolai fiók tooWindows</span><span class="sxs-lookup"><span data-stu-id="5b9b9-171">Add your work or school account tooWindows</span></span> 


<span data-ttu-id="5b9b9-172">**Windows 10 évfordulós frissítés (1607-es verzió):**</span><span class="sxs-lookup"><span data-stu-id="5b9b9-172">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="5b9b9-173">Nyissa meg hello **beállítások** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-173">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="5b9b9-174">Kattintson a **Fiókok** > **Hozzáférés munkahelyi vagy iskolai rendszerhez** elemre.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-174">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="5b9b9-175">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-175">Click **Connect**.</span></span>
4. <span data-ttu-id="5b9b9-176">Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-176">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="5b9b9-177">Próbálja meg újra tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-177">Try again tooaccess hello application.</span></span>


<span data-ttu-id="5b9b9-178">**Windows 10, 2015 novemberi frissítés (1511-es verzió):**</span><span class="sxs-lookup"><span data-stu-id="5b9b9-178">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="5b9b9-179">Nyissa meg hello **beállítások** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-179">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="5b9b9-180">Kattintson a **Fiókok** > **Saját fiókok** elemre.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-180">Click **Accounts** > **Your accounts**.</span></span>
3. <span data-ttu-id="5b9b9-181">Kattintson a **Munkahelyi vagy iskolai fiók beállítása** elemre.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-181">Click **Add work or school account**.</span></span>
4. <span data-ttu-id="5b9b9-182">Tooyour szervezet hitelesítéshez, adja meg a többtényezős hitelesítést, amikor a program kéri, majd hajtsa végre hello látható.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-182">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="5b9b9-183">Próbálja meg újra tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5b9b9-183">Try again tooaccess hello application.</span></span>





## <a name="next-steps"></a><span data-ttu-id="5b9b9-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5b9b9-184">Next steps</span></span>
[<span data-ttu-id="5b9b9-185">Azure Active Directory feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="5b9b9-185">Azure Active Directory conditional access</span></span>](active-directory-conditional-access.md)

