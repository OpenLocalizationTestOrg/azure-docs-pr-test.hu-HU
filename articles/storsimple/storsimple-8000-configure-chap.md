---
title: "a StorSimple 8000 series eszköz CHAP aaaConfigure |} Microsoft Docs"
description: "Ismerteti, hogyan tooconfigure hello a StorSimple eszközön Challenge Handshake Authentication Protocol (CHAP)."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 3351184b0317da7e3deae398bc0d63c3e5bd930f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="01e60-103">A CHAP konfigurálása a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="01e60-103">Configure CHAP for your StorSimple device</span></span>

<span data-ttu-id="01e60-104">Ez az oktatóanyag azt ismerteti, hogyan tooconfigure CHAP a StorSimple eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="01e60-104">This tutorial explains how tooconfigure CHAP for your StorSimple device.</span></span> <span data-ttu-id="01e60-105">Ebben a cikkben ismertetett hello eljárás érvényes tooStorSimple 8000 sorozat eszközeire.</span><span class="sxs-lookup"><span data-stu-id="01e60-105">hello procedure detailed in this article applies tooStorSimple 8000 series devices.</span></span>

<span data-ttu-id="01e60-106">A CHAP hitelesítési protokoll jelenti.</span><span class="sxs-lookup"><span data-stu-id="01e60-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="01e60-107">Olyan kiszolgálók toovalidate hello identitásának távoli ügyfelek által használt hitelesítési sémát.</span><span class="sxs-lookup"><span data-stu-id="01e60-107">It is an authentication scheme used by servers toovalidate hello identity of remote clients.</span></span> <span data-ttu-id="01e60-108">hello ellenőrzési megosztott jelszó vagy titkos kulcs alapul.</span><span class="sxs-lookup"><span data-stu-id="01e60-108">hello verification is based on a shared password or secret.</span></span> <span data-ttu-id="01e60-109">A CHAP biztosítható egy (egyirányú) vagy a kölcsönös (kétirányú).</span><span class="sxs-lookup"><span data-stu-id="01e60-109">CHAP can be one way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="01e60-110">Egyirányú CHAP esetén hello cél hitelesíti az kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="01e60-110">One way CHAP is when hello target authenticates an initiator.</span></span> <span data-ttu-id="01e60-111">A kölcsönös vagy fordított CHAP hello cél hitelesíti hello kezdeményező, és hogy hello kezdeményező majd hitelesíti a hello cél.</span><span class="sxs-lookup"><span data-stu-id="01e60-111">In mutual or reverse CHAP, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="01e60-112">Kezdeményező hitelesítés nélkül cél hitelesítés valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="01e60-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="01e60-113">Cél hitelesítési esetén alkalmazhatóak azonban csak akkor, ha a kezdeményező hitelesítési is tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="01e60-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span>

<span data-ttu-id="01e60-114">Ajánlott eljárásként azt javasoljuk, hogy használja-e CHAP tooenhance iSCSI biztonsági.</span><span class="sxs-lookup"><span data-stu-id="01e60-114">As a best practice, we recommend that you use CHAP tooenhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="01e60-115">Ne feledje, hogy IPSEC jelenleg nem támogatott a StorSimple eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="01e60-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>

<span data-ttu-id="01e60-116">a következő módokon hello hello CHAP beállításokat hello StorSimple eszközön konfigurálható:</span><span class="sxs-lookup"><span data-stu-id="01e60-116">hello CHAP settings on hello StorSimple device can be configured in hello following ways:</span></span>

* <span data-ttu-id="01e60-117">Egyirányú vagy egyirányú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="01e60-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="01e60-118">Kétirányú vagy fordított vagy kölcsönös hitelesítés</span><span class="sxs-lookup"><span data-stu-id="01e60-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="01e60-119">Az egyes ezekben az esetekben hello portal hello eszköz és hello server iSCSI-kezdeményező szoftver kell toobe konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="01e60-119">In each of these cases, hello portal for hello device and hello server iSCSI initiator software needs toobe configured.</span></span> <span data-ttu-id="01e60-120">hello részletes leírást ehhez a konfigurációhoz részelemcímkék ismertetését a következő oktatóanyag hello.</span><span class="sxs-lookup"><span data-stu-id="01e60-120">hello detailed steps for this configuration are described in hello following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="01e60-121">Egyirányú vagy egyirányú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="01e60-121">Unidirectional or one-way authentication</span></span>

<span data-ttu-id="01e60-122">Az egyirányú hitelesítési hello cél hello kezdeményező hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="01e60-122">In unidirectional authentication, hello target authenticates hello initiator.</span></span> <span data-ttu-id="01e60-123">A hitelesítéshez, hogy a beállításokat hello CHAP kezdeményező hello StorSimple eszköz és hello iSCSI-kezdeményező szoftver hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="01e60-123">This authentication requires that you configure hello CHAP initiator settings on hello StorSimple device and hello iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="01e60-124">hello részletesen ismertetik a StorSimple eszköz, és ezután ismerteti a Windows-állomás.</span><span class="sxs-lookup"><span data-stu-id="01e60-124">hello detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a><span data-ttu-id="01e60-125">tooconfigure az eszköz egyirányú hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="01e60-125">tooconfigure your device for one-way authentication</span></span>

1. <span data-ttu-id="01e60-126">A hello Azure-portálon válassza a tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="01e60-126">In hello Azure portal, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="01e60-127">Kattintson a **eszközök** , és válassza ki, és kattintson arra az eszközre tooconfigure CHAP kívánja a.</span><span class="sxs-lookup"><span data-stu-id="01e60-127">Click **Devices** and select and click a device you wish tooconfigure CHAP for.</span></span> <span data-ttu-id="01e60-128">Nyissa meg túl**eszközbeállítások > biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="01e60-128">Go too**Device settings > Security**.</span></span> <span data-ttu-id="01e60-129">A hello **biztonsági beállítások** panelen kattintson a **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="01e60-129">In hello **Security settings** blade, click **CHAP**.</span></span>
   
    ![A CHAP-kezdeményező](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. <span data-ttu-id="01e60-131">A hello **CHAP** panelt, és a hello **CHAP-kezdeményező** szakasz:</span><span class="sxs-lookup"><span data-stu-id="01e60-131">In hello **CHAP** blade, and in hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="01e60-132">Adjon meg egy felhasználónevet a CHAP-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="01e60-132">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="01e60-133">Adjon meg egy jelszót a CHAP-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="01e60-133">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="01e60-134">hello CHAP-felhasználónév kevesebb mint 233 karaktert kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="01e60-134">hello CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="01e60-135">hello CHAP-jelszó 12 és 16 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="01e60-135">hello CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="01e60-136">Hosszabb felhasználónevet vagy jelszót hello Windows gazdagépen hitelesítési hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="01e60-136">A longer user name or password results in an authentication failure on hello Windows host.</span></span>
   
   3. <span data-ttu-id="01e60-137">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="01e60-137">Confirm hello password.</span></span>

       ![A CHAP-kezdeményező](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. <span data-ttu-id="01e60-139">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="01e60-139">Click **Save**.</span></span> <span data-ttu-id="01e60-140">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="01e60-140">A confirmation message is displayed.</span></span> <span data-ttu-id="01e60-141">Kattintson a **OK** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="01e60-141">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a><span data-ttu-id="01e60-142">hello Windows-hitelesítés egyirányú tooconfigure gazdagép-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="01e60-142">tooconfigure one-way authentication on hello Windows host server</span></span>
1. <span data-ttu-id="01e60-143">A Windows hello gazdakiszolgálón indítsa el a hello iSCSI-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="01e60-143">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="01e60-144">A hello **iSCSI-kezdeményező tulajdonságai** ablak, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="01e60-144">In hello **iSCSI Initiator Properties** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="01e60-145">Kattintson a hello **felderítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="01e60-145">Click hello **Discovery** tab.</span></span>
      
       ![iSCSI-kezdeményező tulajdonságai](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="01e60-147">Kattintson a **Portal felderítése**.</span><span class="sxs-lookup"><span data-stu-id="01e60-147">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="01e60-148">A hello **tárolókapu felderítése** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="01e60-148">In hello **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="01e60-149">Adja meg az eszköz hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="01e60-149">Specify hello IP address of your device.</span></span>
   2. <span data-ttu-id="01e60-150">Kattintson a **speciális**.</span><span class="sxs-lookup"><span data-stu-id="01e60-150">Click **Advanced**.</span></span>
      
       ![Tárolókapu felderítése](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="01e60-152">A hello **speciális beállítások** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="01e60-152">In hello **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="01e60-153">Jelölje be hello **CHAP engedélyezése bejelentkezés** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="01e60-153">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="01e60-154">A hello **neve** mező, adjon meg hello felhasználónév hello CHAP-kezdeményező hello a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="01e60-154">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="01e60-155">A hello **cél titkos** mező, adjon meg hello jelszó hello CHAP-kezdeményező hello a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="01e60-155">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="01e60-156">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="01e60-156">Click **OK**.</span></span>
      
       ![Általános speciális beállítások](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="01e60-158">A hello **célok** hello lapján **iSCSI-kezdeményező tulajdonságai** ablakban hello eszköz állapotúnak kell lennie **csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="01e60-158">On hello **Targets** tab of hello **iSCSI Initiator Properties** window, hello device status should appear as **Connected**.</span></span> <span data-ttu-id="01e60-159">A StorSimple 1200-as eszközt használ, ha majd minden olyan kötetre, iSCSI-tároló van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="01e60-159">If you are using a StorSimple 1200 device, then each volume is mounted as an iSCSI target.</span></span> <span data-ttu-id="01e60-160">Ezért 3 – 4. lépést kell toobe ismétlődik minden kötete esetében.</span><span class="sxs-lookup"><span data-stu-id="01e60-160">Hence, steps 3-4 will need toobe repeated for each volume.</span></span>
   
    ![Külön célként csatlakoztatott kötetek](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="01e60-162">Ha módosítja a hello iSCSI nevét, hello új nevet használja az új iSCSI-munkameneteket.</span><span class="sxs-lookup"><span data-stu-id="01e60-162">If you change hello iSCSI name, hello new name is used for new iSCSI sessions.</span></span> <span data-ttu-id="01e60-163">Új beállításai nem érvényesek a meglévő munkamenetekhez, amíg a Kijelentkezés és bejelentkezés újra.</span><span class="sxs-lookup"><span data-stu-id="01e60-163">New settings are not used for existing sessions until you log off and log on again.</span></span>

<span data-ttu-id="01e60-164">További információ a CHAP konfigurálása hello Windows gazdagép-kiszolgálón nyissa meg túl[további szempontok](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="01e60-164">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="01e60-165">Kétirányú vagy a kölcsönös hitelesítés</span><span class="sxs-lookup"><span data-stu-id="01e60-165">Bidirectional or mutual authentication</span></span>

<span data-ttu-id="01e60-166">Kétirányú hitelesítést hello cél hello kezdeményező hitelesíti, és majd hello kezdeményező hitelesíti hello cél.</span><span class="sxs-lookup"><span data-stu-id="01e60-166">In bidirectional authentication, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="01e60-167">Ehhez az eljáráshoz szükséges hello felhasználói tooconfigure hello CHAP kezdeményező beállításait, a fordított irányú CHAP beállításainak hello eszközön, és az iSCSI-kezdeményező szoftver hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="01e60-167">This procedure requires hello user tooconfigure hello CHAP initiator settings, reverse CHAP settings on hello device, and iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="01e60-168">hello következő eljárásokat hello lépéseket tooconfigure kölcsönös hitelesítés hello eszköz és a Windows hello-állomás.</span><span class="sxs-lookup"><span data-stu-id="01e60-168">hello following procedures describe hello steps tooconfigure mutual authentication on hello device and on hello Windows host.</span></span>

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a><span data-ttu-id="01e60-169">tooconfigure az eszköz a kölcsönös hitelesítés</span><span class="sxs-lookup"><span data-stu-id="01e60-169">tooconfigure your device for mutual authentication</span></span>

1. <span data-ttu-id="01e60-170">A hello Azure-portálon válassza a tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="01e60-170">In hello Azure portal, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="01e60-171">Kattintson a **eszközök** , és válassza ki, és kattintson arra az eszközre tooconfigure CHAP kívánja a.</span><span class="sxs-lookup"><span data-stu-id="01e60-171">Click **Devices** and select and click a device you wish tooconfigure CHAP for.</span></span> <span data-ttu-id="01e60-172">Nyissa meg túl**eszközbeállítások > biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="01e60-172">Go too**Device settings > Security**.</span></span> <span data-ttu-id="01e60-173">A hello **biztonsági beállítások** panelen kattintson a **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="01e60-173">In hello **Security settings** blade, click **CHAP**.</span></span>
   
    ![A CHAP-cél](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. <span data-ttu-id="01e60-175">Görgessen lefelé, ezen a lapon, és a hello **CHAP-cél** szakasz:</span><span class="sxs-lookup"><span data-stu-id="01e60-175">Scroll down on this page, and in hello **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="01e60-176">Adjon meg egy **fordított CHAP-felhasználónév** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="01e60-176">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="01e60-177">Adjon meg egy **fordított CHAP-jelszó** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="01e60-177">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="01e60-178">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="01e60-178">Confirm hello password.</span></span>
3. <span data-ttu-id="01e60-179">A hello **CHAP-kezdeményező** szakasz:</span><span class="sxs-lookup"><span data-stu-id="01e60-179">In hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="01e60-180">Adjon meg egy **felhasználónév** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="01e60-180">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="01e60-181">Adjon meg egy **jelszó** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="01e60-181">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="01e60-182">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="01e60-182">Confirm hello password.</span></span>

       ![A CHAP-kezdeményező](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. <span data-ttu-id="01e60-184">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="01e60-184">Click **Save**.</span></span> <span data-ttu-id="01e60-185">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="01e60-185">A confirmation message is displayed.</span></span> <span data-ttu-id="01e60-186">Kattintson a **OK** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="01e60-186">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a><span data-ttu-id="01e60-187">a hello Windows tooconfigure kétirányú hitelesítést gazdagép-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="01e60-187">tooconfigure bidirectional authentication on hello Windows host server</span></span>

1. <span data-ttu-id="01e60-188">A Windows hello gazdakiszolgálón indítsa el a hello iSCSI-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="01e60-188">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="01e60-189">A hello **iSCSI-kezdeményező tulajdonságai** ablakában kattintson hello **konfigurációs** lapon.</span><span class="sxs-lookup"><span data-stu-id="01e60-189">In hello **iSCSI Initiator Properties** window, click hello **Configuration** tab.</span></span>
3. <span data-ttu-id="01e60-190">Kattintson a **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="01e60-190">Click **CHAP**.</span></span>
4. <span data-ttu-id="01e60-191">A hello **iSCSI kezdeményező kölcsönös CHAP titkos** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="01e60-191">In hello **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="01e60-192">Típus hello **fordított CHAP-jelszó** konfigurált hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="01e60-192">Type hello **Reverse CHAP Password** that you configured in hello Azure portal.</span></span>
   2. <span data-ttu-id="01e60-193">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="01e60-193">Click **OK**.</span></span>
      
       ![iSCSI kezdeményező kölcsönös CHAP titkos kulcs](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="01e60-195">Kattintson a hello **célok** fülre.</span><span class="sxs-lookup"><span data-stu-id="01e60-195">Click hello **Targets** tab.</span></span>
6. <span data-ttu-id="01e60-196">Kattintson a hello **Connect** gombra.</span><span class="sxs-lookup"><span data-stu-id="01e60-196">Click hello **Connect** button.</span></span> 
7. <span data-ttu-id="01e60-197">A hello **tooTarget csatlakozás** párbeszédpanel, kattintson a **speciális**.</span><span class="sxs-lookup"><span data-stu-id="01e60-197">In hello **Connect tooTarget** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="01e60-198">A hello **speciális tulajdonságok** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="01e60-198">In hello **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="01e60-199">Jelölje be hello **CHAP engedélyezése bejelentkezés** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="01e60-199">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="01e60-200">A hello **neve** mező, adjon meg hello felhasználónév hello CHAP-kezdeményező hello a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="01e60-200">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="01e60-201">A hello **cél titkos** mező, adjon meg hello jelszó hello CHAP-kezdeményező hello a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="01e60-201">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="01e60-202">Jelölje be hello **kölcsönös hitelesítés végrehajtása** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="01e60-202">Select hello **Perform mutual authentication** check box.</span></span>
      
       ![Speciális beállítások kölcsönös hitelesítés](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="01e60-204">Kattintson a **OK** toocomplete hello CHAP konfigurálása</span><span class="sxs-lookup"><span data-stu-id="01e60-204">Click **OK** toocomplete hello CHAP configuration</span></span>

<span data-ttu-id="01e60-205">További információ a CHAP konfigurálása hello Windows gazdagép-kiszolgálón nyissa meg túl[további szempontok](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="01e60-205">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="01e60-206">Néhány fontos megjegyzés</span><span class="sxs-lookup"><span data-stu-id="01e60-206">Additional considerations</span></span>

<span data-ttu-id="01e60-207">Hello **gyors csatlakozás** funkció nem támogatja a kapcsolatokat, amelyek CHAP engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="01e60-207">hello **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="01e60-208">Ha CHAP engedélyezve van, győződjön meg arról, hogy használja-e hello **Connect** hello elérhető gomb **célok** lapon tooconnect tooa cél.</span><span class="sxs-lookup"><span data-stu-id="01e60-208">When CHAP is enabled, make sure that you use hello **Connect** button that is available on hello **Targets** tab tooconnect tooa target.</span></span>

![Csatlakozás tootarget](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="01e60-210">A hello **tooTarget csatlakozás** nyújtani, jelölje be hello párbeszédpanel **adja hozzá a kedvenc tárolók kapcsolat toohello listája** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="01e60-210">In hello **Connect tooTarget** dialog box that is presented, select hello **Add this connection toohello list of Favorite Targets** check box.</span></span> <span data-ttu-id="01e60-211">Ez a beállítás biztosítja, hogy minden alkalommal, amikor újraindítás hello kísérlet toorestore hello kapcsolat toohello iSCSI-kedvenc tárolók.</span><span class="sxs-lookup"><span data-stu-id="01e60-211">This selection ensures that every time hello computer restarts, an attempt is made toorestore hello connection toohello iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="01e60-212">Konfigurálása során hibák</span><span class="sxs-lookup"><span data-stu-id="01e60-212">Errors during configuration</span></span>

<span data-ttu-id="01e60-213">Ha a CHAP konfigurációja nem megfelelő, akkor valószínűleg toosee áll egy **hitelesítési hiba** hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="01e60-213">If your CHAP configuration is incorrect, then you are likely toosee an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="01e60-214">A CHAP-konfiguráció ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="01e60-214">Verification of CHAP configuration</span></span>

<span data-ttu-id="01e60-215">Ellenőrizheti, hogy a CHAP használatos hello lépések elvégzésével.</span><span class="sxs-lookup"><span data-stu-id="01e60-215">You can verify that CHAP is being used by completing hello following steps.</span></span>

#### <a name="tooverify-your-chap-configuration"></a><span data-ttu-id="01e60-216">tooverify a CHAP konfigurálása</span><span class="sxs-lookup"><span data-stu-id="01e60-216">tooverify your CHAP configuration</span></span>
1. <span data-ttu-id="01e60-217">Kattintson a **kedvenc célok**.</span><span class="sxs-lookup"><span data-stu-id="01e60-217">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="01e60-218">Jelölje ki a hitelesítés használatára jogosult hello cél.</span><span class="sxs-lookup"><span data-stu-id="01e60-218">Select hello target for which you enabled authentication.</span></span>
3. <span data-ttu-id="01e60-219">Kattintson a **részletek**.</span><span class="sxs-lookup"><span data-stu-id="01e60-219">Click **Details**.</span></span>
   
    ![iSCSI-kezdeményező tulajdonságai kedvenc tárolók](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="01e60-221">A hello **kedvenc cél részletek** párbeszédpanelen Megjegyzés hello bejegyzést hello **hitelesítési** mező.</span><span class="sxs-lookup"><span data-stu-id="01e60-221">In hello **Favorite Target Details** dialog box, note hello entry in hello **Authentication** field.</span></span> <span data-ttu-id="01e60-222">Ha hello konfiguráció sikeres volt, akkor kell kapnia **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="01e60-222">If hello configuration was successful, it should say **CHAP**.</span></span>
   
    ![Kedvenc cél részletei](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="01e60-224">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="01e60-224">Next steps</span></span>

* <span data-ttu-id="01e60-225">További információ [StorSimple biztonsági](storsimple-8000-security.md).</span><span class="sxs-lookup"><span data-stu-id="01e60-225">Learn more about [StorSimple security](storsimple-8000-security.md).</span></span>
* <span data-ttu-id="01e60-226">További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="01e60-226">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

