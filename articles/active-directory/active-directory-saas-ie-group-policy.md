---
title: "Az Internet Explorer egy csoportházirend-objektummal Azure hozzáférési Panel bővítmény telepítése |} Microsoft Docs"
description: "Hogyan kell az Internet Explorer bővítményt a saját alkalmazások portál telepítése a csoportházirenddel."
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
ms.openlocfilehash: b402ae326ab34ec71ad9de966e22be00045fee3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a><span data-ttu-id="d93c2-103">A hozzáférési Panel bővítmény telepítése az Internet Explorer csoportházirend használatával</span><span class="sxs-lookup"><span data-stu-id="d93c2-103">How to Deploy the Access Panel Extension for Internet Explorer using Group Policy</span></span>
<span data-ttu-id="d93c2-104">Ez az oktatóanyag bemutatja, hogyan távoli telepítése a felhasználók gépeken az Internet Explorer a hozzáférési Panel bővítmény a csoportházirend segítségével.</span><span class="sxs-lookup"><span data-stu-id="d93c2-104">This tutorial shows how to use group policy to remotely install the Access Panel extension for Internet Explorer on your users' machines.</span></span> <span data-ttu-id="d93c2-105">A bővítmény szükség az Internet Explorer felhasználók ki kell jelentkeznie az alkalmazásokat, amelyek használatával vannak konfigurálva [jelszó-alapú egyszeri bejelentkezést](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="d93c2-105">This extension is required for Internet Explorer users who need to sign into apps that are configured using [password-based single sign-on](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span></span>

<span data-ttu-id="d93c2-106">Javasoljuk, hogy a rendszergazdák a bővítmény telepítésének automatizálását.</span><span class="sxs-lookup"><span data-stu-id="d93c2-106">It is recommended that admins automate the deployment of this extension.</span></span> <span data-ttu-id="d93c2-107">Ellenkező esetben a felhasználóknak kell töltse le és telepítse a bővítményt, amely vannak téve a felhasználói és rendszergazdai engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="d93c2-107">Otherwise, users have to download and install the extension themselves, which is prone to user error and requires administrator permissions.</span></span> <span data-ttu-id="d93c2-108">Ez az oktatóanyag ismerteti egyik lehetséges módja automatizálása a szoftverek központi telepítése csoportházirend használatával.</span><span class="sxs-lookup"><span data-stu-id="d93c2-108">This tutorial covers one method of automating software deployments by using group policy.</span></span> [<span data-ttu-id="d93c2-109">További információ a csoportházirend.</span><span class="sxs-lookup"><span data-stu-id="d93c2-109">Learn more about group policy.</span></span>](https://technet.microsoft.com/windowsserver/bb310732.aspx)

<span data-ttu-id="d93c2-110">A hozzáférési Panel bővítmény érhető el is [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) és [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), egyike sem telepítéséhez rendszergazdai jogosultság szükséges.</span><span class="sxs-lookup"><span data-stu-id="d93c2-110">The Access Panel extension is also available for [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) and [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), neither of which require administrator permissions to install.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d93c2-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d93c2-111">Prerequisites</span></span>
* <span data-ttu-id="d93c2-112">Ezzel beállította [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), és a felhasználók gépek csatlakozott a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="d93c2-112">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines to your domain.</span></span>
* <span data-ttu-id="d93c2-113">A csoportházirend-objektum (GPO) szerkesztése "Beállítások módosítása" engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="d93c2-113">You must have the "Edit settings" permission to edit the Group Policy Object (GPO).</span></span> <span data-ttu-id="d93c2-114">Alapértelmezés szerint a következő biztonsági csoportok tagjai ezzel az engedéllyel rendelkeznek: a tartományi rendszergazdák, a vállalati rendszergazdák és a Csoportházirend-létrehozó tulajdonosok.</span><span class="sxs-lookup"><span data-stu-id="d93c2-114">By default, members of the following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> [<span data-ttu-id="d93c2-115">Részletek</span><span class="sxs-lookup"><span data-stu-id="d93c2-115">Learn more.</span></span>](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a><span data-ttu-id="d93c2-116">1. lépés: A terjesztési pont létrehozása</span><span class="sxs-lookup"><span data-stu-id="d93c2-116">Step 1: Create the Distribution Point</span></span>
<span data-ttu-id="d93c2-117">A telepítőcsomag először a távoli telepítéséhez a bővítményt a gépek által elérhető hálózati helyen kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="d93c2-117">First, you must place the installer package on a network location that can be accessed by the machines that you wish to remotely install the extension on.</span></span> <span data-ttu-id="d93c2-118">Ehhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d93c2-118">To do this, follow these steps:</span></span>

1. <span data-ttu-id="d93c2-119">Rendszergazdaként jelentkezzen be a kiszolgálóra</span><span class="sxs-lookup"><span data-stu-id="d93c2-119">Log on to the server as an administrator</span></span>
2. <span data-ttu-id="d93c2-120">Az a **Kiszolgálókezelő** ablakban, keresse fel **fájl- és tárolási szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-120">In the **Server Manager** window, go to **Files and Storage Services**.</span></span>
   
    ![Nyissa meg a fájl- és tárolási szolgáltatások](./media/active-directory-saas-ie-group-policy/files-services.png)
3. <span data-ttu-id="d93c2-122">Lépjen a **megosztások** fülre. Kattintson a **feladatok** > **új megosztás...**</span><span class="sxs-lookup"><span data-stu-id="d93c2-122">Go to the **Shares** tab. Then click **Tasks** > **New Share...**</span></span>
   
    ![Nyissa meg a fájl- és tárolási szolgáltatások](./media/active-directory-saas-ie-group-policy/shares.png)
4. <span data-ttu-id="d93c2-124">Fejezze be a **új megosztás varázsló** és annak érdekében, hogy az elérhető a felhasználók gépek engedélyek beállítása.</span><span class="sxs-lookup"><span data-stu-id="d93c2-124">Complete the **New Share Wizard** and set permissions to ensure that it can be accessed from your users' machines.</span></span> [<span data-ttu-id="d93c2-125">További információ a megosztásokat.</span><span class="sxs-lookup"><span data-stu-id="d93c2-125">Learn more about shares.</span></span>](https://technet.microsoft.com/library/cc753175.aspx)
5. <span data-ttu-id="d93c2-126">A következő Microsoft Windows Installer (.msi fájlt) csomag: [hozzáférési Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span><span class="sxs-lookup"><span data-stu-id="d93c2-126">Download the following Microsoft Windows Installer package (.msi file): [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span></span>
6. <span data-ttu-id="d93c2-127">Másolja a telepítőcsomag a megosztást a kívánt helyre.</span><span class="sxs-lookup"><span data-stu-id="d93c2-127">Copy the installer package to a desired location on the share.</span></span>
   
    ![Az .msi fájlt másolja a megosztásba.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. <span data-ttu-id="d93c2-129">Ellenőrizze, hogy az ügyfél gépek tudja a telepítőcsomag a megosztás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d93c2-129">Verify that your client machines are able to access the installer package from the share.</span></span> 

## <a name="step-2-create-the-group-policy-object"></a><span data-ttu-id="d93c2-130">2. lépés: A csoportházirend-objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="d93c2-130">Step 2: Create the Group Policy Object</span></span>
1. <span data-ttu-id="d93c2-131">Jelentkezzen be a kiszolgálóra, amelyen az Active Directory tartományi szolgáltatások (AD DS) telepítése.</span><span class="sxs-lookup"><span data-stu-id="d93c2-131">Log on to the server that hosts your Active Directory Domain Services (AD DS) installation.</span></span>
2. <span data-ttu-id="d93c2-132">A Kiszolgálókezelőben nyissa meg a **eszközök** > **csoportházirend-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-132">In the Server Manager, go to **Tools** > **Group Policy Management**.</span></span>
   
    ![Kattintson az eszközök > csoport házirend kezelése](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. <span data-ttu-id="d93c2-134">A bal oldali ablaktáblán, a **csoportházirend-kezelő** ablakban, a szervezeti egység (OU) hierarchia megtekintése, és határozza meg, melyik hatókörből szeretné alkalmazni a csoportházirend.</span><span class="sxs-lookup"><span data-stu-id="d93c2-134">In the left pane of the **Group Policy Management** window, view your Organizational Unit (OU) hierarchy and determine at which scope you would like to apply the group policy.</span></span> <span data-ttu-id="d93c2-135">Például úgy is dönt, hogy válassza ki a telepítendő néhány felhasználó tesztelési kis OU, vagy előfordulhat, hogy válassza ki a legfelső szintű szervezeti egység központi telepítése a teljes szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="d93c2-135">For instance, you may decide to pick a small OU to deploy to a few users for testing, or you may pick a top-level OU to deploy to your entire organization.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d93c2-136">Ha azt szeretné, hozzon létre vagy szerkessze a szervezeti egységekhez (OU-k), váltson vissza a Kiszolgálókezelőhöz, és navigáljon a **eszközök** > **Active Directory – felhasználók és számítógépek**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-136">If you would like to create or edit your Organization Units (OUs), switch back to the Server Manager and go to **Tools** > **Active Directory Users and Computers**.</span></span>
   > 
   > 
4. <span data-ttu-id="d93c2-137">Miután kiválasztotta a szervezeti egység, a jobb gombbal kattintson rá, és válassza ki **egy csoportházirend-objektum létrehozása ebben a tartományban, és hivatkozás létrehozása itt...**</span><span class="sxs-lookup"><span data-stu-id="d93c2-137">Once you have selected an OU, right-click it and select **Create a GPO in this domain, and Link it here...**</span></span>
   
    ![Hozzon létre egy új csoportházirend-objektum](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. <span data-ttu-id="d93c2-139">Az a **új csoportházirend-objektum** parancssort, írja be az új csoportházirend-objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="d93c2-139">In the **New GPO** prompt, type in a name for the new Group Policy Object.</span></span>
   
    ![Az új csoportházirend-objektum neve](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. <span data-ttu-id="d93c2-141">Kattintson a jobb gombbal a csoportházirend-objektumot létrehozta, és válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-141">Right-click the Group Policy Object that you created, and select **Edit**.</span></span>
   
    ![Az új csoportházirend-objektum szerkesztéséhez](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-the-installation-package"></a><span data-ttu-id="d93c2-143">3. lépés: A telepítési csomag hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d93c2-143">Step 3: Assign the Installation Package</span></span>
1. <span data-ttu-id="d93c2-144">Határozza meg, hogy szeretné-e telepíteni a bővítmény alapján **számítógép konfigurációja** vagy **felhasználói konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-144">Determine whether you would like to deploy the extension based on **Computer Configuration** or **User Configuration**.</span></span> <span data-ttu-id="d93c2-145">Használata esetén [számítógép konfigurációja](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), a bővítmény a bejelentkezett, függetlenül a számítógépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="d93c2-145">When using [computer configuration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), the extension is installed on the computer regardless of which users log on to it.</span></span> <span data-ttu-id="d93c2-146">A [felhasználói konfiguráció](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), felhasználók kiterjesztése a telepítve vannak, függetlenül azok a számítógépek jelentkeznek be őket.</span><span class="sxs-lookup"><span data-stu-id="d93c2-146">With [user configuration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), users have the extension installed for them regardless of which computers they log on to.</span></span>
2. <span data-ttu-id="d93c2-147">A bal oldali ablaktáblán, a **Csoportházirendkezelés-szerkesztő** ablakban, keresse fel a következő mappák elérési útjaiban, attól függően, hogy milyen típusú konfigurációs úgy döntött, hogy valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="d93c2-147">In the left pane of the **Group Policy Management Editor** window, go to either of the following folder paths, depending on which type of configuration you chose:</span></span>
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. <span data-ttu-id="d93c2-148">Kattintson a jobb gombbal **Szoftvertelepítés**, majd jelölje be **új** > **csomag...**</span><span class="sxs-lookup"><span data-stu-id="d93c2-148">Right-click **Software installation**, then select **New** > **Package...**</span></span>
   
    ![Hozzon létre egy új telepítési csomag](./media/active-directory-saas-ie-group-policy/new-package.png)
4. <span data-ttu-id="d93c2-150">Nyissa meg a megosztott mappához, amely tartalmazza a telepítőcsomag a [1. lépés: a terjesztési pont létrehozása](#step-1-create-the-distribution-point), válassza ki az .msi fájlt, és kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-150">Go to the shared folder that contains the installer package from [Step 1: Create the Distribution Point](#step-1-create-the-distribution-point), select the .msi file, and click **Open**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="d93c2-151">Ha a megosztás ugyanazon a kiszolgálón található, győződjön meg arról, hogy Ön keresztül érik el az .msi fájl hálózati elérési útját, nem pedig a helyi fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="d93c2-151">If the share is located on this same server, verify that you are accessing the .msi through the network file path, rather than the local file path.</span></span>
   > 
   > 
   
    ![Válassza ki a telepítési csomagot a megosztott mappából.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. <span data-ttu-id="d93c2-153">Az a **szoftver központi telepítése** rákérdezés, jelölje be **hozzárendelt** a központi telepítési módszer.</span><span class="sxs-lookup"><span data-stu-id="d93c2-153">In the **Deploy Software** prompt, select **Assigned** for your deployment method.</span></span> <span data-ttu-id="d93c2-154">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d93c2-154">Then click **OK**.</span></span>
   
    ![Válassza ki a hozzárendelt, majd kattintson az OK gombra.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

<span data-ttu-id="d93c2-156">A bővítmény már telepítve van a kiválasztott szervezeti Egységhez.</span><span class="sxs-lookup"><span data-stu-id="d93c2-156">The extension is now deployed to the OU that you selected.</span></span> [<span data-ttu-id="d93c2-157">További információ a Csoportházirend szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="d93c2-157">Learn more about Group Policy Software Installation.</span></span>](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a><span data-ttu-id="d93c2-158">4. lépés: Automatikus engedélyezése az Internet Explorer bővítmény</span><span class="sxs-lookup"><span data-stu-id="d93c2-158">Step 4: Auto-Enable the Extension for Internet Explorer</span></span>
<span data-ttu-id="d93c2-159">Mellett a telepítő fut, az Internet Explorer minden bővítmény explicit módon engedélyeznie kell használat előtt.</span><span class="sxs-lookup"><span data-stu-id="d93c2-159">In addition to running the installer, every extension for Internet Explorer must be explicitly enabled before it can be used.</span></span> <span data-ttu-id="d93c2-160">A hozzáférési Panel bővítmény csoportházirenddel engedélyezéséhez az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d93c2-160">Follow the steps below to enable the Access Panel Extension using group policy:</span></span>

1. <span data-ttu-id="d93c2-161">Az a **Csoportházirendkezelés-szerkesztő** ablakban, keresse fel vagy attól függően, hogy milyen típusú konfigurációs beállítást választja, a következő elérési [3. lépés: a telepítési csomag hozzárendelése](#step-3-assign-the-installation-package):</span><span class="sxs-lookup"><span data-stu-id="d93c2-161">In the **Group Policy Management Editor** window, go to either of the following paths, depending on which type of configuration you chose in [Step 3: Assign the Installation Package](#step-3-assign-the-installation-package):</span></span>
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. <span data-ttu-id="d93c2-162">Kattintson a jobb gombbal **Bővítménylista**, és válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-162">Right-click **Add-on List**, and select **Edit**.</span></span>
    <span data-ttu-id="d93c2-163">![Bővítménylista szerkesztése.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span><span class="sxs-lookup"><span data-stu-id="d93c2-163">![Edit Add-on List.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span></span>
3. <span data-ttu-id="d93c2-164">Az a **Bővítménylista** ablakban válassza ki **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-164">In the **Add-on List** window, select **Enabled**.</span></span> <span data-ttu-id="d93c2-165">Ekkor a a **beállítások** kattintson **megjelenítése...** .</span><span class="sxs-lookup"><span data-stu-id="d93c2-165">Then, under the **Options** section, click **Show...**.</span></span>
   
    ![Kattintson az Engedélyezés parancsra, majd kattintson a megjelenítése...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. <span data-ttu-id="d93c2-167">Az a **tartalom megjelenítése** ablak, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d93c2-167">In the **Show Contents** window, perform the following steps:</span></span>
   
   1. <span data-ttu-id="d93c2-168">Az első oszlop (a **Azonosítónév** mező), másolja és illessze be a következő osztály azonosítója:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span><span class="sxs-lookup"><span data-stu-id="d93c2-168">For the first column (the **Value Name** field), copy and paste the following Class ID: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span></span>
   2. <span data-ttu-id="d93c2-169">A második oszlopa (a **érték** mező), írja be a következő értéket:`1`</span><span class="sxs-lookup"><span data-stu-id="d93c2-169">For the second column (the **Value** field), type in the following value: `1`</span></span>
   3. <span data-ttu-id="d93c2-170">Kattintson a **OK** bezárásához a **tartalom megjelenítése** ablak.</span><span class="sxs-lookup"><span data-stu-id="d93c2-170">Click **OK** to close the **Show Contents** window.</span></span>
      
      ![Töltse ki a fenti értékek.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. <span data-ttu-id="d93c2-172">Kattintson a **OK** alkalmazza a módosításokat, és zárja be a **Bővítménylista** ablak.</span><span class="sxs-lookup"><span data-stu-id="d93c2-172">Click **OK** to apply your changes and close the **Add-on List** window.</span></span>

<span data-ttu-id="d93c2-173">A bővítmény most engedélyezni kell a gépek a kijelölt szervezeti egységen.</span><span class="sxs-lookup"><span data-stu-id="d93c2-173">The extension should now be enabled for the machines in the selected OU.</span></span> [<span data-ttu-id="d93c2-174">Engedélyezheti vagy tilthatja le a gyakori csoportházirend használatával kapcsolatos további tudnivalókért.</span><span class="sxs-lookup"><span data-stu-id="d93c2-174">Learn more about using group policy to enable or disable Internet Explorer add-ons.</span></span>](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a><span data-ttu-id="d93c2-175">5. lépés (nem kötelező): Prompt "Jelszó megjegyzése" letiltása</span><span class="sxs-lookup"><span data-stu-id="d93c2-175">Step 5 (Optional): Disable "Remember Password" Prompt</span></span>
<span data-ttu-id="d93c2-176">Ha a felhasználók jelentkezzen be a hozzáférési Panel kiterjesztéssel webhelyek, az Internet Explorer előfordulhat, hogy megjelenítése a következő megkérdezi "Szeretné tárolni a jelszó?"</span><span class="sxs-lookup"><span data-stu-id="d93c2-176">When users sign-in to websites using the Access Panel Extension, Internet Explorer may show the following prompt asking "Would you like to store your password?"</span></span>

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

<span data-ttu-id="d93c2-177">Ha kívánja, hogy a felhasználók ne jelent meg a kérdés, majd automatikusan hajthat végre a jelszavak tárolása megelőzése érdekében az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d93c2-177">If you wish to prevent your users from seeing this prompt, then follow the steps below to prevent auto-complete from remembering passwords:</span></span>

1. <span data-ttu-id="d93c2-178">Az a **Csoportházirendkezelés-szerkesztő** ablakban, keresse fel az alábbi elérési út.</span><span class="sxs-lookup"><span data-stu-id="d93c2-178">In the **Group Policy Management Editor** window, go to the path listed below.</span></span> <span data-ttu-id="d93c2-179">A konfigurációs beállítás csak érhető el a **felhasználói konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-179">This configuration setting is only available under **User Configuration**.</span></span>
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. <span data-ttu-id="d93c2-180">Találja a beállítást, nevű **kapcsolja be az automatikus kitöltés funkciót a felhasználónevek és jelszavak űrlapokon**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-180">Find the setting named **Turn on the auto-complete feature for user names and passwords on forms**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d93c2-181">Az Active Directory korábbi verzióiban ezt a beállítást, a nevű sorolhatja **nem teszik lehetővé az automatikus kitöltés jelszavak mentése**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-181">Previous versions of Active Directory may list this setting with the name **Do not allow auto-complete to save passwords**.</span></span> <span data-ttu-id="d93c2-182">A konfigurációs beállítás eltér az ebben az oktatóanyagban leírt beállítások.</span><span class="sxs-lookup"><span data-stu-id="d93c2-182">The configuration for that setting differs from the setting described in this tutorial.</span></span>
   > 
   > 
   
    ![Ne felejtse el, keresse meg a felhasználói beállítások alapján.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. <span data-ttu-id="d93c2-184">Kattintson a jobb gombbal a fenti beállítás, és válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-184">Right click the above setting, and select **Edit**.</span></span>
4. <span data-ttu-id="d93c2-185">A ablakban című **kapcsolja be az automatikus kitöltés funkciót a felhasználónevek és jelszavak űrlapokon**, jelölje be **letiltott**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-185">In the window titled **Turn on the auto-complete feature for user names and passwords on forms**, select **Disabled**.</span></span>
   
    ![Válassza ki a letiltása](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. <span data-ttu-id="d93c2-187">Kattintson a **OK** alkalmazza a módosításokat, és az ablak bezárásához.</span><span class="sxs-lookup"><span data-stu-id="d93c2-187">Click **OK** to apply these changes and close the window.</span></span>

<span data-ttu-id="d93c2-188">Felhasználók már nem lesz képes tárolni a hitelesítő adatok vagy az automatikus kitöltés használatát, a korábban tárolt hitelesítő adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d93c2-188">Users will no longer be able to store their credentials or use auto-complete to access previously stored credentials.</span></span> <span data-ttu-id="d93c2-189">Azonban ez a házirend engedélyezése a felhasználók számára továbbra is használhatja az automatikus kitöltés az űrlap mezőkön, például a keresési mezők más típusú.</span><span class="sxs-lookup"><span data-stu-id="d93c2-189">However, this policy does allow users to continue to use auto-complete for other types of form fields, such as search fields.</span></span>

> [!WARNING]
> <span data-ttu-id="d93c2-190">Ha a házirend engedélyezve van, miután a felhasználók úgy döntött, hogy néhány házirend ezzel hitelesítő adatok tárolására *nem* már tárolt hitelesítő adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="d93c2-190">If this policy is enabled after users have chosen to store some credentials, this policy will *not* clear the credentials that have already been stored.</span></span>
> 
> 

## <a name="step-6-testing-the-deployment"></a><span data-ttu-id="d93c2-191">6. lépés: A központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d93c2-191">Step 6: Testing the Deployment</span></span>
<span data-ttu-id="d93c2-192">Ha a bővítmény telepítése sikeres volt-e ellenőrizni az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d93c2-192">Follow the steps below to verify if the extension deployment was successful:</span></span>

1. <span data-ttu-id="d93c2-193">Ha használatával telepített **számítógép konfigurációja**, jelentkezzen be egy ügyfélszámítógépre, amely a kiválasztott szervezeti Egységhez tartozik [2. lépés: a csoportházirend-objektum létrehozása](#step-2-create-the-group-policy-object).</span><span class="sxs-lookup"><span data-stu-id="d93c2-193">If you deployed using **Computer Configuration**, sign into a client machine that belongs to the OU that you selected in [Step 2: Create the Group Policy Object](#step-2-create-the-group-policy-object).</span></span> <span data-ttu-id="d93c2-194">Ha használatával telepített **felhasználói konfiguráció**, ügyeljen arra, hogy jelentkezzen be egy felhasználó számára, hogy a szervezeti Egységhez tartozik.</span><span class="sxs-lookup"><span data-stu-id="d93c2-194">If you deployed using **User Configuration**, make sure to sign in as a user who belongs to that OU.</span></span>
2. <span data-ttu-id="d93c2-195">Modulok a teljes módosul a csoportházirend frissítése néhány bejelentkezési vehet fel a géppel.</span><span class="sxs-lookup"><span data-stu-id="d93c2-195">It may take a couple sign ins for the group policy changes to fully update with this machine.</span></span> <span data-ttu-id="d93c2-196">A frissítés kényszerítéséhez nyisson meg egy **parancssor** ablakot, és futtassa az alábbi parancsot:`gpupdate /force`</span><span class="sxs-lookup"><span data-stu-id="d93c2-196">To force the update, open a **Command Prompt** window and run the following command: `gpupdate /force`</span></span>
3. <span data-ttu-id="d93c2-197">Újra kell indítania a gépet, a telepítés kerül sor.</span><span class="sxs-lookup"><span data-stu-id="d93c2-197">You must restart the machine for the installation to take place.</span></span> <span data-ttu-id="d93c2-198">Rendszerindítási szokásos közben a bővítmény telepítése jelentősen több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="d93c2-198">Bootup may take significantly more time than usual while the extension installs.</span></span>
4. <span data-ttu-id="d93c2-199">Az újraindítás után nyissa meg a **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-199">After restarting, open **Internet Explorer**.</span></span> <span data-ttu-id="d93c2-200">Kattintson az ablak jobb felső sarkában **eszközök** (fogaskerék ikonra), majd válassza ki **bővítmények kezelése**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-200">On the upper-right corner of the window, click **Tools** (the gear icon), and then select **Manage add-ons**.</span></span>
   
    ![Kattintson az eszközök > Bővítmények kezelése](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. <span data-ttu-id="d93c2-202">A a **bővítmények kezelése** ablakban ellenőrizze, hogy a **hozzáférési Panel bővítmény** telepítve van, és hogy a **állapot** értékre lett állítva **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="d93c2-202">In the **Manage Add-ons** window, verify that the **Access Panel Extension** has been installed and that its **Status** has been set to **Enabled**.</span></span>
   
    ![Győződjön meg arról, hogy a hozzáférési Panel bővítmény telepítve és engedélyezve van.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a><span data-ttu-id="d93c2-204">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="d93c2-204">Related Articles</span></span>
* [<span data-ttu-id="d93c2-205">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="d93c2-205">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="d93c2-206">Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d93c2-206">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="d93c2-207">A hozzáférési Panel bővítményét az Internet Explorer hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="d93c2-207">Troubleshooting the Access Panel Extension for Internet Explorer</span></span>](active-directory-saas-ie-troubleshooting.md)

