---
title: "aaaHow tooconfigure automatikus regisztrálása az Azure Active Directoryval Windows tartományhoz csatlakozó eszközök |} Microsoft Docs"
description: "Állítsa be a tartományhoz csatlakoztatott Windows-eszközök tooregister automatikusan és értesítések nélkül történik az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1d736eba734418231f12e23a8fc1a93405f129c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a><span data-ttu-id="a4832-103">Ismerkedés az Azure Active Directory eszközregisztrációjával</span><span class="sxs-lookup"><span data-stu-id="a4832-103">Get started with Azure Active Directory device registration</span></span>

<span data-ttu-id="a4832-104">Azure Active Directory eszközregisztráció az eszközalapú feltételes hozzáférési forgatókönyvek alapja hello.</span><span class="sxs-lookup"><span data-stu-id="a4832-104">Azure Active Directory device registration is hello foundation for device-based conditional access scenarios.</span></span> <span data-ttu-id="a4832-105">Amikor regisztrál egy eszközt, az Azure Active Directory eszközregisztrációs biztosít hello eszköz történik használt tooauthenticate hello hello felhasználó bejelentkezésekor identitással.</span><span class="sxs-lookup"><span data-stu-id="a4832-105">When a device is registered, Azure Active Directory device registration provides hello device with an identity which is used tooauthenticate hello device when hello user signs in.</span></span> <span data-ttu-id="a4832-106">hello hitelesített eszköz és hello eszköz attribútumai – hello, majd lehet használt tooenforce feltételes hozzáférési házirendek hello felhő és a helyszínen tárolt alkalmazások esetében.</span><span class="sxs-lookup"><span data-stu-id="a4832-106">hello authenticated device, and hello attributes of hello device, can then be used tooenforce conditional access policies for applications that are hosted in hello cloud and on-premises.</span></span>

<span data-ttu-id="a4832-107">Például a Microsoft Intune mobileszköz-lévő eszközattribútumok megoldás együtt, hello eszközattribútumokon az Azure Active Directoryban frissítődik hello eszközzel kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="a4832-107">When combined with a mobile device management(MDM) solution such as Microsoft Intune, hello device attributes in Azure Active Directory are updated with additional information about hello device.</span></span> <span data-ttu-id="a4832-108">Ez lehetővé teszi toocreate feltételes hozzáférési szabályok, amelyeket eszközök toomeet való hozzáférést a biztonsági és megfelelőségi szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="a4832-108">This allows you toocreate conditional access rules that enforce access from devices toomeet your standards for security and compliance.</span></span> <span data-ttu-id="a4832-109">A Microsoft Intune-beli regisztrálásának eszközökön további információkért lásd: eszközök regisztrálása felügyeletre a Intune-ban.</span><span class="sxs-lookup"><span data-stu-id="a4832-109">For more information on enrolling devices in Microsoft Intune, see Enroll devices for management in Intune.</span></span>
<span data-ttu-id="a4832-110">Azure Active Directory eszköz regisztrációs Azure Active Directory Eszközregisztráció által engedélyezett forgatókönyvek támogatja az iOS, Android és Windows eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="a4832-110">Scenarios enabled by Azure Active Directory Device Registration Azure Active Directory Device Registration includes support for iOS, Android, and Windows devices.</span></span> <span data-ttu-id="a4832-111">hello Azure AD Eszközregisztrációt használó egyes forgatókönyvek konkrétabb követelményekkel és platformtámogatással rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="a4832-111">hello individual scenarios that utilize Azure AD Device Registration may have more specific requirements and platform support.</span></span> 

<span data-ttu-id="a4832-112">Ezek a forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="a4832-112">These scenarios are as follows:</span></span>

- <span data-ttu-id="a4832-113">**Feltételes hozzáférés az Office 365-alkalmazások Microsoft Intune-nal:** IT-rendszergazdák építhető feltételes hozzáférési házirendek toosecure vállalati eszközerőforrások, amíg a hello azonos idő engedélyezése az információkkal dolgozó szakemberek a feltételeknek megfelelő eszközökön tooaccess hello szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a4832-113">**Conditional access for Office 365 applications with Microsoft Intune:** IT admins can provision conditional access device policies toosecure corporate resources, while at hello same time allowing information workers on compliant devices tooaccess hello services.</span></span> <span data-ttu-id="a4832-114">További információ: Feltételes hozzáférés eszközházirendjei Office 365-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a4832-114">For more information, see Conditional Access Device Policies for Office 365 services.</span></span>

- <span data-ttu-id="a4832-115">**Feltételes hozzáférés tooapplications, amelyek helyben tárolt:** használhatja a regisztrált eszközök hozzáférési házirendekkel kapcsolatos alkalmazások, amelyek toouse AD FS a Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="a4832-115">**Conditional access tooapplications that are hosted on-premises:** You can use registered devices with access policies for applications that are configured toouse AD FS with Windows Server 2012 R2.</span></span> <span data-ttu-id="a4832-116">A helyszíni feltételes hozzáférés beállításáról további információért lásd: [Helyszíni feltételes hozzáférés beállítása az Azure Active Directory eszközregisztrációjával](active-directory-device-registration-on-premises-setup.md).</span><span class="sxs-lookup"><span data-stu-id="a4832-116">For more information about setting up conditional access for on-premises, see [Setting up On-premises Conditional Access using Azure Active Directory device registration](active-directory-device-registration-on-premises-setup.md).</span></span>

## <a name="setting-up-azure-active-directory-device-registration"></a><span data-ttu-id="a4832-117">Az Azure Active Directory eszközregisztráció beállítása</span><span class="sxs-lookup"><span data-stu-id="a4832-117">Setting up Azure Active Directory Device Registration</span></span>

<span data-ttu-id="a4832-118">toosetup eszközregisztráció, több lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="a4832-118">toosetup device registration, you have multiple options:</span></span>

- <span data-ttu-id="a4832-119">Eszközöket regisztrálhatja, mikor illesztett tooAzure AD-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="a4832-119">Devices can register when joined tooAzure AD domain.</span></span> <span data-ttu-id="a4832-120">A témakörrel kapcsolatban bővebben részletesebb toojoin az Azure AD tartományi felhasználók számára szükséges az Azure AD Join és hello beállításról.</span><span class="sxs-lookup"><span data-stu-id="a4832-120">For more on this topic, you can Learn more about Azure AD Join and hello settings required for users toojoin Azure AD domain.</span></span>

- <span data-ttu-id="a4832-121">Eszközök regisztrálható, amikor a felhasználók hozzáadása a munkahelyi vagy iskolai fiókok tooWindows személyes eszköz, vagy ha a mobil eszközök csatlakoznak a regisztrációs igénylő tooa munkahelyi erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a4832-121">Devices can be registered when users add work or school accounts tooWindows on a personal device or when mobile devices connect tooa work resources requiring registration.</span></span> <span data-ttu-id="a4832-122">tooensure, engedélyeznie kell az Azure AD Eszközregisztrációját hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="a4832-122">tooensure this, you must enable Azure AD Device Registration in hello Azure Portal.</span></span> 

- <span data-ttu-id="a4832-123">Eszközök automatikus regisztrálásának használatával hagyományos tartományhoz csatlakozó gépek regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="a4832-123">Devices can be registered using automatic device registration for traditional domain-joined machines.</span></span> <span data-ttu-id="a4832-124">tooensure, először a telepítő az Azure AD Connectet kell az automatikus eszközregisztráció folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="a4832-124">tooensure this, you must first Setup Azure AD Connect before you continue with automatic device registration.</span></span>

<span data-ttu-id="a4832-125">Legújabb útmutatásért lásd: [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="a4832-125">For latest instructions, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>  
<span data-ttu-id="a4832-126">Is tekintse át és engedélyezheti vagy letilthatja az Azure Active Directory felügyeleti portálon hello regisztrált eszközöket.</span><span class="sxs-lookup"><span data-stu-id="a4832-126">You can also review and enable or disable registered devices using hello Administrator Portal in Azure Active Directory.</span></span>

## <a name="enable-hello-azure-active-directory-device-registration-service"></a><span data-ttu-id="a4832-127">Hello Azure Active Directory eszközregisztrációs szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a4832-127">Enable hello Azure Active Directory device registration service</span></span>

<span data-ttu-id="a4832-128">**tooenable hello Azure Active Directory eszközregisztrációs szolgáltatás:**</span><span class="sxs-lookup"><span data-stu-id="a4832-128">**tooenable hello Azure Active Directory device registration service:**</span></span>

1.  <span data-ttu-id="a4832-129">Jelentkezzen be toohello Microsoft Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a4832-129">Sign in toohello Microsoft Azure portal as administrator.</span></span>

2.  <span data-ttu-id="a4832-130">Hello bal oldali ablaktáblában jelölje ki **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a4832-130">On hello left pane, select **Active Directory**.</span></span>

3.  <span data-ttu-id="a4832-131">Hello könyvtár lapján válassza ki a címtárát.</span><span class="sxs-lookup"><span data-stu-id="a4832-131">On hello Directory tab, select your directory.</span></span>

4.  <span data-ttu-id="a4832-132">Kattintson a **Configure** (Konfigurálás) elemre.</span><span class="sxs-lookup"><span data-stu-id="a4832-132">Click **Configure**.</span></span>

5.  <span data-ttu-id="a4832-133">Görgessen túl**eszközök**.</span><span class="sxs-lookup"><span data-stu-id="a4832-133">Scroll too**Devices**.</span></span>

6.  <span data-ttu-id="a4832-134">A felhasználók számára az összes lehetséges, hogy REGISZTRÁLJÁK az ESZKÖZEIKET az AZURE ad-val.</span><span class="sxs-lookup"><span data-stu-id="a4832-134">Select ALL for USERS MAY REGISTER THEIR DEVICES WITH AZURE AD.</span></span>

7.  <span data-ttu-id="a4832-135">Válassza ki hello maximális számát, az eszközök felhasználónként tooauthorize szeretné.</span><span class="sxs-lookup"><span data-stu-id="a4832-135">Select hello maximum number of devices you want tooauthorize per user.</span></span>

<span data-ttu-id="a4832-136">az Office 365 a Microsoft Intune- vagy mobileszköz-kezelés hello beléptetési eszközregisztráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="a4832-136">hello enrollment with Microsoft Intune or Mobile Device Management for Office 365 requires device registration.</span></span> <span data-ttu-id="a4832-137">Ha konfigurálta a szolgáltatások valamelyikét **összes** van kiválasztva, és **NONE** le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="a4832-137">If you have configured either of these services, **ALL** is selected and **NONE** is disabled.</span></span> <span data-ttu-id="a4832-138">Győződjön meg arról, hogy azok helyesen vannak konfigurálva, és rendelkezik a megfelelő licencelési hello.</span><span class="sxs-lookup"><span data-stu-id="a4832-138">Please ensure that they are configured correctly and have hello appropriate licensing.</span></span>

<span data-ttu-id="a4832-139">Alapértelmezés szerint a kéttényezős hitelesítés hello szolgáltatás nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="a4832-139">By default, two-factor authentication is not enabled for hello service.</span></span> <span data-ttu-id="a4832-140">De a kéttényezős hitelesítés kiválasztása javasolt az eszközök regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="a4832-140">However, two-factor authentication is recommended when registering a device.</span></span>

- <span data-ttu-id="a4832-141">Mielőtt kéttényezős hitelesítést igénylő ezt a szolgáltatást, egy kéttényezős hitelesítési szolgáltató konfigurálása az Azure Active Directoryban és el kell a felhasználói fiókok konfigurálása a multi-factor Authentication, további információ: a multi-factor Authentication felvétele az Active Directory tooAzure</span><span class="sxs-lookup"><span data-stu-id="a4832-141">Before requiring two-factor authentication for this service, you must configure a two-factor authentication provider in Azure Active Directory and configure your user accounts for Multi-Factor Authentication, see Adding Multi-Factor Authentication tooAzure Active Directory</span></span>

- <span data-ttu-id="a4832-142">Ha a Windows Server 2012 R2 AD FS használ, akkor egy kéttényezős hitelesítési modult az AD FS konfigurálása, tekintse meg a multi-factor Authentication használata az Active Directory összevonási szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a4832-142">If you are using AD FS with Windows Server 2012 R2, you must configure a two-factor authentication module in AD FS, see Using Multi-Factor Authentication with Active Directory Federation Services.</span></span>

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a><span data-ttu-id="a4832-143">Eszközobjektumok megtekintése és kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="a4832-143">View and manage device objects in Azure Active Directory</span></span>

<span data-ttu-id="a4832-144">Hello Azure felügyeleti portálról megtekintése, letilthatók, és feloldhatja az eszközök blokkolását.</span><span class="sxs-lookup"><span data-stu-id="a4832-144">From hello Azure Administrator portal, you can view, block, and unblock devices.</span></span> <span data-ttu-id="a4832-145">Blokkolt eszközök már nem fog rendelkezni, amelyek csak regisztrált eszközök konfigurált tooallow hozzáférés tooapplications.</span><span class="sxs-lookup"><span data-stu-id="a4832-145">A device that is blocked will no longer have access tooapplications that are configured tooallow only registered devices.</span></span>

<span data-ttu-id="a4832-146">**tooview és eszközobjektumot az Azure Active Directoryban kezelése:**</span><span class="sxs-lookup"><span data-stu-id="a4832-146">**tooview and manage device objects in Azure Active Directory:**</span></span>
 
1.  <span data-ttu-id="a4832-147">Jelentkezzen be toohello Microsoft Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a4832-147">Log on toohello Microsoft Azure portal as administrator.</span></span>

2.  <span data-ttu-id="a4832-148">Hello bal oldali ablaktáblában jelölje ki **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a4832-148">On hello left pane, select **Active Directory**.</span></span>

3.  <span data-ttu-id="a4832-149">Válassza ki a címtárát.</span><span class="sxs-lookup"><span data-stu-id="a4832-149">Select your directory.</span></span>

4.  <span data-ttu-id="a4832-150">Válassza ki **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="a4832-150">Select **Users**.</span></span> 

5.  <span data-ttu-id="a4832-151">Kattintson a kívánt toosee hello eszközök hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a4832-151">Click hello user for which you want toosee hello devices.</span></span>

6.  <span data-ttu-id="a4832-152">Válassza ki **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="a4832-152">Select **Devices**.</span></span>

7.  <span data-ttu-id="a4832-153">Válassza ki **regisztrált eszközök**.</span><span class="sxs-lookup"><span data-stu-id="a4832-153">Select **Registered Devices**.</span></span>

<span data-ttu-id="a4832-154">Most tekintse át, letiltása, vagy feloldása hello felhasználó regisztrált eszközöket.</span><span class="sxs-lookup"><span data-stu-id="a4832-154">Now, you can review, block, or unblock hello user's registered devices.</span></span>
<span data-ttu-id="a4832-155">A helyszínen a tartományhoz, és automatikusan regisztrált Windows 10-eszközök hello felhasználók lapján nem jelennek meg. Használja a Get-MsolDevice PowerShell-parancs toofind hello minden vállalati eszköz.</span><span class="sxs-lookup"><span data-stu-id="a4832-155">Windows 10 devices that are on-premises domain-joined and automatically registered do not appear under hello Users tab. Please use hello Get-MsolDevice PowerShell command toofind all your enterprise devices.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="a4832-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a4832-156">Next steps</span></span>

<span data-ttu-id="a4832-157">toosetup automatikus eszközregisztráció, lásd: [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="a4832-157">toosetup automated device registration, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>


