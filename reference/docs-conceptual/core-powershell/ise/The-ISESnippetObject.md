---
ms.date: 2017-06-05
keywords: powershell,applet de commande
title: Objet ISESnippet
ms.assetid: 98bc8113-c3cd-4201-bdb9-9d9bdb7e266c
ms.openlocfilehash: f1b023291826d5568eb8bdf5a898a00228825276
ms.sourcegitcommit: 99227f62dcf827354770eb2c3e95c5cf6a3118b4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="the-isesnippetobject"></a>Objet ISESnippet
  Un objet **ISESnippet** est une instance de la classe Microsoft.PowerShell.Host.ISE.ISESnippet. Les membres de la collection **$psISE.CurrentPowerShellTab.Snippets** sont tous des exemples d’objets **ISESnippet**. La façon la plus simple de créer un extrait de code est d’utiliser l’applet de commande [New-IseSnippet&#91;PSITPro5_ISE&#93;](https://technet.microsoft.com/library/0a6339a3-2683-4a8e-8929-90ad9a95c3e0).

## <a name="properties"></a>Propriétés

### <a name="author"></a>Auteur
  Prise en charge dans Windows PowerShell ISE 3.0 et versions ultérieures, ne figure pas dans les versions antérieures.

 Propriété en lecture seule qui obtient le nom de l’auteur de l’extrait de code.

```
# Get the author of the first snippet item
$psISE.CurrentPowerShellTab.Snippets.Item(0).Author

```

### <a name="codefragment"></a>CodeFragment
  Prise en charge dans Windows PowerShell ISE 3.0 et versions ultérieures, ne figure pas dans les versions antérieures.

 Propriété en lecture seule qui obtient le fragment de code à insérer dans l’éditeur.

```
# Get the code fragment associated with the first snippet item.
$psISE.CurrentPowerShellTab.Snippets.Item(0).CodeFragment

```

### <a name="shortcut"></a>Raccourci
  Prise en charge dans Windows PowerShell ISE 3.0 et versions ultérieures, ne figure pas dans les versions antérieures.

 Propriété en lecture seule qui obtient le raccourci clavier Windows associé à l’élément de menu.

```
# Get the shortcut for the first submenu item.
$psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Clear()
$psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add("_Process",{get-process},"Alt+P")
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus[0].Shortcut
```

## <a name="see-also"></a>Voir aussi
- [Objet ISESnippetCollection](The-ISESnippetCollection-Object.md)
- [Objectif du modèle objet de script Windows PowerShell ISE](purpose-of-the-windows-powershell-ise-scripting-object-model.md)
- [Hiérarchie du modèle objet ISE](The-ISE-Object-Model-Hierarchy.md)
