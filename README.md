# TP Selenium CI/CD - Guide Étudiant

## 1. Création du projet
la structure :
```
selenium-cicd-tp/
├── src/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── tests/
│   ├── test_selenium.py
│   └── requirements.txt
├── .github/workflows/
│   └── ci-cd.yml
```

## 2. Application web (src/)

**index.html** - Je copie le code HTML donné dans le TP
**style.css** - Je copie le CSS donné
**script.js** - Je copie le JavaScript donné

Je teste en ouvrant `index.html` dans le navigateur → ça marche

## 3. Tests Selenium

**tests/requirements.txt** :
J'installe :
    cd tests
    pip install -r requirements.txt

**tests/test_selenium.py** - Je copie tout le code de test donné dans le TP

python -m pytest test_selenium.py -v

**Sortie:**
    test_selenium.py::TestCalculator::test_page_loads PASSED
    test_selenium.py::TestCalculator::test_addition PASSED
    test_selenium.py::TestCalculator::test_division_by_zero PASSED
    test_selenium.py::TestCalculator::test_all_operations PASSED
    test_selenium.py::TestCalculator::test_page_load_time PASSED
    test_selenium.py::TestCalculator::test_negative_numbers PASSED

    ====== 6 passed in 20.06s ======

## 4. CI/CD GitHub Actions

**.github/workflows/ci-cd.yml** - Je copie le workflow donné

Je push sur une branche `develop` :
    git checkout -b develop
    git add .
    git commit -m "Initial setup"
    git push origin develop


**Résultat GitHub Actions :**  All checks passed (Successful in 55s)

## 5. Configuration qualité

**tests/pytest.ini**
Je relance les tests avec couverture :

    python -m pytest test_selenium.py

**Sortie :**
6 passed in 19.50s

## 6. Exercices supplémentaires

### Test avec nombres décimaux
J'ajoute  test_decimal_numbers dans `test_selenium.py`

## 7. Commandes utiles

**Lancer tous les tests :**
    python -m pytest -v --html=report.html --self-contained-html


**Tests avec couverture :**
    python -m pytest --cov=../src --cov-report=html
    sortie : 6 passed in 19.26s


**Voir le rapport HTML :** Ouvrir `report.html`

## 8. Réponses aux questions

### Avantages observés :
- **Automatisation** : Plus besoin de tester manuellement à chaque fois
- **CI/CD** : Détecte les bugs avant qu'ils arrivent en production
- **Rapidité** : Tests en quelques minutes vs tests manuels longs

### Défis rencontrés :
- **Selenium lent** : Solution → utiliser WebDriverWait au lieu de time.sleep
- **Tests instables** : Solution → mode headless pour CI, attentes explicites

### Métriques importantes :
- **Couverture de code** : 100% c'est bien
- **Temps d'exécution** : Mes tests prennent ~15 secondes
- **Taux de réussite** : 5/5 tests passent

## 9. Push final
    git add .
    git commit -m "last commit"
    git push origin develop
