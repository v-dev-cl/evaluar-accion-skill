---
name: evaluar-accion
description: "Evalúa una acción individual (ticker) con un scorecard de 100 puntos y entrega veredicto, semáforo de mercado y plan de entrada por tramos. Usar cuando el usuario entregue uno o más tickers o pregunte si una acción vale la pena comprar, tener en la mira o poner en watchlist (ej. «evalúa NKE», «¿compro MSFT?», «evaluate AAPL», «should I buy NVDA»). Requiere búsqueda web."
license: MIT
compatibility: Requiere búsqueda web activa en el asistente
metadata:
  version: "1.2.0"
  author: v-dev-cl
---

# Evaluar acción

Usar cuando el usuario entregue uno o más tickers (ej. "evalúa NKE", "/evaluar-accion MSFT BSX") y quiera saber si vale la pena tenerla en la mira o comprarla. Responder siempre en español. El usuario invierte desde Chile en acciones de EE.UU. (y ocasionalmente chilenas), con horizonte de mediano/largo plazo y compra escalonada por tramos.

## 1. Recolectar datos (siempre frescos, con fecha)

Usar la búsqueda web disponible. Lanzar varias búsquedas en paralelo si la herramienta lo permite, y repetir con otras consultas si los resultados vienen pobres o viejos. Anotar la fecha de cada dato; no usar datos de memoria para precios, estimaciones ni resultados. Si no hay búsqueda web disponible, avisar al usuario y no inventar cifras.

**Presupuesto:** aproximadamente 15 búsquedas por ticker (más 2 para el semáforo), en este orden de prioridad: (1) último reporte y guía, (2) precio, rango y técnico, (3) valuación y pares, (4) consenso y revisiones, (5) exposición geográfica, aranceles y litigios, (6) insiders, recompras y short interest, (7) catalizadores con fecha.

**Reglas de datos:**
- **Sin dato:** si tras dos búsquedas un ítem no aparece con fecha, puntuarlo 0, escribir «sin dato» en el comentario de la tabla y contarlo. Con 3 o más ítems sin dato, el veredicto máximo es «Esperar señales».
- **Fuentes que se contradicen:** usar la más reciente y anotar la discrepancia en el comentario. Para medias móviles, usar la que calce con el recorrido del precio en el año.
- **Reporte inminente:** si el próximo reporte de resultados está a 10 días o menos, decirlo en el resumen y recomendar esperar el reporte antes del primer tramo (la evaluación entera se reprecia con el reporte).

Datos mínimos:
- Precio actual, variación en el año (YTD), rango de 52 semanas y caída desde el máximo de 52 semanas.
- Último reporte trimestral: fecha, ingresos y variación, EPS vs estimado, márgenes, y la guía (subió / mantuvo / bajó / no entrega).
- Tendencia de ingresos y sorpresas de EPS (real vs estimado) de los últimos 4 trimestres.
- Revisiones de estimaciones de EPS en los últimos ~90 días (al alza / planas / a la baja).
- P/E forward actual vs su promedio de 5 años y vs 2–4 pares directos. PEG si está disponible. Margen operativo de los últimos 12 meses vs su promedio de 5 años. FCF y deuda neta/EBITDA.
- Consenso de analistas: % de compra, precio objetivo medio, rango, y si los objetivos vienen subiendo o bajando.
- Por qué cayó (o subió): noticias y causas concretas.
- Exposición geográfica (especialmente % de ventas en China y su tendencia), cadena de suministro y aranceles, regulación o litigios.
- Señales internas: compras/ventas de insiders recientes, recompras, dividendo, short interest.
- Técnico básico: precio vs medias de 50 y 200 días, RSI de 14 días, soportes y resistencias relevantes, volumen de las últimas 4 semanas.
- Próximo catalizador con fecha (resultados, eventos regulatorios, lanzamientos, investor day).

## 2. Contexto de mercado (semáforo rápido, 1–2 búsquedas)

No suma puntos a la acción, pero define el ritmo de entrada:

| Indicador | Verde | Amarillo | Rojo |
|---|---|---|---|
| Bono 10 años EE.UU. | < 4,75% | 4,75–5,25% | > 5,25% |
| Spread high yield | < 350 pb | 350–500 pb | > 500 pb |
| VIX | < 18 | 18–25 | > 25 |
| S&P 500 vs máximo | < 5% abajo | 5–10% abajo | > 10% abajo |

Mayoría verde: tramos más seguidos. Mayoría amarillo/rojo: espaciar tramos y priorizar niveles más bajos. Un VIX muy alto con spreads estables puede ser oportunidad; spreads ampliándose es la alarma más seria. Umbrales fijados en octubre de 2026: revisarlos cada 12 meses.

Para una acción chilena, usar los equivalentes locales: TPM y BCU a 10 años, IPSA vs su máximo, cobre y tipo de cambio.

## 3. Scorecard (100 puntos)

### A. Calidad del negocio — 20
- Crecimiento de ingresos (promedio de los últimos 4 trimestres, en moneda constante si la empresa lo reporta): > 10% = 5; 5–10% = 4; 0–5% = 3; 0 a −5% = 1; < −5% = 0.
- Margen operativo vs la mediana de 2–4 pares: sobre la mediana y estable o subiendo = 5; sobre la mediana pero cayendo, o bajo la mediana pero subiendo = 3; bajo la mediana y estable = 1; bajo la mediana y cayendo = 0.
- Flujo de caja libre: positivo en los últimos 4 trimestres y cubre dividendo + recompras = 5; positivo pero no cubre la distribución = 2; negativo en alguno de los últimos 4 trimestres = 0.
- Deuda neta/EBITDA: caja neta o < 2x = 5; 2–3,5x = 3; > 3,5x = 0.

### B. Momento de resultados — 15
- Revisiones de EPS 90 días (EPS del año fiscal en curso; si faltan menos de 6 meses para su cierre, el siguiente): al alza (> +2%) = 5; planas (±2%) = 3; a la baja = 0.
- Guía del último reporte: subió = 5; mantuvo = 3; bajó = 0; no entrega guía = 3 (anotarlo). Si es la primera guía anual o la empresa solo guía por trimestre, compararla con el consenso previo: sobre = subió, en línea = mantuvo, bajo = bajó.
- Sorpresas de EPS últimos 4 trimestres: 4 de 4 sobre lo esperado = 5; 3 = 3; 2 = 1; 1 o menos = 0.

### C. Valuación — 20
Múltiplo a usar: P/E forward (próximos 12 meses). **Si el margen operativo de los últimos 12 meses está más de un tercio bajo su promedio de 5 años (utilidades deprimidas), el P/E engaña:** usar EV/Ventas forward en las dos primeras líneas y decirlo en la tabla. Comparar siempre el mismo múltiplo. Si no hay promedio de 5 años del múltiplo forward, usar en este orden y anotar cuál: (a) promedio de los múltiplos de cierre de los últimos 5 ejercicios, (b) promedio trailing de 3–5 años.
- Múltiplo vs su promedio de 5 años: > 20% más barato = 7; 0–20% más barato = 4; 0–20% más caro = 2; > 20% más caro = 0.
- Múltiplo vs la mediana de los pares: > 20% más barata = 5; 0–20% más barata = 3; 0–20% más cara = 1; > 20% más cara = 0.
- PEG = P/E forward / crecimiento anual esperado de EPS en los próximos 2 años. Si el crecimiento esperado es < 5% o negativo, PEG no aplica = 0 (anotarlo). < 1 = 4; 1–2 = 2; > 2 = 0.
- Upside al objetivo consenso: > 30% = 4; 15–30% = 2; < 15% o negativo = 0.

### D. Por qué cayó / tesis — 15
Elegir la rama según la caída desde el máximo de 52 semanas.

**Rama 1 — caída ≥ 10%.** Clasificar la causa principal:
- Múltiplo o sentimiento (negocio intacto, crece, pero el mercado paga menos) = 15.
- Cíclico o temporal (macro, inventario, evento puntual como ciberataque o un trimestre malo) = 10.
- Mixto = 6.
- Estructural (pierde participación de mercado, disrupción tecnológica, cambio regulatorio que afecta el modelo de negocio) = 0; 3 si la directiva ya muestra hitos cumplidos de un plan de giro.
Desempate: si hay pérdida de participación documentada en el negocio principal, es estructural aunque coexistan factores temporales; «mixto» es para causas temporales con un riesgo estructural todavía no confirmado. Explicar la clasificación en 2–3 frases. Advertir explícitamente si parece una trampa de valor (por ejemplo: dividendo sobre el FCF o el EPS guiado, ingresos cayendo y múltiplo «barato» solo contra su propia historia).

**Rama 2 — caída < 10% (la acción está cerca de máximos).** Puntuar la durabilidad de la tesis:
- Ventaja competitiva verificable (participación estable o subiendo, poder de precio, costos de cambio): 0–6.
- Visibilidad del crecimiento a 2 años (guía, consenso, backlog o ingresos recurrentes): 0–6.
- Expectativa ya en el precio (múltiplo vs crecimiento esperado): poco descontado = 3; razonable = 1; todo descontado = 0.
Explicar en 2–3 frases. Con esta rama, el veredicto máximo es Watchlist salvo que el bloque C sume 12 o más (no perseguir precio).

### E. Consenso de analistas — 5
- > 70% compra y objetivos estables o subiendo = 5; 50–70% = 3; 30–50% = 1; < 30% u objetivos cayendo más de 10% en 90 días = 0.

### F. Técnico — 10
- Precio vs media de 200 días: sobre (> +2%) = 3; cerca (±2%) = 2; bajo, hasta 10% = 1; más de 10% bajo = 0.
- Estructura: tendencia alcista con mínimos crecientes, base de 2 meses o más, o ruptura confirmada = 3; mínimos crecientes recientes (< 2 meses) dentro de una tendencia bajista = 1; tendencia bajista activa sin base = 0.
- RSI (14 días): 30–50 saliendo de sobreventa = 2; > 70 = 0; resto = 1.
- Volumen (últimas 4 semanas): días de subida con volumen sobre el promedio de 50 días = 2; mixto = 1; rebote con volumen bajo el promedio, o sin dato = 0.

### G. Exposición macro y geopolítica — 10
Partir en 10 y restar:
- China (u otra región fuera del mercado principal) ≥ 25% de ventas y cayendo: −3. Entre 10% y 25% de ventas y cayendo a doble dígito, o si esa región explica la mayor parte de la caída de ingresos: −2. Si China crece fuerte, no restar y mencionarlo como positivo con riesgo de concentración.
- Aranceles: impacto cuantificado por la empresa en margen o utilidad: −2; mencionado sin cifra: −1.
- Alta sensibilidad a tasas (mucha deuda o valuación basada en flujos lejanos): −2.
- Regulación o litigios que la empresa califica de potencialmente materiales en sus reportes, o que afectan el modelo de negocio: −2. Litigios de rutina no restan.
- Dependencia de un solo producto o cliente: −1.

### H. Señales internas — 5
- Insiders: compras en mercado abierto en los últimos 90 días = 2; compras en los últimos 6 meses sin ventas relevantes después = 1; solo ventas o nada = 0.
- Recompras: activas en el último trimestre reportado = 2; programa autorizado pero pausado, o sin programa = 0.
- Short interest < 5% del flotante = 1; ≥ 5% = 0 y mencionar riesgo/posible squeeze.

## 4. Veredicto

| Puntaje | Veredicto |
|---|---|
| 75–100 | Candidata a compra escalonada |
| 60–74 | Watchlist con gatillos definidos |
| 45–59 | Esperar señales concretas |
| < 45 | Evitar por ahora |

Reglas que limitan el veredicto:
- Si la caída se clasificó como estructural, el veredicto máximo es Watchlist, sin importar el puntaje.
- Si los ingresos caen 2+ trimestres seguidos y la guía se recortó, el puntaje máximo es 59.
- Si se usó la rama 2 del bloque D y el bloque C suma menos de 12, el veredicto máximo es Watchlist.
- Con 3 o más ítems «sin dato», el veredicto máximo es Esperar señales.
- Si el semáforo de mercado está mayoritariamente en rojo, bajar un escalón el ritmo de entrada (no el veredicto).

## 5. Plan de entrada

Depende del veredicto:
- **Candidata a compra escalonada:** tres tramos (1/3 cada uno). Tramo 1 al precio actual o hasta 3% abajo; tramos 2 y 3 en niveles concretos basados en soportes técnicos, caída desde máximos o un múltiplo cercano a su mínimo histórico. Indicar el porcentaje bajo el precio actual de cada nivel. Una fecha límite: si no se alcanzan los niveles 2 y 3, completar en cuotas mensuales o reevaluar.
- **Watchlist:** los tres niveles son gatillos de reevaluación, no órdenes de compra; la regla de cuotas mensuales no aplica. Definir además el gatillo de datos (no de precio) que subiría el veredicto, con fecha.
- **Esperar señales / Evitar:** no entregar niveles de compra. Entregar solo las 2–3 señales concretas, con fecha, que harían reevaluar.

Siempre:
- Tamaño máximo sugerido: 5% del portafolio por acción individual (hasta 10% si es una megacap de alta calidad).
- Qué tendría que pasar para comprar antes (catalizador positivo) y qué invalidaría la tesis.
- Nunca promediar a la baja si la causa de la caída pasa a ser estructural.

## 6. Formato de salida

1. Fecha de evaluación y fecha del dato más antiguo usado.
2. Resumen de 2–3 líneas: veredicto, puntaje y la razón principal (más el aviso de reporte inminente si aplica).
3. Tabla del scorecard: bloque, puntaje, comentario breve (incluyendo «sin dato» donde corresponda).
4. Por qué cayó / tesis: rama usada, clasificación y explicación.
5. Semáforo de mercado (una línea por indicador).
6. Plan de entrada según el veredicto (tabla de niveles y fecha límite, o gatillos de reevaluación).
7. Catalizadores con fecha.
8. Riesgos principales y qué me haría cambiar de opinión.
9. Una línea: no es asesoría financiera.
10. Fuentes: lista de fuentes usadas.

Si el usuario entrega varios tickers, evaluar cada uno y cerrar con una tabla comparativa (ticker, puntaje, veredicto, upside, mayor riesgo).

## 7. Notas para un inversionista en Chile (confirmar con un contador)

- **Acciones chilenas:** agregar tipo de cambio, TPM del Banco Central y cobre si aplica, y usar el semáforo local de la sección 2. La ganancia de capital en acciones de sociedades anónimas chilenas con presencia bursátil, compradas y vendidas en bolsa, paga un impuesto único de 10% hasta el 31 de diciembre de 2026 (Ley 21.420); desde el 1 de enero de 2027 vuelve a ser ingreso no renta (art. 107 LIR, Ley 21.850).
- **Acciones extranjeras:** la ganancia de capital en general tributa en el Global Complementario. Los dividendos de EE.UU. tienen una retención de 15% con el convenio Chile–EE.UU. vigente (formulario W-8BEN en el corredor), lo que afecta el rendimiento neto del dividendo que se menciona en el bloque H.
