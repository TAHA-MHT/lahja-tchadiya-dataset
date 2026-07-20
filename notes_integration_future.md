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
7. 
## Stratégie de coûts à grande échelle (réflexion juillet 2026)

### Tarifs Gemini (juillet 2026)
- gemini-3.5-flash : $1,50 / $9,00 par million de tokens (entrée/sortie)
- gemini-2.5-flash-lite : $0,10 / $0,40 par million de tokens (bien moins cher, qualité un peu inférieure)

### Estimation à grande échelle (hypothèse : 10 questions/jour/utilisateur, échanges courts)
- 5 000 utilisateurs : ~$2 250/mois (gemini-3.5-flash) ou ~$105/mois (flash-lite)
- 1 000 000 utilisateurs : ~$450 000/mois (gemini-3.5-flash) ou ~$21 000/mois (flash-lite)

### Pistes de réduction de coûts (à activer progressivement selon la croissance)
1. Router intelligent : commandes simples (domotique, contacts) déjà traitées en local, sans Gemini
2. Mise en cache des réponses fréquentes (questions répétitives)
3. Réduire maxOutputTokens au strict nécessaire
4. Utiliser gemini-2.5-flash-lite pour les questions simples, garder un modèle plus puissant seulement si besoin

### Option datacenter / auto-hébergement (pour plus tard, à grande échelle seulement)
- Un vrai datacenter physique n'est PAS réaliste ni nécessaire à notre stade (coût de matériel énorme, infrastructure électrique/réseau, équipe technique dédiée)
- Alternative réaliste : louer un serveur GPU (RunPod, Vast.ai, Hugging Face Endpoints) et y héberger un modèle open-source (Llama 3, Mistral, Gemma)
- Coût approximatif : $360 à $1 440/mois en continu, indépendant du nombre d'utilisateurs (contrairement à Gemini qui facture par usage)
- Rentable seulement à partir d'un volume important d'utilisateurs (dizaines/centaines de milliers+)
- Inconvénient : demande un fine-tuning supplémentaire pour bien gérer la lahja tchadiya, en plus du travail déjà prévu sur Whisper

### Décision actuelle (juillet 2026)
Rester sur Gemini (gemini-3.5-flash, maxOutputTokens: 800) pendant la phase de test et de croissance initiale. Reconsidérer l'auto-hébergement uniquement quand le volume d'utilisateurs justifiera clairement l'investissement.
