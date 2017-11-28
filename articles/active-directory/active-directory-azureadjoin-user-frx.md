---
title: "az Azure AD a telepítés során egy új eszközt aaaSet |} Microsoft Docs"
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
ms.openlocfilehash: 6afce4be7f084f1956a6f9dbddaa8def0605956d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a><span data-ttu-id="79b6e-103">A telepítés során az Azure AD-val új eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="79b6e-103">Set up a new device with Azure AD during Setup</span></span>
<span data-ttu-id="79b6e-104">A Windows 10 felhasználók úgy csatlakozhatnak a saját eszközök tooAzure Active Directory (Azure AD) a hello kezdőélmény (FRX).</span><span class="sxs-lookup"><span data-stu-id="79b6e-104">In Windows 10, users can join their devices tooAzure Active Directory (Azure AD) in hello first-run experience (FRX).</span></span> <span data-ttu-id="79b6e-105">Ez lehetővé teszi a szervezetek toodistribute légmentes fóliacsomagolásúak eszközök tootheir az alkalmazottak vagy a diákok, vagy azokat válassza ki a saját eszköz (CYOD).</span><span class="sxs-lookup"><span data-stu-id="79b6e-105">This allows organizations toodistribute shrink-wrapped devices tootheir employees or students, or let them choose their own devices (CYOD).</span></span>
<span data-ttu-id="79b6e-106">Ha Windows 10 Professional vagy Windows 10 Enterprise kiadás telepítve van az eszközön, a hello alapértelmezett toohello telepítési folyamat a vállalat által birtokolt eszközök felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="79b6e-106">If either Windows 10 Professional or Windows 10 Enterprise editions is installed on a device, hello experience defaults toohello setup process for company-owned devices.</span></span>

## <a name="toojoin-a-device-tooazure-ad"></a><span data-ttu-id="79b6e-107">egy eszköz tooAzure AD toojoin</span><span class="sxs-lookup"><span data-stu-id="79b6e-107">toojoin a device tooAzure AD</span></span>
1. <span data-ttu-id="79b6e-108">Kapcsolja be az új eszköz és a telepítési folyamat hello, megjelennie hello **készen első** üzenet.</span><span class="sxs-lookup"><span data-stu-id="79b6e-108">When you turn on your new device and start hello setup process, you should see hello  **Getting Ready** message.</span></span> <span data-ttu-id="79b6e-109">Hajtsa végre a hello kér tooset be az eszközt.</span><span class="sxs-lookup"><span data-stu-id="79b6e-109">Follow hello prompts tooset up your device.</span></span>
2. <span data-ttu-id="79b6e-110">Indítsa el a területi és nyelvi beállítások testreszabása.</span><span class="sxs-lookup"><span data-stu-id="79b6e-110">Start by customizing your region and language.</span></span> <span data-ttu-id="79b6e-111">Majd fogadja el a hello Microsoft szoftverlicenc-szerződést.</span><span class="sxs-lookup"><span data-stu-id="79b6e-111">Then accept hello Microsoft Software License Terms.</span></span>
   <span data-ttu-id="79b6e-112">![A régió testreszabása](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span><span class="sxs-lookup"><span data-stu-id="79b6e-112">![Customize for your region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)</span></span>
3. <span data-ttu-id="79b6e-113">Válassza ki a hello hálózati toouse toohello Internet csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="79b6e-113">Select hello network you want toouse for connecting toohello Internet.</span></span>
4. <span data-ttu-id="79b6e-114">Adja meg, hogy egy személyes vagy vállalati tulajdonú eszköz használata.</span><span class="sxs-lookup"><span data-stu-id="79b6e-114">Select whether you're using a personal device or a company-owned device.</span></span> <span data-ttu-id="79b6e-115">Ha a vállalat tulajdonában, kattintson a **az eszköz tartozik toomy szervezet**.</span><span class="sxs-lookup"><span data-stu-id="79b6e-115">If it's company-owned, click **This device belongs toomy organization**.</span></span> <span data-ttu-id="79b6e-116">Ekkor elindul a hello Azure AD Join élmény.</span><span class="sxs-lookup"><span data-stu-id="79b6e-116">This starts hello Azure AD Join experience.</span></span> <span data-ttu-id="79b6e-117">Az alábbiakban látható egy képernyőt, hogy látni fogja, ha a Windows 10 Professional használ.</span><span class="sxs-lookup"><span data-stu-id="79b6e-117">Following is a screen that you'll see if you're using Windows 10 Professional.</span></span>
   <span data-ttu-id="79b6e-118"><center>
   ![Ez a számítógép képernyő tulajdonosára](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span><span class="sxs-lookup"><span data-stu-id="79b6e-118"><center>
![Who owns this PC screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)</span></span>
5. <span data-ttu-id="79b6e-119">Adja meg a hello hitelesítő adatokkal, amelyeket tooyou a szervezet által.</span><span class="sxs-lookup"><span data-stu-id="79b6e-119">Enter hello credentials that were provided tooyou by your organization.</span></span>
   <span data-ttu-id="79b6e-120"><center>
   ![Bejelentkezési képernyő](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span><span class="sxs-lookup"><span data-stu-id="79b6e-120"><center>
![Sign-in screen](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</span></span>
6. <span data-ttu-id="79b6e-121">Miután megadta a felhasználónevét, a megfelelő bérlő az Azure ad-ben található.</span><span class="sxs-lookup"><span data-stu-id="79b6e-121">After you have entered your user name, a matching tenant is located in Azure AD.</span></span> <span data-ttu-id="79b6e-122">Ha egy összevont tartományban vannak, fogja átirányított tooyour helyszíni Secure Token Service (STS) kiszolgáló – például az Active Directory összevonási szolgáltatások (AD FS).</span><span class="sxs-lookup"><span data-stu-id="79b6e-122">If you are in a federated domain, you will be redirected tooyour on-premises Secure Token Service (STS) server--for example, Active Directory Federation Services (AD FS).</span></span>
7. <span data-ttu-id="79b6e-123">Ha egy felhasználó egy nem összevont tartományban, adja meg a hitelesítő adatok közvetlenül az Azure AD által szolgáltatott lap hello.</span><span class="sxs-lookup"><span data-stu-id="79b6e-123">If you are a user in a non-federated domain, enter your credentials directly on hello Azure AD-hosted page.</span></span> <span data-ttu-id="79b6e-124">Vállalati arculat megjelenítése be lett állítva, ha akkor tekintse meg a vállalati embléma és az is támogatja a szöveget.</span><span class="sxs-lookup"><span data-stu-id="79b6e-124">If company branding was configured, you will also see your organization’s logo and support text.</span></span>
8. <span data-ttu-id="79b6e-125">A multi-factor authentication kihívást kéri.</span><span class="sxs-lookup"><span data-stu-id="79b6e-125">You're prompted for a multi-factor authentication challenge.</span></span> <span data-ttu-id="79b6e-126">Ez a probléma a rendszergazdák által konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="79b6e-126">This challenge is configurable by an IT administrator.</span></span>
9. <span data-ttu-id="79b6e-127">Az Azure AD ellenőrzi, hogy a felhasználó/eszköz igényli-e a mobileszköz-kezelés regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="79b6e-127">Azure AD checks whether this user/device requires enrollment in mobile device management.</span></span>
10. <span data-ttu-id="79b6e-128">A Windows hello eszköz regisztrálja hello munkahely címtárában az Azure AD, és regisztrálja a mobileszköz-kezelés, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="79b6e-128">Windows registers hello device in hello organization’s directory in Azure AD and enrolls it in mobile device management, if appropriate.</span></span>
11. <span data-ttu-id="79b6e-129">Ha egy felügyelt felhasználók Windows viszi toohello asztali hello automatikus bejelentkezési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="79b6e-129">If you are a managed user, Windows takes you toohello desktop through hello automatic sign-in process.</span></span>
12. <span data-ttu-id="79b6e-130">Ha egy összevont felhasználó, akkor a rendszer átirányítja a Windows-bejelentkezés toohello képernyőn tooenter a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="79b6e-130">If you are a federated user, you are directed toohello Windows sign-in screen tooenter your credentials.</span></span>

> [!NOTE]
> <span data-ttu-id="79b6e-131">A hello Windows a helyi Windows Server Active Directory-tartományhoz való csatlakozás out-of-box élmény nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="79b6e-131">Joining an on-premises Windows Server Active Directory domain in hello Windows out-of-box experience is not supported.</span></span> <span data-ttu-id="79b6e-132">Ezért, ha azt tervezi, hogy a számítógép tooa tartomány toojoin, ki kell jelölni hello hivatkozás **állítsa be a Windowst helyi fiókkal** helyette.</span><span class="sxs-lookup"><span data-stu-id="79b6e-132">Therefore, if you plan toojoin a computer tooa domain, you should select hello link **Set up Windows with a local account** instead.</span></span> <span data-ttu-id="79b6e-133">Majd csatlakozhat hello tartomány hello-beállítások a számítógépen, mielőtt ezt.</span><span class="sxs-lookup"><span data-stu-id="79b6e-133">You can then join hello domain from hello settings on your computer as you’ve done before.</span></span>
> 
> 

## <a name="additional-information"></a><span data-ttu-id="79b6e-134">További információ</span><span class="sxs-lookup"><span data-stu-id="79b6e-134">Additional information</span></span>
* [<span data-ttu-id="79b6e-135">Windows 10 Enterprise hello: módon toouse eszközök munkára</span><span class="sxs-lookup"><span data-stu-id="79b6e-135">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="79b6e-136">Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="79b6e-136">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="79b6e-137">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="79b6e-137">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="79b6e-138">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="79b6e-138">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="79b6e-139">Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="79b6e-139">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="79b6e-140">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="79b6e-140">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

