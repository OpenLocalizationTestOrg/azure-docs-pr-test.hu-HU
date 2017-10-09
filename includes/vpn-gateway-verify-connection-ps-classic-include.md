Ellenőrizheti, hogy sikerült-e a kapcsolat hello "Get-AzureVNetConnection" parancsmag használatával.

1. A következő példa parancsmagban, hello értékek toomatch konfigurálása használata hello saját. szóközöket tartalmaz, a hello virtuális hálózat nevét hello idézőjelek között kell lennie.

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. Hello parancsmag befejeződését követően hello értékeinek megtekintése. Hello az alábbi példában a hello kapcsolati állapota jeleníti meg, mint a "Csatlakoztatott", és láthatja a bemenő és kimenő bájtokat.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : hello connectivity state for hello local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal