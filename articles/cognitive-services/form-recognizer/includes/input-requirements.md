---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 5de121a1a905a58f8b5eaf8e60c1f8da71ee8cd6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91276938"
---
Form Recognizer fonctionne sur les documents d’entrée qui répondent aux critères suivants :

* Format JPG, PNG, PDF (texte ou numérisé) ou TIFF. Les PDF englobant du texte sont préférables, car il n’existe aucun risque d’erreur au niveau de l’extraction et de l’emplacement des caractères.
* La taille du fichier doit être inférieure à 20 Mo.
* Les dimensions des images doivent être comprises entre 50 x 50 et 10 000 x 10 000 pixels.
* Les dimensions des PDF ne doivent pas dépasser 17 x 17 pouces, ce qui correspond aux formats de papier Legal ou A3, et plus petits.
* Pour PDF et TIFF, seules les 200 premières pages sont traitées (avec un abonnement gratuit, seules les deux premières pages sont traitées).
* La taille totale du jeu de données d’apprentissage doit être de 500 pages maximum.
* Si vos fichiers PDF sont verrouillés par mot de passe, vous devez supprimer le verrou avant de les soumettre.
* S’ils sont numérisés à partir de documents papier, la qualité de numérisation des formulaires doit être supérieure.
* Le texte doit utiliser l’alphabet latin (lettres de l’alphabet).
* Pour un apprentissage non supervisé (sans données étiquetées), les données doivent contenir des clés et des valeurs.
* Pour un apprentissage non supervisé (sans données étiquetées), les clés doivent s’afficher au-dessus ou à gauche des valeurs. Elles ne peuvent pas s’afficher ni en dessous ni à droite.

Form Recognizer ne prend actuellement pas en charge ces types de données d’entrée :

* Tables complexes (tables imbriquées, cellules ou en-têtes fusionnés, etc.).
* Cases à cocher ou cases d’option.
