---
title: "a Linux virtuális gép mentése Apache Tomcat aaaSet |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset be Apache Tomcat7 Linux operációs rendszert futtató Azure virtuális gépek használatával."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: b837a73e91fcb25d5459d993a0e93ceef1a1fc8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a><span data-ttu-id="f180c-103">A Linux virtuális gépek Azure-ral Tomcat7 beállítása</span><span class="sxs-lookup"><span data-stu-id="f180c-103">Set up Tomcat7 on a Linux virtual machine with Azure</span></span>
<span data-ttu-id="f180c-104">Apache Tomcat (vagy egyszerűen Tomcat is korábban Dzsakarta Tomcat) egy nyílt forráskódú webkiszolgáló és a servlet tároló hello Apache szoftver Foundation (ASP) által kidolgozott.</span><span class="sxs-lookup"><span data-stu-id="f180c-104">Apache Tomcat (or simply Tomcat, also formerly called Jakarta Tomcat) is an open source web server and servlet container developed by hello Apache Software Foundation (ASF).</span></span> <span data-ttu-id="f180c-105">Tomcat hello Java Servlet és hello JavaServer lapok (JSP) specifikációk a Sun Microsystems valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="f180c-105">Tomcat implements hello Java Servlet and hello JavaServer Pages (JSP) specifications from Sun Microsystems.</span></span> <span data-ttu-id="f180c-106">Tomcat mely toorun Java-kódot a tiszta Java HTTP-web server környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="f180c-106">Tomcat provides a pure Java HTTP web server environment in which toorun Java code.</span></span> <span data-ttu-id="f180c-107">Hello legegyszerűbb konfiguráció, a Tomcat egyetlen operációs rendszer folyamatban fut.</span><span class="sxs-lookup"><span data-stu-id="f180c-107">In hello simplest configuration, Tomcat runs in a single operating system process.</span></span> <span data-ttu-id="f180c-108">Ez a folyamat fut, a Java virtuális gép (JVM).</span><span class="sxs-lookup"><span data-stu-id="f180c-108">This process runs a Java virtual machine (JVM).</span></span> <span data-ttu-id="f180c-109">Minden HTTP-kérelem a egy böngésző tooTomcat hello Tomcat folyamat külön szálban végzi a rendszer.</span><span class="sxs-lookup"><span data-stu-id="f180c-109">Every HTTP request from a browser tooTomcat is processed as a separate thread in hello Tomcat process.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="f180c-110">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f180c-110">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f180c-111">Ez a cikk ismerteti, hogyan toouse hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="f180c-111">This article covers how toouse hello classic deployment model.</span></span> <span data-ttu-id="f180c-112">Azt javasoljuk, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="f180c-112">We recommend that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="f180c-113">a Resource Manager sablon toodeploy nyitott JDK és Tomcat Ubuntu virtuális gép toouse lásd [Ez a cikk](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span><span class="sxs-lookup"><span data-stu-id="f180c-113">toouse a Resource Manager template toodeploy an Ubuntu VM with Open JDK and Tomcat, see [this article](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span></span>

<span data-ttu-id="f180c-114">Ez a cikk Tomcat7 telepítése egy Linux-lemezképet, és telepítse az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f180c-114">In this article, you will install Tomcat7 on a Linux image and deploy it in Azure.</span></span>  

<span data-ttu-id="f180c-115">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="f180c-115">You will learn:</span></span>  

* <span data-ttu-id="f180c-116">Hogyan toocreate egy virtuális gép az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f180c-116">How toocreate a virtual machine in Azure.</span></span>
* <span data-ttu-id="f180c-117">Hogyan tooprepare Tomcat7 hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="f180c-117">How tooprepare hello virtual machine for Tomcat7.</span></span>
* <span data-ttu-id="f180c-118">Hogyan tooinstall Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="f180c-118">How tooinstall Tomcat7.</span></span>

<span data-ttu-id="f180c-119">Feltételezzük, hogy már rendelkezik Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="f180c-119">It is assumed that you already have an Azure subscription.</span></span>  <span data-ttu-id="f180c-120">Ha nem, regisztrálhat egy ingyenes próbaverziót: [hello Azure-webhelyen](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="f180c-120">If not, you can sign up for a free trial at [hello Azure website](https://azure.microsoft.com/).</span></span> <span data-ttu-id="f180c-121">Ha MSDN-előfizetéssel rendelkezik, tekintse meg [Microsoft Azure különleges árképzési: MSDN, az MPN és a BizSpark előnyök](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span><span class="sxs-lookup"><span data-stu-id="f180c-121">If you have an MSDN subscription, see [Microsoft Azure Special Pricing: MSDN, MPN, and BizSpark Benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span></span> <span data-ttu-id="f180c-122">További információ az Azure-ban toolearn lásd [Mi az Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span><span class="sxs-lookup"><span data-stu-id="f180c-122">toolearn more about Azure, see [What is Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span></span>

<span data-ttu-id="f180c-123">Ez a cikk feltételezi, hogy rendelkezik-e Tomcat- és Linux alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="f180c-123">This article assumes that you have a basic working knowledge of Tomcat and Linux.</span></span>  

## <a name="phase-1-create-an-image"></a><span data-ttu-id="f180c-124">1. fázis: Lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f180c-124">Phase 1: Create an image</span></span>
<span data-ttu-id="f180c-125">Ebben a fázisban egy virtuális gépet hoz létre egy Linux-lemezképet használja az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f180c-125">In this phase, you will create a virtual machine by using a Linux image in Azure.</span></span>  

### <a name="step-1-generate-an-ssh-authentication-key"></a><span data-ttu-id="f180c-126">1. lépés: Az SSH hitelesítési kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="f180c-126">Step 1: Generate an SSH authentication key</span></span>
<span data-ttu-id="f180c-127">Az SSH egy rendszergazdák számára fontos eszköze.</span><span class="sxs-lookup"><span data-stu-id="f180c-127">SSH is an important tool for system administrators.</span></span> <span data-ttu-id="f180c-128">HR-részleg által meghatározott jelszót alapuló hozzáférés biztonsági beállításainak megadása azonban nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="f180c-128">However, configuring access security based on a human-determined password is not recommended.</span></span> <span data-ttu-id="f180c-129">Rosszindulatú felhasználók is felosztása a rendszer a felhasználónév és a gyenge jelszót alapján.</span><span class="sxs-lookup"><span data-stu-id="f180c-129">Malicious users can break into your system based on a username and a weak password.</span></span>

<span data-ttu-id="f180c-130">hello jó hírünk, hogy nincs-e olyan módon tooleave távelérési megnyitásához, és nem kell foglalkoznia jelszavak.</span><span class="sxs-lookup"><span data-stu-id="f180c-130">hello good news is that there is a way tooleave remote access open and not worry about passwords.</span></span> <span data-ttu-id="f180c-131">Ez a módszer a hitelesítés az aszimmetrikus titkosítási áll.</span><span class="sxs-lookup"><span data-stu-id="f180c-131">This method consists of authentication with asymmetric cryptography.</span></span> <span data-ttu-id="f180c-132">hello felhasználó a titkos kulcs egy, amely engedélyezi a hello hitelesítési hello.</span><span class="sxs-lookup"><span data-stu-id="f180c-132">hello user’s private key is hello one that grants hello authentication.</span></span> <span data-ttu-id="f180c-133">Hello felhasználói fiók még akkor is zárolhatja toonot jelszó-hitelesítés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f180c-133">You can even lock hello user’s account toonot allow password authentication.</span></span>

<span data-ttu-id="f180c-134">Egy másik ezt a módszert előnye, hogy nem kell különböző jelszóból toosign toodifferent-kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="f180c-134">Another advantage of this method is that you do not need different passwords toosign in toodifferent servers.</span></span> <span data-ttu-id="f180c-135">Hello személyes titkos kulcs használatával az összes kiszolgálón megakadályozza, hogy jelszavainak tooremember több hitelesítheti.</span><span class="sxs-lookup"><span data-stu-id="f180c-135">You can authenticate by using hello personal private key on all servers, which prevents you from having tooremember several passwords.</span></span>



<span data-ttu-id="f180c-136">Hajtsa végre az alábbi lépéseket toogenerate hello SSH hitelesítési kulcs.</span><span class="sxs-lookup"><span data-stu-id="f180c-136">Follow these steps toogenerate hello SSH authentication key.</span></span>

1. <span data-ttu-id="f180c-137">Töltse le és telepítse a következő helyen hello PuTTYgen: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="f180c-137">Download and install PuTTYgen from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="f180c-138">Futtassa a Puttygen.exe.</span><span class="sxs-lookup"><span data-stu-id="f180c-138">Run Puttygen.exe.</span></span>
3. <span data-ttu-id="f180c-139">Kattintson a **Generate** toogenerate hello kulcsok.</span><span class="sxs-lookup"><span data-stu-id="f180c-139">Click **Generate** toogenerate hello keys.</span></span> <span data-ttu-id="f180c-140">Hello folyamat során is fokozható annak véletlenszerű áthelyezése hello egér által hello üres területet hello ablakban.</span><span class="sxs-lookup"><span data-stu-id="f180c-140">In hello process, you can increase randomness by moving hello mouse over hello blank area in hello window.</span></span>  
   <span data-ttu-id="f180c-141">![PuTTY kulcs generátor képernyőkép hello létrehozás új kulcs gomb][1]</span><span class="sxs-lookup"><span data-stu-id="f180c-141">![PuTTY Key Generator screenshot that shows hello generate new key button][1]</span></span>
4. <span data-ttu-id="f180c-142">Miután hello folyamat létrehozása, Puttygen.exe jeleníti meg az új nyilvános kulcshoz.</span><span class="sxs-lookup"><span data-stu-id="f180c-142">After hello generate process, Puttygen.exe will show your new public key.</span></span>  
   ![PuTTY kulcs generátor képernyőkép hello új nyilvános és titkos kulcs gomb mentés hello][2]
5. <span data-ttu-id="f180c-144">Válassza ki és hello nyilvános kulcsának az átmásolása, és mentse a munkafüzetet egy publicKey.pem nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="f180c-144">Select and copy hello public key, and save it in a file named publicKey.pem.</span></span> <span data-ttu-id="f180c-145">Ne kattintson **mentés nyilvános kulcs**, mert hello mentett nyilvános kulcs fájl formátuma nem azonos a hello azt szeretnénk, ha nyilvános kulcs.</span><span class="sxs-lookup"><span data-stu-id="f180c-145">Don’t click **Save public key**, because hello saved public key’s file format is different from hello public key we want.</span></span>
6. <span data-ttu-id="f180c-146">Kattintson a **mentés titkos kulcs**, és mentse a munkafüzetet egy privateKey.ppk nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="f180c-146">Click **Save private key**, and save it in a file named privateKey.ppk.</span></span>

### <a name="step-2-create-hello-image-in-hello-azure-portal"></a><span data-ttu-id="f180c-147">2. lépés: Hello lemezképének létrehozásához a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f180c-147">Step 2: Create hello image in hello Azure portal</span></span>
1. <span data-ttu-id="f180c-148">A hello [portal](https://portal.azure.com/), kattintson a **új** hello a feladat toocreate látható kép.</span><span class="sxs-lookup"><span data-stu-id="f180c-148">In hello [portal](https://portal.azure.com/), click **New** in hello task bar toocreate an image.</span></span> <span data-ttu-id="f180c-149">Válassza ki a hello Linux képet, amely alapul, az igényeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="f180c-149">Then choose hello Linux image that is based on your needs.</span></span> <span data-ttu-id="f180c-150">hello alábbi példa hello Ubuntu 14.04 lemezképet használja.</span><span class="sxs-lookup"><span data-stu-id="f180c-150">hello following example uses hello Ubuntu 14.04 image.</span></span>
<span data-ttu-id="f180c-151">![Képernyőfelvétel a hello portál, amely hello új gomb][3]</span><span class="sxs-lookup"><span data-stu-id="f180c-151">![Screenshot of hello portal that shows hello New button][3]</span></span>

2. <span data-ttu-id="f180c-152">A **állomásnév**, adja meg, hogy Ön és az internetes ügyfeleket használja tooaccess a virtuális gép hello URL hello nevét.</span><span class="sxs-lookup"><span data-stu-id="f180c-152">For **Host Name**, specify hello name for hello URL that you and Internet clients will use tooaccess this virtual machine.</span></span> <span data-ttu-id="f180c-153">Adja meg a hello hello DNS-nevét, például tomcatdemo utolsó része.</span><span class="sxs-lookup"><span data-stu-id="f180c-153">Define hello last part of hello DNS name, for example, tomcatdemo.</span></span> <span data-ttu-id="f180c-154">Azure ekkor létrehozza, tomcatdemo.cloudapp.net hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f180c-154">Azure will then generate hello URL as tomcatdemo.cloudapp.net.</span></span>  

3. <span data-ttu-id="f180c-155">A **SSH hitelesítési kulcs**, hello kulcsérték másolása hello publicKey.pem fájl, amely tartalmazza a PuTTYgen által létrehozott hello nyilvános kulcs.</span><span class="sxs-lookup"><span data-stu-id="f180c-155">For **SSH Authentication Key**, copy hello key value from hello publicKey.pem file, which contains hello public key generated by PuTTYgen.</span></span>  
<span data-ttu-id="f180c-156">![SSH hitelesítési kulcs hello portal párbeszédpanel][4]</span><span class="sxs-lookup"><span data-stu-id="f180c-156">![SSH Authentication Key box in hello portal][4]</span></span>

4. <span data-ttu-id="f180c-157">Igény szerint konfigurálhatja az egyéb beállításokat, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f180c-157">Configure other settings as needed, and then click **Create**.</span></span>  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a><span data-ttu-id="f180c-158">2. fázis: A Tomcat7 a virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="f180c-158">Phase 2: Prepare your virtual machine for Tomcat7</span></span>
<span data-ttu-id="f180c-159">Ebben a fázisban a Tomcat-forgalmat a végpont konfigurálása, és csatlakoztassa az új virtuális gép tooyour.</span><span class="sxs-lookup"><span data-stu-id="f180c-159">In this phase, you will configure an endpoint for Tomcat traffic, and then connect tooyour new virtual machine.</span></span>

### <a name="step-1-open-hello-http-port-tooallow-web-access"></a><span data-ttu-id="f180c-160">1. lépés: Nyissa meg a hello HTTP port tooallow webes elérés</span><span class="sxs-lookup"><span data-stu-id="f180c-160">Step 1: Open hello HTTP port tooallow web access</span></span>
<span data-ttu-id="f180c-161">Az Azure-végpontok közé tartozik a TCP vagy UDP protokoll, valamint nyilvános és magánhálózati portot.</span><span class="sxs-lookup"><span data-stu-id="f180c-161">Endpoints in Azure consist of a TCP or UDP protocol, along with a public and private port.</span></span> <span data-ttu-id="f180c-162">hello a magánhálózati port megadása, hogy hello szolgáltatás figyeli-e tooon hello virtuális gép hello port.</span><span class="sxs-lookup"><span data-stu-id="f180c-162">hello private port is hello port that hello service is listening tooon hello virtual machine.</span></span> <span data-ttu-id="f180c-163">hello nyilvános port hello Azure-felhőszolgáltatásban hello portot figyeli a bejövő, az internetes forgalmat tooexternally.</span><span class="sxs-lookup"><span data-stu-id="f180c-163">hello public port is hello port that hello Azure cloud service listens tooexternally for incoming, Internet-based traffic.</span></span>  

<span data-ttu-id="f180c-164">8080-as TCP-port hello alapértelmezett portszámot, hogy a Tomcat toolisten használja.</span><span class="sxs-lookup"><span data-stu-id="f180c-164">TCP port 8080 is hello default port number that Tomcat uses toolisten.</span></span> <span data-ttu-id="f180c-165">Ha ezt a portot, ahol az Azure-végpont, és a más internetes ügyfelek hozzáférhet Tomcat lapok.</span><span class="sxs-lookup"><span data-stu-id="f180c-165">If this port is opened with an Azure endpoint, you and other Internet clients can access Tomcat pages.</span></span>  

1. <span data-ttu-id="f180c-166">Hello portálon kattintson **Tallózás** > **virtuális gépek**, majd kattintson a létrehozott hello virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="f180c-166">In hello portal, click **Browse** > **Virtual machines**, and then click hello virtual machine that you created.</span></span>  
   <span data-ttu-id="f180c-167">![Képernyőfelvétel a hello Virtual machines könyvtár][5]</span><span class="sxs-lookup"><span data-stu-id="f180c-167">![Screenshot of hello Virtual machines directory][5]</span></span>
2. <span data-ttu-id="f180c-168">tooadd egy végpont tooyour virtuális gépet, kattintson a hello **végpontok** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f180c-168">tooadd an endpoint tooyour virtual machine, click hello **Endpoints** box.</span></span>
   <span data-ttu-id="f180c-169">![Képernyőkép a hello végpontok mezőbe][6]</span><span class="sxs-lookup"><span data-stu-id="f180c-169">![Screenshot that shows hello Endpoints box][6]</span></span>
3. <span data-ttu-id="f180c-170">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f180c-170">Click **Add**.</span></span>  

   1. <span data-ttu-id="f180c-171">Hello végpont, adjon meg egy nevet a hello végpont **végpont**, és írja be a 80-as **nyilvános Port**.</span><span class="sxs-lookup"><span data-stu-id="f180c-171">For hello endpoint, enter a name for hello endpoint in **Endpoint**, and then enter 80 in **Public Port**.</span></span>  

      <span data-ttu-id="f180c-172">Ha a megadott érték too80, nem kell tooinclude hello portszámot, amely használt tooaccess Tomcat hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="f180c-172">If you set it too80, you don’t need tooinclude hello port number in hello URL that is used tooaccess Tomcat.</span></span> <span data-ttu-id="f180c-173">Például http://tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="f180c-173">For example, http://tomcatdemo.cloudapp.net.</span></span>    

      <span data-ttu-id="f180c-174">Ha azt tooanother érték, például a 81-es, tooadd hello port száma toohello URL-cím tooaccess Tomcat kell.</span><span class="sxs-lookup"><span data-stu-id="f180c-174">If you set it tooanother value, such as 81, you need tooadd hello port number toohello URL tooaccess Tomcat.</span></span> <span data-ttu-id="f180c-175">Például http://tomcatdemo.cloudapp.net:81 /.</span><span class="sxs-lookup"><span data-stu-id="f180c-175">For example,  http://tomcatdemo.cloudapp.net:81/.</span></span>
   2. <span data-ttu-id="f180c-176">Adja meg a 8080-as **magánhálózati Port**.</span><span class="sxs-lookup"><span data-stu-id="f180c-176">Enter 8080 in **Private Port**.</span></span> <span data-ttu-id="f180c-177">Alapértelmezés szerint a Tomcat a 8080-as TCP-portot figyeli.</span><span class="sxs-lookup"><span data-stu-id="f180c-177">By default, Tomcat listens on TCP port 8080.</span></span> <span data-ttu-id="f180c-178">Ha módosította hello alapértelmezett figyelési Tomcat port, frissítenie kell **magánhálózati Port** toobe hello megegyeznek a Tomcat figyelési portja hello.</span><span class="sxs-lookup"><span data-stu-id="f180c-178">If you changed hello default listen port of Tomcat, you should update **Private Port** toobe hello same as hello Tomcat listen port.</span></span>  
      <span data-ttu-id="f180c-179">![Képernyőfelvétel a felhasználói felület Hozzáadás parancs, a nyilvános Port és a magánhálózati Port jeleníti meg][7]</span><span class="sxs-lookup"><span data-stu-id="f180c-179">![Screenshot of UI that shows Add command, Public Port, and Private Port][7]</span></span>
4. <span data-ttu-id="f180c-180">Kattintson a **OK** tooadd hello végpont tooyour virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="f180c-180">Click **OK** tooadd hello endpoint tooyour virtual machine.</span></span>

### <a name="step-2-connect-toohello-image-you-created"></a><span data-ttu-id="f180c-181">2. lépés: Csatlakozás létrehozott toohello kép</span><span class="sxs-lookup"><span data-stu-id="f180c-181">Step 2: Connect toohello image you created</span></span>
<span data-ttu-id="f180c-182">Kiválaszthatja a SSH eszköz tooconnect tooyour virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="f180c-182">You can choose any SSH tool tooconnect tooyour virtual machine.</span></span> <span data-ttu-id="f180c-183">A jelen példában használjuk a PuTTY.</span><span class="sxs-lookup"><span data-stu-id="f180c-183">In this example, we use PuTTY.</span></span>  

1. <span data-ttu-id="f180c-184">A virtuális gép hello DNS-neve beszerzése hello portálról.</span><span class="sxs-lookup"><span data-stu-id="f180c-184">Get hello DNS name of your virtual machine from hello portal.</span></span>
    1. <span data-ttu-id="f180c-185">Kattintson a **Tallózás** > **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="f180c-185">Click **Browse** > **Virtual machines**.</span></span>
    2. <span data-ttu-id="f180c-186">Válassza ki a virtuális gép hello nevét, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="f180c-186">Select hello name of your virtual machine, and then click **Properties**.</span></span>
    3. <span data-ttu-id="f180c-187">A hello **tulajdonságok** csempéjén kattintson a jobb hello hely **tartománynév** mezőben tooget hello DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="f180c-187">In hello **Properties** tile, look in hello **Domain Name** box tooget hello DNS name.</span></span>  

2. <span data-ttu-id="f180c-188">Az SSH-kapcsolatokhoz a hello hello portszám beolvasása **SSH** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f180c-188">Get hello port number for SSH connections from hello **SSH** box.</span></span>  
<span data-ttu-id="f180c-189">![Képernyőkép a hello SSH-kapcsolat portszám][8]</span><span class="sxs-lookup"><span data-stu-id="f180c-189">![Screenshot that shows hello SSH connection port number][8]</span></span>

3. <span data-ttu-id="f180c-190">Töltse le [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="f180c-190">Download [PuTTY](http://www.putty.org/).</span></span>  

4. <span data-ttu-id="f180c-191">A letöltés után kattintson a hello végrehajtható fájl Putty.exe.</span><span class="sxs-lookup"><span data-stu-id="f180c-191">After downloading, click hello executable file Putty.exe.</span></span> <span data-ttu-id="f180c-192">A PuTTY konfigurációs beállításokat adhat meg hello alapszintű hello állomásnévvel és származik a virtuális gép tulajdonságainak hello portszámát.</span><span class="sxs-lookup"><span data-stu-id="f180c-192">In PuTTY configuration, configure hello basic options with hello host name and port number that is obtained from hello properties of your virtual machine.</span></span>   
![Képernyőkép a hello PuTTY konfigurációs állomás neve és portszáma beállítások][9]

5. <span data-ttu-id="f180c-194">Hello bal oldali ablaktáblában kattintson **kapcsolat** > **SSH** > **Auth**, és kattintson a **Tallózás** toospecify hello hello privateKey.ppk fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="f180c-194">In hello left pane, click **Connection** > **SSH** > **Auth**, and then click **Browse** toospecify hello location of hello privateKey.ppk file.</span></span> <span data-ttu-id="f180c-195">hello privateKey.ppk fájl tartalmazza a titkos kulcs hello hello a korábbi PuTTYgen által létrehozott "1. fázis: lemezkép létrehozása" című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="f180c-195">hello privateKey.ppk file contains hello private key that is generated by PuTTYgen earlier in hello "Phase 1: Create an image" section of this article.</span></span>  
<span data-ttu-id="f180c-196">![Képernyőkép a hello kapcsolat könyvtár-hierarchia és a böngészés gombra.][10]</span><span class="sxs-lookup"><span data-stu-id="f180c-196">![Screenshot that shows hello Connection directory hierarchy and Browse button][10]</span></span>

6. <span data-ttu-id="f180c-197">Kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="f180c-197">Click **Open**.</span></span> <span data-ttu-id="f180c-198">Így előfordulhat, hogy riasztást, által egy üzenet mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f180c-198">You might be alerted by a message box.</span></span> <span data-ttu-id="f180c-199">Ha a konfigurált hello DNS-nevét és portszámát megfelelően, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="f180c-199">If you have configured hello DNS name and port number correctly, click **Yes**.</span></span>
<span data-ttu-id="f180c-200">![Képernyőkép a hello értesítés][11]</span><span class="sxs-lookup"><span data-stu-id="f180c-200">![Screenshot that shows hello notification][11]</span></span>

7. <span data-ttu-id="f180c-201">Meg vannak felszólító tooenter a felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="f180c-201">You are prompted tooenter your username.</span></span>  
![Képernyőkép a where tooenter felhasználónév][12]

8. <span data-ttu-id="f180c-203">Adja meg a toocreate hello virtuális gép szerepel hello hello felhasználónév "1. fázis: lemezkép létrehozása" című rész ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="f180c-203">Enter hello username that you used toocreate hello virtual machine in hello "Phase 1: Create an image" section earlier in this article.</span></span> <span data-ttu-id="f180c-204">Hello következő hasonlót fog látni:</span><span class="sxs-lookup"><span data-stu-id="f180c-204">You will see something like hello following:</span></span>  
![Képernyőkép a hello hitelesítési megerősítése][13]

## <a name="phase-3-install-software"></a><span data-ttu-id="f180c-206">3. fázis: A szoftver telepítése</span><span class="sxs-lookup"><span data-stu-id="f180c-206">Phase 3: Install software</span></span>
<span data-ttu-id="f180c-207">Ebben a fázisban telepíti a hello Java-futtatókörnyezet, Tomcat7 és egyéb Tomcat7 összetevőket.</span><span class="sxs-lookup"><span data-stu-id="f180c-207">In this phase, you install hello Java runtime environment, Tomcat7, and other Tomcat7 components.</span></span>  

### <a name="java-runtime-environment"></a><span data-ttu-id="f180c-208">Java-futtatókörnyezet</span><span class="sxs-lookup"><span data-stu-id="f180c-208">Java runtime environment</span></span>
<span data-ttu-id="f180c-209">Tomcat Java nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="f180c-209">Tomcat is written in Java.</span></span> <span data-ttu-id="f180c-210">Java fejlesztői csomagok (JDKs), OpenJDK és Oracle JDK két típusú léteznek.</span><span class="sxs-lookup"><span data-stu-id="f180c-210">There are two kinds of Java Development Kits (JDKs), OpenJDK and Oracle JDK.</span></span> <span data-ttu-id="f180c-211">Kiválaszthatja a kívánt hello.</span><span class="sxs-lookup"><span data-stu-id="f180c-211">You can choose hello one you want.</span></span>  

> [!NOTE]
> <span data-ttu-id="f180c-212">Mind JDKs majdnem megegyezik a hello osztályokhoz hello Java API-kód hello rendelkezik, de hello virtuális gép hello kód nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="f180c-212">Both JDKs have almost hello same code for hello classes in hello Java API, but hello code for hello virtual machine is different.</span></span> <span data-ttu-id="f180c-213">OpenJDK általában toouse nyitott szalagtárak, amíg Oracle JDK általában toouse lezárt azokat.</span><span class="sxs-lookup"><span data-stu-id="f180c-213">OpenJDK tends toouse open libraries, while Oracle JDK tends toouse closed ones.</span></span> <span data-ttu-id="f180c-214">Oracle JDK van több osztályok és néhány rögzített hibák, és Oracle JDK stabilabb, mint OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="f180c-214">Oracle JDK has more classes and some fixed bugs, and Oracle JDK is more stable than OpenJDK.</span></span>

#### <a name="install-openjdk"></a><span data-ttu-id="f180c-215">OpenJDK telepítése</span><span class="sxs-lookup"><span data-stu-id="f180c-215">Install OpenJDK</span></span>  

<span data-ttu-id="f180c-216">A következő parancs toodownload OpenJDK hello használata.</span><span class="sxs-lookup"><span data-stu-id="f180c-216">Use hello following command toodownload OpenJDK.</span></span>   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* <span data-ttu-id="f180c-217">a könyvtár toocontain toocreate hello JDK-fájlok:</span><span class="sxs-lookup"><span data-stu-id="f180c-217">toocreate a directory toocontain hello JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="f180c-218">tooextract hello JDK fájlok hello/usr/lib/jvm/directory:</span><span class="sxs-lookup"><span data-stu-id="f180c-218">tooextract hello JDK files into hello /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a><span data-ttu-id="f180c-219">Oracle JDK telepítése</span><span class="sxs-lookup"><span data-stu-id="f180c-219">Install Oracle JDK</span></span>


<span data-ttu-id="f180c-220">Parancs toodownload Oracle JDK következő hello Oracle webhelyről hello használata.</span><span class="sxs-lookup"><span data-stu-id="f180c-220">Use hello following command toodownload Oracle JDK from hello Oracle website.</span></span>  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* <span data-ttu-id="f180c-221">a könyvtár toocontain toocreate hello JDK-fájlok:</span><span class="sxs-lookup"><span data-stu-id="f180c-221">toocreate a directory toocontain hello JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="f180c-222">tooextract hello JDK fájlok hello/usr/lib/jvm/directory:</span><span class="sxs-lookup"><span data-stu-id="f180c-222">tooextract hello JDK files into hello /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* <span data-ttu-id="f180c-223">Oracle JDK, alapértelmezett Java virtuális gép hello tooset:</span><span class="sxs-lookup"><span data-stu-id="f180c-223">tooset Oracle JDK as hello default Java virtual machine:</span></span>  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a><span data-ttu-id="f180c-224">Győződjön meg arról, hogy Java telepítése sikeres</span><span class="sxs-lookup"><span data-stu-id="f180c-224">Confirm that Java installation is successful</span></span>
<span data-ttu-id="f180c-225">Ha hello Java-futtatókörnyezet helyesen van-e telepítve a következő tootest hello hasonló parancsot is használhatja:</span><span class="sxs-lookup"><span data-stu-id="f180c-225">You can use a command like hello following tootest if hello Java runtime environment is installed correctly:</span></span>  

    java -version  

<span data-ttu-id="f180c-226">Ha OpenJDK telepítette, megjelenik egy üzenet hasonló hello: ![sikeres OpenJDK telepítési üzenet][14]</span><span class="sxs-lookup"><span data-stu-id="f180c-226">If you installed OpenJDK, you should see a message like hello following: ![Successful OpenJDK installation message][14]</span></span>

<span data-ttu-id="f180c-227">Ha Oracle JDK telepítette, megjelenik egy üzenet hasonló hello: ![sikeres Oracle JDK telepítés üzenet][15]</span><span class="sxs-lookup"><span data-stu-id="f180c-227">If you installed Oracle JDK, you should see a message like hello following: ![Successful Oracle JDK installation message][15]</span></span>

### <a name="install-tomcat7"></a><span data-ttu-id="f180c-228">Tomcat7 telepítése</span><span class="sxs-lookup"><span data-stu-id="f180c-228">Install Tomcat7</span></span>
<span data-ttu-id="f180c-229">A következő parancs tooinstall Tomcat7 hello használata.</span><span class="sxs-lookup"><span data-stu-id="f180c-229">Use hello following command tooinstall Tomcat7.</span></span>  

    sudo apt-get install tomcat7  

<span data-ttu-id="f180c-230">Ha nem használ Tomcat7, használja a hello megfelelő változatát, a parancs.</span><span class="sxs-lookup"><span data-stu-id="f180c-230">If you are not using Tomcat7, use hello appropriate variation of this command.</span></span>  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a><span data-ttu-id="f180c-231">Győződjön meg arról, hogy Tomcat7 telepítése sikeres</span><span class="sxs-lookup"><span data-stu-id="f180c-231">Confirm that Tomcat7 installation is successful</span></span>
<span data-ttu-id="f180c-232">toocheck Ha Tomcat7 telepítése sikerült, keresse meg a tooyour Tomcat kiszolgáló DNS-neve.</span><span class="sxs-lookup"><span data-stu-id="f180c-232">toocheck if Tomcat7 is successfully installed, browse tooyour Tomcat server’s DNS name.</span></span> <span data-ttu-id="f180c-233">Ebben a cikkben a hello példa URL-címe: http://tomcatexample.cloudapp.net/.</span><span class="sxs-lookup"><span data-stu-id="f180c-233">In this article, hello example URL is http://tomcatexample.cloudapp.net/.</span></span> <span data-ttu-id="f180c-234">Ha megjelenik egy üzenet hello hasonló, Tomcat7 megfelelően van telepítve.</span><span class="sxs-lookup"><span data-stu-id="f180c-234">If you see a message like hello following, Tomcat7 is installed correctly.</span></span>
<span data-ttu-id="f180c-235">![Sikeres Tomcat7 telepítése üzenet][16]</span><span class="sxs-lookup"><span data-stu-id="f180c-235">![Successful Tomcat7 installation message][16]</span></span>

### <a name="install-other-tomcat7-components"></a><span data-ttu-id="f180c-236">Más Tomcat7 összetevőinek telepítése</span><span class="sxs-lookup"><span data-stu-id="f180c-236">Install other Tomcat7 components</span></span>
<span data-ttu-id="f180c-237">Nincsenek más Tomcat összetevőket is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f180c-237">There are other optional Tomcat components that you can install.</span></span>  

<span data-ttu-id="f180c-238">Használjon hello **sudo apt-gyorsítótár keresési tomcat7** parancs toosee hello elérhető összetevőket.</span><span class="sxs-lookup"><span data-stu-id="f180c-238">Use hello **sudo apt-cache search tomcat7** command toosee all of hello available components.</span></span> <span data-ttu-id="f180c-239">A következő parancsok tooinstall hello néhány hasznos összetevőket használnak.</span><span class="sxs-lookup"><span data-stu-id="f180c-239">Use hello following commands tooinstall some useful components.</span></span>  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools toocreate user instances  

## <a name="phase-4-configure-tomcat7"></a><span data-ttu-id="f180c-240">4. fázis: A Tomcat7 konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f180c-240">Phase 4: Configure Tomcat7</span></span>
<span data-ttu-id="f180c-241">Ebben a fázisban a Tomcat felügyeletéhez.</span><span class="sxs-lookup"><span data-stu-id="f180c-241">In this phase, you administer Tomcat.</span></span>

### <a name="start-and-stop-tomcat7"></a><span data-ttu-id="f180c-242">Elindítása és leállítása Tomcat7</span><span class="sxs-lookup"><span data-stu-id="f180c-242">Start and stop Tomcat7</span></span>
<span data-ttu-id="f180c-243">hello Tomcat7 kiszolgáló automatikusan elindul, amikor a telepítés.</span><span class="sxs-lookup"><span data-stu-id="f180c-243">hello Tomcat7 server automatically starts when you install it.</span></span> <span data-ttu-id="f180c-244">Is elindíthatja a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f180c-244">You can also start it with hello following command:</span></span>   

    sudo /etc/init.d/tomcat7 start

<span data-ttu-id="f180c-245">toostop Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="f180c-245">toostop Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 stop

<span data-ttu-id="f180c-246">Tomcat7 tooview hello állapota:</span><span class="sxs-lookup"><span data-stu-id="f180c-246">tooview hello status of Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 status

<span data-ttu-id="f180c-247">toorestart Tomcat szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="f180c-247">toorestart Tomcat services:</span></span> 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a><span data-ttu-id="f180c-248">Tomcat7 felügyeleti</span><span class="sxs-lookup"><span data-stu-id="f180c-248">Tomcat7 administration</span></span>
<span data-ttu-id="f180c-249">Szerkesztheti a hello Tomcat felhasználói konfigurációs fájl tooset be a rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="f180c-249">You can edit hello Tomcat user configuration file tooset up your admin credentials.</span></span> <span data-ttu-id="f180c-250">A következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="f180c-250">Use hello following command:</span></span>  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

<span data-ttu-id="f180c-251">Például:</span><span class="sxs-lookup"><span data-stu-id="f180c-251">Here is an example:</span></span>  
![Képernyőkép a hello sudo vi parancs kimenete][17]  

> [!NOTE]
> <span data-ttu-id="f180c-253">Hozzon létre egy erős jelszót hello rendszergazda felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="f180c-253">Create a strong password for hello admin username.</span></span>  

<span data-ttu-id="f180c-254">Ez a fájl szerkesztése után újra kell indítania Tomcat7 szolgáltatások rendelkező hello a következő parancs tooensure hello módosítások érvénybe:</span><span class="sxs-lookup"><span data-stu-id="f180c-254">After editing this file, you should restart Tomcat7 services with hello following command tooensure that hello changes take effect:</span></span>  

    sudo /etc/init.d/tomcat7 restart  

<span data-ttu-id="f180c-255">Nyissa meg a böngészőt, és írja be **http://<your tomcat server DNS name>/kezelő/html** , hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f180c-255">Open your browser, and enter **http://<your tomcat server DNS name>/manager/html** as hello URL.</span></span> <span data-ttu-id="f180c-256">Ebben a cikkben hello például hello URL-cím http://tomcatexample.cloudapp.net/manager/html.</span><span class="sxs-lookup"><span data-stu-id="f180c-256">For hello example in this article, hello URL is http://tomcatexample.cloudapp.net/manager/html.</span></span>  

<span data-ttu-id="f180c-257">A csatlakozás után valami hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="f180c-257">After connecting, you should see something similar toohello following:</span></span>  
![Képernyőfelvétel a hello Tomcat webalkalmazás-kezelő][18]

## <a name="common-issues"></a><span data-ttu-id="f180c-259">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="f180c-259">Common issues</span></span>
### <a name="cant-access-hello-virtual-machine-with-tomcat-and-moodle-from-hello-internet"></a><span data-ttu-id="f180c-260">Nem érhető el virtuális gép hello a Moodle és Tomcat hello Internet</span><span class="sxs-lookup"><span data-stu-id="f180c-260">Can't access hello virtual machine with Tomcat and Moodle from hello Internet</span></span>
#### <a name="symptom"></a><span data-ttu-id="f180c-261">Jelenség</span><span class="sxs-lookup"><span data-stu-id="f180c-261">Symptom</span></span>  
  <span data-ttu-id="f180c-262">Tomcat fut, de a böngésző hello Tomcat alapértelmezett lap nem látható.</span><span class="sxs-lookup"><span data-stu-id="f180c-262">Tomcat is running but you can’t see hello Tomcat default page with your browser.</span></span>
#### <a name="possible-root-cause"></a><span data-ttu-id="f180c-263">Probléma lehetséges kiváltó okai</span><span class="sxs-lookup"><span data-stu-id="f180c-263">Possible root cause</span></span>   

  * <span data-ttu-id="f180c-264">hello Tomcat figyelési portja nem hello ugyanaz, mint a Tomcat-forgalmat a virtuális gép végpontjának hello a magánhálózati port.</span><span class="sxs-lookup"><span data-stu-id="f180c-264">hello Tomcat listen port is not hello same as hello private port of your virtual machine's endpoint for Tomcat traffic.</span></span>  

     <span data-ttu-id="f180c-265">Ellenőrizze a nyilvános port és a titkos végpont portbeállításokat, és győződjön meg arról, hogy hello magánhálózati port van hello megegyeznek a hello Tomcat port figyelésére.</span><span class="sxs-lookup"><span data-stu-id="f180c-265">Check your public port and private port endpoint settings and make sure hello private port is hello same as hello Tomcat listen port.</span></span> <span data-ttu-id="f180c-266">Lásd: "1. fázis: lemezkép létrehozása" című szakaszát, a virtuális gép végpontjai konfigurálásával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f180c-266">See "Phase 1: Create an image" section of this article for instructions on configuring endpoints for your virtual machine.</span></span>  

     <span data-ttu-id="f180c-267">toodetermine hello Tomcat port figyelésére, nyissa meg a /etc/httpd/conf/httpd.conf (Red Hat kiadás), vagy /etc/tomcat7/server.xml (Debian kiadás).</span><span class="sxs-lookup"><span data-stu-id="f180c-267">toodetermine hello Tomcat listen port, open /etc/httpd/conf/httpd.conf (Red Hat release), or /etc/tomcat7/server.xml (Debian release).</span></span> <span data-ttu-id="f180c-268">Alapértelmezés szerint a Tomcat figyelési portja hello a 8080-as.</span><span class="sxs-lookup"><span data-stu-id="f180c-268">By default, hello Tomcat listen port is 8080.</span></span> <span data-ttu-id="f180c-269">Például:</span><span class="sxs-lookup"><span data-stu-id="f180c-269">Here is an example:</span></span>  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     <span data-ttu-id="f180c-270">Ha például a Debian és Ubuntu és azt szeretné, toochange hello alapértelmezett port a Tomcat figyelésére (például 8081) használ a virtuális gép, is meg kell nyitnia hello portot hello operációs rendszerhez.</span><span class="sxs-lookup"><span data-stu-id="f180c-270">If you are using a virtual machine like Debian or Ubuntu and you want toochange hello default port of Tomcat Listen (for example 8081), you should also open hello port for hello operating system.</span></span> <span data-ttu-id="f180c-271">Első lépésként nyissa meg a hello-profil:</span><span class="sxs-lookup"><span data-stu-id="f180c-271">First, open hello profile:</span></span>  

        sudo vi /etc/default/tomcat7  

     <span data-ttu-id="f180c-272">Ezután állítsa vissza a hello utolsó sort, és módosítsa "nem" túl "yes".</span><span class="sxs-lookup"><span data-stu-id="f180c-272">Then uncomment hello last line and change “no” too“yes”.</span></span>  

        AUTHBIND=yes
  2. <span data-ttu-id="f180c-273">hello tűzfal hello figyelési port a Tomcat letiltotta.</span><span class="sxs-lookup"><span data-stu-id="f180c-273">hello firewall has disabled hello listen port of Tomcat.</span></span>

     <span data-ttu-id="f180c-274">Hogy hello Tomcat alapértelmezett oldal hello helyi állomásról.</span><span class="sxs-lookup"><span data-stu-id="f180c-274">You can only see hello Tomcat default page from hello local host.</span></span> <span data-ttu-id="f180c-275">hello probléma a legvalószínűbb, hogy hello portot, amely figyelt tooby Tomcat, hello tűzfal által blokkolva van.</span><span class="sxs-lookup"><span data-stu-id="f180c-275">hello problem is most likely that hello port, which is listened tooby Tomcat, is blocked by hello firewall.</span></span> <span data-ttu-id="f180c-276">Hello w3m eszköz toobrowse hello weblap használható.</span><span class="sxs-lookup"><span data-stu-id="f180c-276">You can use hello w3m tool toobrowse hello webpage.</span></span> <span data-ttu-id="f180c-277">hello következő parancsok w3m telepítse, és keresse meg toohello Tomcat alapértelmezett oldal:</span><span class="sxs-lookup"><span data-stu-id="f180c-277">hello following commands install w3m and browse toohello Tomcat default page:</span></span>  


        <span data-ttu-id="f180c-278">sudo yum telepítés w3m w3m-img</span><span class="sxs-lookup"><span data-stu-id="f180c-278">sudo yum  install w3m w3m-img</span></span>


        <span data-ttu-id="f180c-279">w3m 8080</span><span class="sxs-lookup"><span data-stu-id="f180c-279">w3m http://localhost:8080</span></span>  
#### <a name="solution"></a><span data-ttu-id="f180c-280">Megoldás</span><span class="sxs-lookup"><span data-stu-id="f180c-280">Solution</span></span>

  * <span data-ttu-id="f180c-281">Hello Tomcat figyelési portja van nem hello ugyanaz, mint hello magánhálózati port hello végpont forgalom toohello virtuális gép, ha módosítania kell a magánhálózati port hello toobe hello megegyeznek a Tomcat figyelési portja hello.</span><span class="sxs-lookup"><span data-stu-id="f180c-281">If hello Tomcat listen port is not hello same as hello private port of hello endpoint for traffic toohello virtual machine, you need change hello private port toobe hello same as hello Tomcat listen port.</span></span>   
  2. <span data-ttu-id="f180c-282">Hello tűzfal/iptables okozza, ha adja hozzá a következő sorokat túl/etc/sysconfig/iptables hello.</span><span class="sxs-lookup"><span data-stu-id="f180c-282">If hello issue is caused by firewall/iptables, add hello following lines too/etc/sysconfig/iptables.</span></span> <span data-ttu-id="f180c-283">hello második sor csak akkor szükséges, a https-forgalmat:</span><span class="sxs-lookup"><span data-stu-id="f180c-283">hello second line is only needed for https traffic:</span></span>  

      <span data-ttu-id="f180c-284">-A -p tcp -m tcp--dport 80 – j ELFOGADÁS bemeneti</span><span class="sxs-lookup"><span data-stu-id="f180c-284">-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT</span></span>

      <span data-ttu-id="f180c-285">-A -p tcp -m tcp--dport 443 -j ELFOGADÁS bemeneti</span><span class="sxs-lookup"><span data-stu-id="f180c-285">-A INPUT -p tcp -m tcp --dport 443 -j ACCEPT</span></span>  

     > [!IMPORTANT]
     > <span data-ttu-id="f180c-286">Ellenőrizze, hogy hello előző sorok legyenek elhelyezve fent, amelyek globálisan szeretné korlátozni a hozzáférést, például hello következő sorokat: - A bemeneti -j ELUTASÍTÁS – elutasítás-az icmp-állomás által tiltott</span><span class="sxs-lookup"><span data-stu-id="f180c-286">Make sure hello previous lines are positioned above any lines that would globally restrict access, such as hello following: -A INPUT -j REJECT --reject-with icmp-host-prohibited</span></span>



<span data-ttu-id="f180c-287">tooreload hello iptables, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f180c-287">tooreload hello iptables, run hello following command:</span></span>

    service iptables restart

<span data-ttu-id="f180c-288">Ez a CentOS 6.3 tesztelték.</span><span class="sxs-lookup"><span data-stu-id="f180c-288">This has been tested on CentOS 6.3.</span></span>

### <a name="permission-denied-when-you-upload-project-files-toovarlibtomcat7webapps"></a><span data-ttu-id="f180c-289">Engedély megtagadva a projekt feltöltésekor fájlok túl/var/lib/tomcat7/webapps /</span><span class="sxs-lookup"><span data-stu-id="f180c-289">Permission denied when you upload project files too/var/lib/tomcat7/webapps/</span></span>
#### <a name="symptom"></a><span data-ttu-id="f180c-290">Jelenség</span><span class="sxs-lookup"><span data-stu-id="f180c-290">Symptom</span></span>
  <span data-ttu-id="f180c-291">Ha egy SFTP ügyfélről (például FileZilla) tooconnect tooyour virtuális gép, és keresse meg a túl/var/lib/tomcat7/webapps/toopublish webhelyét, kap egy hiba üzenet hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="f180c-291">When you use an SFTP client (such as FileZilla) tooconnect tooyour virtual machine and navigate too/var/lib/tomcat7/webapps/ toopublish your site, you get an error message similar toohello following:</span></span>  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a><span data-ttu-id="f180c-292">Probléma lehetséges kiváltó okai</span><span class="sxs-lookup"><span data-stu-id="f180c-292">Possible root cause</span></span>
  <span data-ttu-id="f180c-293">Nincs engedélyek tooaccess hello /var/lib/tomcat7/webapps mappa rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f180c-293">You have no permissions tooaccess hello /var/lib/tomcat7/webapps folder.</span></span>  
#### <a name="solution"></a><span data-ttu-id="f180c-294">Megoldás</span><span class="sxs-lookup"><span data-stu-id="f180c-294">Solution</span></span>  
  <span data-ttu-id="f180c-295">A rendszergazdafiók hello tooget engedéllyel kell rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="f180c-295">You need tooget permission from hello root account.</span></span> <span data-ttu-id="f180c-296">Legfelső szintű toohello felhasználónév hello gép létesített használt hello tulajdonjogát, hogy a mappa módosítható.</span><span class="sxs-lookup"><span data-stu-id="f180c-296">You can change hello ownership of that folder from root toohello username you used when you provisioned hello machine.</span></span> <span data-ttu-id="f180c-297">Íme egy példa a hello azureuser fióknévvel:</span><span class="sxs-lookup"><span data-stu-id="f180c-297">Here is an example with hello azureuser account name:</span></span>  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  <span data-ttu-id="f180c-298">Hello -R beállítás tooapply hello engedélyeket használ az összes fájl egy könyvtár belül túl.</span><span class="sxs-lookup"><span data-stu-id="f180c-298">Use hello -R option tooapply hello permissions for all files inside of a directory too.</span></span>  

  <span data-ttu-id="f180c-299">Ez a parancs könyvtárak is működik.</span><span class="sxs-lookup"><span data-stu-id="f180c-299">This command also works for directories.</span></span> <span data-ttu-id="f180c-300">hello -R beállítás módosítások hello engedélyeit a fájlok és könyvtárak hello könyvtárán belül.</span><span class="sxs-lookup"><span data-stu-id="f180c-300">hello -R option changes hello permissions for all files and directories inside hello directory.</span></span> <span data-ttu-id="f180c-301">Például:</span><span class="sxs-lookup"><span data-stu-id="f180c-301">Here is an example:</span></span>  

     sudo chown -R username:group directory  

  <span data-ttu-id="f180c-302">Ez a parancs (felhasználó és csoport) változik a fájlok és könyvtárak, amelyek hello könyvtárán belül.</span><span class="sxs-lookup"><span data-stu-id="f180c-302">This command changes ownership (both user and group) for all files and directories that are inside hello directory.</span></span>  

  <span data-ttu-id="f180c-303">hello következő parancs csak akkor változik meg hello mappa directory hello engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="f180c-303">hello following command only changes hello permission of hello folder directory.</span></span> <span data-ttu-id="f180c-304">hello fájlok és mappák hello könyvtárán belül nem változnak.</span><span class="sxs-lookup"><span data-stu-id="f180c-304">hello files and folders inside hello directory are not changed.</span></span>  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
