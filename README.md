# Deck Arena_Final

Unity **6000.0.70f1**. Open `Assets/Scenes/Deck Arena_Final.unity` and press Play. The scene uses the existing Starter Assets third-person controller (WASD, mouse, Shift to sprint, Space to jump).

**Total time:** approximately **6 hours**.

`Deck Greybox Map.png` is the annotated top-down capture of this final scene. Other scene variants are not the submission scene.

## Design rationale

1. A is an open defence zone with separated low cover, B uses channelled ground approaches and a local upper position, and C combines an elevated terrace with simultaneous eastern and southern pressure.
2. The intended initial split is one player at A, one at B and two at C; the second C defender is the rotating support player, not a permanently assigned role.
3. The cargo partitions and the central sightline screen prevent the starting area from becoming a position that can effectively defend all three objectives.
4. A-B has one fast, exposed shortcut across the blocks and a screened northern detour. The open A-C route uses the shortcut and joins the open B-C lane.
5. B1 and B2 mark ground defence positions for B. The upper position overlooks a local sector.
6. The long B-C route runs around the south of the central cargo and returns behind the eastern screens, avoiding both the under-platform combat area and the south spawn's east-facing exit.
7. “Screened” means less exposed to the primary firing lanes, not invulnerable: enemies can pursue players, and lost objectives can change the nearest-target flow.
8. A representative wave begins from the west and north, then adds an eastern/southern push; basic enemies maintain pressure, large enemies use the broad ground lanes to B, and shooters move out of protected entry pockets to punish static defenders and exposed upper edges.
9. Resupply sits away from the containers, forcing a choice between maintaining local defence and leaving to replenish during combat or the 20-30 second inter-wave interval.
10. The main playtest concern is the shared southern rotation: coordinated players may overuse it, so the first tuning pass should measure travel and exposure under shooter pressure before changing cover or wave timing.

## Metrics and reasons

| Metric | Value | Reason |
|---|---|---|
| Playable deck | about 65.4 x 40 m | Room for three distinct local fights without making regrouping exceed the inter-wave window. |
| Controller capsule | height 1.8 m, radius 0.28 m | Matches the supplied Starter Assets character rather than an assumed character size. |
| Walk / sprint speed | 2 / 5.335 m/s | Uses the prefab's actual values for route-time estimates. |
| Jump height / step offset | 1.2 / 0.25 m | Keeps the main rotations walkable; jumping is not required to reach another objective. |
| Slope limit | 45 degrees | Both main access ramps are below the controller limit. |
| Objective containers | 6.06 x 2.59 x 2.44 m | Approximately standard 20-foot cargo containers. |
| Container Y rotation | A: 0; B: 27.53; C: 55.06 degrees | Matches the test's stated orientation convention. |
| Basic enemy | 1.8 m high, 0.56 m wide | Uses the player's body size as the baseline. |
| Large enemy | 3.0 m high, 1.4 m wide | Creates a meaningful clearance requirement on ground approaches; do not rely on jump shortcuts. |
| Shooter | player-sized; ranged role | Can use ground firing pockets after leaving screened spawns; combat range is a wave-design parameter, not implemented AI. |
| Main ramp width | 3.0 m nominal | Supports passing and the wider enemy body; the previous access obstructions have been moved aside. |
| B / C ramp inclination | about 24 / 20 degrees | Provides readable access to +3.6 m without ladders or controller changes. |
| Upper floor | +3.6 m | Adds useful local elevation; players still have to traverse ramps or commit to a drop to disengage. |
| Low combat cover | mainly 1.1-1.2 m | Breaks incoming fire without creating full-height corridors in A. |
| New central rotation screens | 3.0 m high | Break ground-level firing lanes into the southern detour. |
| Enemy entries | 5 pockets: S-W, S-NW, S-N, S-E, S-S | Pressure comes from both sides and both ends; offset walls and canopies screen entry points. |
| Resupply | 4 points: R-A ammo + medkit, R-C ammo, R-S medkit, and the deck medkit below C3 | Each requires leaving the immediate container position. |
| Round structure | 5-8 escalating waves; 20-30 s breaks | Required encounter envelope; markers and this description stand in for wave scripts. |

Route lengths below are measured along the annotated paths between local access points, not between container centres. Times are distance/speed estimates and exclude acceleration, combat, congestion and moving around the container itself.

| Pair | Open / exposed | Long / screened | Sprint estimate: open / screened |
|---|---:|---:|---:|
| A-B | 13.1 m | 55.9 m | 2.4 / 10.5 s |
| B-C | 15.5 m | 50.5 m | 2.9 / 9.5 s |
| A-C | 36.5 m | 48.6 m | 6.8 / 9.1 s |

Walking the long routes takes approximately 24-28 seconds, so sprinting leaves a useful resupply and regrouping margin in a short break. Some routes share portions of the rotation network; these are alternatives between each pair, not six entirely independent corridors.

## Defence positions and enemy flow

Defence marker names describe the sector or role of the position. They do not rank distance to an enemy entry.

- **A1 / A2 / A3:** South Hold, West Cover, and West Watch, a northern position looking around the container toward the western approach. A4 is a two-exit fallback, not an extra objective.
- **B1 / B2 / B3:** Ramp Side, North Approach, and the +3.6 m terrace. The terrace's western firing edge is low; ground shooters can challenge that edge from a suitable flank. The northern pocket marker is an initial firing location, not the only possible shooter position.
- **C1 / C2 / C3:** South Hold, Behind Ramp at deck level, and the +3.6 m terrace. The south-facing firing opening allows return fire; the ramp is a slower retreat than simply stepping behind ground cover. C4 is a fallback.
- Basic enemies and shooters can appear in A, B and C. Large enemies are assigned to B only. The map illustrates the intended encounter flow.
- Upper access and the ground below the connecting walkway are combat space. Neither is labelled as the safe B-C rotation.
- Bulwarks define the playable perimeter. The small volumes outside the section represent boarding context, not additional playable ship areas. Falling overboard is not an intended route or tactical option; perimeter behaviour still needs a manual movement pass.

## Technical conventions

`LD_Ref_Kestrel` is an empty root object at the scene origin. The final environment retains its movement colliders and adds three separate child collider objects per environment collider, on **Hitscan (9)**, **Projectile (10)** and **Explosion (11)**. Physics layers belong to GameObjects, which is why the damage colliders are separate children. They currently share the blockout collision shape; differing explosion/weapon behaviour is for weapon integration, which is outside this test. Weapon queries should use their corresponding layer mask so overlapping collision representations are not counted as multiple hits.

## Verification and remaining review

- Earlier ground routes were sampled with the Unity player capsule. The current map distances measure the drawn paths in plan view. Traversal and timing on the A-B shortcut still need a Play Mode pass.
- The PNG uses an actual Unity orthographic capture of the saved final scene. The coloured routes and white sightline annotations explain intended flow; annotations are not a simulated wave.
- A full four-player combat playtest, controller traversal of every ramp/perimeter, and behaviour after losing a container remain to be checked in play. Enemy AI, damage, weapons and wave scripts are intentionally out of scope.

## AI assistance

I used OpenAI Codex to generate and revise the annotated map from my instructions. It also helped check route distances, polish labels and README wording, and make selected greybox and collider adjustments.

---

# Deck Arena_Final — українська версія

Unity **6000.0.70f1**. Відкрийте `Assets/Scenes/Deck Arena_Final.unity` і натисніть Play. Сцена використовує наявний контролер від третьої особи зі Starter Assets. WASD — рух, миша — огляд, Shift — спринт, пробіл — стрибок.

**Загальний час роботи:** приблизно **6 годин**.

`Deck Greybox Map.png` — знімок фінальної сцени зверху з позначеннями. Для здачі призначена сцена `Deck Arena_Final`.

## Обґрунтування дизайну

1. A — відкрита зона оборони з окремими низькими укриттями. У B підходи на палубі спрямовані перегородками й доповнені верхньою позицією. C поєднує підняту терасу з одночасним тиском зі сходу та півдня.
2. Початковий розподіл — по одному гравцю на A й B та двоє на C. Другий гравець на C підсилює інші зони за потреби.
3. Перегородки між вантажами та центральна стіна перекривають огляд так, що зі старту неможливо ефективно обороняти всі три цілі.
4. Між A й B є один швидкий відкритий зріз через блоки та захищений північний обхід. Відкритий маршрут A–C використовує цей зріз і приєднується до відкритого B–C.
5. Маркери B1 і B2 відповідають наземним оборонним позиціям B, а верхня позиція дає огляд локального сектора.
6. Довгий B–C огинає центральний вантаж із півдня й повертає за східними перегородками, оминаючи бойову зону під платформою та східний вихід південного спавну.
7. Захищений маршрут зменшує час під вогнем з основних напрямків. Вороги все одно можуть переслідувати гравців, а втрата цілі змінює напрям їхнього руху до найближчого контейнера.
8. Приклад хвилі починається з тиску із заходу й півночі, після чого додається атака зі сходу та півдня. Звичайні вороги підтримують тиск, великі рухаються широкими проходами до B, а стрільці виходять із прикритих точок появи й загрожують статичним захисникам та відкритим краям верхніх позицій.
9. Запаси винесені за межі безпосередньої оборони контейнерів. Гравці вирішують, коли залишити позицію заради поповнення — під час бою чи в перерві 20–30 секунд між хвилями.
10. На плейтесті варто перевірити, чи не користується злагоджена команда південним маршрутом надто часто. Перед зміною укриттів або таймінгів хвиль слід виміряти час переходу й час під вогнем стрільців.

## Метрики й обґрунтування

| Метрика | Значення | Обґрунтування |
|---|---|---|
| Ігрова палуба | приблизно 65,4 × 40 м | Є місце для трьох окремих боїв і перегрупування між хвилями. |
| Капсула контролера | висота 1,8 м, радіус 0,28 м | Відповідає персонажу зі Starter Assets. |
| Швидкість ходьби / спринту | 2 / 5,335 м/с | Для оцінки часу використано значення префабу. |
| Висота стрибка / висота кроку | 1,2 / 0,25 м | Між цілями можна переміщатися основними маршрутами без стрибків. |
| Граничний нахил | 45° | Нахили обох основних підйомів у межах можливостей контролера. |
| Контейнери-цілі | 6,06 × 2,59 × 2,44 м | Приблизні розміри стандартного 20-футового контейнера. |
| Поворот контейнерів навколо Y | A — 0°, B — 27,53°, C — 55,06° | Відповідає вимогам тестового завдання. |
| Звичайний ворог | висота 1,8 м, ширина 0,56 м | За основу взято розміри гравця. |
| Великий ворог | висота 3,0 м, ширина 1,4 м | Потребує ширших наземних проходів і не залежить від зрізів зі стрибками. |
| Стрілець | розмір гравця, дальня атака | Після виходу зі спавну може займати наземні вогневі позиції. Дальність атаки задається під час проєктування хвиль. |
| Ширина основних підйомів | номінально 3,0 м | Дозволяє розминутися й залишає місце для більшого персонажа. Перешкоди біля входів зміщені вбік. |
| Нахил підйомів B / C | приблизно 24° / 20° | Доступ до рівня +3,6 м без драбин і змін контролера. |
| Верхній рівень | +3,6 м | Дає локальну перевагу у висоті. Для відступу треба спуститися підйомом або зістрибнути. |
| Низькі укриття | переважно 1,1–1,2 м | Перекривають вогонь, зберігаючи відкритий характер A. |
| Центральні перегородки | висота 3,0 м | Перекривають наземні лінії вогню в бік південного обходу. |
| Точки появи ворогів | 5 точок — S-W, S-NW, S-N, S-E, S-S | Атаки йдуть із бортів та обох кінців палуби. Входи прикриті зміщеними стінами й навісами. |
| Поповнення запасів | 4 точки — боєприпаси й аптечка R-A, боєприпаси R-C, аптечка R-S та аптечка на палубі під C3 | Щоб поповнити запаси, гравець залишає безпосередню позицію біля контейнера. |
| Структура раунду | 5–8 хвиль зі зростанням складності, перерви 20–30 с | Відповідає рамкам завдання. Хвилі представлені маркерами й описом. |

Довжини виміряно вздовж ліній на карті між входами в зони цілей. Час оцінено за довжиною та швидкістю спринту, без урахування розгону, бою, скупчення гравців та обходу самого контейнера.

| Між точками | Відкритий маршрут | Довгий захищений маршрут | Оцінка спринту, відкритий / захищений |
|---|---:|---:|---:|
| A–B | 13,1 м | 55,9 м | 2,4 / 10,5 с |
| B–C | 15,5 м | 50,5 м | 2,9 / 9,5 с |
| A–C | 36,5 м | 48,6 м | 6,8 / 9,1 с |

Довгі маршрути займають приблизно 24–28 секунд ходьби. Спринт залишає час на поповнення запасів і перегрупування навіть під час короткої перерви. Частина альтернативних маршрутів має спільні відрізки.

## Оборонні позиції та рух ворогів

Назви маркерів описують сектор або роль позиції.

- **A1 / A2 / A3** — South Hold (південна оборона), West Cover (західне укриття) і West Watch (спостереження за західним підходом із північної позиції). A4 — запасна позиція з двома виходами.
- **B1 / B2 / B3** — Ramp Side (біля підйому), North Approach (північний підхід) і тераса на +3,6 м. Низький західний край тераси дозволяє наземним стрільцям атакувати з флангу. Північна точка — один із можливих початкових секторів для стрільців.
- **C1 / C2 / C3** — South Hold (південна оборона), Behind Ramp (на палубі за підйомом) і тераса на +3,6 м. Південний отвір дозволяє вести вогонь у відповідь. Відступ підйомом займає більше часу, ніж відхід за укриття на палубі. C4 — запасна позиція.
- Звичайні вороги та стрільці можуть з’являтися в A, B і C. Великі вороги призначені лише для B. Карта показує задум руху ворогів.
- Верхні підходи й палуба під перехідним містком — бойові зони. Захищений B–C прокладений південним обходом.
- Бортові огородження задають межу ігрової зони. Невеликі об’єми зовні позначають контекст висадки. Падіння за борт не передбачено як маршрут. Поведінку контролера вздовж периметра ще слід перевірити вручну.

## Технічні домовленості

`LD_Ref_Kestrel` — порожній кореневий об’єкт у початку координат сцени. Оточення зберігає колайдери руху й має по три окремі дочірні колайдери для кожного колайдера оточення. Вони належать шарам **Hitscan (9)**, **Projectile (10)** і **Explosion (11)**. Шар фізики задається на рівні GameObject, тому для кожного шару використано окремий дочірній об’єкт. Колайдери поки повторюють форму грейбоксу. Різну поведінку зброї та вибухів можна налаштувати під час інтеграції. Запити зброї мають використовувати маску свого шару, щоб збіг форм не давав кількох влучань.

## Перевірки та подальший плейтест

- Попередні наземні маршрути перевірено капсулою гравця в Unity. Поточні відстані виміряно вздовж ліній на плані. Прохідність і час на зрізі A–B ще потрібно перевірити в Play Mode.
- Основою PNG є ортографічний знімок збереженої сцени Unity. Кольорові маршрути й білі лінії видимості пояснюють задум руху та бою.
- Ще потрібні бойовий плейтест на чотирьох гравців, проходження всіх підйомів і периметра контролером та перевірка перебігу бою після втрати контейнера. AI ворогів, шкода, зброя та скрипти хвиль виходять за межі завдання.

## Використання AI

Я використала OpenAI Codex, щоб згенерувати й оновити карту з позначеннями за моїми вказівками. Він також допоміг перевірити довжини маршрутів, відредагувати підписи й формулювання в README та внести окремі правки до грейбоксу й колайдерів.
