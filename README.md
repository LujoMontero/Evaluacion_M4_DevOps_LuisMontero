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

## ✅ Código

```java
public class Usuario {
    private String nombre;
    private double peso;

    public Usuario(String nombre, double peso) {
        this.nombre = nombre;
        this.peso = peso;
    }

    public String getNombre() {
        return nombre;
    }

    public double getPeso() {
        return peso;
    }

    public void actualizarPeso(double nuevoPeso) {
        // ERROR: En lugar de asignar el nuevo peso, se está restando 1kg.
        this.peso -= 1;
    }

    public void mostrarInformacion() {
        System.out.println("Usuario: " + nombre + ", Peso Actual: " + peso + " kg");
    }
} ```

---

## ✅ Corrección de Código

**Código original con error:**

```java
public void actualizarPeso(double nuevoPeso) {
    this.peso -= 1;
}```

---
