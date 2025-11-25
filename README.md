MyAndroidApp – TP GitHub Actions

Ce projet a été réalisé dans le cadre du TP 2 : GitHub Actions, dont l’objectif est de mettre en place un workflow CI automatisé pour une application Android.
Le but est de comprendre comment GitHub Actions peut automatiser :

la compilation d’un projet Android

l'exécution des tests unitaires

l’intégration via Pull Requests

la vérification du code avant le merge

📌 1. Création du projet Android

Le projet a été créé avec Android Studio en utilisant le template Empty Activity.
Paramètres utilisés :

Language : Kotlin

Minimum SDK : API 21

Nom du projet : MyAndroidApp

Structure générée :

MyAndroidApp/
 ├── app/
 ├── gradle/
 ├── gradlew
 ├── build.gradle
 └── settings.gradle





📌 2. Partage du projet sur GitHub

Le projet a été partagé via Android Studio → VCS → Share Project on GitHub.


📌 3. Mise en place du Workflow GitHub Actions

Un fichier YAML a été créé dans :

.github/workflows/android-ci.yml


Ce workflow se déclenche sur :

chaque push vers la branche main

chaque pull request vers main

✔ Voici le workflow utilisé :
name: Android CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: '11'

    - name: Grant execute permission for gradlew
      run: chmod +x gradlew

    - name: Build with Gradle
      run: ./gradlew build

    - name: Run Unit Tests
      run: ./gradlew test



📌 4. Exécution du workflow

Une fois le fichier YAML ajouté et poussé vers GitHub, le workflow s’exécute automatiquement.





📌 5. Création d’une branche + Pull Request

Une nouvelle branche a été créée :

feature-tests

Des modifications ont été faites dans le projet puis poussées.
Ensuite, une Pull Request a été ouverte vers la branche main.
GitHub Actions lance automatiquement le workflow pour vérifier le code.


📌 6. Ajout d’un test unitaire volontairement erroné

Le test suivant a été modifié :

@Test
fun addition_isCorrect() {
    assertEquals(5, 2 + 2) // Test volontairement faux
}


➡ Cela entraîne un échec du workflow (résultat attendu par le TP).


📌 7. Correction du test et validation

Une fois le test corrigé :

@Test
fun addition_isCorrect() {
    assertEquals(4, 2 + 2) // Test correct
}


Le workflow repasse au vert, ce qui permet de merger la Pull Request.


📌 8. Conclusion

Ce TP m’a permis de :

comprendre et configurer GitHub Actions

automatiser la compilation d’un projet Android

exécuter des tests unitaires automatiquement

utiliser les Pull Requests avec validation automatique

identifier et corriger des erreurs détectées par le CI

GitHub Actions s’avère être un outil puissant pour assurer la qualité, la cohérence et l’automatisation du développement.
