---
title: az Azure Table storage aaaOverview |} Microsoft Docs
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/23/2017
ms.author: mimig
ms.openlocfilehash: 8643cc666f000e078644981381a086e1ccc44067
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-table-storage-overview"></a><span data-ttu-id="6019c-103">Az Azure Table Storage áttekintése</span><span class="sxs-lookup"><span data-stu-id="6019c-103">Azure Table storage overview</span></span>

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="6019c-104">Azure Table storage egy olyan szolgáltatás, amely strukturált NoSQL hello felhőben biztosít egy kulcsattribútum adattároló séma nélküli kialakítás tárolja.</span><span class="sxs-lookup"><span data-stu-id="6019c-104">Azure Table storage is a service that stores structured NoSQL data in hello cloud, providing a key/attribute store with a schemaless design.</span></span> <span data-ttu-id="6019c-105">Mivel a Table storage séma nélküli, akkor egyszerűen tooadapt alkalmazás igényeinek változásával kell hello az adatokat.</span><span class="sxs-lookup"><span data-stu-id="6019c-105">Because Table storage is schemaless, it's easy tooadapt your data as hello needs of your application evolve.</span></span> <span data-ttu-id="6019c-106">Hozzáférés tooTable tárolási adatok gyors és költséghatékony, számos különböző típusú alkalmazásokat, és általában kevesebb költséggel jár, mint a hagyományos SQL hasonló adatmennyiséggel.</span><span class="sxs-lookup"><span data-stu-id="6019c-106">Access tooTable storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data.</span></span>

<span data-ttu-id="6019c-107">Webes alkalmazásokhoz, címtárakat, eszközadatokat és egyéb metaadatokat a szolgáltatásnak szüksége van a Table storage toostore rugalmas adatkészleteket például a felhasználói adatok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6019c-107">You can use Table storage toostore flexible datasets like user data for web applications, address books, device information, or other types of metadata your service requires.</span></span> <span data-ttu-id="6019c-108">Korlátlan számú entitást tárolhat egy táblát, és a storage-fiókok tartalmazhat korlátlan számú táblát toohello kapacitásán belül hello storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="6019c-108">You can store any number of entities in a table, and a storage account may contain any number of tables, up toohello capacity limit of hello storage account.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a><span data-ttu-id="6019c-109">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6019c-109">Next steps</span></span>

* <span data-ttu-id="6019c-110">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6019c-110">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

* [<span data-ttu-id="6019c-111">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="6019c-111">Getting Started with Azure Table Storage in .NET</span></span>](table-storage-how-to-use-dotnet.md)

* <span data-ttu-id="6019c-112">Nézet hello tábla szolgáltatás elérhető API-kat vonatkozó dokumentációját:</span><span class="sxs-lookup"><span data-stu-id="6019c-112">View hello Table service reference documentation for complete details about available APIs:</span></span>

    * [<span data-ttu-id="6019c-113">Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia</span><span class="sxs-lookup"><span data-stu-id="6019c-113">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

    * [<span data-ttu-id="6019c-114">REST API – referencia</span><span class="sxs-lookup"><span data-stu-id="6019c-114">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
