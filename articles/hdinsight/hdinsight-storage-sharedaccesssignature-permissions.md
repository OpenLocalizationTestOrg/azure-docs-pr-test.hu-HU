---
title: "Megosztott hozzáférési aláírásokkal - Azure HDInsight-hozzáférés korlátozása |} Microsoft Docs"
description: "Ismerje meg megosztott hozzáférési aláírásokkal használata a HDInsight-hozzáférés korlátozása az Azure storage blobs szolgáltatásban tárolt adatokat."
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
ms.openlocfilehash: 2e4b1a307fae06c0639d93b9804c6f0f703d5900
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a><span data-ttu-id="e2454-103">Azure Storage megosztott hozzáférési aláírásokkal segítségével adatokat a hdinsight eszközben való hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="e2454-103">Use Azure Storage Shared Access Signatures to restrict access to data in HDInsight</span></span>

<span data-ttu-id="e2454-104">HDInsight a fürthöz tartozó Azure Storage-fiókokat az adatok teljes hozzáféréssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e2454-104">HDInsight has full access to data in the Azure Storage accounts associated with the cluster.</span></span> <span data-ttu-id="e2454-105">A blob tárolóra megosztott hozzáférési aláírásokkal használatával korlátozza a hozzáférést az adatokat.</span><span class="sxs-lookup"><span data-stu-id="e2454-105">You can use Shared Access Signatures on the blob container to restrict access to the data.</span></span> <span data-ttu-id="e2454-106">Például írásvédett hozzáférést biztosít az adatokat.</span><span class="sxs-lookup"><span data-stu-id="e2454-106">For example, to provide read-only access to the data.</span></span> <span data-ttu-id="e2454-107">Megosztott hozzáférési aláírásokkal (SAS) az Azure storage-fiókok egy szolgáltatása, amely lehetővé teszi az adatokhoz való hozzáférés korlátozása.</span><span class="sxs-lookup"><span data-stu-id="e2454-107">Shared Access Signatures (SAS) are a feature of Azure storage accounts that allows you to limit access to data.</span></span> <span data-ttu-id="e2454-108">Például az adatok csak olvasható hozzáférést biztosító.</span><span class="sxs-lookup"><span data-stu-id="e2454-108">For example, providing read-only access to data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2454-109">Apache Pletyka használó megoldás érdemes a HDInsight-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="e2454-109">For a solution using Apache Ranger, consider using domain-joined HDInsight.</span></span> <span data-ttu-id="e2454-110">További információkért lásd: a [konfigurálása tartományhoz csatlakoztatott HDInsight](hdinsight-domain-joined-configure.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="e2454-110">For more information, see the [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) document.</span></span>

> [!WARNING]
> <span data-ttu-id="e2454-111">HDInsight a fürt az alapértelmezett tároló teljes hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="e2454-111">HDInsight must have full access to the default storage for the cluster.</span></span>

## <a name="requirements"></a><span data-ttu-id="e2454-112">Követelmények</span><span class="sxs-lookup"><span data-stu-id="e2454-112">Requirements</span></span>

* <span data-ttu-id="e2454-113">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="e2454-113">An Azure subscription</span></span>
* <span data-ttu-id="e2454-114">C# vagy Python.</span><span class="sxs-lookup"><span data-stu-id="e2454-114">C# or Python.</span></span> <span data-ttu-id="e2454-115">C#-példakód a Visual Studio megoldás valósul meg.</span><span class="sxs-lookup"><span data-stu-id="e2454-115">C# example code is provided as a Visual Studio solution.</span></span>

  * <span data-ttu-id="e2454-116">A Visual Studio 2013, 2015-öt vagy 2017 verziót kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2454-116">Visual Studio must be version 2013, 2015, or 2017</span></span>
  * <span data-ttu-id="e2454-117">Python 2.7 vagy újabb verzióját kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2454-117">Python must be version 2.7 or higher</span></span>

* <span data-ttu-id="e2454-118">A Linux-alapú HDInsight-fürt vagy [Azure PowerShell] [ powershell] – Ha egy meglévő Linux-alapú fürtöt, Ambari egy közös hozzáférésű Jogosultságkód hozzáadása a fürt használhatja.</span><span class="sxs-lookup"><span data-stu-id="e2454-118">A Linux-based HDInsight cluster OR [Azure PowerShell][powershell] - If you have an existing Linux-based cluster, you can use Ambari to add a Shared Access Signature to the cluster.</span></span> <span data-ttu-id="e2454-119">Ha nem, az Azure PowerShell segítségével hozzon létre egy fürtöt, és egy közös hozzáférésű Jogosultságkód hozzáadása a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="e2454-119">If not, you can use Azure PowerShell to create a cluster and add a Shared Access Signature during cluster creation.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e2454-120">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="e2454-120">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e2454-121">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e2454-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="e2454-122">A példa fájljainak [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="e2454-122">The example files from [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span></span> <span data-ttu-id="e2454-123">A tárház a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e2454-123">This repository contains the following items:</span></span>

  * <span data-ttu-id="e2454-124">Egy tároló, a tárolt házirend, és a SAS hozhat létre, és a HDInsight együttes használata a Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="e2454-124">A Visual Studio project that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="e2454-125">Egy olyan tároló, a tárolt házirend és a SAS hozhat létre, és a HDInsight együttes használata a Python-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="e2454-125">A Python script that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="e2454-126">Egy PowerShell-parancsfájlt, amely létre HDInsight-fürtöt és konfigurálja úgy, hogy az SA-kat használjon.</span><span class="sxs-lookup"><span data-stu-id="e2454-126">A PowerShell script that can create a HDInsight cluster and configure it to use the SAS.</span></span>

## <a name="shared-access-signatures"></a><span data-ttu-id="e2454-127">Megosztott hozzáférési aláírásokkal</span><span class="sxs-lookup"><span data-stu-id="e2454-127">Shared Access Signatures</span></span>

<span data-ttu-id="e2454-128">Nincsenek megosztott hozzáférési aláírásokkal kétféle:</span><span class="sxs-lookup"><span data-stu-id="e2454-128">There are two forms of Shared Access Signatures:</span></span>

* <span data-ttu-id="e2454-129">Az ad hoc: A kezdési ideje, a lejárati idő és a SAS engedélyeinek összes adhatók meg a SAS URI-t.</span><span class="sxs-lookup"><span data-stu-id="e2454-129">Ad hoc: The start time, expiry time, and permissions for the SAS are all specified on the SAS URI.</span></span>

* <span data-ttu-id="e2454-130">Hozzáférési házirendben tárolt: A tárolt házirend erőforrás tárolóba, mint a blobtárolót van definiálva.</span><span class="sxs-lookup"><span data-stu-id="e2454-130">Stored access policy: A stored access policy is defined on a resource container, such as a blob container.</span></span> <span data-ttu-id="e2454-131">A házirend legalább egy közös hozzáférésű jogosultságkód megkötéseit kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="e2454-131">A policy can be used to manage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="e2454-132">SAS-kód társítása a tárolt házirend, a SAS - a kezdési ideje, a lejárat időpontjának és a-vonatkozó engedélyeit a tárolt házirend korlátozásait örökölnek.</span><span class="sxs-lookup"><span data-stu-id="e2454-132">When you associate a SAS with a stored access policy, the SAS inherits the constraints - the start time, expiry time, and permissions - defined for the stored access policy.</span></span>

<span data-ttu-id="e2454-133">A különbség a két űrlap fontos egyik-forgatókönyvben: visszavont tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="e2454-133">The difference between the two forms is important for one key scenario: revocation.</span></span> <span data-ttu-id="e2454-134">SAS-kód egy URL-címet, így bárki, aki beolvassa a biztonsági Társítások használható, függetlenül attól, először aki kérte.</span><span class="sxs-lookup"><span data-stu-id="e2454-134">A SAS is a URL, so anyone who obtains the SAS can use it, regardless of who requested it to begin with.</span></span> <span data-ttu-id="e2454-135">Ha SAS-kód nyilvánosságra, bárki a világ használhatná.</span><span class="sxs-lookup"><span data-stu-id="e2454-135">If a SAS is published publicly, it can be used by anyone in the world.</span></span> <span data-ttu-id="e2454-136">Terjesztett SAS-kód nem érvényes, amíg a négy dolog történik:</span><span class="sxs-lookup"><span data-stu-id="e2454-136">A SAS that is distributed is valid until one of four things happens:</span></span>

1. <span data-ttu-id="e2454-137">A lejárat időpontjának a SAS megadott elérésekor.</span><span class="sxs-lookup"><span data-stu-id="e2454-137">The expiry time specified on the SAS is reached.</span></span>

2. <span data-ttu-id="e2454-138">A lejárat időpontjának a tárolt hozzáférési házirendet a biztonsági Társítások által hivatkozott megadott elérésekor.</span><span class="sxs-lookup"><span data-stu-id="e2454-138">The expiry time specified on the stored access policy referenced by the SAS is reached.</span></span> <span data-ttu-id="e2454-139">A következő esetekben okozhat a lejárati időpont érhető el:</span><span class="sxs-lookup"><span data-stu-id="e2454-139">The following scenarios cause the expiry time to be reached:</span></span>

    * <span data-ttu-id="e2454-140">Az időtartam lejárt.</span><span class="sxs-lookup"><span data-stu-id="e2454-140">The time interval has elapsed.</span></span>
    * <span data-ttu-id="e2454-141">A tárolt házirend úgy módosul, hogy egy lejárati dátuma a múltban van.</span><span class="sxs-lookup"><span data-stu-id="e2454-141">The stored access policy is modified to have an expiry time in the past.</span></span> <span data-ttu-id="e2454-142">A lejárati időpont módosítjuk az egyik módja a biztonsági Társítások visszavonása.</span><span class="sxs-lookup"><span data-stu-id="e2454-142">Changing the expiry time is one way to revoke the SAS.</span></span>

3. <span data-ttu-id="e2454-143">A tárolt házirend SAS által hivatkozott törölve van, amely másik módja is visszavonja a biztonsági Társítások.</span><span class="sxs-lookup"><span data-stu-id="e2454-143">The stored access policy referenced by the SAS is deleted, which is another way to revoke the SAS.</span></span> <span data-ttu-id="e2454-144">A tárolt házirend ugyanazzal a névvel hozza létre, ha a korábbi házirendet minden SAS-tokenje érvényesek (Ha még nem múlt el a biztonsági Társítások a lejárati idő).</span><span class="sxs-lookup"><span data-stu-id="e2454-144">If you recreate the stored access policy with the same name, all  SAS tokens for the previous policy are valid (if the expiry time on the SAS has not passed).</span></span> <span data-ttu-id="e2454-145">Ha azt tervezi, a biztonsági Társítások visszavonni, ügyeljen arra, hogy más nevet használjon, ha a hozzáférési házirendben a jövőben egy lejárati idővel hozza létre újra.</span><span class="sxs-lookup"><span data-stu-id="e2454-145">If you intend to revoke the SAS, be sure to use a different name if you recreate the access policy with an expiry time in the future.</span></span>

4. <span data-ttu-id="e2454-146">A biztonsági Társítások létrehozásához használt fiók kulcs újragenerálják.</span><span class="sxs-lookup"><span data-stu-id="e2454-146">The account key that was used to create the SAS is regenerated.</span></span> <span data-ttu-id="e2454-147">A kulcs újragenerálása hatására az összes sikertelen hitelesítésre az előző kulcsot használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e2454-147">Regenerating the key causes all applications that use the previous key to fail authentication.</span></span> <span data-ttu-id="e2454-148">Frissítse az összes összetevő az új kulccsal.</span><span class="sxs-lookup"><span data-stu-id="e2454-148">Update all components to the new key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2454-149">A közös hozzáférésű jogosultságkód URI társított aláírásának létrehozására használt fiók a kulccsal, és a társított tárolja hozzáférési házirend (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="e2454-149">A shared access signature URI is associated with the account key used to create the signature, and the associated stored access policy (if any).</span></span> <span data-ttu-id="e2454-150">Ha nincs tárolt házirend van megadva, csak visszavonni egy közös hozzáférésű jogosultságkódot, módosíthatja a fiókkulcsot.</span><span class="sxs-lookup"><span data-stu-id="e2454-150">If no stored access policy is specified, the only way to revoke a shared access signature is to change the account key.</span></span>

<span data-ttu-id="e2454-151">Javasoljuk, hogy mindig használjon tárolt hozzáférési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="e2454-151">We recommend that you always use stored access policies.</span></span> <span data-ttu-id="e2454-152">Tárolt házirendek használatakor aláírások visszavonására, és a lejárati dátum meghosszabbításához igény szerint.</span><span class="sxs-lookup"><span data-stu-id="e2454-152">When using stored policies, you can either revoke signatures or extend the expiry date as needed.</span></span> <span data-ttu-id="e2454-153">Ez a dokumentum használatát lépéseit tárolt hozzáférési házirendek biztonsági Társítások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e2454-153">The steps in this document use stored access policies to generate SAS.</span></span>

<span data-ttu-id="e2454-154">További információ a megosztott hozzáférési aláírásokkal: [ismertetése a SAS-modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="e2454-154">For more information on Shared Access Signatures, see [Understanding the SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

### <a name="create-a-stored-policy-and-sas-using-c"></a><span data-ttu-id="e2454-155">A tárolt házirend és a C használatával SAS létrehozása\#</span><span class="sxs-lookup"><span data-stu-id="e2454-155">Create a stored policy and SAS using C\#</span></span>

1. <span data-ttu-id="e2454-156">Nyissa meg a megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="e2454-156">Open the solution in Visual Studio.</span></span>

2. <span data-ttu-id="e2454-157">A Megoldáskezelőben kattintson a jobb gombbal a a **SASToken** projektre, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e2454-157">In Solution Explorer, right-click on the **SASToken** project and select **Properties**.</span></span>

3. <span data-ttu-id="e2454-158">Válassza ki **beállítások** , és adjon értékeket az alábbi bejegyzéseket:</span><span class="sxs-lookup"><span data-stu-id="e2454-158">Select **Settings** and add values for the following entries:</span></span>

   * <span data-ttu-id="e2454-159">StorageConnectionString: A tárolt házirend és az SAS-kód létrehozásához használni kívánt tárfiók kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="e2454-159">StorageConnectionString: The connection string for the storage account that you want to create a stored policy and SAS for.</span></span> <span data-ttu-id="e2454-160">A következő formátumban kell megadni `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` ahol `myaccount` a tárfiók neve és `mykey` a tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="e2454-160">The format should be `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` where `myaccount` is the name of your storage account and `mykey` is the key for the storage account.</span></span>

   * <span data-ttu-id="e2454-161">ContainerName: A tárfiókot, amely szeretné korlátozni a hozzáférést a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="e2454-161">ContainerName: The container in the storage account that you want to restrict access to.</span></span>

   * <span data-ttu-id="e2454-162">SASPolicyName: A tárolt házirend létrehozásához használni kívánt nevet.</span><span class="sxs-lookup"><span data-stu-id="e2454-162">SASPolicyName: The name to use for the stored policy to create.</span></span>

   * <span data-ttu-id="e2454-163">FileToUpload: A tárolóba feltöltött fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="e2454-163">FileToUpload: The path to a file that is uploaded to the container.</span></span>

4. <span data-ttu-id="e2454-164">Futtassa a projektet.</span><span class="sxs-lookup"><span data-stu-id="e2454-164">Run the project.</span></span> <span data-ttu-id="e2454-165">Az alábbi hasonló információk után a biztonsági Társítások hozott létre:</span><span class="sxs-lookup"><span data-stu-id="e2454-165">Information similar to the following text is displayed once the SAS has been generated:</span></span>

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="e2454-166">Mentse a SAS házirend jogkivonat, a tárfiók nevét és a tároló neve.</span><span class="sxs-lookup"><span data-stu-id="e2454-166">Save the SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="e2454-167">A storage-fiók társítása a HDInsight-fürt használja ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e2454-167">These values are used when associating the storage account with your HDInsight cluster.</span></span>

### <a name="create-a-stored-policy-and-sas-using-python"></a><span data-ttu-id="e2454-168">Hozzon létre egy tárolt házirend és a SAS pythonos környezetekben</span><span class="sxs-lookup"><span data-stu-id="e2454-168">Create a stored policy and SAS using Python</span></span>

1. <span data-ttu-id="e2454-169">Nyissa meg a SASToken.py fájlt, és módosítsa a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="e2454-169">Open the SASToken.py file and change the following values:</span></span>

   * <span data-ttu-id="e2454-170">házirend\_name: a tárolt házirend létrehozásához használni kívánt nevet.</span><span class="sxs-lookup"><span data-stu-id="e2454-170">policy\_name: The name to use for the stored policy to create.</span></span>

   * <span data-ttu-id="e2454-171">tárolási\_fiók\_name: a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="e2454-171">storage\_account\_name: The name of your storage account.</span></span>

   * <span data-ttu-id="e2454-172">tárolási\_fiók\_kulcs: a tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="e2454-172">storage\_account\_key: The key for the storage account.</span></span>

   * <span data-ttu-id="e2454-173">tárolási\_tároló\_name: a tárfiókot, amely szeretné korlátozni a hozzáférést a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="e2454-173">storage\_container\_name: The container in the storage account that you want to restrict access to.</span></span>

   * <span data-ttu-id="e2454-174">Példa\_fájl\_elérési út: egy a tárolóba feltöltött fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="e2454-174">example\_file\_path: The path to a file that is uploaded to the container.</span></span>

2. <span data-ttu-id="e2454-175">Futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="e2454-175">Run the script.</span></span> <span data-ttu-id="e2454-176">A parancsfájl lefutásakor a SAS-jogkivonatot az alábbihoz hasonló jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="e2454-176">It displays the SAS token similar to the following text when the script completes:</span></span>

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="e2454-177">Mentse a SAS házirend jogkivonat, a tárfiók nevét és a tároló neve.</span><span class="sxs-lookup"><span data-stu-id="e2454-177">Save the SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="e2454-178">A storage-fiók társítása a HDInsight-fürt használja ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e2454-178">These values are used when associating the storage account with your HDInsight cluster.</span></span>

## <a name="use-the-sas-with-hdinsight"></a><span data-ttu-id="e2454-179">A biztonsági Társítások használhat a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="e2454-179">Use the SAS with HDInsight</span></span>

<span data-ttu-id="e2454-180">HDInsight-fürtök létrehozásakor meg kell adnia egy elsődleges tárfiók, és opcionálisan megadhat további tárfiókokat.</span><span class="sxs-lookup"><span data-stu-id="e2454-180">When creating an HDInsight cluster, you must specify a primary storage account and you can optionally specify additional storage accounts.</span></span> <span data-ttu-id="e2454-181">Mindkét módszer tároló hozzáadása a storage-fiókok és a tárolók használt teljes hozzáférést igényelnek.</span><span class="sxs-lookup"><span data-stu-id="e2454-181">Both of these methods of adding storage require full access to the storage accounts and containers that are used.</span></span>

<span data-ttu-id="e2454-182">Egy közös hozzáférésű Jogosultságkód segítségével tárolóba való hozzáférés korlátozására, hogy egyéni bejegyzés hozzáadása a **core-hely** a fürt konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="e2454-182">To use a Shared Access Signature to limit access to a container, add a custom entry to the **core-site** configuration for the cluster.</span></span>

* <span data-ttu-id="e2454-183">A **Windows-alapú** vagy **Linux-alapú** HDInsight-fürtök, a bejegyzést adhat PowerShell-lel fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="e2454-183">For **Windows-based** or **Linux-based** HDInsight clusters, you can add the entry during cluster creation using PowerShell.</span></span>
* <span data-ttu-id="e2454-184">A **Linux-alapú** a HDInsight-fürtök, módosítsa a Ambari használatával fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="e2454-184">For **Linux-based** HDInsight clusters, change the configuration after cluster creation using Ambari.</span></span>

### <a name="create-a-cluster-that-uses-the-sas"></a><span data-ttu-id="e2454-185">A biztonsági Társítások használó fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2454-185">Create a cluster that uses the SAS</span></span>

<span data-ttu-id="e2454-186">A SAS használó HDInsight-fürtök létrehozására láthat példát megtalálható a `CreateCluster` mappában található a tárházban.</span><span class="sxs-lookup"><span data-stu-id="e2454-186">An example of creating an HDInsight cluster that uses the SAS is included in the `CreateCluster` directory of the repository.</span></span> <span data-ttu-id="e2454-187">A használatához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e2454-187">To use it, use the following steps:</span></span>

1. <span data-ttu-id="e2454-188">Nyissa meg a `CreateCluster\HDInsightSAS.ps1` fájlt egy szövegszerkesztőben, és módosítsa a következő értékeket a dokumentum elején.</span><span class="sxs-lookup"><span data-stu-id="e2454-188">Open the `CreateCluster\HDInsightSAS.ps1` file in a text editor and modify the following values at the beginning of the document.</span></span>

    ```powershell
    # Replace 'mycluster' with the name of the cluster to be created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with the name of the group to be created
    $resourceGroupName = 'myresourcegroup'
    # Replace with the Azure data center you want to the cluster to live in
    $location = 'North Europe'
    # Replace with the name of the default storage account to be created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with the name of the SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with the name of the SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with the SAS token generated earlier
    $SASToken = 'sastoken'
    # Set the number of worker nodes in the cluster
    $clusterSizeInNodes = 3
    ```

    <span data-ttu-id="e2454-189">Például `'mycluster'` a létrehozni kívánt fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="e2454-189">For example, change `'mycluster'` to the name of the cluster you want to create.</span></span> <span data-ttu-id="e2454-190">Az SAS-értékeket egy tárfiók és a SAS-jogkivonat létrehozásakor meg kell felelnie az értékeket az előző lépésekben.</span><span class="sxs-lookup"><span data-stu-id="e2454-190">The SAS values should match the values from the previous steps when creating a storage account and SAS token.</span></span>

    <span data-ttu-id="e2454-191">Ha módosította az értékeket, mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="e2454-191">Once you have changed the values, save the file.</span></span>

2. <span data-ttu-id="e2454-192">Nyisson meg egy új Azure PowerShell-parancssorba.</span><span class="sxs-lookup"><span data-stu-id="e2454-192">Open a new Azure PowerShell prompt.</span></span> <span data-ttu-id="e2454-193">Ha nem ismeri az Azure PowerShell, vagy nem telepítette azt, lásd: [telepítse és konfigurálja az Azure Powershellt][powershell].</span><span class="sxs-lookup"><span data-stu-id="e2454-193">If you are unfamiliar with Azure PowerShell, or have not installed it, see [Install and configure Azure PowerShell][powershell].</span></span>

1. <span data-ttu-id="e2454-194">A parancssorból a következő paranccsal, hogy az Azure-előfizetéshez hitelesítést:</span><span class="sxs-lookup"><span data-stu-id="e2454-194">From the prompt, use the following command to authenticate to your Azure subscription:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="e2454-195">Amikor a rendszer kéri, jelentkezzen be az Azure-előfizetéshez tartozó fiókkal.</span><span class="sxs-lookup"><span data-stu-id="e2454-195">When prompted, log in with the account for your Azure subscription.</span></span>

    <span data-ttu-id="e2454-196">Ha a fiók több Azure-előfizetéssel társítva, szükség lehet használandó `Select-AzureRmSubscription` való válassza ki a használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="e2454-196">If your account is associated with multiple Azure subscriptions, you may need to use `Select-AzureRmSubscription` to select the subscription you wish to use.</span></span>

4. <span data-ttu-id="e2454-197">A parancssorból lépjen a `CreateCluster` HDInsightSAS.ps1 fájlt tartalmazó könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e2454-197">From the prompt, change directories to the `CreateCluster` directory that contains the HDInsightSAS.ps1 file.</span></span> <span data-ttu-id="e2454-198">Az alábbi parancs segítségével futtassa a parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="e2454-198">Then use the following command to run the script</span></span>

    ```powershell
    .\HDInsightSAS.ps1
    ```

    <span data-ttu-id="e2454-199">A parancsfájl futtatása, mert azt naplóit kimeneti a PowerShell-parancssorba csoport és a storage-fiókokat készít az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="e2454-199">As the script runs, it logs output to the PowerShell prompt as it creates the resource group and storage accounts.</span></span> <span data-ttu-id="e2454-200">Adja meg a HTTP-felhasználó a HDInsight-fürthöz kéri.</span><span class="sxs-lookup"><span data-stu-id="e2454-200">You are prompted to enter the HTTP user for the HDInsight cluster.</span></span> <span data-ttu-id="e2454-201">Ez a fiók HTTP/s hozzáférést a fürthöz biztonságossá tételére szolgál.</span><span class="sxs-lookup"><span data-stu-id="e2454-201">This account is used to secure HTTP/s access to the cluster.</span></span>

    <span data-ttu-id="e2454-202">A Linux-alapú fürt létrehozásakor egy SSH felhasználói fiók felhasználónevét és jelszavát kéri.</span><span class="sxs-lookup"><span data-stu-id="e2454-202">If you are creating a Linux-based cluster, you are prompted for an SSH user account name and password.</span></span> <span data-ttu-id="e2454-203">Ez a fiók segítségével távolról jelentkezzen be a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="e2454-203">This account is used to remotely log in to the cluster.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="e2454-204">Ha a HTTP/s- vagy SSH-felhasználónév és jelszó megadására kéri, meg kell adnia egy jelszót, amely megfelel a következő feltételeknek:</span><span class="sxs-lookup"><span data-stu-id="e2454-204">When prompted for the HTTP/s or SSH user name and password, you must provide a password that meets the following criteria:</span></span>
   >
   > * <span data-ttu-id="e2454-205">Legalább 10 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2454-205">Must be at least 10 characters in length</span></span>
   > * <span data-ttu-id="e2454-206">Legalább egy számot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="e2454-206">Must contain at least one digit</span></span>
   > * <span data-ttu-id="e2454-207">Legalább egy nem alfanumerikus karaktert kell tartalmaznia</span><span class="sxs-lookup"><span data-stu-id="e2454-207">Must contain at least one non-alphanumeric character</span></span>
   > * <span data-ttu-id="e2454-208">Tartalmaznia kell legalább egy nagy- vagy kisbetűt</span><span class="sxs-lookup"><span data-stu-id="e2454-208">Must contain at least one upper or lower case letter</span></span>

<span data-ttu-id="e2454-209">Egy ideig, míg a parancsfájl végrehajtására, általában körülbelül 15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e2454-209">It takes a while for this script to complete, usually around 15 minutes.</span></span> <span data-ttu-id="e2454-210">Ha a parancsfájl hiba nélkül befejeződött, a fürt létrehozását.</span><span class="sxs-lookup"><span data-stu-id="e2454-210">When the script completes without any errors, the cluster has been created.</span></span>

### <a name="use-the-sas-with-an-existing-cluster"></a><span data-ttu-id="e2454-211">Az SA-kat használ egy meglévő fürthöz</span><span class="sxs-lookup"><span data-stu-id="e2454-211">Use the SAS with an existing cluster</span></span>

<span data-ttu-id="e2454-212">Ha egy meglévő Linux-alapú fürtöt, a SAS-t is hozzáadhat a **core-hely** konfigurációja az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="e2454-212">If you have an existing Linux-based cluster, you can add the SAS to the **core-site** configuration by using the following steps:</span></span>

1. <span data-ttu-id="e2454-213">Nyissa meg a fürt Ambari webes felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="e2454-213">Open the Ambari web UI for your cluster.</span></span> <span data-ttu-id="e2454-214">Ez a lap címe https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="e2454-214">The address for this page is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="e2454-215">Amikor a rendszer kéri, a fürtre, a felügyeleti neve (rendszergazda) használatával hitelesíteni és jelszó használatával, ha a fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e2454-215">When prompted, authenticate to the cluster using the admin name (admin) and password you used when creating the cluster.</span></span>

2. <span data-ttu-id="e2454-216">Válassza ki az Ambari webes felhasználói felület bal oldalán, **HDFS** , és válassza a **Configs** fülre az oldal közepén.</span><span class="sxs-lookup"><span data-stu-id="e2454-216">From the left side of the Ambari web UI, select **HDFS** and then select the **Configs** tab in the middle of the page.</span></span>

3. <span data-ttu-id="e2454-217">Válassza ki a **speciális** lapot, és görgessen, amíg meg nem látja a **egyéni core-hely** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2454-217">Select the **Advanced** tab, and then scroll until you find the **Custom core-site** section.</span></span>

4. <span data-ttu-id="e2454-218">Bontsa ki a **egyéni core-hely** szakaszban, majd görgessen a célból, és válassza ki a **tulajdonság hozzáadása...**  hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="e2454-218">Expand the **Custom core-site** section, then scroll to the end and select the **Add property...** link.</span></span> <span data-ttu-id="e2454-219">A következő értékeket használja a **kulcs** és **érték** mezők:</span><span class="sxs-lookup"><span data-stu-id="e2454-219">Use the following values for the **Key** and **Value** fields:</span></span>

   * <span data-ttu-id="e2454-220">**Kulcs**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="e2454-220">**Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span></span>
   * <span data-ttu-id="e2454-221">**Érték**: az SAS futtatta korábban C# vagy Python-alkalmazás által visszaadott</span><span class="sxs-lookup"><span data-stu-id="e2454-221">**Value**: The SAS returned by the C# or Python application you ran previously</span></span>

     <span data-ttu-id="e2454-222">Cserélje le **CONTAINERNAME** tároló nevű használt a C# vagy SAS alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e2454-222">Replace **CONTAINERNAME** with the container name you used with the C# or SAS application.</span></span> <span data-ttu-id="e2454-223">Cserélje le **STORAGEACCOUNTNAME** használt fiók nevével.</span><span class="sxs-lookup"><span data-stu-id="e2454-223">Replace **STORAGEACCOUNTNAME** with the storage account name you used.</span></span>

5. <span data-ttu-id="e2454-224">Kattintson a **Hozzáadás** gombra kattint, hogy a kulcs-érték mentéséhez, majd kattintson a **mentése** gombra a beállítások módosításainak mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="e2454-224">Click the **Add** button to save this key and value, then click the **Save** button to save the configuration changes.</span></span> <span data-ttu-id="e2454-225">Amikor a rendszer kéri, adjon meg egy leírást a változás ("hozzáadása SAS tárolók eléréséhez" például), és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e2454-225">When prompted, add a description of the change ("adding SAS storage access" for example) and then click **Save**.</span></span>

    <span data-ttu-id="e2454-226">Kattintson a **OK** Ha végrehajtotta a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e2454-226">Click **OK** when the changes have been completed.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="e2454-227">A módosítás érvénybe léptetéséhez újra kell indítania számos szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e2454-227">You must restart several services before the change takes effect.</span></span>

6. <span data-ttu-id="e2454-228">Válassza ki az Ambari webes felhasználói felület **HDFS** a bal oldali listában, és válassza **indítsa újra az összes** a a **szolgáltatás műveletek** legördülő listában kattintson a jobb.</span><span class="sxs-lookup"><span data-stu-id="e2454-228">In the Ambari web UI, select **HDFS** from the list on the left, and then select **Restart All** from the **Service Actions** drop down list on the right.</span></span> <span data-ttu-id="e2454-229">Amikor a rendszer kéri, válassza ki a **kapcsolja be a karbantartási mód** és majd válassza ki __Conform indítsa újra az összes ".</span><span class="sxs-lookup"><span data-stu-id="e2454-229">When prompted, select **Turn on maintenance mode** and then select __Conform Restart All".</span></span>

    <span data-ttu-id="e2454-230">Ismételje meg ezt a folyamatot MapReduce2 és YARN.</span><span class="sxs-lookup"><span data-stu-id="e2454-230">Repeat this process for MapReduce2 and YARN.</span></span>

7. <span data-ttu-id="e2454-231">A szolgáltatások újraindítása, ha mindegyiknél válassza ki, és a karbantartási mód letiltása a **szolgáltatás műveletek** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="e2454-231">Once the services have restarted, select each one and disable maintenance mode from the **Service Actions** drop down.</span></span>

## <a name="test-restricted-access"></a><span data-ttu-id="e2454-232">Korlátozott hozzáférés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e2454-232">Test restricted access</span></span>

<span data-ttu-id="e2454-233">Ha ellenőrizni szeretné, hogy korlátozott hozzáféréssel rendelkező, az alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="e2454-233">To verify that you have restricted access, use the following methods:</span></span>

* <span data-ttu-id="e2454-234">A **Windows-alapú** a HDInsight-fürtök, a távoli asztal használatával csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="e2454-234">For **Windows-based** HDInsight clusters, use Remote Desktop to connect to the cluster.</span></span> <span data-ttu-id="e2454-235">További információkért lásd: [csatlakozás RDP Funkciót használnak a HDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="e2454-235">For more information, see [Connect to HDInsight using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

    <span data-ttu-id="e2454-236">Miután csatlakozott, használja a **parancssori Hadoop** ikonjára az asztal nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="e2454-236">Once connected, use the **Hadoop Command-Line** icon on the desktop to open a command prompt.</span></span>

* <span data-ttu-id="e2454-237">A **Linux-alapú** a HDInsight-fürtök az SSH segítségével csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="e2454-237">For **Linux-based** HDInsight clusters, use SSH to connect to the cluster.</span></span> <span data-ttu-id="e2454-238">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="e2454-238">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="e2454-239">Miután csatlakozott a fürthöz, a következő lépések segítségével győződjön meg arról, hogy a csak olvasható és lista elemeket a biztonsági Társítások tárfiók is:</span><span class="sxs-lookup"><span data-stu-id="e2454-239">Once connected to the cluster, use the following steps to verify that you can only read and list items on the SAS storage account:</span></span>

1. <span data-ttu-id="e2454-240">A tároló tartalmának listájában, használja a következő parancsot a parancssorból:</span><span class="sxs-lookup"><span data-stu-id="e2454-240">To list the contents of the container, use the following command from the prompt:</span></span> 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    <span data-ttu-id="e2454-241">Cserélje le **SASCONTAINER** a SAS-tárfiók létrehozása a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="e2454-241">Replace **SASCONTAINER** with the name of the container created for the SAS storage account.</span></span> <span data-ttu-id="e2454-242">Cserélje le **SASACCOUNTNAME** SAS használt tárfiók nevével.</span><span class="sxs-lookup"><span data-stu-id="e2454-242">Replace **SASACCOUNTNAME** with the name of the storage account used for the SAS.</span></span>

    <span data-ttu-id="e2454-243">A lista tartalmazza a fájl feltöltése során a tároló és a SAS hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="e2454-243">The list includes the file uploaded when the container and SAS were created.</span></span>

2. <span data-ttu-id="e2454-244">A következő parancs használatával győződjön meg arról, hogy a fájl tartalmát érheti el.</span><span class="sxs-lookup"><span data-stu-id="e2454-244">Use the following command to verify that you can read the contents of the file.</span></span> <span data-ttu-id="e2454-245">Cserélje le a **SASCONTAINER** és **SASACCOUNTNAME** ahogy az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="e2454-245">Replace the **SASCONTAINER** and **SASACCOUNTNAME** as in the previous step.</span></span> <span data-ttu-id="e2454-246">Cserélje le **Fájlnév** jelennek meg az előző parancs a fájl nevét:</span><span class="sxs-lookup"><span data-stu-id="e2454-246">Replace **FILENAME** with the name of the file displayed in the previous command:</span></span>

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    <span data-ttu-id="e2454-247">Ez a parancs felsorolja a fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="e2454-247">This command lists the contents of the file.</span></span>

3. <span data-ttu-id="e2454-248">Az alábbi parancs segítségével töltse le a fájlt a helyi fájlrendszer:</span><span class="sxs-lookup"><span data-stu-id="e2454-248">Use the following command to download the file to the local file system:</span></span>

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    <span data-ttu-id="e2454-249">Ez a parancs letölti a fájlt egy helyi fájlba nevű **példa.txt**.</span><span class="sxs-lookup"><span data-stu-id="e2454-249">This command downloads the file to a local file named **testfile.txt**.</span></span>

4. <span data-ttu-id="e2454-250">A következő paranccsal egy új fájlt a helyi fájl feltöltése **testupload.txt** a SAS-tároló:</span><span class="sxs-lookup"><span data-stu-id="e2454-250">Use the following command to upload the local file to a new file named **testupload.txt** on the SAS storage:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    <span data-ttu-id="e2454-251">Az alábbihoz hasonló üzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="e2454-251">You receive a message similar to the following text:</span></span>

        put: java.io.IOException

    <span data-ttu-id="e2454-252">Ez a hiba akkor fordul elő, mivel a tárolási hely olvasási + lista csak.</span><span class="sxs-lookup"><span data-stu-id="e2454-252">This error occurs because the storage location is read+list only.</span></span> <span data-ttu-id="e2454-253">A következő paranccsal helyezze az adatokat a fürthöz, írható alapértelmezett tárolón:</span><span class="sxs-lookup"><span data-stu-id="e2454-253">Use the following command to put the data on the default storage for the cluster, which is writable:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    <span data-ttu-id="e2454-254">Most, a művelet sikeresen befejeződik.</span><span class="sxs-lookup"><span data-stu-id="e2454-254">This time, the operation should complete successfully.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="e2454-255">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="e2454-255">Troubleshooting</span></span>

### <a name="a-task-was-canceled"></a><span data-ttu-id="e2454-256">A feladat meg lett szakítva</span><span class="sxs-lookup"><span data-stu-id="e2454-256">A task was canceled</span></span>

<span data-ttu-id="e2454-257">**A jelenség**: a PowerShell-parancsfájlt a fürt létrehozásakor a következő hibaüzenet jelenhet:</span><span class="sxs-lookup"><span data-stu-id="e2454-257">**Symptoms**: When creating a cluster using the PowerShell script, you may receive the following error message:</span></span>

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

<span data-ttu-id="e2454-258">**OK**: Ez a hiba akkor fordulhat elő, ha a rendszergazda/HTTP felhasználó a fürt számára, vagy (a Linux-alapú fürtök) az SSH-felhasználó használja jelszó.</span><span class="sxs-lookup"><span data-stu-id="e2454-258">**Cause**: This error can occur if you use a password for the admin/HTTP user for the cluster, or (for Linux-based clusters) the SSH user.</span></span>

<span data-ttu-id="e2454-259">**Megoldási**: használhat olyan jelszót, amely megfelel a következő feltételeknek:</span><span class="sxs-lookup"><span data-stu-id="e2454-259">**Resolution**: Use a password that meets the following criteria:</span></span>

* <span data-ttu-id="e2454-260">Legalább 10 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2454-260">Must be at least 10 characters in length</span></span>
* <span data-ttu-id="e2454-261">Legalább egy számot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="e2454-261">Must contain at least one digit</span></span>
* <span data-ttu-id="e2454-262">Legalább egy nem alfanumerikus karaktert kell tartalmaznia</span><span class="sxs-lookup"><span data-stu-id="e2454-262">Must contain at least one non-alphanumeric character</span></span>
* <span data-ttu-id="e2454-263">Tartalmaznia kell legalább egy nagy- vagy kisbetűt</span><span class="sxs-lookup"><span data-stu-id="e2454-263">Must contain at least one upper or lower case letter</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2454-264">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2454-264">Next steps</span></span>

<span data-ttu-id="e2454-265">Most, hogy megismerte a korlátozott hozzáférésű tároló felvétele a HDInsight-fürt rendelkezik, ismerje meg, a fürtön lévő adatokkal dolgozni egyéb módjai:</span><span class="sxs-lookup"><span data-stu-id="e2454-265">Now that you have learned how to add limited-access storage to your HDInsight cluster, learn other ways to work with data on your cluster:</span></span>

* [<span data-ttu-id="e2454-266">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="e2454-266">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="e2454-267">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="e2454-267">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="e2454-268">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="e2454-268">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
