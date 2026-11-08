# Engine Config 45e PAD Test

Experimental test fork of [idrive7-apps](https://github.com/cauliflowermossgrape/idrive7-apps), Engine Config 0.8.1. MIT license retained. See UPSTREAM.txt for the pinned commit and upstream-pad-change.patch (PAD logic diff before standalone identity changes) for the full changes.

Version: `0.8.1-45e-pad.2`; display name: `Engine Config 45e Test`.
Separate installed identity: `engine_config_45e_test`; separate logging ID: `E45T`. The build derives app UUID, entry point, resource IDs and package identity from the new app ID. Original Engine Config (`live_adjustments`) is not replaced. Settings belong to this separate app. Do not operate the original and test xDrive/exhaust controllers concurrently.

## Change

VIP 0F2530 uses the engine-off/PAD approach reported for a 45e in issue #96. The report does not identify its VIP index; 0F2530 comes from issue #52. This remains an experimental compatibility assumption.
Only VehicleCondition 7 (PAD), non-demo data, speed from 0 to under 0.5 km/h, and positively confirmed P permit a request. Hybrid READY is blocked even at zero ICE RPM. Like upstream, PAD takes precedence over potentially stale RPM. Parking brake must be set manually; the app cannot verify it. Double confirmation and the default-disabled xDrive setting remain.
Diagnostic bytes stay `2E DB 2F 00` to VIP 0x39. No coding, flashing or security unlock added. A positive response does not prove physical disengagement. A full ignition cycle is the upstream restoration method. ABS/stability behavior and prolonged use remain unverified.

## Repository preparation

This is a SOURCE repository overlay, not an installable .mgu/.tgz package.
Copy its CONTENTS into the root of bloodbenzine/mgu-capabilities-test, preserving any unrelated existing files. Review conflicts before overwriting an existing mgu-repo.json. The current public main branch had only README.md when inspected.
The included upstream mgu-repo.json discovers apps/* and uses kit/tools/build.py; only engine_config_45e_test is included. Renderer and controller-gate files and their original licenses are retained.
MGU Store must supply the car's genuine oap-sdk through the declared SDK integration. No SDK or fake SDK is bundled. Store installation has not been tested for this fork. If the store does not offer the prerelease as an automatic update, inspect its version/details and select it explicitly; do not assume it installed.

## Verification

Passed: node kit/tools/test_live_adjustments.js (modified vehicle simulations), node kit/tools/test_live_adjustments_exhaust.js (52 checks), python kit/tools/build.py --preview-only engine_config_45e_test, git diff --check.
No installable build or vehicle test was performed. The simulated ECU responses are test fixtures, not measurements.

## Первичная проверка на автомобиле

Проверить название Engine Config 45e Test и версию 0.8.1-45e-pad.2. Включить xDrive (experimental) в настройках. Машина неподвижна, P, стояночный тормоз включён, двигатель выключен, диагностическое зажигание PAD включено — не READY. Сначала проверить определённый VIP и условия на экране. При отказе сохранить сообщение и xdrive-diagnostic из журнала; не обходить проверки. Принятая команда сама по себе не доказывает отключение передней оси. Поведение проверять только на закрытой площадке; эта сборка экспериментальная.

## Загрузка отдельного приложения

В этом архиве папка приложения — apps/engine_config_45e_test. В корень репозитория загружается содержимое внешней папки. Не загружайте старую папку apps/live_adjustments из предыдущего архива. Если её уже загрузили в свой тестовый репозиторий, удалите именно её из репозитория, чтобы магазин не предлагал прежний вариант замены. Основное приложение на машине удалять не требуется.

Новая версия — 0.8.1-45e-pad.2. В MGU Store выбирайте Engine Config 45e Test, идентификатор engine_config_45e_test. При первом запуске настройки отдельные: xDrive (experimental) требуется включить заново. Оба приложения можно установить, но не использовать их управление одновременно.
