# Opening Levels — Gaps + OPL

ICT-style TradingView indicator (Pine Script v6).

## Сурвалжууд

- **Pine v6**, timezone сонгомжтой (General → Timezone, default `America/New_York`; `Exchange` = symbol-ийн бирж)
- Зөвхөн **intraday** TF (1m–30m)-д OPL ажиллана
- Бүх timeframe-д HTF Opening Gaps болон Notes/Checklist ажиллана

## Sections

### HTF OPENING GAPS
3 ширхэг gap zone (2 ray + 90% transparent box):

| Gap | Detection | Reset | Color |
|---|---|---|---|
| MONTHLY | прев month close → cur month open | new month | `#4a148c` |
| WEEKLY | прев week close → cur week open | new week | `#0c3299` |
| DAILY | прев day close → cur day open | new week | `#006064` |

**Priority on overlap**: MONTHLY > WEEKLY > DAILY (smaller skipped if bigger fires).

### OPENING PRICE LINES
Сонгосон timezone-ийн чухал цагуудын open price түвшинг шугам + label-аар тэмдэглэнэ.
4 session нэг л `Opl` model-оор давталтаар зурагдана (hr/mn нь кодод тогтмол):

| Time | Default | Label (засварлаж болно) |
|---|---|---|
| 00:00 | ON  | `00:00` |
| 08:30 | ON  | `08:30` |
| 09:30 | ON  | `09:30` |
| 13:30 | OFF | `01:30` |

- Зөвхөн **intraday** (`multiplier ≤ 31`)-д ажиллана
- Session open-ийг `time`-ийн crossing-аар илрүүлнэ → ямар ч intraday TF дээр найдвартай (зэрэгцээгүй TF дээр ч хамгийн ойрын бараар зурна)
- Шугамын баруун төгсгөл `timenow + 1hr` (Terminus)-д татагдана → бүх label нэг баганад тэгшилнэ

### NOTES (Top Center)
- 2 мөрт текст: Title (size.huge) + Subtitle (size.normal)
- Тус бүр checkbox + text input + color picker
- Default color: `#000000`

### SETUP CHECKLIST (Top Right)
- 5 ширхэг checklist item, тус бүрд label + checkbox
- ☑ ногоон = OK, ☐ саарал = pending
- Default labels: Bias confirmed / Liquidity taken / FVG aligned / Entry confirmed / Risk OK

## Хэрэглэх заавар

1. TradingView нээж Pine Editor нээ (chart-ын доор)
2. New blank indicator → `daily-opening-gaps.pine`-ийн агуулга paste хий
3. Save → Add to chart
4. Settings нээгээд бүх group-ийг тохируулж болно

## Файлын бүтэц

```
indicator/
├── daily-opening-gaps.pine   ← үндсэн indicator (Pine v6)
├── ict.pine                  ← reference indicator (ICT Everything @coldbrewrosh, Pine v5)
└── README.md
```

## Pine v6 анхааруулга

- Indent rules: үргэлжлэлийн мөр 4-ийн үржвэр зайтай биш байх (options массив 5 зайтай)
- `bool` strict — `na` болохгүй, explicit cast хэрэгтэй
- Session цагийг `timestamp(TZ, year(time,TZ), month(time,TZ), dayofmonth(time,TZ), hr, mn, 0)` + crossing-аар илрүүлнэ — огноог **заавал тухайн TZ-оор** уншина (биржийн tz-той хольвол өдөр гулсаж шугам буруу бар дээр ангидна)
- OPL объектуудыг `var array<Opl>`-д хадгалж, `Opl` UDT-ийн line/label field-ийг in-place мутаци хийдэг
