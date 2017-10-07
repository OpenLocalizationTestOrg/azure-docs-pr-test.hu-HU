---
title: "Az Azure Active Directory tartományi szolgáltatások: Az Azure AD hello DC rendszergazdák csoport létrehozása |} Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: maheshu
ms.openlocfilehash: d629ab9632ef6a45b549630999ff9a122d60c040
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a><span data-ttu-id="5977b-103">Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="5977b-103">Enable Azure Active Directory Domain Services using hello Azure classic portal</span></span>
<span data-ttu-id="5977b-104">Ez a cikk ismerteti, és bemutatja, hogyan kell elvégeznie tooenable Azure Active Directory tartományi szolgáltatások (az Azure Active Directory tartományi szolgáltatások) az Azure Active Directory (Azure AD) bérlő hello konfigurációs feladatok.</span><span class="sxs-lookup"><span data-stu-id="5977b-104">This article describes and walks through hello configuration tasks that are required for you tooenable Azure Active Directory Domain Services (Azure AD DS) for your Azure Active Directory (Azure AD) tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="5977b-105">[**Próbálja meg helyette hello új (előzetes verzió) Azure portál élmény**](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="5977b-105">[**Try hello new (preview) Azure portal experience instead**](active-directory-ds-getting-started.md).</span></span> 
>

## <a name="task-1-create-hello-azure-ad-dc-administrators-group"></a><span data-ttu-id="5977b-106">1. feladat: hello Azure AD-tartományvezérlő rendszergazdák csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="5977b-106">Task 1: create hello Azure AD DC administrators group</span></span>
<span data-ttu-id="5977b-107">első hello feladata a toocreate egy felügyeleti csoport az Azure AD-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="5977b-107">hello first task is toocreate an administrative group in your Azure AD tenant.</span></span> <span data-ttu-id="5977b-108">Ez a különleges felügyeleti csoport neve *AAD DC rendszergazdák*.</span><span class="sxs-lookup"><span data-stu-id="5977b-108">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="5977b-109">A csoport tagjai a gépeken, amelyek a tartományhoz csatlakoztatott toohello Azure Active Directory tartományi szolgáltatások által kezelt tartomány rendszergazdai jogosultsággal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="5977b-109">Members of this group are granted administrative permissions on machines that are domain-joined toohello Azure Active Directory Domain Services-managed domain.</span></span> <span data-ttu-id="5977b-110">A tartományhoz csatlakoztatott számítógépeken ez a csoport toohello rendszergazdák csoport jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5977b-110">On domain-joined machines, this group is added toohello administrators group.</span></span> <span data-ttu-id="5977b-111">Emellett a csoport tagjai is használ a távoli asztal tooconnect távolról toodomain csatlakoztatott számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="5977b-111">Additionally, members of this group can use Remote Desktop tooconnect remotely toodomain-joined machines.</span></span>  

> [!NOTE]
> <span data-ttu-id="5977b-112">A felügyelt tartományra hello Azure Active Directory tartományi szolgáltatások által létrehozott nincs tartományi rendszergazda vagy a vállalati rendszergazda engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="5977b-112">You do not have Domain Administrator or Enterprise Administrator permissions on hello managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="5977b-113">A felügyelt tartományok ezek az engedélyek hello szolgáltatás által fenntartott és az nem elérhető toousers hello bérlői belül.</span><span class="sxs-lookup"><span data-stu-id="5977b-113">On managed domains, these permissions are reserved by hello service and are not made available toousers within hello tenant.</span></span> <span data-ttu-id="5977b-114">Azonban használhat hello speciális felügyeleti csoport létrehozása a konfigurációs feladat tooperform a néhány privilegizált műveleteket.</span><span class="sxs-lookup"><span data-stu-id="5977b-114">However, you can use hello special administrative group created in this configuration task tooperform some privileged operations.</span></span> <span data-ttu-id="5977b-115">Ezek a műveletek közé tartoznak a számítógépek toohello tartományhoz való csatlakozás, tartományhoz csatlakoztatott számítógépeken toohello felügyeleti csoporthoz tartozó és a csoportházirend konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="5977b-115">These operations include joining computers toohello domain, belonging toohello administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="5977b-116">A konfigurációs feladat hello felügyeleti csoport létrehozása, és a directory toohello csoportban lévő egy vagy több felhasználó hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="5977b-116">In this configuration task, you create hello administrative group and add one or more users in your directory toohello group.</span></span> <span data-ttu-id="5977b-117">toocreate hello felügyeleti csoport az Azure Active Directory tartományi szolgáltatásokhoz, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5977b-117">toocreate hello administrative group for Azure Active Directory Domain Services, do hello following:</span></span>

1. <span data-ttu-id="5977b-118">Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5977b-118">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="5977b-119">Hello bal oldali ablaktáblában jelöljön ki hello **Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="5977b-119">In hello left pane, select hello **Active Directory** button.</span></span>
3. <span data-ttu-id="5977b-120">Válassza ki a hello Azure AD bérlőt (címtárat), amelynek tooenable Azure Active Directory tartományi szolgáltatások keresi.</span><span class="sxs-lookup"><span data-stu-id="5977b-120">Select hello Azure AD tenant (directory) for which you want tooenable Azure Active Directory Domain Services.</span></span> <span data-ttu-id="5977b-121">Csak egy tartomány minden Azure AD-címtár is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="5977b-121">You can create only one domain for each Azure AD directory.</span></span>

    ![Válassza ki az Azure AD-címtár](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="5977b-123">A hello **preview directory** hello kattintson **csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="5977b-123">On hello **preview directory** page, click hello **Groups** tab.</span></span>
5. <span data-ttu-id="5977b-124">tooadd egy csoport tooyour az Azure AD bérlő hello munkaablak hello ablakban hello alján kattintson **csoport hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5977b-124">tooadd a group tooyour Azure AD tenant, on hello task pane at hello bottom of hello window, click **Add Group**.</span></span>

    ![hello csoport hozzáadása gomb](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. <span data-ttu-id="5977b-126">A hello **csoport hozzáadása** párbeszédpanelen hozzon létre egy nevű csoportot **AAD DC rendszergazdák**, és utána állítsa be **csoporttípus** túl**biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="5977b-126">In hello **Add Group** dialog box, create a group named **AAD DC Administrators**, and then set **Group Type** too**Security**.</span></span>

   > [!WARNING]
   > <span data-ttu-id="5977b-127">tooenable hozzáférés az Azure Active Directory tartományi szolgáltatások által felügyelt tartományon belüli ilyen pontos nevű hozzon létre egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="5977b-127">tooenable access within your Azure Active Directory Domain Services-managed domain, create a group with this exact name.</span></span>
   >
   >

    ![hello csoport hozzáadása párbeszédpanel](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. <span data-ttu-id="5977b-129">A hello **leírás** mezőbe írjon be egy leírást, amely lehetővé teszi, hogy mások számára, hogy ez a csoport engedélyt ad a rendszergazda belül Azure Active Directory tartományi szolgáltatások toounderstand.</span><span class="sxs-lookup"><span data-stu-id="5977b-129">In hello **Description** box, enter a description that enables others toounderstand that this group grants administrative permissions within Azure Active Directory Domain Services.</span></span>
8. <span data-ttu-id="5977b-130">Hello csoport létrehozása után kattintson a Tulajdonságok hello csoport neve tooview.</span><span class="sxs-lookup"><span data-stu-id="5977b-130">After you've created hello group, click hello group name tooview its properties.</span></span>
9. <span data-ttu-id="5977b-131">tooadd felhasználók hello csoportba hello ablakban hello alján kattintson hello **tagok hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="5977b-131">tooadd users as members of hello group, at hello bottom of hello window, click hello **Add Members** button.</span></span>

    ![Adja hozzá a csoport tagjai gomb](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. <span data-ttu-id="5977b-133">A hello **tagok hozzáadása** párbeszédpanelen jelölje be hello felhasználók, akik a csoport tagjai legyen, és kattintson a pipa ikonra hello hello alacsonyabb jobbra.</span><span class="sxs-lookup"><span data-stu-id="5977b-133">In hello **Add members** dialog box, select hello users who should be members of this group, and then click hello checkmark icon at hello lower right.</span></span>

    ![Felhasználók tooadministrators csoport hozzáadása](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a><span data-ttu-id="5977b-135">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="5977b-135">Next step</span></span>
[<span data-ttu-id="5977b-136">2. feladat: hozzon létre, vagy válasszon egy Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="5977b-136">Task 2: create or select an Azure virtual network</span></span>](active-directory-ds-getting-started-vnet.md)
