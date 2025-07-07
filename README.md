# 💡 HealthTrack - Evaluación Módulo 4: Automatización de Pruebas

Proyecto desarrollado en Java como parte del Módulo 4 - Automatización de Pruebas.  
La plataforma **HealthTrack** permite a los usuarios registrar y actualizar su peso corporal.

---

## ⚠️ Problema Detectado

El método encargado de actualizar el peso contiene un error crítico:

```java
public void actualizarPeso(double nuevoPeso) {
    this.peso -= 1; // ❌ Error: siempre descuenta 1 kg
}
```

### Impacto:

- ❗ **Datos incorrectos**: el peso no se actualiza correctamente.
- ❗ **Pérdida de confianza**: los usuarios no confían en los datos registrados.
- ❗ **Riesgos legales**: una app de salud debe cumplir con estándares de precisión.

---

## 🔧 Solución Aplicada

Se corrigió el método `actualizarPeso`:

```java
public void actualizarPeso(double nuevoPeso) {
    this.peso = nuevoPeso; // ✅ Solución correcta
}
```

Además, se diseñó una batería de pruebas automatizadas para evitar regresiones y asegurar la calidad del software.

---

## 🧪 Pruebas Automatizadas

Se aplicaron distintos tipos de pruebas:

### 1. ✅ Pruebas Unitarias (JUnit)

**Objetivo:** Validar la lógica del método `actualizarPeso`.

📄 `src/test/java/com/healthtrack/UsuarioTest.java`

```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class UsuarioTest {

    @Test
    public void testActualizarPeso() {
        Usuario usuario = new Usuario("Pedro", 70.0);
        usuario.actualizarPeso(72.5);
        assertEquals(72.5, usuario.getPeso(), 0.001);
    }

    @Test
    public void testPesoInicial() {
        Usuario usuario = new Usuario("Ana", 65.0);
        assertEquals(65.0, usuario.getPeso(), 0.001);
    }
}
```

---

### 2. ✅ Pruebas Funcionales (Selenium)

**Objetivo:** Simular un flujo real de usuario: login, actualización y visualización del nuevo peso.

```java
import org.openqa.selenium.*;
import org.openqa.selenium.chrome.ChromeDriver;
import org.junit.jupiter.api.*;

public class HealthTrackFunctionalTest {
    private WebDriver driver;

    @BeforeEach
    public void setUp() {
        System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
        driver = new ChromeDriver();
    }

    @Test
    public void testActualizarPeso() {
        driver.get("http://localhost:8080/login");
        driver.findElement(By.id("username")).sendKeys("testuser");
        driver.findElement(By.id("password")).sendKeys("1234");
        driver.findElement(By.id("loginButton")).click();
        driver.findElement(By.id("pesoInput")).clear();
        driver.findElement(By.id("pesoInput")).sendKeys("75");
        driver.findElement(By.id("btnActualizar")).click();
        WebElement resultado = driver.findElement(By.id("pesoActual"));
        Assertions.assertEquals("75 kg", resultado.getText());
    }

    @AfterEach
    public void tearDown() {
        driver.quit();
    }
}
```

---

### 3. ✅ Pruebas de Regresión

**Objetivo:** Garantizar que futuras modificaciones no afecten la funcionalidad corregida.

- Etiquetas `@Tag("regression")` en pruebas clave.
- Ejecución continua mediante CI.

---

### 4. ✅ Pruebas de Rendimiento (JMeter)

**Objetivo:** Medir el comportamiento de la app con múltiples usuarios.

**Configuración:**
- 50 usuarios simultáneos
- Ramp-up: 10 segundos
- 5 iteraciones
- Endpoint: `POST /actualizarPeso`

**Requisitos esperados:**

- ⏱️ Latencia promedio < 500 ms  
- 📈 Throughput mínimo: 20 req/s  
- ❌ Error rate < 1%

**Ejecutar en consola:**

```bash
jmeter -n -t tests/actualizarPeso.jmx -l results/resultados.jtl -e -o results/html
```

---

## 🚀 CI/CD con GitHub Actions

**Archivo:** `.github/workflows/ci.yml`

```yaml
name: HealthTrack CI Pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'

      - name: Run Unit Tests (JUnit)
        run: mvn clean test

      - name: Run Functional Tests (Selenium)
        run: mvn verify -Pselenium-tests

      - name: Run Performance Tests (JMeter)
        run: |
          jmeter -n -t tests/actualizarPeso.jmx -l results/resultados.jtl -e -o results/html

      - name: SonarQube Scan
        uses: sonarsource/sonarqube-scan-action@v2
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

---

## 🧾 Conclusiones

### ✅ Problema resuelto:
- Lógica corregida en el método `actualizarPeso`.

### ✅ Buenas prácticas aplicadas:
- Pruebas unitarias y funcionales.
- Pruebas de rendimiento.
- Pipeline automatizado con GitHub Actions.
- Reportes y métricas con SonarQube y JMeter.

### ✅ Beneficios obtenidos:
- Mayor confianza en la plataforma.
- Reducción de errores en producción.
- Cumplimiento de estándares de calidad y precisión en el sector salud.

---

## ✍️ Autor

**Luis Montero**  
*Ingeniero en Informática | DevOps Enthusiast*  
📅 Julio 2025

