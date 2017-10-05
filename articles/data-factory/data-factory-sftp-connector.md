---
title: "Adatok áthelyezése az Azure Data Factory használatával SFTP kiszolgálóról |} Microsoft Docs"
description: "További tudnivalók az adatok mozgatása egy helyszíni vagy egy Azure Data Factory használatával felhő SFTP-kiszolgáló."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jingwang
ms.openlocfilehash: 3a73311342489af031ed2ea1489e56292ebf2e09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a><span data-ttu-id="996a8-103">Adatok áthelyezése az Azure Data Factory használatával SFTP-kiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="996a8-103">Move data from an SFTP server using Azure Data Factory</span></span>
<span data-ttu-id="996a8-104">Ez a cikk ismerteti, hogyan Azure Data Factory a másolási tevékenység használatával helyezze át az adatok áttelepítését egy helyszíni/felhőbeli SFTP támogatott fogadó adattárat.</span><span class="sxs-lookup"><span data-stu-id="996a8-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from an on-premises/cloud SFTP server to a supported sink data store.</span></span> <span data-ttu-id="996a8-105">Ez a cikk épít, a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja az adatmozgás általános áttekintést a másolási tevékenység és a támogatott adatforrások/mosdók adattárolókhoz listáját.</span><span class="sxs-lookup"><span data-stu-id="996a8-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="996a8-106">Adat-előállító jelenleg csak áthelyezése adatainak áttelepítését egy SFTP más adattárolókhoz, de nem adatok egyéb adattárakhoz egy SFTP kiszolgálóra helyezi át.</span><span class="sxs-lookup"><span data-stu-id="996a8-106">Data factory currently supports only moving data from an SFTP server to other data stores, but not for moving data from other data stores to an SFTP server.</span></span> <span data-ttu-id="996a8-107">Az támogatja-e mind a helyszíni és felhőalapú SFTP kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="996a8-107">It supports both on-premises and cloud SFTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="996a8-108">Másolási tevékenység nem törli a forrásfájl, miután sikerült átmásolni a cél.</span><span class="sxs-lookup"><span data-stu-id="996a8-108">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="996a8-109">Ha a forrásfájl törlése után sikeres másolatot van szüksége, létrehozhat egy egyéni törölje a fájlt, és az adatcsatorna használja a tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="996a8-109">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="996a8-110">Támogatott esetek és hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="996a8-110">Supported scenarios and authentication types</span></span>
<span data-ttu-id="996a8-111">Az SFTP-összekötő segítségével adatokat másolni **mind a felhőalapú SFTP és a helyszíni SFTP kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="996a8-111">You can use this SFTP connector to copy data from **both cloud SFTP servers and on-premises SFTP servers**.</span></span> <span data-ttu-id="996a8-112">**Alapszintű** és **SshPublicKey** hitelesítési típusok támogatottak a SFTP-kiszolgálóhoz való csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="996a8-112">**Basic** and **SshPublicKey** authentication types are supported when connecting to the SFTP server.</span></span>

<span data-ttu-id="996a8-113">Amikor adatokat másol egy helyszíni SFTP kiszolgáló, a helyszíni környezetben vagy az Azure virtuális gép adatkezelési átjárót kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="996a8-113">When copying data from an on-premises SFTP server, you need install a Data Management Gateway in the on-premises environment/Azure VM.</span></span> <span data-ttu-id="996a8-114">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) talál részletes információt az átjárót.</span><span class="sxs-lookup"><span data-stu-id="996a8-114">See [Data Management Gateway](data-factory-data-management-gateway.md) for details on the gateway.</span></span> <span data-ttu-id="996a8-115">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk részletes ismertetése az átjáró beállítása és használja azt.</span><span class="sxs-lookup"><span data-stu-id="996a8-115">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway and using it.</span></span>

## <a name="getting-started"></a><span data-ttu-id="996a8-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="996a8-116">Getting started</span></span>
<span data-ttu-id="996a8-117">A másolási tevékenység, amely helyezi át az adatokat SFTP forrásból származó különböző eszközök/API-k használatával hozhatja létre egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="996a8-117">You can create a pipeline with a copy activity that moves data from an SFTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="996a8-118">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="996a8-118">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="996a8-119">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="996a8-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

- <span data-ttu-id="996a8-120">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="996a8-120">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="996a8-121">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="996a8-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> <span data-ttu-id="996a8-122">Adatok másolása az SFTP server az Azure Blob Storage a JSON-minták, lásd: [JSON-példa: adatok másolása az SFTP server az Azure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="996a8-122">For JSON samples to copy data from SFTP server to Azure Blob Storage, see [JSON Example: Copy data from SFTP server to Azure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) section of this article.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="996a8-123">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="996a8-123">Linked service properties</span></span>
<span data-ttu-id="996a8-124">A következő táblázat vonatkozó FTP-társított szolgáltatás JSON-elemeket leírását.</span><span class="sxs-lookup"><span data-stu-id="996a8-124">The following table provides description for JSON elements specific to FTP linked service.</span></span>

| <span data-ttu-id="996a8-125">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="996a8-125">Property</span></span> | <span data-ttu-id="996a8-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="996a8-126">Description</span></span> | <span data-ttu-id="996a8-127">Szükséges</span><span class="sxs-lookup"><span data-stu-id="996a8-127">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="996a8-128">type</span><span class="sxs-lookup"><span data-stu-id="996a8-128">type</span></span> | <span data-ttu-id="996a8-129">A type tulajdonságot meg kell `Sftp`.</span><span class="sxs-lookup"><span data-stu-id="996a8-129">The type property must be set to `Sftp`.</span></span> |<span data-ttu-id="996a8-130">Igen</span><span class="sxs-lookup"><span data-stu-id="996a8-130">Yes</span></span> |
| <span data-ttu-id="996a8-131">állomás</span><span class="sxs-lookup"><span data-stu-id="996a8-131">host</span></span> | <span data-ttu-id="996a8-132">Az SFTP-kiszolgáló neve vagy IP-címét.</span><span class="sxs-lookup"><span data-stu-id="996a8-132">Name or IP address of the SFTP server.</span></span> |<span data-ttu-id="996a8-133">Igen</span><span class="sxs-lookup"><span data-stu-id="996a8-133">Yes</span></span> |
| <span data-ttu-id="996a8-134">port</span><span class="sxs-lookup"><span data-stu-id="996a8-134">port</span></span> |<span data-ttu-id="996a8-135">Port, amelyen az SFTP kiszolgáló figyel.</span><span class="sxs-lookup"><span data-stu-id="996a8-135">Port on which the SFTP server is listening.</span></span> <span data-ttu-id="996a8-136">Az alapértelmezett érték: 21.</span><span class="sxs-lookup"><span data-stu-id="996a8-136">The default value is: 21</span></span> |<span data-ttu-id="996a8-137">Nem</span><span class="sxs-lookup"><span data-stu-id="996a8-137">No</span></span> |
| <span data-ttu-id="996a8-138">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="996a8-138">authenticationType</span></span> |<span data-ttu-id="996a8-139">Adja meg a hitelesítés típusát.</span><span class="sxs-lookup"><span data-stu-id="996a8-139">Specify authentication type.</span></span> <span data-ttu-id="996a8-140">Megengedett értékek: **alapvető**, **SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="996a8-140">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="996a8-141">Tekintse meg [használja az egyszerű hitelesítés](#using-basic-authentication) és [használatával SSH nyilvános kulcsos hitelesítés](#using-ssh-public-key-authentication) további tulajdonságokat és JSON-minták szakasz.</span><span class="sxs-lookup"><span data-stu-id="996a8-141">Refer to [Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="996a8-142">Igen</span><span class="sxs-lookup"><span data-stu-id="996a8-142">Yes</span></span> |
| <span data-ttu-id="996a8-143">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="996a8-143">skipHostKeyValidation</span></span> | <span data-ttu-id="996a8-144">Adja meg, hogy a gazdagép kulcs ellenőrzésének kihagyására.</span><span class="sxs-lookup"><span data-stu-id="996a8-144">Specify whether to skip host key validation.</span></span> | <span data-ttu-id="996a8-145">Nem.</span><span class="sxs-lookup"><span data-stu-id="996a8-145">No.</span></span> <span data-ttu-id="996a8-146">Az alapértelmezett érték: hamis</span><span class="sxs-lookup"><span data-stu-id="996a8-146">The default value: false</span></span> |
| <span data-ttu-id="996a8-147">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="996a8-147">hostKeyFingerprint</span></span> | <span data-ttu-id="996a8-148">Adja meg a gazdagép kulcs az ujjlenyomat.</span><span class="sxs-lookup"><span data-stu-id="996a8-148">Specify the finger print of the host key.</span></span> | <span data-ttu-id="996a8-149">Igen, ha a `skipHostKeyValidation` hamis értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="996a8-149">Yes if the `skipHostKeyValidation` is set to false.</span></span>  |
| <span data-ttu-id="996a8-150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="996a8-150">gatewayName</span></span> |<span data-ttu-id="996a8-151">Az adatkezelési átjáró egy helyszíni SFTP-kiszolgálóhoz való csatlakozáshoz neve.</span><span class="sxs-lookup"><span data-stu-id="996a8-151">Name of the Data Management Gateway to connect to an on-premises SFTP server.</span></span> | <span data-ttu-id="996a8-152">Igen, ha az adatok másolása egy helyszíni SFTP-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="996a8-152">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="996a8-153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="996a8-153">encryptedCredential</span></span> | <span data-ttu-id="996a8-154">Titkosított hitelesítő adatokat a SFTP-kiszolgálóhoz való hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="996a8-154">Encrypted credential to access the SFTP server.</span></span> <span data-ttu-id="996a8-155">Automatikusan létrehozott Ha megadja az egyszerű hitelesítés (felhasználónév + jelszó) vagy az SshPublicKey hitelesítési (felhasználónév + titkos kulcs elérési útja vagy tartalom) másolása varázsló vagy a ClickOnce előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="996a8-155">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="996a8-156">Nem.</span><span class="sxs-lookup"><span data-stu-id="996a8-156">No.</span></span> <span data-ttu-id="996a8-157">Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="996a8-157">Apply only when copying data from an on-premises SFTP server.</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="996a8-158">Alapszintű hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="996a8-158">Using basic authentication</span></span>

<span data-ttu-id="996a8-159">Egyszerű hitelesítést használ, állítsa be `authenticationType` , `Basic`, és adja meg az SFTP összekötő általános néhányat a meglévők közül az utolsó szakaszban bemutatott mellett az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="996a8-159">To use basic authentication, set `authenticationType` as `Basic`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="996a8-160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="996a8-160">Property</span></span> | <span data-ttu-id="996a8-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="996a8-161">Description</span></span> | <span data-ttu-id="996a8-162">Szükséges</span><span class="sxs-lookup"><span data-stu-id="996a8-162">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="996a8-163">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="996a8-163">username</span></span> | <span data-ttu-id="996a8-164">Felhasználó, aki hozzáfér az SFTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="996a8-164">User who has access to the SFTP server.</span></span> |<span data-ttu-id="996a8-165">Igen</span><span class="sxs-lookup"><span data-stu-id="996a8-165">Yes</span></span> |
| <span data-ttu-id="996a8-166">jelszó</span><span class="sxs-lookup"><span data-stu-id="996a8-166">password</span></span> | <span data-ttu-id="996a8-167">A felhasználó (felhasználónév) jelszavát.</span><span class="sxs-lookup"><span data-stu-id="996a8-167">Password for the user (username).</span></span> | <span data-ttu-id="996a8-168">Igen</span><span class="sxs-lookup"><span data-stu-id="996a8-168">Yes</span></span> |

#### <a name="example-basic-authentication"></a><span data-ttu-id="996a8-169">Példa: Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="996a8-169">Example: Basic authentication</span></span>
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="996a8-170">Példa: Alapszintű hitelesítési titkosított hitelesítő adat</span><span class="sxs-lookup"><span data-stu-id="996a8-170">Example: Basic authentication with encrypted credential</span></span>

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="996a8-171">SSH nyilvános kulcsos hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="996a8-171">Using SSH public key authentication</span></span>

<span data-ttu-id="996a8-172">SSH nyilvános kulcsos hitelesítés használatához állítsa `authenticationType` , `SshPublicKey`, és adja meg az SFTP összekötő általános néhányat a meglévők közül az utolsó szakaszban bemutatott mellett az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="996a8-172">To use SSH public key authentication, set `authenticationType` as `SshPublicKey`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="996a8-173">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="996a8-173">Property</span></span> | <span data-ttu-id="996a8-174">Leírás</span><span class="sxs-lookup"><span data-stu-id="996a8-174">Description</span></span> | <span data-ttu-id="996a8-175">Szükséges</span><span class="sxs-lookup"><span data-stu-id="996a8-175">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="996a8-176">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="996a8-176">username</span></span> |<span data-ttu-id="996a8-177">SFTP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó</span><span class="sxs-lookup"><span data-stu-id="996a8-177">User who has access to the SFTP server</span></span> |<span data-ttu-id="996a8-178">Igen</span><span class="sxs-lookup"><span data-stu-id="996a8-178">Yes</span></span> |
| <span data-ttu-id="996a8-179">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="996a8-179">privateKeyPath</span></span> | <span data-ttu-id="996a8-180">Adjon meg abszolút elérési útját a titkos kulcs fájlját, hogy az átjáró férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="996a8-180">Specify absolute path to the private key file that gateway can access.</span></span> | <span data-ttu-id="996a8-181">Adja meg a `privateKeyPath` vagy `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="996a8-181">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="996a8-182">Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="996a8-182">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="996a8-183">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="996a8-183">privateKeyContent</span></span> | <span data-ttu-id="996a8-184">A titkos kulcs tartalmát, mivel a szerializált karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="996a8-184">A serialized string of the private key content.</span></span> <span data-ttu-id="996a8-185">A varázsló a titkos kulcsfájl olvashatja, és automatikusan bontsa ki a titkos kulcs tartalmát.</span><span class="sxs-lookup"><span data-stu-id="996a8-185">The Copy Wizard can read the private key file and extract the private key content automatically.</span></span> <span data-ttu-id="996a8-186">Ha minden egyéb eszköz/SDK használja, használja a privateKeyPath tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="996a8-186">If you are using any other tool/SDK, use the privateKeyPath property instead.</span></span> | <span data-ttu-id="996a8-187">Adja meg a `privateKeyPath` vagy `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="996a8-187">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="996a8-188">hozzáférési kód</span><span class="sxs-lookup"><span data-stu-id="996a8-188">passPhrase</span></span> | <span data-ttu-id="996a8-189">Adja meg a pass kifejezést/jelszót a titkos kulcs visszafejtésére, ha a kulcs fájlját egy hozzáférési kódot védi.</span><span class="sxs-lookup"><span data-stu-id="996a8-189">Specify the pass phrase/password to decrypt the private key if the key file is protected by a pass phrase.</span></span> | <span data-ttu-id="996a8-190">Igen, ha a titkos kulcsfájl védik a hozzáférési kód.</span><span class="sxs-lookup"><span data-stu-id="996a8-190">Yes if the private key file is protected by a pass phrase.</span></span> |

> [!NOTE]
> <span data-ttu-id="996a8-191">SFTP-összekötő csak támogatja a protokoll OpenSSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="996a8-191">SFTP connector only support OpenSSH key.</span></span> <span data-ttu-id="996a8-192">Győződjön meg arról, hogy a fájl nem megfelelő formátumú.</span><span class="sxs-lookup"><span data-stu-id="996a8-192">Make sure your key file is in the proper format.</span></span> <span data-ttu-id="996a8-193">Használhatja a Putty eszközt .ppk átalakítása OpenSSH formátumban.</span><span class="sxs-lookup"><span data-stu-id="996a8-193">You can use Putty tool to convert from .ppk to OpenSSH format.</span></span>

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a><span data-ttu-id="996a8-194">Példa: Az SshPublicKey hitelesítés használata a titkos kulcs fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="996a8-194">Example: SshPublicKey authentication using private key filePath</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="996a8-195">Példa: Az SshPublicKey hitelesítés használata a titkos kulcs tartalmát</span><span class="sxs-lookup"><span data-stu-id="996a8-195">Example: SshPublicKey authentication using private key content</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="996a8-196">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="996a8-196">Dataset properties</span></span>
<span data-ttu-id="996a8-197">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="996a8-197">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="996a8-198">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="996a8-198">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="996a8-199">A **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="996a8-199">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="996a8-200">A dataset típusra vonatkozó adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="996a8-200">It provides information that is specific to the dataset type.</span></span> <span data-ttu-id="996a8-201">A typeProperties szakasz egy adatkészlet típusú **fájlmegosztási** adatkészlet tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="996a8-201">The typeProperties section for a dataset of type **FileShare** dataset has the following properties:</span></span>

| <span data-ttu-id="996a8-202">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="996a8-202">Property</span></span> | <span data-ttu-id="996a8-203">Leírás</span><span class="sxs-lookup"><span data-stu-id="996a8-203">Description</span></span> | <span data-ttu-id="996a8-204">Szükséges</span><span class="sxs-lookup"><span data-stu-id="996a8-204">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="996a8-205">folderPath</span><span class="sxs-lookup"><span data-stu-id="996a8-205">folderPath</span></span> |<span data-ttu-id="996a8-206">Sub mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="996a8-206">Sub path to the folder.</span></span> <span data-ttu-id="996a8-207">Használja az escape-karakter "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="996a8-207">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="996a8-208">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="996a8-208">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="996a8-209">Ez a tulajdonság a kombinálhatja **partitionBy** szeretné, hogy a mappa elérési utak alapján szelet kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="996a8-209">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="996a8-210">Igen</span><span class="sxs-lookup"><span data-stu-id="996a8-210">Yes</span></span> |
| <span data-ttu-id="996a8-211">fileName</span><span class="sxs-lookup"><span data-stu-id="996a8-211">fileName</span></span> |<span data-ttu-id="996a8-212">Adja meg a fájl nevét a **folderPath** Ha azt szeretné, hogy a tábla egy adott fájlra a mappában.</span><span class="sxs-lookup"><span data-stu-id="996a8-212">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="996a8-213">Ha nem ad meg ehhez a tulajdonsághoz értéket, a tábla a mappában lévő összes fájlt mutat.</span><span class="sxs-lookup"><span data-stu-id="996a8-213">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="996a8-214">Ha nincs megadva fájlnév egy kimeneti adatkészletet, a létrehozott fájl nevét a következő lenne ebben a formátumban:</span><span class="sxs-lookup"><span data-stu-id="996a8-214">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="996a8-215">Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="996a8-215">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="996a8-216">Nem</span><span class="sxs-lookup"><span data-stu-id="996a8-216">No</span></span> |
| <span data-ttu-id="996a8-217">fileFilter</span><span class="sxs-lookup"><span data-stu-id="996a8-217">fileFilter</span></span> |<span data-ttu-id="996a8-218">Adjon meg egy szűrőt, amely minden fájl helyett a fájlok Tárolónév részhalmazának kiválasztására szolgál.</span><span class="sxs-lookup"><span data-stu-id="996a8-218">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="996a8-219">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="996a8-219">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="996a8-220">1. példa:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="996a8-220">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="996a8-221">2. példa:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="996a8-221">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="996a8-222">fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="996a8-222">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="996a8-223">Ez a tulajdonság a HDFS nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="996a8-223">This property is not supported with HDFS.</span></span> |<span data-ttu-id="996a8-224">Nem</span><span class="sxs-lookup"><span data-stu-id="996a8-224">No</span></span> |
| <span data-ttu-id="996a8-225">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="996a8-225">partitionedBy</span></span> |<span data-ttu-id="996a8-226">Adjon meg egy dinamikus folderPath idő adatsor fájlnevét partitionedBy használható.</span><span class="sxs-lookup"><span data-stu-id="996a8-226">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="996a8-227">Például folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="996a8-227">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="996a8-228">Nem</span><span class="sxs-lookup"><span data-stu-id="996a8-228">No</span></span> |
| <span data-ttu-id="996a8-229">Formátumban</span><span class="sxs-lookup"><span data-stu-id="996a8-229">format</span></span> | <span data-ttu-id="996a8-230">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="996a8-230">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="996a8-231">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="996a8-231">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="996a8-232">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="996a8-232">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="996a8-233">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="996a8-233">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="996a8-234">Nem</span><span class="sxs-lookup"><span data-stu-id="996a8-234">No</span></span> |
| <span data-ttu-id="996a8-235">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="996a8-235">compression</span></span> | <span data-ttu-id="996a8-236">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="996a8-236">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="996a8-237">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="996a8-237">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="996a8-238">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="996a8-238">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="996a8-239">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="996a8-239">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="996a8-240">Nem</span><span class="sxs-lookup"><span data-stu-id="996a8-240">No</span></span> |
| <span data-ttu-id="996a8-241">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="996a8-241">useBinaryTransfer</span></span> |<span data-ttu-id="996a8-242">Adja meg, hogy a bináris átviteli mód használata.</span><span class="sxs-lookup"><span data-stu-id="996a8-242">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="996a8-243">A bináris mód és a hamis értéket ASCII igaz.</span><span class="sxs-lookup"><span data-stu-id="996a8-243">True for binary mode and false ASCII.</span></span> <span data-ttu-id="996a8-244">Alapértelmezett érték: igaz.</span><span class="sxs-lookup"><span data-stu-id="996a8-244">Default value: True.</span></span> <span data-ttu-id="996a8-245">A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="996a8-245">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="996a8-246">Nem</span><span class="sxs-lookup"><span data-stu-id="996a8-246">No</span></span> |

> [!NOTE]
> <span data-ttu-id="996a8-247">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="996a8-247">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="996a8-248">PartionedBy tulajdonság használatával</span><span class="sxs-lookup"><span data-stu-id="996a8-248">Using partionedBy property</span></span>
<span data-ttu-id="996a8-249">Az előző szakaszban említett, megadhat egy dinamikus folderPath idő adatsorozat adatok partitionedBy fájlnevét.</span><span class="sxs-lookup"><span data-stu-id="996a8-249">As mentioned in the previous section, you can specify a dynamic folderPath, filename for time series data with partitionedBy.</span></span> <span data-ttu-id="996a8-250">A Data Factory makrók és a rendszer változó SliceStart, egy adott adatszelet logikai időtartamnak jelző SliceEnd azt is megteheti.</span><span class="sxs-lookup"><span data-stu-id="996a8-250">You can do so with the Data Factory macros and the system variable SliceStart, SliceEnd that indicate the logical time period for a given data slice.</span></span>

<span data-ttu-id="996a8-251">Idő adatsorozat adatkészleteket, az ütemezés és a szeletek kapcsolatos további tudnivalókért lásd: [létrehozása adatkészletek](data-factory-create-datasets.md), [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md), és [létrehozása folyamatok](data-factory-create-pipelines.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="996a8-251">To learn about time series datasets, scheduling, and slices, See [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="996a8-252">1. példa:</span><span class="sxs-lookup"><span data-stu-id="996a8-252">Sample 1:</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="996a8-253">Ebben a példában {szelet} adat-előállító rendszer változó SliceStart (YYYYMMDDHH) formátumban megadott érték helyére.</span><span class="sxs-lookup"><span data-stu-id="996a8-253">In this example {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="996a8-254">A szelet kezdete a SliceStart hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="996a8-254">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="996a8-255">A folderPath nem azonos az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="996a8-255">The folderPath is different for each slice.</span></span> <span data-ttu-id="996a8-256">Példa: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="996a8-256">Example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="996a8-257">2. példa:</span><span class="sxs-lookup"><span data-stu-id="996a8-257">Sample 2:</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
<span data-ttu-id="996a8-258">Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek folderPath és a fájlnév tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="996a8-258">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="996a8-259">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="996a8-259">Copy activity properties</span></span>
<span data-ttu-id="996a8-260">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="996a8-260">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="996a8-261">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="996a8-261">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="996a8-262">Mivel a rendelkezésre álló tulajdonságok a tevékenység typeProperties szakaszában tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="996a8-262">Whereas, the properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="996a8-263">A másolási tevékenység során a típus tulajdonságokat. az adatforrások és mosdók függenek.</span><span class="sxs-lookup"><span data-stu-id="996a8-263">For Copy activity, the type properties vary depending on the types of sources and sinks.</span></span>

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="996a8-264">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="996a8-264">Supported file and compression formats</span></span>
<span data-ttu-id="996a8-265">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.</span><span class="sxs-lookup"><span data-stu-id="996a8-265">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-sftp-server-to-azure-blob"></a><span data-ttu-id="996a8-266">JSON-NÁ. példa: Adatok másolása az SFTP server az Azure blob</span><span class="sxs-lookup"><span data-stu-id="996a8-266">JSON Example: Copy data from SFTP server to Azure blob</span></span>
<span data-ttu-id="996a8-267">Az alábbi példa minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="996a8-267">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="996a8-268">SFTP forrásból származó adatok másolása az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="996a8-268">They show how to copy data from SFTP source to Azure Blob Storage.</span></span> <span data-ttu-id="996a8-269">Azonban az adatok átmásolhatók **közvetlenül** a forrásokban, sem a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="996a8-269">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="996a8-270">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="996a8-270">This sample provides JSON snippets.</span></span> <span data-ttu-id="996a8-271">Nem tartalmazza az adat-előállítóban létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="996a8-271">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="996a8-272">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="996a8-272">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="996a8-273">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="996a8-273">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="996a8-274">A társított szolgáltatás típusa [sftp](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="996a8-274">A linked service of type [sftp](#linked-service-properties).</span></span>
* <span data-ttu-id="996a8-275">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="996a8-275">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="996a8-276">Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="996a8-276">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="996a8-277">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="996a8-277">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="996a8-278">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="996a8-278">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="996a8-279">A minta másol adatokat az SFTP-kiszolgáló egy Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="996a8-279">The sample copies data from an SFTP server to an Azure blob every hour.</span></span> <span data-ttu-id="996a8-280">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="996a8-280">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="996a8-281">**Kapcsolódó SFTP szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="996a8-281">**SFTP linked service**</span></span>

<span data-ttu-id="996a8-282">Ebben a példában az egyszerű hitelesítést használ, a felhasználónevet és jelszót egyszerű szövegként.</span><span class="sxs-lookup"><span data-stu-id="996a8-282">This example uses the basic authentication with user name and password in plain text.</span></span> <span data-ttu-id="996a8-283">Használhatja a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="996a8-283">You can also use one of the following ways:</span></span>

* <span data-ttu-id="996a8-284">Egyszerű hitelesítés titkosított hitelesítő adatokkal</span><span class="sxs-lookup"><span data-stu-id="996a8-284">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="996a8-285">SSH nyilvános kulcsos hitelesítés</span><span class="sxs-lookup"><span data-stu-id="996a8-285">SSH public key authentication</span></span>

<span data-ttu-id="996a8-286">Lásd: [FTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="996a8-286">See [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
<span data-ttu-id="996a8-287">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="996a8-287">**Azure Storage linked service**</span></span>

```JSON
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="996a8-288">**SFTP bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="996a8-288">**SFTP input dataset**</span></span>

<span data-ttu-id="996a8-289">Ehhez az adatkészlethez az SFTP mappára hivatkozik `mysharedfolder` és fájl `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="996a8-289">This dataset refers to the SFTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="996a8-290">A feldolgozási sor átmásolja a fájlt a cél.</span><span class="sxs-lookup"><span data-stu-id="996a8-290">The pipeline copies the file to the destination.</span></span>

<span data-ttu-id="996a8-291">"External" beállítása: "true" arról tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet külső data factoryval való és adat-előállító tevékenység nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="996a8-291">Setting "external": "true" informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="996a8-292">**Az Azure Blob kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="996a8-292">**Azure Blob output dataset**</span></span>

<span data-ttu-id="996a8-293">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="996a8-293">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="996a8-294">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="996a8-294">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="996a8-295">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="996a8-295">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="996a8-296">**A másolási tevékenység-feldolgozási folyamat**</span><span class="sxs-lookup"><span data-stu-id="996a8-296">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="996a8-297">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="996a8-297">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="996a8-298">Az adatcsatorna JSON-definícióból a **forrás** típusúra **FileSystemSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="996a8-298">In the pipeline JSON definition, the **source** type is set to **FileSystemSource** and **sink** type is set to **BlobSink**.</span></span>

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a><span data-ttu-id="996a8-299">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="996a8-299">Performance and Tuning</span></span>
<span data-ttu-id="996a8-300">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="996a8-300">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="996a8-301">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="996a8-301">Next Steps</span></span>
<span data-ttu-id="996a8-302">Lásd az alábbi cikkeket:</span><span class="sxs-lookup"><span data-stu-id="996a8-302">See the following articles:</span></span>

* <span data-ttu-id="996a8-303">[Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="996a8-303">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
