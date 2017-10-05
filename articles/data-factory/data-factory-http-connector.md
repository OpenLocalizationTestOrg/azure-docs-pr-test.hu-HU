---
title: "Adatok áthelyezése forrásból HTTP - Azure |} Microsoft Docs"
description: "További tudnivalók az adatok mozgatása egy helyszíni vagy felhőalapú HTTP forrássá Azure Data Factory használatával."
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
ms.openlocfilehash: 3cc1bd293868b0bb093f617ac12e16c26780fc89
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a><span data-ttu-id="90d21-103">Adatok áthelyezése az Azure Data Factory használatával HTTP forrásból származó</span><span class="sxs-lookup"><span data-stu-id="90d21-103">Move data from an HTTP source using Azure Data Factory</span></span>
<span data-ttu-id="90d21-104">Ez a cikk ismerteti a másolási tevékenység használata az Azure Data Factory tárolt adatok mozgatása egy helyszíni/felhőbeli HTTP-végpont támogatott fogadó adattárat.</span><span class="sxs-lookup"><span data-stu-id="90d21-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from an on-premises/cloud HTTP endpoint to a supported sink data store.</span></span> <span data-ttu-id="90d21-105">Ez a cikk épít, a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely megadja az adatmozgás általános áttekintést a másolási tevékenység és a támogatott adatforrások/mosdók adattárolókhoz listáját.</span><span class="sxs-lookup"><span data-stu-id="90d21-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="90d21-106">Adat-előállító jelenleg támogatja egyéb adattárakhoz HTTP forrásból származó csak áthelyezése adatokat, de nem az adatok áthelyezése más adatok egy HTTP-helyen tárolja.</span><span class="sxs-lookup"><span data-stu-id="90d21-106">Data factory currently supports only moving data from an HTTP source to other data stores, but not moving data from other data stores to an HTTP destination.</span></span>

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="90d21-107">Támogatott esetek és hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="90d21-107">Supported scenarios and authentication types</span></span>
<span data-ttu-id="90d21-108">A HTTP-összekötő segítségével adatokat lekérnie **felhő- és a helyszíni HTTP/s-végpont** HTTP-n keresztül **beolvasása** vagy **POST** metódust.</span><span class="sxs-lookup"><span data-stu-id="90d21-108">You can use this HTTP connector to retrieve data from **both cloud and on-premises HTTP/s endpoint** by using HTTP **GET** or **POST** method.</span></span> <span data-ttu-id="90d21-109">A következő hitelesítési típusok támogatottak: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, és **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="90d21-109">The following authentication types are supported: **Anonymous**, **Basic**, **Digest**, **Windows**, and **ClientCertificate**.</span></span> <span data-ttu-id="90d21-110">Jegyezze fel ezt az összekötőt különbségének és a [webes tábla összekötő](data-factory-web-table-connector.md) van: tábla tartalma kibontani HTML weblap használt.</span><span class="sxs-lookup"><span data-stu-id="90d21-110">Note the difference between this connector and the [Web table connector](data-factory-web-table-connector.md) is: the latter is used to extract table content from web HTML page.</span></span>

<span data-ttu-id="90d21-111">Amikor adatokat másol egy helyszíni HTTP-végpont, a helyszíni környezetben vagy az Azure virtuális gép adatkezelési átjárót kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="90d21-111">When copying data from an on-premises HTTP endpoint, you need install a Data Management Gateway in the on-premises environment/Azure VM.</span></span> <span data-ttu-id="90d21-112">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikkben tájékozódhat az adatkezelési átjáró és az átjáró beállításával kapcsolatos részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="90d21-112">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="90d21-113">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="90d21-113">Getting started</span></span>
<span data-ttu-id="90d21-114">A másolási tevékenység, amely HTTP forrásból származó adatokat különböző eszközök/API-k használatával helyezi át a feldolgozási sor hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="90d21-114">You can create a pipeline with a copy activity that moves data from an HTTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="90d21-115">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="90d21-115">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="90d21-116">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="90d21-116">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

- <span data-ttu-id="90d21-117">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="90d21-117">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="90d21-118">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="90d21-118">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> <span data-ttu-id="90d21-119">A HTTP-adatforrás adatainak másolása Azure Blob Storage minták JSON, lásd: [JSON példák](#json-examples) Ez a cikk szakasza.</span><span class="sxs-lookup"><span data-stu-id="90d21-119">For JSON samples to copy data from HTTP source to Azure Blob Storage, see [JSON examples](#json-examples) section of this articles.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="90d21-120">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="90d21-120">Linked service properties</span></span>
<span data-ttu-id="90d21-121">A következő táblázat a társított szolgáltatás JSON-elemek szerepelnek HTTP jellemző leírást.</span><span class="sxs-lookup"><span data-stu-id="90d21-121">The following table provides description for JSON elements specific to HTTP linked service.</span></span>

| <span data-ttu-id="90d21-122">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="90d21-122">Property</span></span> | <span data-ttu-id="90d21-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="90d21-123">Description</span></span> | <span data-ttu-id="90d21-124">Szükséges</span><span class="sxs-lookup"><span data-stu-id="90d21-124">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="90d21-125">type</span><span class="sxs-lookup"><span data-stu-id="90d21-125">type</span></span> | <span data-ttu-id="90d21-126">A type tulajdonságot kell beállítani: `Http`.</span><span class="sxs-lookup"><span data-stu-id="90d21-126">The type property must be set to: `Http`.</span></span> | <span data-ttu-id="90d21-127">Igen</span><span class="sxs-lookup"><span data-stu-id="90d21-127">Yes</span></span> |
| <span data-ttu-id="90d21-128">URL-címe</span><span class="sxs-lookup"><span data-stu-id="90d21-128">url</span></span> | <span data-ttu-id="90d21-129">A webkiszolgáló alap URL-címe</span><span class="sxs-lookup"><span data-stu-id="90d21-129">Base URL to the Web Server</span></span> | <span data-ttu-id="90d21-130">Igen</span><span class="sxs-lookup"><span data-stu-id="90d21-130">Yes</span></span> |
| <span data-ttu-id="90d21-131">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="90d21-131">authenticationType</span></span> | <span data-ttu-id="90d21-132">Megadja a hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="90d21-132">Specifies the authentication type.</span></span> <span data-ttu-id="90d21-133">Két érték engedélyezett: **névtelen**, **alapvető**, **kivonatoló**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="90d21-133">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="90d21-134">Tekintse meg a további tulajdonságok és adott hitelesítési típusok JSON-példák a táblázat alatti részek kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="90d21-134">Refer to sections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="90d21-135">Igen</span><span class="sxs-lookup"><span data-stu-id="90d21-135">Yes</span></span> |
| <span data-ttu-id="90d21-136">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="90d21-136">enableServerCertificateValidation</span></span> | <span data-ttu-id="90d21-137">Adja meg, hogy a kiszolgálói SSL-tanúsítvány hitelesítése engedélyezése, ha a forrás HTTPS webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="90d21-137">Specify whether to enable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="90d21-138">Nem, alapértelmezett érték true</span><span class="sxs-lookup"><span data-stu-id="90d21-138">No, default is true</span></span> |
| <span data-ttu-id="90d21-139">gatewayName</span><span class="sxs-lookup"><span data-stu-id="90d21-139">gatewayName</span></span> | <span data-ttu-id="90d21-140">Neve az adatkezelési átjáró HTTP a helyszíni adatforráshoz kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="90d21-140">Name of the Data Management Gateway to connect to an on-premises HTTP source.</span></span> | <span data-ttu-id="90d21-141">Igen, ha a helyszíni HTTP forrásból származó adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="90d21-141">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="90d21-142">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="90d21-142">encryptedCredential</span></span> | <span data-ttu-id="90d21-143">Titkosított hitelesítő adatokat a HTTP-végpont elérésére.</span><span class="sxs-lookup"><span data-stu-id="90d21-143">Encrypted credential to access the HTTP endpoint.</span></span> <span data-ttu-id="90d21-144">Automatikusan létrehozott másolása varázsló vagy a ClickOnce felugró párbeszédpanel a hitelesítő adatok konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="90d21-144">Auto-generated when you configure the authentication information in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="90d21-145">Nem.</span><span class="sxs-lookup"><span data-stu-id="90d21-145">No.</span></span> <span data-ttu-id="90d21-146">Csak akkor, ha az adatok másolása helyi HTTP-kiszolgáló alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="90d21-146">Apply only when copying data from an on-premises HTTP server.</span></span> |

<span data-ttu-id="90d21-147">Lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró a felhő közötti](data-factory-move-data-between-onprem-and-cloud.md) helyszíni HTTP összekötő adatforráshoz tartozó hitelesítő adatok beállítása vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="90d21-147">See [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for details about setting credentials for on-premises HTTP connector data source.</span></span>

### <a name="using-basic-digest-or-windows-authentication"></a><span data-ttu-id="90d21-148">Basic, a kivonatoló vagy a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="90d21-148">Using Basic, Digest, or Windows authentication</span></span>

<span data-ttu-id="90d21-149">Állítsa be `authenticationType` , `Basic`, `Digest`, vagy `Windows`, és adja meg a HTTP-összekötő fent bevezetett általános ők mellett az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="90d21-149">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="90d21-150">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="90d21-150">Property</span></span> | <span data-ttu-id="90d21-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="90d21-151">Description</span></span> | <span data-ttu-id="90d21-152">Szükséges</span><span class="sxs-lookup"><span data-stu-id="90d21-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="90d21-153">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="90d21-153">username</span></span> | <span data-ttu-id="90d21-154">Felhasználónév a HTTP-végpont elérésére.</span><span class="sxs-lookup"><span data-stu-id="90d21-154">Username to access the HTTP endpoint.</span></span> | <span data-ttu-id="90d21-155">Igen</span><span class="sxs-lookup"><span data-stu-id="90d21-155">Yes</span></span> |
| <span data-ttu-id="90d21-156">jelszó</span><span class="sxs-lookup"><span data-stu-id="90d21-156">password</span></span> | <span data-ttu-id="90d21-157">A felhasználó (felhasználónév) jelszavát.</span><span class="sxs-lookup"><span data-stu-id="90d21-157">Password for the user (username).</span></span> | <span data-ttu-id="90d21-158">Igen</span><span class="sxs-lookup"><span data-stu-id="90d21-158">Yes</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="90d21-159">Példa: Basic, a kivonatoló vagy a Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="90d21-159">Example: using Basic, Digest, or Windows authentication</span></span>

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

### <a name="using-clientcertificate-authentication"></a><span data-ttu-id="90d21-160">ClientCertificate hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="90d21-160">Using ClientCertificate authentication</span></span>

<span data-ttu-id="90d21-161">Egyszerű hitelesítést használ, állítsa be `authenticationType` , `ClientCertificate`, és adja meg a HTTP-összekötő fent bevezetett általános ők mellett az alábbi tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="90d21-161">To use basic authentication, set `authenticationType` as `ClientCertificate`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="90d21-162">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="90d21-162">Property</span></span> | <span data-ttu-id="90d21-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="90d21-163">Description</span></span> | <span data-ttu-id="90d21-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="90d21-164">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="90d21-165">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="90d21-165">embeddedCertData</span></span> | <span data-ttu-id="90d21-166">A személyes információcseréhez kapcsolódó (PFX) fájl bináris adatok Base64-kódolású tartalmát.</span><span class="sxs-lookup"><span data-stu-id="90d21-166">The Base64-encoded contents of binary data of the Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="90d21-167">Adja meg a `embeddedCertData` vagy `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="90d21-167">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="90d21-168">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="90d21-168">certThumbprint</span></span> | <span data-ttu-id="90d21-169">A tanúsítványtároló átjáró számítógépre telepített tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="90d21-169">The thumbprint of the certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="90d21-170">Csak akkor, ha a helyszíni HTTP forrásból származó adat másolása alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="90d21-170">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="90d21-171">Adja meg a `embeddedCertData` vagy `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="90d21-171">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="90d21-172">jelszó</span><span class="sxs-lookup"><span data-stu-id="90d21-172">password</span></span> | <span data-ttu-id="90d21-173">A tanúsítványhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="90d21-173">Password associated with the certificate.</span></span> | <span data-ttu-id="90d21-174">Nem</span><span class="sxs-lookup"><span data-stu-id="90d21-174">No</span></span> |

<span data-ttu-id="90d21-175">Ha `certThumbprint` hitelesítés és a tanúsítvány telepítése a helyi számítógép személyes tanúsítványokat tartalmazó tárolójában kell az olvasási engedélyt az átjárószolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="90d21-175">If you use `certThumbprint` for authentication and the certificate is installed in the personal store of the local computer, you need to grant the read permission to the gateway service:</span></span>

1. <span data-ttu-id="90d21-176">Indítsa el a Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="90d21-176">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="90d21-177">Adja hozzá a **tanúsítványok** beépülő modul, amelynek célpontja a **helyi számítógép**.</span><span class="sxs-lookup"><span data-stu-id="90d21-177">Add the **Certificates** snap-in that targets the **Local Computer**.</span></span>
2. <span data-ttu-id="90d21-178">Bontsa ki a **tanúsítványok**, **személyes**, és kattintson a **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="90d21-178">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="90d21-179">Kattintson a jobb gombbal a tanúsítványt a személyes tárolóba, és válassza ki **feladataival**->**titkos kulcsok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="90d21-179">Right-click the certificate from the personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="90d21-180">Az a **biztonsági** lapon maradva adja hozzá a felhasználói fiók, amely alatt az adatkezelési átjáró gazdaszolgáltatása fut az olvasási joggal rendelkező tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="90d21-180">On the **Security** tab, add the user account under which Data Management Gateway Host Service is running with the read access to the certificate.</span></span>  

#### <a name="example-using-client-certificate"></a><span data-ttu-id="90d21-181">Példa: ügyfél tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="90d21-181">Example: using client certificate</span></span>
<span data-ttu-id="90d21-182">A kapcsolódó szolgáltatás hivatkozások a data factory egy helyszíni HTTP-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="90d21-182">This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="90d21-183">Az adatkezelési átjáró telepítve a számítógépen telepített ügyféltanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="90d21-183">It uses a client certificate that is installed on the machine with Data Management Gateway installed.</span></span>

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

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="90d21-184">Példa: ügyfél-tanúsítványt használ egy fájlban</span><span class="sxs-lookup"><span data-stu-id="90d21-184">Example: using client certificate in a file</span></span>
<span data-ttu-id="90d21-185">A kapcsolódó szolgáltatás hivatkozások a data factory egy helyszíni HTTP-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="90d21-185">This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="90d21-186">Az adatkezelési átjáró telepítése egy ügyfél tanúsítványfájlt, a gép használ.</span><span class="sxs-lookup"><span data-stu-id="90d21-186">It uses a client certificate file on the machine with Data Management Gateway installed.</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="90d21-187">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="90d21-187">Dataset properties</span></span>
<span data-ttu-id="90d21-188">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="90d21-188">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="90d21-189">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="90d21-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="90d21-190">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="90d21-190">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="90d21-191">A typeProperties szakasz típusú adatkészlet **Http** tulajdonságai a következők</span><span class="sxs-lookup"><span data-stu-id="90d21-191">The typeProperties section for dataset of type **Http** has the following properties</span></span>

| <span data-ttu-id="90d21-192">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="90d21-192">Property</span></span> | <span data-ttu-id="90d21-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="90d21-193">Description</span></span> | <span data-ttu-id="90d21-194">Szükséges</span><span class="sxs-lookup"><span data-stu-id="90d21-194">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="90d21-195">type</span><span class="sxs-lookup"><span data-stu-id="90d21-195">type</span></span> | <span data-ttu-id="90d21-196">A dataset típusának megadása.</span><span class="sxs-lookup"><span data-stu-id="90d21-196">Specified the type of the dataset.</span></span> <span data-ttu-id="90d21-197">meg kell `Http`.</span><span class="sxs-lookup"><span data-stu-id="90d21-197">must be set to `Http`.</span></span> | <span data-ttu-id="90d21-198">Igen</span><span class="sxs-lookup"><span data-stu-id="90d21-198">Yes</span></span> |
| <span data-ttu-id="90d21-199">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="90d21-199">relativeUrl</span></span> | <span data-ttu-id="90d21-200">Az erőforrás adatokat tartalmazó relatív URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="90d21-200">A relative URL to the resource that contains the data.</span></span> <span data-ttu-id="90d21-201">Ha nincs megadva, csak a megadott URL-cím a társított szolgáltatás definíciójának használja.</span><span class="sxs-lookup"><span data-stu-id="90d21-201">When path is not specified, only the URL specified in the linked service definition is used.</span></span> <br><br> <span data-ttu-id="90d21-202">Dinamikus URL-cím létrehozásához használható [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md), pl. "relativeUrl": "$$Text.Format (" / személyes/jelentés? hónap = {0:yyyy}-{0:MM} & fmt = csv', SliceStart) ".</span><span class="sxs-lookup"><span data-stu-id="90d21-202">To construct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), e.g. "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)".</span></span> | <span data-ttu-id="90d21-203">Nem</span><span class="sxs-lookup"><span data-stu-id="90d21-203">No</span></span> |
| <span data-ttu-id="90d21-204">requestMethod</span><span class="sxs-lookup"><span data-stu-id="90d21-204">requestMethod</span></span> | <span data-ttu-id="90d21-205">HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="90d21-205">Http method.</span></span> <span data-ttu-id="90d21-206">Két érték engedélyezett **beolvasása** vagy **POST**.</span><span class="sxs-lookup"><span data-stu-id="90d21-206">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="90d21-207">Nem.</span><span class="sxs-lookup"><span data-stu-id="90d21-207">No.</span></span> <span data-ttu-id="90d21-208">Alapértelmezett érték a `GET`.</span><span class="sxs-lookup"><span data-stu-id="90d21-208">Default is `GET`.</span></span> |
| <span data-ttu-id="90d21-209">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="90d21-209">additionalHeaders</span></span> | <span data-ttu-id="90d21-210">További HTTP-kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="90d21-210">Additional HTTP request headers.</span></span> | <span data-ttu-id="90d21-211">Nem</span><span class="sxs-lookup"><span data-stu-id="90d21-211">No</span></span> |
| <span data-ttu-id="90d21-212">requestBody</span><span class="sxs-lookup"><span data-stu-id="90d21-212">requestBody</span></span> | <span data-ttu-id="90d21-213">A HTTP-kérelmek törzsében.</span><span class="sxs-lookup"><span data-stu-id="90d21-213">Body for HTTP request.</span></span> | <span data-ttu-id="90d21-214">Nem</span><span class="sxs-lookup"><span data-stu-id="90d21-214">No</span></span> |
| <span data-ttu-id="90d21-215">Formátumban</span><span class="sxs-lookup"><span data-stu-id="90d21-215">format</span></span> | <span data-ttu-id="90d21-216">Ha azt szeretné, hogy egyszerűen **lekérik az adatokat, HTTP-végpont-van** nélkül elemzés azt, hagyja ki a formátumot beállítások.</span><span class="sxs-lookup"><span data-stu-id="90d21-216">If you want to simply **retrieve the data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="90d21-217">Ha azt szeretné, a HTTP-válasz tartalom elemzése során másolása, a következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="90d21-217">If you want to parse the HTTP response content during copy, the following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="90d21-218">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="90d21-218">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="90d21-219">Nem</span><span class="sxs-lookup"><span data-stu-id="90d21-219">No</span></span> |
| <span data-ttu-id="90d21-220">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="90d21-220">compression</span></span> | <span data-ttu-id="90d21-221">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="90d21-221">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="90d21-222">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="90d21-222">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="90d21-223">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="90d21-223">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="90d21-224">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="90d21-224">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="90d21-225">Nem</span><span class="sxs-lookup"><span data-stu-id="90d21-225">No</span></span> |

### <a name="example-using-the-get-default-method"></a><span data-ttu-id="90d21-226">Példa: a GET (alapértelmezett) metódussal</span><span class="sxs-lookup"><span data-stu-id="90d21-226">Example: using the GET (default) method</span></span>

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

### <a name="example-using-the-post-method"></a><span data-ttu-id="90d21-227">Példa: a POST metódussal</span><span class="sxs-lookup"><span data-stu-id="90d21-227">Example: using the POST method</span></span>

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

## <a name="copy-activity-properties"></a><span data-ttu-id="90d21-228">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="90d21-228">Copy activity properties</span></span>
<span data-ttu-id="90d21-229">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="90d21-229">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="90d21-230">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="90d21-230">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="90d21-231">Tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység viszont eltérőek a tevékenységek minden típusának.</span><span class="sxs-lookup"><span data-stu-id="90d21-231">Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="90d21-232">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="90d21-232">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="90d21-233">Ha a forrás, a másolási tevékenység jelenleg típusú **HttpSource**, a következő tulajdonságok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="90d21-233">Currently, when the source in copy activity is of type **HttpSource**, the following properties are supported.</span></span>

| <span data-ttu-id="90d21-234">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="90d21-234">Property</span></span> | <span data-ttu-id="90d21-235">Leírás</span><span class="sxs-lookup"><span data-stu-id="90d21-235">Description</span></span> | <span data-ttu-id="90d21-236">Szükséges</span><span class="sxs-lookup"><span data-stu-id="90d21-236">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="90d21-237">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="90d21-237">httpRequestTimeout</span></span> | <span data-ttu-id="90d21-238">Időtúllépés (időtartam) válaszol a HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="90d21-238">The timeout (TimeSpan) for the HTTP request to get a response.</span></span> <span data-ttu-id="90d21-239">Az időtúllépés is válaszol, nem lehet olvasni a válasz adatokat időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="90d21-239">It is the timeout to get a response, not the timeout to read response data.</span></span> | <span data-ttu-id="90d21-240">Nem.</span><span class="sxs-lookup"><span data-stu-id="90d21-240">No.</span></span> <span data-ttu-id="90d21-241">Alapértelmezett érték: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="90d21-241">Default value: 00:01:40</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="90d21-242">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="90d21-242">Supported file and compression formats</span></span>
<span data-ttu-id="90d21-243">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.</span><span class="sxs-lookup"><span data-stu-id="90d21-243">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="90d21-244">JSON-példák</span><span class="sxs-lookup"><span data-stu-id="90d21-244">JSON examples</span></span>
<span data-ttu-id="90d21-245">Az alábbi példában adja meg a minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="90d21-245">The following example provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="90d21-246">HTTP forrásból származó adatok másolása az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="90d21-246">They show how to copy data from HTTP source to Azure Blob Storage.</span></span> <span data-ttu-id="90d21-247">Azonban az adatok átmásolhatók **közvetlenül** a forrásokban, sem a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="90d21-247">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-http-source-to-azure-blob-storage"></a><span data-ttu-id="90d21-248">Példa: Adatok másolása az HTTP-forrás az Azure-Blobtárolóba</span><span class="sxs-lookup"><span data-stu-id="90d21-248">Example: Copy data from HTTP source to Azure Blob Storage</span></span>
<span data-ttu-id="90d21-249">Ez a minta a Data Factory megoldást a következő adat-előállító entitásokat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="90d21-249">The Data Factory solution for this sample contains the following Data Factory entities:</span></span>

1. <span data-ttu-id="90d21-250">A társított szolgáltatás típusa [HTTP](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="90d21-250">A linked service of type [HTTP](#linked-service-properties).</span></span>
2. <span data-ttu-id="90d21-251">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="90d21-251">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="90d21-252">Bemeneti [dataset](data-factory-create-datasets.md) típusú [Http](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="90d21-252">An input [dataset](data-factory-create-datasets.md) of type [Http](#dataset-properties).</span></span>
4. <span data-ttu-id="90d21-253">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="90d21-253">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="90d21-254">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [HttpSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="90d21-254">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [HttpSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="90d21-255">A minta másol adatokat egy HTTP-forrás egy Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="90d21-255">The sample copies data from an HTTP source to an Azure blob every hour.</span></span> <span data-ttu-id="90d21-256">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="90d21-256">The JSON properties used in these samples are described in sections following the samples.</span></span>

### <a name="http-linked-service"></a><span data-ttu-id="90d21-257">Kapcsolódó HTTP-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="90d21-257">HTTP linked service</span></span>
<span data-ttu-id="90d21-258">A példa a kapcsolódó HTTP-szolgáltatás a névtelen hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="90d21-258">This example uses the HTTP linked service with anonymous authentication.</span></span> <span data-ttu-id="90d21-259">Lásd: [HTTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="90d21-259">See [HTTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="90d21-260">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="90d21-260">Azure Storage linked service</span></span>

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

### <a name="http-input-dataset"></a><span data-ttu-id="90d21-261">A bemeneti HTTP-adatkészlet</span><span class="sxs-lookup"><span data-stu-id="90d21-261">HTTP input dataset</span></span>
<span data-ttu-id="90d21-262">Beállítás **külső** való **igaz** tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="90d21-262">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="90d21-263">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="90d21-263">Azure Blob output dataset</span></span>

<span data-ttu-id="90d21-264">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="90d21-264">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

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

### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="90d21-265">A másolási tevékenység-feldolgozási folyamat</span><span class="sxs-lookup"><span data-stu-id="90d21-265">Pipeline with Copy activity</span></span>

<span data-ttu-id="90d21-266">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="90d21-266">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="90d21-267">Az adatcsatorna JSON-definícióból a **forrás** típusúra **HttpSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="90d21-267">In the pipeline JSON definition, the **source** type is set to **HttpSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="90d21-268">Lásd: [HttpSource](#copy-activity-properties) a HttpSource által támogatott tulajdonságok listája.</span><span class="sxs-lookup"><span data-stu-id="90d21-268">See [HttpSource](#copy-activity-properties) for the list of properties supported by the HttpSource.</span></span>

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
        "description": "Copy from an HTTP source to an Azure blob",
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
> <span data-ttu-id="90d21-269">Képezze le a fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="90d21-269">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="90d21-270">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="90d21-270">Performance and Tuning</span></span>
<span data-ttu-id="90d21-271">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="90d21-271">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
