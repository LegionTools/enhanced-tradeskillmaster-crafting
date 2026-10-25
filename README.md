# TradeSkillMaster_Crafting (Enhanced)

An enhanced fork of TradeSkillMaster's Crafting module. All credit for the original addon goes to the TradeSkillMaster team; this fork adds inventory-tracking infrastructure and fixes a real data bug on top.

## What changed

- **`ItemCountTracker` module** — polls `TSMAPI.Inventory:GetTotalQuantity` for every item with a known recipe (ingredients) and every item in a TSM group, throttled write-on-change into a new `TSM.db.factionrealm.itemCounts` field (same pattern as the stock `GoldTracker.lua`). This exists to feed inventory-aware features in a companion desktop app (stock-aware craft margins, days-to-clear estimates, etc.) — core TSM tracks market prices but not actual bag/bank quantities over time.
- **Fixed: `ItemTransaction.price` was being treated as a transaction total everywhere it was read, but TSM actually stores it per-unit.** This had been silently undercounting revenue/cost calculations for every transaction with quantity > 1 since the field was first written. Found via an impossible-looking cost basis on a real item (45g cost vs 349g market value) and confirmed against TSM's own `Data.lua`/`Mail.lua`/`Merchant.lua` source, which all divide the real total by item count before storing `price`.
- A `KnownRecipeTracker` variant (polling `IsSpellKnown` to disambiguate which of several same-output recipe variants a character actually knows) was tried and reverted — `IsSpellKnown` does not reliably reflect profession recipe knowledge on this server (confirmed live: it returned false for every one of 681 tested recipes against a character who should have known several).

## Installation

Drop this folder into `Interface/AddOns/TradeSkillMaster_Crafting`, replacing the stock module. Requires core TradeSkillMaster to be installed as usual.

---

# TradeSkillMaster_Crafting (улучшенная версия)

Улучшенный форк модуля Crafting из TradeSkillMaster. Вся заслуга за оригинальный аддон принадлежит команде TradeSkillMaster; этот форк добавляет инфраструктуру отслеживания инвентаря и исправляет реальную ошибку в данных.

## Что изменено

- **Модуль `ItemCountTracker`** — опрашивает `TSMAPI.Inventory:GetTotalQuantity` для каждого предмета с известным рецептом (ингредиенты) и каждого предмета в группе TSM, записывая с ограничением частоты изменения в новое поле `TSM.db.factionrealm.itemCounts` (та же схема, что и у штатного `GoldTracker.lua`). Нужно для питания функций отдельного десктоп-приложения, учитывающих реальные остатки (маржа крафта с учётом запаса, оценка дней до распродажи и т.д.) — базовый TSM отслеживает рыночные цены, но не реальное количество на руках/в банке со временем.
- **Исправлено: `ItemTransaction.price` везде читался как общая сумма транзакции, хотя TSM на самом деле хранит его за единицу товара.** Это незаметно занижало расчёты выручки/себестоимости для каждой транзакции с количеством > 1 с момента первой записи этого поля. Найдено через нереалистичную себестоимость реального предмета (45з закупка против 349з рыночной цены) и подтверждено по исходникам самого TSM (`Data.lua`/`Mail.lua`/`Merchant.lua`), которые все делят реальную сумму на количество перед записью в `price`.
- Был опробован и откачен вариант `KnownRecipeTracker` (опрос `IsSpellKnown` для различения, какой именно из нескольких вариантов рецепта с одинаковым результатом реально известен персонажу) — `IsSpellKnown` ненадёжно отражает знание рецептов профессий на этом сервере (подтверждено вживую: вернул false для всех 681 проверенных рецептов у персонажа, который точно должен был знать несколько из них).

## Установка

Поместите эту папку в `Interface/AddOns/TradeSkillMaster_Crafting`, заменив штатный модуль. Требует установленного базового TradeSkillMaster, как обычно.
