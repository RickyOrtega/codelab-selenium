# Importamos los módulos necesarios

## logging
Se utiliza para registrar eventos que ocurren mientras se ejecuta el software.

```python
import logging
```

## time
Este módulo proporciona varias funciones relacionadas con el tiempo.
```python
import time
```

## selenium
Es un marco de trabajo para automatizar los navegadores web.
```python
from selenium import webdriver
```

## By
Este módulo se utiliza para localizar elementos en la página.
```python
from selenium.webdriver.common.by import By
```

## WebDriverWait y expected_conditions
Estos módulos se utilizan para implementar las esperas explícitas.
```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
```

# Configuramos el logging
```python
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')
logger = logging.getLogger()
```

# Inicializamos el driver de Chrome
```python
driver = webdriver.Chrome()
```

# Abrimos la página del demo store
```python
driver.get("http://demo-store.seleniumacademy.com/")
```

# Esperamos a que el selector de idioma sea visible
```python
WebDriverWait(driver, 10).until(
    EC.visibility_of_element_located((By.ID, "select-language"))
)
```

# Cambiamos el idioma a francés
```python
language_selector = driver.find_element(By.ID, "select-language")
for option in language_selector.find_elements(By.TAG_NAME, 'option'):
    if 'French' in option.text:
        option.click()
        break
```

# Esperamos que la página se recargue en francés
```python
WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.CSS_SELECTOR, ".welcome-msg"))
)
```

# Realizamos una búsqueda de 'shirt'
```python
search_box = driver.find_element(By.ID, "search")
search_box.send_keys("shirt")
search_box.submit()
```

# Esperamos a que los resultados de la búsqueda sean visibles
```python
WebDriverWait(driver, 10).until(
    EC.visibility_of_element_located((By.CSS_SELECTOR, ".category-products"))
)
```

# Obtenemos los productos de la búsqueda
```python
products = driver.find_elements(By.CSS_SELECTOR, ".category-products li")
```

# Abrimos el primer producto en otra pestaña
```python
first_product = products[0].find_element(By.TAG_NAME, "a")
first_product.click()
```

# Seleccionamos el color
```python
colores = driver.find_element(By.ID, "configurable_swatch_color").find_elements(By.TAG_NAME, "li")
for color in colores:
    if "white" in color.get_attribute("class"):
        color.click()
        break
```

# Seleccionamos el tamaño
```python
tallas = driver.find_element(By.ID, "configurable_swatch_size").find_elements(By.TAG_NAME, "li")
found_size = False
for talla in tallas:
    if "option-m" in talla.get_attribute("class") or "option-l" in talla.get_attribute("class"):
        talla.click()
        found_size = True
        break
```

# Añadimos el producto al carrito
```python
WebDriverWait(driver, 10).until(
    EC.element_to_be_clickable((By.XPATH, "//button[@onclick='productAddToCartForm.submit(this)']"))
)
add_to_cart_button = driver.find_element(By.XPATH, "//button[@onclick='productAddToCartForm.submit(this)']")
add_to_cart_button.click()
```

# Esperamos 5 segundos antes de cerrar el navegador
```python
time.sleep(5)
```

# Cerramos el navegador
```python
driver.quit()
```
