# ⚙️ Sterowanie przetwarzaniem w czasie rzeczywistym za pomocą `trigger()` w Spark Structured Streaming

## 🔄 Co to jest `trigger()`?

Funkcja `trigger()` pozwala kontrolować **kiedy** Spark uruchamia przetwarzanie mikrobatcha w strumieniu danych.

Domyślnie Spark przetwarza dane **tak szybko jak to możliwe** ("as fast as possible").

Dzięki `trigger()` możemy:

* regulować opóźnienie przetwarzania,
* kontrolować obciążenie systemu,
* dopasować tempo do interwałów raportowych (np. co 1 minutę).

---

## 📆 Przykład użycia:

```python
query = (df.writeStream
         .outputMode("append")
         .format("console")
         .trigger(processingTime="10 seconds")
         .start())
```

➡️ Co 10 sekund Spark sprawdza, czy są nowe dane i uruchamia batch przetwarzania.

---

## 📊 Dlaczego warto?

| Zastosowanie                | Dlaczego warto użyć `trigger()`                                 |
| --------------------------- | --------------------------------------------------------------- |
| ⏱️ Opóźnienie vs obciążenie | Możesz zmniejszyć liczbę batchy, żeby zmniejszyć obciążenie CPU |
| 📊 Raporty okresowe         | Regularne przetwarzanie co 1 min / 5 min dla BI lub alertów     |
| 🔧 Systemy testowe          | Masz pełną kontrolę kiedy i jak dane są przetwarzane            |
| ⚡ Produkcja z opóźnieniami  | Gdy źródło danych dostarcza dane z opóźnieniem                  |

---

## 📌 Typy `trigger()`:

```python
.trigger(processingTime="10 seconds")   # Najczęściej stosowane
.trigger(once=True)                      # Jeden batch (np. dla wsadowego snapshotu)
.trigger(continuous="1 second")         # Continuous processing (eksperymentalne)
```

> 🚨 Uwaga: `continuous` wymaga innego silnika i nie wspiera wielu transformacji.

---

## 📚 Przykład dydaktyczny

```python
stream.writeStream \
    .format("console") \
    .trigger(processingTime="5 seconds") \
    .outputMode("append") \
    .start()
```

➡️ Spark sprawdza nowe dane co 5 sekund i przetwarza tylko wtedy, gdy batch jest gotowy.

---

## 📅 Podsumowanie

* `trigger()` **kontroluje rytm przetwarzania strumienia**.
* Jest przydatne w testach, raportowaniu, optymalizacji wydajności.
* Łączy się dobrze z `foreachBatch` i z agregacjami czasowymi.

```python
.trigger(processingTime="15 seconds")  # np. do przetwarzania danych sprzedaży co 15 sek
```

---
