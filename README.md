#  NeoGaming – Rama test

Esta rama está destinada a las pruebas y verificación de calidad (QA) del proyecto NeoGaming.
Aquí se integran las funcionalidades provenientes de develop para evaluarlas antes de su paso a producción.



##  Objetivo de la Rama test

La rama test permite:

-  Verificar que las nuevas funcionalidades trabajan correctamente juntas.
-  Detectar errores antes de subir cambios a main.  
-  Realizar pruebas manuales y automáticas.
-  Validar que el proyecto cumple con los criterios de calidad definidos.
-  Simular un entorno previo a producción (pre-release). 



##  Tipos de Pruebas a Realizar

- Pruebas de integración  
- Chatbot de asistencia gamer con IA.  
- Pruebas del chatbot e IA
- Pruebas del buscador inteligente (NLP)  
- Pruebas de interfaz (UI/UX)
- Pruebas de rendimiento básicas  

*(Las pruebas avanzadas se documentarán en producción)*.



## Flujo de Trabajo

El flujo de Git usando esta rama es:

> feature/*  →  develop  →  **test**  →  main
 
- Los cambios llegan a test solo cuando **develop** está estable.  
- Tras probar y validar en test, los cambios podrán pasar a *main*. 



##  Tecnologías Bajo Prueba

- *Frontend*: Next.js, TypeScript, React, TailwindCSS  
- *Backend*: Spring Boot (Java), Node.js
- *Base de Datos*: MySQL / Workbench
- *IA*: Modelos NLP, chatbot, sistema de recomendaciones



## Aspectos Pendientes

> ⚠ *Las validaciones de despliegue final y pruebas automatizadas completas se finalizarán en la próxima fase del proyecto.*



## 👩‍💻 Equipo del Proyecto

| Nombre | Rol | Cargo |
|--------|------|-------|
| *Lisandro José Rodríguez Mercado* | Scrum Master | Full Stack Developer |
| *Karol Yuseth Salas Correa* | Product Owner | Full Stack Developer |
| *Juan Manuel Velasco Duque* | Equipo de Desarrollo | Front End Developer |
| *Juan Pablo Galviz Marulanda* | Equipo de Desarrollo | Front End Developer |



**Rama TEST – Validación y aseguramiento de calidad. **
