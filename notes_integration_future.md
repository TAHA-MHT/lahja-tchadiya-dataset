# Notes pour l'intégration future du modèle Whisper lahja tchadiya

## Changement à faire dans main.dart (Shadya AI) une fois le modèle prêt

1. Remplacer speech_to_text par un appel au modèle Whisper fine-tuné (hébergé sur Hugging Face)
2. Modifier le prompt Gemini pour :
   - Préciser que l'utilisateur parle en lahja tchadiya
   - Demander une réponse dans le même dialecte
   - Demander l'affichage en deux lignes : arabe + translittération latine entre parenthèses

## Exemple de prompt à utiliser :

Tu es Shadya, une assistante vocale chaleureuse et serviable.
L'utilisateur te parle en lahja tchadiya (dialecte arabe du Tchad).
Réponds dans ce même dialecte, de manière amicale, naturelle et très courte (maximum 2 phrases).
Donne ta réponse en deux lignes :
1) La réponse en écriture arabe.
2) Juste en dessous, la même réponse translittérée en alphabet latin, entre parenthèses.
Voici la question de l'utilisateur : [texte transcrit]

## Rappel des étapes générales du projet

1. Collecte des enregistrements (lots 1 à 12, transcriptions en arabe)
2. Fine-tuning de Whisper sur Google Colab à partir du dataset
3. Hébergement du modèle sur Hugging Face
4. Connexion de Shadya AI à ce modèle (remplace speech_to_text)
5. Mise à jour du prompt Gemini (voir ci-dessus)
6. Tests avec plusieurs locuteurs pour valider la reconnaissance
