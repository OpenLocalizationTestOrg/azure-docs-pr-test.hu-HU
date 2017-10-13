---
title: "aaaHow toouse Azure RemoteApp az Office 365 felhasználói fiókjainak |} Microsoft Docs"
description: "Ismerje meg, hogy az Office 365 felhasználói fiókkal rendelkező Azure RemoteApp toouse"
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: aba70fd3-60b0-4b44-9321-1ab18760ccd5
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d2dbed2a6838adf9bb0f7508eb7dcecb0a74a62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-remoteapp-with-office-365-user-accounts"></a><span data-ttu-id="92efa-103">Hogyan toouse Azure RemoteApp az Office 365 felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="92efa-103">How toouse Azure RemoteApp with Office 365 user accounts</span></span>
> [!IMPORTANT]
> <span data-ttu-id="92efa-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="92efa-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="92efa-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="92efa-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="92efa-106">Ha az Office 365-előfizetéssel rendelkezik egy Azure Active Directoryban, amely tárolja a felhasználói neveket és jelszavakat tooaccess Office 365-szolgáltatásokhoz használt.</span><span class="sxs-lookup"><span data-stu-id="92efa-106">If you have an Office 365 subscription you have an Azure Active Directory that stores your user names and passwords used tooaccess Office 365 services.</span></span> <span data-ttu-id="92efa-107">Például amikor a felhasználók Office 365 ProPlus aktiválása hitelesítéshez az Azure AD toocheck licencek ellen.</span><span class="sxs-lookup"><span data-stu-id="92efa-107">For example, when your users activate Office 365 ProPlus they authenticate against Azure AD toocheck for licenses.</span></span> <span data-ttu-id="92efa-108">A legtöbb ügyfél volna például toouse hello ugyanazt az Azure RemoteApp könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="92efa-108">Most customers would like toouse hello same directory with Azure RemoteApp.</span></span>

<span data-ttu-id="92efa-109">Azure RemoteApp telepítése valószínűleg használ egy másik Azure AD társított Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="92efa-109">If you are deploying Azure RemoteApp you are most likely using an Azure subscription that is associated with a different Azure AD.</span></span> <span data-ttu-id="92efa-110">A rendezés toouse az Office 365-könyvtár, szüksége lesz a toomove hello Azure-előfizetés abba a könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="92efa-110">In order toouse your Office 365 directory, you will need toomove hello Azure subscription into that directory.</span></span>

<span data-ttu-id="92efa-111">Hogyan információk toodeploy Office 365 ügyfélalkalmazások, lásd: [hogyan toouse az Office 365-előfizetéshez az Azure RemoteApp](remoteapp-officesubscription.md).</span><span class="sxs-lookup"><span data-stu-id="92efa-111">For info on how toodeploy Office 365 client applications, see [How toouse your Office 365 subscription with Azure RemoteApp](remoteapp-officesubscription.md).</span></span>

## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a><span data-ttu-id="92efa-112">1. fázis: Az Office 365 Azure Active Directory ingyenes előfizetés regisztrálása</span><span class="sxs-lookup"><span data-stu-id="92efa-112">Phase 1: Register your free Office 365 Azure Active Directory subscription</span></span>
<span data-ttu-id="92efa-113">A klasszikus Azure portálon hello használatakor lépésekkel hello a [regisztrálása az Azure Active Directory ingyenes előfizetés](https://technet.microsoft.com/library/dn832618.aspx) tooget rendszergazdai hozzáférés tooyour az Azure AD hello Azure felügyeleti portálján keresztül.</span><span class="sxs-lookup"><span data-stu-id="92efa-113">If you are using hello Azure classic portal, use hello steps in [Register your free Azure Active Directory subscription](https://technet.microsoft.com/library/dn832618.aspx) tooget administrative access tooyour Azure AD via hello Azure Management Portal.</span></span> <span data-ttu-id="92efa-114">Hello így a folyamat legyen képes toolog hello Azure-portálon való és lásd: a könyvtár létezik – ezen a ponton nem láthatók sokkal mivel hello használ az Azure RemoteApp teljes Azure-előfizetés használatban van egy másik címtárhoz.</span><span class="sxs-lookup"><span data-stu-id="92efa-114">As hello result of this process you should be able toolog into hello Azure portal and see your directory there – at this point you won’t see much more since hello full Azure subscription you are using with Azure RemoteApp is in a different directory.</span></span>

<span data-ttu-id="92efa-115">Hello nevét és hello rendszergazdai fiókjának ebben a lépésben létrehozott jelszó megjegyzése – azok a 2. szakasza szükség lesz.</span><span class="sxs-lookup"><span data-stu-id="92efa-115">Remember hello name and password of hello administrator account you created in this step – they will be needed in Phase 2.</span></span>

<span data-ttu-id="92efa-116">Ha hello Azure-portált használja, tekintse meg [hogyan tooregister és aktiválása az Office 365-portál használatával, ingyenes Azure Active Directory](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).</span><span class="sxs-lookup"><span data-stu-id="92efa-116">If you are using hello Azure portal, check out [How tooregister and activate a free Azure Active Directory using Office 365 portal](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).</span></span>

## <a name="phase-2-change-hello-azure-ad-associated-with-your-azure-subscription"></a><span data-ttu-id="92efa-117">2. fázis: Módosítás hello Azure AD az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="92efa-117">Phase 2: Change hello Azure AD associated with your Azure subscription.</span></span>
<span data-ttu-id="92efa-118">Fogjuk toochange az Azure-előfizetéshez az aktuális könyvtárból azt az 1. fázis együttműködik hello Office 365 könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="92efa-118">We are going toochange your Azure subscription from its current directory into hello Office 365 directory we worked with in Phase 1.</span></span>

<span data-ttu-id="92efa-119">Útmutatás alapján hello ismertetett [módosítás hello Azure Active Directory-bérlőt az Azure Remoteappban](remoteapp-changetenant.md).</span><span class="sxs-lookup"><span data-stu-id="92efa-119">Follow hello instructions described in [Change hello Azure Active Directory tenant in Azure RemoteApp](remoteapp-changetenant.md).</span></span> <span data-ttu-id="92efa-120">Fordítson különös figyelmet toohello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="92efa-120">Pay particular attention toohello following steps:</span></span>

* <span data-ttu-id="92efa-121">#1. lépés: Ha telepítette az Azure RemoteApp (ARA) ebben az előfizetésben, győződjön meg arról, előtt távolítsa el az Azure AD felhasználói fiókokhoz bármely ARA gyűjtemények először, semmi mást.</span><span class="sxs-lookup"><span data-stu-id="92efa-121">Step #1: If you have deployed Azure RemoteApp (ARA) in this subscription, make sure you remove all Azure AD user accounts from any ARA collections first, before trying anything else.</span></span> <span data-ttu-id="92efa-122">Azt is megteheti érdemes lehet a meglévő gyűjtemények törlése.</span><span class="sxs-lookup"><span data-stu-id="92efa-122">Alternatively, you can consider deleting any existing collections.</span></span>
* <span data-ttu-id="92efa-123">#2. lépés: Ez egy fontos lépés.</span><span class="sxs-lookup"><span data-stu-id="92efa-123">Step #2: This is a critical step.</span></span> <span data-ttu-id="92efa-124">Toouse Microsoft-fiókkal van szüksége (pl. @outlook.com), a szolgáltatás-rendszergazdáknak hello előfizetésben; ennek az az oka nem tudunk hello csatolva az Azure AD toohello előfizetés – meglévő, ha azt választja, az összes felhasználói fiókot a Microsoft nem fogja tudni toomove azt tooa másik Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92efa-124">You need toouse a Microsoft account (e.g. @outlook.com) as a Service Administrator on hello subscription; this is because we cannot have any user accounts from hello existing Azure AD attached toohello subscription – if we do, we won’t be able toomove it tooa different Azure AD.</span></span>
* <span data-ttu-id="92efa-125">#4. lépés: Egy létező könyvtár hozzáadásakor hello rendszer kérni fogja toosign be hello rendszergazdai fiókkal könyvtárhoz.</span><span class="sxs-lookup"><span data-stu-id="92efa-125">Step #4: When adding an existing directory, hello system will ask you toosign in with hello administrator account for that directory.</span></span> <span data-ttu-id="92efa-126">Győződjön meg arról, hogy toouse hello rendszergazdai fiók az 1. szakasza.</span><span class="sxs-lookup"><span data-stu-id="92efa-126">Make sure toouse hello administrator account from Phase 1.</span></span>
* <span data-ttu-id="92efa-127">#5. lépés: Hello hello előfizetés tooyour Office 365-könyvtár-jének szülőkönyvtárában módosítása.</span><span class="sxs-lookup"><span data-stu-id="92efa-127">Step #5: Change hello parent directory of hello subscription tooyour Office 365 directory.</span></span> <span data-ttu-id="92efa-128">hello végeredménynek kell, hogy a beállítások -> az előfizetés sorolja fel az Office 365 hello directory előfizetések.</span><span class="sxs-lookup"><span data-stu-id="92efa-128">hello end result should be that under Settings -> Subscriptions your subscription lists hello Office 365 directory.</span></span> 
  <span data-ttu-id="92efa-129">![Hello hello előfizetés-jének szülőkönyvtárában módosítása](./media/remoteapp-o365user/settings.png)</span><span class="sxs-lookup"><span data-stu-id="92efa-129">![Change hello parent directory of hello subscription](./media/remoteapp-o365user/settings.png)</span></span>

<span data-ttu-id="92efa-130">Ezen a ponton az Azure RemoteApp előfizetés társul az Office 365 az Azure AD; hello meglévő Office 365 felhasználói fiókjainak használhatja az Azure RemoteApp!</span><span class="sxs-lookup"><span data-stu-id="92efa-130">At this point your Azure RemoteApp subscription is associated with your Office 365 Azure AD; you can use hello existing Office 365 user accounts with Azure RemoteApp!</span></span>
