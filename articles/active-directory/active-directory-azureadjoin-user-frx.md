---
title: "A telepítés során az Azure AD-val új eszköz beállítása |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan felhasználók állíthat be az Azure AD Join a first run Experience összetevő során."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 4367f453ef7c794653dfa9f305b137a168e0d207
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a><span data-ttu-id="72049-103">A telepítés során az Azure AD-val új eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="72049-103">Set up a new device with Azure AD during Setup</span></span>
<span data-ttu-id="72049-104">A Windows 10-es felhasználók is csatlakozott az eszközeiket az Azure Active Directory (Azure AD) a kezdőélmény (FRX).</span><span class="sxs-lookup"><span data-stu-id="72049-104">In Windows 10, users can join their devices to Azure Active Directory (Azure AD) in the first-run experience (FRX).</span></span> <span data-ttu-id="72049-105">Ez lehetővé teszi a szervezetek számára, hogy az alkalmazottak vagy az diákok légmentes fóliacsomagolásúak eszközök terjesztése a, illetve hogy azok válassza ki a saját eszköz (CYOD).</span><span class="sxs-lookup"><span data-stu-id="72049-105">This allows organizations to distribute shrink-wrapped devices to their employees or students, or let them choose their own devices (CYOD).</span></span>
<span data-ttu-id="72049-106">Ha Windows 10 Professional vagy Windows 10 Enterprise kiadás telepítve van az eszközön, a felhasználói élmény alapértelmezés szerint a telepítési folyamat a vállalat által birtokolt eszközök.</span><span class="sxs-lookup"><span data-stu-id="72049-106">If either Windows 10 Professional or Windows 10 Enterprise editions is installed on a device, the experience defaults to the setup process for company-owned devices.</span></span>

## <a name="to-join-a-device-to-azure-ad"></a><span data-ttu-id="72049-107">Eszköz csatlakoztatása az Azure AD</span><span class="sxs-lookup"><span data-stu-id="72049-107">To join a device to Azure AD</span></span>
1. <span data-ttu-id="72049-108">Kapcsolja be az új eszközt, és a telepítés megkezdéséhez, láthatja a **készen első** üzenet.</span><span class="sxs-lookup"><span data-stu-id="72049-108">When you turn on your new device and start the setup process, you should see the  **Getting Ready** message.</span></span> <span data-ttu-id="72049-109">Kövesse az utasításokat, az eszköz beállításához.</span><span class="sxs-lookup"><span data-stu-id="72049-109">Follow the prompts to set up your device.</span></span>
2. <span data-ttu-id="72049-110">Indítsa el a területi és nyelvi beállítások testreszabása.</span><span class="sxs-lookup"><span data-stu-id="72049-110">Start by customizing your region and language.</span></span> <span data-ttu-id="72049-111">Majd fogadja el a Microsoft szoftverlicenc-szerződést.</span><span class="sxs-lookup"><span data-stu-id="72049-111">Then accept the Microsoft Software License Terms.</span></span>
   <span data-ttu-id="72049-112">![A régió testreszabása](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span><span class="sxs-lookup"><span data-stu-id="72049-112">![Customize for your region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span></span>
3. <span data-ttu-id="72049-113">Válassza ki a hálózatot, az internethez való kapcsolódáshoz használandó.</span><span class="sxs-lookup"><span data-stu-id="72049-113">Select the network you want to use for connecting to the Internet.</span></span>
4. <span data-ttu-id="72049-114">Adja meg, hogy egy személyes vagy vállalati tulajdonú eszköz használata.</span><span class="sxs-lookup"><span data-stu-id="72049-114">Select whether you're using a personal device or a company-owned device.</span></span> <span data-ttu-id="72049-115">Ha a vállalat tulajdonában, kattintson a **saját szervezethez tartozik az eszköz**.</span><span class="sxs-lookup"><span data-stu-id="72049-115">If it's company-owned, click **This device belongs to my organization**.</span></span> <span data-ttu-id="72049-116">Ezzel elindítja az Azure AD Join élmény.</span><span class="sxs-lookup"><span data-stu-id="72049-116">This starts the Azure AD Join experience.</span></span> <span data-ttu-id="72049-117">Az alábbiakban látható egy képernyőt, hogy látni fogja, ha a Windows 10 Professional használ.</span><span class="sxs-lookup"><span data-stu-id="72049-117">Following is a screen that you'll see if you're using Windows 10 Professional.</span></span>
   <span data-ttu-id="72049-118"><center>
   ![Ez a számítógép képernyő tulajdonosára](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span><span class="sxs-lookup"><span data-stu-id="72049-118"><center>
![Who owns this PC screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span></span>
5. <span data-ttu-id="72049-119">Adja meg a hitelesítő adatokkal, amelyeket Ön számára a szervezet által.</span><span class="sxs-lookup"><span data-stu-id="72049-119">Enter the credentials that were provided to you by your organization.</span></span>
   <span data-ttu-id="72049-120"><center>
   ![Bejelentkezési képernyő](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span><span class="sxs-lookup"><span data-stu-id="72049-120"><center>
![Sign-in screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span></span>
6. <span data-ttu-id="72049-121">Miután megadta a felhasználónevét, a megfelelő bérlő az Azure ad-ben található.</span><span class="sxs-lookup"><span data-stu-id="72049-121">After you have entered your user name, a matching tenant is located in Azure AD.</span></span> <span data-ttu-id="72049-122">Ha egy összevont tartományban vannak, a helyszíni Secure Token Service (STS) kiszolgálóhoz – például az Active Directory összevonási szolgáltatások (AD FS) irányítja.</span><span class="sxs-lookup"><span data-stu-id="72049-122">If you are in a federated domain, you will be redirected to your on-premises Secure Token Service (STS) server--for example, Active Directory Federation Services (AD FS).</span></span>
7. <span data-ttu-id="72049-123">Ha egy felhasználó egy nem összevont tartományban, adja meg a hitelesítő adatok közvetlenül az Azure AD által szolgáltatott lapon.</span><span class="sxs-lookup"><span data-stu-id="72049-123">If you are a user in a non-federated domain, enter your credentials directly on the Azure AD-hosted page.</span></span> <span data-ttu-id="72049-124">Vállalati arculat megjelenítése be lett állítva, ha akkor tekintse meg a vállalati embléma és az is támogatja a szöveget.</span><span class="sxs-lookup"><span data-stu-id="72049-124">If company branding was configured, you will also see your organization’s logo and support text.</span></span>
8. <span data-ttu-id="72049-125">A multi-factor authentication kihívást kéri.</span><span class="sxs-lookup"><span data-stu-id="72049-125">You're prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="72049-126">Ez a probléma a rendszergazdák által konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="72049-126">This challenge is configurable by an IT administrator.</span></span>
9. <span data-ttu-id="72049-127">Az Azure AD ellenőrzi, hogy a felhasználó/eszköz igényli-e a mobileszköz-kezelés regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="72049-127">Azure AD checks whether this user/device requires enrollment in mobile device management.</span></span>
10. <span data-ttu-id="72049-128">A Windows regisztrálja az eszközt a szervezet címtárához az Azure AD-ben, és regisztrálja a mobileszköz-kezelés, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="72049-128">Windows registers the device in the organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
11. <span data-ttu-id="72049-129">Ha egy felügyelt felhasználók Windows viszi az automatikus bejelentkezési folyamat során az asztalon.</span><span class="sxs-lookup"><span data-stu-id="72049-129">If you are a managed user, Windows takes you to the desktop through the automatic sign-in process.</span></span>
12. <span data-ttu-id="72049-130">Ha egy összevont felhasználó, a rendszer irányítja a Windows bejelentkezési képernyő, a hitelesítő adatok megadását.</span><span class="sxs-lookup"><span data-stu-id="72049-130">If you are a federated user, you are directed to the Windows sign-in screen to enter your credentials.</span></span>

> [!NOTE]
> <span data-ttu-id="72049-131">A Windows-out-of-box élményt nyújt a helyi Windows Server Active Directory-tartományhoz való csatlakozás nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="72049-131">Joining an on-premises Windows Server Active Directory domain in the Windows out-of-box experience is not supported.</span></span> <span data-ttu-id="72049-132">Ezért, ha azt tervezi, a számítógép csatlakoztatása a tartományhoz, ki kell jelölni a hivatkozás **állítsa be a Windowst helyi fiókkal** helyette.</span><span class="sxs-lookup"><span data-stu-id="72049-132">Therefore, if you plan to join a computer to a domain, you should select the link **Set up Windows with a local account** instead.</span></span> <span data-ttu-id="72049-133">Majd csatlakozhat a tartományhoz, a beállítások a számítógépen, mielőtt ezt.</span><span class="sxs-lookup"><span data-stu-id="72049-133">You can then join the domain from the settings on your computer as you’ve done before.</span></span>
> 
> 

## <a name="additional-information"></a><span data-ttu-id="72049-134">További információ</span><span class="sxs-lookup"><span data-stu-id="72049-134">Additional information</span></span>
* [<span data-ttu-id="72049-135">Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata</span><span class="sxs-lookup"><span data-stu-id="72049-135">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="72049-136">A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül</span><span class="sxs-lookup"><span data-stu-id="72049-136">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="72049-137">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="72049-137">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="72049-138">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="72049-138">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="72049-139">Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="72049-139">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="72049-140">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="72049-140">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

