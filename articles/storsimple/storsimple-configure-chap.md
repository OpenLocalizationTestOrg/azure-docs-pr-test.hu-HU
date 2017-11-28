---
title: "a StorSimple 8000 series eszköz CHAP aaaConfigure |} Microsoft Docs"
description: "Ismerteti, hogyan tooconfigure hello a StorSimple eszközön Challenge Handshake Authentication Protocol (CHAP)."
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
ms.openlocfilehash: 272ef2c184f56ad262e55410357494c72e45cf83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="8be43-103">A CHAP konfigurálása a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="8be43-103">Configure CHAP for your StorSimple device</span></span>
<span data-ttu-id="8be43-104">Ez az oktatóanyag azt ismerteti, hogyan tooconfigure CHAP a StorSimple eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="8be43-104">This tutorial explains how tooconfigure CHAP for your StorSimple device.</span></span> <span data-ttu-id="8be43-105">Ebben a cikkben ismertetett hello eljárás tooStorSimple 8000 sorozat, valamint a StorSimple 1200-as eszközök vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="8be43-105">hello procedure detailed in this article applies tooStorSimple 8000 series as well as StorSimple 1200 devices.</span></span>

<span data-ttu-id="8be43-106">A CHAP hitelesítési protokoll jelenti.</span><span class="sxs-lookup"><span data-stu-id="8be43-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="8be43-107">Olyan kiszolgálók toovalidate hello identitásának távoli ügyfelek által használt hitelesítési sémát.</span><span class="sxs-lookup"><span data-stu-id="8be43-107">It is an authentication scheme used by servers toovalidate hello identity of remote clients.</span></span> <span data-ttu-id="8be43-108">hello ellenőrzési megosztott jelszó vagy titkos kulcs alapul.</span><span class="sxs-lookup"><span data-stu-id="8be43-108">hello verification is based on a shared password or secret.</span></span> <span data-ttu-id="8be43-109">Lehet, hogy a CHAP egyirányú (egyirányú) vagy a kölcsönös (kétirányú).</span><span class="sxs-lookup"><span data-stu-id="8be43-109">CHAP can be one-way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="8be43-110">Egyirányú CHAP akkor, ha hello cél hitelesíti az kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="8be43-110">One-way CHAP is when hello target authenticates an initiator.</span></span> <span data-ttu-id="8be43-111">Kölcsönös vagy fordított CHAP, a hello, ugyanakkor szükséges, hogy hello cél hitelesítéséhez hello kezdeményező hello kezdeményező hitelesítse hello cél.</span><span class="sxs-lookup"><span data-stu-id="8be43-111">Mutual or reverse CHAP, on hello other hand, requires that hello target authenticate hello initiator and then hello initiator authenticate hello target.</span></span> <span data-ttu-id="8be43-112">Kezdeményező hitelesítés nélkül cél hitelesítés valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="8be43-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="8be43-113">Cél hitelesítési esetén alkalmazhatóak azonban csak akkor, ha a kezdeményező hitelesítési is tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="8be43-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span> 

<span data-ttu-id="8be43-114">Ajánlott eljárásként azt javasoljuk, hogy használja-e CHAP tooenhance iSCSI biztonsági.</span><span class="sxs-lookup"><span data-stu-id="8be43-114">As a best practice, we recommend that you use CHAP tooenhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="8be43-115">Ne feledje, hogy IPSEC jelenleg nem támogatott a StorSimple eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="8be43-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>
> 
> 

<span data-ttu-id="8be43-116">a következő módokon hello hello CHAP beállításokat hello StorSimple eszközön konfigurálható:</span><span class="sxs-lookup"><span data-stu-id="8be43-116">hello CHAP settings on hello StorSimple device can be configured in hello following ways:</span></span>

* <span data-ttu-id="8be43-117">Egyirányú vagy egyirányú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8be43-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="8be43-118">Kétirányú vagy fordított vagy kölcsönös hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8be43-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="8be43-119">Az egyes ezekben az esetekben hello portal hello eszköz és hello server iSCSI-kezdeményező szoftver kell toobe konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="8be43-119">In each of these cases, hello portal for hello device and hello server iSCSI initiator software needs toobe configured.</span></span> <span data-ttu-id="8be43-120">hello részletes leírást ehhez a konfigurációhoz részelemcímkék ismertetését a következő oktatóanyag hello.</span><span class="sxs-lookup"><span data-stu-id="8be43-120">hello detailed steps for this configuration are described in hello following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="8be43-121">Egyirányú vagy egyirányú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8be43-121">Unidirectional or one-way authentication</span></span>
<span data-ttu-id="8be43-122">Az egyirányú hitelesítési hello cél hello kezdeményező hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="8be43-122">In unidirectional authentication, hello target authenticates hello initiator.</span></span> <span data-ttu-id="8be43-123">A hitelesítéshez, hogy a beállításokat hello CHAP kezdeményező hello StorSimple eszköz és hello iSCSI-kezdeményező szoftver hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="8be43-123">This authentication requires that you configure hello CHAP initiator settings on hello StorSimple device and hello iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="8be43-124">hello részletesen ismertetik a StorSimple eszköz, és ezután ismerteti a Windows-állomás.</span><span class="sxs-lookup"><span data-stu-id="8be43-124">hello detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a><span data-ttu-id="8be43-125">tooconfigure az eszköz egyirányú hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="8be43-125">tooconfigure your device for one-way authentication</span></span>
1. <span data-ttu-id="8be43-126">A klasszikus Azure portálon, a hello hello **eszközök** hello kattintson **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="8be43-126">In hello Azure classic portal, on hello **Devices** page, click hello **Configure** tab.</span></span>
   
    ![A CHAP-kezdeményező](./media/storsimple-configure-chap/IC740943.png)
2. <span data-ttu-id="8be43-128">Görgessen lefelé, ezen a lapon, és a hello **CHAP-kezdeményező** szakasz:</span><span class="sxs-lookup"><span data-stu-id="8be43-128">Scroll down on this page, and in hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="8be43-129">Adjon meg egy felhasználónevet a CHAP-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="8be43-129">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="8be43-130">Adjon meg egy jelszót a CHAP-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="8be43-130">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="8be43-131">hello CHAP-felhasználónév kevesebb mint 233 karaktert kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="8be43-131">hello CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="8be43-132">hello CHAP-jelszó 12 és 16 karakter hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="8be43-132">hello CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="8be43-133">Hosszabb felhasználónevet vagy jelszót hello Windows gazdagépen hitelesítési hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="8be43-133">A longer user name or password will result in an authentication failure on hello Windows host.</span></span>
   
   3. <span data-ttu-id="8be43-134">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="8be43-134">Confirm hello password.</span></span>
3. <span data-ttu-id="8be43-135">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8be43-135">Click **Save**.</span></span> <span data-ttu-id="8be43-136">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8be43-136">A confirmation message will be displayed.</span></span> <span data-ttu-id="8be43-137">Kattintson a **OK** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="8be43-137">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a><span data-ttu-id="8be43-138">hello Windows-hitelesítés egyirányú tooconfigure gazdagép-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8be43-138">tooconfigure one-way authentication on hello Windows host server</span></span>
1. <span data-ttu-id="8be43-139">A Windows hello gazdakiszolgálón indítsa el a hello iSCSI-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="8be43-139">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="8be43-140">A hello **iSCSI-kezdeményező tulajdonságai** ablak, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8be43-140">In hello **iSCSI Initiator Properties** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="8be43-141">Kattintson a hello **felderítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="8be43-141">Click hello **Discovery** tab.</span></span>
      
       ![iSCSI-kezdeményező tulajdonságai](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="8be43-143">Kattintson a **Portal felderítése**.</span><span class="sxs-lookup"><span data-stu-id="8be43-143">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="8be43-144">A hello **tárolókapu felderítése** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="8be43-144">In hello **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="8be43-145">Adja meg az eszköz hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="8be43-145">Specify hello IP address of your device.</span></span>
   2. <span data-ttu-id="8be43-146">Kattintson a **speciális**.</span><span class="sxs-lookup"><span data-stu-id="8be43-146">Click **Advanced**.</span></span>
      
       ![Tárolókapu felderítése](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="8be43-148">A hello **speciális beállítások** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="8be43-148">In hello **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="8be43-149">Jelölje be hello **CHAP engedélyezése bejelentkezés** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8be43-149">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="8be43-150">A hello **neve** mező, adjon meg hello felhasználónév hello CHAP-kezdeményező hello a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="8be43-150">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="8be43-151">A hello **cél titkos** mező, adjon meg hello jelszó hello CHAP-kezdeményező hello a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="8be43-151">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="8be43-152">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8be43-152">Click **OK**.</span></span>
      
       ![Általános speciális beállítások](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="8be43-154">A hello **célok** hello lapján **iSCSI-kezdeményező tulajdonságai** ablakban hello eszköz állapotúnak kell lennie **csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="8be43-154">On hello **Targets** tab of hello **iSCSI Initiator Properties** window, hello device status should appear as **Connected**.</span></span> <span data-ttu-id="8be43-155">Ha egy StorSimple 1200-as eszközt használ, majd minden olyan kötetre is csatlakoztatva, iSCSI-tároló alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="8be43-155">If you are using a StorSimple 1200 device, then each volume will be mounted as an iSCSI target as shown below.</span></span> <span data-ttu-id="8be43-156">Ezért 3 – 4. lépést kell toobe ismétlődik minden kötete esetében.</span><span class="sxs-lookup"><span data-stu-id="8be43-156">Hence, steps  3-4 will need toobe repeated for each volume.</span></span>
   
    ![Külön célként csatlakoztatott kötetek](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="8be43-158">Ha hello iSCSI nevének módosításához hello új nevet az új iSCSI-munkamenetek használható.</span><span class="sxs-lookup"><span data-stu-id="8be43-158">If you change hello iSCSI name, hello new name will be used for new iSCSI sessions.</span></span> <span data-ttu-id="8be43-159">Új beállításai nem érvényesek a meglévő munkamenetekhez, amíg a Kijelentkezés és bejelentkezés újra.</span><span class="sxs-lookup"><span data-stu-id="8be43-159">New settings are not used for existing sessions until you log off and log on again.</span></span>
   > 
   > 

<span data-ttu-id="8be43-160">További információ a CHAP konfigurálása hello Windows gazdagép-kiszolgálón nyissa meg túl[további szempontok](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="8be43-160">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="8be43-161">Kétirányú vagy a kölcsönös hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8be43-161">Bidirectional or mutual authentication</span></span>
<span data-ttu-id="8be43-162">Kétirányú hitelesítést hello cél hello kezdeményező hitelesíti, és majd hello kezdeményező hitelesíti hello cél.</span><span class="sxs-lookup"><span data-stu-id="8be43-162">In bidirectional authentication, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="8be43-163">Ehhez a hello felhasználói tooconfigure hello CHAP kezdeményező beállításait, valamint hello fordított irányú CHAP beállításainak hello eszközön, és az iSCSI-kezdeményező szoftver hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="8be43-163">This requires hello user tooconfigure hello CHAP initiator settings, as well as hello reverse CHAP settings on hello device and iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="8be43-164">hello következő eljárásokat hello lépéseket tooconfigure kölcsönös hitelesítés hello eszköz és a Windows hello-állomás.</span><span class="sxs-lookup"><span data-stu-id="8be43-164">hello following procedures describe hello steps tooconfigure mutual authentication on hello device and on hello Windows host.</span></span>

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a><span data-ttu-id="8be43-165">tooconfigure az eszköz a kölcsönös hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8be43-165">tooconfigure your device for mutual authentication</span></span>
1. <span data-ttu-id="8be43-166">A klasszikus Azure portálon, a hello hello **eszközök** hello kattintson **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="8be43-166">In hello Azure classic portal, on hello **Devices** page, click hello **Configure** tab.</span></span>
   
    ![A CHAP-cél](./media/storsimple-configure-chap/IC740948.png)
2. <span data-ttu-id="8be43-168">Görgessen lefelé, ezen a lapon, és a hello **CHAP-cél** szakasz:</span><span class="sxs-lookup"><span data-stu-id="8be43-168">Scroll down on this page, and in hello **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="8be43-169">Adjon meg egy **fordított CHAP-felhasználónév** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="8be43-169">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="8be43-170">Adjon meg egy **fordított CHAP-jelszó** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="8be43-170">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="8be43-171">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="8be43-171">Confirm hello password.</span></span>
3. <span data-ttu-id="8be43-172">A hello **CHAP-kezdeményező** szakasz:</span><span class="sxs-lookup"><span data-stu-id="8be43-172">In hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="8be43-173">Adjon meg egy **felhasználónév** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="8be43-173">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="8be43-174">Adjon meg egy **jelszó** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="8be43-174">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="8be43-175">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="8be43-175">Confirm hello password.</span></span>
4. <span data-ttu-id="8be43-176">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8be43-176">Click **Save**.</span></span> <span data-ttu-id="8be43-177">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8be43-177">A confirmation message will be displayed.</span></span> <span data-ttu-id="8be43-178">Kattintson a **OK** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="8be43-178">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a><span data-ttu-id="8be43-179">a hello Windows tooconfigure kétirányú hitelesítést gazdagép-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8be43-179">tooconfigure bidirectional authentication on hello Windows host server</span></span>
1. <span data-ttu-id="8be43-180">A Windows hello gazdakiszolgálón indítsa el a hello iSCSI-kezdeményező.</span><span class="sxs-lookup"><span data-stu-id="8be43-180">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="8be43-181">A hello **iSCSI-kezdeményező tulajdonságai** ablakában kattintson hello **konfigurációs** lapon.</span><span class="sxs-lookup"><span data-stu-id="8be43-181">In hello **iSCSI Initiator Properties** window, click hello **Configuration** tab.</span></span>
3. <span data-ttu-id="8be43-182">Kattintson a **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="8be43-182">Click **CHAP**.</span></span>
4. <span data-ttu-id="8be43-183">A hello **iSCSI kezdeményező kölcsönös CHAP titkos** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="8be43-183">In hello **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="8be43-184">Típus hello **fordított CHAP-jelszó** konfigurált hello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="8be43-184">Type hello **Reverse CHAP Password** that you configured in hello Azure classic portal.</span></span>
   2. <span data-ttu-id="8be43-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8be43-185">Click **OK**.</span></span>
      
       ![iSCSI kezdeményező kölcsönös CHAP titkos kulcs](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="8be43-187">Kattintson a hello **célok** fülre.</span><span class="sxs-lookup"><span data-stu-id="8be43-187">Click hello **Targets** tab.</span></span>
6. <span data-ttu-id="8be43-188">Kattintson a hello **Connect** gombra.</span><span class="sxs-lookup"><span data-stu-id="8be43-188">Click hello **Connect** button.</span></span> 
7. <span data-ttu-id="8be43-189">A hello **tooTarget csatlakozás** párbeszédpanel, kattintson a **speciális**.</span><span class="sxs-lookup"><span data-stu-id="8be43-189">In hello **Connect tooTarget** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="8be43-190">A hello **speciális tulajdonságok** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="8be43-190">In hello **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="8be43-191">Jelölje be hello **CHAP engedélyezése bejelentkezés** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8be43-191">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="8be43-192">A hello **neve** mező, adjon meg hello felhasználónév hello CHAP-kezdeményező hello a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="8be43-192">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="8be43-193">A hello **cél titkos** mező, adjon meg hello jelszó hello CHAP-kezdeményező hello a klasszikus portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="8be43-193">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="8be43-194">Jelölje be hello **kölcsönös hitelesítés végrehajtása** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8be43-194">Select hello **Perform mutual authentication** check box.</span></span>
      
       ![Speciális beállítások kölcsönös hitelesítés](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="8be43-196">Kattintson a **OK** toocomplete hello CHAP konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8be43-196">Click **OK** toocomplete hello CHAP configuration</span></span>

<span data-ttu-id="8be43-197">További információ a CHAP konfigurálása hello Windows gazdagép-kiszolgálón nyissa meg túl[további szempontok](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="8be43-197">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="8be43-198">Néhány fontos megjegyzés</span><span class="sxs-lookup"><span data-stu-id="8be43-198">Additional considerations</span></span>
<span data-ttu-id="8be43-199">Hello **gyors csatlakozás** funkció nem támogatja a kapcsolatokat, amelyek CHAP engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="8be43-199">hello **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="8be43-200">Ha CHAP engedélyezve van, győződjön meg arról, hogy használja-e hello **Connect** hello elérhető gomb **célok** lapon tooconnect tooa cél.</span><span class="sxs-lookup"><span data-stu-id="8be43-200">When CHAP is enabled, make sure that you use hello **Connect** button that is available on hello **Targets** tab tooconnect tooa target.</span></span>

![Csatlakozás tootarget](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="8be43-202">A hello **tooTarget csatlakozás** nyújtani, jelölje be hello párbeszédpanel **adja hozzá a kedvenc tárolók kapcsolat toohello listája** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8be43-202">In hello **Connect tooTarget** dialog box that is presented, select hello **Add this connection toohello list of Favorite Targets** check box.</span></span> <span data-ttu-id="8be43-203">Ez biztosítja, hogy minden alkalommal, amikor hello számítógép újraindítását követően tett kísérlet toorestore hello kapcsolat toohello iSCSI-kedvenc tárolók.</span><span class="sxs-lookup"><span data-stu-id="8be43-203">This ensures that every time hello computer restarts, an attempt is made toorestore hello connection toohello iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="8be43-204">Konfigurálása során hibák</span><span class="sxs-lookup"><span data-stu-id="8be43-204">Errors during configuration</span></span>
<span data-ttu-id="8be43-205">Ha a CHAP konfigurációja nem megfelelő, akkor valószínűleg toosee áll egy **hitelesítési hiba** hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8be43-205">If your CHAP configuration is incorrect, then you are likely toosee an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="8be43-206">A CHAP-konfiguráció ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8be43-206">Verification of CHAP configuration</span></span>
<span data-ttu-id="8be43-207">Ellenőrizheti, hogy a CHAP használatos hello lépések elvégzésével.</span><span class="sxs-lookup"><span data-stu-id="8be43-207">You can verify that CHAP is being used by completing hello following steps.</span></span>

#### <a name="tooverify-your-chap-configuration"></a><span data-ttu-id="8be43-208">tooverify a CHAP konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8be43-208">tooverify your CHAP configuration</span></span>
1. <span data-ttu-id="8be43-209">Kattintson a **kedvenc célok**.</span><span class="sxs-lookup"><span data-stu-id="8be43-209">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="8be43-210">Jelölje ki a hitelesítés használatára jogosult hello cél.</span><span class="sxs-lookup"><span data-stu-id="8be43-210">Select hello target for which you enabled authentication.</span></span>
3. <span data-ttu-id="8be43-211">Kattintson a **részletek**.</span><span class="sxs-lookup"><span data-stu-id="8be43-211">Click **Details**.</span></span>
   
    ![iSCSI-kezdeményező tulajdonságai kedvenc tárolók](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="8be43-213">A hello **kedvenc cél részletek** párbeszédpanelen Megjegyzés hello bejegyzést hello **hitelesítési** mező.</span><span class="sxs-lookup"><span data-stu-id="8be43-213">In hello **Favorite Target Details** dialog box, note hello entry in hello **Authentication** field.</span></span> <span data-ttu-id="8be43-214">Ha hello konfiguráció sikeres volt, akkor kell kapnia **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="8be43-214">If hello configuration was successful, it should say **CHAP**.</span></span>
   
    ![Kedvenc cél részletei](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="8be43-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8be43-216">Next steps</span></span>
* <span data-ttu-id="8be43-217">További információ [StorSimple biztonsági](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="8be43-217">Learn more about [StorSimple security](storsimple-security.md).</span></span>
* <span data-ttu-id="8be43-218">További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="8be43-218">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

