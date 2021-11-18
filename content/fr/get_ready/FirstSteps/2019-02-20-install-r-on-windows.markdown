---
title: "Installer R"
author: "Damien Jourdain"
date: '2019-02-20'
output:
  html_document:
    keep_md: true
    df_print: paged
slug: install-r-on-windows
tags: []
categories: R
type: book
weight: 2
---


Utiliser le site officiel <a href="https://cran.r-project.org/" target="_blank">https://cran.r-project.org/ </a> et faire un tour d’horizon du site ( en anglais);

{{% callout note %}}

*Dois-je vraiment télécharger R si je veux utiliser R Studio ?

Même si vous avez l'intention d'utiliser RStudio, vous devez télécharger R sur votre ordinateur. RStudio vous aide à utiliser la version de R qui se trouve sur votre ordinateur, mais il n'est pas livré avec une version de R.

{{% /callout %}}


{{< figure src="cran_root.jpg" title="Page d'accueil du site CRAN" numbered="true">}}

Nous nous concentrerons sur l'installation sur un système d'exploitation **Windows**. Clickez donc sur le lien "Download R for Windows"

Il vous sera proposé d'installer la dernière version de R qui correspond à votre système d'exploitation (premier lien en haut de la page comme indiqué dans la Figure 2).

{{< figure src="cran-download.png" title="Ecran d'installation à partir du CRAN" numbered="true">}}

N'hésitez pas à explorer cette page. En particulier, vous pouvez explorer le lien "Does R run under my version of Windows" avant de commencer le processus d'installation.

Lorsque vous êtes prêt, cliquez sur le lien de téléchargement.

Vous recevrez un message vous demandant d'enregistrer le fichier. Cliquez sur le bouton "Enregistrer". Veuillez noter que le téléchargement peut prendre un certain temps. Une fois le téléchargement terminé, ouvrez le fichier et lancez le programme. (Je suppose que vous savez où et comment ouvrir le fichier qui vient d'être sauvegardé. Comme le mode de fonctionnement est spécifique à votre système, nous ne donnons pas d'instructions particulières ici).

Un assistant d'installation devrait apparaître. Suivez les différentes étapes : paramétrez votre langue, les répertoires où il sera installé, etc. En tant que débutant, il est conseillé de suivre les options par défaut proposées par l'assistant !

Jusqu'à ce que vous arriviez à l'étape `Terminer`.

Un raccourci R devrait maintenant être présent sur l'écran du bureau de votre ordinateur.
Pour vérifier que tout s'est bien passé, vous pouvez ouvrir le programme R.

Cliquez sur la nouvelle icône R et vous devriez voir apparaître la console R comme dans la figure 3 :

{{< figure src="RConsole.jpg" title="La console R" numbered="true">}}


Vous pouvez taper votre première commande R. Tapez 1+1, puis tapez la touche "Entrée". Normalement, la console devrait créer une nouvelle ligne :  `[1] 2`

{{< figure src="RConsole2.jpg" title="Votre première commande" numbered="true">}}

**Félicitations, vous venez d'installer R sur votre ordinateur !**


