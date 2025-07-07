# HealthTrack - Evaluación Módulo 4: Automatización de Pruebas

Proyecto de evaluación del Módulo 4 - Automatización de Pruebas, sobre la plataforma HealthTrack, desarrollado en Java.

---

## 🎯 Contexto

La plataforma HealthTrack permite a los usuarios registrar y actualizar su peso. Actualmente, tiene un error crítico en el método de actualización de peso, ya que descuenta siempre 1 kg en lugar de guardar el valor ingresado.

Este repositorio soluciona el error y aplica prácticas de pruebas automatizadas, incluyendo:
- Pruebas unitarias (JUnit)
- Pruebas funcionales (Selenium)
- Pruebas de rendimiento (JMeter)
- Integración en pipeline CI/CD (GitHub Actions)

---

## ✅ Corrección de Código

**Código original con error:**

```java
public void actualizarPeso(double nuevoPeso) {
    this.peso -= 1;
}
