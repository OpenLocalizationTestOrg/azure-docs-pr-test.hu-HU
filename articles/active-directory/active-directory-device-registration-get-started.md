---
title: "Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása konfigurálása az Azure Active Directoryval |} Microsoft Docs"
description: "A tartományhoz csatlakoztatott Windows-eszközök beállítása automatikusan és a csendes regisztrálása az Azure Active Directoryban."
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
ms.openlocfilehash: 38750050e8525272079e1f3a5509da1e8f0a557b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a><span data-ttu-id="b8952-103">Ismerkedés az Azure Active Directory eszközregisztrációjával</span><span class="sxs-lookup"><span data-stu-id="b8952-103">Get started with Azure Active Directory device registration</span></span>

<span data-ttu-id="b8952-104">Az Azure Active Directory eszközregisztráció az eszközalapú feltételes hozzáférési forgatókönyvek alapja.</span><span class="sxs-lookup"><span data-stu-id="b8952-104">Azure Active Directory device registration is the foundation for device-based conditional access scenarios.</span></span> <span data-ttu-id="b8952-105">Amikor regisztrál egy eszközt, az Azure Active Directory eszközregisztráció egy identitással látja el az eszközt, amely az eszköz hitelesítésére használható a felhasználó bejelentkezésekor.</span><span class="sxs-lookup"><span data-stu-id="b8952-105">When a device is registered, Azure Active Directory device registration provides the device with an identity which is used to authenticate the device when the user signs in.</span></span> <span data-ttu-id="b8952-106">A hitelesített eszköz és az eszköz attribútumai ezután a feltételes hozzáférési házirendek betartatásához használhatók a felhőben és a helyszínen tárolt alkalmazások esetében.</span><span class="sxs-lookup"><span data-stu-id="b8952-106">The authenticated device, and the attributes of the device, can then be used to enforce conditional access policies for applications that are hosted in the cloud and on-premises.</span></span>

<span data-ttu-id="b8952-107">Amikor mobileszköz-kezelési (MDM) megoldással, például a Microsoft Intune-nal ötvözi, frissülnek az Azure Active Directoryban lévő eszközattribútumok az eszköz további információival.</span><span class="sxs-lookup"><span data-stu-id="b8952-107">When combined with a mobile device management(MDM) solution such as Microsoft Intune, the device attributes in Azure Active Directory are updated with additional information about the device.</span></span> <span data-ttu-id="b8952-108">Ez lehetővé teszi további feltételes hozzáférési szabályok létrehozását, amelyek arra kényszerítik az eszközhozzáféréseket, hogy megfeleljenek a biztonsági és megfelelőségi szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="b8952-108">This allows you to create conditional access rules that enforce access from devices to meet your standards for security and compliance.</span></span> <span data-ttu-id="b8952-109">A Microsoft Intune-beli regisztrálásának eszközökön további információkért lásd: eszközök regisztrálása felügyeletre a Intune-ban.</span><span class="sxs-lookup"><span data-stu-id="b8952-109">For more information on enrolling devices in Microsoft Intune, see Enroll devices for management in Intune.</span></span>
<span data-ttu-id="b8952-110">Azure Active Directory eszköz regisztrációs Azure Active Directory Eszközregisztráció által engedélyezett forgatókönyvek támogatja az iOS, Android és Windows eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="b8952-110">Scenarios enabled by Azure Active Directory Device Registration Azure Active Directory Device Registration includes support for iOS, Android, and Windows devices.</span></span> <span data-ttu-id="b8952-111">Az Azure AD eszközregisztrációt használó egyes forgatókönyvek konkrétabb követelményekkel és platformtámogatással rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="b8952-111">The individual scenarios that utilize Azure AD Device Registration may have more specific requirements and platform support.</span></span> 

<span data-ttu-id="b8952-112">Ezek a forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="b8952-112">These scenarios are as follows:</span></span>

- <span data-ttu-id="b8952-113">**Feltételes hozzáférés az Office 365-alkalmazások Microsoft Intune-nal:** IT-rendszergazdák hozhat létre feltételes hozzáférési szabályzatok, lehetővé téve az információkkal dolgozó szakemberek a szolgáltatásokat a feltételeknek megfelelő eszközökön a vállalati erőforrások biztonságossá tételére.</span><span class="sxs-lookup"><span data-stu-id="b8952-113">**Conditional access for Office 365 applications with Microsoft Intune:** IT admins can provision conditional access device policies to secure corporate resources, while at the same time allowing information workers on compliant devices to access the services.</span></span> <span data-ttu-id="b8952-114">További információ: Feltételes hozzáférés eszközházirendjei Office 365-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b8952-114">For more information, see Conditional Access Device Policies for Office 365 services.</span></span>

- <span data-ttu-id="b8952-115">**Feltételes hozzáférés az alkalmazásokhoz a helyszínen szolgáltatott:** használhat regisztrált eszközöket hozzáférési házirendekkel alkalmazásokat, amelyek a Windows Server 2012 R2 AD FS használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b8952-115">**Conditional access to applications that are hosted on-premises:** You can use registered devices with access policies for applications that are configured to use AD FS with Windows Server 2012 R2.</span></span> <span data-ttu-id="b8952-116">A helyszíni feltételes hozzáférés beállításáról további információért lásd: [Helyszíni feltételes hozzáférés beállítása az Azure Active Directory eszközregisztrációjával](active-directory-device-registration-on-premises-setup.md).</span><span class="sxs-lookup"><span data-stu-id="b8952-116">For more information about setting up conditional access for on-premises, see [Setting up On-premises Conditional Access using Azure Active Directory device registration](active-directory-device-registration-on-premises-setup.md).</span></span>

## <a name="setting-up-azure-active-directory-device-registration"></a><span data-ttu-id="b8952-117">Az Azure Active Directory eszközregisztráció beállítása</span><span class="sxs-lookup"><span data-stu-id="b8952-117">Setting up Azure Active Directory Device Registration</span></span>

<span data-ttu-id="b8952-118">Eszközregisztráció beállítása, hogy több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="b8952-118">To setup device registration, you have multiple options:</span></span>

- <span data-ttu-id="b8952-119">Eszközöket regisztrálhatja, amikor az Azure AD-tartományhoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b8952-119">Devices can register when joined to Azure AD domain.</span></span> <span data-ttu-id="b8952-120">A témakörrel kapcsolatban bővebben részletesebb Azure AD Join és a szükséges felhasználói beállításokat az Azure AD-tartományhoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="b8952-120">For more on this topic, you can Learn more about Azure AD Join and the settings required for users to join Azure AD domain.</span></span>

- <span data-ttu-id="b8952-121">Ha a felhasználók fel a munkahelyi vagy iskolai fiókok Windows személyes eszköz esetén a mobileszközök csatlakozni a munkahelyi erőforrásokhoz regisztrációs igénylő eszközöket regisztrálni lehet.</span><span class="sxs-lookup"><span data-stu-id="b8952-121">Devices can be registered when users add work or school accounts to Windows on a personal device or when mobile devices connect to a work resources requiring registration.</span></span> <span data-ttu-id="b8952-122">Győződjön meg arról, ez, engedélyeznie kell az Azure AD Eszközregisztrációját az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b8952-122">To ensure this, you must enable Azure AD Device Registration in the Azure Portal.</span></span> 

- <span data-ttu-id="b8952-123">Eszközök automatikus regisztrálásának használatával hagyományos tartományhoz csatlakozó gépek regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="b8952-123">Devices can be registered using automatic device registration for traditional domain-joined machines.</span></span> <span data-ttu-id="b8952-124">Ennek biztosítására kell első beállítása az Azure AD Connectet automatikus eszközregisztráció folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="b8952-124">To ensure this, you must first Setup Azure AD Connect before you continue with automatic device registration.</span></span>

<span data-ttu-id="b8952-125">Legújabb útmutatásért lásd: [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="b8952-125">For latest instructions, see [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>  
<span data-ttu-id="b8952-126">Is tekintse át és engedélyezheti vagy letilthatja az Azure Active Directory felügyeleti portálján regisztrált eszközöket.</span><span class="sxs-lookup"><span data-stu-id="b8952-126">You can also review and enable or disable registered devices using the Administrator Portal in Azure Active Directory.</span></span>

## <a name="enable-the-azure-active-directory-device-registration-service"></a><span data-ttu-id="b8952-127">Az Azure Active Directory eszközregisztrációs szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b8952-127">Enable the Azure Active Directory device registration service</span></span>

<span data-ttu-id="b8952-128">**Az Azure Active Directory eszközregisztrációs szolgáltatás engedélyezése:**</span><span class="sxs-lookup"><span data-stu-id="b8952-128">**To enable the Azure Active Directory device registration service:**</span></span>

1.  <span data-ttu-id="b8952-129">Jelentkezzen be a Microsoft Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b8952-129">Sign in to the Microsoft Azure portal as administrator.</span></span>

2.  <span data-ttu-id="b8952-130">A bal oldali panelen válassza az **Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b8952-130">On the left pane, select **Active Directory**.</span></span>

3.  <span data-ttu-id="b8952-131">A könyvtár lapon válassza ki a címtárát.</span><span class="sxs-lookup"><span data-stu-id="b8952-131">On the Directory tab, select your directory.</span></span>

4.  <span data-ttu-id="b8952-132">Kattintson a **Configure** (Konfigurálás) elemre.</span><span class="sxs-lookup"><span data-stu-id="b8952-132">Click **Configure**.</span></span>

5.  <span data-ttu-id="b8952-133">Görgessen **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="b8952-133">Scroll to **Devices**.</span></span>

6.  <span data-ttu-id="b8952-134">A felhasználók számára az összes lehetséges, hogy REGISZTRÁLJÁK az ESZKÖZEIKET az AZURE ad-val.</span><span class="sxs-lookup"><span data-stu-id="b8952-134">Select ALL for USERS MAY REGISTER THEIR DEVICES WITH AZURE AD.</span></span>

7.  <span data-ttu-id="b8952-135">Válassza ki a felhasználónként regisztrálni kívánt eszközök maximális számát.</span><span class="sxs-lookup"><span data-stu-id="b8952-135">Select the maximum number of devices you want to authorize per user.</span></span>

<span data-ttu-id="b8952-136">Az Office 365 a Microsoft Intune-vagy mobileszköz-kezelés regisztrációs eszközregisztráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="b8952-136">The enrollment with Microsoft Intune or Mobile Device Management for Office 365 requires device registration.</span></span> <span data-ttu-id="b8952-137">Ha konfigurálta a szolgáltatások valamelyikét **összes** van kiválasztva, és **NONE** le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="b8952-137">If you have configured either of these services, **ALL** is selected and **NONE** is disabled.</span></span> <span data-ttu-id="b8952-138">Győződjön meg arról, hogy azok helyesen vannak konfigurálva, és rendelkezik a megfelelő licencelés.</span><span class="sxs-lookup"><span data-stu-id="b8952-138">Please ensure that they are configured correctly and have the appropriate licensing.</span></span>

<span data-ttu-id="b8952-139">Alapértelmezés szerint a kéttényezős hitelesítés nem engedélyezett a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b8952-139">By default, two-factor authentication is not enabled for the service.</span></span> <span data-ttu-id="b8952-140">De a kéttényezős hitelesítés kiválasztása javasolt az eszközök regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="b8952-140">However, two-factor authentication is recommended when registering a device.</span></span>

- <span data-ttu-id="b8952-141">Mielőtt kéttényezős hitelesítést igénylő ezt a szolgáltatást, kell konfigurálni egy kéttényezős hitelesítésszolgáltatót az Azure Active Directoryban és a felhasználói fiókok konfigurálása a multi-factor Authentication. további tekintse meg a multi-factor Authentication felvétele az Azure Active Directoryhoz</span><span class="sxs-lookup"><span data-stu-id="b8952-141">Before requiring two-factor authentication for this service, you must configure a two-factor authentication provider in Azure Active Directory and configure your user accounts for Multi-Factor Authentication, see Adding Multi-Factor Authentication to Azure Active Directory</span></span>

- <span data-ttu-id="b8952-142">Ha a Windows Server 2012 R2 AD FS használ, akkor egy kéttényezős hitelesítési modult az AD FS konfigurálása, tekintse meg a multi-factor Authentication használata az Active Directory összevonási szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b8952-142">If you are using AD FS with Windows Server 2012 R2, you must configure a two-factor authentication module in AD FS, see Using Multi-Factor Authentication with Active Directory Federation Services.</span></span>

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a><span data-ttu-id="b8952-143">Eszközobjektumok megtekintése és kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="b8952-143">View and manage device objects in Azure Active Directory</span></span>

<span data-ttu-id="b8952-144">Az Azure felügyeleti portálon megtekintheti, blokkolhatja és feloldhatja az eszközök blokkolását.</span><span class="sxs-lookup"><span data-stu-id="b8952-144">From the Azure Administrator portal, you can view, block, and unblock devices.</span></span> <span data-ttu-id="b8952-145">A blokkolt eszközök már nem érhetik el azon alkalmazásokat, amelyek csak regisztrált eszközök engedélyezésére vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b8952-145">A device that is blocked will no longer have access to applications that are configured to allow only registered devices.</span></span>

<span data-ttu-id="b8952-146">**Megtekintheti és kezelheti az eszközt az Azure Active Directory-objektumokat:**</span><span class="sxs-lookup"><span data-stu-id="b8952-146">**To view and manage device objects in Azure Active Directory:**</span></span>
 
1.  <span data-ttu-id="b8952-147">Jelentkezzen be a Microsoft Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b8952-147">Log on to the Microsoft Azure portal as administrator.</span></span>

2.  <span data-ttu-id="b8952-148">A bal oldali panelen válassza az **Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b8952-148">On the left pane, select **Active Directory**.</span></span>

3.  <span data-ttu-id="b8952-149">Válassza ki a címtárát.</span><span class="sxs-lookup"><span data-stu-id="b8952-149">Select your directory.</span></span>

4.  <span data-ttu-id="b8952-150">Válassza ki **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="b8952-150">Select **Users**.</span></span> 

5.  <span data-ttu-id="b8952-151">Kattintson arra a felhasználóra, amelynek meg szeretné tekinteni az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="b8952-151">Click the user for which you want to see the devices.</span></span>

6.  <span data-ttu-id="b8952-152">Válassza ki **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="b8952-152">Select **Devices**.</span></span>

7.  <span data-ttu-id="b8952-153">Válassza ki **regisztrált eszközök**.</span><span class="sxs-lookup"><span data-stu-id="b8952-153">Select **Registered Devices**.</span></span>

<span data-ttu-id="b8952-154">Most tekintse át, letiltása, vagy a felhasználó regisztrált eszközök feloldása.</span><span class="sxs-lookup"><span data-stu-id="b8952-154">Now, you can review, block, or unblock the user's registered devices.</span></span>
<span data-ttu-id="b8952-155">A helyszínen a tartományhoz, és automatikusan regisztrált Windows 10-eszközök nem jelennek meg a felhasználók lapon.</span><span class="sxs-lookup"><span data-stu-id="b8952-155">Windows 10 devices that are on-premises domain-joined and automatically registered do not appear under the Users tab.</span></span> <span data-ttu-id="b8952-156">A Get-MsolDevice PowerShell-parancs segítségével a vállalati eszközök keresése.</span><span class="sxs-lookup"><span data-stu-id="b8952-156">Please use the Get-MsolDevice PowerShell command to find all your enterprise devices.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="b8952-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8952-157">Next steps</span></span>

<span data-ttu-id="b8952-158">Automatikus eszközregisztráció beállítása, lásd: [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="b8952-158">To setup automated device registration, see [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>


