---
title: "Azure Data Factory használatával SFTP kiszolgálóról aaaMove adatok |} Microsoft Docs"
description: "Megtudhatja, hogyan egy helyszíni vagy egy Azure Data Factory használatával felhő SFTP server toomove adatait."
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
ms.openlocfilehash: b976289e2c1b1899634bb5565b375499077fa554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a><span data-ttu-id="cc052-103">Adatok áthelyezése az Azure Data Factory használatával SFTP-kiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="cc052-103">Move data from an SFTP server using Azure Data Factory</span></span>
<span data-ttu-id="cc052-104">Ez a cikk ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatait egy helyszíni/felhőbeli SFTP server tooa támogatott fogadó adattár.</span><span class="sxs-lookup"><span data-stu-id="cc052-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises/cloud SFTP server tooa supported sink data store.</span></span> <span data-ttu-id="cc052-105">Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja a másolási tevékenység és hello listáját támogatott adatforrások/mosdók adattárolókhoz adatmozgás általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="cc052-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="cc052-106">Adat-előállító jelenleg csak áthelyezése egy SFTP kiszolgálóadatok tooother adatait tárolja, de nem támogatja az adatok áthelyezését más adatok tooan SFTP kiszolgáló tárolja.</span><span class="sxs-lookup"><span data-stu-id="cc052-106">Data factory currently supports only moving data from an SFTP server tooother data stores, but not for moving data from other data stores tooan SFTP server.</span></span> <span data-ttu-id="cc052-107">Az támogatja-e mind a helyszíni és felhőalapú SFTP kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="cc052-107">It supports both on-premises and cloud SFTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="cc052-108">Másolási tevékenység nem hello forrásfájl törlése, miután sikeresen másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="cc052-108">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="cc052-109">Ha toodelete hello forrásfájl után sikeres másolatát, hozzon létre egy egyéni tevékenység toodelete hello fájlt, és a hello tevékenység hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="cc052-109">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="cc052-110">Támogatott esetek és hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="cc052-110">Supported scenarios and authentication types</span></span>
<span data-ttu-id="cc052-111">Használhatja az SFTP összekötő toocopy adatait **mind a felhőalapú SFTP és a helyszíni SFTP kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="cc052-111">You can use this SFTP connector toocopy data from **both cloud SFTP servers and on-premises SFTP servers**.</span></span> <span data-ttu-id="cc052-112">**Alapszintű** és **SshPublicKey** hitelesítési típusok támogatottak toohello SFTP kiszolgálóhoz kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="cc052-112">**Basic** and **SshPublicKey** authentication types are supported when connecting toohello SFTP server.</span></span>

<span data-ttu-id="cc052-113">Adatok áttelepítését egy helyszíni SFTP másolásakor hello a helyszíni környezetben vagy az Azure virtuális gép adatkezelési átjárót kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="cc052-113">When copying data from an on-premises SFTP server, you need install a Data Management Gateway in hello on-premises environment/Azure VM.</span></span> <span data-ttu-id="cc052-114">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) hello átjáró leírását.</span><span class="sxs-lookup"><span data-stu-id="cc052-114">See [Data Management Gateway](data-factory-data-management-gateway.md) for details on hello gateway.</span></span> <span data-ttu-id="cc052-115">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) lépésenként hello átjáró beállításával és használatával, a cikkben találhat.</span><span class="sxs-lookup"><span data-stu-id="cc052-115">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway and using it.</span></span>

## <a name="getting-started"></a><span data-ttu-id="cc052-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="cc052-116">Getting started</span></span>
<span data-ttu-id="cc052-117">A másolási tevékenység, amely helyezi át az adatokat SFTP forrásból származó különböző eszközök/API-k használatával hozhatja létre egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="cc052-117">You can create a pipeline with a copy activity that moves data from an SFTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="cc052-118">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="cc052-118">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="cc052-119">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="cc052-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

- <span data-ttu-id="cc052-120">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="cc052-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="cc052-121">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="cc052-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> <span data-ttu-id="cc052-122">JSON-minták SFTP server tooAzure Blob Storage toocopy adatait, a következő témakörben: [JSON-példa: adatok másolása az SFTP server tooAzure blobból](#json-example-copy-data-from-sftp-server-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="cc052-122">For JSON samples toocopy data from SFTP server tooAzure Blob Storage, see [JSON Example: Copy data from SFTP server tooAzure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) section of this article.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="cc052-123">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="cc052-123">Linked service properties</span></span>
<span data-ttu-id="cc052-124">a következő táblázat hello biztosít JSON elemek adott tooFTP kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="cc052-124">hello following table provides description for JSON elements specific tooFTP linked service.</span></span>

| <span data-ttu-id="cc052-125">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc052-125">Property</span></span> | <span data-ttu-id="cc052-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc052-126">Description</span></span> | <span data-ttu-id="cc052-127">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc052-127">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc052-128">type</span><span class="sxs-lookup"><span data-stu-id="cc052-128">type</span></span> | <span data-ttu-id="cc052-129">hello type tulajdonság túl be kell állítani`Sftp`.</span><span class="sxs-lookup"><span data-stu-id="cc052-129">hello type property must be set too`Sftp`.</span></span> |<span data-ttu-id="cc052-130">Igen</span><span class="sxs-lookup"><span data-stu-id="cc052-130">Yes</span></span> |
| <span data-ttu-id="cc052-131">állomás</span><span class="sxs-lookup"><span data-stu-id="cc052-131">host</span></span> | <span data-ttu-id="cc052-132">Hello SFTP kiszolgáló neve vagy IP-címét.</span><span class="sxs-lookup"><span data-stu-id="cc052-132">Name or IP address of hello SFTP server.</span></span> |<span data-ttu-id="cc052-133">Igen</span><span class="sxs-lookup"><span data-stu-id="cc052-133">Yes</span></span> |
| <span data-ttu-id="cc052-134">port</span><span class="sxs-lookup"><span data-stu-id="cc052-134">port</span></span> |<span data-ttu-id="cc052-135">Az port, mely hello SFTP kiszolgáló figyel.</span><span class="sxs-lookup"><span data-stu-id="cc052-135">Port on which hello SFTP server is listening.</span></span> <span data-ttu-id="cc052-136">hello alapértelmezett érték: 21.</span><span class="sxs-lookup"><span data-stu-id="cc052-136">hello default value is: 21</span></span> |<span data-ttu-id="cc052-137">Nem</span><span class="sxs-lookup"><span data-stu-id="cc052-137">No</span></span> |
| <span data-ttu-id="cc052-138">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="cc052-138">authenticationType</span></span> |<span data-ttu-id="cc052-139">Adja meg a hitelesítés típusát.</span><span class="sxs-lookup"><span data-stu-id="cc052-139">Specify authentication type.</span></span> <span data-ttu-id="cc052-140">Megengedett értékek: **alapvető**, **SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="cc052-140">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="cc052-141">Tekintse meg a túl[használja az egyszerű hitelesítés](#using-basic-authentication) és [használatával SSH nyilvános kulcsos hitelesítés](#using-ssh-public-key-authentication) további tulajdonságokat és JSON-minták szakasz.</span><span class="sxs-lookup"><span data-stu-id="cc052-141">Refer too[Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="cc052-142">Igen</span><span class="sxs-lookup"><span data-stu-id="cc052-142">Yes</span></span> |
| <span data-ttu-id="cc052-143">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="cc052-143">skipHostKeyValidation</span></span> | <span data-ttu-id="cc052-144">Adja meg, hogy tooskip gazdagép kulcs érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="cc052-144">Specify whether tooskip host key validation.</span></span> | <span data-ttu-id="cc052-145">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc052-145">No.</span></span> <span data-ttu-id="cc052-146">alapértelmezett érték hello: hamis</span><span class="sxs-lookup"><span data-stu-id="cc052-146">hello default value: false</span></span> |
| <span data-ttu-id="cc052-147">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="cc052-147">hostKeyFingerprint</span></span> | <span data-ttu-id="cc052-148">Adja meg a hello ujjlenyomat hello állomás kulcs.</span><span class="sxs-lookup"><span data-stu-id="cc052-148">Specify hello finger print of hello host key.</span></span> | <span data-ttu-id="cc052-149">Igen, ha a hello `skipHostKeyValidation` toofalse van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cc052-149">Yes if hello `skipHostKeyValidation` is set toofalse.</span></span>  |
| <span data-ttu-id="cc052-150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="cc052-150">gatewayName</span></span> |<span data-ttu-id="cc052-151">Hello az adatkezelési átjáró tooconnect tooan nevét a helyszíni SFTP kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="cc052-151">Name of hello Data Management Gateway tooconnect tooan on-premises SFTP server.</span></span> | <span data-ttu-id="cc052-152">Igen, ha az adatok másolása egy helyszíni SFTP-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="cc052-152">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="cc052-153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="cc052-153">encryptedCredential</span></span> | <span data-ttu-id="cc052-154">Titkosított hitelesítő adatokban tooaccess hello SFTP kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="cc052-154">Encrypted credential tooaccess hello SFTP server.</span></span> <span data-ttu-id="cc052-155">Automatikusan létrehozott Ha megadja az egyszerű hitelesítés (felhasználónév + jelszó) vagy az SshPublicKey hitelesítési (felhasználónév + titkos kulcs elérési útja vagy tartalom) másolása varázsló vagy a hello ClickOnce előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="cc052-155">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="cc052-156">Nem.</span><span class="sxs-lookup"><span data-stu-id="cc052-156">No.</span></span> <span data-ttu-id="cc052-157">Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="cc052-157">Apply only when copying data from an on-premises SFTP server.</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="cc052-158">Alapszintű hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="cc052-158">Using basic authentication</span></span>

<span data-ttu-id="cc052-159">Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `Basic`, és adja meg a következő tulajdonságai módosításokon kívül SFTP összekötő hello utolsó szakaszában bevezetett általános ők hello hello:</span><span class="sxs-lookup"><span data-stu-id="cc052-159">toouse basic authentication, set `authenticationType` as `Basic`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="cc052-160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc052-160">Property</span></span> | <span data-ttu-id="cc052-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc052-161">Description</span></span> | <span data-ttu-id="cc052-162">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc052-162">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc052-163">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc052-163">username</span></span> | <span data-ttu-id="cc052-164">Az felhasználó, aki rendelkezik toohello SFTP kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="cc052-164">User who has access toohello SFTP server.</span></span> |<span data-ttu-id="cc052-165">Igen</span><span class="sxs-lookup"><span data-stu-id="cc052-165">Yes</span></span> |
| <span data-ttu-id="cc052-166">jelszó</span><span class="sxs-lookup"><span data-stu-id="cc052-166">password</span></span> | <span data-ttu-id="cc052-167">(Felhasználónév) hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cc052-167">Password for hello user (username).</span></span> | <span data-ttu-id="cc052-168">Igen</span><span class="sxs-lookup"><span data-stu-id="cc052-168">Yes</span></span> |

#### <a name="example-basic-authentication"></a><span data-ttu-id="cc052-169">Példa: Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="cc052-169">Example: Basic authentication</span></span>
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

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="cc052-170">Példa: Alapszintű hitelesítési titkosított hitelesítő adat</span><span class="sxs-lookup"><span data-stu-id="cc052-170">Example: Basic authentication with encrypted credential</span></span>

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

### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="cc052-171">SSH nyilvános kulcsos hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="cc052-171">Using SSH public key authentication</span></span>

<span data-ttu-id="cc052-172">toouse SSH nyilvános kulcsos hitelesítés beállítása `authenticationType` , `SshPublicKey`, és adja meg a következő tulajdonságai módosításokon kívül SFTP összekötő hello utolsó szakaszában bevezetett általános ők hello hello:</span><span class="sxs-lookup"><span data-stu-id="cc052-172">toouse SSH public key authentication, set `authenticationType` as `SshPublicKey`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="cc052-173">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc052-173">Property</span></span> | <span data-ttu-id="cc052-174">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc052-174">Description</span></span> | <span data-ttu-id="cc052-175">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc052-175">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cc052-176">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="cc052-176">username</span></span> |<span data-ttu-id="cc052-177">Felhasználó, aki rendelkezik toohello SFTP kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cc052-177">User who has access toohello SFTP server</span></span> |<span data-ttu-id="cc052-178">Igen</span><span class="sxs-lookup"><span data-stu-id="cc052-178">Yes</span></span> |
| <span data-ttu-id="cc052-179">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="cc052-179">privateKeyPath</span></span> | <span data-ttu-id="cc052-180">Adjon meg abszolút elérési út toohello titkos kulcsot tartalmazó fájlt, hogy az átjáró férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="cc052-180">Specify absolute path toohello private key file that gateway can access.</span></span> | <span data-ttu-id="cc052-181">Adja meg vagy hello `privateKeyPath` vagy `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="cc052-181">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="cc052-182">Csak akkor, ha az adatok másolása egy helyszíni SFTP kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="cc052-182">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="cc052-183">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="cc052-183">privateKeyContent</span></span> | <span data-ttu-id="cc052-184">Hello titkos kulcs tartalmát, mivel a szerializált karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cc052-184">A serialized string of hello private key content.</span></span> <span data-ttu-id="cc052-185">hello másolása varázsló olvashatja hello titkos kulcsot tartalmazó fájlt, és bontsa ki a titkos kulcs tartalmát hello automatikusan.</span><span class="sxs-lookup"><span data-stu-id="cc052-185">hello Copy Wizard can read hello private key file and extract hello private key content automatically.</span></span> <span data-ttu-id="cc052-186">Minden egyéb eszköz/SDK használatakor használja hello privateKeyPath tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="cc052-186">If you are using any other tool/SDK, use hello privateKeyPath property instead.</span></span> | <span data-ttu-id="cc052-187">Adja meg vagy hello `privateKeyPath` vagy `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="cc052-187">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="cc052-188">hozzáférési kód</span><span class="sxs-lookup"><span data-stu-id="cc052-188">passPhrase</span></span> | <span data-ttu-id="cc052-189">Adja meg hello hozzáférési kódot vagy jelszót toodecrypt hello titkos kulcsot, ha hello kulcsfájl védi egy hozzáférési kódot.</span><span class="sxs-lookup"><span data-stu-id="cc052-189">Specify hello pass phrase/password toodecrypt hello private key if hello key file is protected by a pass phrase.</span></span> | <span data-ttu-id="cc052-190">Igen, ha a hozzáférési kód hello titkos kulcsot tartalmazó fájlt védi.</span><span class="sxs-lookup"><span data-stu-id="cc052-190">Yes if hello private key file is protected by a pass phrase.</span></span> |

> [!NOTE]
> <span data-ttu-id="cc052-191">SFTP-összekötő csak támogatja a protokoll OpenSSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cc052-191">SFTP connector only support OpenSSH key.</span></span> <span data-ttu-id="cc052-192">Ellenőrizze, hogy a kulcsfájl hello megfelelő formátumban van.</span><span class="sxs-lookup"><span data-stu-id="cc052-192">Make sure your key file is in hello proper format.</span></span> <span data-ttu-id="cc052-193">Putty eszközt tooconvert .ppk tooOpenSSH formátumból is használhatja.</span><span class="sxs-lookup"><span data-stu-id="cc052-193">You can use Putty tool tooconvert from .ppk tooOpenSSH format.</span></span>

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a><span data-ttu-id="cc052-194">Példa: Az SshPublicKey hitelesítés használata a titkos kulcs fájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="cc052-194">Example: SshPublicKey authentication using private key filePath</span></span>

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="cc052-195">Példa: Az SshPublicKey hitelesítés használata a titkos kulcs tartalmát</span><span class="sxs-lookup"><span data-stu-id="cc052-195">Example: SshPublicKey authentication using private key content</span></span>

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
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="cc052-196">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="cc052-196">Dataset properties</span></span>
<span data-ttu-id="cc052-197">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc052-197">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="cc052-198">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="cc052-198">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="cc052-199">Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="cc052-199">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="cc052-200">Adott toohello adathalmaztípushoz információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="cc052-200">It provides information that is specific toohello dataset type.</span></span> <span data-ttu-id="cc052-201">hello typeProperties szakasz egy adatkészlet típusú **fájlmegosztási** dataset adatkészletben hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="cc052-201">hello typeProperties section for a dataset of type **FileShare** dataset has hello following properties:</span></span>

| <span data-ttu-id="cc052-202">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cc052-202">Property</span></span> | <span data-ttu-id="cc052-203">Leírás</span><span class="sxs-lookup"><span data-stu-id="cc052-203">Description</span></span> | <span data-ttu-id="cc052-204">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cc052-204">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc052-205">folderPath</span><span class="sxs-lookup"><span data-stu-id="cc052-205">folderPath</span></span> |<span data-ttu-id="cc052-206">Elérési út toohello almappa.</span><span class="sxs-lookup"><span data-stu-id="cc052-206">Sub path toohello folder.</span></span> <span data-ttu-id="cc052-207">Használja az escape-karakter "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="cc052-207">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="cc052-208">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="cc052-208">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="cc052-209">Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="cc052-209">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="cc052-210">Igen</span><span class="sxs-lookup"><span data-stu-id="cc052-210">Yes</span></span> |
| <span data-ttu-id="cc052-211">fileName</span><span class="sxs-lookup"><span data-stu-id="cc052-211">fileName</span></span> |<span data-ttu-id="cc052-212">Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában.</span><span class="sxs-lookup"><span data-stu-id="cc052-212">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="cc052-213">Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="cc052-213">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="cc052-214">Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne:</span><span class="sxs-lookup"><span data-stu-id="cc052-214">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="cc052-215">Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="cc052-215">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="cc052-216">Nem</span><span class="sxs-lookup"><span data-stu-id="cc052-216">No</span></span> |
| <span data-ttu-id="cc052-217">fileFilter</span><span class="sxs-lookup"><span data-stu-id="cc052-217">fileFilter</span></span> |<span data-ttu-id="cc052-218">Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét.</span><span class="sxs-lookup"><span data-stu-id="cc052-218">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="cc052-219">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="cc052-219">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="cc052-220">1. példa:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="cc052-220">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="cc052-221">2. példa:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="cc052-221">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="cc052-222">fileFilter is alkalmazható egy bemeneti fájlmegosztási az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="cc052-222">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="cc052-223">Ez a tulajdonság a HDFS nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="cc052-223">This property is not supported with HDFS.</span></span> |<span data-ttu-id="cc052-224">Nem</span><span class="sxs-lookup"><span data-stu-id="cc052-224">No</span></span> |
| <span data-ttu-id="cc052-225">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="cc052-225">partitionedBy</span></span> |<span data-ttu-id="cc052-226">partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét.</span><span class="sxs-lookup"><span data-stu-id="cc052-226">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="cc052-227">Például folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="cc052-227">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="cc052-228">Nem</span><span class="sxs-lookup"><span data-stu-id="cc052-228">No</span></span> |
| <span data-ttu-id="cc052-229">Formátumban</span><span class="sxs-lookup"><span data-stu-id="cc052-229">format</span></span> | <span data-ttu-id="cc052-230">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="cc052-230">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="cc052-231">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="cc052-231">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="cc052-232">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="cc052-232">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="cc052-233">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="cc052-233">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="cc052-234">Nem</span><span class="sxs-lookup"><span data-stu-id="cc052-234">No</span></span> |
| <span data-ttu-id="cc052-235">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc052-235">compression</span></span> | <span data-ttu-id="cc052-236">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="cc052-236">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="cc052-237">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="cc052-237">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="cc052-238">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="cc052-238">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="cc052-239">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="cc052-239">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="cc052-240">Nem</span><span class="sxs-lookup"><span data-stu-id="cc052-240">No</span></span> |
| <span data-ttu-id="cc052-241">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="cc052-241">useBinaryTransfer</span></span> |<span data-ttu-id="cc052-242">Adja meg, hogy a bináris átviteli mód használata.</span><span class="sxs-lookup"><span data-stu-id="cc052-242">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="cc052-243">A bináris mód és a hamis értéket ASCII igaz.</span><span class="sxs-lookup"><span data-stu-id="cc052-243">True for binary mode and false ASCII.</span></span> <span data-ttu-id="cc052-244">Alapértelmezett érték: igaz.</span><span class="sxs-lookup"><span data-stu-id="cc052-244">Default value: True.</span></span> <span data-ttu-id="cc052-245">A tulajdonság csak akkor használható, típusú társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="cc052-245">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="cc052-246">Nem</span><span class="sxs-lookup"><span data-stu-id="cc052-246">No</span></span> |

> [!NOTE]
> <span data-ttu-id="cc052-247">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="cc052-247">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="cc052-248">PartionedBy tulajdonság használatával</span><span class="sxs-lookup"><span data-stu-id="cc052-248">Using partionedBy property</span></span>
<span data-ttu-id="cc052-249">Hello előző szakaszban említett, megadhat egy dinamikus folderPath idő adatsorozat adatok partitionedBy fájlnevét.</span><span class="sxs-lookup"><span data-stu-id="cc052-249">As mentioned in hello previous section, you can specify a dynamic folderPath, filename for time series data with partitionedBy.</span></span> <span data-ttu-id="cc052-250">Megteheti hello adat-előállító makrók és hello rendszerváltozó SliceStart, hello jelző logikai időszakban egy adott adatszelet a SliceEnd.</span><span class="sxs-lookup"><span data-stu-id="cc052-250">You can do so with hello Data Factory macros and hello system variable SliceStart, SliceEnd that indicate hello logical time period for a given data slice.</span></span>

<span data-ttu-id="cc052-251">toolearn idő adatsorozat adatkészleteket, ütemezés és szeletek, lásd: [létrehozása adatkészletek](data-factory-create-datasets.md), [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md), és [létrehozása folyamatok](data-factory-create-pipelines.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="cc052-251">toolearn about time series datasets, scheduling, and slices, See [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="cc052-252">1. példa:</span><span class="sxs-lookup"><span data-stu-id="cc052-252">Sample 1:</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="cc052-253">Ebben a példában {szelet} hello adat-előállító rendszer változó SliceStart hello formátumban (YYYYMMDDHH) megadott érték helyére.</span><span class="sxs-lookup"><span data-stu-id="cc052-253">In this example {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="cc052-254">hello SliceStart toostart idő hello szelet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc052-254">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="cc052-255">hello folderPath nem azonos az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="cc052-255">hello folderPath is different for each slice.</span></span> <span data-ttu-id="cc052-256">Példa: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="cc052-256">Example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="cc052-257">2. példa:</span><span class="sxs-lookup"><span data-stu-id="cc052-257">Sample 2:</span></span>

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
<span data-ttu-id="cc052-258">Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek folderPath és a fájlnév tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc052-258">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="cc052-259">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="cc052-259">Copy activity properties</span></span>
<span data-ttu-id="cc052-260">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="cc052-260">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="cc052-261">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="cc052-261">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="cc052-262">Mivel a hello hello tevékenység részében typeProperties elérhető hello tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="cc052-262">Whereas, hello properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="cc052-263">A másolási tevékenység során hello típustulajdonságokat hello típusú források és mosdók függenek.</span><span class="sxs-lookup"><span data-stu-id="cc052-263">For Copy activity, hello type properties vary depending on hello types of sources and sinks.</span></span>

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="cc052-264">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="cc052-264">Supported file and compression formats</span></span>
<span data-ttu-id="cc052-265">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.</span><span class="sxs-lookup"><span data-stu-id="cc052-265">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-sftp-server-tooazure-blob"></a><span data-ttu-id="cc052-266">JSON-NÁ. példa: Adatok másolása az SFTP server tooAzure blobból</span><span class="sxs-lookup"><span data-stu-id="cc052-266">JSON Example: Copy data from SFTP server tooAzure blob</span></span>
<span data-ttu-id="cc052-267">hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="cc052-267">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="cc052-268">Azok bemutatják, hogyan toocopy SFTP az adatforrás-tooAzure Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="cc052-268">They show how toocopy data from SFTP source tooAzure Blob Storage.</span></span> <span data-ttu-id="cc052-269">Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="cc052-269">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc052-270">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="cc052-270">This sample provides JSON snippets.</span></span> <span data-ttu-id="cc052-271">Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="cc052-271">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="cc052-272">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="cc052-272">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="cc052-273">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="cc052-273">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="cc052-274">A társított szolgáltatás típusa [sftp](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="cc052-274">A linked service of type [sftp](#linked-service-properties).</span></span>
* <span data-ttu-id="cc052-275">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="cc052-275">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="cc052-276">Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="cc052-276">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="cc052-277">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="cc052-277">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="cc052-278">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="cc052-278">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="cc052-279">hello minta másol adatokat az SFTP server tooan Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="cc052-279">hello sample copies data from an SFTP server tooan Azure blob every hour.</span></span> <span data-ttu-id="cc052-280">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="cc052-280">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="cc052-281">**Kapcsolódó SFTP szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="cc052-281">**SFTP linked service**</span></span>

<span data-ttu-id="cc052-282">Ebben a példában a felhasználó nevét és a jelszót egyszerű szövegként hello egyszerű hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="cc052-282">This example uses hello basic authentication with user name and password in plain text.</span></span> <span data-ttu-id="cc052-283">A következő módokon hello egyikét is használhatja:</span><span class="sxs-lookup"><span data-stu-id="cc052-283">You can also use one of hello following ways:</span></span>

* <span data-ttu-id="cc052-284">Egyszerű hitelesítés titkosított hitelesítő adatokkal</span><span class="sxs-lookup"><span data-stu-id="cc052-284">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="cc052-285">SSH nyilvános kulcsos hitelesítés</span><span class="sxs-lookup"><span data-stu-id="cc052-285">SSH public key authentication</span></span>

<span data-ttu-id="cc052-286">Lásd: [FTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="cc052-286">See [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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
<span data-ttu-id="cc052-287">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="cc052-287">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="cc052-288">**SFTP bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="cc052-288">**SFTP input dataset**</span></span>

<span data-ttu-id="cc052-289">Ez az adatkészlet hivatkozik toohello SFTP mappa `mysharedfolder` és fájl `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="cc052-289">This dataset refers toohello SFTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="cc052-290">hello csővezeték hello fájl toohello cél másolja.</span><span class="sxs-lookup"><span data-stu-id="cc052-290">hello pipeline copies hello file toohello destination.</span></span>

<span data-ttu-id="cc052-291">"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="cc052-291">Setting "external": "true" informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="cc052-292">**Az Azure Blob kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="cc052-292">**Azure Blob output dataset**</span></span>

<span data-ttu-id="cc052-293">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="cc052-293">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="cc052-294">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="cc052-294">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="cc052-295">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="cc052-295">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="cc052-296">**A másolási tevékenység-feldolgozási folyamat**</span><span class="sxs-lookup"><span data-stu-id="cc052-296">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="cc052-297">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="cc052-297">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="cc052-298">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="cc052-298">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource** and **sink** type is set too**BlobSink**.</span></span>

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

## <a name="performance-and-tuning"></a><span data-ttu-id="cc052-299">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="cc052-299">Performance and Tuning</span></span>
<span data-ttu-id="cc052-300">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="cc052-300">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc052-301">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc052-301">Next Steps</span></span>
<span data-ttu-id="cc052-302">Tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="cc052-302">See hello following articles:</span></span>

* <span data-ttu-id="cc052-303">[Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="cc052-303">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
