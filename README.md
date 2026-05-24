# LAB 12 : Bypass de la Détection de Root Android avec Medusa

**Réalisé par :** Youssef Charaf  
**Établissement :** ENSA Marrakech  
**Environnement de travail :** Hôte Windows (avec Frida préinstallé) / Appareil Android (ou émulateur) rooté.

---

## 🎯 Objectif du Lab
Ce laboratoire vise à démontrer comment contourner les mécanismes de détection de root d'une application Android. L'approche s'appuie sur l'instrumentation dynamique pour intercepter et modifier le comportement des appels de l'application à l'exécution, en utilisant **Medusa** ainsi que des scripts **Frida** purs.

---

<img width="865" height="155" alt="image" src="https://github.com/user-attachments/assets/7ef83960-23b7-4458-a4b3-f08bebbefd20" />


<img width="814" height="88" alt="image" src="https://github.com/user-attachments/assets/4f600bf5-b819-489a-91a0-320219098a6b" />





---

## 🐍 Étape 2 : Installation et Configuration de Medusa

Medusa est utilisé pour automatiser le processus de hook et simplifier le contournement des sécurités (anti-root, anti-frida, etc.).

1. **Clonage et installation des dépendances :**



<img width="969" height="231" alt="image" src="https://github.com/user-attachments/assets/7be3afde-4394-485c-bb85-bf23a19bd5e7" />


2. **Vérification du fonctionnement de la CLI :**

<img width="941" height="311" alt="image" src="https://github.com/user-attachments/assets/47f37ce1-3860-4059-8638-22cd88c10181" />




---

## 🚀 Étape 3 : Exécution du Bypass avec Medusa

L'application cible effectue plusieurs vérifications (lecture de `android.os.Build.TAGS`, présence de binaires comme `su` ou `busybox`, exécution de `Runtime.getRuntime().exec("su")`). Medusa permet de neutraliser ces vérifications dynamiquement.




*(


> **Preuves :



<img width="326" height="644" alt="image" src="https://github.com/user-attachments/assets/2df325de-67dd-4750-9657-98d2be8dc492" />


<img width="767" height="394" alt="image" src="https://github.com/user-attachments/assets/3047a14d-49e4-423e-ac5f-1d799af24905" />



<img width="956" height="484" alt="image" src="https://github.com/user-attachments/assets/e4703f55-0b36-40d7-a0d5-43b90770ab6f" />



<img width="680" height="159" alt="image" src="https://github.com/user-attachments/assets/0f64f55d-e0d7-44f3-9465-0e0acda7219d" />





<img width="300" height="635" alt="image" src="https://github.com/user-attachments/assets/21a9de26-3395-4f0f-a5ab-3ecd0e2ae33e" />




---




```

```
