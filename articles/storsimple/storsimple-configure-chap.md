---
title: "A CHAP konfigurálása a StorSimple 8000 series eszköz |} Microsoft Docs"
description: "A StorSimple eszközön a Challenge Handshake Authentication Protocol (CHAP) beállításának módját ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 36b4e73d0336deb9560d44163fc5330d1c9d775c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="d5837-103">A CHAP konfigurálása a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="d5837-103">Configure CHAP for your StorSimple device</span></span>
<span data-ttu-id="d5837-104">Ez az oktatóanyag azt ismerteti, hogyan CHAP konfigurálása a StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="d5837-104">This tutorial explains how to configure CHAP for your StorSimple device.</span></span> <span data-ttu-id="d5837-105">Ebben a cikkben ismertetett eljárás a StorSimple 8000 series, valamint a StorSimple 1200-as eszközök vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d5837-105">The procedure detailed in this article applies to StorSimple 8000 series as well as StorSimple 1200 devices.</span></span>

<span data-ttu-id="d5837-106">A CHAP hitelesítési protokoll jelenti.</span><span class="sxs-lookup"><span data-stu-id="d5837-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="d5837-107">Egy távoli ügyfelek identitása érvényesítéséhez kiszolgálók által használt hitelesítési séma.</span><span class="sxs-lookup"><span data-stu-id="d5837-107">It is an authentication scheme used by servers to validate the identity of remote clients.</span></span> <span data-ttu-id="d5837-108">Az ellenőrzés megosztott jelszó vagy titkos kulcs alapul.</span><span class="sxs-lookup"><span data-stu-id="d5837-108">The verification is based on a shared password or secret.</span></span> <span data-ttu-id="d5837-109">Lehet, hogy a CHAP egyirányú (egyirányú) vagy a kölcsönös (kétirányú).</span><span class="sxs-lookup"><span data-stu-id="d5837-109">CHAP can be one-way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="d5837-110">Egyirányú CHAP akkor, ha a cél hitelesíti az kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="d5837-110">One-way CHAP is when the target authenticates an initiator.</span></span> <span data-ttu-id="d5837-111">Kölcsönös vagy fordított CHAP, másrészt megköveteli, hogy a cél hitelesítéséhez a kezdeményező és a kezdeményező hitelesítse a cél.</span><span class="sxs-lookup"><span data-stu-id="d5837-111">Mutual or reverse CHAP, on the other hand, requires that the target authenticate the initiator and then the initiator authenticate the target.</span></span> <span data-ttu-id="d5837-112">Kezdeményező hitelesítés nélkül cél hitelesítés valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="d5837-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="d5837-113">Cél hitelesítési esetén alkalmazhatóak azonban csak akkor, ha a kezdeményező hitelesítési is tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="d5837-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span> 

<span data-ttu-id="d5837-114">Ajánlott eljárásként azt javasoljuk, hogy használja-e CHAP iSCSI-biztonság javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d5837-114">As a best practice, we recommend that you use CHAP to enhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="d5837-115">Ne feledje, hogy IPSEC jelenleg nem támogatott a StorSimple eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="d5837-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>
> 
> 

<span data-ttu-id="d5837-116">A CHAP-beállításokat a StorSimple eszközön a következőképpen konfigurálható:</span><span class="sxs-lookup"><span data-stu-id="d5837-116">The CHAP settings on the StorSimple device can be configured in the following ways:</span></span>

* <span data-ttu-id="d5837-117">Egyirányú vagy egyirányú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="d5837-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="d5837-118">Kétirányú vagy fordított vagy kölcsönös hitelesítés</span><span class="sxs-lookup"><span data-stu-id="d5837-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="d5837-119">Az egyes ezekben az esetekben a portál az eszköz és a kiszolgáló iSCSI-kezdeményező szoftver kell megadni.</span><span class="sxs-lookup"><span data-stu-id="d5837-119">In each of these cases, the portal for the device and the server iSCSI initiator software needs to be configured.</span></span> <span data-ttu-id="d5837-120">Ez a konfiguráció részletes lépéseit a következő oktatóanyag ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d5837-120">The detailed steps for this configuration are described in the following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="d5837-121">Egyirányú vagy egyirányú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="d5837-121">Unidirectional or one-way authentication</span></span>
<span data-ttu-id="d5837-122">Egyirányú hitelesítést a tároló hitelesíti a kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="d5837-122">In unidirectional authentication, the target authenticates the initiator.</span></span> <span data-ttu-id="d5837-123">Ez a hitelesítés meg kell adni a CHAP-kezdeményező beállításokat a StorSimple eszközön, és az iSCSI-kezdeményező szoftver a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="d5837-123">This authentication requires that you configure the CHAP initiator settings on the StorSimple device and the iSCSI Initiator software on the host.</span></span> <span data-ttu-id="d5837-124">A StorSimple eszköz és a Windows-állomás részletes eljárásait olvashat.</span><span class="sxs-lookup"><span data-stu-id="d5837-124">The detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="to-configure-your-device-for-one-way-authentication"></a><span data-ttu-id="d5837-125">Az eszköz egyirányú hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d5837-125">To configure your device for one-way authentication</span></span>
1. <span data-ttu-id="d5837-126">A klasszikus Azure portálon a a **eszközök** lapján kattintson a **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="d5837-126">In the Azure classic portal, on the **Devices** page, click the **Configure** tab.</span></span>
   
    ![A CHAP-kezdeményező](./media/storsimple-configure-chap/IC740943.png)
2. <span data-ttu-id="d5837-128">Ezen a lapon, majd a görgessen lefelé a **CHAP-kezdeményező** szakasz:</span><span class="sxs-lookup"><span data-stu-id="d5837-128">Scroll down on this page, and in the **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="d5837-129">Adjon meg egy felhasználónevet a CHAP-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="d5837-129">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="d5837-130">Adjon meg egy jelszót a CHAP-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="d5837-130">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="d5837-131">A CHAP-felhasználónév kevesebb mint 233 karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d5837-131">The CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="d5837-132">A CHAP-jelszó 12 és 16 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="d5837-132">The CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="d5837-133">Hosszabb felhasználónevet vagy jelszót a Windows-állomás hitelesítési hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="d5837-133">A longer user name or password will result in an authentication failure on the Windows host.</span></span>
   
   3. <span data-ttu-id="d5837-134">Erősítse meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="d5837-134">Confirm the password.</span></span>
3. <span data-ttu-id="d5837-135">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d5837-135">Click **Save**.</span></span> <span data-ttu-id="d5837-136">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d5837-136">A confirmation message will be displayed.</span></span> <span data-ttu-id="d5837-137">A módosítások mentéséhez kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5837-137">Click **OK** to save the changes.</span></span>

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a><span data-ttu-id="d5837-138">Egyirányú hitelesítés konfigurálása a Windows gazdagép-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="d5837-138">To configure one-way authentication on the Windows host server</span></span>
1. <span data-ttu-id="d5837-139">A Windows gazdagép-kiszolgálón indítsa el az iSCSI-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="d5837-139">On the Windows host server, start the iSCSI Initiator.</span></span>
2. <span data-ttu-id="d5837-140">Az a **iSCSI-kezdeményező tulajdonságai** ablak, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d5837-140">In the **iSCSI Initiator Properties** window, perform the following steps:</span></span>
   
   1. <span data-ttu-id="d5837-141">Kattintson a **felderítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="d5837-141">Click the **Discovery** tab.</span></span>
      
       ![iSCSI-kezdeményező tulajdonságai](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="d5837-143">Kattintson a **Portal felderítése**.</span><span class="sxs-lookup"><span data-stu-id="d5837-143">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="d5837-144">Az a **tárolókapu felderítése** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="d5837-144">In the **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="d5837-145">Adja meg az eszköz IP-címét.</span><span class="sxs-lookup"><span data-stu-id="d5837-145">Specify the IP address of your device.</span></span>
   2. <span data-ttu-id="d5837-146">Kattintson a **speciális**.</span><span class="sxs-lookup"><span data-stu-id="d5837-146">Click **Advanced**.</span></span>
      
       ![Tárolókapu felderítése](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="d5837-148">Az a **speciális beállítások** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="d5837-148">In the **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="d5837-149">Válassza ki a **CHAP engedélyezése bejelentkezés** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d5837-149">Select the **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="d5837-150">Az a **neve** mezőben adja meg a felhasználónevet, a CHAP-kezdeményező a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="d5837-150">In the **Name** field, supply the user name that you specified for the CHAP Initiator in the classic portal.</span></span>
   3. <span data-ttu-id="d5837-151">Az a **cél titkos** mezőbe a jelszót a CHAP-kezdeményező a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="d5837-151">In the **Target secret** field, supply the password that you specified for the CHAP Initiator in the classic portal.</span></span>
   4. <span data-ttu-id="d5837-152">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5837-152">Click **OK**.</span></span>
      
       ![Általános speciális beállítások](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="d5837-154">Az a **célok** lapján a **iSCSI-kezdeményező tulajdonságai** ablakot, az eszköz állapotúnak kell lennie **csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="d5837-154">On the **Targets** tab of the **iSCSI Initiator Properties** window, the device status should appear as **Connected**.</span></span> <span data-ttu-id="d5837-155">Ha egy StorSimple 1200-as eszközt használ, majd minden olyan kötetre is csatlakoztatva, iSCSI-tároló alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="d5837-155">If you are using a StorSimple 1200 device, then each volume will be mounted as an iSCSI target as shown below.</span></span> <span data-ttu-id="d5837-156">Ezért 3 – 4. lépést kell minden olyan kötetre kell ismételni.</span><span class="sxs-lookup"><span data-stu-id="d5837-156">Hence, steps  3-4 will need to be repeated for each volume.</span></span>
   
    ![Külön célként csatlakoztatott kötetek](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="d5837-158">Ha módosítja az iSCSI-név, az új nevet az új iSCSI-munkamenetek használható.</span><span class="sxs-lookup"><span data-stu-id="d5837-158">If you change the iSCSI name, the new name will be used for new iSCSI sessions.</span></span> <span data-ttu-id="d5837-159">Új beállításai nem érvényesek a meglévő munkamenetekhez, amíg a Kijelentkezés és bejelentkezés újra.</span><span class="sxs-lookup"><span data-stu-id="d5837-159">New settings are not used for existing sessions until you log off and log on again.</span></span>
   > 
   > 

<span data-ttu-id="d5837-160">A Windows gazdakiszolgáló CHAP konfigurálásával kapcsolatos további információkért látogasson el [további szempontok](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="d5837-160">For more information about configuring CHAP on the Windows host server, go to [Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="d5837-161">Kétirányú vagy a kölcsönös hitelesítés</span><span class="sxs-lookup"><span data-stu-id="d5837-161">Bidirectional or mutual authentication</span></span>
<span data-ttu-id="d5837-162">Kétirányú hitelesítést a tároló hitelesíti a kezdeményező, és ezután a kezdeményező hitelesíti a cél.</span><span class="sxs-lookup"><span data-stu-id="d5837-162">In bidirectional authentication, the target authenticates the initiator and then the initiator authenticates the target.</span></span> <span data-ttu-id="d5837-163">A felhasználónak a CHAP-kezdeményező beállításait, valamint a fordított CHAP-beállítások konfigurálása az eszköz- és iSCSI-kezdeményező szoftver a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="d5837-163">This requires the user to configure the CHAP initiator settings, as well as the reverse CHAP settings on the device and iSCSI Initiator software on the host.</span></span> <span data-ttu-id="d5837-164">Az alábbi eljárásokat konfigurálásáról a kölcsönös hitelesítés, az eszközön, és a Windows-gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="d5837-164">The following procedures describe the steps to configure mutual authentication on the device and on the Windows host.</span></span>

#### <a name="to-configure-your-device-for-mutual-authentication"></a><span data-ttu-id="d5837-165">Az eszköz a kölcsönös hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d5837-165">To configure your device for mutual authentication</span></span>
1. <span data-ttu-id="d5837-166">A klasszikus Azure portálon a a **eszközök** lapján kattintson a **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="d5837-166">In the Azure classic portal, on the **Devices** page, click the **Configure** tab.</span></span>
   
    ![A CHAP-cél](./media/storsimple-configure-chap/IC740948.png)
2. <span data-ttu-id="d5837-168">Ezen a lapon, majd a görgessen lefelé a **CHAP-cél** szakasz:</span><span class="sxs-lookup"><span data-stu-id="d5837-168">Scroll down on this page, and in the **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="d5837-169">Adjon meg egy **fordított CHAP-felhasználónév** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="d5837-169">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="d5837-170">Adjon meg egy **fordított CHAP-jelszó** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="d5837-170">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="d5837-171">Erősítse meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="d5837-171">Confirm the password.</span></span>
3. <span data-ttu-id="d5837-172">Az a **CHAP-kezdeményező** szakasz:</span><span class="sxs-lookup"><span data-stu-id="d5837-172">In the **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="d5837-173">Adjon meg egy **felhasználónév** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="d5837-173">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="d5837-174">Adjon meg egy **jelszó** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="d5837-174">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="d5837-175">Erősítse meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="d5837-175">Confirm the password.</span></span>
4. <span data-ttu-id="d5837-176">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d5837-176">Click **Save**.</span></span> <span data-ttu-id="d5837-177">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d5837-177">A confirmation message will be displayed.</span></span> <span data-ttu-id="d5837-178">A módosítások mentéséhez kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5837-178">Click **OK** to save the changes.</span></span>

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a><span data-ttu-id="d5837-179">Kétirányú hitelesítés konfigurálása a Windows gazdagép-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="d5837-179">To configure bidirectional authentication on the Windows host server</span></span>
1. <span data-ttu-id="d5837-180">A Windows gazdagép-kiszolgálón indítsa el az iSCSI-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="d5837-180">On the Windows host server, start the iSCSI Initiator.</span></span>
2. <span data-ttu-id="d5837-181">Az a **iSCSI-kezdeményező tulajdonságai** ablak, kattintson a **konfigurációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="d5837-181">In the **iSCSI Initiator Properties** window, click the **Configuration** tab.</span></span>
3. <span data-ttu-id="d5837-182">Kattintson a **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="d5837-182">Click **CHAP**.</span></span>
4. <span data-ttu-id="d5837-183">Az a **iSCSI kezdeményező kölcsönös CHAP titkos** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="d5837-183">In the **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="d5837-184">Típus a **fordított CHAP-jelszó** konfigurált a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="d5837-184">Type the **Reverse CHAP Password** that you configured in the Azure classic portal.</span></span>
   2. <span data-ttu-id="d5837-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5837-185">Click **OK**.</span></span>
      
       ![iSCSI kezdeményező kölcsönös CHAP titkos kulcs](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="d5837-187">Kattintson a **célok** fülre.</span><span class="sxs-lookup"><span data-stu-id="d5837-187">Click the **Targets** tab.</span></span>
6. <span data-ttu-id="d5837-188">Kattintson a **Connect** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5837-188">Click the **Connect** button.</span></span> 
7. <span data-ttu-id="d5837-189">Az a **csatlakozás cél** párbeszédpanel, kattintson a **speciális**.</span><span class="sxs-lookup"><span data-stu-id="d5837-189">In the **Connect To Target** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="d5837-190">Az a **speciális tulajdonságok** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="d5837-190">In the **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="d5837-191">Válassza ki a **CHAP engedélyezése bejelentkezés** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d5837-191">Select the **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="d5837-192">Az a **neve** mezőben adja meg a felhasználónevet, a CHAP-kezdeményező a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="d5837-192">In the **Name** field, supply the user name that you specified for the CHAP Initiator in the classic portal.</span></span>
   3. <span data-ttu-id="d5837-193">Az a **cél titkos** mezőbe a jelszót a CHAP-kezdeményező a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="d5837-193">In the **Target secret** field, supply the password that you specified for the CHAP Initiator in the classic portal.</span></span>
   4. <span data-ttu-id="d5837-194">Válassza ki a **kölcsönös hitelesítés végrehajtása** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d5837-194">Select the **Perform mutual authentication** check box.</span></span>
      
       ![Speciális beállítások kölcsönös hitelesítés](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="d5837-196">Kattintson a **OK** CHAP konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d5837-196">Click **OK** to complete the CHAP configuration</span></span>

<span data-ttu-id="d5837-197">A Windows gazdakiszolgáló CHAP konfigurálásával kapcsolatos további információkért látogasson el [további szempontok](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="d5837-197">For more information about configuring CHAP on the Windows host server, go to [Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="d5837-198">Néhány fontos megjegyzés</span><span class="sxs-lookup"><span data-stu-id="d5837-198">Additional considerations</span></span>
<span data-ttu-id="d5837-199">A **gyors csatlakozás** funkció nem támogatja a kapcsolatokat, amelyek CHAP engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d5837-199">The **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="d5837-200">Ha CHAP engedélyezve van, győződjön meg arról, hogy használja a **Connect** gomb, amellyel elérhető a **célok** kapcsolódni a cél fülre.</span><span class="sxs-lookup"><span data-stu-id="d5837-200">When CHAP is enabled, make sure that you use the **Connect** button that is available on the **Targets** tab to connect to a target.</span></span>

![Cél kapcsolódni](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="d5837-202">Az a **cél kapcsolódás** párbeszédpanelen látható, válassza ki a **vegye fel ezt a kapcsolatot a kedvenc célok** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d5837-202">In the **Connect to Target** dialog box that is presented, select the **Add this connection to the list of Favorite Targets** check box.</span></span> <span data-ttu-id="d5837-203">Ez biztosítja, hogy minden alkalommal, amikor a számítógép újraindítását követően tett kísérlet a kapcsolat helyreállítására az iSCSI-kedvenc tárolókra.</span><span class="sxs-lookup"><span data-stu-id="d5837-203">This ensures that every time the computer restarts, an attempt is made to restore the connection to the iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="d5837-204">Konfigurálása során hibák</span><span class="sxs-lookup"><span data-stu-id="d5837-204">Errors during configuration</span></span>
<span data-ttu-id="d5837-205">Ha a CHAP konfigurációja nem megfelelő, akkor valószínű, hogy egy **hitelesítési hiba** hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d5837-205">If your CHAP configuration is incorrect, then you are likely to see an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="d5837-206">A CHAP-konfiguráció ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d5837-206">Verification of CHAP configuration</span></span>
<span data-ttu-id="d5837-207">Azt, hogy a CHAP használja a következő lépések végrehajtásával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="d5837-207">You can verify that CHAP is being used by completing the following steps.</span></span>

#### <a name="to-verify-your-chap-configuration"></a><span data-ttu-id="d5837-208">A CHAP konfigurációjának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d5837-208">To verify your CHAP configuration</span></span>
1. <span data-ttu-id="d5837-209">Kattintson a **kedvenc célok**.</span><span class="sxs-lookup"><span data-stu-id="d5837-209">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="d5837-210">Jelölje ki a cél, amelyhez hitelesítés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="d5837-210">Select the target for which you enabled authentication.</span></span>
3. <span data-ttu-id="d5837-211">Kattintson a **részletek**.</span><span class="sxs-lookup"><span data-stu-id="d5837-211">Click **Details**.</span></span>
   
    ![iSCSI-kezdeményező tulajdonságai kedvenc tárolók](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="d5837-213">Az a **kedvenc cél részletek** párbeszédpanelen jegyezze fel a bejegyzést a **hitelesítési** mező.</span><span class="sxs-lookup"><span data-stu-id="d5837-213">In the **Favorite Target Details** dialog box, note the entry in the **Authentication** field.</span></span> <span data-ttu-id="d5837-214">Ha a konfigurálás sikeres volt, akkor kell kapnia **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="d5837-214">If the configuration was successful, it should say **CHAP**.</span></span>
   
    ![Kedvenc cél részletei](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="d5837-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5837-216">Next steps</span></span>
* <span data-ttu-id="d5837-217">További információ [StorSimple biztonsági](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="d5837-217">Learn more about [StorSimple security](storsimple-security.md).</span></span>
* <span data-ttu-id="d5837-218">További információ [a StorSimple Manager szolgáltatás használata a StorSimple eszköz felügyeletéhez](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="d5837-218">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

