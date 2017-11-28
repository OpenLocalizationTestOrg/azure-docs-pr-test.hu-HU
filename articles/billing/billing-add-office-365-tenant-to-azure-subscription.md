---
title: "Az Office 365-bérlő használata Azure-előfizetés |} Microsoft Docs"
description: "Ismerje meg az Office 365-címtár (bérlői) felvétele az Azure-előfizetésre."
services: 
documentationcenter: 
author: JiangChen79
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: cjiang
ms.openlocfilehash: 7affb4a83cdb8ababef60e786a3c824e7477ed68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="associate-an-office-365-tenant-to-an-azure-subscription"></a><span data-ttu-id="610af-103">Az Office 365-bérlő egy Azure-előfizetések társítása</span><span class="sxs-lookup"><span data-stu-id="610af-103">Associate an Office 365 tenant to an Azure subscription</span></span>
<span data-ttu-id="610af-104">Csatolja az önálló Azure és az Office 365-előfizetések, hogy az Azure-előfizetéssel az Office 365-bérlő érhető el.</span><span class="sxs-lookup"><span data-stu-id="610af-104">Link your separate Azure and Office 365 subscriptions so that you can access the Office 365 tenant from your Azure subscription.</span></span> <span data-ttu-id="610af-105">Az előfizetések csatolásához jelentkezzen be az Azure-bA az Azure szolgáltatás-rendszergazdai fiók, adja hozzá a könyvtárát, és az Office 365 munkahelyi és iskolai fiókok hozzáadása az Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="610af-105">To link your subscriptions, sign in to Azure with the Azure service administrator account, add a directory, and add the Office 365 organizational accounts to the Azure Active Directory tenant.</span></span>

<span data-ttu-id="610af-106">Ha a felhasználók Office 365-előfizetéssel szeretne az Azure Active Directory-példányban, vagy az Office 365-fiókkal, de nem Azure-fiókkal rendelkezik, tekintse meg [regisztráció Office 365-fiókkal az Azure](billing-use-existing-office-365-account-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="610af-106">If you want an Office 365 subscription for users in your Azure Active Directory instance or you have an Office 365 account but not an Azure account, see [Sign up for Azure with Office 365 account](billing-use-existing-office-365-account-azure-subscription.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="610af-107">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="610af-107">Before you begin</span></span>
* <span data-ttu-id="610af-108">Az Azure-előfizetés szolgáltatás-rendszergazda hitelesítő adatait kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="610af-108">You must have the credentials of the Azure subscription service administrator.</span></span> <span data-ttu-id="610af-109">Közös rendszergazdai fiókok nem hajtható végre néhány cikkben leírt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="610af-109">Co-administrator accounts can't do some of the steps in this article.</span></span> <span data-ttu-id="610af-110">A szolgáltatás-rendszergazda módosításához lásd [hozzáadása vagy módosítása az Azure-rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span><span class="sxs-lookup"><span data-stu-id="610af-110">To change your service administrator, see [How to add or change Azure administrator roles](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span></span>
* <span data-ttu-id="610af-111">Az Office 365-bérlő globális rendszergazdájának hitelesítő adatait kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="610af-111">You must have the credentials of a global administrator of the Office 365 tenant.</span></span>
* <span data-ttu-id="610af-112">A szolgáltatás-rendszergazda e-mail címe nem lehet az Office 365-bérlő.</span><span class="sxs-lookup"><span data-stu-id="610af-112">The email address of the service administrator must not be in the Office 365 tenant.</span></span>
* <span data-ttu-id="610af-113">A szolgáltatás-rendszergazda e-mail címe nem egyezik, az Office 365-bérlő globális rendszergazdái.</span><span class="sxs-lookup"><span data-stu-id="610af-113">The email address of the service administrator must not match that of any global administrator of the Office 365 tenant.</span></span>
* <span data-ttu-id="610af-114">Ha egy e-mail címet, amely a Microsoft-fiók és a szervezeti fiók használatához, átmenetileg váltson egy másik Microsoft-fiók használata az Azure-előfizetéshez a szolgáltatás rendszergazdájával.</span><span class="sxs-lookup"><span data-stu-id="610af-114">If you use an email address that is both a Microsoft account and an organizational account, temporarily change the service administrator of your Azure subscription to use another Microsoft account.</span></span> <span data-ttu-id="610af-115">A Microsoft-fiókjával a hozhat létre a [Microsoft-fiók bejelentkezési oldalának](https://signup.live.com/).</span><span class="sxs-lookup"><span data-stu-id="610af-115">You can create a Microsoft account at the [Microsoft account signup page](https://signup.live.com/).</span></span>

## <a name="link-office-365-tenant-to-azure-subscription"></a><span data-ttu-id="610af-116">Office 365-bérlő hivatkozás Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="610af-116">Link Office 365 tenant to Azure subscription</span></span>
<span data-ttu-id="610af-117">Rendelje hozzá a az Office 365-bérlő az Azure-előfizetéshez, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="610af-117">To associate the Office 365 tenant to the Azure subscription, follow these steps:</span></span>

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a><span data-ttu-id="610af-118">1. lépés: Az Office 365-bérlő hozzáadása az Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="610af-118">Step 1: Add Office 365 tenant to your Azure subscription</span></span>

1. <span data-ttu-id="610af-119">Jelentkezzen be a [a klasszikus Azure portálon](https://manage.windowsazure.com/) a szolgáltatás rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="610af-119">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with the service administrator credentials.</span></span>

    ![Képernyőfelvétel az Azure-bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. <span data-ttu-id="610af-121">A bal oldali panelen válassza ki a **ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="610af-121">In the left pane, select **ACTIVE DIRECTORY**.</span></span> <span data-ttu-id="610af-122">Ne tekintse meg az Office 365-bérlő.</span><span class="sxs-lookup"><span data-stu-id="610af-122">You shouldn't see the Office 365 tenant.</span></span> <span data-ttu-id="610af-123">Ha azt látja, folytassa a [2. lépés: módosítsa a könyvtárat, az Azure-előfizetéshez társított](#Step2).</span><span class="sxs-lookup"><span data-stu-id="610af-123">If you see it, skip to [Step 2: Change the directory associated with the Azure subscription](#Step2).</span></span>
   
   ![Képernyőkép az Active Directory-bejegyzés](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. <span data-ttu-id="610af-125">Válassza ki **új** > **DIRECTORY** > **egyéni létrehozás**.</span><span class="sxs-lookup"><span data-stu-id="610af-125">Select **NEW** > **DIRECTORY** > **CUSTOM CREATE**.</span></span>
   
    ![Képernyőfelvétel az Azure Active Directory egyéni létrehozása](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. <span data-ttu-id="610af-127">Az a **Hozzáadás directory** lap **DIRECTORY**, jelölje be **meglévő címtár használata**.</span><span class="sxs-lookup"><span data-stu-id="610af-127">On the **Add directory** page, under **DIRECTORY**, select **Use existing directory**.</span></span> <span data-ttu-id="610af-128">Válassza ki **készen állok a kijelentkezésre**, és válassza ki **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="610af-128">Then select **I am ready to be signed out now**, and select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Képernyőkép a "Meglévő könyvtár használata"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. <span data-ttu-id="610af-130">Aláírása után jelentkezzen be az Office 365-bérlő globális rendszergazdájának hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="610af-130">After you are signed out, sign in with the global administrator’s credentials of your Office 365 tenant.</span></span>
   
    ![Képernyőkép az Office 365 globális rendszergazdai bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. <span data-ttu-id="610af-132">Válassza ki **továbbra is**.</span><span class="sxs-lookup"><span data-stu-id="610af-132">Select **Continue**.</span></span>
   
    ![Képernyőkép a ellenőrzése](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. <span data-ttu-id="610af-134">Válassza ki **azonnali Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="610af-134">Select **Sign out now**.</span></span>
   
    ![Képernyőkép a kijelentkezési](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. <span data-ttu-id="610af-136">Jelentkezzen be a [a klasszikus Azure portálon](https://manage.windowsazure.com/) a szolgáltatás rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="610af-136">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with the service administrator credentials.</span></span>
   
    ![Képernyőfelvétel az Azure-bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. <span data-ttu-id="610af-138">Meg kell jelennie az Office 365-bérlő az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="610af-138">You should see your Office 365 tenant in the dashboard.</span></span>
   
    ![Irányítópultjának képernyőképe](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <span data-ttu-id="610af-140"><a name="Step2"></a>2. lépés: Módosítsa a könyvtárat, az Azure-előfizetéshez társított</span><span class="sxs-lookup"><span data-stu-id="610af-140"><a name="Step2"></a>Step 2: Change the directory associated with the Azure subscription</span></span>
   
1. <span data-ttu-id="610af-141">Válassza ki **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="610af-141">Select **Settings**.</span></span>
   
    ![Képernyőfelvétel az Azure klasszikus portál beállítások ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. <span data-ttu-id="610af-143">Az Azure-előfizetéshez, majd válassza ki és **könyvtár szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="610af-143">Select your Azure subscription, and then select **EDIT DIRECTORY**.</span></span>

    ![Képernyőfelvétel az Azure-előfizetés Szerkesztés könyvtára](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. <span data-ttu-id="610af-145">Válassza ki **következő** ![tovább ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span><span class="sxs-lookup"><span data-stu-id="610af-145">Select **Next** ![Next icon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span></span>
   
    ![Képernyőkép a "Módosítása a társított könyvtár"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. <span data-ttu-id="610af-147">Tekintse át az érintett fiókokat.</span><span class="sxs-lookup"><span data-stu-id="610af-147">Review the affected accounts.</span></span> <span data-ttu-id="610af-148">Minden társrendszergazdák és [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) a meglévő erőforrás-csoportok a hozzárendelt hozzáféréssel rendelkező felhasználók törlődnek.</span><span class="sxs-lookup"><span data-stu-id="610af-148">All co-administrators and [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) users with assigned access in the existing resource groups are removed.</span></span> <span data-ttu-id="610af-149">A figyelmeztetést kap csak akkor említi, társrendszergazdák eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="610af-149">The warning you receive only mentions the removal of co-administrators.</span></span>
      
    ![Képernyőkép a a közös rendszergazdai fiókok, el kell távolítani.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Képernyőkép a egy példa-felhasználói fiókot el kell távolítani.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. <span data-ttu-id="610af-152">Válassza ki **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="610af-152">Select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-to-the-azure-active-directory-tenant"></a><span data-ttu-id="610af-153">3. lépés: Az Office 365 szervezeti fiókokat hozzá társadminisztrátorként az Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="610af-153">Step 3: Add your Office 365 organizational accounts as co-administrators to the Azure Active Directory tenant</span></span>
   
1. <span data-ttu-id="610af-154">Válassza ki a **RENDSZERGAZDÁK** lapra, majd válassza ki **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="610af-154">Select the **ADMINISTRATORS** tab, and then select **ADD**.</span></span>
   
    ![Képernyőfelvétel az Azure klasszikus portál beállításai rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. <span data-ttu-id="610af-156">Adja meg az Office 365-bérlő szervezeti fiókkal, az Azure-előfizetés, majd válassza ki és **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="610af-156">Enter an organizational account of your Office 365 tenant, select the Azure subscription, and then select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Képernyőfelvétel az Azure hozzáadása társadminisztrátoraként párbeszédpanel](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. <span data-ttu-id="610af-158">Lépjen vissza a **RENDSZERGAZDÁK** fülre.</span><span class="sxs-lookup"><span data-stu-id="610af-158">Go back to the **ADMINISTRATORS** tab.</span></span> <span data-ttu-id="610af-159">Meg kell jelennie a szervezeti fiók társadminisztrátoraként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="610af-159">You should see the organizational account displayed as co-administrator.</span></span>
   
    ![Képernyőkép a rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  <span data-ttu-id="610af-161">A közös rendszergazdai fiókkal az Azure-hozzáférés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="610af-161">Test access to Azure with the co-administrator account.</span></span>
   
    <span data-ttu-id="610af-162">a.</span><span class="sxs-lookup"><span data-stu-id="610af-162">a.</span></span> <span data-ttu-id="610af-163">Jelentkezzen ki a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="610af-163">Sign out of the Azure classic portal.</span></span>
   
    <span data-ttu-id="610af-164">b.</span><span class="sxs-lookup"><span data-stu-id="610af-164">b.</span></span> <span data-ttu-id="610af-165">Nyissa meg az [Azure portált](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="610af-165">Open the [Azure portal](https://portal.azure.com/).</span></span>
   
    <span data-ttu-id="610af-166">c.</span><span class="sxs-lookup"><span data-stu-id="610af-166">c.</span></span> <span data-ttu-id="610af-167">Adja meg a közös rendszergazdájának hitelesítő adatait, majd válassza ki **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="610af-167">Enter the credentials of the co-administrator, and then select **Sign in**.</span></span>
   
    ![Képernyőfelvétel az Azure bejelentkezési oldal](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="610af-169">Segítség</span><span class="sxs-lookup"><span data-stu-id="610af-169">Need help?</span></span> <span data-ttu-id="610af-170">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="610af-170">Contact support.</span></span>
<span data-ttu-id="610af-171">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="610af-171">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>


