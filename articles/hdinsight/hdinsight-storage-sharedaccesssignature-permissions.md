---
title: "Megosztott hozzáférési aláírásokkal - Azure HDInsight aaaRestrict hozzáférés |} Microsoft Docs"
description: "Ismerje meg, hogyan férnek hozzá a toouse megosztott hozzáférési aláírásokkal toorestrict HDInsight az Azure storage blobs szolgáltatásában tárolja toodata."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: a34a2f8e52e47a15b09f09bc1fc67fc6159ec75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-shared-access-signatures-toorestrict-access-toodata-in-hdinsight"></a><span data-ttu-id="b4e81-103">Azure Storage megosztott hozzáférési aláírásokkal toorestrict hozzáférés toodata használata a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="b4e81-103">Use Azure Storage Shared Access Signatures toorestrict access toodata in HDInsight</span></span>

<span data-ttu-id="b4e81-104">HDInsight hello Azure Storage-fiókok hello-fürthöz tartozó teljes körű hozzáférési toodata van.</span><span class="sxs-lookup"><span data-stu-id="b4e81-104">HDInsight has full access toodata in hello Azure Storage accounts associated with hello cluster.</span></span> <span data-ttu-id="b4e81-105">Megosztott hozzáférési aláírásokkal hello blob tároló toorestrict access toohello adatokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b4e81-105">You can use Shared Access Signatures on hello blob container toorestrict access toohello data.</span></span> <span data-ttu-id="b4e81-106">Például tooprovide csak olvasási hozzáféréssel toohello adatok.</span><span class="sxs-lookup"><span data-stu-id="b4e81-106">For example, tooprovide read-only access toohello data.</span></span> <span data-ttu-id="b4e81-107">Megosztott hozzáférési aláírásokkal (SAS) az Azure storage-fiókok egy szolgáltatása, amely lehetővé teszi toolimit hozzáférés toodata.</span><span class="sxs-lookup"><span data-stu-id="b4e81-107">Shared Access Signatures (SAS) are a feature of Azure storage accounts that allows you toolimit access toodata.</span></span> <span data-ttu-id="b4e81-108">Például a csak olvasási hozzáféréssel toodata biztosítása.</span><span class="sxs-lookup"><span data-stu-id="b4e81-108">For example, providing read-only access toodata.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b4e81-109">Apache Pletyka használó megoldás érdemes a HDInsight-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="b4e81-109">For a solution using Apache Ranger, consider using domain-joined HDInsight.</span></span> <span data-ttu-id="b4e81-110">További információkért lásd: hello [konfigurálása tartományhoz csatlakoztatott HDInsight](hdinsight-domain-joined-configure.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b4e81-110">For more information, see hello [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) document.</span></span>

> [!WARNING]
> <span data-ttu-id="b4e81-111">HDInsight teljes körű hozzáférési toohello alapértelmezett hello fürt tárolóhelyét kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="b4e81-111">HDInsight must have full access toohello default storage for hello cluster.</span></span>

## <a name="requirements"></a><span data-ttu-id="b4e81-112">Követelmények</span><span class="sxs-lookup"><span data-stu-id="b4e81-112">Requirements</span></span>

* <span data-ttu-id="b4e81-113">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="b4e81-113">An Azure subscription</span></span>
* <span data-ttu-id="b4e81-114">C# vagy Python.</span><span class="sxs-lookup"><span data-stu-id="b4e81-114">C# or Python.</span></span> <span data-ttu-id="b4e81-115">C#-példakód a Visual Studio megoldás valósul meg.</span><span class="sxs-lookup"><span data-stu-id="b4e81-115">C# example code is provided as a Visual Studio solution.</span></span>

  * <span data-ttu-id="b4e81-116">A Visual Studio 2013, 2015-öt vagy 2017 verziót kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4e81-116">Visual Studio must be version 2013, 2015, or 2017</span></span>
  * <span data-ttu-id="b4e81-117">Python 2.7 vagy újabb verzióját kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4e81-117">Python must be version 2.7 or higher</span></span>

* <span data-ttu-id="b4e81-118">A Linux-alapú HDInsight-fürt vagy [Azure PowerShell] [ powershell] – Ha egy meglévő Linux-alapú fürtöt, használhatja az Ambari tooadd egy közös hozzáférésű Jogosultságkód toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="b4e81-118">A Linux-based HDInsight cluster OR [Azure PowerShell][powershell] - If you have an existing Linux-based cluster, you can use Ambari tooadd a Shared Access Signature toohello cluster.</span></span> <span data-ttu-id="b4e81-119">Ha nem, akkor az Azure PowerShell toocreate egy fürt használja, és egy közös hozzáférésű Jogosultságkód hozzáadása a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="b4e81-119">If not, you can use Azure PowerShell toocreate a cluster and add a Shared Access Signature during cluster creation.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b4e81-120">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="b4e81-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b4e81-121">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b4e81-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="b4e81-122">Példa fájlok hello [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="b4e81-122">hello example files from [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span></span> <span data-ttu-id="b4e81-123">Ebben a tárházban hello a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b4e81-123">This repository contains hello following items:</span></span>

  * <span data-ttu-id="b4e81-124">Egy tároló, a tárolt házirend, és a SAS hozhat létre, és a HDInsight együttes használata a Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="b4e81-124">A Visual Studio project that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="b4e81-125">Egy olyan tároló, a tárolt házirend és a SAS hozhat létre, és a HDInsight együttes használata a Python-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b4e81-125">A Python script that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="b4e81-126">Egy PowerShell-parancsfájlt, amely hozhat létre a HDInsight fürt és toouse hello SAS konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="b4e81-126">A PowerShell script that can create a HDInsight cluster and configure it toouse hello SAS.</span></span>

## <a name="shared-access-signatures"></a><span data-ttu-id="b4e81-127">Megosztott hozzáférési aláírásokkal</span><span class="sxs-lookup"><span data-stu-id="b4e81-127">Shared Access Signatures</span></span>

<span data-ttu-id="b4e81-128">Nincsenek megosztott hozzáférési aláírásokkal kétféle:</span><span class="sxs-lookup"><span data-stu-id="b4e81-128">There are two forms of Shared Access Signatures:</span></span>

* <span data-ttu-id="b4e81-129">Az ad hoc: hello start idő, a lejárat időpontjának és a SAS összes adhatók meg hello SAS URI hello engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="b4e81-129">Ad hoc: hello start time, expiry time, and permissions for hello SAS are all specified on hello SAS URI.</span></span>

* <span data-ttu-id="b4e81-130">Hozzáférési házirendben tárolt: A tárolt házirend erőforrás tárolóba, mint a blobtárolót van definiálva.</span><span class="sxs-lookup"><span data-stu-id="b4e81-130">Stored access policy: A stored access policy is defined on a resource container, such as a blob container.</span></span> <span data-ttu-id="b4e81-131">A házirend legalább egy közös hozzáférésű jogosultságkód használt toomanage megkötéseit lehet.</span><span class="sxs-lookup"><span data-stu-id="b4e81-131">A policy can be used toomanage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="b4e81-132">Ha SAS-kód társítja a tárolt házirend, hello SAS hello korlátozásokat örököl - hello start idő, a lejárati idő és a tárolt hello hozzáférési házirend definiált - engedélyek.</span><span class="sxs-lookup"><span data-stu-id="b4e81-132">When you associate a SAS with a stored access policy, hello SAS inherits hello constraints - hello start time, expiry time, and permissions - defined for hello stored access policy.</span></span>

<span data-ttu-id="b4e81-133">hello hello két űrlapok közötti különbség fontos egyik-forgatókönyvben: visszavont tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="b4e81-133">hello difference between hello two forms is important for one key scenario: revocation.</span></span> <span data-ttu-id="b4e81-134">SAS-kód egy URL-címet, így bárki, aki jut hozzá a biztonsági Társítások hello használható, függetlenül attól, aki kérte, hogy a toobegin.</span><span class="sxs-lookup"><span data-stu-id="b4e81-134">A SAS is a URL, so anyone who obtains hello SAS can use it, regardless of who requested it toobegin with.</span></span> <span data-ttu-id="b4e81-135">Ha SAS-kód nyilvánosságra, azt bármely hello world használható.</span><span class="sxs-lookup"><span data-stu-id="b4e81-135">If a SAS is published publicly, it can be used by anyone in hello world.</span></span> <span data-ttu-id="b4e81-136">Terjesztett SAS-kód nem érvényes, amíg a négy dolog történik:</span><span class="sxs-lookup"><span data-stu-id="b4e81-136">A SAS that is distributed is valid until one of four things happens:</span></span>

1. <span data-ttu-id="b4e81-137">hello lejárati idejének megadott hello SAS éri el.</span><span class="sxs-lookup"><span data-stu-id="b4e81-137">hello expiry time specified on hello SAS is reached.</span></span>

2. <span data-ttu-id="b4e81-138">hello lejárati idejének megadott hello tárolt házirend SAS elérésekor hello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4e81-138">hello expiry time specified on hello stored access policy referenced by hello SAS is reached.</span></span> <span data-ttu-id="b4e81-139">hello következő forgatókönyvek következtében hello lejárati idejének toobe érhető el:</span><span class="sxs-lookup"><span data-stu-id="b4e81-139">hello following scenarios cause hello expiry time toobe reached:</span></span>

    * <span data-ttu-id="b4e81-140">hello időtartam lejárt.</span><span class="sxs-lookup"><span data-stu-id="b4e81-140">hello time interval has elapsed.</span></span>
    * <span data-ttu-id="b4e81-141">hello tárolt házirend módosított toohave egy korábbi hello lejárati idő.</span><span class="sxs-lookup"><span data-stu-id="b4e81-141">hello stored access policy is modified toohave an expiry time in hello past.</span></span> <span data-ttu-id="b4e81-142">Egyirányú toorevoke hello SAS hello lejárati idejének módosításával.</span><span class="sxs-lookup"><span data-stu-id="b4e81-142">Changing hello expiry time is one way toorevoke hello SAS.</span></span>

3. <span data-ttu-id="b4e81-143">hello hivatkozás által hello SAS törlődik, amely egy másik módja toorevoke hello SAS hozzáférési házirendben tárolt.</span><span class="sxs-lookup"><span data-stu-id="b4e81-143">hello stored access policy referenced by hello SAS is deleted, which is another way toorevoke hello SAS.</span></span> <span data-ttu-id="b4e81-144">Ha azonos nevet, a SAS-tokenje hello tárolt hello hozzáférési házirend hozza létre újra hello előző házirend érvényesek (Ha nem ment hello hello lejárati ideje és a SAS).</span><span class="sxs-lookup"><span data-stu-id="b4e81-144">If you recreate hello stored access policy with hello same name, all  SAS tokens for hello previous policy are valid (if hello expiry time on hello SAS has not passed).</span></span> <span data-ttu-id="b4e81-145">Ha azt tervezi, toorevoke hello SAS, esetén toouse meg arról, hogy egy másik nevet hello hozzáférési házirend a jövőbeli hello egy lejárati idővel hozza létre újra.</span><span class="sxs-lookup"><span data-stu-id="b4e81-145">If you intend toorevoke hello SAS, be sure toouse a different name if you recreate hello access policy with an expiry time in hello future.</span></span>

4. <span data-ttu-id="b4e81-146">hello kulcsára, de a használt toocreate hello SAS újragenerálják.</span><span class="sxs-lookup"><span data-stu-id="b4e81-146">hello account key that was used toocreate hello SAS is regenerated.</span></span> <span data-ttu-id="b4e81-147">Hello kulcsának újragenerálása hatására az összes hello előző kulcs toofail hitelesítést használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b4e81-147">Regenerating hello key causes all applications that use hello previous key toofail authentication.</span></span> <span data-ttu-id="b4e81-148">Az összes összetevő toohello új kulcs frissítése.</span><span class="sxs-lookup"><span data-stu-id="b4e81-148">Update all components toohello new key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b4e81-149">A közös hozzáférésű jogosultságkód URI hello fiók kulcs használt toocreate hello aláírás társítva, és hello tartozó tárolt házirend (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="b4e81-149">A shared access signature URI is associated with hello account key used toocreate hello signature, and hello associated stored access policy (if any).</span></span> <span data-ttu-id="b4e81-150">Ha nincs tárolt házirend van megadva, hello csak úgy toorevoke egy közös hozzáférésű jogosultságkódot toochange hello fiókkulcs.</span><span class="sxs-lookup"><span data-stu-id="b4e81-150">If no stored access policy is specified, hello only way toorevoke a shared access signature is toochange hello account key.</span></span>

<span data-ttu-id="b4e81-151">Javasoljuk, hogy mindig használjon tárolt hozzáférési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="b4e81-151">We recommend that you always use stored access policies.</span></span> <span data-ttu-id="b4e81-152">Tárolt házirendek használatakor aláírások visszavonására, és hello lejárati dátum meghosszabbításához igény szerint.</span><span class="sxs-lookup"><span data-stu-id="b4e81-152">When using stored policies, you can either revoke signatures or extend hello expiry date as needed.</span></span> <span data-ttu-id="b4e81-153">jelen dokumentumban leírt lépések hello tárolt hozzáférési házirendek toogenerate SAS használja.</span><span class="sxs-lookup"><span data-stu-id="b4e81-153">hello steps in this document use stored access policies toogenerate SAS.</span></span>

<span data-ttu-id="b4e81-154">További információ a megosztott hozzáférési aláírásokkal: [ismertetése hello SAS-modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="b4e81-154">For more information on Shared Access Signatures, see [Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

### <a name="create-a-stored-policy-and-sas-using-c"></a><span data-ttu-id="b4e81-155">A tárolt házirend és a C használatával SAS létrehozása\#</span><span class="sxs-lookup"><span data-stu-id="b4e81-155">Create a stored policy and SAS using C\#</span></span>

1. <span data-ttu-id="b4e81-156">Nyissa meg a hello megoldást a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4e81-156">Open hello solution in Visual Studio.</span></span>

2. <span data-ttu-id="b4e81-157">A Megoldáskezelőben kattintson a jobb gombbal a hello **SASToken** projektre, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="b4e81-157">In Solution Explorer, right-click on hello **SASToken** project and select **Properties**.</span></span>

3. <span data-ttu-id="b4e81-158">Válassza ki **beállítások** , és adja hozzá a következő tételek hello értékeit:</span><span class="sxs-lookup"><span data-stu-id="b4e81-158">Select **Settings** and add values for hello following entries:</span></span>

   * <span data-ttu-id="b4e81-159">StorageConnectionString: hello kapcsolati karakterlánc, amelyet az toocreate hello tárfiók a tárolt házirend és az SAS-kód.</span><span class="sxs-lookup"><span data-stu-id="b4e81-159">StorageConnectionString: hello connection string for hello storage account that you want toocreate a stored policy and SAS for.</span></span> <span data-ttu-id="b4e81-160">hello formátumúnak kell lennie `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` ahol `myaccount` hello a tárfiók neve és `mykey` hello hello tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="b4e81-160">hello format should be `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` where `myaccount` is hello name of your storage account and `mykey` is hello key for hello storage account.</span></span>

   * <span data-ttu-id="b4e81-161">ContainerName: hello tároló toorestrict eléréséhez használni kívánt hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="b4e81-161">ContainerName: hello container in hello storage account that you want toorestrict access to.</span></span>

   * <span data-ttu-id="b4e81-162">SASPolicyName: hello neve toouse hello a tárolt házirend toocreate.</span><span class="sxs-lookup"><span data-stu-id="b4e81-162">SASPolicyName: hello name toouse for hello stored policy toocreate.</span></span>

   * <span data-ttu-id="b4e81-163">FileToUpload: hello elérési tooa fájl feltöltött toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="b4e81-163">FileToUpload: hello path tooa file that is uploaded toohello container.</span></span>

4. <span data-ttu-id="b4e81-164">Futtassa a hello projektet.</span><span class="sxs-lookup"><span data-stu-id="b4e81-164">Run hello project.</span></span> <span data-ttu-id="b4e81-165">Információk a következő szöveg hasonló toohello után hello SAS létrejött jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b4e81-165">Information similar toohello following text is displayed once hello SAS has been generated:</span></span>

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="b4e81-166">Mentse a hello SAS-házirend jogkivonat, a tárfiók nevét és a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="b4e81-166">Save hello SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="b4e81-167">Hello storage-fiók társítása a HDInsight-fürt használja ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4e81-167">These values are used when associating hello storage account with your HDInsight cluster.</span></span>

### <a name="create-a-stored-policy-and-sas-using-python"></a><span data-ttu-id="b4e81-168">Hozzon létre egy tárolt házirend és a SAS pythonos környezetekben</span><span class="sxs-lookup"><span data-stu-id="b4e81-168">Create a stored policy and SAS using Python</span></span>

1. <span data-ttu-id="b4e81-169">Nyissa meg a hello SASToken.py fájlt, és módosítsa a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="b4e81-169">Open hello SASToken.py file and change hello following values:</span></span>

   * <span data-ttu-id="b4e81-170">házirend\_name: hello neve toouse hello a tárolt házirend toocreate.</span><span class="sxs-lookup"><span data-stu-id="b4e81-170">policy\_name: hello name toouse for hello stored policy toocreate.</span></span>

   * <span data-ttu-id="b4e81-171">tárolási\_fiók\_name: hello a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="b4e81-171">storage\_account\_name: hello name of your storage account.</span></span>

   * <span data-ttu-id="b4e81-172">tárolási\_fiók\_kulcs: hello hello kulcsának.</span><span class="sxs-lookup"><span data-stu-id="b4e81-172">storage\_account\_key: hello key for hello storage account.</span></span>

   * <span data-ttu-id="b4e81-173">tárolási\_tároló\_name: hello tároló toorestrict eléréséhez használni kívánt hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="b4e81-173">storage\_container\_name: hello container in hello storage account that you want toorestrict access to.</span></span>

   * <span data-ttu-id="b4e81-174">Példa\_fájl\_elérési út: hello elérési tooa fájl feltöltött toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="b4e81-174">example\_file\_path: hello path tooa file that is uploaded toohello container.</span></span>

2. <span data-ttu-id="b4e81-175">Hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="b4e81-175">Run hello script.</span></span> <span data-ttu-id="b4e81-176">Hello SAS-token hasonló toohello hello parancsfájl befejeződésekor a következő szöveg jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="b4e81-176">It displays hello SAS token similar toohello following text when hello script completes:</span></span>

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="b4e81-177">Mentse a hello SAS-házirend jogkivonat, a tárfiók nevét és a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="b4e81-177">Save hello SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="b4e81-178">Hello storage-fiók társítása a HDInsight-fürt használja ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b4e81-178">These values are used when associating hello storage account with your HDInsight cluster.</span></span>

## <a name="use-hello-sas-with-hdinsight"></a><span data-ttu-id="b4e81-179">Hello SAS használata a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="b4e81-179">Use hello SAS with HDInsight</span></span>

<span data-ttu-id="b4e81-180">HDInsight-fürtök létrehozásakor meg kell adnia egy elsődleges tárfiók, és opcionálisan megadhat további tárfiókokat.</span><span class="sxs-lookup"><span data-stu-id="b4e81-180">When creating an HDInsight cluster, you must specify a primary storage account and you can optionally specify additional storage accounts.</span></span> <span data-ttu-id="b4e81-181">Mindkét módszer tárolási hozzáadásának szükséges teljes körű hozzáférési toohello storage-fiókok és a tárolók használt.</span><span class="sxs-lookup"><span data-stu-id="b4e81-181">Both of these methods of adding storage require full access toohello storage accounts and containers that are used.</span></span>

<span data-ttu-id="b4e81-182">egy közös hozzáférésű Jogosultságkód toolimit hozzáférés tooa tároló toouse hozzáadása egy egyéni bejegyzés toohello **core-hely** hello fürt konfigurációjában.</span><span class="sxs-lookup"><span data-stu-id="b4e81-182">toouse a Shared Access Signature toolimit access tooa container, add a custom entry toohello **core-site** configuration for hello cluster.</span></span>

* <span data-ttu-id="b4e81-183">A **Windows-alapú** vagy **Linux-alapú** HDInsight-fürtök, a PowerShell használatával, a fürt létrehozása során hello bejegyzést adhat.</span><span class="sxs-lookup"><span data-stu-id="b4e81-183">For **Windows-based** or **Linux-based** HDInsight clusters, you can add hello entry during cluster creation using PowerShell.</span></span>
* <span data-ttu-id="b4e81-184">A **Linux-alapú** a HDInsight-fürtök Ambari használatával fürt létrehozása után hello konfigurációjának módosítása.</span><span class="sxs-lookup"><span data-stu-id="b4e81-184">For **Linux-based** HDInsight clusters, change hello configuration after cluster creation using Ambari.</span></span>

### <a name="create-a-cluster-that-uses-hello-sas"></a><span data-ttu-id="b4e81-185">Hello SAS használó fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4e81-185">Create a cluster that uses hello SAS</span></span>

<span data-ttu-id="b4e81-186">Például, hogy SAS megtalálható hello használ hello HDInsight fürtök létrehozásával `CreateCluster` hello tárház könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="b4e81-186">An example of creating an HDInsight cluster that uses hello SAS is included in hello `CreateCluster` directory of hello repository.</span></span> <span data-ttu-id="b4e81-187">toouse azt használja hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b4e81-187">toouse it, use hello following steps:</span></span>

1. <span data-ttu-id="b4e81-188">Nyissa meg hello `CreateCluster\HDInsightSAS.ps1` fájlt egy szövegszerkesztőben, és módosítsa a következő értékek hello dokumentum hello elején hello.</span><span class="sxs-lookup"><span data-stu-id="b4e81-188">Open hello `CreateCluster\HDInsightSAS.ps1` file in a text editor and modify hello following values at hello beginning of hello document.</span></span>

    ```powershell
    # Replace 'mycluster' with hello name of hello cluster toobe created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with hello name of hello group toobe created
    $resourceGroupName = 'myresourcegroup'
    # Replace with hello Azure data center you want toohello cluster toolive in
    $location = 'North Europe'
    # Replace with hello name of hello default storage account toobe created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with hello name of hello SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with hello name of hello SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with hello SAS token generated earlier
    $SASToken = 'sastoken'
    # Set hello number of worker nodes in hello cluster
    $clusterSizeInNodes = 3
    ```

    <span data-ttu-id="b4e81-189">Például `'mycluster'` toohello neve hello fürt toocreate szeretné.</span><span class="sxs-lookup"><span data-stu-id="b4e81-189">For example, change `'mycluster'` toohello name of hello cluster you want toocreate.</span></span> <span data-ttu-id="b4e81-190">hello SAS értéket meg kell felelnie hello értékek hello előző lépéseiből, egy tárfiók és a SAS-jogkivonat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b4e81-190">hello SAS values should match hello values from hello previous steps when creating a storage account and SAS token.</span></span>

    <span data-ttu-id="b4e81-191">Miután hello értékek megváltoztak, mentse a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4e81-191">Once you have changed hello values, save hello file.</span></span>

2. <span data-ttu-id="b4e81-192">Nyisson meg egy új Azure PowerShell-parancssorba.</span><span class="sxs-lookup"><span data-stu-id="b4e81-192">Open a new Azure PowerShell prompt.</span></span> <span data-ttu-id="b4e81-193">Ha nem ismeri az Azure PowerShell, vagy nem telepítette azt, lásd: [telepítse és konfigurálja az Azure Powershellt][powershell].</span><span class="sxs-lookup"><span data-stu-id="b4e81-193">If you are unfamiliar with Azure PowerShell, or have not installed it, see [Install and configure Azure PowerShell][powershell].</span></span>

1. <span data-ttu-id="b4e81-194">Hello parancssorába a következő parancs tooauthenticate tooyour Azure-előfizetés hello használata:</span><span class="sxs-lookup"><span data-stu-id="b4e81-194">From hello prompt, use hello following command tooauthenticate tooyour Azure subscription:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="b4e81-195">Amikor a rendszer kéri, jelentkezzen be Azure-előfizetése hello fiók.</span><span class="sxs-lookup"><span data-stu-id="b4e81-195">When prompted, log in with hello account for your Azure subscription.</span></span>

    <span data-ttu-id="b4e81-196">Ha a fiók több Azure-előfizetéssel társítva, szükség lehet a toouse `Select-AzureRmSubscription` tooselect hello előfizetés toouse kívánja.</span><span class="sxs-lookup"><span data-stu-id="b4e81-196">If your account is associated with multiple Azure subscriptions, you may need toouse `Select-AzureRmSubscription` tooselect hello subscription you wish toouse.</span></span>

4. <span data-ttu-id="b4e81-197">Hello parancssorból módosítsa a könyvtárakat toohello `CreateCluster` hello HDInsightSAS.ps1 fájlt tartalmazó könyvtár.</span><span class="sxs-lookup"><span data-stu-id="b4e81-197">From hello prompt, change directories toohello `CreateCluster` directory that contains hello HDInsightSAS.ps1 file.</span></span> <span data-ttu-id="b4e81-198">Ezután használja a következő parancsfájl toorun hello hello</span><span class="sxs-lookup"><span data-stu-id="b4e81-198">Then use hello following command toorun hello script</span></span>

    ```powershell
    .\HDInsightSAS.ps1
    ```

    <span data-ttu-id="b4e81-199">Hello parancsfájlt futtat, mint naplózza kimeneti toohello PowerShell-parancssorba csoport és a storage-fiókokat készít hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b4e81-199">As hello script runs, it logs output toohello PowerShell prompt as it creates hello resource group and storage accounts.</span></span> <span data-ttu-id="b4e81-200">Biztosan felszólító tooenter hello HTTP felhasználó hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="b4e81-200">You are prompted tooenter hello HTTP user for hello HDInsight cluster.</span></span> <span data-ttu-id="b4e81-201">Ez a fiók akkor használt toosecure HTTP/s hozzáférést toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="b4e81-201">This account is used toosecure HTTP/s access toohello cluster.</span></span>

    <span data-ttu-id="b4e81-202">A Linux-alapú fürt létrehozásakor egy SSH felhasználói fiók felhasználónevét és jelszavát kéri.</span><span class="sxs-lookup"><span data-stu-id="b4e81-202">If you are creating a Linux-based cluster, you are prompted for an SSH user account name and password.</span></span> <span data-ttu-id="b4e81-203">Ez a fiók akkor használt tooremotely napló toohello fürtben.</span><span class="sxs-lookup"><span data-stu-id="b4e81-203">This account is used tooremotely log in toohello cluster.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="b4e81-204">Ha a HTTP/s hello vagy SSH-felhasználónév és jelszó megadására kéri, meg kell adnia egy jelszót, amely megfelel a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="b4e81-204">When prompted for hello HTTP/s or SSH user name and password, you must provide a password that meets hello following criteria:</span></span>
   >
   > * <span data-ttu-id="b4e81-205">Legalább 10 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4e81-205">Must be at least 10 characters in length</span></span>
   > * <span data-ttu-id="b4e81-206">Legalább egy számot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="b4e81-206">Must contain at least one digit</span></span>
   > * <span data-ttu-id="b4e81-207">Legalább egy nem alfanumerikus karaktert kell tartalmaznia</span><span class="sxs-lookup"><span data-stu-id="b4e81-207">Must contain at least one non-alphanumeric character</span></span>
   > * <span data-ttu-id="b4e81-208">Tartalmaznia kell legalább egy nagy- vagy kisbetűt</span><span class="sxs-lookup"><span data-stu-id="b4e81-208">Must contain at least one upper or lower case letter</span></span>

<span data-ttu-id="b4e81-209">A parancsfájl toocomplete, míg általában körülbelül 15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b4e81-209">It takes a while for this script toocomplete, usually around 15 minutes.</span></span> <span data-ttu-id="b4e81-210">Amikor hello parancsfájl hiba nélkül befejeződött, hello fürt létrehozását.</span><span class="sxs-lookup"><span data-stu-id="b4e81-210">When hello script completes without any errors, hello cluster has been created.</span></span>

### <a name="use-hello-sas-with-an-existing-cluster"></a><span data-ttu-id="b4e81-211">Meglévő fürt hello SAS használata</span><span class="sxs-lookup"><span data-stu-id="b4e81-211">Use hello SAS with an existing cluster</span></span>

<span data-ttu-id="b4e81-212">Ha egy meglévő Linux-alapú fürtöt, adhat hozzá hello SAS toohello **core-hely** konfigurációs lépések hello segítségével:</span><span class="sxs-lookup"><span data-stu-id="b4e81-212">If you have an existing Linux-based cluster, you can add hello SAS toohello **core-site** configuration by using hello following steps:</span></span>

1. <span data-ttu-id="b4e81-213">Nyissa meg a fürt hello Ambari webes felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="b4e81-213">Open hello Ambari web UI for your cluster.</span></span> <span data-ttu-id="b4e81-214">Ezen a lapon hello címet https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="b4e81-214">hello address for this page is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="b4e81-215">Amikor a rendszer kéri, toohello fürt hello felügyeleti neve (rendszergazda) használatával hitelesíteni és jelszó használatával mikor hello fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4e81-215">When prompted, authenticate toohello cluster using hello admin name (admin) and password you used when creating hello cluster.</span></span>

2. <span data-ttu-id="b4e81-216">Hello bal oldalán található hello Ambari webes felhasználói felület, válassza ki **HDFS** , és válassza a hello **Configs** hello középső hello lap fülre.</span><span class="sxs-lookup"><span data-stu-id="b4e81-216">From hello left side of hello Ambari web UI, select **HDFS** and then select hello **Configs** tab in hello middle of hello page.</span></span>

3. <span data-ttu-id="b4e81-217">Jelölje be hello **speciális** lapot, és görgessen, amíg meg nem látja hello **egyéni core-hely** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4e81-217">Select hello **Advanced** tab, and then scroll until you find hello **Custom core-site** section.</span></span>

4. <span data-ttu-id="b4e81-218">Bontsa ki a hello **egyéni core-hely** területen, majd görgessen toohello end és select hello **tulajdonság hozzáadása... ** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b4e81-218">Expand hello **Custom core-site** section, then scroll toohello end and select hello **Add property...** link.</span></span> <span data-ttu-id="b4e81-219">Hello használata hello következő értékei **kulcs** és **érték** mezők:</span><span class="sxs-lookup"><span data-stu-id="b4e81-219">Use hello following values for hello **Key** and **Value** fields:</span></span>

   * <span data-ttu-id="b4e81-220">**Kulcs**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="b4e81-220">**Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span></span>
   * <span data-ttu-id="b4e81-221">**Érték**: hello futtatta korábban C# vagy Python-alkalmazás által visszaadott SAS hello</span><span class="sxs-lookup"><span data-stu-id="b4e81-221">**Value**: hello SAS returned by hello C# or Python application you ran previously</span></span>

     <span data-ttu-id="b4e81-222">Cserélje le **CONTAINERNAME** hello tároló nevű használt hello C# vagy SAS alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b4e81-222">Replace **CONTAINERNAME** with hello container name you used with hello C# or SAS application.</span></span> <span data-ttu-id="b4e81-223">Cserélje le **STORAGEACCOUNTNAME** a tárfiók neve hello használt.</span><span class="sxs-lookup"><span data-stu-id="b4e81-223">Replace **STORAGEACCOUNTNAME** with hello storage account name you used.</span></span>

5. <span data-ttu-id="b4e81-224">Hello kattintson **Hozzáadás** toosave a kulcs-érték gombra, majd kattintson az hello **mentése** toosave hello konfigurációs módosítások gombra.</span><span class="sxs-lookup"><span data-stu-id="b4e81-224">Click hello **Add** button toosave this key and value, then click hello **Save** button toosave hello configuration changes.</span></span> <span data-ttu-id="b4e81-225">Amikor a rendszer kéri, adjon meg egy leírást ("hozzáadása SAS tárolók eléréséhez" például) hello változás, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b4e81-225">When prompted, add a description of hello change ("adding SAS storage access" for example) and then click **Save**.</span></span>

    <span data-ttu-id="b4e81-226">Kattintson a **OK** amikor hello módosítások elvégzése után.</span><span class="sxs-lookup"><span data-stu-id="b4e81-226">Click **OK** when hello changes have been completed.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="b4e81-227">Hello módosítás érvénybe léptetéséhez újra kell indítania számos szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b4e81-227">You must restart several services before hello change takes effect.</span></span>

6. <span data-ttu-id="b4e81-228">Hello Ambari webes felhasználói felület, válassza ki **HDFS** hello listából hello maradt, és válassza a **indítsa újra az összes** a hello **szolgáltatás műveletek** legördülő listából a megfelelő hello.</span><span class="sxs-lookup"><span data-stu-id="b4e81-228">In hello Ambari web UI, select **HDFS** from hello list on hello left, and then select **Restart All** from hello **Service Actions** drop down list on hello right.</span></span> <span data-ttu-id="b4e81-229">Amikor a rendszer kéri, válassza ki a **kapcsolja be a karbantartási mód** és majd válassza ki __Conform indítsa újra az összes ".</span><span class="sxs-lookup"><span data-stu-id="b4e81-229">When prompted, select **Turn on maintenance mode** and then select __Conform Restart All".</span></span>

    <span data-ttu-id="b4e81-230">Ismételje meg ezt a folyamatot MapReduce2 és YARN.</span><span class="sxs-lookup"><span data-stu-id="b4e81-230">Repeat this process for MapReduce2 and YARN.</span></span>

7. <span data-ttu-id="b4e81-231">Hello szolgáltatás újraindítása, ha mindegyiknél válassza ki, és tiltsa le a karbantartási mód a hello **szolgáltatás műveletek** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="b4e81-231">Once hello services have restarted, select each one and disable maintenance mode from hello **Service Actions** drop down.</span></span>

## <a name="test-restricted-access"></a><span data-ttu-id="b4e81-232">Korlátozott hozzáférés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b4e81-232">Test restricted access</span></span>

<span data-ttu-id="b4e81-233">tooverify, hogy korlátozott hozzáféréssel rendelkező, a következő módszerek használatát hello:</span><span class="sxs-lookup"><span data-stu-id="b4e81-233">tooverify that you have restricted access, use hello following methods:</span></span>

* <span data-ttu-id="b4e81-234">A **Windows-alapú** a HDInsight-fürtök, a távoli asztal tooconnect toohello-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="b4e81-234">For **Windows-based** HDInsight clusters, use Remote Desktop tooconnect toohello cluster.</span></span> <span data-ttu-id="b4e81-235">További információkért lásd: [csatlakozás RDP Funkciót használnak tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="b4e81-235">For more information, see [Connect tooHDInsight using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

    <span data-ttu-id="b4e81-236">Miután csatlakozott, a hello **parancssori Hadoop** hello asztali tooopen egy parancs parancssori futtatásával ikonra.</span><span class="sxs-lookup"><span data-stu-id="b4e81-236">Once connected, use hello **Hadoop Command-Line** icon on hello desktop tooopen a command prompt.</span></span>

* <span data-ttu-id="b4e81-237">A **Linux-alapú** a HDInsight-fürtök SSH tooconnect toohello-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="b4e81-237">For **Linux-based** HDInsight clusters, use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="b4e81-238">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b4e81-238">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="b4e81-239">Miután csatlakozott toohello fürt, használja a következő lépéseket tooverify, hogy a csak olvasási és listában található elemek hello SAS tárfiók is hello:</span><span class="sxs-lookup"><span data-stu-id="b4e81-239">Once connected toohello cluster, use hello following steps tooverify that you can only read and list items on hello SAS storage account:</span></span>

1. <span data-ttu-id="b4e81-240">hello tároló, toolist hello tartalmának parancs hello parancssorból a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="b4e81-240">toolist hello contents of hello container, use hello following command from hello prompt:</span></span> 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    <span data-ttu-id="b4e81-241">Cserélje le **SASCONTAINER** hello nevű hello tároló hello tárfiók SAS létre.</span><span class="sxs-lookup"><span data-stu-id="b4e81-241">Replace **SASCONTAINER** with hello name of hello container created for hello SAS storage account.</span></span> <span data-ttu-id="b4e81-242">Cserélje le **SASACCOUNTNAME** hello nevű hello tárfiók hello SAS használatos.</span><span class="sxs-lookup"><span data-stu-id="b4e81-242">Replace **SASACCOUNTNAME** with hello name of hello storage account used for hello SAS.</span></span>

    <span data-ttu-id="b4e81-243">hello listán hello fájl feltöltése, amikor hello tároló és a SAS hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="b4e81-243">hello list includes hello file uploaded when hello container and SAS were created.</span></span>

2. <span data-ttu-id="b4e81-244">A következő parancs tooverify hello fájl tartalmának hello elolvasni hello használata.</span><span class="sxs-lookup"><span data-stu-id="b4e81-244">Use hello following command tooverify that you can read hello contents of hello file.</span></span> <span data-ttu-id="b4e81-245">Cserélje le a hello **SASCONTAINER** és **SASACCOUNTNAME** ahogy hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="b4e81-245">Replace hello **SASCONTAINER** and **SASACCOUNTNAME** as in hello previous step.</span></span> <span data-ttu-id="b4e81-246">Cserélje le **Fájlnév** hello nevű hello fájl hello előző parancs jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b4e81-246">Replace **FILENAME** with hello name of hello file displayed in hello previous command:</span></span>

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    <span data-ttu-id="b4e81-247">Ez a parancs kilistázza hello hello fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="b4e81-247">This command lists hello contents of hello file.</span></span>

3. <span data-ttu-id="b4e81-248">A következő parancs toodownload hello fájl toohello helyi fájlrendszer hello használata:</span><span class="sxs-lookup"><span data-stu-id="b4e81-248">Use hello following command toodownload hello file toohello local file system:</span></span>

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    <span data-ttu-id="b4e81-249">Ez a parancs letölti a fájl tooa helyi fájl nevű hello **példa.txt**.</span><span class="sxs-lookup"><span data-stu-id="b4e81-249">This command downloads hello file tooa local file named **testfile.txt**.</span></span>

4. <span data-ttu-id="b4e81-250">Használjon hello következő parancsot a tooupload hello helyi fájl tooa új fájlt **testupload.txt** a hello SAS-tárolót:</span><span class="sxs-lookup"><span data-stu-id="b4e81-250">Use hello following command tooupload hello local file tooa new file named **testupload.txt** on hello SAS storage:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    <span data-ttu-id="b4e81-251">Megjelenik egy üzenet hasonló toohello, a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="b4e81-251">You receive a message similar toohello following text:</span></span>

        put: java.io.IOException

    <span data-ttu-id="b4e81-252">Ez a hiba akkor fordul elő, mert hello tárolási helye nem olvasható + lista csak.</span><span class="sxs-lookup"><span data-stu-id="b4e81-252">This error occurs because hello storage location is read+list only.</span></span> <span data-ttu-id="b4e81-253">Parancs tooput hello adatok hello alapértelmezett tároló hello fürt, írható a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="b4e81-253">Use hello following command tooput hello data on hello default storage for hello cluster, which is writable:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    <span data-ttu-id="b4e81-254">Megadott idő hello műveletet kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="b4e81-254">This time, hello operation should complete successfully.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b4e81-255">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="b4e81-255">Troubleshooting</span></span>

### <a name="a-task-was-canceled"></a><span data-ttu-id="b4e81-256">A feladat meg lett szakítva</span><span class="sxs-lookup"><span data-stu-id="b4e81-256">A task was canceled</span></span>

<span data-ttu-id="b4e81-257">**A jelenség**: hello PowerShell-parancsfájlt a fürt létrehozásakor jelenhet meg a következő hibaüzenet hello:</span><span class="sxs-lookup"><span data-stu-id="b4e81-257">**Symptoms**: When creating a cluster using hello PowerShell script, you may receive hello following error message:</span></span>

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

<span data-ttu-id="b4e81-258">**OK**: Ez a hiba akkor fordulhat elő, ha jelszót használhat hello admin/HTTP felhasználói hello fürt, vagy (a Linux-alapú fürtök) hello SSH-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b4e81-258">**Cause**: This error can occur if you use a password for hello admin/HTTP user for hello cluster, or (for Linux-based clusters) hello SSH user.</span></span>

<span data-ttu-id="b4e81-259">**Megoldási**: használhat olyan jelszót, amely megfelel a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="b4e81-259">**Resolution**: Use a password that meets hello following criteria:</span></span>

* <span data-ttu-id="b4e81-260">Legalább 10 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4e81-260">Must be at least 10 characters in length</span></span>
* <span data-ttu-id="b4e81-261">Legalább egy számot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="b4e81-261">Must contain at least one digit</span></span>
* <span data-ttu-id="b4e81-262">Legalább egy nem alfanumerikus karaktert kell tartalmaznia</span><span class="sxs-lookup"><span data-stu-id="b4e81-262">Must contain at least one non-alphanumeric character</span></span>
* <span data-ttu-id="b4e81-263">Tartalmaznia kell legalább egy nagy- vagy kisbetűt</span><span class="sxs-lookup"><span data-stu-id="b4e81-263">Must contain at least one upper or lower case letter</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4e81-264">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4e81-264">Next steps</span></span>

<span data-ttu-id="b4e81-265">Most, hogy megtanulta, hogyan tooadd korlátozott hozzáférésű tárolási tooyour HDInsight-fürtjéhez, ismerje meg, más módokon toowork a fürtön lévő adatokkal:</span><span class="sxs-lookup"><span data-stu-id="b4e81-265">Now that you have learned how tooadd limited-access storage tooyour HDInsight cluster, learn other ways toowork with data on your cluster:</span></span>

* [<span data-ttu-id="b4e81-266">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="b4e81-266">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="b4e81-267">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="b4e81-267">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="b4e81-268">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="b4e81-268">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
