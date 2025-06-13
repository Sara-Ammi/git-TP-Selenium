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


Je crée une Pull Request vers `main` → Tests se lancent automatiquement

**Résultat GitHub Actions :** ✅ All checks passed

## 5. Configuration qualité

**tests/pytest.ini** :
```ini
[tool:pytest]
testpaths = .
addopts = -v --html=report.html --self-contained-html --cov=../src --cov-report=html
```

Je relance les tests avec couverture :
```bash
python -m pytest test_selenium.py
```

**Sortie :**
```
---------- coverage: platform linux, python 3.9.18 -----------
Name                Stmts   Miss  Cover   Missing
-------------------------------------------------
../src/script.js       15      0   100%
-------------------------------------------------
TOTAL                  15      0   100%
```

## 6. Exercices supplémentaires

### Test avec nombres décimaux
J'ajoute dans `test_selenium.py` :
```python
def test_decimal_numbers(self, driver):
    file_path = os.path.abspath("../src/index.html")
    driver.get(f"file://{file_path}")
    
    driver.find_element(By.ID, "num1").send_keys("3.5")
    driver.find_element(By.ID, "num2").send_keys("2.5")
    select = Select(driver.find_element(By.ID, "operation"))
    select.select_by_value("add")
    driver.find_element(By.ID, "calculate").click()
    
    result = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, "result"))
    )
    assert "Résultat: 6" in result.text
```

### Page Object Pattern
**tests/calculator_page.py** :
```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select, WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import os

class CalculatorPage:
    def __init__(self, driver):
        self.driver = driver
    
    def load_page(self):
        file_path = os.path.abspath("../src/index.html")
        self.driver.get(f"file://{file_path}")
    
    def enter_numbers(self, num1, num2):
        self.driver.find_element(By.ID, "num1").clear()
        self.driver.find_element(By.ID, "num1").send_keys(str(num1))
        self.driver.find_element(By.ID, "num2").clear()
        self.driver.find_element(By.ID, "num2").send_keys(str(num2))
    
    def select_operation(self, operation):
        select = Select(self.driver.find_element(By.ID, "operation"))
        select.select_by_value(operation)
    
    def calculate(self):
        self.driver.find_element(By.ID, "calculate").click()
    
    def get_result(self):
        result = WebDriverWait(self.driver, 10).until(
            EC.presence_of_element_located((By.ID, "result"))
        )
        return result.text
```

Test avec Page Object :
```python
from calculator_page import CalculatorPage

def test_with_page_object(self, driver):
    page = CalculatorPage(driver)
    page.load_page()
    page.enter_numbers(10, 5)
    page.select_operation("multiply")
    page.calculate()
    
    result = page.get_result()
    assert "Résultat: 50" in result
```

## 7. Commandes utiles

**Lancer tous les tests :**
```bash
python -m pytest -v --html=report.html --self-contained-html
```

**Tests avec couverture :**
```bash
python -m pytest --cov=../src --cov-report=html
```

**Voir le rapport HTML :** Ouvrir `report.html` dans le navigateur

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

```bash
git add .
git commit -m "Complete TP with all tests"
git push origin develop
```

Merger la PR → Déploiement automatique sur GitHub Pages ✅

**Site disponible sur :** `https://[username].github.io/selenium-cicd-tp/`