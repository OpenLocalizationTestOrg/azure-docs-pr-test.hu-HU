---
title: "aaaAzure Data Catalog kibocsátási megjegyzései |} Microsoft Docs"
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
ms.openlocfilehash: 661826f66020ba72a885c6b14522b53c8b469d20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-release-notes"></a><span data-ttu-id="e0ea1-103">Az Azure Data Catalog kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="e0ea1-103">Azure Data Catalog release notes</span></span>
## <a name="notes-for-hello-november-20-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e0ea1-104">Az Azure Data Catalog hello 2015. November 20 kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="e0ea1-104">Notes for hello November 20, 2015 release of Azure Data Catalog</span></span>
### <a name="opening-data-sources-in-power-bi-desktop"></a><span data-ttu-id="e0ea1-105">A Power BI Desktop nyitó adatforrások</span><span class="sxs-lookup"><span data-stu-id="e0ea1-105">Opening Data Sources in Power BI Desktop</span></span>
<span data-ttu-id="e0ea1-106">Ha hello "Nyissa meg a Power BI Desktop" lehetőséget választja, a hello **Azure Data Catalog** portálon felhasználók szembesülhetnek a Power BI Desktop alkalmazás hello két problémák egyikét:</span><span class="sxs-lookup"><span data-stu-id="e0ea1-106">When using hello "Open in Power BI Desktop" option from hello **Azure Data Catalog** portal, users may encounter one of two problems in hello Power BI Desktop application:</span></span>

* <span data-ttu-id="e0ea1-107">A "Nem tooOpen dokumentum" hello címmel párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="e0ea1-107">A dialog box with hello title "Unable tooOpen Document" is displayed</span></span>
* <span data-ttu-id="e0ea1-108">a Power BI Desktop alkalmazás hello nyílik meg, de hello fájl üres toobe jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-108">hello Power BI Desktop application opens, but hello file appears toobe empty</span></span>

<span data-ttu-id="e0ea1-109">Az egyes esetekben hello probléma megoldásához érdemes letöltése és telepítése a Power BI Desktop legújabb verziójának hello [powerbi.com webhelyen](https://powerbi.com).</span><span class="sxs-lookup"><span data-stu-id="e0ea1-109">For each situation, hello problem can be resolved by downloading and installing hello latest version of Power BI Desktop from [PowerBI.com](https://powerbi.com).</span></span>

## <a name="notes-for-hello-november-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e0ea1-110">Az Azure Data Catalog hello 2015. November 13 kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="e0ea1-110">Notes for hello November 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-tooteradata"></a><span data-ttu-id="e0ea1-111">Regisztráció és tooTeradata csatlakozás</span><span class="sxs-lookup"><span data-stu-id="e0ea1-111">Registering and connecting tooTeradata</span></span>
<span data-ttu-id="e0ea1-112">TooTeradata adatforrások felhasználók hello megfelelő Teradata ODBC-illesztőprogram van telepítve, amely egyezik hello (32 bites vagy 64 bites) a használt hello szoftver kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-112">When connecting tooTeradata data sources users must have installed hello correct Teradata ODBC driver that match hello bitness (32-bit or 64-bit) of hello software being used.</span></span>

<span data-ttu-id="e0ea1-113">Ez LÉPETT frissítésétől kiadás dátuma, hello legutóbbi [Teradata ODBC-illesztőprogram (verzió: 15.10) Windows](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) kompatibilis Office 2013, de nem Office 2016.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-113">As of this ADC release date, hello most recent [Teradata ODBC driver for windows ( version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) is compatible with Office 2013, but not with Office 2016.</span></span>

## <a name="notes-for-hello-july-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e0ea1-114">Az Azure Data Catalog hello 2015. július 13 kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="e0ea1-114">Notes for hello July 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-toooracle-database"></a><span data-ttu-id="e0ea1-115">Regisztráció és tooOracle adatbázis csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="e0ea1-115">Registering and connecting tooOracle Database</span></span>
<span data-ttu-id="e0ea1-116">Ha tooOracle adatbázis adatok források felhasználók csatlakozás telepíthető hello megfelelő Oracle illesztőprogramokat, amely egyezik hello (32 bites vagy 64 bites) a használt hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-116">When connecting tooOracle Database data sources users must have installed hello correct Oracle drivers that match hello bitness (32-bit or 64-bit) of hello software being used.</span></span>

* <span data-ttu-id="e0ea1-117">32 bites Windows rendszert futtató kiszolgálón tárolt olyan adatforrások Oracle regisztrálásakor hello 32 bites Oracle illesztőprogramok fogja használni</span><span class="sxs-lookup"><span data-stu-id="e0ea1-117">When registering Oracle data sources on a computer running 32-bit Windows, hello 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e0ea1-118">64 bites Windows rendszert futtató kiszolgálón tárolt olyan adatforrások Oracle regisztrálásakor hello 64 bites Oracle illesztőprogramok fogja használni</span><span class="sxs-lookup"><span data-stu-id="e0ea1-118">When registering Oracle data sources on a computer running 64-bit Windows, hello 64-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e0ea1-119">Ha tooOracle adatforrások Excel használatával a Microsoft Office hello 32 bites verzióját futtató számítógépen, beleértve a 64 bites Windows rendszeren hello 32 bites Oracle illesztőprogramok fogja használni</span><span class="sxs-lookup"><span data-stu-id="e0ea1-119">When connecting tooOracle data sources using Excel on a computer running hello 32-bit version of Microsoft Office, including on 64-bit Windows, hello 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e0ea1-120">Excel használatával a Microsoft Office hello 64 bites verzióját futtató számítógépen tooOracle adatforrások kapcsolódáskor használandó hello 64 bites Oracle illesztőprogramok</span><span class="sxs-lookup"><span data-stu-id="e0ea1-120">When connecting tooOracle data sources using Excel on a computer running hello 64-bit version of Microsoft Office, hello 64-bit Oracle drivers will be used</span></span>

### <a name="registering-and-connecting-toosql-server-reporting-services"></a><span data-ttu-id="e0ea1-121">Regisztráció és tooSQL Server Reporting Services csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="e0ea1-121">Registering and connecting tooSQL Server Reporting Services</span></span>
<span data-ttu-id="e0ea1-122">A rendszer jelenleg korlátozott tooNative mód csak kiszolgálók az SQL Server Reporting Services (SSRS) adatforrásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-122">Support for SQL Server Reporting Services (SSRS) data sources is currently limited tooNative Mode servers only.</span></span> <span data-ttu-id="e0ea1-123">A SharePoint módban SSRS támogatása egy későbbi kiadásban lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-123">Support for SSRS in SharePoint mode will be added in a later release.</span></span>

### <a name="opening-data-assets-in-excel"></a><span data-ttu-id="e0ea1-124">Az Excel megnyitásakor adategységeket</span><span class="sxs-lookup"><span data-stu-id="e0ea1-124">Opening data assets in Excel</span></span>
<span data-ttu-id="e0ea1-125">Amikor megnyitja a Microsoft Excel adategységeket hello **Azure Data Catalog** portálon kéri a egy **Microsoft Excel biztonsági figyelmeztetés** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-125">When opening data assets in Microsoft Excel from hello **Azure Data Catalog** portal, users may be prompted with a **Microsoft Excel Security Notice** dialog box.</span></span> <span data-ttu-id="e0ea1-126">Standard szintű, ami normális működés, és a felhasználók választható **engedélyezése** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-126">This is standard, expected behavior, and users can select **Enable** toocontinue.</span></span>

<span data-ttu-id="e0ea1-127">További információkért lásd: [engedélyezheti vagy tilthatja le a biztonsági riasztások hivatkozások és a gyanús webhelyekről származó fájlokat](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span><span class="sxs-lookup"><span data-stu-id="e0ea1-127">For more information, see [Enable or disable security alerts about links and files from suspicious websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span></span>

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a><span data-ttu-id="e0ea1-128">Proxy- és házirend-konfiguráció és adatok adatforrás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e0ea1-128">Proxy and policy configuration and data source registration</span></span>
<span data-ttu-id="e0ea1-129">Felhasználók is talál, ahol toohello Azure Data Catalog-portál, de amikor azok megkísérlik az toohello adatforrás-regisztráló eszköz szembesülnek olyan hibaüzenetet, amely megakadályozza, hogy bejelentkezzen a toolog jelentkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-129">Users may encounter a situation where they can log on toohello Azure Data Catalog portal, but when they attempt toolog on toohello data source registration tool they encounter an error message that prevents them from logging on.</span></span>

<span data-ttu-id="e0ea1-130">A probléma megoldásához két lehetséges oka is van:</span><span class="sxs-lookup"><span data-stu-id="e0ea1-130">There are two potential causes for this problem behavior:</span></span>

<span data-ttu-id="e0ea1-131">**1. ok: Az Active Directory összevonási szolgáltatások konfigurációs** hello adatforrás-regisztráló eszköz használja az űrlapos hitelesítés toovalidate felhasználói bejelentkezések Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-131">**Cause 1: Active Directory Federation Services configuration** hello data source registration tool uses Forms Authentication toovalidate user logons against Active Directory.</span></span> <span data-ttu-id="e0ea1-132">Sikeres bejelentkezés az űrlapos hitelesítés engedélyezni kell a globális hitelesítési házirend hello egy Active Directory-rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-132">For successful logon, Forms Authentication must be enabled in hello Global Authentication Policy by an Active Directory administrator.</span></span>

<span data-ttu-id="e0ea1-133">Bizonyos esetekben a hiba oka lehet csak ha hello felhasználó hello vállalati hálózaton található, vagy csak akkor, ha hello felhasználó külső hello vállalati hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-133">In some situations, this error behavior may occur only when hello user is on hello company network, or only when hello user is connecting from outside hello company network.</span></span> <span data-ttu-id="e0ea1-134">hello globális hitelesítési házirend lehetővé teszi, hogy a hitelesítési módszerek toobe az intranetes és extranetes kapcsolatok külön engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-134">hello Global Authentication Policy allows authentication methods toobe enabled separately for intranet and extranet connections.</span></span> <span data-ttu-id="e0ea1-135">Bejelentkezési hiba akkor fordulhat elő, ha nincs engedélyezve az űrlapos hitelesítés hello hálózati mely hello a csatlakozó felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-135">Logon errors may occur if Forms Authentication is not enabled for hello network from which hello user is connecting.</span></span>

<span data-ttu-id="e0ea1-136">További információkért lásd: [hitelesítési házirendek konfigurálása](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0ea1-136">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>

<span data-ttu-id="e0ea1-137">**2. ok: Hálózati proxykonfigurációt** hello vállalati hálózat proxykiszolgálót használ, ha hello regisztrációs eszköz valószínűleg nem fogja tudni tooconnect tooAzure Active Directory hello proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-137">**Cause 2: Network proxy configuration** If hello corporate network uses a proxy server, hello registration tool may not be able tooconnect tooAzure Active Directory through hello proxy.</span></span> <span data-ttu-id="e0ea1-138">Felhasználók bizonyosodjon meg arról, hogy hello frissítésregisztráló eszköz hello eszköz konfigurációs fájlt, ez a szakasz a toohello fájl hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="e0ea1-138">Users can ensure that hello registration tool by editing hello tool’s configuration file, adding this section toohello file:</span></span>

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


<span data-ttu-id="e0ea1-139">toolocate hello RegistrationTool.exe.config fájlt, indítsa el a hello regisztráló eszközzel, és nyissa meg a Windows Feladatkezelő segédprogram hello.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-139">toolocate hello RegistrationTool.exe.config file, launch hello registration tool, and then open hello Windows Task Manager utility.</span></span> <span data-ttu-id="e0ea1-140">A Feladatkezelő hello Részletek lapján kattintson a jobb gombbal a RegistrationTool.exe és hello legördülő menüből válassza ki a fájl helyének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="e0ea1-140">On hello Details tab in Task manager, right-click on RegistrationTool.exe and choose Open file location from hello pop-up menu.</span></span>
