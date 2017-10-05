---
title: "Az Azure Data Catalog kibocsátási megjegyzései |} Microsoft Docs"
description: "Kibocsátási megjegyzések az Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d3db9bee0558cac5fb4f5b8fb4ab67a35ce0f141
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-release-notes"></a><span data-ttu-id="e61c5-103">Az Azure Data Catalog kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="e61c5-103">Azure Data Catalog release notes</span></span>
## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e61c5-104">Az Azure Data Catalog kibocsátási megjegyzések a 2015. November 20, a</span><span class="sxs-lookup"><span data-stu-id="e61c5-104">Notes for the November 20, 2015 release of Azure Data Catalog</span></span>
### <a name="opening-data-sources-in-power-bi-desktop"></a><span data-ttu-id="e61c5-105">A Power BI Desktop nyitó adatforrások</span><span class="sxs-lookup"><span data-stu-id="e61c5-105">Opening Data Sources in Power BI Desktop</span></span>
<span data-ttu-id="e61c5-106">Ha az "Nyissa meg a Power BI Desktop" elemet a **Azure Data Catalog** portálon felhasználók szembesülhetnek a Power BI Desktop-kérelemben két problémák egyikét:</span><span class="sxs-lookup"><span data-stu-id="e61c5-106">When using the "Open in Power BI Desktop" option from the **Azure Data Catalog** portal, users may encounter one of two problems in the Power BI Desktop application:</span></span>

* <span data-ttu-id="e61c5-107">A "Nem nyissa meg a dokumentumhoz" címmel párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="e61c5-107">A dialog box with the title "Unable to Open Document" is displayed</span></span>
* <span data-ttu-id="e61c5-108">A Power BI Desktop alkalmazás nyílik meg, de a fájl úgy tűnik, hogy üres</span><span class="sxs-lookup"><span data-stu-id="e61c5-108">The Power BI Desktop application opens, but the file appears to be empty</span></span>

<span data-ttu-id="e61c5-109">Az egyes esetekben a probléma megoldásához érdemes letölti és telepíti a legújabb verzióját a Power BI Desktop [powerbi.com webhelyen](https://powerbi.com).</span><span class="sxs-lookup"><span data-stu-id="e61c5-109">For each situation, the problem can be resolved by downloading and installing the latest version of Power BI Desktop from [PowerBI.com](https://powerbi.com).</span></span>

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e61c5-110">Az Azure Data Catalog kibocsátási megjegyzések a 2015. November 13, a</span><span class="sxs-lookup"><span data-stu-id="e61c5-110">Notes for the November 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-to-teradata"></a><span data-ttu-id="e61c5-111">Regisztrálja, és csatlakozik a teradata rendszerhez</span><span class="sxs-lookup"><span data-stu-id="e61c5-111">Registering and connecting to Teradata</span></span>
<span data-ttu-id="e61c5-112">Amikor Teradata adatforrások felhasználók a megfelelő Teradata ODBC-illesztőprogram van telepítve, amely egyezik a (32 bites vagy 64 bites) a használt szoftver csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="e61c5-112">When connecting to Teradata data sources users must have installed the correct Teradata ODBC driver that match the bitness (32-bit or 64-bit) of the software being used.</span></span>

<span data-ttu-id="e61c5-113">LÉPETT kiadási dátumot, a legutóbbi [Teradata ODBC-illesztőprogram (verzió: 15.10) Windows](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) kompatibilis Office 2013, de nem Office 2016.</span><span class="sxs-lookup"><span data-stu-id="e61c5-113">As of this ADC release date, the most recent [Teradata ODBC driver for windows ( version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) is compatible with Office 2013, but not with Office 2016.</span></span>

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e61c5-114">Az Azure Data Catalog kibocsátási megjegyzések a 2015. július 13 a</span><span class="sxs-lookup"><span data-stu-id="e61c5-114">Notes for the July 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-to-oracle-database"></a><span data-ttu-id="e61c5-115">Regisztráció és Oracle-adatbázishoz szeretne csatlakozni</span><span class="sxs-lookup"><span data-stu-id="e61c5-115">Registering and connecting to Oracle Database</span></span>
<span data-ttu-id="e61c5-116">Amikor a felhasználók a megfelelő Oracle-illesztőprogramokat, amely egyezik a (32 bites vagy 64 bites) a használt a szoftverfrissítések által telepített adatforrások Oracle-adatbázishoz való kapcsolódás.</span><span class="sxs-lookup"><span data-stu-id="e61c5-116">When connecting to Oracle Database data sources users must have installed the correct Oracle drivers that match the bitness (32-bit or 64-bit) of the software being used.</span></span>

* <span data-ttu-id="e61c5-117">32 bites Windows rendszert futtató kiszolgálón tárolt olyan adatforrások Oracle regisztrálásakor a 32 bites Oracle illesztőprogramok fogja használni</span><span class="sxs-lookup"><span data-stu-id="e61c5-117">When registering Oracle data sources on a computer running 32-bit Windows, the 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e61c5-118">64 bites Windows rendszert futtató kiszolgálón tárolt olyan adatforrások Oracle regisztrálásakor a 64 bites Oracle illesztőprogramok fogja használni</span><span class="sxs-lookup"><span data-stu-id="e61c5-118">When registering Oracle data sources on a computer running 64-bit Windows, the 64-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e61c5-119">Kapcsolódás Oracle-adatforrások Excel használatával a Microsoft Office 32 bites verzióját futtató számítógépen, ha a 64 bites Windows esetén, beleértve a 32 bites Oracle illesztőprogramok fogja használni</span><span class="sxs-lookup"><span data-stu-id="e61c5-119">When connecting to Oracle data sources using Excel on a computer running the 32-bit version of Microsoft Office, including on 64-bit Windows, the 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e61c5-120">Oracle-adatforrások Excel használatával a Microsoft Office 64 bites verzióját futtató számítógépen való kapcsolódáskor használandó a 64 bites Oracle illesztőprogramok</span><span class="sxs-lookup"><span data-stu-id="e61c5-120">When connecting to Oracle data sources using Excel on a computer running the 64-bit version of Microsoft Office, the 64-bit Oracle drivers will be used</span></span>

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a><span data-ttu-id="e61c5-121">Regisztráció és SQL Server Reporting Services csatlakozik</span><span class="sxs-lookup"><span data-stu-id="e61c5-121">Registering and connecting to SQL Server Reporting Services</span></span>
<span data-ttu-id="e61c5-122">Az SQL Server Reporting Services (SSRS) adatforrásokat korlátozódik jelenleg csak a natív üzemmódra kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="e61c5-122">Support for SQL Server Reporting Services (SSRS) data sources is currently limited to Native Mode servers only.</span></span> <span data-ttu-id="e61c5-123">A SharePoint módban SSRS támogatása egy későbbi kiadásban lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="e61c5-123">Support for SSRS in SharePoint mode will be added in a later release.</span></span>

### <a name="opening-data-assets-in-excel"></a><span data-ttu-id="e61c5-124">Az Excel megnyitásakor adategységeket</span><span class="sxs-lookup"><span data-stu-id="e61c5-124">Opening data assets in Excel</span></span>
<span data-ttu-id="e61c5-125">A Microsoft Excel az adategységek megnyitásakor a **Azure Data Catalog** portálon kéri a egy **Microsoft Excel biztonsági figyelmeztetés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e61c5-125">When opening data assets in Microsoft Excel from the **Azure Data Catalog** portal, users may be prompted with a **Microsoft Excel Security Notice** dialog box.</span></span> <span data-ttu-id="e61c5-126">Standard szintű, ami normális működés, és a felhasználók is kiválaszthat **engedélyezése** folytatja.</span><span class="sxs-lookup"><span data-stu-id="e61c5-126">This is standard, expected behavior, and users can select **Enable** to continue.</span></span>

<span data-ttu-id="e61c5-127">További információkért lásd: [engedélyezheti vagy tilthatja le a biztonsági riasztások hivatkozások és a gyanús webhelyekről származó fájlokat](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span><span class="sxs-lookup"><span data-stu-id="e61c5-127">For more information, see [Enable or disable security alerts about links and files from suspicious websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span></span>

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a><span data-ttu-id="e61c5-128">Proxy- és házirend-konfiguráció és adatok adatforrás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e61c5-128">Proxy and policy configuration and data source registration</span></span>
<span data-ttu-id="e61c5-129">Felhasználók is talál, ahol bejelentkezés az Azure Data Catalog-portált, de amikor megpróbálják jelentkezzen be az adatforrás-regisztráló eszköz olyan hibaüzenetet, amely megakadályozza, hogy bejelentkezzen szembesülnek.</span><span class="sxs-lookup"><span data-stu-id="e61c5-129">Users may encounter a situation where they can log on to the Azure Data Catalog portal, but when they attempt to log on to the data source registration tool they encounter an error message that prevents them from logging on.</span></span>

<span data-ttu-id="e61c5-130">A probléma megoldásához két lehetséges oka is van:</span><span class="sxs-lookup"><span data-stu-id="e61c5-130">There are two potential causes for this problem behavior:</span></span>

<span data-ttu-id="e61c5-131">**1. ok: Az Active Directory összevonási szolgáltatások konfigurációs** az adatforrás-regisztráló eszköz űrlapok-hitelesítést használ a felhasználói bejelentkezés Active Directory általi érvényesítésére.</span><span class="sxs-lookup"><span data-stu-id="e61c5-131">**Cause 1: Active Directory Federation Services configuration** The data source registration tool uses Forms Authentication to validate user logons against Active Directory.</span></span> <span data-ttu-id="e61c5-132">Sikeres bejelentkezés az űrlapos hitelesítés engedélyezni kell a globális hitelesítési házirend Active Directory-rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="e61c5-132">For successful logon, Forms Authentication must be enabled in the Global Authentication Policy by an Active Directory administrator.</span></span>

<span data-ttu-id="e61c5-133">Bizonyos esetekben a hiba oka lehet csak akkor, ha a felhasználó a vállalati hálózaton van, vagy csak akkor, ha a felhasználó a kapcsolódik a vállalati hálózaton kívülről.</span><span class="sxs-lookup"><span data-stu-id="e61c5-133">In some situations, this error behavior may occur only when the user is on the company network, or only when the user is connecting from outside the company network.</span></span> <span data-ttu-id="e61c5-134">A globális hitelesítési házirend lehetővé teszi, hogy a hitelesítési módszerek külön-külön engedélyezni kell az intranetes és extranetes kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="e61c5-134">The Global Authentication Policy allows authentication methods to be enabled separately for intranet and extranet connections.</span></span> <span data-ttu-id="e61c5-135">Bejelentkezési hiba akkor fordulhat elő, ha a hálózathoz, amelyről a felhasználó kapcsolódik nincs engedélyezve az űrlapos hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="e61c5-135">Logon errors may occur if Forms Authentication is not enabled for the network from which the user is connecting.</span></span>

<span data-ttu-id="e61c5-136">További információkért lásd: [hitelesítési házirendek konfigurálása](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="e61c5-136">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>

<span data-ttu-id="e61c5-137">**2. ok: Hálózati proxykonfigurációt** a vállalati hálózat proxykiszolgálót használ, ha a frissítésregisztráló eszköz nem lehet csatlakozni az Azure Active Directoryba a proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="e61c5-137">**Cause 2: Network proxy configuration** If the corporate network uses a proxy server, the registration tool may not be able to connect to Azure Active Directory through the proxy.</span></span> <span data-ttu-id="e61c5-138">Felhasználók is biztosítja, hogy a frissítésregisztráló eszköz az eszköz konfigurációs fájlt, ez a szakasz a fájl hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="e61c5-138">Users can ensure that the registration tool by editing the tool’s configuration file, adding this section to the file:</span></span>

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


<span data-ttu-id="e61c5-139">Keresse meg a RegistrationTool.exe.config fájlt, indítsa el a regisztrációs eszköz, és nyissa meg a Windows Feladatkezelő segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="e61c5-139">To locate the RegistrationTool.exe.config file, launch the registration tool, and then open the Windows Task Manager utility.</span></span> <span data-ttu-id="e61c5-140">A Feladatkezelő Részletek lapján kattintson a jobb gombbal a RegistrationTool.exe, és válassza ki a fájl helyének megnyitása a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="e61c5-140">On the Details tab in Task manager, right-click on RegistrationTool.exe and choose Open file location from the pop-up menu.</span></span>
