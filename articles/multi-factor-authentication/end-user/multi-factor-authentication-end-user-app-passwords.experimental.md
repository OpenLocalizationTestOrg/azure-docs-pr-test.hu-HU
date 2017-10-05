---
title: "Hogyan Alkalmazásjelszók használható az Azure MFA? | Microsoft Docs"
description: "Ezen a lapon segít megérteni az alkalmazásjelszók és azok szerepét a az Azure MFA figyelembe véve a felhasználóknak."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 1ecc2bdef5ff7ef8ed8dded7dc12428ce9657821
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a><span data-ttu-id="0e233-104">Mik a Azure multi-factor Authentication Alkalmazásjelszókat?</span><span class="sxs-lookup"><span data-stu-id="0e233-104">What are App Passwords in Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="0e233-105">Bizonyos böngészőn kívüli alkalmazások, például az Apple natív e-mail-ügyfélprogram, amely használja az Exchange Active Sync jelenleg nem támogatják a multi-factor authentication.</span><span class="sxs-lookup"><span data-stu-id="0e233-105">Certain non-browser apps, such as the Apple native email client that uses Exchange Active Sync, currently do not support multi-factor authentication.</span></span> <span data-ttu-id="0e233-106">Többtényezős hitelesítést felhasználónként kell engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="0e233-106">Multi-factor authentication is enabled per user.</span></span>  <span data-ttu-id="0e233-107">Ez azt jelenti, hogy a felhasználó multi-factor Authentication hitelesítés nem használható, ha:</span><span class="sxs-lookup"><span data-stu-id="0e233-107">This means that a user can't use multi-factor authentication if:</span></span>

- <span data-ttu-id="0e233-108">A felhasználó engedélyezte a többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="0e233-108">The user has been enabled for multi-factor authentication</span></span>
- <span data-ttu-id="0e233-109">A felhasználó próbál a böngészőn kívüli alkalmazások használatához.</span><span class="sxs-lookup"><span data-stu-id="0e233-109">The user is trying to use a non-browser app.</span></span>

<span data-ttu-id="0e233-110">Az alkalmazásjelszó lehetővé teszi, hogy a felhasználó használhatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0e233-110">An app password allows the user to use the app.</span></span>

<span data-ttu-id="0e233-111">Miután egy alkalmazásjelszót, használhatja az eredeti jelszóval ezen böngészőn kívüli alkalmazások helyett.</span><span class="sxs-lookup"><span data-stu-id="0e233-111">Once you have an app password, you use it in place of your original password with these non-browser apps.</span></span> <span data-ttu-id="0e233-112">Amikor regisztrál a kétlépéses ellenőrzést, akkor még szólítja fel a bárki jelentkezzen be a jelszót, ha azok nem is hajtsa végre a második ellenőrzési lehetővé teszik a nem Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0e233-112">When you register for two-step verification, you're telling Microsoft not to let anyone sign in with your password if they can't also perform the second verification.</span></span> <span data-ttu-id="0e233-113">A telefonján Apple natív e-mail-ügyfélprogram nem tud bejelentkezni, ha Ön, mert nem a kétlépéses ellenőrzéshez kérje meg.</span><span class="sxs-lookup"><span data-stu-id="0e233-113">The Apple native email client on your phone can't sign in as you because it can't ask for two-step verification.</span></span> <span data-ttu-id="0e233-114">Ez a probléma megoldása, ha egy biztonságosabb alkalmazásjelszót, amelyek nem használják napi.</span><span class="sxs-lookup"><span data-stu-id="0e233-114">The solution to this problem is to create a more secure app password that you don't use day-to-day.</span></span> <span data-ttu-id="0e233-115">Az alkalmazásjelszók csak ezek az alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="0e233-115">App passwords are only for those apps that can't support two-step verification.</span></span> <span data-ttu-id="0e233-116">Használja a jelszót, hogy az alkalmazások multi-factor authentication kihagyásához és továbbra is működnek.</span><span class="sxs-lookup"><span data-stu-id="0e233-116">Use the app password so that apps can bypass multi-factor authentication and continue to work.</span></span>


> [!NOTE]
> <span data-ttu-id="0e233-117">Office 2013-ügyfelek (például az Outlook) támogatja az új hitelesítési protokollok és a kétlépéses ellenőrzéshez használttal használható.</span><span class="sxs-lookup"><span data-stu-id="0e233-117">Office 2013 clients (including Outlook) support new authentication protocols and can be used with two-step verification.</span></span> <span data-ttu-id="0e233-118">Alkalmazásjelszók esetén nincs szükség Office 2013 ügyfelek való használatra.</span><span class="sxs-lookup"><span data-stu-id="0e233-118">App passwords are not required for use with Office 2013 clients.</span></span>  <span data-ttu-id="0e233-119">További információkért lásd: [Office 2013 modern hitelesítés nyilvános előzetes bejelentette](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span><span class="sxs-lookup"><span data-stu-id="0e233-119">For more information, see [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span></span>


## <a name="how-to-use-app-passwords"></a><span data-ttu-id="0e233-120">Alkalmazásjelszavak használata</span><span class="sxs-lookup"><span data-stu-id="0e233-120">How to use app passwords</span></span>
<span data-ttu-id="0e233-121">Az alábbiakban néhány feltétlenül tudni az alkalmazásjelszókról:</span><span class="sxs-lookup"><span data-stu-id="0e233-121">Here are some things to know about app passwords:</span></span>

* <span data-ttu-id="0e233-122">Ne hozzon létre egy saját alkalmazásjelszókat.</span><span class="sxs-lookup"><span data-stu-id="0e233-122">You don't create your own app passwords.</span></span> <span data-ttu-id="0e233-123">Ezek automatikusan jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="0e233-123">They are automatically generated.</span></span>
* <span data-ttu-id="0e233-124">Jelenleg csak egy legfeljebb 40 jelszó felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="0e233-124">Currently there is a limit of 40 passwords per user.</span></span> 
* <span data-ttu-id="0e233-125">Ha megpróbál létrehozni egy alkalmazásjelszót a határérték elérése után, összekapcsolta törölje az egyik a meglévő alkalmazásjelszavak előtt hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="0e233-125">If you try to create an app password after you have reached the limit, you'll have to delete one of your existing app passwords before you create a new one.</span></span>
* <span data-ttu-id="0e233-126">Minden eszközhöz, ne alkalmazásonként egy alkalmazásjelszót használni.</span><span class="sxs-lookup"><span data-stu-id="0e233-126">Use one app password per device, not per application.</span></span> <span data-ttu-id="0e233-127">Például hozzon létre egy alkalmazásjelszót a hordozható számítógép, és azt használja az összes, az alkalmazások adott hordozható számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0e233-127">For example, you can create one app password for your laptop and use that app password for all of your applications on that laptop.</span></span> <span data-ttu-id="0e233-128">Ezután hozzon létre egy második alkalmazásjelszót az asztalon az alkalmazások használatára.</span><span class="sxs-lookup"><span data-stu-id="0e233-128">Then, create a second app password to use for all your apps on your desktop.</span></span> 
* <span data-ttu-id="0e233-129">Lehetősége van egy alkalmazásjelszót az első alkalommal regisztrál a kétlépéses ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="0e233-129">You are given one app password the first time you register for two-step verification.</span></span>  <span data-ttu-id="0e233-130">Ha további megfelelően van szüksége, létrehozhat őket.</span><span class="sxs-lookup"><span data-stu-id="0e233-130">If you need additional ones, you can create them.</span></span>



## <a name="creating-and-deleting-app-passwords"></a><span data-ttu-id="0e233-131">Létrehozása és törlése alkalmazásjelszók</span><span class="sxs-lookup"><span data-stu-id="0e233-131">Creating and deleting app passwords</span></span>
<span data-ttu-id="0e233-132">Az első bejelentkezés során lehetősége van az alkalmazásjelszó használható.</span><span class="sxs-lookup"><span data-stu-id="0e233-132">During your initial sign-in, you are given an app password that you can use.</span></span>  <span data-ttu-id="0e233-133">Hozhat létre, és később törölni alkalmazásjelszókat.</span><span class="sxs-lookup"><span data-stu-id="0e233-133">You can also create and delete app passwords later on.</span></span> <span data-ttu-id="0e233-134">Alkalmazásjelszók törlési módja attól függ, hogy többtényezős hitelesítés használatát.</span><span class="sxs-lookup"><span data-stu-id="0e233-134">How you delete app passwords depends on how you use multi-factor authentication.</span></span> <span data-ttu-id="0e233-135">Annak meghatározásához, ahol kell az alkalmazásjelszók kezeléséhez lépjen a következő kérdések megválaszolása:</span><span class="sxs-lookup"><span data-stu-id="0e233-135">Answer the following questions to determine where you should go to manage app passwords:</span></span> 

1. <span data-ttu-id="0e233-136">Használhatók a kétlépéses ellenőrzést személyes Microsoft-fiókja?</span><span class="sxs-lookup"><span data-stu-id="0e233-136">Do you use two-step verification for your personal Microsoft account?</span></span> <span data-ttu-id="0e233-137">Ha igen, tekintse át a [alkalmazásjelszók és a kétlépéses ellenőrzést](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) cikk segítségét.</span><span class="sxs-lookup"><span data-stu-id="0e233-137">If yes, you should refer to the [App passwords and two-step verification](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) article for help.</span></span> <span data-ttu-id="0e233-138">Ha nem, továbbra is két kérdés.</span><span class="sxs-lookup"><span data-stu-id="0e233-138">If no, continue to question two.</span></span>

2. <span data-ttu-id="0e233-139">OK, hogy a kétlépéses ellenőrzést használni a munkahelyi vagy iskolai fiókját.</span><span class="sxs-lookup"><span data-stu-id="0e233-139">Ok, so you use two-step verification for your work or school account.</span></span> <span data-ttu-id="0e233-140">Használ az Office 365-alkalmazásokhoz való bejelentkezéshez?</span><span class="sxs-lookup"><span data-stu-id="0e233-140">Do you use it to sign in to Office 365 apps?</span></span> <span data-ttu-id="0e233-141">Ha igen, tekintse át [hozza létre az alkalmazásjelszót az Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) segítségét.</span><span class="sxs-lookup"><span data-stu-id="0e233-141">If yes, you should refer to [Create an app password for Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) for help.</span></span> <span data-ttu-id="0e233-142">Ha nem, továbbra is a három kérdést.</span><span class="sxs-lookup"><span data-stu-id="0e233-142">If no, continue to question three.</span></span> 

3. <span data-ttu-id="0e233-143">Használhatók a kétlépéses ellenőrzésről a Microsoft Azure-ban?</span><span class="sxs-lookup"><span data-stu-id="0e233-143">Do you use two-step verification with Microsoft Azure?</span></span> <span data-ttu-id="0e233-144">Ha igen, továbbra is a [kezelése az Azure portálon alkalmazásjelszók](#manage-app-passwords-in-the-Azure-portal) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0e233-144">If yes, continue to the [Manage app passwords in the Azure portal](#manage-app-passwords-in-the-Azure-portal) section of this article.</span></span> <span data-ttu-id="0e233-145">Ha nem, továbbra is négy kérdés.</span><span class="sxs-lookup"><span data-stu-id="0e233-145">If no, continue to question four.</span></span>

4. <span data-ttu-id="0e233-146">Nem biztos benne, ahol használhatja a kétlépéses ellenőrzést?</span><span class="sxs-lookup"><span data-stu-id="0e233-146">Not sure where you use two-step verification?</span></span> <span data-ttu-id="0e233-147">Továbbra is a [MyApps Portal alkalmazás jelszavak kezelése](#manage-app-passwords-with-the-myapps-portal) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0e233-147">Continue to the [Manage app passwords with the MyApps portal](#manage-app-passwords-with-the-myapps-portal) section of this article.</span></span> 


## <a name="manage-app-passwords-in-the-azure-portal"></a><span data-ttu-id="0e233-148">Az Azure portálon app jelszavak kezelése</span><span class="sxs-lookup"><span data-stu-id="0e233-148">Manage app passwords in the Azure portal</span></span>
<span data-ttu-id="0e233-149">Az Azure-ral használatakor a kétlépéses ellenőrzést, hozzon létre az Azure portálon keresztül szeretné.</span><span class="sxs-lookup"><span data-stu-id="0e233-149">If you use two-step verification with Azure, you want to create app passwords through the Azure portal.</span></span>

### <a name="to-create-app-passwords-in-the-azure-portal"></a><span data-ttu-id="0e233-150">Alkalmazásjelszók létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0e233-150">To create app passwords in the Azure portal</span></span>
1. <span data-ttu-id="0e233-151">Jelentkezzen be a klasszikus Azure portálra.</span><span class="sxs-lookup"><span data-stu-id="0e233-151">Sign in to the Azure classic portal.</span></span>
2. <span data-ttu-id="0e233-152">A lap tetején kattintson a jobb gombbal a felhasználónevét, és válassza ki a további biztonsági ellenőrzés.</span><span class="sxs-lookup"><span data-stu-id="0e233-152">At the top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="0e233-153">A proofup lapon, a lap tetején jelölje ki az alkalmazásjelszók</span><span class="sxs-lookup"><span data-stu-id="0e233-153">On the proofup page, at the top, select app passwords</span></span>
4. <span data-ttu-id="0e233-154">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0e233-154">Click **Create**.</span></span>
5. <span data-ttu-id="0e233-155">Adjon meg egy nevet a jelszót, és kattintson a **következő**</span><span class="sxs-lookup"><span data-stu-id="0e233-155">Enter a name for the app password and click **Next**</span></span>
6. <span data-ttu-id="0e233-156">A jelszót a vágólapra másolja és illessze be az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0e233-156">Copy the app password to the clipboard and paste it into your app.</span></span>
   
   ![Felhő](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="to-delete-app-passwords-in-the-azure-portal"></a><span data-ttu-id="0e233-158">Az Azure portálon alkalmazásjelszók törlése</span><span class="sxs-lookup"><span data-stu-id="0e233-158">To delete app passwords in the Azure portal</span></span>
1. <span data-ttu-id="0e233-159">Jelentkezzen be a klasszikus Azure portálra.</span><span class="sxs-lookup"><span data-stu-id="0e233-159">Sign in to the Azure classic portal.</span></span>
2. <span data-ttu-id="0e233-160">A lap tetején kattintson a jobb gombbal a felhasználónevét, és válassza ki a további biztonsági ellenőrzés.</span><span class="sxs-lookup"><span data-stu-id="0e233-160">At the top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="0e233-161">A lap tetején mellett további biztonsági ellenőrzési, válasszon **alkalmazásjelszókat.**</span><span class="sxs-lookup"><span data-stu-id="0e233-161">At the top, next to additional security verification, select **app passwords.**</span></span>
4. <span data-ttu-id="0e233-162">A törölni kívánt alkalmazásjelszót, mellett válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="0e233-162">Next to the app password you want to delete, select **Delete**.</span></span>
5. <span data-ttu-id="0e233-163">Kattintson a törlés jóváhagyásához **Igen**.</span><span class="sxs-lookup"><span data-stu-id="0e233-163">Confirm the deletion by clicking **yes**.</span></span>
6. <span data-ttu-id="0e233-164">Az alkalmazásjelszót a törölt kattinthat **bezárása**.</span><span class="sxs-lookup"><span data-stu-id="0e233-164">Once the app password is deleted, you can click **close**.</span></span>


## <a name="manage-app-passwords-with-the-myapps-portal"></a><span data-ttu-id="0e233-165">A MyApps portal szolgáltatással használt alkalmazásjelszók kezelése.</span><span class="sxs-lookup"><span data-stu-id="0e233-165">Manage app passwords with the MyApps portal.</span></span>
<span data-ttu-id="0e233-166">Ha nem biztos abban, hogy a multi-factor authentication használatát, majd bármikor létrehozása és törlése a myapps portálon keresztül alkalmazásjelszókat.</span><span class="sxs-lookup"><span data-stu-id="0e233-166">If you are not sure how you use multi-factor authentication, then you can always create and delete app passwords through the myapps portal.</span></span>

### <a name="to-create-an-app-password-using-the-myapps-portal"></a><span data-ttu-id="0e233-167">A hozza létre az alkalmazásjelszót a Myapps portál használatával</span><span class="sxs-lookup"><span data-stu-id="0e233-167">To create an app password using the Myapps portal</span></span>
1. <span data-ttu-id="0e233-168">Jelentkezzen be [https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="0e233-168">Sign in to [https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="0e233-169">Kattintson a jobb felső sarokban, és válassza a **profil**.</span><span class="sxs-lookup"><span data-stu-id="0e233-169">Click your name at the top right, and choose **Profile**.</span></span>
3. <span data-ttu-id="0e233-170">Válassza ki **további biztonsági ellenőrzés**.</span><span class="sxs-lookup"><span data-stu-id="0e233-170">Select **Additional Security Verification**.</span></span>
   <span data-ttu-id="0e233-171">![Válassza ki a további biztonsági ellenőrzés – képernyőkép](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span><span class="sxs-lookup"><span data-stu-id="0e233-171">![Select additional security verification - screenshot](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span></span>

4. <span data-ttu-id="0e233-172">Válassza ki **alkalmazásjelszók**.</span><span class="sxs-lookup"><span data-stu-id="0e233-172">Select **app passwords**.</span></span>
   <span data-ttu-id="0e233-173">![Válassza ki az alkalmazásjelszók – képernyőkép](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span><span class="sxs-lookup"><span data-stu-id="0e233-173">![Select app passwords - screenshot](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span></span>

5. <span data-ttu-id="0e233-174">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0e233-174">Click **Create**.</span></span>
6. <span data-ttu-id="0e233-175">Adjon meg egy nevet a jelszót, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0e233-175">Enter a name for the app password and click **Next**.</span></span>
7. <span data-ttu-id="0e233-176">A jelszót a vágólapra másolja és illessze be az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0e233-176">Copy the app password to the clipboard and paste it into your app.</span></span>
   <span data-ttu-id="0e233-177">![Hozza létre az alkalmazásjelszót](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span><span class="sxs-lookup"><span data-stu-id="0e233-177">![Create an app password](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span></span>

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a><span data-ttu-id="0e233-178">Törli az alkalmazásjelszó Myapps portál használatával</span><span class="sxs-lookup"><span data-stu-id="0e233-178">To delete an app password using the Myapps portal</span></span>
1. <span data-ttu-id="0e233-179">Jelentkezzen be [https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="0e233-179">Sign in to [https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="0e233-180">A lap tetején jelölje ki a profilt.</span><span class="sxs-lookup"><span data-stu-id="0e233-180">At the top, select profile.</span></span>
3. <span data-ttu-id="0e233-181">Válassza ki **további biztonsági ellenőrzés**.</span><span class="sxs-lookup"><span data-stu-id="0e233-181">Select **Additional Security Verification**.</span></span>

   ![Válassza ki a további biztonsági ellenőrzés – képernyőkép](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. <span data-ttu-id="0e233-183">Válassza ki **alkalmazásjelszók**.</span><span class="sxs-lookup"><span data-stu-id="0e233-183">Select **app passwords**.</span></span>

   ![Válassza ki az alkalmazásjelszók – képernyőkép](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. <span data-ttu-id="0e233-185">A törölni kívánt alkalmazásjelszót, mellett kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="0e233-185">Next to the app password you want to delete, click **Delete**.</span></span>

   ![Az alkalmazásjelszó törlése](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. <span data-ttu-id="0e233-187">Győződjön meg arról, hogy szeretné-e, hogy a jelszó törlése gombra kattintva **Igen**.</span><span class="sxs-lookup"><span data-stu-id="0e233-187">Confirm that you want to delete that password by clicking **yes**.</span></span>
7. <span data-ttu-id="0e233-188">Az alkalmazásjelszót a törölt kattinthat **bezárása**.</span><span class="sxs-lookup"><span data-stu-id="0e233-188">Once the app password is deleted, you can click **close**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e233-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0e233-189">Next steps</span></span>

- [<span data-ttu-id="0e233-190">A kétlépéses ellenőrzés beállításait kezelheti</span><span class="sxs-lookup"><span data-stu-id="0e233-190">Manage your two-step verification settings</span></span>](multi-factor-authentication-end-user-manage-settings.md)

- <span data-ttu-id="0e233-191">Próbálja ki a [Microsoft Authenticator alkalmazás](microsoft-authenticator-app-how-to.md) ellenőrzése a bejelentkezéseket app értesítések szövegek vagy hívások fogadása helyett.</span><span class="sxs-lookup"><span data-stu-id="0e233-191">Try out the [Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) to verify your sign-ins with app notifications, instead of receiving texts or calls.</span></span> 
