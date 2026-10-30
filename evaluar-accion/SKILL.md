---
name: "evaluar-accion"
description: "Evalúa una acción individual con un scorecard de 100 puntos (calidad, resultados, valuación, por qué cayó, técnico, macro/China) y entrega veredicto y niveles de entrada."
---

# Evaluar acción

Usar cuando el usuario entregue uno o más tickers (ej. "evalúa NKE", "/evaluar-accion MSFT BSX") y quiera saber si vale la pena tenerla en la mira o comprarla. Responder siempre en español. El usuario invierte desde Chile en acciones de EE.UU. (y ocasionalmente chilenas), con horizonte de mediano/largo plazo y compra escalonada por tramos.

## 1. Recolectar datos (siempre frescos, con fecha)

Usar la búsqueda web disponible. Lanzar varias búsquedas en paralelo si la herramienta lo permite, y repetir con otras consultas si los resultados vienen pobres o viejos. Anotar la fecha de cada dato; no usar datos de memoria para precios, estimaciones ni resultados. Si no hay búsqueda web disponible, avisar al usuario y no inventar cifras.

Datos mínimos:
- Precio actual, variación en el año (YTD), rango de 52 semanas y caída desde el máximo.
- Último reporte trimestral: fecha, ingresos y variación, EPS vs estimado, márgenes, y la guía (subió / mantuvo / bajó).
- Tendencia de ingresos de los últimos 4 trimestres.
- Revisiones de estimaciones de EPS en los últimos ~90 días (al alza / planas / a la baja).
- P/E forward actual vs su promedio de 5 años y vs 2–4 pares directos. PEG si está disponible. FCF y deuda neta/EBITDA.
- Consenso de analistas: % de compra, precio objetivo medio, rango, y si los objetivos vienen subiendo o bajando.
- Por qué cayó (o subió): noticias y causas concretas.
- Exposición geográfica (especialmente % de ventas en China y su tendencia), cadena de suministro y aranceles, regulación o litigios.
- Señales internas: compras/ventas de insiders recientes, recompras, dividendo, short interest.
- Técnico básico: precio vs medias de 50 y 200 días, RSI, soportes y resistencias relevantes.
- Próximo catalizador con fecha (resultados, eventos regulatorios, lanzamientos).

## 2. Contexto de mercado (semáforo rápido, 1–2 búsquedas)

No suma puntos a la acción, pero define el ritmo de entrada:

| Indicador | Verde | Amarillo | Rojo |
|---|---|---|---|
| Bono 10 años EE.UU. | < 4,75% | 4,75–5,25% | > 5,25% |
| Spread high yield | < 350 pb | 350–500 pb | > 500 pb |
| VIX | < 18 | 18–25 | > 25 |
| S&P 500 vs máximo | < 5% abajo | 5–10% abajo | > 10% abajo |

Mayoría verde: tramos más seguidos. Mayoría amarillo/rojo: espaciar tramos y priorizar niveles más bajos. Un VIX muy alto con spreads estables puede ser oportunidad; spreads ampliándose es la alarma más seria.

## 3. Scorecard (100 puntos)

### A. Calidad del negocio — 20
- Crecimiento de ingresos (últimos 4 trimestres): > 10% = 5; 0–10% = 3; negativo = 0–1.
- Márgenes vs pares y su tendencia: 0–5.
- Flujo de caja libre positivo y estable: 0–5.
- Deuda neta/EBITDA: < 2x = 5; 2–3,5x = 3; > 3,5x = 0–1.

### B. Momento de resultados — 15
- Revisiones de EPS 90 días: al alza = 5; planas = 3; a la baja = 0.
- Guía del último reporte: subió = 5; mantuvo = 3; bajó = 0.
- Sorpresas últimos 4 trimestres: 4 de 4 sobre lo esperado = 5; 3 = 3; 2 o menos = 0–1.

### C. Valuación — 20
- P/E forward vs su promedio de 5 años: > 20% más barato = 7; 0–20% más barato = 4; más caro = 0–2.
- P/E vs pares: 0–5.
- PEG: < 1 = 4; 1–2 = 2; > 2 = 0.
- Upside al objetivo consenso: > 30% = 4; 15–30% = 2; < 15% = 0.

### D. Por qué cayó / tesis — 15
Clasificar la causa principal:
- Múltiplo o sentimiento (negocio intacto, crece, pero el mercado paga menos) = 15.
- Cíclico o temporal (macro, inventario, evento puntual como ciberataque o un trimestre malo) = 10.
- Mixto = 6.
- Estructural (pierde participación de mercado, disrupción tecnológica, cambio regulatorio que afecta el modelo de negocio) = 0–3.
Explicar la clasificación en 2–3 frases. Advertir explícitamente si parece una trampa de valor.

### E. Consenso de analistas — 5
- > 70% compra y objetivos estables o subiendo = 5; 50–70% = 3; < 50% u objetivos cayendo fuerte = 0–1.

### F. Técnico — 10
- Precio vs media de 200 días: sobre = 3; cerca = 2; bajo = 0–1.
- Estructura: base formada o mínimos crecientes = 3; tendencia bajista activa = 0.
- RSI: 30–50 saliendo de sobreventa = 2; > 70 = 0; resto = 1.
- Volumen / acumulación: 0–2.

### G. Exposición macro y geopolítica — 10
Partir en 10 y restar:
- China > 25% de ventas y cayendo: −3 (si China crece fuerte, no restar y mencionarlo como positivo con riesgo de concentración).
- Impacto material de aranceles: −2.
- Alta sensibilidad a tasas (mucha deuda o valuación basada en flujos lejanos): −2.
- Riesgo regulatorio o litigios relevantes: −2.
- Dependencia de un solo producto o cliente: −1.

### H. Señales internas — 5
- Compras de insiders recientes: 2.
- Recompras activas: 2.
- Short interest < 5%: 1 (si es alto, 0 y mencionar riesgo/posible squeeze).

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
- Si el semáforo de mercado está mayoritariamente en rojo, bajar un escalón el ritmo de entrada (no el veredicto).

## 5. Plan de entrada

- Tres tramos (1/3 cada uno) con niveles concretos basados en soportes técnicos, caída desde máximos o un P/E cercano a su mínimo histórico. Indicar el porcentaje bajo el precio actual de cada nivel.
- Una fecha límite: si no se alcanzan los niveles, entrar en cuotas mensuales o reevaluar.
- Tamaño máximo sugerido: 5% del portafolio por acción individual (hasta 10% si es una megacap de alta calidad).
- Qué tendría que pasar para comprar antes (catalizador positivo) y qué invalidaría la tesis.

## 6. Formato de salida

1. Resumen de 2–3 líneas: veredicto, puntaje y la razón principal.
2. Tabla del scorecard: bloque, puntaje, comentario breve.
3. Por qué cayó: clasificación y explicación.
4. Semáforo de mercado (una línea por indicador).
5. Tabla de niveles de entrada y fecha límite.
6. Catalizadores con fecha.
7. Riesgos principales y qué me haría cambiar de opinión.
8. Una línea: no es asesoría financiera.
9. Sources: lista de fuentes usadas.

Si el usuario entrega varios tickers, evaluar cada uno y cerrar con una tabla comparativa (ticker, puntaje, veredicto, upside, mayor riesgo).

Si es una acción chilena, agregar: tipo de cambio, TPM del Banco Central, cobre si aplica, y recordar que la ganancia de capital de acciones con presencia bursátil ha estado históricamente exenta (art. 107), sugiriendo confirmarlo con un contador. Para acciones extranjeras, recordar que la ganancia en general tributa en el Global Complementario.