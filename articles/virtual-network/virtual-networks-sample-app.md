---
title: "Az Azure mintaalkalmazás DMZ-k való használatra |} Microsoft Docs"
description: "Telepíti az egyszerű webes alkalmazást DMZ létrehozása után forgalom folyamata forgatókönyvek tesztelése"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 60340ab7-b82b-40e0-bd87-83e41fe4519c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 8506238e41c5d9dac8d76d729d4919b30a0528b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sample-application-for-use-with-dmzs"></a><span data-ttu-id="59361-103">Mintaalkalmazás DMZ-k való használatra</span><span class="sxs-lookup"><span data-stu-id="59361-103">Sample application for use with DMZs</span></span>
<span data-ttu-id="59361-104">[A biztonsági határ bevált gyakorlatok laphoz való visszatéréshez][HOME]</span><span class="sxs-lookup"><span data-stu-id="59361-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="59361-105">A PowerShell-parancsfájlok helyi futtathatja a kiszolgálókon IIS01 és AppVM01 telepítsen és konfiguráljon egy egyszerű webalkalmazást fog megjelenni a tartalmat a háttér-kiszolgálófiók AppVM01 az előtér-kiszolgálóról IIS01 html-lapot.</span><span class="sxs-lookup"><span data-stu-id="59361-105">These PowerShell scripts can be run locally on the IIS01 and AppVM01 servers to install and set up a simple web application that displays an html page from the front-end IIS01 server with content from the back-end AppVM01 server.</span></span>

<span data-ttu-id="59361-106">Ez az alkalmazás egy egyszerű tesztelési környezetben biztosít számos DMZ példája és milyen módosításokat a végpontok, a NSG-k, a UDR és a tűzfalon a szabályok hatással lehet a forgalom.</span><span class="sxs-lookup"><span data-stu-id="59361-106">This application provides a simple testing environment for many of the DMZ Examples and how changes on the Endpoints, NSGs, UDR, and Firewall rules can affect traffic flows.</span></span>

## <a name="firewall-rule-to-allow-icmp"></a><span data-ttu-id="59361-107">Tűzfalszabály ICMP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="59361-107">Firewall rule to allow ICMP</span></span>
<span data-ttu-id="59361-108">Ez egyszerű PowerShell utasítás bármely Windows virtuális gépen, amely engedélyezi az ICMP (pingelések) forgalmat is futtatható.</span><span class="sxs-lookup"><span data-stu-id="59361-108">This simple PowerShell statement can be run on any Windows VM to allow ICMP (Ping) traffic.</span></span> <span data-ttu-id="59361-109">A tűzfal frissítés lehetővé teszi, hogy könnyebben tesztelés és hibaelhárítás céljából, azzal, hogy a ping protokoll lehetővé az áthaladás, a windows tűzfal (a legtöbb Linux disztribúciókkal ICMP alapértelmezés szerint be van).</span><span class="sxs-lookup"><span data-stu-id="59361-109">This firewall update allows for easier testing and troubleshooting by allowing the ping protocol to pass through the windows firewall (for most Linux distros ICMP is on by default).</span></span>

```PowerShell
# Turn On ICMPv4
New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
    -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
```

<span data-ttu-id="59361-110">A következő parancsfájlokat használ, a tűzfal szabály hozzáadása esetén az első utasításban.</span><span class="sxs-lookup"><span data-stu-id="59361-110">If you use the following scripts, this firewall rule addition is the first statement.</span></span>

## <a name="iis01---web-application-installation-script"></a><span data-ttu-id="59361-111">IIS01 - webes alkalmazás telepítési parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="59361-111">IIS01 - Web application installation script</span></span>
<span data-ttu-id="59361-112">A rendszer ezt a parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="59361-112">This script will:</span></span>

1. <span data-ttu-id="59361-113">Nyissa meg a IMCPv4 (Ping) a helyi kiszolgáló windows tűzfalon könnyebb teszteléshez</span><span class="sxs-lookup"><span data-stu-id="59361-113">Open IMCPv4 (Ping) on the local server windows firewall for easier testing</span></span>
2. <span data-ttu-id="59361-114">Telepítse az IIS és a .net keretrendszer legalább 4.5</span><span class="sxs-lookup"><span data-stu-id="59361-114">Install IIS and the .Net Framework v4.5</span></span>
3. <span data-ttu-id="59361-115">Hozzon létre egy ASP.NET-weblap és a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="59361-115">Create an ASP.NET web page and a Web.config file</span></span>
4. <span data-ttu-id="59361-116">A fájlhozzáférés könnyebben alapértelmezett alkalmazáskészlet módosítása</span><span class="sxs-lookup"><span data-stu-id="59361-116">Change the Default application pool to make file access easier</span></span>
5. <span data-ttu-id="59361-117">A névtelen felhasználó beállítása a rendszergazdai fiókot és jelszót</span><span class="sxs-lookup"><span data-stu-id="59361-117">Set the Anonymous user to your admin account and password</span></span>

<span data-ttu-id="59361-118">A PowerShell parancsfájl fusson helyileg RDP IIS01 be kellett közben.</span><span class="sxs-lookup"><span data-stu-id="59361-118">This PowerShell script should be run locally while RDP’d into IIS01.</span></span>

```PowerShell
# IIS Server Post Build Config Script
# Get Admin Account and Password
    Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
    $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
    $thePassword = Read-Host -Prompt "The Admin Password"

# Turn On ICMPv4
    Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Install IIS
    Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
    add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console

# Create Web App Pages
    Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
    $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
    <%@ Import Namespace="System.IO" %>
    <script language="vb" runat="server">
        Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
            Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
            Dim objStreamReader As StreamReader
            objStreamReader = File.OpenText(FILENAME)
            Dim contents As String = objStreamReader.ReadToEnd()
            lblOutput.Text = contents
            objStreamReader.Close()
            lblTime.Text = Now()
        End Sub
    </script>

    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
        <title>DMZ Example App</title>
    </head>
    <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
      <form id="frmMain" runat="server">
        <div>
          <h1>Looks like you made it!</h1>
          This is a page from the inside (a web server on a private network),<br />
          and it is making its way to the outside! (If you are viewing this from the internet)<br />
          <br />
          The following sections show:
          <ul style="margin-top: 0px;">
            <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
            <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
            <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
          </ul>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Image File Linked from the Internet</b>:<br />
            <br />
            <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
        </div>
      </form>
    </body>
    </html>'

    $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.web>
        <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
        <httpRuntime targetFramework="4.5" />
        <identity impersonate="true" />
        <customErrors mode="Off"/>
      </system.web>
      <system.webServer>
        <defaultDocument>
          <files>
            <add value="Home.aspx" />
          </files>
        </defaultDocument>
      </system.webServer>
    </configuration>'

    $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
    $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii

# Set App Pool to Clasic Pipeline to remote file access will work easier
    Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
    c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
    c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost

# Make sure the IIS settings take
    Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
    Restart-Service -Name W3SVC

    Write-Host
    Write-Host "Web App Creation Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="appvm01---file-server-installation-script"></a><span data-ttu-id="59361-119">AppVM01 - kiszolgáló telepítési parancsfájlját</span><span class="sxs-lookup"><span data-stu-id="59361-119">AppVM01 - File server installation script</span></span>
<span data-ttu-id="59361-120">Ezt a parancsfájlt hoz létre a háttérkiszolgálók egyszerű alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="59361-120">This script sets up the back-end for this simple application.</span></span> <span data-ttu-id="59361-121">A rendszer ezt a parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="59361-121">This script will:</span></span>

1. <span data-ttu-id="59361-122">Nyissa meg a IMCPv4 könnyebb tesztelési a tűzfalon (Ping)</span><span class="sxs-lookup"><span data-stu-id="59361-122">Open IMCPv4 (Ping) on the firewall for easier testing</span></span>
2. <span data-ttu-id="59361-123">Hozzon létre egy könyvtárat a webhelyhez</span><span class="sxs-lookup"><span data-stu-id="59361-123">Create a directory for the web site</span></span>
3. <span data-ttu-id="59361-124">Hozzon létre egy szövegfájlt, hogy távolról hozzáférést a weblap által</span><span class="sxs-lookup"><span data-stu-id="59361-124">Create a text file to be remotely access by the web page</span></span>
4. <span data-ttu-id="59361-125">A könyvtár- és engedélyezi a hozzáférést a névtelen hozzáférés engedélyeinek beállítása</span><span class="sxs-lookup"><span data-stu-id="59361-125">Set permissions on the directory and file to Anonymous to allow access</span></span>
5. <span data-ttu-id="59361-126">Kapcsolja ki az Internet Explorer fokozott biztonsági engedélyezi a könnyebb böngészés erről a kiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="59361-126">Turn off IE Enhanced Security to allow easier browsing from this server</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="59361-127">**Az ajánlott eljárás**: Soha ne kapcsolja ki az Internet Explorer fokozott biztonsági egy üzemi kiszolgálón, valamint célszerű általában rossz webböngészéshez egy éles kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="59361-127">**Best Practice**: Never turn off IE Enhanced Security on a production server, plus it's generally a bad idea to surf the web from a production server.</span></span> <span data-ttu-id="59361-128">A névtelen hozzáféréshez fájlmegosztások megnyitása is hibás ötlet, de kész itt az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="59361-128">Also, opening up file shares for anonymous access is a bad idea, but done here for simplicity.</span></span>
> 
> 

<span data-ttu-id="59361-129">A PowerShell parancsfájl fusson helyileg RDP AppVM01 be kellett közben.</span><span class="sxs-lookup"><span data-stu-id="59361-129">This PowerShell script should be run locally while RDP’d into AppVM01.</span></span> <span data-ttu-id="59361-130">PowerShell annak biztosítása érdekében a sikeres végrehajtáshoz rendszergazdaként kell futtatni.</span><span class="sxs-lookup"><span data-stu-id="59361-130">PowerShell is required to be run as Administrator to ensure successful execution.</span></span>

```PowerShell
# AppVM01 Server Post Build Config Script
# PowerShell must be run as Administrator for Net Share commands to work

# Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Create Directory
    New-Item "C:\WebShare" -ItemType Directory

# Write out Rand.txt
    $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
    $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii

# Set Permissions on share
    $Acl = Get-Acl "C:\WebShare"
    $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
    $Acl.SetAccessRule($AccessRule)
    Set-Acl "C:\WebShare" $Acl

# Create network share
    Net Share WebShare=C:\WebShare "/grant:Everyone,READ"

# Turn Off IE Enhanced Security Configuration for Admins
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0

    Write-Host
    Write-Host "File Server Set up Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="dns01---dns-server-installation-script"></a><span data-ttu-id="59361-131">DNS01 - DNS-kiszolgáló telepítési parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="59361-131">DNS01 - DNS server installation script</span></span>
<span data-ttu-id="59361-132">Nem szerepel a DNS-kiszolgáló beállítása a mintaalkalmazás parancsprogram van.</span><span class="sxs-lookup"><span data-stu-id="59361-132">There is no script included in this sample application to set up the DNS server.</span></span> <span data-ttu-id="59361-133">Ha a tűzfalszabályok, NSG-t vagy UDR vizsgálata kell lennie, a DNS-forgalom, kell a DNS01 kiszolgálót manuálisan kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="59361-133">If testing of the firewall rules, NSG, or UDR needs to include DNS traffic, the DNS01 server needs to be set up manually.</span></span> <span data-ttu-id="59361-134">A hálózati konfigurációs XML-fájl és a Resource Manager-sablon mindkét példákat tartalmaz DNS01 az elsődleges DNS-kiszolgáló és a nyilvános DNS-kiszolgálóként, 3. szintet, a biztonsági mentési DNS-kiszolgáló által üzemeltetett.</span><span class="sxs-lookup"><span data-stu-id="59361-134">The Network Configuration xml file and Resource Manager Template for both examples includes DNS01 as the primary DNS server and the public DNS server hosted by Level 3 as the backup DNS server.</span></span> <span data-ttu-id="59361-135">A szint 3 DNS-kiszolgáló volna a tényleges nem helyi adatforgalomhoz használt DNS-kiszolgálóra, és DNS01 nem állítania, nem helyi hálózati DNS lépne.</span><span class="sxs-lookup"><span data-stu-id="59361-135">The Level 3 DNS server would be the actual DNS server used for non-local traffic, and with DNS01 not setup, no local network DNS would occur.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59361-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59361-136">Next steps</span></span>
* <span data-ttu-id="59361-137">A IIS01 parancsfájl futtatása az IIS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="59361-137">Run the IIS01 script on an IIS server</span></span>
* <span data-ttu-id="59361-138">AppVM01 fájlkiszolgáló parancsprogrammal</span><span class="sxs-lookup"><span data-stu-id="59361-138">Run File Server script on AppVM01</span></span>
* <span data-ttu-id="59361-139">Keresse meg a nyilvános IP-cím a IIS01 a build ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="59361-139">Browse to the Public IP on IIS01 to validate your build</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
