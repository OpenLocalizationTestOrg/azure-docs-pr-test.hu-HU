---
title: "Azure hozzáférési Panel bővítményét az Internet Explorer egy csoportházirend-objektummal aaaDeploy |} Microsoft Docs"
description: "Hogyan toouse csoport házirend toodeploy hello Internet Explorer bővítmény hello saját alkalmazások portálhoz."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 7c2d49c8-5be0-4e7e-abac-332f9dfda736
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 926f15950bbe81d2fbfe1b0b856470da40880d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-hello-access-panel-extension-for-internet-explorer-using-group-policy"></a><span data-ttu-id="0d77c-103">Hogyan tooDeploy hello hozzáférési Panel bővítményét az Internet Explorer csoportházirend használatával</span><span class="sxs-lookup"><span data-stu-id="0d77c-103">How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy</span></span>
<span data-ttu-id="0d77c-104">Ez az oktatóanyag bemutatja, hogyan toouse csoport házirend tooremotely hello hozzáférési Panel bővítmény Internet Explorer telepítése a felhasználók gépeken.</span><span class="sxs-lookup"><span data-stu-id="0d77c-104">This tutorial shows how toouse group policy tooremotely install hello Access Panel extension for Internet Explorer on your users' machines.</span></span> <span data-ttu-id="0d77c-105">A bővítmény szükség, az Internet Explorer felhasználók, akik toosign szolgáltatással konfigurált alkalmazásokba [jelszó-alapú egyszeri bejelentkezést](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="0d77c-105">This extension is required for Internet Explorer users who need toosign into apps that are configured using [password-based single sign-on](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span></span>

<span data-ttu-id="0d77c-106">Javasoljuk, hogy a rendszergazdák hello telepítési bővítmény automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="0d77c-106">It is recommended that admins automate hello deployment of this extension.</span></span> <span data-ttu-id="0d77c-107">Ellenkező esetben a felhasználók toodownload rendelkezik, és hello-kiterjesztés telepítése, amelyek nagyon eséllyel fordulnak elő toouser hiba, és rendszergazdai engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="0d77c-107">Otherwise, users have toodownload and install hello extension themselves, which is prone toouser error and requires administrator permissions.</span></span> <span data-ttu-id="0d77c-108">Ez az oktatóanyag ismerteti egyik lehetséges módja automatizálása a szoftverek központi telepítése csoportházirend használatával.</span><span class="sxs-lookup"><span data-stu-id="0d77c-108">This tutorial covers one method of automating software deployments by using group policy.</span></span> [<span data-ttu-id="0d77c-109">További információ a csoportházirend.</span><span class="sxs-lookup"><span data-stu-id="0d77c-109">Learn more about group policy.</span></span>](https://technet.microsoft.com/windowsserver/bb310732.aspx)

<span data-ttu-id="0d77c-110">hello hozzáférési Panel bővítmény érhető el is [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) és [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), egyike sem szükséges rendszergazdai engedélyek tooinstall.</span><span class="sxs-lookup"><span data-stu-id="0d77c-110">hello Access Panel extension is also available for [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) and [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), neither of which require administrator permissions tooinstall.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d77c-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0d77c-111">Prerequisites</span></span>
* <span data-ttu-id="0d77c-112">Ezzel beállította [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), és a felhasználók gépek tooyour tartományhoz csatlakozott.</span><span class="sxs-lookup"><span data-stu-id="0d77c-112">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines tooyour domain.</span></span>
* <span data-ttu-id="0d77c-113">Hello "Beállítások módosítása" engedéllyel tooedit hello csoportházirend-objektumot (GPO) kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="0d77c-113">You must have hello "Edit settings" permission tooedit hello Group Policy Object (GPO).</span></span> <span data-ttu-id="0d77c-114">Alapértelmezés szerint a következő biztonsági csoportokat hello tagjai rendelkezik ilyen engedéllyel: tartományi rendszergazdák, a vállalati rendszergazdák és a Csoportházirend-létrehozó tulajdonosok.</span><span class="sxs-lookup"><span data-stu-id="0d77c-114">By default, members of hello following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> [<span data-ttu-id="0d77c-115">Részletek</span><span class="sxs-lookup"><span data-stu-id="0d77c-115">Learn more.</span></span>](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-hello-distribution-point"></a><span data-ttu-id="0d77c-116">1. lépés: Hello terjesztési pont létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d77c-116">Step 1: Create hello Distribution Point</span></span>
<span data-ttu-id="0d77c-117">Először egy hálózati helyre hello gépek által elérhető, amely a tooremotely telepítés hello bővítmény kívánja hello telepítőcsomag kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="0d77c-117">First, you must place hello installer package on a network location that can be accessed by hello machines that you wish tooremotely install hello extension on.</span></span> <span data-ttu-id="0d77c-118">toodo, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0d77c-118">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="0d77c-119">Jelentkezzen be rendszergazdaként toohello kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="0d77c-119">Log on toohello server as an administrator</span></span>
2. <span data-ttu-id="0d77c-120">A hello **Kiszolgálókezelő** ablakban nyissa meg túl**fájl- és tárolási szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-120">In hello **Server Manager** window, go too**Files and Storage Services**.</span></span>
   
    ![Nyissa meg a fájl- és tárolási szolgáltatások](./media/active-directory-saas-ie-group-policy/files-services.png)
3. <span data-ttu-id="0d77c-122">Nyissa meg toohello **megosztások** fülre. Kattintson a **feladatok** > **új megosztás...**</span><span class="sxs-lookup"><span data-stu-id="0d77c-122">Go toohello **Shares** tab. Then click **Tasks** > **New Share...**</span></span>
   
    ![Nyissa meg a fájl- és tárolási szolgáltatások](./media/active-directory-saas-ie-group-policy/shares.png)
4. <span data-ttu-id="0d77c-124">Teljes hello **új megosztás varázsló** és set engedélyek tooensure, hogy az elérhető a felhasználók gépek.</span><span class="sxs-lookup"><span data-stu-id="0d77c-124">Complete hello **New Share Wizard** and set permissions tooensure that it can be accessed from your users' machines.</span></span> [<span data-ttu-id="0d77c-125">További információ a megosztásokat.</span><span class="sxs-lookup"><span data-stu-id="0d77c-125">Learn more about shares.</span></span>](https://technet.microsoft.com/library/cc753175.aspx)
5. <span data-ttu-id="0d77c-126">Töltse le a Microsoft Windows Installer-csomag (.msi fájlt) a következő hello: [hozzáférési Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span><span class="sxs-lookup"><span data-stu-id="0d77c-126">Download hello following Microsoft Windows Installer package (.msi file): [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span></span>
6. <span data-ttu-id="0d77c-127">Másolja a hello telepítő szükséges tooa csomaghely hello megosztáson.</span><span class="sxs-lookup"><span data-stu-id="0d77c-127">Copy hello installer package tooa desired location on hello share.</span></span>
   
    ![Hello .msi toohello fájlmegosztásról.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. <span data-ttu-id="0d77c-129">Ellenőrizze, hogy az ügyfél gépek képesek tooaccess hello telepítőcsomag hello megosztásból.</span><span class="sxs-lookup"><span data-stu-id="0d77c-129">Verify that your client machines are able tooaccess hello installer package from hello share.</span></span> 

## <a name="step-2-create-hello-group-policy-object"></a><span data-ttu-id="0d77c-130">2. lépés: Hello csoportházirend-objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d77c-130">Step 2: Create hello Group Policy Object</span></span>
1. <span data-ttu-id="0d77c-131">Jelentkezzen be az Active Directory tartományi szolgáltatások (AD DS) telepítése üzemeltető toohello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="0d77c-131">Log on toohello server that hosts your Active Directory Domain Services (AD DS) installation.</span></span>
2. <span data-ttu-id="0d77c-132">A Kiszolgálókezelő hello, váltson túl**eszközök** > **csoportházirend-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-132">In hello Server Manager, go too**Tools** > **Group Policy Management**.</span></span>
   
    ![Nyissa meg tooTools > csoport házirend kezelése](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. <span data-ttu-id="0d77c-134">A bal oldali ablaktáblájában hello hello **csoportházirend-kezelő** ablakban, a szervezeti egység (OU) hierarchia megtekintése, és milyen hatókörben tooapply hello csoportházirend szeretné meghatározni.</span><span class="sxs-lookup"><span data-stu-id="0d77c-134">In hello left pane of hello **Group Policy Management** window, view your Organizational Unit (OU) hierarchy and determine at which scope you would like tooapply hello group policy.</span></span> <span data-ttu-id="0d77c-135">Dönthet egy kis OU toodeploy tooa toopick néhány felhasználó tesztelési, vagy például előfordulhat, hogy válassza ki a legfelső szintű szervezeti egység toodeploy tooyour teljes szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="0d77c-135">For instance, you may decide toopick a small OU toodeploy tooa few users for testing, or you may pick a top-level OU toodeploy tooyour entire organization.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0d77c-136">Volna, például toocreate vagy szerkesztésekor a szervezeti egységekhez (OU-k), hátsó toohello Kiszolgálókezelő váltson, és nyissa meg túl**eszközök** > **Active Directory – felhasználók és számítógépek**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-136">If you would like toocreate or edit your Organization Units (OUs), switch back toohello Server Manager and go too**Tools** > **Active Directory Users and Computers**.</span></span>
   > 
   > 
4. <span data-ttu-id="0d77c-137">Miután kiválasztotta a szervezeti egység, a jobb gombbal kattintson rá, és válassza ki **egy csoportházirend-objektum létrehozása ebben a tartományban, és hivatkozás létrehozása itt...**</span><span class="sxs-lookup"><span data-stu-id="0d77c-137">Once you have selected an OU, right-click it and select **Create a GPO in this domain, and Link it here...**</span></span>
   
    ![Hozzon létre egy új csoportházirend-objektum](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. <span data-ttu-id="0d77c-139">A hello **új csoportházirend-objektum** parancssorból, írja be egy nevet a hello új csoportházirend-objektum.</span><span class="sxs-lookup"><span data-stu-id="0d77c-139">In hello **New GPO** prompt, type in a name for hello new Group Policy Object.</span></span>
   
    ![Név hello új csoportházirend-objektum](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. <span data-ttu-id="0d77c-141">Kattintson a jobb gombbal hello csoportházirend-objektumot létrehozta, és válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-141">Right-click hello Group Policy Object that you created, and select **Edit**.</span></span>
   
    ![Hello új csoportházirend-objektum szerkesztéséhez](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-hello-installation-package"></a><span data-ttu-id="0d77c-143">3. lépés: Hello telepítőcsomag hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0d77c-143">Step 3: Assign hello Installation Package</span></span>
1. <span data-ttu-id="0d77c-144">Határozza meg, hogy szeretné-e toodeploy hello bővítmény alapján **számítógép konfigurációja** vagy **felhasználói konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-144">Determine whether you would like toodeploy hello extension based on **Computer Configuration** or **User Configuration**.</span></span> <span data-ttu-id="0d77c-145">Használata esetén [számítógép konfigurációja](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), függetlenül attól, amely a felhasználók jelentkezhetnek be tooit hello számítógépen telepítve van a hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="0d77c-145">When using [computer configuration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), hello extension is installed on hello computer regardless of which users log on tooit.</span></span> <span data-ttu-id="0d77c-146">A [felhasználói konfiguráció](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), felhasználóknál hello bővítmények vannak telepítve, függetlenül azok a számítógépek jelentkeznek be rájuk vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="0d77c-146">With [user configuration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), users have hello extension installed for them regardless of which computers they log on to.</span></span>
2. <span data-ttu-id="0d77c-147">A bal oldali ablaktáblájában hello hello **Csoportházirendkezelés-szerkesztő** ablakban, a következő mappák elérési útjaiban, attól függően, hogy milyen típusú konfigurációs úgy döntött, hogy hello lépjen tooeither:</span><span class="sxs-lookup"><span data-stu-id="0d77c-147">In hello left pane of hello **Group Policy Management Editor** window, go tooeither of hello following folder paths, depending on which type of configuration you chose:</span></span>
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. <span data-ttu-id="0d77c-148">Kattintson a jobb gombbal **Szoftvertelepítés**, majd jelölje be **új** > **csomag...**</span><span class="sxs-lookup"><span data-stu-id="0d77c-148">Right-click **Software installation**, then select **New** > **Package...**</span></span>
   
    ![Hozzon létre egy új telepítési csomag](./media/active-directory-saas-ie-group-policy/new-package.png)
4. <span data-ttu-id="0d77c-150">Lépjen toohello megosztott mappa, amely tartalmazza a telepítőcsomag hello a [1. lépés: hello terjesztési pont létrehozása](#step-1-create-the-distribution-point), válasszon hello .msi fájlt, és kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-150">Go toohello shared folder that contains hello installer package from [Step 1: Create hello Distribution Point](#step-1-create-the-distribution-point), select hello .msi file, and click **Open**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="0d77c-151">Ha hello megosztás ugyanazon a kiszolgálón található, győződjön meg arról, hogy Ön keresztül érik el hello .msi hello hálózati fájl elérési útját, nem pedig hello helyi fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="0d77c-151">If hello share is located on this same server, verify that you are accessing hello .msi through hello network file path, rather than hello local file path.</span></span>
   > 
   > 
   
    ![Válassza ki a telepítőcsomag hello hello megosztott mappából.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. <span data-ttu-id="0d77c-153">A hello **szoftver központi telepítése** rákérdezés, jelölje be **hozzárendelt** a központi telepítési módszer.</span><span class="sxs-lookup"><span data-stu-id="0d77c-153">In hello **Deploy Software** prompt, select **Assigned** for your deployment method.</span></span> <span data-ttu-id="0d77c-154">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0d77c-154">Then click **OK**.</span></span>
   
    ![Válassza ki a hozzárendelt, majd kattintson az OK gombra.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

<span data-ttu-id="0d77c-156">hello bővítmény már telepített toohello szervezeti Egységet, verziószámával.</span><span class="sxs-lookup"><span data-stu-id="0d77c-156">hello extension is now deployed toohello OU that you selected.</span></span> [<span data-ttu-id="0d77c-157">További információ a Csoportházirend szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="0d77c-157">Learn more about Group Policy Software Installation.</span></span>](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-hello-extension-for-internet-explorer"></a><span data-ttu-id="0d77c-158">4. lépés: Automatikus engedélyezése hello az Internet Explorer bővítmény</span><span class="sxs-lookup"><span data-stu-id="0d77c-158">Step 4: Auto-Enable hello Extension for Internet Explorer</span></span>
<span data-ttu-id="0d77c-159">Továbbá toorunning hello installer, az Internet Explorer minden bővítmény explicit módon engedélyezni kell ahhoz, hogy kell használni.</span><span class="sxs-lookup"><span data-stu-id="0d77c-159">In addition toorunning hello installer, every extension for Internet Explorer must be explicitly enabled before it can be used.</span></span> <span data-ttu-id="0d77c-160">Hajtsa végre hello lépéseket alatti tooenable hello hozzáférési Panel bővítmény csoportházirend használatával:</span><span class="sxs-lookup"><span data-stu-id="0d77c-160">Follow hello steps below tooenable hello Access Panel Extension using group policy:</span></span>

1. <span data-ttu-id="0d77c-161">A hello **Csoportházirendkezelés-szerkesztő** ablakot, az elérési utak, attól függően, hogy milyen típusú konfigurációs beállítást választja a következő hello lépjen tooeither [3. lépés: hozzárendelése hello telepítőcsomag](#step-3-assign-the-installation-package):</span><span class="sxs-lookup"><span data-stu-id="0d77c-161">In hello **Group Policy Management Editor** window, go tooeither of hello following paths, depending on which type of configuration you chose in [Step 3: Assign hello Installation Package](#step-3-assign-the-installation-package):</span></span>
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. <span data-ttu-id="0d77c-162">Kattintson a jobb gombbal **Bővítménylista**, és válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-162">Right-click **Add-on List**, and select **Edit**.</span></span>
    <span data-ttu-id="0d77c-163">![Bővítménylista szerkesztése.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span><span class="sxs-lookup"><span data-stu-id="0d77c-163">![Edit Add-on List.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span></span>
3. <span data-ttu-id="0d77c-164">A hello **Bővítménylista** ablakban válassza ki **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-164">In hello **Add-on List** window, select **Enabled**.</span></span> <span data-ttu-id="0d77c-165">Ekkor a hello **beállítások** kattintson **megjelenítése...** .</span><span class="sxs-lookup"><span data-stu-id="0d77c-165">Then, under hello **Options** section, click **Show...**.</span></span>
   
    ![Kattintson az Engedélyezés parancsra, majd kattintson a megjelenítése...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. <span data-ttu-id="0d77c-167">A hello **tartalom megjelenítése** ablak, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d77c-167">In hello **Show Contents** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="0d77c-168">Az első oszlop hello (hello **Azonosítónév** mező), másolja és illessze be a következő Osztályazonosító hello:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span><span class="sxs-lookup"><span data-stu-id="0d77c-168">For hello first column (hello **Value Name** field), copy and paste hello following Class ID: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span></span>
   2. <span data-ttu-id="0d77c-169">Hello második oszlop (hello **érték** mező), írja be a következő érték hello:`1`</span><span class="sxs-lookup"><span data-stu-id="0d77c-169">For hello second column (hello **Value** field), type in hello following value: `1`</span></span>
   3. <span data-ttu-id="0d77c-170">Kattintson a **OK** tooclose hello **tartalom megjelenítése** ablak.</span><span class="sxs-lookup"><span data-stu-id="0d77c-170">Click **OK** tooclose hello **Show Contents** window.</span></span>
      
      ![Töltse ki a fenti hello értékeket.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. <span data-ttu-id="0d77c-172">Kattintson a **OK** tooapply a módosításokat és Bezárás hello **Bővítménylista** ablak.</span><span class="sxs-lookup"><span data-stu-id="0d77c-172">Click **OK** tooapply your changes and close hello **Add-on List** window.</span></span>

<span data-ttu-id="0d77c-173">hello bővítményét most engedélyezni kell a kiválasztott hello hello gépek szervezeti Egységet.</span><span class="sxs-lookup"><span data-stu-id="0d77c-173">hello extension should now be enabled for hello machines in hello selected OU.</span></span> [<span data-ttu-id="0d77c-174">Csoport házirend tooenable használatával kapcsolatos további, vagy tiltsa le a gyakori.</span><span class="sxs-lookup"><span data-stu-id="0d77c-174">Learn more about using group policy tooenable or disable Internet Explorer add-ons.</span></span>](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a><span data-ttu-id="0d77c-175">5. lépés (nem kötelező): Prompt "Jelszó megjegyzése" letiltása</span><span class="sxs-lookup"><span data-stu-id="0d77c-175">Step 5 (Optional): Disable "Remember Password" Prompt</span></span>
<span data-ttu-id="0d77c-176">Amikor a felhasználók bejelentkezési toowebsites hello hozzáférési Panel bővítmény használatával, az Internet Explorer előfordulhat, hogy megjelenítése hello következő kérni, azzal a kérdéssel "szeretné toostore jelszavát?"</span><span class="sxs-lookup"><span data-stu-id="0d77c-176">When users sign-in toowebsites using hello Access Panel Extension, Internet Explorer may show hello following prompt asking "Would you like toostore your password?"</span></span>

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

<span data-ttu-id="0d77c-177">Ha tooprevent kívánja a felhasználók megtekinthessék a kérdéshez, majd lépésekkel hello automatikus kitöltés tooprevent alatt a jelszavak megjegyzése:</span><span class="sxs-lookup"><span data-stu-id="0d77c-177">If you wish tooprevent your users from seeing this prompt, then follow hello steps below tooprevent auto-complete from remembering passwords:</span></span>

1. <span data-ttu-id="0d77c-178">A hello **Csoportházirendkezelés-szerkesztő** ablakban, a lenti lépjen toohello elérési út.</span><span class="sxs-lookup"><span data-stu-id="0d77c-178">In hello **Group Policy Management Editor** window, go toohello path listed below.</span></span> <span data-ttu-id="0d77c-179">A konfigurációs beállítás csak érhető el a **felhasználói konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-179">This configuration setting is only available under **User Configuration**.</span></span>
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. <span data-ttu-id="0d77c-180">Nevű hello beállítás található **hello automatikus kitöltés szolgáltatás a felhasználónevek és jelszavak űrlapokon**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-180">Find hello setting named **Turn on hello auto-complete feature for user names and passwords on forms**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0d77c-181">Active Directory korábbi verzióiban ez a beállítás hello nevű sorolhatja **nem teszik lehetővé az automatikus kitöltés toosave jelszavak**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-181">Previous versions of Active Directory may list this setting with hello name **Do not allow auto-complete toosave passwords**.</span></span> <span data-ttu-id="0d77c-182">hello konfigurációs beállítás különbözik a hello beállítása ebben az oktatóanyagban leírt.</span><span class="sxs-lookup"><span data-stu-id="0d77c-182">hello configuration for that setting differs from hello setting described in this tutorial.</span></span>
   > 
   > 
   
    ![Ne felejtse el toolook ehhez a felhasználói beállítások szakasz.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. <span data-ttu-id="0d77c-184">Hello beállítás fölött kattintson jobb gombbal, és válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-184">Right click hello above setting, and select **Edit**.</span></span>
4. <span data-ttu-id="0d77c-185">Című hello ablakban **hello automatikus kitöltés szolgáltatás a felhasználónevek és jelszavak űrlapokon**, jelölje be **letiltott**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-185">In hello window titled **Turn on hello auto-complete feature for user names and passwords on forms**, select **Disabled**.</span></span>
   
    ![Válassza ki a letiltása](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. <span data-ttu-id="0d77c-187">Kattintson a **OK** tooapply ezeket a módosításokat és Bezárás hello ablak.</span><span class="sxs-lookup"><span data-stu-id="0d77c-187">Click **OK** tooapply these changes and close hello window.</span></span>

<span data-ttu-id="0d77c-188">Felhasználók többé nem kell tudni toostore hitelesítő adataikat, és automatikusan hajthat végre tooaccess korábban tárolt hitelesítő adatok használata.</span><span class="sxs-lookup"><span data-stu-id="0d77c-188">Users will no longer be able toostore their credentials or use auto-complete tooaccess previously stored credentials.</span></span> <span data-ttu-id="0d77c-189">Azonban ez a házirend lehetővé teszi felhasználók toocontinue toouse más típusú űrlapmezők, például a keresési mezők automatikusan hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="0d77c-189">However, this policy does allow users toocontinue toouse auto-complete for other types of form fields, such as search fields.</span></span>

> [!WARNING]
> <span data-ttu-id="0d77c-190">Ha a házirend engedélyezve van, miután a felhasználók kiválasztott toostore egyes hitelesítő adatokat, ez a házirend lesz *nem* hello már tárolt hitelesítő adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="0d77c-190">If this policy is enabled after users have chosen toostore some credentials, this policy will *not* clear hello credentials that have already been stored.</span></span>
> 
> 

## <a name="step-6-testing-hello-deployment"></a><span data-ttu-id="0d77c-191">6. lépés: Hello központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0d77c-191">Step 6: Testing hello Deployment</span></span>
<span data-ttu-id="0d77c-192">Lépésekkel hello tooverify alatt Ha hello bővítmény telepítése sikeres volt:</span><span class="sxs-lookup"><span data-stu-id="0d77c-192">Follow hello steps below tooverify if hello extension deployment was successful:</span></span>

1. <span data-ttu-id="0d77c-193">Ha használatával telepített **számítógép konfigurációja**, jelentkezzen be egy ügyfélszámítógépre, amelyhez tartozik a kiválasztott OU toohello [2. lépés: hello csoportházirend-objektum létrehozása](#step-2-create-the-group-policy-object).</span><span class="sxs-lookup"><span data-stu-id="0d77c-193">If you deployed using **Computer Configuration**, sign into a client machine that belongs toohello OU that you selected in [Step 2: Create hello Group Policy Object](#step-2-create-the-group-policy-object).</span></span> <span data-ttu-id="0d77c-194">Ha használatával telepített **felhasználói konfiguráció**, győződjön meg arról, hogy toosign a toothat szervezeti Egységhez tartozik felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="0d77c-194">If you deployed using **User Configuration**, make sure toosign in as a user who belongs toothat OU.</span></span>
2. <span data-ttu-id="0d77c-195">Ez eltarthat néhány bejelentkezési hello csoportházirend modulok toofully frissítés változik-e.</span><span class="sxs-lookup"><span data-stu-id="0d77c-195">It may take a couple sign ins for hello group policy changes toofully update with this machine.</span></span> <span data-ttu-id="0d77c-196">tooforce hello frissítés, nyissa meg a **parancssor** ablakot, és futtassa a következő parancs hello:`gpupdate /force`</span><span class="sxs-lookup"><span data-stu-id="0d77c-196">tooforce hello update, open a **Command Prompt** window and run hello following command: `gpupdate /force`</span></span>
3. <span data-ttu-id="0d77c-197">Hello telepítési tootake hely hello gépet újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="0d77c-197">You must restart hello machine for hello installation tootake place.</span></span> <span data-ttu-id="0d77c-198">Rendszerindítási szokásos hello bővítmény során telepíti jelentősen több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="0d77c-198">Bootup may take significantly more time than usual while hello extension installs.</span></span>
4. <span data-ttu-id="0d77c-199">Az újraindítás után nyissa meg a **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-199">After restarting, open **Internet Explorer**.</span></span> <span data-ttu-id="0d77c-200">A hello ablak hello jobb felső sarkában kattintson **eszközök** (hello fogaskerék ikonra), majd válassza ki **bővítmények kezelése**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-200">On hello upper-right corner of hello window, click **Tools** (hello gear icon), and then select **Manage add-ons**.</span></span>
   
    ![Nyissa meg tooTools > Bővítmények kezelése](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. <span data-ttu-id="0d77c-202">A hello **bővítmények kezelése** ablakban győződjön meg arról, hogy hello **hozzáférési Panel bővítmény** telepítve van, és hogy a **állapot** túl lett beállítva**engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="0d77c-202">In hello **Manage Add-ons** window, verify that hello **Access Panel Extension** has been installed and that its **Status** has been set too**Enabled**.</span></span>
   
    ![Győződjön meg arról, hogy hello hozzáférési Panel bővítmény van telepítve és engedélyezve van.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a><span data-ttu-id="0d77c-204">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="0d77c-204">Related Articles</span></span>
* [<span data-ttu-id="0d77c-205">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="0d77c-205">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="0d77c-206">Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="0d77c-206">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="0d77c-207">Az Internet Explorer hello hozzáférési Panel bővítmény hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="0d77c-207">Troubleshooting hello Access Panel Extension for Internet Explorer</span></span>](active-directory-saas-ie-troubleshooting.md)

