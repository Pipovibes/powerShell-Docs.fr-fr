---
ms.date: 2017-06-05
keywords: powershell,applet de commande
title: "Création d'une zone d'entrée personnalisée"
ms.assetid: 0b12e56c-299f-40ee-afbf-d30d23ed2565
ms.openlocfilehash: 94172102fb81a9b31b7e84188f3e60a372e9cba2
ms.sourcegitcommit: d6ab9ab5909ed59cce4ce30e29457e0e75c7ac12
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2017
---
# <a name="creating-a-custom-input-box"></a>Création d'une zone d'entrée personnalisée
Dans Windows PowerShell 3.0 et versions ultérieures, utilisez les fonctionnalités de création de formulaires de Microsoft .NET Framework pour créer une zone d'entrée graphique personnalisée à l'aide d'un script.

## <a name="create-a-custom-graphical-input-box"></a>Créer une zone d'entrée graphique personnalisée
Copiez le code suivant, puis collez-le dans Windows PowerShell ISE. Ensuite, enregistrez-le en tant que script Windows PowerShell (.ps1).

```
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form 
$form.Text = "Data Entry Form"
$form.Size = New-Object System.Drawing.Size(300,200) 
$form.StartPosition = "CenterScreen"

$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)

$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20) 
$label.Size = New-Object System.Drawing.Size(280,20) 
$label.Text = "Please enter the information in the space below:"
$form.Controls.Add($label) 

$textBox = New-Object System.Windows.Forms.TextBox 
$textBox.Location = New-Object System.Drawing.Point(10,40) 
$textBox.Size = New-Object System.Drawing.Size(260,20) 
$form.Controls.Add($textBox) 

$form.Topmost = $True

$form.Add_Shown({$textBox.Select()})
$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

Le script commence par charger deux classes .NET Framework : **System.Drawing** et **System.Windows.Forms**. Démarrez ensuite une nouvelle instance de la classe .NET Framework **System.Windows.Forms.Form**. Celle-ci affiche une fenêtre ou un formulaire vide où vous pouvez commencer à ajouter des contrôles.

```
$form = New-Object System.Windows.Forms.Form
```

Après avoir créé une instance de la classe Form, affectez des valeurs à trois propriétés de cette classe.

- **Text.** Il s'agit du titre de la fenêtre.

- **Size.** Il s'agit de la taille du formulaire en pixels. Le script précédent crée un formulaire de 300 pixels de largeur par 200 pixels de hauteur.

- **StartingPosition.** Cette propriété facultative a la valeur **CenterScreen** dans le script précédent. Si vous n'ajoutez pas cette propriété, Windows sélectionne un emplacement quand le formulaire est ouvert. Si vous affectez à **StartingPosition** la valeur **CenterScreen**, le formulaire s’affiche automatiquement au milieu de l’écran à chaque chargement.

```
$form.Text = "Data Entry Form"
$form.Size = New-Object System.Drawing.Size(300,200) 
$form.StartPosition = "CenterScreen"
```

Ensuite, créez un bouton **OK** dans votre formulaire. Spécifiez la taille et le comportement du bouton **OK**. Dans cet exemple, le bouton se trouve à 120 pixels du bord supérieur du formulaire et à 75 pixels du bord gauche. Le bouton mesure 23 pixels de haut et 75 pixels de long. Le script utilise des types Windows Forms prédéfinis pour déterminer le comportement du bouton.

```
$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)
```

De la même façon, créez un bouton **Annuler**. Le bouton **Annuler** se trouve à 120 pixels du bord supérieur de la fenêtre, mais à 150 pixels du bord gauche.

```
$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)
```

Ensuite, entrez le texte d'étiquette décrivant les informations que doivent fournir les utilisateurs dans la fenêtre.

```
$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20) 
$label.Size = New-Object System.Drawing.Size(280,20) 
$label.Text = "Please enter the information in the space below:"
$form.Controls.Add($label)
```

Ajoutez le contrôle (dans ce cas, une zone de texte) qui permet aux utilisateurs de fournir les informations que vous avez décrites dans votre texte d'étiquette. Hormis les zones de texte, vous pouvez appliquer de nombreux autres contrôles. Pour plus de contrôles, voir [Espace de noms System.Windows.Forms](http://msdn.microsoft.com/library/k50ex0x9(v=vs.110).aspx) sur MSDN.

```
$textBox = New-Object System.Windows.Forms.TextBox 
$textBox.Location = New-Object System.Drawing.Point(10,40) 
$textBox.Size = New-Object System.Drawing.Size(260,20) 
$form.Controls.Add($textBox)
```

Affectez à la propriété **Topmost** la valeur **$True** pour forcer la fenêtre à s’ouvrir au-dessus des autres fenêtres et boîtes de dialogue.

```
$form.Topmost = $True
```

Ensuite, ajoutez cette ligne de code pour activer le formulaire et définissez le focus sur la zone de texte que vous avez créée.

```
$form.Add_Shown({$textBox.Select()})
```

Ajoutez la ligne de code suivante pour afficher le formulaire dans Windows.

```
$result = $form.ShowDialog()
```

Enfin, le code à l’intérieur du bloc **If** indique à Windows comment traiter le formulaire quand un utilisateur entre du texte dans la zone de texte, puis clique sur le bouton **OK** ou appuie sur la touche **Entrée**.

```
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

## <a name="see-also"></a>Voir aussi
- [Hey Scripting Guy:  Why don’t these PowerShell GUI examples work?](http://go.microsoft.com/fwlink/?LinkId=506644)
- [GitHub: Dave Wyatt’s WinFormsExampleUpdates](https://github.com/dlwyatt/WinFormsExampleUpdates)
- [Astuce Windows PowerShell de la semaine : création d’une zone d’entrée personnalisée](http://technet.microsoft.com/library/ff730941.aspx)

