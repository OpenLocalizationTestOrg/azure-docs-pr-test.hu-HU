---
title: "Az Azure Active Directory tartományi szolgáltatások: Az Azure Active Directory-Tartományvezérlők rendszergazdák csoport létrehozása |} Microsoft Docs"
description: "Az Azure Active Directory Domain Services engedélyezése a klasszikus Azure portál használatával"
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
ms.openlocfilehash: 5ed0125e05928cf0f6d9941e099b433ecb46e6e2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-classic-portal"></a><span data-ttu-id="fd9a6-103">Az Azure Active Directory Domain Services engedélyezése a klasszikus Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="fd9a6-103">Enable Azure Active Directory Domain Services using the Azure classic portal</span></span>
<span data-ttu-id="fd9a6-104">Ez a cikk ismerteti, és végigvezeti a konfigurációs feladatok, amelyek szükségesek ahhoz, hogy engedélyezze az Azure Active Directory tartományi szolgáltatások (az Azure Active Directory tartományi szolgáltatások) az Azure Active Directory (Azure AD) bérlő.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-104">This article describes and walks through the configuration tasks that are required for you to enable Azure Active Directory Domain Services (Azure AD DS) for your Azure Active Directory (Azure AD) tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="fd9a6-105">[**Próbálja meg helyette az új (előzetes verzió) Azure portál felületet**](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="fd9a6-105">[**Try the new (preview) Azure portal experience instead**](active-directory-ds-getting-started.md).</span></span> 
>

## <a name="task-1-create-the-azure-ad-dc-administrators-group"></a><span data-ttu-id="fd9a6-106">1. feladat: az Azure Active Directory-Tartományvezérlők rendszergazdák csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd9a6-106">Task 1: create the Azure AD DC administrators group</span></span>
<span data-ttu-id="fd9a6-107">Az első tevékenységet, ha egy felügyeleti csoport az Azure AD-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-107">The first task is to create an administrative group in your Azure AD tenant.</span></span> <span data-ttu-id="fd9a6-108">Ez a különleges felügyeleti csoport neve *AAD DC rendszergazdák*.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-108">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="fd9a6-109">A csoport tagjai a gépeken, amelyek a tartományhoz az Azure Active Directory tartományi szolgáltatások által kezelt tartomány rendszergazdai jogosultsággal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-109">Members of this group are granted administrative permissions on machines that are domain-joined to the Azure Active Directory Domain Services-managed domain.</span></span> <span data-ttu-id="fd9a6-110">A tartományhoz csatlakoztatott számítógépeken ehhez a csoporthoz hozzáadni a Rendszergazdák csoport.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-110">On domain-joined machines, this group is added to the administrators group.</span></span> <span data-ttu-id="fd9a6-111">A csoport tagjai emellett használhatja a távoli asztal távolról csatlakozni a tartományhoz csatlakoztatott számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-111">Additionally, members of this group can use Remote Desktop to connect remotely to domain-joined machines.</span></span>  

> [!NOTE]
> <span data-ttu-id="fd9a6-112">Nincs tartományi rendszergazda vagy a vállalati rendszergazda engedélyekkel kell rendelkeznie az Azure Active Directory tartományi szolgáltatások által létrehozott felügyelt tartományon.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-112">You do not have Domain Administrator or Enterprise Administrator permissions on the managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="fd9a6-113">A felügyelt tartományok ezeket az engedélyeket a szolgáltatás által fenntartott, és nem érhetik el a bérlő belüli felhasználók.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-113">On managed domains, these permissions are reserved by the service and are not made available to users within the tenant.</span></span> <span data-ttu-id="fd9a6-114">A speciális felügyeleti csoport létrehozása a konfigurációs feladat használatával azonban néhány kiemelt műveletek végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-114">However, you can use the special administrative group created in this configuration task to perform some privileged operations.</span></span> <span data-ttu-id="fd9a6-115">Ezek a műveletek közé tartoznak a számítógépek csatlakoztatása a tartományhoz, tartományhoz csatlakozó számítógépeken a felügyeleti csoportba tartozó és a csoportházirend konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-115">These operations include joining computers to the domain, belonging to the administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="fd9a6-116">A konfigurációs feladat a felügyeleti csoport létrehozásához, és egy vagy több felhasználó hozzáadása a csoporthoz a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-116">In this configuration task, you create the administrative group and add one or more users in your directory to the group.</span></span> <span data-ttu-id="fd9a6-117">A felügyeleti csoport létrehozása az Azure Active Directory tartományi szolgáltatások, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fd9a6-117">To create the administrative group for Azure Active Directory Domain Services, do the following:</span></span>

1. <span data-ttu-id="fd9a6-118">Nyissa meg a [klasszikus Azure portált](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fd9a6-118">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="fd9a6-119">A bal oldali panelen válassza az **Active Directory** gombot.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-119">In the left pane, select the **Active Directory** button.</span></span>
3. <span data-ttu-id="fd9a6-120">Válassza ki, amelynek az Azure Active Directory tartományi szolgáltatások engedélyezni szeretné az Azure AD bérlőt (címtárat).</span><span class="sxs-lookup"><span data-stu-id="fd9a6-120">Select the Azure AD tenant (directory) for which you want to enable Azure Active Directory Domain Services.</span></span> <span data-ttu-id="fd9a6-121">Csak egy tartomány minden Azure AD-címtár is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-121">You can create only one domain for each Azure AD directory.</span></span>

    ![Válassza ki az Azure AD-címtár](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="fd9a6-123">Az a **preview directory** lapján kattintson a **csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-123">On the **preview directory** page, click the **Groups** tab.</span></span>
5. <span data-ttu-id="fd9a6-124">Csoport hozzáadása az Azure AD-bérlő, a az ablak alján feladat panelen kattintson a **csoport hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-124">To add a group to your Azure AD tenant, on the task pane at the bottom of the window, click **Add Group**.</span></span>

    ![A csoport hozzáadása gomb](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. <span data-ttu-id="fd9a6-126">A a **csoport hozzáadása** párbeszédpanelen hozzon létre egy nevű csoportot **AAD DC rendszergazdák**, és utána állítsa be **csoporttípus** való **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-126">In the **Add Group** dialog box, create a group named **AAD DC Administrators**, and then set **Group Type** to **Security**.</span></span>

   > [!WARNING]
   > <span data-ttu-id="fd9a6-127">Engedélyezi a hozzáférést az Azure Active Directory tartományi szolgáltatások által felügyelt tartományon belül, a pontos nevű csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-127">To enable access within your Azure Active Directory Domain Services-managed domain, create a group with this exact name.</span></span>
   >
   >

    ![A csoport hozzáadása párbeszédpanel](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. <span data-ttu-id="fd9a6-129">Az a **leírás** mezőbe írjon be egy leírást, amely lehetővé teszi, hogy mások számára, hogy ez a csoport engedélyt ad a rendszergazda belül Azure Active Directory tartományi szolgáltatások megismerése.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-129">In the **Description** box, enter a description that enables others to understand that this group grants administrative permissions within Azure Active Directory Domain Services.</span></span>
8. <span data-ttu-id="fd9a6-130">A csoport létrehozása után kattintson a csoport nevét a tulajdonságai megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-130">After you've created the group, click the group name to view its properties.</span></span>
9. <span data-ttu-id="fd9a6-131">Felhasználók hozzáadása a csoporthoz, az ablak alján kattintson a **tagok hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-131">To add users as members of the group, at the bottom of the window, click the **Add Members** button.</span></span>

    ![Adja hozzá a csoport tagjai gomb](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. <span data-ttu-id="fd9a6-133">Az a **tagok hozzáadása** párbeszédpanelen jelölje ki a felhasználókat, akik a csoport tagjai legyen, és kattintson a pipa ikonra a képernyő jobb alsó sarkában.</span><span class="sxs-lookup"><span data-stu-id="fd9a6-133">In the **Add members** dialog box, select the users who should be members of this group, and then click the checkmark icon at the lower right.</span></span>

    ![Felhasználók hozzáadása a rendszergazdák csoporthoz](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a><span data-ttu-id="fd9a6-135">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="fd9a6-135">Next step</span></span>
[<span data-ttu-id="fd9a6-136">2. feladat: hozzon létre, vagy válasszon egy Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="fd9a6-136">Task 2: create or select an Azure virtual network</span></span>](active-directory-ds-getting-started-vnet.md)
