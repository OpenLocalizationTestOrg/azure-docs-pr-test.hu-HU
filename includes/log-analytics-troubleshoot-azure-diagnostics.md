### <a name="troubleshoot-azure-diagnostics"></a>Az Azure Diagnostics hibaelhárítása

Ha hibaüzenet jelenik meg a következő hello, hello Microsoft.insights erőforrás-szolgáltató nincs regisztrálva:

`Failed tooupdate diagnostics for 'resource'. {"code":"Forbidden","message":"Please register hello subscription 'subscription id' with Microsoft.Insights."}`

tooregister hello erőforrás-szolgáltató, hajtsa végre a következő lépéseket az Azure-portálon hello hello:

1.  Hello bal oldali hello navigációs ablaktábláján kattintson *előfizetések*
2.  Válassza ki a hello hibaüzenet azonosított hello előfizetést
3.  Kattintson az *Erőforrás-szolgáltatók* elemre
4.  Hello található *Microsoft.insights* szolgáltató
5.  Kattintson a hello *regisztrálása* hivatkozás

![Regisztrálja a microsoft.insights erőforrás-szolgáltatót](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

Egyszer hello *Microsoft.insights* erőforrás-szolgáltató regisztrálva van, próbálja meg újra konfigurálni diagnosztika.


A PowerShellben Ha jelenik meg hibaüzenet, a következő hello kell tooupdate PowerShell verziójának:

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

Frissítse a PowerShell toohello November 2016 (v2.3.0), vagy később, a hello utasítások használatát hello feloldása [Ismerkedés az Azure PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) cikk.
