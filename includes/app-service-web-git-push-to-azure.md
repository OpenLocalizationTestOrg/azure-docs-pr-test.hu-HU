## <a name="push-tooazure-from-git"></a>A Git Push tooAzure

Adjon hozzá egy helyi Git-tárház Azure távoli tooyour.

```bash
git remote add azure <URI from previous step>
```

Leküldéses toohello Azure távoli toodeploy az alkalmazást. A korábban létrehozott hello központi felhasználói létrehozásakor hello jelszó megadását kéri. Győződjön meg arról, hogy a létrehozott hello jelszó megadása [konfigurálása a központi felhasználói](#configure-a-deployment-user), nem hello jelszó toolog toohello Azure-portált használja.

```bash
git push azure master
```

hello előző parancs megjeleníti a toohello hasonló információkat a következő példa:
