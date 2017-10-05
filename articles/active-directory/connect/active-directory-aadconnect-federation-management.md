---
title: "Active Directory összevonási szolgáltatások kezelése és testreszabása az Azure AD Connect |} Microsoft Docs"
description: "AD FS felügyelet az Azure AD Connect és felhasználói AD FS bejelentkezési élmény és az Azure AD Connect PowerShell testreszabása."
keywords: "Az AD FS-, ADFS, az AD FS felügyeleti AAD-csatlakozás, csatlakozás, bejelentkezés, az AD FS testreszabás javítani bizalmi kapcsolat, O365, összevonási, függő entitás"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 14f03542a6553c5bb697192828368ffe6b96441c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a><span data-ttu-id="42e45-104">Kezelése és testreszabása az Active Directory összevonási szolgáltatások az Azure AD Connect használatával</span><span class="sxs-lookup"><span data-stu-id="42e45-104">Manage and customize Active Directory Federation Services by using Azure AD Connect</span></span>
<span data-ttu-id="42e45-105">Ez a cikk ismerteti a kezelése és testreszabása az Active Directory összevonási szolgáltatások (AD FS) Azure Active Directory (Azure AD) Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="42e45-105">This article describes how to manage and customize Active Directory Federation Services (AD FS) by using Azure Active Directory (Azure AD) Connect.</span></span> <span data-ttu-id="42e45-106">Egyéb gyakori AD FS feladatok, amelyek hasznosak lehetnek az AD FS-farm teljes konfigurációját is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="42e45-106">It also includes other common AD FS tasks that you might need to do for a complete configuration of an AD FS farm.</span></span>

| <span data-ttu-id="42e45-107">Témakör</span><span class="sxs-lookup"><span data-stu-id="42e45-107">Topic</span></span> | <span data-ttu-id="42e45-108">Ez vonatkozik</span><span class="sxs-lookup"><span data-stu-id="42e45-108">What it covers</span></span> |
|:--- |:--- |
| <span data-ttu-id="42e45-109">**Az AD FS kezelése**</span><span class="sxs-lookup"><span data-stu-id="42e45-109">**Manage AD FS**</span></span> | |
| [<span data-ttu-id="42e45-110">Javítsa ki a bizalmi kapcsolat</span><span class="sxs-lookup"><span data-stu-id="42e45-110">Repair the trust</span></span>](#repairthetrust) |<span data-ttu-id="42e45-111">Javítsa ki az összevonási megbízhatósági kapcsolat, és az Office 365 módjáról.</span><span class="sxs-lookup"><span data-stu-id="42e45-111">How to repair the federation trust with Office 365.</span></span> |
| [<span data-ttu-id="42e45-112">Az Azure AD használatával másodlagos bejelentkezési Azonosítóval összevonni</span><span class="sxs-lookup"><span data-stu-id="42e45-112">Federate with Azure AD using alternate login ID </span></span>](#alternateid) | <span data-ttu-id="42e45-113">Másodlagos bejelentkezési azonosítóval összevonás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42e45-113">Configure federation using alternate login ID</span></span>  |
| [<span data-ttu-id="42e45-114">Az AD FS-kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42e45-114">Add an AD FS server</span></span>](#addadfsserver) |<span data-ttu-id="42e45-115">Hogyan bontsa ki az AD FS-farm további AD FS-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="42e45-115">How to expand an AD FS farm with an additional AD FS server.</span></span> |
| [<span data-ttu-id="42e45-116">AD FS webalkalmazás-Proxy kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42e45-116">Add an AD FS Web Application Proxy server</span></span>](#addwapserver) |<span data-ttu-id="42e45-117">Bontsa ki a további webalkalmazás-proxykiszolgálóként (WAP) kiszolgáló AD FS farmot módjáról.</span><span class="sxs-lookup"><span data-stu-id="42e45-117">How to expand an AD FS farm with an additional Web Application Proxy (WAP) server.</span></span> |
| [<span data-ttu-id="42e45-118">Összevont tartomány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42e45-118">Add a federated domain</span></span>](#addfeddomain) |<span data-ttu-id="42e45-119">Hogyan adhat egy összevont tartományt.</span><span class="sxs-lookup"><span data-stu-id="42e45-119">How to add a federated domain.</span></span> |
| [<span data-ttu-id="42e45-120">Az SSL-tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="42e45-120">Update the SSL certificate</span></span>](active-directory-aadconnectfed-ssl-update.md)| <span data-ttu-id="42e45-121">Hogyan lehet frissíteni az SSL-tanúsítványt az AD FS-farm.</span><span class="sxs-lookup"><span data-stu-id="42e45-121">How to update the SSL certificate for an AD FS farm.</span></span> |
| <span data-ttu-id="42e45-122">**Az AD FS testreszabása**</span><span class="sxs-lookup"><span data-stu-id="42e45-122">**Customize AD FS**</span></span> | |
| [<span data-ttu-id="42e45-123">Adjon hozzá egy egyéni vállalati emblémát vagy ábra</span><span class="sxs-lookup"><span data-stu-id="42e45-123">Add a custom company logo or illustration</span></span>](#customlogo) |<span data-ttu-id="42e45-124">Az AD FS bejelentkezési oldalát a vállalati embléma és illusztráció testreszabásának módját.</span><span class="sxs-lookup"><span data-stu-id="42e45-124">How to customize an AD FS sign-in page with a company logo and illustration.</span></span> |
| [<span data-ttu-id="42e45-125">Adjon meg egy bejelentkezési leírást</span><span class="sxs-lookup"><span data-stu-id="42e45-125">Add a sign-in description</span></span>](#addsignindescription) |<span data-ttu-id="42e45-126">Hogyan bejelentkezésilap-leírást.</span><span class="sxs-lookup"><span data-stu-id="42e45-126">How to add a sign-in page description.</span></span> |
| [<span data-ttu-id="42e45-127">AD FS jogcím szabályok módosítása</span><span class="sxs-lookup"><span data-stu-id="42e45-127">Modify AD FS claim rules</span></span>](#modclaims) |<span data-ttu-id="42e45-128">Hogyan lehet módosítani az AD FS összevonási több, különböző esetekre jogcímek.</span><span class="sxs-lookup"><span data-stu-id="42e45-128">How to modify AD FS claims for various federation scenarios.</span></span> |

## <a name="manage-ad-fs"></a><span data-ttu-id="42e45-129">Az AD FS kezelése</span><span class="sxs-lookup"><span data-stu-id="42e45-129">Manage AD FS</span></span>
<span data-ttu-id="42e45-130">Különböző AD FS-sel kapcsolatos feladat az Azure AD Connect varázsló segítségével elvégezhető az Azure AD Connect minimális felhasználói beavatkozás.</span><span class="sxs-lookup"><span data-stu-id="42e45-130">You can perform various AD FS-related tasks in Azure AD Connect with minimal user intervention by using the Azure AD Connect wizard.</span></span> <span data-ttu-id="42e45-131">Miután befejezte az Azure AD Connect telepítése varázslóval, a varázsló futtatása újra további feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="42e45-131">After you've finished installing Azure AD Connect by running the wizard, you can run the wizard again to perform additional tasks.</span></span>

## <span data-ttu-id="42e45-132">Javítsa ki a bizalmi kapcsolat<a name=repairthetrust></a></span><span class="sxs-lookup"><span data-stu-id="42e45-132">Repair the trust <a name=repairthetrust></a></span></span>
<span data-ttu-id="42e45-133">Az Azure AD Connect használatával ellenőrizze az AD FS és az Azure AD megbízik, és tegye meg a megfelelő lépéseket a megbízhatóság javításához, aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="42e45-133">You can use Azure AD Connect to check the current health of the AD FS and Azure AD trust and take appropriate actions to repair the trust.</span></span> <span data-ttu-id="42e45-134">Kövesse az alábbi lépéseket az Azure AD kijavításához és az AD FS megbízható.</span><span class="sxs-lookup"><span data-stu-id="42e45-134">Follow these steps to repair your Azure AD and AD FS trust.</span></span>

1. <span data-ttu-id="42e45-135">Válassza ki **javítási aad-ben és az AD FS megbízható** további feladatok közül.</span><span class="sxs-lookup"><span data-stu-id="42e45-135">Select **Repair AAD and ADFS Trust** from the list of additional tasks.</span></span>
   <span data-ttu-id="42e45-136">![Javítsa az aad-ben és az AD FS megbízható](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span><span class="sxs-lookup"><span data-stu-id="42e45-136">![Repair AAD and ADFS Trust](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span></span>

2. <span data-ttu-id="42e45-137">Az a **az Azure AD Connect** lapon adja meg az Azure AD globális rendszergazdai hitelesítő adataival, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="42e45-137">On the **Connect to Azure AD** page, provide your global administrator credentials for Azure AD, and click **Next**.</span></span>
   <span data-ttu-id="42e45-138">![Az Azure AD Connect](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span><span class="sxs-lookup"><span data-stu-id="42e45-138">![Connect to Azure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span></span>

3. <span data-ttu-id="42e45-139">Az a **távelérési hitelesítő adatainak** lapon, a tartományi rendszergazda adja meg a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="42e45-139">On the **Remote access credentials** page, enter the credentials for the domain administrator.</span></span>

   ![Távoli hozzáférési hitelesítő adatok](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    <span data-ttu-id="42e45-141">Miután rákattintott **következő**, az Azure AD Connect tanúsítványállapot keres, és az esetleges problémákat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="42e45-141">After you click **Next**, Azure AD Connect checks for certificate health and shows any issues.</span></span>

    ![A tanúsítványok állapota](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    <span data-ttu-id="42e45-143">A **beállíthatja az** lap a megbízhatóság javításához végrehajtott műveletek listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="42e45-143">The **Ready to configure** page shows the list of actions that will be performed to repair the trust.</span></span>

    ![Ready to configure (Konfigurálásra kész)](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. <span data-ttu-id="42e45-145">Kattintson a **telepítése** a megbízhatóság javításához.</span><span class="sxs-lookup"><span data-stu-id="42e45-145">Click **Install** to repair the trust.</span></span>

> [!NOTE]
> <span data-ttu-id="42e45-146">Az Azure AD Connect is csak javítása vagy a törvény önaláírt tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="42e45-146">Azure AD Connect can only repair or act on certificates that are self-signed.</span></span> <span data-ttu-id="42e45-147">Az Azure AD Connect harmadik féltől származó tanúsítványok nem lehet kijavítani.</span><span class="sxs-lookup"><span data-stu-id="42e45-147">Azure AD Connect can't repair third-party certificates.</span></span>

## <span data-ttu-id="42e45-148">Az Azure AD használatával AlternateID összevonni<a name=alternateid></a></span><span class="sxs-lookup"><span data-stu-id="42e45-148">Federate with Azure AD using AlternateID <a name=alternateid></a></span></span>
<span data-ttu-id="42e45-149">Javasoljuk, hogy a helyszíni felhasználói egyszerű Name(UPN) és a felhő egyszerű felhasználónév mindig azonos.</span><span class="sxs-lookup"><span data-stu-id="42e45-149">It is recommended that the on-premises User Principal Name(UPN) and the cloud User Principal Name are kept the same.</span></span> <span data-ttu-id="42e45-150">Ha a helyszíni egyszerű Felhasználónévvel használ egy nem átirányítható tartomány (például)</span><span class="sxs-lookup"><span data-stu-id="42e45-150">If the on-premises UPN uses a non-routable domain (ex.</span></span> <span data-ttu-id="42e45-151">Contoso.local), vagy nem módosítható miatt helyi alkalmazásfüggőségek, azt javasoljuk másodlagos bejelentkezési azonosító beállítása</span><span class="sxs-lookup"><span data-stu-id="42e45-151">Contoso.local) or cannot be changed due to local application dependencies, we recommend setting up alternate login ID.</span></span> <span data-ttu-id="42e45-152">Másodlagos bejelentkezési Azonosítóval is lehetővé teszi a bejelentkezés során tapasztal élmény konfigurálását ahol felhasználók bejelentkezhetnek csak az egyszerű Felhasználónevük, például a mail attribútum.</span><span class="sxs-lookup"><span data-stu-id="42e45-152">Alternate login ID allows you to configure a sign-in experience where users can sign in with an attribute other than their UPN, such as mail.</span></span> <span data-ttu-id="42e45-153">A választás az egyszerű felhasználónév az Azure AD Connect az alapértelmezett érték a userPrincipalName attribútum az Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="42e45-153">The choice for User Principal Name in Azure AD Connect defaults to the userPrincipalName attribute in Active Directory.</span></span> <span data-ttu-id="42e45-154">Ha az attribútum válasszon az egyszerű felhasználónév, és az AD FS segítségével összevonja, majd az Azure AD Connect AD FS beállítják másodlagos bejelentkezési azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="42e45-154">If you choose any other attribute for User Principal Name and are federating using AD FS, then Azure AD Connect will configure AD FS for alternate login ID.</span></span> <span data-ttu-id="42e45-155">Egy másik attribútum kiválasztása az egyszerű felhasználónév példát alább találja:</span><span class="sxs-lookup"><span data-stu-id="42e45-155">An example of choosing a different attribute for User Principal Name is shown below:</span></span>

![Alternatív ID attribútum kiválasztása](media/active-directory-aadconnect-federation-management/attributeselection.png)

<span data-ttu-id="42e45-157">Másodlagos bejelentkezési azonosító konfigurálása az AD FS két fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="42e45-157">Configuring alternate login ID for AD FS consists of two main steps:</span></span>
1. <span data-ttu-id="42e45-158">**Állítsa be az jobb kiállítási jogcímek**: az Azure AD függő entitáshoz kiállítási jogcímszabályok megbízható módosultak, hogy a kijelölt UserPrincipalName attribútum használata a másik azonosító a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="42e45-158">**Configure the right set of issuance claims**: The issuance claim rules in the Azure AD relying party trust are modified to use the selected UserPrincipalName attribute as the alternate ID of the user.</span></span>
2. <span data-ttu-id="42e45-159">**Engedélyezze a másodlagos bejelentkezési Azonosítóval a az AD FS konfigurációt**: az AD FS konfigurációt frissül, hogy az Active Directory összevonási szolgáltatások is kereshet a felhasználók a megfelelő erdőben, a másik azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="42e45-159">**Enable alternate login ID in the AD FS configuration**: The AD FS configuration is updated so that AD FS can look up users in the appropriate forests using the alternate ID.</span></span> <span data-ttu-id="42e45-160">Ez a konfiguráció az AD FS Windows Server 2012 R2 (az KB2919355) vagy újabb rendszeren támogatott.</span><span class="sxs-lookup"><span data-stu-id="42e45-160">This configuration is supported for AD FS on Windows Server 2012 R2 (with KB2919355) or later.</span></span> <span data-ttu-id="42e45-161">Ha az AD FS-kiszolgáló 2012 R2, az Azure AD Connect ellenőrzi, hogy a szükséges KB.</span><span class="sxs-lookup"><span data-stu-id="42e45-161">If the AD FS servers are 2012 R2, Azure AD Connect checks for the presence of the required KB.</span></span> <span data-ttu-id="42e45-162">Ha a rendszer nem ismeri fel a KB, egy figyelmeztetés jelenik meg konfiguráció befejezése után alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="42e45-162">If the KB is not detected, a warning will be displayed after configuration completes, as shown below:</span></span>

    ![A hiányzó KB 2012R2 a figyelmeztetés](media/active-directory-aadconnect-federation-management/kbwarning.png)

    <span data-ttu-id="42e45-164">A konfiguráció esetén a hiányzó KB kijavításához, telepítse a szükséges [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) , és javítsa a bizalmi kapcsolat használatával [javítása aad-ben és az AD FS-megbízhatóság](#repairthetrust).</span><span class="sxs-lookup"><span data-stu-id="42e45-164">To rectify the configuration in case of missing KB, install the required [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) and then repair the trust using [Repair AAD and AD FS Trust](#repairthetrust).</span></span>

> [!NOTE]
> <span data-ttu-id="42e45-165">További információk a alternateID és a kézi konfigurálásának lépései, [másodlagos bejelentkezési azonosító beállítása](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span><span class="sxs-lookup"><span data-stu-id="42e45-165">For more information on alternateID and steps to manually configure, read [Configuring Alternate Login ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span></span>

## <span data-ttu-id="42e45-166">Az AD FS-kiszolgáló hozzáadása<a name=addadfsserver></a></span><span class="sxs-lookup"><span data-stu-id="42e45-166">Add an AD FS server <a name=addadfsserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="42e45-167">Az AD FS-kiszolgáló hozzáadása az Azure AD Connect a PFX-tanúsítványt igényel.</span><span class="sxs-lookup"><span data-stu-id="42e45-167">To add an AD FS server, Azure AD Connect requires the PFX certificate.</span></span> <span data-ttu-id="42e45-168">Ezért a művelet végrehajtása, csak akkor, ha az AD FS-farm konfigurálta az Azure AD Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="42e45-168">Therefore, you can perform this operation only if you configured the AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="42e45-169">Válassza ki **további összevonási kiszolgáló üzembe helyezése**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="42e45-169">Select **Deploy an additional Federation Server**, and click **Next**.</span></span>

   ![További összevonási kiszolgáló](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. <span data-ttu-id="42e45-171">Az a **az Azure AD Connect** lapon adja meg az Azure AD globális rendszergazdai hitelesítő adataival, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="42e45-171">On the **Connect to Azure AD** page, enter your global administrator credentials for Azure AD, and click **Next**.</span></span>

   ![Csatlakozás az Azure AD szolgáltatáshoz](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. <span data-ttu-id="42e45-173">Adja meg a tartomány rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="42e45-173">Provide the domain administrator credentials.</span></span>

   ![Tartományi rendszergazda hitelesítő adatai](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. <span data-ttu-id="42e45-175">Az Azure AD Connect kéri a jelszót az új AD FS-farm konfigurálása az Azure AD Connect közben megadott PFX-fájl.</span><span class="sxs-lookup"><span data-stu-id="42e45-175">Azure AD Connect asks for the password of the PFX file that you provided while configuring your new AD FS farm with Azure AD Connect.</span></span> <span data-ttu-id="42e45-176">Kattintson a **jelszó megadása** kell adnia a jelszót a PFX-fájlból.</span><span class="sxs-lookup"><span data-stu-id="42e45-176">Click **Enter Password** to provide the password for the PFX file.</span></span>

   ![Tanúsítvány jelszava](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Adja meg az SSL-tanúsítvány](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. <span data-ttu-id="42e45-179">Az a **AD FS-kiszolgálók** lapján adja meg a kiszolgáló neve vagy IP-cím, fel kell venni az AD FS-farm.</span><span class="sxs-lookup"><span data-stu-id="42e45-179">On the **AD FS Servers** page, enter the server name or IP address to be added to the AD FS farm.</span></span>

   ![AD FS-kiszolgálók](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. <span data-ttu-id="42e45-181">Kattintson a **következő**, és hajtsa végre a végleges **konfigurálása** lap.</span><span class="sxs-lookup"><span data-stu-id="42e45-181">Click **Next**, and go through the final **Configure** page.</span></span> <span data-ttu-id="42e45-182">Miután az Azure AD Connect befejezte a kiszolgálók hozzáadását az AD FS-farm, nyílik lehetőség a kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="42e45-182">After Azure AD Connect has finished adding the servers to the AD FS farm, you will be given the option to verify the connectivity.</span></span>

   ![Ready to configure (Konfigurálásra kész)](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![A telepítés befejeződött](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## <span data-ttu-id="42e45-185">AD FS WAP-kiszolgáló hozzáadása<a name=addwapserver></a></span><span class="sxs-lookup"><span data-stu-id="42e45-185">Add an AD FS WAP server <a name=addwapserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="42e45-186">A WAP-kiszolgáló, az Azure AD Connect megköveteli a PFX-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="42e45-186">To add a WAP server, Azure AD Connect requires the PFX certificate.</span></span> <span data-ttu-id="42e45-187">Ezért csak hajthatja végre ezt a műveletet, ha az AD FS-farm konfigurálta az Azure AD Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="42e45-187">Therefore, you can only perform this operation if you configured the AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="42e45-188">Válassza ki **webalkalmazás-Proxy telepítése** rendelkezésre álló feladatok közül.</span><span class="sxs-lookup"><span data-stu-id="42e45-188">Select **Deploy Web Application Proxy** from the list of available tasks.</span></span>

   ![Webalkalmazás-Proxy telepítéséhez](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. <span data-ttu-id="42e45-190">Adja meg az Azure globális rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="42e45-190">Provide the Azure global administrator credentials.</span></span>

   ![Csatlakozás az Azure AD szolgáltatáshoz](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. <span data-ttu-id="42e45-192">Az a **adja meg az SSL-tanúsítvány** lapján adja meg a jelszót az AD FS-farm konfigurálása az Azure AD Connect során megadott PFX-fájl.</span><span class="sxs-lookup"><span data-stu-id="42e45-192">On the **Specify SSL certificate** page, provide the password for the PFX file that you provided when you configured the AD FS farm with Azure AD Connect.</span></span>
   <span data-ttu-id="42e45-193">![Tanúsítvány jelszava](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span><span class="sxs-lookup"><span data-stu-id="42e45-193">![Certificate password](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span></span>

    ![Adja meg az SSL-tanúsítvány](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. <span data-ttu-id="42e45-195">Adja hozzá a kiszolgálót, lehet hozzáadni a WAP-kiszolgálóként.</span><span class="sxs-lookup"><span data-stu-id="42e45-195">Add the server to be added as a WAP server.</span></span> <span data-ttu-id="42e45-196">A WAP-kiszolgáló nem lehet, hogy a tartományhoz csatlakoztatni, mert a varázsló megkérdezi, a hozzáadni kívánt kiszolgáló rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="42e45-196">Because the WAP server might not be joined to the domain, the wizard asks for administrative credentials to the server being added.</span></span>

   ![A felügyeleti kiszolgáló hitelesítő adatai](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. <span data-ttu-id="42e45-198">Az a **Proxy megbízhatósági hitelesítő adatok** lapján adja meg a rendszergazdai hitelesítő adatokat konfigurálja a proxyt megbízik, és hozzáférés az AD FS-farm elsődleges kiszolgálóját.</span><span class="sxs-lookup"><span data-stu-id="42e45-198">On the **Proxy trust credentials** page, provide administrative credentials to configure the proxy trust and access the primary server in the AD FS farm.</span></span>

   ![Proxy megbízhatósági hitelesítő adatai](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. <span data-ttu-id="42e45-200">Az a **beállíthatja az** lap, a varázsló végrehajtott műveletek listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="42e45-200">On the **Ready to configure** page, the wizard shows the list of actions that will be performed.</span></span>

   ![Ready to configure (Konfigurálásra kész)](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. <span data-ttu-id="42e45-202">Kattintson a **telepítése** a konfiguráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="42e45-202">Click **Install** to finish the configuration.</span></span> <span data-ttu-id="42e45-203">A konfiguráció befejezése után a varázsló lehetővé teszi a a kiszolgálók ellenőrzésére is.</span><span class="sxs-lookup"><span data-stu-id="42e45-203">After the configuration is complete, the wizard gives you the option to verify the connectivity to the servers.</span></span> <span data-ttu-id="42e45-204">Kattintson a **ellenőrizze** kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="42e45-204">Click **Verify** to check connectivity.</span></span>

   ![A telepítés befejeződött](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## <span data-ttu-id="42e45-206">Összevont tartomány hozzáadása<a name=addfeddomain></a></span><span class="sxs-lookup"><span data-stu-id="42e45-206">Add a federated domain <a name=addfeddomain></a></span></span>

<span data-ttu-id="42e45-207">Nem lehet összevont Azure AD-val az Azure AD Connect használatával tartomány hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="42e45-207">It's easy to add a domain to be federated with Azure AD by using Azure AD Connect.</span></span> <span data-ttu-id="42e45-208">Az Azure AD Connect a tartományt összevonásra hozzáadja, és módosítja a jogcímszabályok a kibocsátó megfelelően megfelelően, és az Azure AD összevont több tartomány esetén.</span><span class="sxs-lookup"><span data-stu-id="42e45-208">Azure AD Connect adds the domain for federation and modifies the claim rules to correctly reflect the issuer when you have multiple domains federated with Azure AD.</span></span>

1. <span data-ttu-id="42e45-209">Egy összevont tartományt hozzáadásához válassza ki a feladatot **adnia egy további Azure AD-tartomány**.</span><span class="sxs-lookup"><span data-stu-id="42e45-209">To add a federated domain, select the task **Add an additional Azure AD domain**.</span></span>

   ![További Azure AD-tartomány](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. <span data-ttu-id="42e45-211">A varázsló következő oldalán az Azure AD globális rendszergazdai hitelesítő adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="42e45-211">On the next page of the wizard, provide the global administrator credentials for Azure AD.</span></span>

   ![Csatlakozás az Azure AD szolgáltatáshoz](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. <span data-ttu-id="42e45-213">Az a **távelérési hitelesítő adatainak** lapján adja meg a tartományi rendszergazda hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="42e45-213">On the **Remote access credentials** page, provide the domain administrator credentials.</span></span>

   ![Távoli hozzáférési hitelesítő adatok](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. <span data-ttu-id="42e45-215">A következő oldalon a varázsló a lehet összevonást végezni a helyszíni címtár az Azure AD-tartományok listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="42e45-215">On the next page, the wizard provides a list of Azure AD domains that you can federate your on-premises directory with.</span></span> <span data-ttu-id="42e45-216">Válassza ki a tartományt a listából.</span><span class="sxs-lookup"><span data-stu-id="42e45-216">Choose the domain from the list.</span></span>

   ![Az Azure AD-tartomány](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    <span data-ttu-id="42e45-218">Után úgy dönt, hogy a tartomány, a varázsló nyújt további a varázsló által elvégzendő műveleteket és a hatása a konfiguráció megfelelő információt.</span><span class="sxs-lookup"><span data-stu-id="42e45-218">After you choose the domain, the wizard provides you with appropriate information about further actions that the wizard will take and the impact of the configuration.</span></span> <span data-ttu-id="42e45-219">Bizonyos esetekben egy tartományhoz, amely még nincs ellenőrizve az Azure ad-ben, ha a varázsló nyújt segítséget, ellenőrizze a tartományt.</span><span class="sxs-lookup"><span data-stu-id="42e45-219">In some cases, if you select a domain that isn't yet verified in Azure AD, the wizard provides you with information to help you verify the domain.</span></span> <span data-ttu-id="42e45-220">Lásd: [az egyéni tartománynév hozzáadása az Azure Active Directory](../active-directory-add-domain.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="42e45-220">See [Add your custom domain name to Azure Active Directory](../active-directory-add-domain.md) for more details.</span></span>

5. <span data-ttu-id="42e45-221">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="42e45-221">Click **Next**.</span></span> <span data-ttu-id="42e45-222">A **beállíthatja az** lap, amely az Azure AD Connect hajtja végre műveletek listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="42e45-222">The **Ready to configure** page shows the list of actions that Azure AD Connect will perform.</span></span> <span data-ttu-id="42e45-223">Kattintson a **telepítése** a konfiguráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="42e45-223">Click **Install** to finish the configuration.</span></span>

   ![Ready to configure (Konfigurálásra kész)](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> <span data-ttu-id="42e45-225">A hozzáadott összevont tartomány felhasználóinak szinkronizálása előtt fogja tudni belépni az Azure AD kell.</span><span class="sxs-lookup"><span data-stu-id="42e45-225">Users from the added federated domain must be synchronized before they will be able to login to Azure AD.</span></span>

## <a name="ad-fs-customization"></a><span data-ttu-id="42e45-226">AD FS Testreszabás</span><span class="sxs-lookup"><span data-stu-id="42e45-226">AD FS customization</span></span>
<span data-ttu-id="42e45-227">A következő szakaszok részletesen bemutatják a gyakori feladatokat, amelyek akkor lehet végrehajtani, ha a az AD FS bejelentkezési oldal testreszabása.</span><span class="sxs-lookup"><span data-stu-id="42e45-227">The following sections provide details about some of the common tasks that you might have to perform when you customize your AD FS sign-in page.</span></span>

## <span data-ttu-id="42e45-228">Adjon hozzá egy egyéni vállalati emblémát vagy ábra<a name=customlogo></a></span><span class="sxs-lookup"><span data-stu-id="42e45-228">Add a custom company logo or illustration <a name=customlogo></a></span></span>
<span data-ttu-id="42e45-229">A megjelenő vállalatembléma módosításához a **bejelentkezési** lapon, használja a következő Windows PowerShell-parancsmagot és szintaxist.</span><span class="sxs-lookup"><span data-stu-id="42e45-229">To change the logo of the company that's displayed on the **Sign-in** page, use the following Windows PowerShell cmdlet and syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="42e45-230">Az embléma ajánlott dimenziói 260 x 35 pixel, legfeljebb 10 KB-os fájlméretben 96 dpi.</span><span class="sxs-lookup"><span data-stu-id="42e45-230">The recommended dimensions for the logo are 260 x 35 @ 96 dpi with a file size no greater than 10 KB.</span></span>

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> <span data-ttu-id="42e45-231">A *TargetName* paraméter megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="42e45-231">The *TargetName* parameter is required.</span></span> <span data-ttu-id="42e45-232">Az alapértelmezett témájának az AD FS alapértelmezett neve.</span><span class="sxs-lookup"><span data-stu-id="42e45-232">The default theme that's released with AD FS is named Default.</span></span>

## <span data-ttu-id="42e45-233">Adjon meg egy bejelentkezési leírást<a name=addsignindescription></a></span><span class="sxs-lookup"><span data-stu-id="42e45-233">Add a sign-in description <a name=addsignindescription></a></span></span>
<span data-ttu-id="42e45-234">A bejelentkezésilap-leírást a **bejelentkezési oldal**, használja a következő Windows PowerShell-parancsmagot és szintaxist.</span><span class="sxs-lookup"><span data-stu-id="42e45-234">To add a sign-in page description to the **Sign-in page**, use the following Windows PowerShell cmdlet and syntax.</span></span>

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <span data-ttu-id="42e45-235">AD FS jogcím szabályok módosítása<a name=modclaims></a></span><span class="sxs-lookup"><span data-stu-id="42e45-235">Modify AD FS claim rules <a name=modclaims></a></span></span>
<span data-ttu-id="42e45-236">Az AD FS támogatja a gazdag jogcím nyelv, amely segítségével egyéni jogcímszabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="42e45-236">AD FS supports a rich claim language that you can use to create custom claim rules.</span></span> <span data-ttu-id="42e45-237">További információkért lásd: [a Jogcímszabály nyelvének szerepe](https://technet.microsoft.com/library/dd807118.aspx).</span><span class="sxs-lookup"><span data-stu-id="42e45-237">For more information, see [The Role of the Claim Rule Language](https://technet.microsoft.com/library/dd807118.aspx).</span></span>

<span data-ttu-id="42e45-238">A következő szakaszok ismertetik, hogyan írhat egyéni szabályok bizonyos forgatókönyvek esetén is az Azure AD és az AD FS összevonási.</span><span class="sxs-lookup"><span data-stu-id="42e45-238">The following sections describe how you can write custom rules for some scenarios that relate to Azure AD and AD FS federation.</span></span>

### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a><span data-ttu-id="42e45-239">Nem módosítható egy értéket, hogy az attribútum a jelen feltételes azonosítója</span><span class="sxs-lookup"><span data-stu-id="42e45-239">Immutable ID conditional on a value being present in the attribute</span></span>
<span data-ttu-id="42e45-240">Az Azure AD Connect lehetővé teszi egy adatforrás kapcsolatainak használt objektumok szinkronizált az Azure AD-attribútum megadását.</span><span class="sxs-lookup"><span data-stu-id="42e45-240">Azure AD Connect lets you specify an attribute to be used as a source anchor when objects are synced to Azure AD.</span></span> <span data-ttu-id="42e45-241">Ha az egyéni attribútum értéke nem üres, érdemes egy nem módosítható Azonosítót jogcímet.</span><span class="sxs-lookup"><span data-stu-id="42e45-241">If the value in the custom attribute is not empty, you might want to issue an immutable ID claim.</span></span>

<span data-ttu-id="42e45-242">Például kiválaszthatja **ms-ds-consistencyguid** forráshorgony és probléma attribútumaként **ImmutableID** , **ms-ds-consistencyguid** abban az esetben, ha az attribútum értéke rajta.</span><span class="sxs-lookup"><span data-stu-id="42e45-242">For example, you might select **ms-ds-consistencyguid** as the attribute for the source anchor and issue **ImmutableID** as **ms-ds-consistencyguid** in case the attribute has a value against it.</span></span> <span data-ttu-id="42e45-243">Ha nincs érték az attribútum ellen, **objectGuid** , az nem módosítható azonosítót.</span><span class="sxs-lookup"><span data-stu-id="42e45-243">If there's no value against the attribute, issue **objectGuid** as the immutable ID.</span></span> <span data-ttu-id="42e45-244">Egyéni jogcímszabályok a következő szakaszban leírt módon készletét, hogyan hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="42e45-244">You can construct the set of custom claim rules as described in the following section.</span></span>

<span data-ttu-id="42e45-245">**1. szabály: Lekérdezés attribútumok**</span><span class="sxs-lookup"><span data-stu-id="42e45-245">**Rule 1: Query attributes**</span></span>

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

<span data-ttu-id="42e45-246">Ebben a szabályban értékének lekérdezésére **ms-ds-consistencyguid** és **objectGuid** a felhasználó az Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="42e45-246">In this rule, you're querying the values of **ms-ds-consistencyguid** and **objectGuid** for the user from Active Directory.</span></span> <span data-ttu-id="42e45-247">Módosítsa a tároló neve az AD FS üzembe helyezése a megfelelő névre.</span><span class="sxs-lookup"><span data-stu-id="42e45-247">Change the store name to an appropriate store name in your AD FS deployment.</span></span> <span data-ttu-id="42e45-248">Is módosítása a jogcímek írja be a megfelelő jogcím-e az összevonási típus meghatározottak szerint **objectGuid** és **ms-ds-consistencyguid**.</span><span class="sxs-lookup"><span data-stu-id="42e45-248">Also change the claims type to a proper claims type for your federation, as defined for **objectGuid** and **ms-ds-consistencyguid**.</span></span>

<span data-ttu-id="42e45-249">Továbbá használatával **hozzáadása** és nem **probléma**, ne telepítsen az entitás kimenő problémát, és az értékeket is használhat köztes értékként.</span><span class="sxs-lookup"><span data-stu-id="42e45-249">Also, by using **add** and not **issue**, you avoid adding an outgoing issue for the entity, and can use the values as intermediate values.</span></span> <span data-ttu-id="42e45-250">A jogcím egy újabb szabályt állít ki, melyik értéket nem módosítható azonosítóként létrehozása után</span><span class="sxs-lookup"><span data-stu-id="42e45-250">You will issue the claim in a later rule after you establish which value to use as the immutable ID.</span></span>

<span data-ttu-id="42e45-251">**2. szabály: Ellenőrizze, hogy létezik-e az ms-ds-consistencyguid a felhasználó**</span><span class="sxs-lookup"><span data-stu-id="42e45-251">**Rule 2: Check if ms-ds-consistencyguid exists for the user**</span></span>

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

<span data-ttu-id="42e45-252">Ez a szabály meghatározása nevű ideiglenes jelzőt **idflag** értékre van állítva, amely **useguid** esetén nem **ms-ds-consistencyguid** feltöltve a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="42e45-252">This rule defines a temporary flag called **idflag** that is set to **useguid** if there's no **ms-ds-consistencyguid** populated for the user.</span></span> <span data-ttu-id="42e45-253">Ez mögötti logika a tényt, hogy az AD FS nem engedélyezi üres jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="42e45-253">The logic behind this is the fact that AD FS doesn't allow empty claims.</span></span> <span data-ttu-id="42e45-254">Ezért amikor hozzáadja jogcímek http://contoso.com/ws/2016/02/identity/claims/objectguid és http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid Rule 1, hogy végül egy **msdsconsistencyguid** csak jogcím, ha a rendszer a felhasználó tölti fel az értéket.</span><span class="sxs-lookup"><span data-stu-id="42e45-254">So when you add claims http://contoso.com/ws/2016/02/identity/claims/objectguid and http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid in Rule 1, you end up with an **msdsconsistencyguid** claim only if the value is populated for the user.</span></span> <span data-ttu-id="42e45-255">Ha azt nem üres, az AD FS látja, hogy egy üres érték fog szerepelni, elutasítja azokat azonnal.</span><span class="sxs-lookup"><span data-stu-id="42e45-255">If it isn't populated, AD FS sees that it will have an empty value and drops it immediately.</span></span> <span data-ttu-id="42e45-256">Minden objektumot kell **objectGuid**, így az igény mindig van 1. szabály végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="42e45-256">All objects will have **objectGuid**, so that claim will always be there after Rule 1 is executed.</span></span>

<span data-ttu-id="42e45-257">**3. szabály: Ki az ms-ds-consistencyguid ID nem módosítható, ha telepítve**</span><span class="sxs-lookup"><span data-stu-id="42e45-257">**Rule 3: Issue ms-ds-consistencyguid as immutable ID if it's present**</span></span>

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

<span data-ttu-id="42e45-258">Ez az egy implicit **Exist** ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="42e45-258">This is an implicit **Exist** check.</span></span> <span data-ttu-id="42e45-259">Ha a jogcím értéke, hogyan adhat ki, amely nem módosítható azonosítóként</span><span class="sxs-lookup"><span data-stu-id="42e45-259">If the value for the claim exists, then issue that as the immutable ID.</span></span> <span data-ttu-id="42e45-260">Az előző példában a **nameidentifier** jogcímek.</span><span class="sxs-lookup"><span data-stu-id="42e45-260">The previous example uses the **nameidentifier** claim.</span></span> <span data-ttu-id="42e45-261">Állítson be a megfelelő jogcím típusa a környezetben nem módosítható a(z) kell.</span><span class="sxs-lookup"><span data-stu-id="42e45-261">You'll have to change this to the appropriate claim type for the immutable ID in your environment.</span></span>

<span data-ttu-id="42e45-262">**4. szabály: Ki objectGuid ID nem módosítható, ha az ms-ds-consistencyGuid nincs jelen**</span><span class="sxs-lookup"><span data-stu-id="42e45-262">**Rule 4: Issue objectGuid as immutable ID if ms-ds-consistencyGuid is not present**</span></span>

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

<span data-ttu-id="42e45-263">Ebben a szabályban, akkor még egyszerűen ellenőrzése az ideiglenes jelző **idflag**.</span><span class="sxs-lookup"><span data-stu-id="42e45-263">In this rule, you're simply checking the temporary flag **idflag**.</span></span> <span data-ttu-id="42e45-264">Eldöntheti, hogy a jogcím értéke alapján kiadása.</span><span class="sxs-lookup"><span data-stu-id="42e45-264">You decide whether to issue the claim based on its value.</span></span>

> [!NOTE]
> <span data-ttu-id="42e45-265">Ezek a szabályok sorrendjének fontos.</span><span class="sxs-lookup"><span data-stu-id="42e45-265">The sequence of these rules is important.</span></span>

### <a name="sso-with-a-subdomain-upn"></a><span data-ttu-id="42e45-266">Egyszerű felhasználónév altartomány egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="42e45-266">SSO with a subdomain UPN</span></span>
<span data-ttu-id="42e45-267">Összevont Azure AD Connect használatával, a több tartomány is hozzáadhat [hozzá egy új összevont tartományt](active-directory-aadconnect-federation-management.md#addfeddomain).</span><span class="sxs-lookup"><span data-stu-id="42e45-267">You can add more than one domain to be federated by using Azure AD Connect, as described in [Add a new federated domain](active-directory-aadconnect-federation-management.md#addfeddomain).</span></span> <span data-ttu-id="42e45-268">Módosítania kell a felhasználó egyszerű felhasználónév (UPN) jogcím, hogy a kibocsátó azonosító felel meg a legfelső szintű tartomány és nem a altartomány, mert az összevont gyökértartomány is magában foglalja a gyermek.</span><span class="sxs-lookup"><span data-stu-id="42e45-268">You must modify the user principal name (UPN) claim so that the issuer ID corresponds to the root domain and not the subdomain, because the federated root domain also covers the child.</span></span>

<span data-ttu-id="42e45-269">Alapértelmezés szerint a jogcímszabály kibocsátó azonosító értéke:</span><span class="sxs-lookup"><span data-stu-id="42e45-269">By default, the claim rule for issuer ID is set as:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Alapértelmezett kibocsátó azonosító jogcím](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

<span data-ttu-id="42e45-271">Az alapértelmezett szabály egyszerűen vesz igénybe az UPN-utótagot, és használja a kibocsátó azonosító jogcímek.</span><span class="sxs-lookup"><span data-stu-id="42e45-271">The default rule simply takes the UPN suffix and uses it in the issuer ID claim.</span></span> <span data-ttu-id="42e45-272">Például John sub.contoso.com felhasználóként, és contoso.com össze van vonva az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="42e45-272">For example, John is a user in sub.contoso.com, and contoso.com is federated with Azure AD.</span></span> <span data-ttu-id="42e45-273">John megadja john@sub.contoso.com során az Azure AD bejelentkezés felhasználóneveként.</span><span class="sxs-lookup"><span data-stu-id="42e45-273">John enters john@sub.contoso.com as the username while signing in to Azure AD.</span></span> <span data-ttu-id="42e45-274">Az alapértelmezett kibocsátó azonosító jogcímszabály, ha az AD FS kezelő a következő módon:</span><span class="sxs-lookup"><span data-stu-id="42e45-274">The default issuer ID claim rule in AD FS handles it in the following manner:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

<span data-ttu-id="42e45-275">**A jogcím értéke:** http://sub.contoso.com/adfs/services/trust/</span><span class="sxs-lookup"><span data-stu-id="42e45-275">**Claim value:**  http://sub.contoso.com/adfs/services/trust/</span></span>

<span data-ttu-id="42e45-276">A kibocsátó jogcím értéke csak a legfelső szintű tartomány van, módosítsa megfelelően a következő jogcímszabály:</span><span class="sxs-lookup"><span data-stu-id="42e45-276">To have only the root domain in the issuer claim value, change the claim rule to match the following:</span></span>

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a><span data-ttu-id="42e45-277">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42e45-277">Next steps</span></span>
<span data-ttu-id="42e45-278">További információ [felhasználói bejelentkezés lehetőségei](active-directory-aadconnect-user-signin.md).</span><span class="sxs-lookup"><span data-stu-id="42e45-278">Learn more about [user sign-in options](active-directory-aadconnect-user-signin.md).</span></span>
