---
title: "aaaMove adatforrásból származó egy HTTP - Azure |} Microsoft Docs"
description: "További információk a hogyan toomove egy helyszíni vagy HTTP felhő adatforrás Azure Data Factory használatával."
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
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: e39b9cbff870aef4be91938cacff39a2fd12d64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a><span data-ttu-id="77bf5-103">Adatok áthelyezése az Azure Data Factory használatával HTTP forrásból származó</span><span class="sxs-lookup"><span data-stu-id="77bf5-103">Move data from an HTTP source using Azure Data Factory</span></span>
<span data-ttu-id="77bf5-104">Ez a cikk ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatait egy helyszíni/felhőbeli HTTP-végpont tooa támogatott fogadó adattár.</span><span class="sxs-lookup"><span data-stu-id="77bf5-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises/cloud HTTP endpoint tooa supported sink data store.</span></span> <span data-ttu-id="77bf5-105">Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja a másolási tevékenység és hello listáját támogatott adatforrások/mosdók adattárolókhoz adatmozgás általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="77bf5-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="77bf5-106">Adat-előállító jelenleg csak az adatok áthelyezése HTTP által támogatott forrás tooother adattárolókhoz, de tooan HTTP-cél nem adatok áthelyezését más adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="77bf5-106">Data factory currently supports only moving data from an HTTP source tooother data stores, but not moving data from other data stores tooan HTTP destination.</span></span>

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="77bf5-107">Támogatott esetek és hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="77bf5-107">Supported scenarios and authentication types</span></span>
<span data-ttu-id="77bf5-108">A HTTP összekötő tooretrieve adatait is használhat **felhő- és a helyszíni HTTP/s-végpont** HTTP-n keresztül **beolvasása** vagy **POST** metódus.</span><span class="sxs-lookup"><span data-stu-id="77bf5-108">You can use this HTTP connector tooretrieve data from **both cloud and on-premises HTTP/s endpoint** by using HTTP **GET** or **POST** method.</span></span> <span data-ttu-id="77bf5-109">a következő hitelesítési típusok hello támogatottak: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, és  **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-109">hello following authentication types are supported: **Anonymous**, **Basic**, **Digest**, **Windows**, and **ClientCertificate**.</span></span> <span data-ttu-id="77bf5-110">Megjegyzés: az összekötő és hello hello különbségének [webes tábla összekötő](data-factory-web-table-connector.md) van: Ez utóbbi hello használt tooextract tábla HTML weblapról tartalom.</span><span class="sxs-lookup"><span data-stu-id="77bf5-110">Note hello difference between this connector and hello [Web table connector](data-factory-web-table-connector.md) is: hello latter is used tooextract table content from web HTML page.</span></span>

<span data-ttu-id="77bf5-111">Amikor adatokat másol egy helyszíni HTTP-végpont, hello a helyszíni környezetben vagy az Azure virtuális gép adatkezelési átjárót kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="77bf5-111">When copying data from an on-premises HTTP endpoint, you need install a Data Management Gateway in hello on-premises environment/Azure VM.</span></span> <span data-ttu-id="77bf5-112">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="77bf5-112">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="77bf5-113">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="77bf5-113">Getting started</span></span>
<span data-ttu-id="77bf5-114">A másolási tevékenység, amely HTTP forrásból származó adatokat különböző eszközök/API-k használatával helyezi át a feldolgozási sor hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="77bf5-114">You can create a pipeline with a copy activity that moves data from an HTTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="77bf5-115">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-115">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="77bf5-116">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="77bf5-116">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

- <span data-ttu-id="77bf5-117">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-117">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="77bf5-118">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="77bf5-118">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> <span data-ttu-id="77bf5-119">JSON-minták HTTP forrás tooAzure Blob Storage toocopy adatait, a következő témakörben: [JSON példák](#json-examples) Ez a cikk szakasza.</span><span class="sxs-lookup"><span data-stu-id="77bf5-119">For JSON samples toocopy data from HTTP source tooAzure Blob Storage, see [JSON examples](#json-examples) section of this articles.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="77bf5-120">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="77bf5-120">Linked service properties</span></span>
<span data-ttu-id="77bf5-121">a következő táblázat hello biztosít JSON elemek adott tooHTTP kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="77bf5-121">hello following table provides description for JSON elements specific tooHTTP linked service.</span></span>

| <span data-ttu-id="77bf5-122">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="77bf5-122">Property</span></span> | <span data-ttu-id="77bf5-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="77bf5-123">Description</span></span> | <span data-ttu-id="77bf5-124">Szükséges</span><span class="sxs-lookup"><span data-stu-id="77bf5-124">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="77bf5-125">type</span><span class="sxs-lookup"><span data-stu-id="77bf5-125">type</span></span> | <span data-ttu-id="77bf5-126">hello type tulajdonságot kell beállítani: `Http`.</span><span class="sxs-lookup"><span data-stu-id="77bf5-126">hello type property must be set to: `Http`.</span></span> | <span data-ttu-id="77bf5-127">Igen</span><span class="sxs-lookup"><span data-stu-id="77bf5-127">Yes</span></span> |
| <span data-ttu-id="77bf5-128">URL-címe</span><span class="sxs-lookup"><span data-stu-id="77bf5-128">url</span></span> | <span data-ttu-id="77bf5-129">Alap URL-cím toohello webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="77bf5-129">Base URL toohello Web Server</span></span> | <span data-ttu-id="77bf5-130">Igen</span><span class="sxs-lookup"><span data-stu-id="77bf5-130">Yes</span></span> |
| <span data-ttu-id="77bf5-131">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="77bf5-131">authenticationType</span></span> | <span data-ttu-id="77bf5-132">Hello hitelesítés típusát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="77bf5-132">Specifies hello authentication type.</span></span> <span data-ttu-id="77bf5-133">Két érték engedélyezett: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-133">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="77bf5-134">További tulajdonságokat és JSON-minták a táblázat alatti toosections rendre adott hitelesítési típusok olvassa.</span><span class="sxs-lookup"><span data-stu-id="77bf5-134">Refer toosections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="77bf5-135">Igen</span><span class="sxs-lookup"><span data-stu-id="77bf5-135">Yes</span></span> |
| <span data-ttu-id="77bf5-136">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="77bf5-136">enableServerCertificateValidation</span></span> | <span data-ttu-id="77bf5-137">Adja meg, hogy tooenable server SSL tanúsítvány érvényesítése, ha forrás HTTPS webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="77bf5-137">Specify whether tooenable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="77bf5-138">Nem, alapértelmezett érték true</span><span class="sxs-lookup"><span data-stu-id="77bf5-138">No, default is true</span></span> |
| <span data-ttu-id="77bf5-139">gatewayName</span><span class="sxs-lookup"><span data-stu-id="77bf5-139">gatewayName</span></span> | <span data-ttu-id="77bf5-140">Hello az adatkezelési átjáró tooconnect tooan nevét a helyszíni HTTP-forrás.</span><span class="sxs-lookup"><span data-stu-id="77bf5-140">Name of hello Data Management Gateway tooconnect tooan on-premises HTTP source.</span></span> | <span data-ttu-id="77bf5-141">Igen, ha a helyszíni HTTP forrásból származó adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="77bf5-141">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="77bf5-142">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="77bf5-142">encryptedCredential</span></span> | <span data-ttu-id="77bf5-143">Titkosított hitelesítő adatokban tooaccess hello HTTP-végpont.</span><span class="sxs-lookup"><span data-stu-id="77bf5-143">Encrypted credential tooaccess hello HTTP endpoint.</span></span> <span data-ttu-id="77bf5-144">Automatikusan létrehozott hello hitelesítési adatok másolása varázsló vagy a hello ClickOnce előugró párbeszédpanelen konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="77bf5-144">Auto-generated when you configure hello authentication information in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="77bf5-145">Nem.</span><span class="sxs-lookup"><span data-stu-id="77bf5-145">No.</span></span> <span data-ttu-id="77bf5-146">Csak akkor, ha az adatok másolása helyi HTTP-kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="77bf5-146">Apply only when copying data from an on-premises HTTP server.</span></span> |

<span data-ttu-id="77bf5-147">Lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró hello felhő között](data-factory-move-data-between-onprem-and-cloud.md) helyszíni HTTP összekötő adatforráshoz tartozó hitelesítő adatok beállítása vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="77bf5-147">See [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for details about setting credentials for on-premises HTTP connector data source.</span></span>

### <a name="using-basic-digest-or-windows-authentication"></a><span data-ttu-id="77bf5-148">Basic, a kivonatoló vagy a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="77bf5-148">Using Basic, Digest, or Windows authentication</span></span>

<span data-ttu-id="77bf5-149">Állítsa be `authenticationType` , `Basic`, `Digest`, vagy `Windows`, és adja meg a következő tulajdonságai módosításokon kívül HTTP összekötő általános azokat, a fenti bevezetett hello hello:</span><span class="sxs-lookup"><span data-stu-id="77bf5-149">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="77bf5-150">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="77bf5-150">Property</span></span> | <span data-ttu-id="77bf5-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="77bf5-151">Description</span></span> | <span data-ttu-id="77bf5-152">Szükséges</span><span class="sxs-lookup"><span data-stu-id="77bf5-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="77bf5-153">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="77bf5-153">username</span></span> | <span data-ttu-id="77bf5-154">Felhasználónév tooaccess hello HTTP-végpont.</span><span class="sxs-lookup"><span data-stu-id="77bf5-154">Username tooaccess hello HTTP endpoint.</span></span> | <span data-ttu-id="77bf5-155">Igen</span><span class="sxs-lookup"><span data-stu-id="77bf5-155">Yes</span></span> |
| <span data-ttu-id="77bf5-156">jelszó</span><span class="sxs-lookup"><span data-stu-id="77bf5-156">password</span></span> | <span data-ttu-id="77bf5-157">(Felhasználónév) hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="77bf5-157">Password for hello user (username).</span></span> | <span data-ttu-id="77bf5-158">Igen</span><span class="sxs-lookup"><span data-stu-id="77bf5-158">Yes</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="77bf5-159">Példa: Basic, a kivonatoló vagy a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="77bf5-159">Example: using Basic, Digest, or Windows authentication</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a><span data-ttu-id="77bf5-160">ClientCertificate hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="77bf5-160">Using ClientCertificate authentication</span></span>

<span data-ttu-id="77bf5-161">Alapszintű hitelesítés toouse, állítsa be `authenticationType` , `ClientCertificate`, és adja meg a következő tulajdonságai módosításokon kívül HTTP összekötő általános azokat, a fenti bevezetett hello hello:</span><span class="sxs-lookup"><span data-stu-id="77bf5-161">toouse basic authentication, set `authenticationType` as `ClientCertificate`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="77bf5-162">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="77bf5-162">Property</span></span> | <span data-ttu-id="77bf5-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="77bf5-163">Description</span></span> | <span data-ttu-id="77bf5-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="77bf5-164">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="77bf5-165">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="77bf5-165">embeddedCertData</span></span> | <span data-ttu-id="77bf5-166">hello Base64-kódolású tartalmak a bináris adatok hello személyes információcseréhez kapcsolódó (PFX) fájl.</span><span class="sxs-lookup"><span data-stu-id="77bf5-166">hello Base64-encoded contents of binary data of hello Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="77bf5-167">Adja meg vagy hello `embeddedCertData` vagy `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="77bf5-167">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="77bf5-168">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="77bf5-168">certThumbprint</span></span> | <span data-ttu-id="77bf5-169">hello telepítve lett az átjáró gépen tanúsítványtároló hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="77bf5-169">hello thumbprint of hello certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="77bf5-170">Csak akkor, ha a helyszíni HTTP forrásból származó adat másolása alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="77bf5-170">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="77bf5-171">Adja meg vagy hello `embeddedCertData` vagy `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="77bf5-171">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="77bf5-172">jelszó</span><span class="sxs-lookup"><span data-stu-id="77bf5-172">password</span></span> | <span data-ttu-id="77bf5-173">Hello tanúsítványhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="77bf5-173">Password associated with hello certificate.</span></span> | <span data-ttu-id="77bf5-174">Nem</span><span class="sxs-lookup"><span data-stu-id="77bf5-174">No</span></span> |

<span data-ttu-id="77bf5-175">Ha `certThumbprint` hitelesítési és hello tanúsítvány telepítve van a hello hello helyi számítógép személyes tárolójában, kell toogrant hello olvasási hozzáférést toohello átjáró szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="77bf5-175">If you use `certThumbprint` for authentication and hello certificate is installed in hello personal store of hello local computer, you need toogrant hello read permission toohello gateway service:</span></span>

1. <span data-ttu-id="77bf5-176">Indítsa el a Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="77bf5-176">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="77bf5-177">Adja hozzá a hello **tanúsítványok** beépülő modul a célok hello **helyi számítógép**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-177">Add hello **Certificates** snap-in that targets hello **Local Computer**.</span></span>
2. <span data-ttu-id="77bf5-178">Bontsa ki a **tanúsítványok**, **személyes**, és kattintson a **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-178">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="77bf5-179">Kattintson a jobb gombbal a hello tanúsítványt hello személyes tárolójában, és válassza ki **feladataival**->**titkos kulcsok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="77bf5-179">Right-click hello certificate from hello personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="77bf5-180">A hello **biztonsági** lapon maradva adja hozzá a hello felhasználói fiók alatt az adatkezelési átjáró gazdaszolgáltatása fut. hello olvasási hozzáférés toohello tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="77bf5-180">On hello **Security** tab, add hello user account under which Data Management Gateway Host Service is running with hello read access toohello certificate.</span></span>  

#### <a name="example-using-client-certificate"></a><span data-ttu-id="77bf5-181">Példa: ügyfél tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="77bf5-181">Example: using client certificate</span></span>
<span data-ttu-id="77bf5-182">Ez a data factory tooan helyszíni HTTP webkiszolgáló kapcsolódó szolgáltatás hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="77bf5-182">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="77bf5-183">Az adatkezelési átjáró telepített hello gépen telepített ügyféltanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="77bf5-183">It uses a client certificate that is installed on hello machine with Data Management Gateway installed.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="77bf5-184">Példa: ügyfél-tanúsítványt használ egy fájlban</span><span class="sxs-lookup"><span data-stu-id="77bf5-184">Example: using client certificate in a file</span></span>
<span data-ttu-id="77bf5-185">Ez a data factory tooan helyszíni HTTP webkiszolgáló kapcsolódó szolgáltatás hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="77bf5-185">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="77bf5-186">Az adatkezelési átjáró telepítése egy ügyfél tanúsítványfájl hello gépen használ.</span><span class="sxs-lookup"><span data-stu-id="77bf5-186">It uses a client certificate file on hello machine with Data Management Gateway installed.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="77bf5-187">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="77bf5-187">Dataset properties</span></span>
<span data-ttu-id="77bf5-188">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="77bf5-188">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="77bf5-189">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="77bf5-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="77bf5-190">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="77bf5-190">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="77bf5-191">hello typeProperties szakasz típusú adatkészlet **Http** rendelkezik hello következő tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="77bf5-191">hello typeProperties section for dataset of type **Http** has hello following properties</span></span>

| <span data-ttu-id="77bf5-192">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="77bf5-192">Property</span></span> | <span data-ttu-id="77bf5-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="77bf5-193">Description</span></span> | <span data-ttu-id="77bf5-194">Szükséges</span><span class="sxs-lookup"><span data-stu-id="77bf5-194">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="77bf5-195">type</span><span class="sxs-lookup"><span data-stu-id="77bf5-195">type</span></span> | <span data-ttu-id="77bf5-196">A megadott hello dataset hello típusú.</span><span class="sxs-lookup"><span data-stu-id="77bf5-196">Specified hello type of hello dataset.</span></span> <span data-ttu-id="77bf5-197">be kell állítani túl`Http`.</span><span class="sxs-lookup"><span data-stu-id="77bf5-197">must be set too`Http`.</span></span> | <span data-ttu-id="77bf5-198">Igen</span><span class="sxs-lookup"><span data-stu-id="77bf5-198">Yes</span></span> |
| <span data-ttu-id="77bf5-199">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="77bf5-199">relativeUrl</span></span> | <span data-ttu-id="77bf5-200">Relatív URL-cím toohello erőforrás hello adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="77bf5-200">A relative URL toohello resource that contains hello data.</span></span> <span data-ttu-id="77bf5-201">Ha nincs megadva elérési út, kapcsolódó hello szolgáltatásdefinícióban megadott csak hello URL szolgál.</span><span class="sxs-lookup"><span data-stu-id="77bf5-201">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> <br><br> <span data-ttu-id="77bf5-202">tooconstruct dinamikus URL-címe, használhat [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md), pl. "relativeUrl": "$$Text.Format (" / személyes/jelentés? hónap = {0:yyyy}-{0:MM} & fmt = csv ", SliceStart)".</span><span class="sxs-lookup"><span data-stu-id="77bf5-202">tooconstruct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), e.g. "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)".</span></span> | <span data-ttu-id="77bf5-203">Nem</span><span class="sxs-lookup"><span data-stu-id="77bf5-203">No</span></span> |
| <span data-ttu-id="77bf5-204">requestMethod</span><span class="sxs-lookup"><span data-stu-id="77bf5-204">requestMethod</span></span> | <span data-ttu-id="77bf5-205">HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="77bf5-205">Http method.</span></span> <span data-ttu-id="77bf5-206">Két érték engedélyezett **beolvasása** vagy **POST**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-206">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="77bf5-207">Nem.</span><span class="sxs-lookup"><span data-stu-id="77bf5-207">No.</span></span> <span data-ttu-id="77bf5-208">Alapértelmezett érték a `GET`.</span><span class="sxs-lookup"><span data-stu-id="77bf5-208">Default is `GET`.</span></span> |
| <span data-ttu-id="77bf5-209">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="77bf5-209">additionalHeaders</span></span> | <span data-ttu-id="77bf5-210">További HTTP-kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="77bf5-210">Additional HTTP request headers.</span></span> | <span data-ttu-id="77bf5-211">Nem</span><span class="sxs-lookup"><span data-stu-id="77bf5-211">No</span></span> |
| <span data-ttu-id="77bf5-212">requestBody</span><span class="sxs-lookup"><span data-stu-id="77bf5-212">requestBody</span></span> | <span data-ttu-id="77bf5-213">A HTTP-kérelmek törzsében.</span><span class="sxs-lookup"><span data-stu-id="77bf5-213">Body for HTTP request.</span></span> | <span data-ttu-id="77bf5-214">Nem</span><span class="sxs-lookup"><span data-stu-id="77bf5-214">No</span></span> |
| <span data-ttu-id="77bf5-215">Formátumban</span><span class="sxs-lookup"><span data-stu-id="77bf5-215">format</span></span> | <span data-ttu-id="77bf5-216">Ha azt szeretné, hogy toosimply **hello adatainak lekérése, a HTTP-végpont-van** nélkül elemzés azt, hagyja ki a formátumot beállítások.</span><span class="sxs-lookup"><span data-stu-id="77bf5-216">If you want toosimply **retrieve hello data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="77bf5-217">Ha szeretné tooparse hello HTTP-válasz tartalom másolása során, a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-217">If you want tooparse hello HTTP response content during copy, hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="77bf5-218">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="77bf5-218">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="77bf5-219">Nem</span><span class="sxs-lookup"><span data-stu-id="77bf5-219">No</span></span> |
| <span data-ttu-id="77bf5-220">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="77bf5-220">compression</span></span> | <span data-ttu-id="77bf5-221">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="77bf5-221">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="77bf5-222">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-222">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="77bf5-223">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-223">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="77bf5-224">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="77bf5-224">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="77bf5-225">Nem</span><span class="sxs-lookup"><span data-stu-id="77bf5-225">No</span></span> |

### <a name="example-using-hello-get-default-method"></a><span data-ttu-id="77bf5-226">Példa: hello (alapértelmezett) GET metódussal</span><span class="sxs-lookup"><span data-stu-id="77bf5-226">Example: using hello GET (default) method</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

### <a name="example-using-hello-post-method"></a><span data-ttu-id="77bf5-227">Példa: hello POST metódussal</span><span class="sxs-lookup"><span data-stu-id="77bf5-227">Example: using hello POST method</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a><span data-ttu-id="77bf5-228">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="77bf5-228">Copy activity properties</span></span>
<span data-ttu-id="77bf5-229">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="77bf5-229">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="77bf5-230">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="77bf5-230">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="77bf5-231">Tulajdonságok érhetők el hello **typeProperties** szakasz hello hello tevékenységekre ugyanakkor tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="77bf5-231">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="77bf5-232">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="77bf5-232">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="77bf5-233">Ha a másolási tevékenység hello forrás jelenleg típusú **HttpSource**, a következő tulajdonságok hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="77bf5-233">Currently, when hello source in copy activity is of type **HttpSource**, hello following properties are supported.</span></span>

| <span data-ttu-id="77bf5-234">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="77bf5-234">Property</span></span> | <span data-ttu-id="77bf5-235">Leírás</span><span class="sxs-lookup"><span data-stu-id="77bf5-235">Description</span></span> | <span data-ttu-id="77bf5-236">Szükséges</span><span class="sxs-lookup"><span data-stu-id="77bf5-236">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="77bf5-237">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="77bf5-237">httpRequestTimeout</span></span> | <span data-ttu-id="77bf5-238">hello hello HTTP kérelem tooget választ (időtartam) időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="77bf5-238">hello timeout (TimeSpan) for hello HTTP request tooget a response.</span></span> <span data-ttu-id="77bf5-239">Hello időtúllépés tooget választ, hello időtúllépés tooread érkezett válasz adatait is.</span><span class="sxs-lookup"><span data-stu-id="77bf5-239">It is hello timeout tooget a response, not hello timeout tooread response data.</span></span> | <span data-ttu-id="77bf5-240">Nem.</span><span class="sxs-lookup"><span data-stu-id="77bf5-240">No.</span></span> <span data-ttu-id="77bf5-241">Alapértelmezett érték: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="77bf5-241">Default value: 00:01:40</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="77bf5-242">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="77bf5-242">Supported file and compression formats</span></span>
<span data-ttu-id="77bf5-243">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.</span><span class="sxs-lookup"><span data-stu-id="77bf5-243">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="77bf5-244">JSON-példák</span><span class="sxs-lookup"><span data-stu-id="77bf5-244">JSON examples</span></span>
<span data-ttu-id="77bf5-245">a következő példa hello adja meg a minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="77bf5-245">hello following example provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="77bf5-246">Azok bemutatják, hogyan toocopy HTTP adatforrás tooAzure Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="77bf5-246">They show how toocopy data from HTTP source tooAzure Blob Storage.</span></span> <span data-ttu-id="77bf5-247">Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="77bf5-247">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-http-source-tooazure-blob-storage"></a><span data-ttu-id="77bf5-248">Példa: Adatok másolása az HTTP forrás tooAzure Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="77bf5-248">Example: Copy data from HTTP source tooAzure Blob Storage</span></span>
<span data-ttu-id="77bf5-249">hello Ez a minta Data Factory-megoldást a következő adat-előállító entitások hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="77bf5-249">hello Data Factory solution for this sample contains hello following Data Factory entities:</span></span>

1. <span data-ttu-id="77bf5-250">A társított szolgáltatás típusa [HTTP](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="77bf5-250">A linked service of type [HTTP](#linked-service-properties).</span></span>
2. <span data-ttu-id="77bf5-251">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="77bf5-251">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="77bf5-252">Bemeneti [dataset](data-factory-create-datasets.md) típusú [Http](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="77bf5-252">An input [dataset](data-factory-create-datasets.md) of type [Http](#dataset-properties).</span></span>
4. <span data-ttu-id="77bf5-253">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="77bf5-253">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="77bf5-254">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [HttpSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="77bf5-254">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [HttpSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="77bf5-255">hello minta másol adatokat egy HTTP-forrás tooan Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="77bf5-255">hello sample copies data from an HTTP source tooan Azure blob every hour.</span></span> <span data-ttu-id="77bf5-256">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="77bf5-256">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="http-linked-service"></a><span data-ttu-id="77bf5-257">Kapcsolódó HTTP-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="77bf5-257">HTTP linked service</span></span>
<span data-ttu-id="77bf5-258">Ebben a példában használt hello HTTP társított szolgáltatás névtelen hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="77bf5-258">This example uses hello HTTP linked service with anonymous authentication.</span></span> <span data-ttu-id="77bf5-259">Lásd: [HTTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="77bf5-259">See [HTTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="77bf5-260">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="77bf5-260">Azure Storage linked service</span></span>

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

### <a name="http-input-dataset"></a><span data-ttu-id="77bf5-261">A bemeneti HTTP-adatkészlet</span><span class="sxs-lookup"><span data-stu-id="77bf5-261">HTTP input dataset</span></span>
<span data-ttu-id="77bf5-262">Beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="77bf5-262">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="77bf5-263">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="77bf5-263">Azure Blob output dataset</span></span>

<span data-ttu-id="77bf5-264">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="77bf5-264">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="77bf5-265">A másolási tevékenység-feldolgozási folyamat</span><span class="sxs-lookup"><span data-stu-id="77bf5-265">Pipeline with Copy activity</span></span>

<span data-ttu-id="77bf5-266">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="77bf5-266">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="77bf5-267">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**HttpSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="77bf5-267">In hello pipeline JSON definition, hello **source** type is set too**HttpSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="77bf5-268">Lásd: [HttpSource](#copy-activity-properties) hello HttpSource által támogatott tulajdonságokról hello listáját.</span><span class="sxs-lookup"><span data-stu-id="77bf5-268">See [HttpSource](#copy-activity-properties) for hello list of properties supported by hello HttpSource.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source tooan Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

> [!NOTE]
> <span data-ttu-id="77bf5-269">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="77bf5-269">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="77bf5-270">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="77bf5-270">Performance and Tuning</span></span>
<span data-ttu-id="77bf5-271">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="77bf5-271">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
