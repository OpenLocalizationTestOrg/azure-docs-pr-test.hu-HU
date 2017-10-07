---
title: "mintaalkalmazás aaaAzure DMZ-k való használatra |} Microsoft Docs"
description: "Ez egy Szegélyhálózaton tootest forgalom folyamata forgatókönyvek létrehozása után a egyszerű webes alkalmazás központi telepítése"
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
ms.openlocfilehash: e0d9cf14590f75b50c64b677efce2c5425b83ec6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-application-for-use-with-dmzs"></a>Mintaalkalmazás DMZ-k való használatra
[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]

A PowerShell-parancsfájlok hello IIS01 és AppVM01 kiszolgálók tooinstall helyileg futtassa, és állítson be egy egyszerű webalkalmazást fog megjelenni a hello AppVM01 háttérkiszolgálói tartalmú kiszolgálóról hello előtér-IIS01 html-lapot.

Ez az alkalmazás egy egyszerű tesztelési környezetben biztosít számos hello DMZ példák és hogyan módosítások hello végpontok, NSG-ket, UDR, és tűzfalszabályok hatással lehet a forgalom.

## <a name="firewall-rule-tooallow-icmp"></a>Tűzfal szabály tooallow ICMP
Ez egyszerű PowerShell utasítás futtatható bármely windowsos virtuális gép tooallow (Ping) ICMP-forgalmat. A tűzfal frissítés könnyebb tesztelés és hibaelhárítás hello ping protokoll toopass hello windows tűzfalon (a legtöbb Linux disztribúciókkal ICMP alapértelmezés szerint be van kapcsolva) lehetővé teszi lehetővé.

```PowerShell
# Turn On ICMPv4
New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
    -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
```

A következő parancsfájlok hello használatához a tűzfal szabály hozzáadása esetén hello első utasítása.

## <a name="iis01---web-application-installation-script"></a>IIS01 - webes alkalmazás telepítési parancsfájlt
A rendszer ezt a parancsfájlt:

1. Nyissa meg a IMCPv4 hello helyi kiszolgáló windows tűzfalon könnyebb tesztelési (Ping)
2. Az IIS telepítése és hello .net keretrendszer legalább 4.5
3. Hozzon létre egy ASP.NET-weblap és a Web.config fájlban
4. Hello alapértelmezett alkalmazás készlet toomake fájlok hozzáférésének könnyebb módosítása
5. Állítsa be a hello névtelen felhasználó tooyour rendszergazdai fiókot és jelszót

A PowerShell parancsfájl fusson helyileg RDP IIS01 be kellett közben.

```PowerShell
# IIS Server Post Build Config Script
# Get Admin Account and Password
    Write-Host "Please enter hello admin account information used toocreate this VM:" -ForegroundColor Cyan
    $theAdmin = Read-Host -Prompt "hello Admin Account Name (no domain or machine name)"
    $thePassword = Read-Host -Prompt "hello Admin Password"

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
          This is a page from hello inside (a web server on a private network),<br />
          and it is making its way toohello outside! (If you are viewing this from hello internet)<br />
          <br />
          hello following sections show:
          <ul style="margin-top: 0px;">
            <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
            <li> File Output - Shows that hello web server is reaching AppVM01 on hello backend subnet and successfully returning content</li>
            <li> Image from hello Internet - Doesnt really show anything, but it made me happy toosee this when hello app worked</li>
          </ul>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Image File Linked from hello Internet</b>:<br />
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

# Set App Pool tooClasic Pipeline tooremote file access will work easier
    Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
    c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
    c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost

# Make sure hello IIS settings take
    Write-Host "Restarting hello W3SVC" -ForegroundColor Cyan
    Restart-Service -Name W3SVC

    Write-Host
    Write-Host "Web App Creation Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="appvm01---file-server-installation-script"></a>AppVM01 - kiszolgáló telepítési parancsfájlját
Ez a parancsfájl hello háttér-egyszerű alkalmazás beállítása. A rendszer ezt a parancsfájlt:

1. Nyissa meg a IMCPv4 könnyebb tesztelési hello tűzfalon (Ping)
2. Hozzon létre egy könyvtárat hello webhely
3. Hozzon létre egy szöveges fájl toobe távolról hello weblap eléréséhez
4. Hello könyvtárat és fájlnevet tooAnonymous tooallow hozzáférési engedélyeket
5. Kapcsolja ki a Internet Explorer fokozott biztonsági tooallow könnyebb böngészés erről a kiszolgálóról 

> [!IMPORTANT]
> **Az ajánlott eljárás**: Soha ne kapcsolja ki az Internet Explorer fokozott biztonsági egy üzemi kiszolgálón, valamint általában egy éles kiszolgálóról rossz ötlet toosurf hello webes. A névtelen hozzáféréshez fájlmegosztások megnyitása is hibás ötlet, de kész itt az egyszerűség érdekében.
> 
> 

A PowerShell parancsfájl fusson helyileg RDP AppVM01 be kellett közben. PowerShell futtató rendszergazda tooensure sikeres végrehajtáshoz szükséges toobe.

```PowerShell
# AppVM01 Server Post Build Config Script
# PowerShell must be run as Administrator for Net Share commands toowork

# Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Create Directory
    New-Item "C:\WebShare" -ItemType Directory

# Write out Rand.txt
    $FileContent = "Hello, I'm hello contents of a remote file on AppVM01."
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

## <a name="dns01---dns-server-installation-script"></a>DNS01 - DNS-kiszolgáló telepítési parancsfájlt
Nem szerepel a minta alkalmazás tooset hello DNS-kiszolgáló parancsprogram van. Ha a tesztelés hello tűzfalszabályok, NSG-t vagy UDR tooinclude DNS-forgalom, kell-e toobe hello DNS01 kiszolgálót manuálisan állítsa be. hello hálózati konfigurációs XML-fájl és a Resource Manager-sablon mindkét példákat tartalmaz DNS01 hello elsődleges DNS-kiszolgáló és a hello 3. szintet, hello biztonsági mentési DNS-kiszolgáló által üzemeltetett nyilvános DNS-kiszolgáló. hello szint 3 DNS-kiszolgáló ehhez hello nem helyi adatforgalomhoz használt tényleges DNS-kiszolgálóra, és DNS01 nem állítania, nem helyi hálózati DNS lépne.

## <a name="next-steps"></a>Következő lépések
* Hello IIS01 parancsfájl futtatása az IIS-kiszolgálón
* AppVM01 fájlkiszolgáló parancsprogrammal
* Keresse meg a nyilvános IP-cím toohello a IIS01 toovalidate a build

<!--Link References-->
[HOME]: ../best-practices-network-security.md
