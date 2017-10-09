---
title: "Azure-előfizetés aaaUse az Office 365 bérlői |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd az Office 365-könyvtár (bérlői) tooan Azure-előfizetés."
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
ms.openlocfilehash: e560370417bd074a7e37ceb7c60da45dbbbcf775
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="associate-an-office-365-tenant-tooan-azure-subscription"></a><span data-ttu-id="5e82c-103">Az Office 365 bérlői tooan Azure-előfizetés társítása</span><span class="sxs-lookup"><span data-stu-id="5e82c-103">Associate an Office 365 tenant tooan Azure subscription</span></span>
<span data-ttu-id="5e82c-104">Csatolja az önálló Azure és az Office 365-előfizetések, hogy az Azure-előfizetéshez hello Office 365 bérlői érhető el.</span><span class="sxs-lookup"><span data-stu-id="5e82c-104">Link your separate Azure and Office 365 subscriptions so that you can access hello Office 365 tenant from your Azure subscription.</span></span> <span data-ttu-id="5e82c-105">toolink előfizetése, jelentkezzen be az Azure szolgáltatás-rendszergazdai fiók, hello tooAzure adja hozzá a könyvtárát, és adja hozzá az Office 365 hello munkahelyi és iskolai fiókok toohello Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5e82c-105">toolink your subscriptions, sign in tooAzure with hello Azure service administrator account, add a directory, and add hello Office 365 organizational accounts toohello Azure Active Directory tenant.</span></span>

<span data-ttu-id="5e82c-106">Ha a felhasználók Office 365-előfizetéssel szeretne az Azure Active Directory-példányban, vagy az Office 365-fiókkal, de nem Azure-fiókkal rendelkezik, tekintse meg [regisztráció Office 365-fiókkal az Azure](billing-use-existing-office-365-account-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="5e82c-106">If you want an Office 365 subscription for users in your Azure Active Directory instance or you have an Office 365 account but not an Azure account, see [Sign up for Azure with Office 365 account](billing-use-existing-office-365-account-azure-subscription.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="5e82c-107">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5e82c-107">Before you begin</span></span>
* <span data-ttu-id="5e82c-108">Hello hello Azure-előfizetés szolgáltatás-rendszergazda hitelesítő adataival kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="5e82c-108">You must have hello credentials of hello Azure subscription service administrator.</span></span> <span data-ttu-id="5e82c-109">Közös rendszergazdai fiókok nem hajtható végre néhány hello cikkben leírt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5e82c-109">Co-administrator accounts can't do some of hello steps in this article.</span></span> <span data-ttu-id="5e82c-110">toochange a szolgáltatás-rendszergazda, lásd: [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span><span class="sxs-lookup"><span data-stu-id="5e82c-110">toochange your service administrator, see [How tooadd or change Azure administrator roles](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span></span>
* <span data-ttu-id="5e82c-111">Hello hello Office 365-bérlő globális rendszergazdájának hitelesítő adatait kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="5e82c-111">You must have hello credentials of a global administrator of hello Office 365 tenant.</span></span>
* <span data-ttu-id="5e82c-112">hello hello szolgáltatás-rendszergazda e-mail címe nem lehet hello Office 365-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5e82c-112">hello email address of hello service administrator must not be in hello Office 365 tenant.</span></span>
* <span data-ttu-id="5e82c-113">hello hello szolgáltatás-rendszergazda e-mail címe nem egyezik az hello Office 365-bérlő globális rendszergazdái.</span><span class="sxs-lookup"><span data-stu-id="5e82c-113">hello email address of hello service administrator must not match that of any global administrator of hello Office 365 tenant.</span></span>
* <span data-ttu-id="5e82c-114">Egy e-mail címet, amely a Microsoft-fiók és a szervezeti fiók használatakor, átmenetileg váltson az Azure-előfizetés toouse hello szolgáltatás rendszergazdája másik Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="5e82c-114">If you use an email address that is both a Microsoft account and an organizational account, temporarily change hello service administrator of your Azure subscription toouse another Microsoft account.</span></span> <span data-ttu-id="5e82c-115">Létrehozhat egy Microsoft-fiók: hello [Microsoft-fiók bejelentkezési oldalának](https://signup.live.com/).</span><span class="sxs-lookup"><span data-stu-id="5e82c-115">You can create a Microsoft account at hello [Microsoft account signup page](https://signup.live.com/).</span></span>

## <a name="link-office-365-tenant-tooazure-subscription"></a><span data-ttu-id="5e82c-116">Office 365 bérlői tooAzure előfizetési hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="5e82c-116">Link Office 365 tenant tooAzure subscription</span></span>
<span data-ttu-id="5e82c-117">tooassociate hello Office 365 bérlői toohello Azure-előfizetésre, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5e82c-117">tooassociate hello Office 365 tenant toohello Azure subscription, follow these steps:</span></span>

### <a name="step-1-add-office-365-tenant-tooyour-azure-subscription"></a><span data-ttu-id="5e82c-118">1. lépés: Az Office 365 bérlői tooyour Azure-előfizetés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5e82c-118">Step 1: Add Office 365 tenant tooyour Azure subscription</span></span>

1. <span data-ttu-id="5e82c-119">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) hello szolgáltatás rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="5e82c-119">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with hello service administrator credentials.</span></span>

    ![Képernyőfelvétel az Azure-bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. <span data-ttu-id="5e82c-121">Hello bal oldali ablaktáblában jelöljön ki **ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-121">In hello left pane, select **ACTIVE DIRECTORY**.</span></span> <span data-ttu-id="5e82c-122">Hello Office 365 bérlői nem látható.</span><span class="sxs-lookup"><span data-stu-id="5e82c-122">You shouldn't see hello Office 365 tenant.</span></span> <span data-ttu-id="5e82c-123">Ha azt látja, hagyja ki túl[2. lépés: hello Azure-előfizetéssel társított hello könyvtárváltás](#Step2).</span><span class="sxs-lookup"><span data-stu-id="5e82c-123">If you see it, skip too[Step 2: Change hello directory associated with hello Azure subscription](#Step2).</span></span>
   
   ![Képernyőkép az Active Directory-bejegyzés](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. <span data-ttu-id="5e82c-125">Válassza ki **új** > **DIRECTORY** > **egyéni létrehozás**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-125">Select **NEW** > **DIRECTORY** > **CUSTOM CREATE**.</span></span>
   
    ![Képernyőfelvétel az Azure Active Directory egyéni létrehozása](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. <span data-ttu-id="5e82c-127">A hello **Hozzáadás directory** lap **DIRECTORY**, jelölje be **meglévő címtár használata**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-127">On hello **Add directory** page, under **DIRECTORY**, select **Use existing directory**.</span></span> <span data-ttu-id="5e82c-128">Válassza ki **kijelentkezésre készen toobe vagyok**, és válassza ki **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="5e82c-128">Then select **I am ready toobe signed out now**, and select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Képernyőkép a "Meglévő könyvtár használata"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. <span data-ttu-id="5e82c-130">Aláírása után jelentkezzen be az Office 365-bérlő hello globális rendszergazda hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5e82c-130">After you are signed out, sign in with hello global administrator’s credentials of your Office 365 tenant.</span></span>
   
    ![Képernyőkép az Office 365 globális rendszergazdai bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. <span data-ttu-id="5e82c-132">Válassza ki **továbbra is**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-132">Select **Continue**.</span></span>
   
    ![Képernyőkép a ellenőrzése](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. <span data-ttu-id="5e82c-134">Válassza ki **azonnali Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-134">Select **Sign out now**.</span></span>
   
    ![Képernyőkép a kijelentkezési](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. <span data-ttu-id="5e82c-136">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) hello szolgáltatás rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="5e82c-136">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with hello service administrator credentials.</span></span>
   
    ![Képernyőfelvétel az Azure-bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. <span data-ttu-id="5e82c-138">Meg kell jelennie az Office 365-bérlő hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="5e82c-138">You should see your Office 365 tenant in hello dashboard.</span></span>
   
    ![Irányítópultjának képernyőképe](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <span data-ttu-id="5e82c-140"><a name="Step2"></a>2. lépés: Hello Azure-előfizetéssel társított hello könyvtár módosítása</span><span class="sxs-lookup"><span data-stu-id="5e82c-140"><a name="Step2"></a>Step 2: Change hello directory associated with hello Azure subscription</span></span>
   
1. <span data-ttu-id="5e82c-141">Válassza ki **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-141">Select **Settings**.</span></span>
   
    ![Képernyőfelvétel az Azure klasszikus portál beállítások ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. <span data-ttu-id="5e82c-143">Az Azure-előfizetéshez, majd válassza ki és **könyvtár szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-143">Select your Azure subscription, and then select **EDIT DIRECTORY**.</span></span>

    ![Képernyőfelvétel az Azure-előfizetés Szerkesztés könyvtára](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. <span data-ttu-id="5e82c-145">Válassza ki **következő** ![tovább ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span><span class="sxs-lookup"><span data-stu-id="5e82c-145">Select **Next** ![Next icon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span></span>
   
    ![Képernyőkép a "módosítás társított directory hello"szövegrészt](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. <span data-ttu-id="5e82c-147">Tekintse át az érintett hello fiókok.</span><span class="sxs-lookup"><span data-stu-id="5e82c-147">Review hello affected accounts.</span></span> <span data-ttu-id="5e82c-148">Minden társrendszergazdák és [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) hello meglévő erőforráscsoportok a hozzárendelt hozzáféréssel rendelkező felhasználók törlődnek.</span><span class="sxs-lookup"><span data-stu-id="5e82c-148">All co-administrators and [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) users with assigned access in hello existing resource groups are removed.</span></span> <span data-ttu-id="5e82c-149">hello figyelmeztetést kap csak akkor említi, társrendszergazdák hello eltávolítását.</span><span class="sxs-lookup"><span data-stu-id="5e82c-149">hello warning you receive only mentions hello removal of co-administrators.</span></span>
      
    ![Képernyőkép a hello közös rendszergazdai fiókok toobe eltávolítva.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Képernyőkép a egy példa felhasználói fiók toobe eltávolítva.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. <span data-ttu-id="5e82c-152">Válassza ki **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="5e82c-152">Select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-toohello-azure-active-directory-tenant"></a><span data-ttu-id="5e82c-153">3. lépés: Adja hozzá az Office 365 munkahelyi és iskolai fiókok társrendszergazdák toohello Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="5e82c-153">Step 3: Add your Office 365 organizational accounts as co-administrators toohello Azure Active Directory tenant</span></span>
   
1. <span data-ttu-id="5e82c-154">Jelölje be hello **RENDSZERGAZDÁK** lapra, majd válassza ki **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-154">Select hello **ADMINISTRATORS** tab, and then select **ADD**.</span></span>
   
    ![Képernyőfelvétel az Azure klasszikus portál beállításai rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. <span data-ttu-id="5e82c-156">Adja meg az Office 365-bérlő szervezeti fiókkal, hello Azure-előfizetésre, majd válassza ki és **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="5e82c-156">Enter an organizational account of your Office 365 tenant, select hello Azure subscription, and then select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Képernyőfelvétel az Azure hozzáadása társadminisztrátoraként párbeszédpanel](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. <span data-ttu-id="5e82c-158">Lépjen vissza toohello **RENDSZERGAZDÁK** fülre. Meg kell jelennie hello szervezeti fiók társadminisztrátoraként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5e82c-158">Go back toohello **ADMINISTRATORS** tab. You should see hello organizational account displayed as co-administrator.</span></span>
   
    ![Képernyőkép a rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  <span data-ttu-id="5e82c-160">Teszt hozzáférés tooAzure hello közös rendszergazdai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="5e82c-160">Test access tooAzure with hello co-administrator account.</span></span>
   
    <span data-ttu-id="5e82c-161">a.</span><span class="sxs-lookup"><span data-stu-id="5e82c-161">a.</span></span> <span data-ttu-id="5e82c-162">Kijelentkezik hello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="5e82c-162">Sign out of hello Azure classic portal.</span></span>
   
    <span data-ttu-id="5e82c-163">b.</span><span class="sxs-lookup"><span data-stu-id="5e82c-163">b.</span></span> <span data-ttu-id="5e82c-164">Nyissa meg hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5e82c-164">Open hello [Azure portal](https://portal.azure.com/).</span></span>
   
    <span data-ttu-id="5e82c-165">c.</span><span class="sxs-lookup"><span data-stu-id="5e82c-165">c.</span></span> <span data-ttu-id="5e82c-166">Adja meg a hello hello közös rendszergazdájának hitelesítő adatait, majd válassza ki **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5e82c-166">Enter hello credentials of hello co-administrator, and then select **Sign in**.</span></span>
   
    ![Képernyőfelvétel az Azure bejelentkezési oldal](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="5e82c-168">Segítség</span><span class="sxs-lookup"><span data-stu-id="5e82c-168">Need help?</span></span> <span data-ttu-id="5e82c-169">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="5e82c-169">Contact support.</span></span>
<span data-ttu-id="5e82c-170">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.</span><span class="sxs-lookup"><span data-stu-id="5e82c-170">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>


