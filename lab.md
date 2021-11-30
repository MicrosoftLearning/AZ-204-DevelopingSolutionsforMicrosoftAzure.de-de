---
ms.openlocfilehash: 57e7101c824903545932fce809cd5661f95cd3dd
ms.sourcegitcommit: d2d374fffa4fcbf92b9c4bdc9c9ecc470152e033
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132626125"
---
# <a name="lab-virtual-machine-setup"></a>Einrichten eines virtuellen Labcomputers

## <a name="installed-software"></a>Installierte Software

| Software | Link |
| --- | --- |
| Windows 10 (Build 2004) | <https://www.microsoft.com/software-download/windows10> |
| Visual Studio Code | <https://code.visualstudio.com> |
| Azure-Kontoerweiterung für Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account> |
| Azure Functions-Erweiterung für Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions> |
| Azure Resource Manager-Tools-Erweiterung für Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools> |
| Azure CLI-Tools-Erweiterung für Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli> |
| Visual Studio Code PowerShell-Erweiterung | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell> |
| Visual Studio Code C#-Erweiterung | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp> |
| PowerShell 7 | <https://github.com/PowerShell/PowerShell/releases/tag/v7.0.3> |
| .NET Core 3.1 SDK | <https://dotnet.microsoft.com/download/dotnet-core/3.1> |
| Azure PowerShell | <https://docs.microsoft.com/powershell/azure/install-az-ps> |
| Azure CLI | <https://docs.microsoft.com/cli/azure/install-azure-cli> |
| Azure Storage-Explorer | <https://azure.microsoft.com/features/storage-explorer> |
| .NET-Tool – HttpRepl | <https://github.com/dotnet/HttpRepl> |
| Azure Functions Core Tools | <https://docs.microsoft.com/azure/azure-functions/functions-run-local#v3> |
| Windows-Terminal | <https://aka.ms/terminal> |
| Edge (Chromium) | <https://www.microsoft.com/edge> |

## <a name="additional-configuration"></a>Zusätzliche Konfiguration

- ClearType aktivieren
  
- Microsoft Edge als Standardbrowser konfigurieren

- VSCode-Konfiguration aktualisieren

  ```json
  {
    "editor.fontFamily": "'Cascadia Code', Consolas, 'Courier New', monospace",
    "update.enableWindowsBackgroundUpdates": false,
    "update.mode": "manual",
    "terminal.integrated.shell.windows": "C:\\Program Files\\PowerShell\\7\\pwsh.exe",
    "workbench.startupEditor": "none",
    "terminal.integrated.rendererType": "dom",
    "csharp.suppressDotnetInstallWarning": true,
    "csharp.suppressDotnetRestoreNotification": true,
    "csharp.supressBuildAssetsNotification": true,
    "azureFunctions.showProjectWarning": false
  }
  ```

- Windows-Terminalkonfiguration aktualisieren

  ```json
  {
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "defaultProfile": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
    "profiles": [
      {
        "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
        "useAcrylic": true,
        "acrylicOpacity": 0.85,
        "colorScheme": "Campbell",
        "fontFace": "Cascadia Code",
        "hidden": false,
        "name": "PowerShell",
        "source": "Windows.Terminal.PowershellCore"
      },
      {
        "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
        "hidden": false,
        "name": "Azure Cloud Shell",
        "source": "Windows.Terminal.Azure"
      }
    ],
    "schemes": [],
    "keybindings": []
  }
  ```

- Startmenü und Taskleiste so konfigurieren, dass nur die folgenden Symbole enthalten sind:
  - Datei-Explorer
  - Microsoft Edge
  - Windows-Terminal
  - Visual Studio Code
  - Azure Storage-Explorer

- PowerShell 7-Updatebenachrichtigungen deaktivieren

  1. [Eine Umgebungsvariable](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_update_notifications?view=powershell-7) namens ``POWERSHELL_UPDATECHECK`` erstellen
  
  1. Den Wert der Umgebungsvariablen auf ``Off`` (Groß-/Kleinschreibung beachten) festlegen

- Azure Functions Core Tools mindestens einmal ausführen, um Windows-Firewall zu konfigurieren

  ```bash
  func init test --worker-runtime dotnet
  cd test
  func new --template 'HTTP trigger' --name web
  func start --build
  ```
