Ez a konfiguráció megkezdése előtt be kell jelentkeznie tooyour Azure-fiók. hello parancsmag kéri hello bejelentkezési hitelesítő adatait az Azure-fiókjával. A bejelentkezés után az tölti le a fiók beállításait,-e elérhető tooAzure PowerShell. További információ: [A Windows PowerShell használata a Resource Managerrel](../articles/powershell-azure-resource-manager.md).

toolog, nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon tooyour fiók. A következő példa toohelp csatlakozás hello használata:

```powershell
Login-AzureRmAccount
```

Ha több Azure-előfizetéssel rendelkezik, ellenőrizze a hello fiókhoz hello előfizetések.

```powershell
Get-AzureRmSubscription
```

Adja meg, hogy szeretné-e toouse hello előfizetés.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```