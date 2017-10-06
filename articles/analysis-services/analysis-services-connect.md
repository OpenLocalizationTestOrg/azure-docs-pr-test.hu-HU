---
title: Analysis Services aaaConnect tooAzure |} Microsoft Docs
description: "Ismerje meg, hogyan tooconnect tooand adatokat lekérni az Analysis Services-kiszolgálóhoz, az Azure-ban."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 5df94492feb48034f156b72e83e1009683988fc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-analysis-services-server"></a><span data-ttu-id="71962-103">Csatlakozás tooan Azure Analysis Services-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="71962-103">Connect tooan Azure Analysis Services server</span></span>

<span data-ttu-id="71962-104">Ez a cikk ismerteti a kapcsolódó tooa kiszolgáló adatok modellezési és a felügyeleti alkalmazások, például az SQL Server Management Studio (SSMS) vagy SQL Server Data Tools (SSDT) használatával.</span><span class="sxs-lookup"><span data-stu-id="71962-104">This article describes connecting tooa server by using data modeling and management applications like SQL Server Management Studio (SSMS) or SQL Server Data Tools (SSDT).</span></span> <span data-ttu-id="71962-105">Vagy az ügyfél jelentés az alkalmazások, például a Microsoft Excel-, Power BI Desktop, vagy egyéni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="71962-105">Or, with client reporting applications like Microsoft Excel, Power BI Desktop, or custom applications.</span></span> <span data-ttu-id="71962-106">Kapcsolatok tooAzure Analysis Services HTTPS használatára.</span><span class="sxs-lookup"><span data-stu-id="71962-106">Connections tooAzure Analysis Services use HTTPS.</span></span>

## <a name="client-libraries"></a><span data-ttu-id="71962-107">Klienskódtárak</span><span class="sxs-lookup"><span data-stu-id="71962-107">Client libraries</span></span>
[<span data-ttu-id="71962-108">Hello legújabb klienskódtárak beolvasása</span><span class="sxs-lookup"><span data-stu-id="71962-108">Get hello latest Client libraries</span></span>](analysis-services-data-providers.md)

<span data-ttu-id="71962-109">Minden kapcsolatok tooa kiszolgálók, típusa, függetlenül a frissített AMO ADOMD.NET és OLEDB szalagtárak tooconnect tooand ügyféladapter az Analysis Services kiszolgálóval igényelnek.</span><span class="sxs-lookup"><span data-stu-id="71962-109">All connections tooa server, regardless of type, require updated AMO, ADOMD.NET, and OLEDB client libraries tooconnect tooand interface with an Analysis Services server.</span></span> <span data-ttu-id="71962-110">SSMS, a SSDT, az Excel 2016 és a Power bi-ban hello legújabb klienskódtárak van telepítve vagy havi kiadásainak frissítve.</span><span class="sxs-lookup"><span data-stu-id="71962-110">For SSMS, SSDT, Excel 2016, and Power BI, hello latest client libraries are installed or updated with monthly releases.</span></span> <span data-ttu-id="71962-111">Azonban néhány esetben is lehet az alkalmazás nem rendelkezhet hello legújabb.</span><span class="sxs-lookup"><span data-stu-id="71962-111">However, in some cases, it's possible an application may not have hello latest.</span></span> <span data-ttu-id="71962-112">Például amikor frissíti a házirendek késleltetést vagy az Office 365 frissítések esetén hello késleltetett csatorna.</span><span class="sxs-lookup"><span data-stu-id="71962-112">For example, when policies delay updates, or Office 365 updates are on hello Deferred Channel.</span></span>

## <a name="server-name"></a><span data-ttu-id="71962-113">Kiszolgálónév</span><span class="sxs-lookup"><span data-stu-id="71962-113">Server name</span></span>

<span data-ttu-id="71962-114">Egy Analysis Services-kiszolgálóhoz az Azure-ban létrehozásakor meg kell adnia egy egyedi nevet és hello terület, ahol hello server létrehozott toobe.</span><span class="sxs-lookup"><span data-stu-id="71962-114">When you create an Analysis Services server in Azure, you specify a unique name and hello region where hello server is toobe created.</span></span> <span data-ttu-id="71962-115">Hello kiszolgálónév megadása a kapcsolatot, hello server elnevezési rendszer esetén:</span><span class="sxs-lookup"><span data-stu-id="71962-115">When specifying hello server name in a connection, hello server naming scheme is:</span></span>

```
<protocol>://<region>/<servername>
```
 <span data-ttu-id="71962-116">Ahol-jére protokoll **asazure**, régió hello Uri, amelyen létrehozták hello server (például westus.asazure.windows.net) és kiszolgálónév hello régión belül egyedi kiszolgálóját hello neve.</span><span class="sxs-lookup"><span data-stu-id="71962-116">Where protocol is string **asazure**, region is hello Uri where hello server was created (for example, westus.asazure.windows.net) and servername is hello name of your unique server within hello region.</span></span>

### <a name="get-hello-server-name"></a><span data-ttu-id="71962-117">Hello kiszolgáló nevének beolvasása</span><span class="sxs-lookup"><span data-stu-id="71962-117">Get hello server name</span></span>
<span data-ttu-id="71962-118">A **Azure-portálon** > server > **áttekintése** > **kiszolgálónév**, másolása hello teljes kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="71962-118">In **Azure portal** > server > **Overview** > **Server name**, copy hello entire server name.</span></span> <span data-ttu-id="71962-119">Ha a munkahely más felhasználóinak túl toothis kiszolgáló kapcsolódik, ezt a kiszolgálónevet megoszthatja velük.</span><span class="sxs-lookup"><span data-stu-id="71962-119">If other users in your organization are connecting toothis server too, you can share this server name with them.</span></span> <span data-ttu-id="71962-120">Ha a kiszolgáló nevét, hello teljes elérési útját kell használni.</span><span class="sxs-lookup"><span data-stu-id="71962-120">When specifying a server name, hello entire path must be used.</span></span>

![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a><span data-ttu-id="71962-122">Kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="71962-122">Connection string</span></span>

<span data-ttu-id="71962-123">TooAzure kapcsolódás esetén az Analysis Services használata hello táblázatos objektummodell, a következő kapcsolati karakterlánc-formátumairól használata hello:</span><span class="sxs-lookup"><span data-stu-id="71962-123">When connecting tooAzure Analysis Services using hello Tabular Object Model, use hello following connection string formats:</span></span>

###### <a name="integrated-azure-active-directory-authentication"></a><span data-ttu-id="71962-124">Integrált Azure Active Directory-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="71962-124">Integrated Azure Active Directory authentication</span></span>
<span data-ttu-id="71962-125">Integrált hitelesítés szerzi be hello Azure Active Directory hitelesítő adatok gyorsítótárában ha rendelkezésre áll.</span><span class="sxs-lookup"><span data-stu-id="71962-125">Integrated authentication picks up hello Azure Active Directory credential cache if available.</span></span> <span data-ttu-id="71962-126">Ha nem, hello Azure bejelentkezési ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="71962-126">If not, hello Azure login window is shown.</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a><span data-ttu-id="71962-127">Az Azure Active Directory authentication felhasználónévvel és jelszóval</span><span class="sxs-lookup"><span data-stu-id="71962-127">Azure Active Directory authentication with username and password</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a><span data-ttu-id="71962-128">Windows-hitelesítés (integrált biztonsági)</span><span class="sxs-lookup"><span data-stu-id="71962-128">Windows authentication (Integrated security)</span></span>
<span data-ttu-id="71962-129">Hello aktuális folyamatának futtatása hello Windows-fiókot használni.</span><span class="sxs-lookup"><span data-stu-id="71962-129">Use hello Windows account running hello current process.</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a><span data-ttu-id="71962-130">Csatlakozás .odc fájl használatával</span><span class="sxs-lookup"><span data-stu-id="71962-130">Connect using an .odc file</span></span>
<span data-ttu-id="71962-131">Az Excel régebbi verzióival felhasználók Office Data Connection (.odc) fájl használatával is elérheti tooan Azure Analysis Services-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="71962-131">With older versions of Excel, users can connect tooan Azure Analysis Services server by using an Office Data Connection (.odc) file.</span></span> <span data-ttu-id="71962-132">több, lásd: toolearn [Office Data Connection (.odc) fájl létrehozása](analysis-services-odc.md).</span><span class="sxs-lookup"><span data-stu-id="71962-132">toolearn more, see [Create an Office Data Connection (.odc) file](analysis-services-odc.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="71962-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71962-133">Next steps</span></span>
<span data-ttu-id="71962-134">[Csatlakozás az Excel használatával](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="71962-134">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
<span data-ttu-id="71962-135">[Csatlakozás a Power BI](analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="71962-135">[Connect with Power BI](analysis-services-connect-pbi.md) </span></span>  
[<span data-ttu-id="71962-136">A kiszolgáló kezelése</span><span class="sxs-lookup"><span data-stu-id="71962-136">Manage your server</span></span>](analysis-services-manage.md)   

