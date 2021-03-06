---
ms.date: 2017-06-12
ms.topic: conceptual
keywords: dsc,powershell,configuration,setup
title: "Ressources composites : utilisation d’une configuration DSC comme ressource"
ms.openlocfilehash: 1d5fb89eb9845820de8543f388ddb6aaeaaa3e44
ms.sourcegitcommit: 18e3bfae83ffe282d3fd1a45f5386f3b7250f0c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="composite-resources-using-a-dsc-configuration-as-a-resource"></a>Ressources composites : utilisation d’une configuration DSC comme ressource

> S’applique à : Windows PowerShell 4.0, Windows PowerShell 5.0

Les configurations peuvent parfois s’avérer longues et complexes, faire appel à de nombreuses ressources différentes et définir un grand nombre de propriétés. Pour vous faciliter la vie, vous pouvez utiliser une configuration DSC Windows PowerShell comme ressource pour d’autres configurations. Nous l’appelons « ressource composite ». Une ressource composite est une configuration DSC qui accepte des paramètres. Les paramètres de la configuration font office de propriétés de la ressource. La configuration est enregistrée dans un fichier avec une extension **.schema.psm1**. Elle remplace à la fois le schéma MOF et le script de ressource dans une ressource DSC classique. Pour plus d’informations sur les ressources DSC, consultez [DSC Resources](resources.md).

## <a name="creating-the-composite-resource"></a>Création de la ressource composite

Dans notre exemple, nous créons une configuration qui appelle plusieurs ressources existantes pour configurer des machines virtuelles. Au lieu de spécifier les valeurs à définir dans des blocs de configuration, la configuration choisit plusieurs paramètres qui sont ensuite utilisés dans les blocs de configuration.

```powershell
Configuration xVirtualMachine
{
    param
    (
        # Name of VMs
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String[]] $VMName,

        # Name of Switch to create
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $SwitchName,

        # Type of Switch to create
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $SwitchType,

        # Source Path for VHD
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VHDParentPath,

        # Destination path for diff VHD
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VHDPath,

        # Startup Memory for VM
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VMStartupMemory,

        # State of the VM
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VMState
    )

    # Import the module that defines custom resources
    Import-DscResource -Module xComputerManagement,xHyper-V

    # Install the Hyper-V role
    WindowsFeature HyperV
    {
        Ensure = "Present"
        Name = "Hyper-V"
    }

    # Create the virtual switch
    xVMSwitch $SwitchName
    {
        Ensure = "Present"
        Name = $SwitchName
        Type = $SwitchType
        DependsOn = "[WindowsFeature]HyperV"
    }

    # Check for Parent VHD file
    File ParentVHDFile
    {
        Ensure = "Present"
        DestinationPath = $VHDParentPath
        Type = "File"
        DependsOn = "[WindowsFeature]HyperV"
    }

    # Check the destination VHD folder
    File VHDFolder
    {
        Ensure = "Present"
        DestinationPath = $VHDPath
        Type = "Directory"
        DependsOn = "[File]ParentVHDFile"
    }

    # Create VM specific diff VHD
    foreach ($Name in $VMName)
    {
        xVHD "VHD$Name"
        {
            Ensure = "Present"
            Name = $Name
            Path = $VHDPath
            ParentPath = $VHDParentPath
            DependsOn = @("[WindowsFeature]HyperV",
                          "[File]VHDFolder")
        }
    }

    # Create VM using the above VHD
    foreach($Name in $VMName)
    {
        xVMHyperV "VMachine$Name"
        {
            Ensure = "Present"
            Name = $Name
            VhDPath = (Join-Path -Path $VHDPath -ChildPath $Name)
            SwitchName = $SwitchName
            StartupMemory = $VMStartupMemory
            State = $VMState
            MACAddress = $MACAddress
            WaitForIP = $true
            DependsOn = @("[WindowsFeature]HyperV",
                          "[xVHD]VHD$Name")
        }
    }
}
```

### <a name="saving-the-configuration-as-a-composite-resource"></a>Enregistrement de la configuration comme ressource composite

Pour utiliser la configuration paramétrable comme ressource DSC, enregistrez-la dans une structure de répertoires similaire à celle d’une ressource MOF, puis attribuez-lui un nom et une extension **.schema.psm1**. Pour cet exemple, nous allons nommer le fichier **xVirtualMachine.schema.psm1**. Vous devez également créer un manifeste nommé **xVirtualMachine.psd1** contenant la ligne suivante. Notez que cela vient s’ajouter à **MyDscResources.psd1**, manifeste de module de toutes les ressources situé dans le dossier **MyDscResources**.

```powershell
RootModule = 'xVirtualMachine.schema.psm1'
```

Quand vous avez terminé, la structure de dossiers doit ressembler à ceci :

```
$env: psmodulepath
    |- MyDscResources
           MyDscResources.psd1
        |- DSCResources
            |- xVirtualMachine
                |- xVirtualMachine.psd1
                |- xVirtualMachine.schema.psm1
```

La ressource est désormais détectable à l’aide de l’applet de commande Get-DscResource. Ses propriétés sont détectables par cette applet de commande ou par la saisie semi-automatique **Ctrl+Espace** dans Windows PowerShell ISE.

## <a name="using-the-composite-resource"></a>Utilisation de la ressource composite

Vous créez ensuite une configuration qui appelle la ressource composite. Cette configuration appelle la ressource composite xVirtualMachine pour créer une machine virtuelle, puis appelle la ressource **xComputer** pour la renommer.

```powershell

configuration RenameVM
{

    Import-DscResource -Module TestCompositeResource
    Node localhost
    {
        xVirtualMachine VM
        {
            VMName = "Test"
            SwitchName = "Internal"
            SwitchType = "Internal"
            VhdParentPath = "C:\Demo\VHD\RTM.vhd"
            VHDPath = "C:\Demo\VHD"
            VMStartupMemory = 1024MB
            VMState = "Running"
        }
    }

    Node "192.168.10.1"
    {
        xComputer Name
        {
            Name = "SQL01"
            DomainName = "fourthcoffee.com"
        }
    }
}
```

## <a name="supporting-psdscrunascredential"></a>Prise en charge de PsDscRunAsCredential

>**Remarque :** **PsDscRunAsCredential** est pris en charge dans PowerShell 5.0 et versions ultérieures.

La propriété **PsDscRunAsCredential** peut être utilisée dans le bloc de ressources [Configurations DSC](configurations.md) pour spécifier que la ressource doit être exécutée sous un jeu d’informations d’identification spécifié.
Pour plus d’informations, consultez [Exécution de DSC avec les informations d’identification de l’utilisateur](runAsUser.md).

Pour accéder au contexte utilisateur dans une ressource personnalisée, vous pouvez utiliser la variable automatique `$PsDscContext`.

Par exemple, le code suivant écrit le contexte de l’utilisateur sous lequel la ressource s’exécute dans le flux de sortie des messages :

```powershell
if ($PsDscContext.RunAsUser) {
    Write-Verbose "User: $PsDscContext.RunAsUser";
}
```

## <a name="see-also"></a>Voir aussi
### <a name="concepts"></a>Concepts
* [Écriture d’une ressource DSC personnalisée avec MOF](authoringResourceMOF.md)
* [Get Started with Windows PowerShell Desired State Configuration](overview.md)

