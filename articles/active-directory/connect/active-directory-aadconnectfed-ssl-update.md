---
title: "Az Azure AD Connect: Frissítés hello SSL-tanúsítványa egy Active Directory összevonási szolgáltatások (AD FS) farm |} Microsoft Docs"
description: "Ez a dokumentum részletek hello lépéseket tooupdate hello SSL-tanúsítvány egy AD FS-farm, az Azure AD Connect használatával."
services: active-directory
keywords: "az Azure ad connect, az AD FS ssl-frissítés, az AD FS-tanúsítványt frissítés, módosítsa az AD FS-tanúsítványt, új AD FS-tanúsítványt, az AD FS tanúsítványt, a frissítés AD FS ssl-tanúsítványt, a frissítés ssl tanúsítvány adfs konfigurálása az AD FS ssl-tanúsítvány, az AD FS, ssl, tanúsítvány, az AD FS szolgáltatás közötti kommunikációs tanúsítványt, a frissítés összevonási, összevonás konfigurálása, aad-csatlakozás"
authors: anandyadavmsft
manager: femila
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: anandy
ms.openlocfilehash: bce7f75aab83b6abacb8472a6895054d137e10e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-hello-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a><span data-ttu-id="a2e7d-104">Az Active Directory összevonási szolgáltatások (AD FS) farm hello SSL-tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a2e7d-104">Update hello SSL certificate for an Active Directory Federation Services (AD FS) farm</span></span>

## <a name="overview"></a><span data-ttu-id="a2e7d-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a2e7d-105">Overview</span></span>
<span data-ttu-id="a2e7d-106">Ez a cikk ismerteti, hogyan használhatja az Azure AD Connect tooupdate hello SSL-tanúsítványt az Active Directory összevonási szolgáltatások (AD FS) farmhoz.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-106">This article describes how you can use Azure AD Connect tooupdate hello SSL certificate for an Active Directory Federation Services (AD FS) farm.</span></span> <span data-ttu-id="a2e7d-107">Akkor is, ha hello felhasználói bejelentkezési módszer kijelölt nem AD FS, hello Azure AD Connect eszköz tooeasily frissítés hello SSL-tanúsítvány használható hello AD FS-farm.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-107">You can use hello Azure AD Connect tool tooeasily update hello SSL certificate for hello AD FS farm even if hello user sign-in method selected is not AD FS.</span></span>

<span data-ttu-id="a2e7d-108">Az SSL-tanúsítványa hello AD FS-farm összes összevonási és három egyszerű lépését a webalkalmazás-proxykiszolgálóként (WAP) kiszolgálók frissítése hello a teljes műveletet végezheti el:</span><span class="sxs-lookup"><span data-stu-id="a2e7d-108">You can perform hello whole operation of updating SSL certificate for hello AD FS farm across all federation and Web Application Proxy (WAP) servers in three simple steps:</span></span>

![Három lépést](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
><span data-ttu-id="a2e7d-110">További információ az AD FS által használt tanúsítványok toolearn lásd [AD FS által használt tanúsítványok ismertetése](https://technet.microsoft.com/library/cc730660.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2e7d-110">toolearn more about certificates that are used by AD FS, see [Understanding certificates used by AD FS](https://technet.microsoft.com/library/cc730660.aspx).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a2e7d-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a2e7d-111">Prerequisites</span></span>

* <span data-ttu-id="a2e7d-112">**AD FS-Farm**: Győződjön meg arról, hogy az AD FS farm Windows Server 2012 R2-alapú vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-112">**AD FS Farm**: Make sure that your AD FS farm is Windows Server 2012 R2-based or later.</span></span>
* <span data-ttu-id="a2e7d-113">**Az Azure AD Connect**: Győződjön meg arról, hogy hello Azure AD Connect verziója 1.1.443.0 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-113">**Azure AD Connect**: Ensure that hello version of Azure AD Connect is 1.1.443.0 or later.</span></span> <span data-ttu-id="a2e7d-114">Hello feladat fogjuk **frissítés az AD FS SSL-tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-114">You'll use hello task **Update AD FS SSL certificate**.</span></span>

![SSL-tevékenység frissítése](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a><span data-ttu-id="a2e7d-116">1. lépés: Az AD FS farm adatok megadása</span><span class="sxs-lookup"><span data-stu-id="a2e7d-116">Step 1: Provide AD FS farm information</span></span>

<span data-ttu-id="a2e7d-117">Az Azure AD Connect megkísérli automatikusan hello AD FS-farm tooobtain információt:</span><span class="sxs-lookup"><span data-stu-id="a2e7d-117">Azure AD Connect attempts tooobtain information about hello AD FS farm automatically by:</span></span>
1. <span data-ttu-id="a2e7d-118">Active Directory összevonási szolgáltatások (Windows Server 2016 vagy újabb) hello farm adatainak lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-118">Querying hello farm information from AD FS (Windows Server 2016 or later).</span></span>
2. <span data-ttu-id="a2e7d-119">Hivatkozási hello adatait az Azure AD Connect helyileg tárolt korábbi futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-119">Referencing hello information from previous runs, which are stored locally with Azure AD Connect.</span></span>

<span data-ttu-id="a2e7d-120">Módosíthatja a kiszolgálók hozzáadásával vagy eltávolításával hello kiszolgálók tooreflect hello aktuális konfigurációja hello AD FS-farm hello listája.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-120">You can modify hello list of servers that are displayed by adding or removing hello servers tooreflect hello current configuration of hello AD FS farm.</span></span> <span data-ttu-id="a2e7d-121">Amint hello server információt, az Azure AD Connect hello kapcsolat és az SSL-tanúsítvány aktuális állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-121">As soon as hello server information is provided, Azure AD Connect displays hello connectivity and current SSL certificate status.</span></span>

![AD FS-kiszolgáló adatai](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

<span data-ttu-id="a2e7d-123">Ha hello lista tartalmaz egy kiszolgálót, amely már nem hello AD FS-farm része, kattintson a **eltávolítása** toodelete hello kiszolgálót a kiszolgálók az AD FS farmon hello listája.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-123">If hello list contains a server that's no longer part of hello AD FS farm, click **Remove** toodelete hello server from hello list of servers in your AD FS farm.</span></span>

![Kapcsolat nélküli server listában](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> <span data-ttu-id="a2e7d-125">A kiszolgáló eltávolításával hello listából az AD FS-kiszolgálók az Azure AD Connectben farm helyi művelet, és frissítések hello hello helyileg tárolja az Azure AD Connect AD FS-farm adatait.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-125">Removing a server from hello list of servers for an AD FS farm in Azure AD Connect is a local operation and updates hello information for hello AD FS farm that Azure AD Connect maintains locally.</span></span> <span data-ttu-id="a2e7d-126">Az Azure AD Connect nem módosítja az AD FS tooreflect hello változás hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-126">Azure AD Connect doesn't modify hello configuration on AD FS tooreflect hello change.</span></span>    

## <a name="step-2-provide-a-new-ssl-certificate"></a><span data-ttu-id="a2e7d-127">2. lépés:, Adjon meg egy új SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="a2e7d-127">Step 2: Provide a new SSL certificate</span></span>

<span data-ttu-id="a2e7d-128">Az AD FS farm kiszolgálói hello információ megerősítését követően az Azure AD Connect hello új SSL-tanúsítványt kér.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-128">After you've confirmed hello information about AD FS farm servers, Azure AD Connect asks for hello new SSL certificate.</span></span> <span data-ttu-id="a2e7d-129">Tanúsítvány jelszóval védett PFX toocontinue hello telepítést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-129">Provide a password-protected PFX certificate toocontinue hello installation.</span></span>

![SSL-tanúsítvány](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

<span data-ttu-id="a2e7d-131">Miután megadta a hello tanúsítványt, az Azure AD Connect végighalad az Előfeltételek sorozata.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-131">After you provide hello certificate, Azure AD Connect goes through a series of prerequisites.</span></span> <span data-ttu-id="a2e7d-132">Győződjön meg arról, hogy a tanúsítvány hello hello tanúsítvány tooensure megfelelő hello AD FS-farm:</span><span class="sxs-lookup"><span data-stu-id="a2e7d-132">Verify hello certificate tooensure that hello certificate is correct for hello AD FS farm:</span></span>

-   <span data-ttu-id="a2e7d-133">hello tulajdonos neve vagy másodlagos tulajdonosneve hello tanúsítvány vagy hello ugyanaz, mint hello összevonási szolgáltatás neve, vagy helyettesítő tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-133">hello subject name/alternate subject name for hello certificate is either hello same as hello federation service name, or it's a wildcard certificate.</span></span>
-   <span data-ttu-id="a2e7d-134">több mint 30 napig hello tanúsítvány érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-134">hello certificate is valid for more than 30 days.</span></span>
-   <span data-ttu-id="a2e7d-135">Érvénytelen a hello tanúsítvány megbízhatósági láncában.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-135">hello certificate trust chain is valid.</span></span>
-   <span data-ttu-id="a2e7d-136">hello tanúsítvány jelszóval védve.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-136">hello certificate is password protected.</span></span>

## <a name="step-3-select-servers-for-hello-update"></a><span data-ttu-id="a2e7d-137">3. lépés: Válassza a kiszolgálók hello frissítés</span><span class="sxs-lookup"><span data-stu-id="a2e7d-137">Step 3: Select servers for hello update</span></span>

<span data-ttu-id="a2e7d-138">Hello a következő lépésben válassza ki a hello kiszolgálók toohave SSL-tanúsítvány hello frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-138">In hello next step, select hello servers that need toohave hello SSL certificate updated.</span></span> <span data-ttu-id="a2e7d-139">Az offline kiszolgálók hello frissítés nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-139">Servers that are offline can't be selected for hello update.</span></span>

![Válassza ki a kiszolgálók tooupdate](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

<span data-ttu-id="a2e7d-141">Hello konfiguráció befejezése után az Azure AD Connect hello frissítés hello állapotát jelzi, és egy beállítás tooverify hello AD FS-bejelentkezés biztosít hello üzenetet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-141">After you complete hello configuration, Azure AD Connect displays hello message that indicates hello status of hello update and provides an option tooverify hello AD FS sign-in.</span></span>

![Konfigurálás befejezése](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a><span data-ttu-id="a2e7d-143">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a2e7d-143">FAQs</span></span>

* <span data-ttu-id="a2e7d-144">**Milyen kell hello hello tanúsítvány tulajdonosnevének hello új AD FS SSL-tanúsítvány?**</span><span class="sxs-lookup"><span data-stu-id="a2e7d-144">**What should be hello subject name of hello certificate for hello new AD FS SSL certificate?**</span></span>

    <span data-ttu-id="a2e7d-145">Az Azure AD Connect ellenőrzi, hogy hello tulajdonos neve vagy másodlagos hello tanúsítvány tulajdonosnevének tartalmazza-e a hello összevonási szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-145">Azure AD Connect checks if hello subject name/alternate subject name of hello certificate contains hello federation service name.</span></span> <span data-ttu-id="a2e7d-146">Például ha az összevonási szolgáltatás nevének fs.contoso.com, a tulajdonos neve vagy másodlagos tulajdonosnév hello kell fs.contoso.com.  Helyettesítő tanúsítványok is használhatók.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-146">For example, if your federation service name is fs.contoso.com, hello subject name/alternate subject name must be fs.contoso.com.  Wildcard certificates are also accepted.</span></span>

* <span data-ttu-id="a2e7d-147">**Miért kérdezi meg az alkalmazás hitelesítő adatainak újra hello WAP-kiszolgáló lapon?**</span><span class="sxs-lookup"><span data-stu-id="a2e7d-147">**Why am I asked for credentials again on hello WAP server page?**</span></span>

    <span data-ttu-id="a2e7d-148">Ha a hello hitelesítő adatokat ad meg a tooAD FS kiszolgálókat is nincs hello jogosultság toomanage hello WAP-kiszolgálókkal, majd az Azure AD Connect hitelesítő adatokat kér, amely rendszergazdai jogosultságokkal rendelkezik a hello WAP-kiszolgálókkal.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-148">If hello credentials you provide for connecting tooAD FS servers don't also have hello privilege toomanage hello WAP servers, then Azure AD Connect asks for credentials that have administrative privilege on hello WAP servers.</span></span>

* <span data-ttu-id="a2e7d-149">**hello kiszolgáló offline állapotúként jelenik meg. Mit tegyek?**</span><span class="sxs-lookup"><span data-stu-id="a2e7d-149">**hello server is shown as offline. What should I do?**</span></span>

    <span data-ttu-id="a2e7d-150">Az Azure AD Connect nem hajtható végre semmilyen műveletet, ha hello kiszolgáló offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-150">Azure AD Connect can't perform any operation if hello server is offline.</span></span> <span data-ttu-id="a2e7d-151">Ha hello kiszolgáló hello AD FS-farm része, akkor ellenőrizze a hello kapcsolat toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-151">If hello server is part of hello AD FS farm, then check hello connectivity toohello server.</span></span> <span data-ttu-id="a2e7d-152">Miután hello problémát már megoldott, nyomja le az ENTER hello frissítés ikon tooupdate hello állapotának hello varázslóban.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-152">After you've resolved hello issue, press hello refresh icon tooupdate hello status in hello wizard.</span></span> <span data-ttu-id="a2e7d-153">Ha hello kiszolgáló része volt a hello farm korábban, de most már nem létezik, kattintson a **eltávolítása** toodelete hello listából az Azure AD Connect-kiszolgálók tart fenn.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-153">If hello server was part of hello farm earlier but now no longer exists, click **Remove** toodelete it from hello list of servers that Azure AD Connect maintains.</span></span> <span data-ttu-id="a2e7d-154">Az Azure AD Connect hello listájából eltávolításakor hello server hello maga az AD FS konfigurációs nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-154">Removing hello server from hello list in Azure AD Connect doesn't alter hello AD FS configuration itself.</span></span> <span data-ttu-id="a2e7d-155">Ha az AD FS a Windows Server 2016 vagy újabb, illetve hello server marad hello konfigurációs beállításokat használ, és megjelenik újra hello legközelebb hello feladat fut.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-155">If you're using AD FS in Windows Server 2016 or later, hello server remains in hello configuration settings and will be shown again hello next time hello task is run.</span></span>

* <span data-ttu-id="a2e7d-156">**Frissíthetem a farm kiszolgálói részhalmazának hello új SSL-tanúsítvány?**</span><span class="sxs-lookup"><span data-stu-id="a2e7d-156">**Can I update a subset of my farm servers with hello new SSL certificate?**</span></span>

    <span data-ttu-id="a2e7d-157">Igen.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-157">Yes.</span></span> <span data-ttu-id="a2e7d-158">Mindig hello feladatot futtathatja **frissítés SSL-tanúsítvány** újra tooupdate hello fennmaradó kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-158">You can always run hello task **Update SSL Certificate** again tooupdate hello remaining servers.</span></span> <span data-ttu-id="a2e7d-159">A hello **válassza kiszolgálók SSL tanúsítvány frissítés** lapon rendezheti hello azon kiszolgálók listájára, **SSL lejárati** tooeasily hello kiszolgálók, amelyek még nincsenek frissítve.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-159">On hello **Select servers for SSL certificate update** page, you can sort hello list of servers on **SSL Expiry date** tooeasily access hello servers that aren't updated yet.</span></span>

* <span data-ttu-id="a2e7d-160">**Hello-kiszolgálónak az előző hello eltávolított, de továbbra is alatt megjelenő, a kapcsolat nélküli és a felsorolt hello AD FS-kiszolgálók lapon. Miért van hello még a kapcsolat nélküli kiszolgáló, az eltávolított után is?**</span><span class="sxs-lookup"><span data-stu-id="a2e7d-160">**I removed hello server in hello previous run, but it's still being shown as offline and listed on hello AD FS Servers page. Why is hello offline server still there even after I removed it?**</span></span>

    <span data-ttu-id="a2e7d-161">Eltávolításakor hello server hello listájából az Azure AD Connect nem távolítható el az AD FS-konfiguráció hello.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-161">Removing hello server from hello list in Azure AD Connect doesn't remove it in hello AD FS configuration.</span></span> <span data-ttu-id="a2e7d-162">Az Azure AD Connect semmilyen információt hello farm AD FS (Windows Server 2016 vagy újabb) hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-162">Azure AD Connect references AD FS (Windows Server 2016 or higher) for any information about hello farm.</span></span> <span data-ttu-id="a2e7d-163">Ha hello kiszolgáló továbbra is megtalálhatók hello AD FS konfigurációt, az jelenik vissza hello listájában.</span><span class="sxs-lookup"><span data-stu-id="a2e7d-163">If hello server is still present in hello AD FS configuration, it will be listed back in hello list.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="a2e7d-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a2e7d-164">Next steps</span></span>

- [<span data-ttu-id="a2e7d-165">Azure AD Connect és összevonás</span><span class="sxs-lookup"><span data-stu-id="a2e7d-165">Azure AD Connect and federation</span></span>](active-directory-aadconnectfed-whatis.md)
- [<span data-ttu-id="a2e7d-166">Active Directory összevonási szolgáltatások kezelése és testreszabása az Azure AD Connecttel</span><span class="sxs-lookup"><span data-stu-id="a2e7d-166">Active Directory Federation Services management and customization with Azure AD Connect</span></span>](active-directory-aadconnect-federation-management.md)
