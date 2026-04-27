<div align="center">

# 🏃 HealthTrack — Automatización de Pruebas

### Java · JUnit 5 · Selenium · JMeter · SonarQube · GitHub Actions

![Java](https://img.shields.io/badge/Java_17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![JUnit](https://img.shields.io/badge/JUnit_5-25A162?style=for-the-badge&logo=junit5&logoColor=white)
![Selenium](https://img.shields.io/badge/Selenium-43B02A?style=for-the-badge&logo=selenium&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?style=for-the-badge&logo=sonarqube&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white)

</div>

---

## 📌 ¿Qué hace este proyecto?

**HealthTrack** es una plataforma de seguimiento de salud donde los usuarios registran su peso corporal. Este repositorio implementa una **estrategia completa de automatización de pruebas** para detectar, corregir y prevenir un bug crítico en la lógica de actualización de peso, usando JUnit, Selenium y JMeter integrados en un pipeline CI/CD.

---

## 🐛 Bug detectado y corrección

El método `actualizarPeso` contenía un error crítico que descontaba 1 kg fijo en lugar de asignar el nuevo valor:

```java
// ❌ ANTES — Bug crítico
public void actualizarPeso(double nuevoPeso) {
    this.peso -= 1; // Siempre descuenta 1 kg
}

// ✅ DESPUÉS — Corrección aplicada
public void actualizarPeso(double nuevoPeso) {
    this.peso = nuevoPeso; // Asigna el valor correcto
}
```

**Impacto del bug:** datos incorrectos en app de salud → pérdida de confianza del usuario + riesgo legal.

---

## 🧪 Estrategia de pruebas implementada

### 1. Pruebas unitarias — JUnit 5

Validan la lógica del método `actualizarPeso` de forma aislada:

```java
@Test
void testActualizarPeso() {
    Usuario usuario = new Usuario("Pedro", 70.0);
    usuario.actualizarPeso(72.5);
    assertEquals(72.5, usuario.getPeso(), 0.001);
}

@Test
void testPesoInicial() {
    Usuario usuario = new Usuario("Ana", 65.0);
    assertEquals(65.0, usuario.getPeso(), 0.001);
}
```

### 2. Pruebas funcionales — Selenium

Simulan el flujo completo de usuario: login → actualización → verificación visual:

```java
@Test
public void testActualizarPesoEnUI() {
    driver.get("http://localhost:8080/login");
    driver.findElement(By.id("username")).sendKeys("testuser");
    driver.findElement(By.id("password")).sendKeys("1234");
    driver.findElement(By.id("loginButton")).click();
    driver.findElement(By.id("pesoInput")).clear();
    driver.findElement(By.id("pesoInput")).sendKeys("75");
    driver.findElement(By.id("btnActualizar")).click();
    assertEquals("75 kg", driver.findElement(By.id("pesoActual")).getText());
}
```

### 3. Pruebas de rendimiento — JMeter

Verifican el comportamiento de la API bajo carga simultánea:

| Parámetro | Valor objetivo |
|---|---|
| Usuarios concurrentes | 50 |
| Latencia promedio | < 500 ms |
| Throughput mínimo | 20 req/s |
| Tasa de error | < 1% |

```bash
# Ejecutar pruebas de carga
jmeter -n -t tests/actualizarPeso.jmx -l results/resultados.jtl -e -o results/html
```

### 4. Pruebas de regresión

Etiquetas `@Tag("regression")` en tests clave para ejecución selectiva en CI:

```bash
mvn test -Dgroups=regression
```

---

## 🔁 Pipeline CI/CD — GitHub Actions

```yaml
name: HealthTrack CI Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-java@v3
        with:
          java-version: '17'
      - name: Pruebas unitarias
        run: mvn clean test
      - name: Pruebas funcionales (Selenium)
        run: mvn verify -Pselenium-tests
      - name: Análisis SonarQube
        uses: sonarsource/sonarqube-scan-action@v2
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

---

## 🚀 Ejecutar el proyecto

```bash
# Clonar el repositorio
git clone https://github.com/LujoMontero/Evaluacion_M4_DevOps_LuisMontero.git
cd Evaluacion_M4_DevOps_LuisMontero

# Ejecutar pruebas unitarias
mvn clean test

# Ver reporte de pruebas
open target/surefire-reports/index.html
```

---

## 📄 Documentación completa

Disponible en: [`Evaluacion_M4_DevOps_LuisMontero.pdf`](./Evaluacion_M4_DevOps_LuisMontero.pdf)

---

## 👨‍💻 Autor

**Luis Montero** · Especialización DevOps · Julio 2025  
[GitHub](https://github.com/LujoMontero) · [LinkedIn](https://www.linkedin.com/in/luis-montero-if/)
