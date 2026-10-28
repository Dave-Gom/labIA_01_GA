# PROGRESO — Lab 1: Algoritmo Genético para optimizar una parábola

**Materia:** Inteligencia Artificial (FP-UNA) — Prof. Christian von Lücken
**Enunciado:** `../../Programacion de un GA desde 0.pdf`
**Tutorial base:** https://www.datacamp.com/tutorial/genetic-algorithm-python
**Última actualización:** 2026-10-28

## Decisiones tomadas

- **Entorno:** local, VS Code + Jupyter (no Colab). Mac con pyenv.
  - Python global es 3.8.18 (viejo) → usamos `pyenv local 3.13.2` en esta carpeta + venv.
  - Faltan numpy, matplotlib, prettytable, jupyter → se instalan en el venv.
- **Ritmo:** modo profe (no apurado). El núcleo del GA lo escribe David; boilerplate y gráficos se los doy hechos.
- **Formato de entrega:** el enunciado NO lo aclara. Asumimos `lab1_ga.ipynb` autocontenido (respuestas escritas en celdas markdown) + PDF exportado.

## Requisitos del enunciado (checklist de entrega)

- [ ] **Parte 1** — 3 respuestas teóricas: qué es un GA y en qué se inspira; explicar población/fitness/selección/crossover/mutación; por qué se usa para optimización.
- [ ] **Parte 2** — GA que optimiza (a,b,c) de y=ax²+bx+c buscando la parábola más plana hacia arriba:
  - [ ] Tabla de mejores individuos por generación
  - [ ] Gráfica de evolución de parámetros
  - [ ] Gráfica de evolución del fitness
  - [ ] Curvas de las parábolas a lo largo de las generaciones
  - [ ] Pregunta de reflexión: por qué a≈0 (positivo), b≈0, c cualquiera; por qué se penaliza a≤0
- [ ] **Parte 3** — al menos 3 experimentos, cada uno con gráficas + un párrafo de análisis:
  - [ ] Exp 1: population_size = 50 vs 200
  - [ ] Exp 2: mutation_rate = 0.1 vs 0.8 (el tutorial usa 1)
  - [ ] Exp 3: selección por ruleta en vez de torneo, o crossover de punto simple
  - [ ] Exp 4 (OPCIONAL avanzado): parábola que pase cerca de puntos dados, o cambiar bounds
- [ ] **Parte 4** — 4 respuestas: GA vs hill climbing; 2 ventajas y 2 desventajas; problemas reales; qué mejorarías del código.

## Plan (14 pasos)

0. Entorno (pyenv local, venv, pip, requirements.txt, kernel VS Code)
1. Estructura del notebook + Parte 1 (teoría)
2. Imports + `create_initial_population`
3. ★ `fitness_function` (planitud + penalización a≤0)
4. ★ `selection` por torneo
5. ★ `crossover` aritmético + `mutation` con clamp
6. Loop principal + tabla PrettyTable
7. Las 3 gráficas (boilerplate dado)
8. Corrida base + reflexión Parte 2
9. Refactor: separar ejecución de graficado
10. Experimentos 1 y 2 (población, mutación)
11. ★ Experimento 3 (ruleta + crossover de punto simple)
12. Experimento 4 (opcional)
13. Parte 4 + checklist + kernel limpio + export PDF

★ = lo escribe David primero, con firma y pseudocódigo; después comparamos con la referencia.

## Estado

- **Pasos completados:** ninguno todavía. Plan propuesto, esperando aprobación de David.
- **Siguiente paso:** Paso 0 — armar el entorno local.

## Conceptos de Python ya vistos

(ninguno todavía)

## Notas / gotchas detectados en el código del tutorial

Material para la pregunta 4 de la Parte 4 ("¿qué mejorarías?"):

1. `mutation_rate = 1` → muta el 100% de los genes cada generación. No es un "rate" razonable.
2. El fitness devuelve `-float('inf')` para a≤0 → rompe `fill_between` del gráfico de rango de fitness en las primeras generaciones.
3. El loop de reproducción usa `population[i+1]` con `range(0, len(pop), 2)` → IndexError si `population_size` es impar.
4. Matemáticamente: curviness = 2a + b²/(2a), así que el óptimo exige a→0⁺ **y** b→0; c es libre (no aparece en el fitness).
