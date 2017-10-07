---
title: "aaaHow toouse Alkalmazásjelszók az Azure MFA? | Microsoft Docs"
description: "Ezen a lapon segít megérteni az alkalmazásjelszók és a definíció a felhasználóknak az MFA legutóbb tooAzure használatos."
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
ms.openlocfilehash: 3afa2003d8e87576f035bf9440a1dba67bd85f5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a><span data-ttu-id="24e66-104">Mik a Azure multi-factor Authentication Alkalmazásjelszókat?</span><span class="sxs-lookup"><span data-stu-id="24e66-104">What are App Passwords in Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="24e66-105">Bizonyos böngészőn kívüli alkalmazások, többek között a hello Apple natív e-mail-ügyfélprogram, amely használja az Exchange Active Sync jelenleg nem támogatják a multi-factor authentication.</span><span class="sxs-lookup"><span data-stu-id="24e66-105">Certain non-browser apps, such as hello Apple native email client that uses Exchange Active Sync, currently do not support multi-factor authentication.</span></span> <span data-ttu-id="24e66-106">Többtényezős hitelesítést felhasználónként kell engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="24e66-106">Multi-factor authentication is enabled per user.</span></span>  <span data-ttu-id="24e66-107">Ez azt jelenti, hogy a felhasználó multi-factor Authentication hitelesítés nem használható, ha:</span><span class="sxs-lookup"><span data-stu-id="24e66-107">This means that a user can't use multi-factor authentication if:</span></span>

- <span data-ttu-id="24e66-108">hello felhasználó engedélyezte a többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="24e66-108">hello user has been enabled for multi-factor authentication</span></span>
- <span data-ttu-id="24e66-109">hello felhasználó próbál toouse a böngészőn kívüli alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="24e66-109">hello user is trying toouse a non-browser app.</span></span>

<span data-ttu-id="24e66-110">Az alkalmazásjelszó lehetővé teszi, hogy a hello felhasználói toouse hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="24e66-110">An app password allows hello user toouse hello app.</span></span>

<span data-ttu-id="24e66-111">Miután egy alkalmazásjelszót, használhatja az eredeti jelszóval ezen böngészőn kívüli alkalmazások helyett.</span><span class="sxs-lookup"><span data-stu-id="24e66-111">Once you have an app password, you use it in place of your original password with these non-browser apps.</span></span> <span data-ttu-id="24e66-112">Amikor regisztrál a kétlépéses ellenőrzést, akkor még szólítja fel nem toolet bárki aláírására Microsoft be a jelszót még nem végzett hello második ellenőrzési.</span><span class="sxs-lookup"><span data-stu-id="24e66-112">When you register for two-step verification, you're telling Microsoft not toolet anyone sign in with your password if they can't also perform hello second verification.</span></span> <span data-ttu-id="24e66-113">hello Apple natív e-mail-ügyfélprogram telefonján nem tud bejelentkezni, ha Ön, mert nem a kétlépéses ellenőrzéshez kérje meg.</span><span class="sxs-lookup"><span data-stu-id="24e66-113">hello Apple native email client on your phone can't sign in as you because it can't ask for two-step verification.</span></span> <span data-ttu-id="24e66-114">hello megoldás toothis probléma egy biztonságosabb alkalmazásjelszót, amelyek nem használják toocreate napi.</span><span class="sxs-lookup"><span data-stu-id="24e66-114">hello solution toothis problem is toocreate a more secure app password that you don't use day-to-day.</span></span> <span data-ttu-id="24e66-115">Az alkalmazásjelszók csak ezek az alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="24e66-115">App passwords are only for those apps that can't support two-step verification.</span></span> <span data-ttu-id="24e66-116">Hello alkalmazásjelszót használnak, így alkalmazások multi-factor authentication kihagyásához és toowork folytatni.</span><span class="sxs-lookup"><span data-stu-id="24e66-116">Use hello app password so that apps can bypass multi-factor authentication and continue toowork.</span></span>


> [!NOTE]
> <span data-ttu-id="24e66-117">Office 2013-ügyfelek (például az Outlook) támogatja az új hitelesítési protokollok és a kétlépéses ellenőrzéshez használttal használható.</span><span class="sxs-lookup"><span data-stu-id="24e66-117">Office 2013 clients (including Outlook) support new authentication protocols and can be used with two-step verification.</span></span> <span data-ttu-id="24e66-118">Alkalmazásjelszók esetén nincs szükség Office 2013 ügyfelek való használatra.</span><span class="sxs-lookup"><span data-stu-id="24e66-118">App passwords are not required for use with Office 2013 clients.</span></span>  <span data-ttu-id="24e66-119">További információkért lásd: [Office 2013 modern hitelesítés nyilvános előzetes bejelentette](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span><span class="sxs-lookup"><span data-stu-id="24e66-119">For more information, see [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span></span>


## <a name="how-toouse-app-passwords"></a><span data-ttu-id="24e66-120">Hogyan toouse alkalmazásjelszók</span><span class="sxs-lookup"><span data-stu-id="24e66-120">How toouse app passwords</span></span>
<span data-ttu-id="24e66-121">Az alábbiakban néhány dolgot tooknow alkalmazásjelszókról:</span><span class="sxs-lookup"><span data-stu-id="24e66-121">Here are some things tooknow about app passwords:</span></span>

* <span data-ttu-id="24e66-122">Ne hozzon létre egy saját alkalmazásjelszókat.</span><span class="sxs-lookup"><span data-stu-id="24e66-122">You don't create your own app passwords.</span></span> <span data-ttu-id="24e66-123">Ezek automatikusan jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="24e66-123">They are automatically generated.</span></span>
* <span data-ttu-id="24e66-124">Jelenleg csak egy legfeljebb 40 jelszó felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="24e66-124">Currently there is a limit of 40 passwords per user.</span></span> 
* <span data-ttu-id="24e66-125">Ha hello korlát elérése után megpróbál toocreate alkalmazásjelszót, akkor kell toodelete egyet a meglévő alkalmazásjelszavak előtt hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="24e66-125">If you try toocreate an app password after you have reached hello limit, you'll have toodelete one of your existing app passwords before you create a new one.</span></span>
* <span data-ttu-id="24e66-126">Minden eszközhöz, ne alkalmazásonként egy alkalmazásjelszót használni.</span><span class="sxs-lookup"><span data-stu-id="24e66-126">Use one app password per device, not per application.</span></span> <span data-ttu-id="24e66-127">Például hozzon létre egy alkalmazásjelszót a hordozható számítógép, és azt használja az összes, az alkalmazások adott hordozható számítógépen.</span><span class="sxs-lookup"><span data-stu-id="24e66-127">For example, you can create one app password for your laptop and use that app password for all of your applications on that laptop.</span></span> <span data-ttu-id="24e66-128">Ezután hozzon létre egy második app jelszó toouse az alkalmazások az asztalon.</span><span class="sxs-lookup"><span data-stu-id="24e66-128">Then, create a second app password toouse for all your apps on your desktop.</span></span> 
* <span data-ttu-id="24e66-129">Lehetősége van egy alkalmazás jelszó hello első alkalommal regisztrál a kétlépéses ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="24e66-129">You are given one app password hello first time you register for two-step verification.</span></span>  <span data-ttu-id="24e66-130">Ha további megfelelően van szüksége, létrehozhat őket.</span><span class="sxs-lookup"><span data-stu-id="24e66-130">If you need additional ones, you can create them.</span></span>



## <a name="creating-and-deleting-app-passwords"></a><span data-ttu-id="24e66-131">Létrehozása és törlése alkalmazásjelszók</span><span class="sxs-lookup"><span data-stu-id="24e66-131">Creating and deleting app passwords</span></span>
<span data-ttu-id="24e66-132">Az első bejelentkezés során lehetősége van az alkalmazásjelszó használható.</span><span class="sxs-lookup"><span data-stu-id="24e66-132">During your initial sign-in, you are given an app password that you can use.</span></span>  <span data-ttu-id="24e66-133">Hozhat létre, és később törölni alkalmazásjelszókat.</span><span class="sxs-lookup"><span data-stu-id="24e66-133">You can also create and delete app passwords later on.</span></span> <span data-ttu-id="24e66-134">Alkalmazásjelszók törlési módja attól függ, hogy többtényezős hitelesítés használatát.</span><span class="sxs-lookup"><span data-stu-id="24e66-134">How you delete app passwords depends on how you use multi-factor authentication.</span></span> <span data-ttu-id="24e66-135">A következő válaszfájl hello toomanage alkalmazásjelszók hová kell toodetermine kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="24e66-135">Answer hello following questions toodetermine where you should go toomanage app passwords:</span></span> 

1. <span data-ttu-id="24e66-136">Használhatók a kétlépéses ellenőrzést személyes Microsoft-fiókja?</span><span class="sxs-lookup"><span data-stu-id="24e66-136">Do you use two-step verification for your personal Microsoft account?</span></span> <span data-ttu-id="24e66-137">Ha igen, akkor olvassa el toohello [alkalmazásjelszók és a kétlépéses ellenőrzést](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) cikk segítségét.</span><span class="sxs-lookup"><span data-stu-id="24e66-137">If yes, you should refer toohello [App passwords and two-step verification](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) article for help.</span></span> <span data-ttu-id="24e66-138">Ha nem, továbbra is két tooquestion.</span><span class="sxs-lookup"><span data-stu-id="24e66-138">If no, continue tooquestion two.</span></span>

2. <span data-ttu-id="24e66-139">OK, hogy a kétlépéses ellenőrzést használni a munkahelyi vagy iskolai fiókját.</span><span class="sxs-lookup"><span data-stu-id="24e66-139">Ok, so you use two-step verification for your work or school account.</span></span> <span data-ttu-id="24e66-140">Használja az toosign tooOffice 365 alkalmazásokban?</span><span class="sxs-lookup"><span data-stu-id="24e66-140">Do you use it toosign in tooOffice 365 apps?</span></span> <span data-ttu-id="24e66-141">Ha igen, tekintse át túl[hozza létre az alkalmazásjelszót az Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) segítségét.</span><span class="sxs-lookup"><span data-stu-id="24e66-141">If yes, you should refer too[Create an app password for Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) for help.</span></span> <span data-ttu-id="24e66-142">Ha nem, továbbra is három tooquestion.</span><span class="sxs-lookup"><span data-stu-id="24e66-142">If no, continue tooquestion three.</span></span> 

3. <span data-ttu-id="24e66-143">Használhatók a kétlépéses ellenőrzésről a Microsoft Azure-ban?</span><span class="sxs-lookup"><span data-stu-id="24e66-143">Do you use two-step verification with Microsoft Azure?</span></span> <span data-ttu-id="24e66-144">Ha igen, akkor folytassa a toohello [hello Azure portálra az alkalmazás-jelszavak kezelése](#manage-app-passwords-in-the-Azure-portal) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="24e66-144">If yes, continue toohello [Manage app passwords in hello Azure portal](#manage-app-passwords-in-the-Azure-portal) section of this article.</span></span> <span data-ttu-id="24e66-145">Ha nem, továbbra is tooquestion négy.</span><span class="sxs-lookup"><span data-stu-id="24e66-145">If no, continue tooquestion four.</span></span>

4. <span data-ttu-id="24e66-146">Nem biztos benne, ahol használhatja a kétlépéses ellenőrzést?</span><span class="sxs-lookup"><span data-stu-id="24e66-146">Not sure where you use two-step verification?</span></span> <span data-ttu-id="24e66-147">Továbbra is toohello [hello MyApps portal szolgáltatással használt alkalmazásjelszók kezelése](#manage-app-passwords-with-the-myapps-portal) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="24e66-147">Continue toohello [Manage app passwords with hello MyApps portal](#manage-app-passwords-with-the-myapps-portal) section of this article.</span></span> 


## <a name="manage-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="24e66-148">Hello Azure portálra az alkalmazás-jelszavak kezelése</span><span class="sxs-lookup"><span data-stu-id="24e66-148">Manage app passwords in hello Azure portal</span></span>
<span data-ttu-id="24e66-149">Az Azure-ral használhatja a kétlépéses ellenőrzést, ha azt szeretné, toocreate alkalmazásjelszók hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="24e66-149">If you use two-step verification with Azure, you want toocreate app passwords through hello Azure portal.</span></span>

### <a name="toocreate-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="24e66-150">alkalmazásjelszók toocreate a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="24e66-150">toocreate app passwords in hello Azure portal</span></span>
1. <span data-ttu-id="24e66-151">Jelentkezzen be toohello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="24e66-151">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="24e66-152">Hello tetején kattintson a jobb gombbal a felhasználónevét, és válassza ki a további biztonsági ellenőrzés.</span><span class="sxs-lookup"><span data-stu-id="24e66-152">At hello top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="24e66-153">Hello proofup lapon hello bal felső válassza ki az alkalmazásjelszók</span><span class="sxs-lookup"><span data-stu-id="24e66-153">On hello proofup page, at hello top, select app passwords</span></span>
4. <span data-ttu-id="24e66-154">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="24e66-154">Click **Create**.</span></span>
5. <span data-ttu-id="24e66-155">Adjon meg egy nevet hello alkalmazásjelszót, majd kattintson **tovább**</span><span class="sxs-lookup"><span data-stu-id="24e66-155">Enter a name for hello app password and click **Next**</span></span>
6. <span data-ttu-id="24e66-156">Hello app jelszó toohello vágólapra másolja és illessze be az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="24e66-156">Copy hello app password toohello clipboard and paste it into your app.</span></span>
   
   ![Felhő](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="toodelete-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="24e66-158">alkalmazásjelszók toodelete a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="24e66-158">toodelete app passwords in hello Azure portal</span></span>
1. <span data-ttu-id="24e66-159">Jelentkezzen be toohello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="24e66-159">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="24e66-160">Hello tetején kattintson a jobb gombbal a felhasználónevét, és válassza ki a további biztonsági ellenőrzés.</span><span class="sxs-lookup"><span data-stu-id="24e66-160">At hello top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="24e66-161">Hello bal felső, tovább tooadditional biztonsági ellenőrzési, válasszon **alkalmazásjelszókat.**</span><span class="sxs-lookup"><span data-stu-id="24e66-161">At hello top, next tooadditional security verification, select **app passwords.**</span></span>
4. <span data-ttu-id="24e66-162">Azt szeretné, hogy toodelete, jelölje be tovább toohello alkalmazásjelszót **törlése**.</span><span class="sxs-lookup"><span data-stu-id="24e66-162">Next toohello app password you want toodelete, select **Delete**.</span></span>
5. <span data-ttu-id="24e66-163">Hello törlésének megerősítése kattintva **Igen**.</span><span class="sxs-lookup"><span data-stu-id="24e66-163">Confirm hello deletion by clicking **yes**.</span></span>
6. <span data-ttu-id="24e66-164">A törölt hello alkalmazásjelszót kattinthat **bezárása**.</span><span class="sxs-lookup"><span data-stu-id="24e66-164">Once hello app password is deleted, you can click **close**.</span></span>


## <a name="manage-app-passwords-with-hello-myapps-portal"></a><span data-ttu-id="24e66-165">Hello MyApps portal szolgáltatással használt alkalmazásjelszók kezelése.</span><span class="sxs-lookup"><span data-stu-id="24e66-165">Manage app passwords with hello MyApps portal.</span></span>
<span data-ttu-id="24e66-166">Ha nem biztos abban, hogy a multi-factor authentication használatát, majd bármikor létrehozása és törlése alkalmazásjelszók hello myapps portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="24e66-166">If you are not sure how you use multi-factor authentication, then you can always create and delete app passwords through hello myapps portal.</span></span>

### <a name="toocreate-an-app-password-using-hello-myapps-portal"></a><span data-ttu-id="24e66-167">egy alkalmazás jelszó használatával toocreate hello Myapps portál</span><span class="sxs-lookup"><span data-stu-id="24e66-167">toocreate an app password using hello Myapps portal</span></span>
1. <span data-ttu-id="24e66-168">Jelentkezzen be a túl[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="24e66-168">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="24e66-169">Kattintson a jobb oldali hello tetején nevére, és válassza a **profil**.</span><span class="sxs-lookup"><span data-stu-id="24e66-169">Click your name at hello top right, and choose **Profile**.</span></span>
3. <span data-ttu-id="24e66-170">Válassza ki **további biztonsági ellenőrzés**.</span><span class="sxs-lookup"><span data-stu-id="24e66-170">Select **Additional Security Verification**.</span></span>
   <span data-ttu-id="24e66-171">![Válassza ki a további biztonsági ellenőrzés – képernyőkép](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span><span class="sxs-lookup"><span data-stu-id="24e66-171">![Select additional security verification - screenshot](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span></span>

4. <span data-ttu-id="24e66-172">Válassza ki **alkalmazásjelszók**.</span><span class="sxs-lookup"><span data-stu-id="24e66-172">Select **app passwords**.</span></span>
   <span data-ttu-id="24e66-173">![Válassza ki az alkalmazásjelszók – képernyőkép](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span><span class="sxs-lookup"><span data-stu-id="24e66-173">![Select app passwords - screenshot](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span></span>

5. <span data-ttu-id="24e66-174">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="24e66-174">Click **Create**.</span></span>
6. <span data-ttu-id="24e66-175">Adjon meg egy nevet hello alkalmazásjelszót, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="24e66-175">Enter a name for hello app password and click **Next**.</span></span>
7. <span data-ttu-id="24e66-176">Hello app jelszó toohello vágólapra másolja és illessze be az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="24e66-176">Copy hello app password toohello clipboard and paste it into your app.</span></span>
   <span data-ttu-id="24e66-177">![Hozza létre az alkalmazásjelszót](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span><span class="sxs-lookup"><span data-stu-id="24e66-177">![Create an app password](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span></span>

### <a name="toodelete-an-app-password-using-hello-myapps-portal"></a><span data-ttu-id="24e66-178">egy alkalmazás jelszó használatával toodelete hello Myapps portál</span><span class="sxs-lookup"><span data-stu-id="24e66-178">toodelete an app password using hello Myapps portal</span></span>
1. <span data-ttu-id="24e66-179">Jelentkezzen be a túl[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="24e66-179">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="24e66-180">Hello bal felső válassza ki a profilt.</span><span class="sxs-lookup"><span data-stu-id="24e66-180">At hello top, select profile.</span></span>
3. <span data-ttu-id="24e66-181">Válassza ki **további biztonsági ellenőrzés**.</span><span class="sxs-lookup"><span data-stu-id="24e66-181">Select **Additional Security Verification**.</span></span>

   ![Válassza ki a további biztonsági ellenőrzés – képernyőkép](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. <span data-ttu-id="24e66-183">Válassza ki **alkalmazásjelszók**.</span><span class="sxs-lookup"><span data-stu-id="24e66-183">Select **app passwords**.</span></span>

   ![Válassza ki az alkalmazásjelszók – képernyőkép](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. <span data-ttu-id="24e66-185">Kattintson a kívánt toodelete, tovább toohello alkalmazásjelszót **törlése**.</span><span class="sxs-lookup"><span data-stu-id="24e66-185">Next toohello app password you want toodelete, click **Delete**.</span></span>

   ![Az alkalmazásjelszó törlése](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. <span data-ttu-id="24e66-187">Erősítse meg toodelete, hogy a jelszó kattintva **Igen**.</span><span class="sxs-lookup"><span data-stu-id="24e66-187">Confirm that you want toodelete that password by clicking **yes**.</span></span>
7. <span data-ttu-id="24e66-188">A törölt hello alkalmazásjelszót kattinthat **bezárása**.</span><span class="sxs-lookup"><span data-stu-id="24e66-188">Once hello app password is deleted, you can click **close**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24e66-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24e66-189">Next steps</span></span>

- [<span data-ttu-id="24e66-190">A kétlépéses ellenőrzés beállításait kezelheti</span><span class="sxs-lookup"><span data-stu-id="24e66-190">Manage your two-step verification settings</span></span>](multi-factor-authentication-end-user-manage-settings.md)

- <span data-ttu-id="24e66-191">Próbálja ki hello [Microsoft Authenticator alkalmazás](microsoft-authenticator-app-how-to.md) tooverify a bejelentkezéseket app értesítések szövegek vagy hívások fogadása helyett.</span><span class="sxs-lookup"><span data-stu-id="24e66-191">Try out hello [Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) tooverify your sign-ins with app notifications, instead of receiving texts or calls.</span></span> 
