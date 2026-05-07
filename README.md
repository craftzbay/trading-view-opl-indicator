# Opening Levels — Gaps + OPL

ICT-style TradingView indicator (Pine Script v6).

## Сурвалжууд

- **Pine v6**, NY timezone (`America/New_York`) hardcoded
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
NY-ийн чухал цагуудын open price түвшинг шугам + label-аар тэмдэглэнэ:

| Time (NY) | Default | Label |
|---|---|---|
| 00:00 | ON | `00:00` |
| 08:30 | ON | `08:30` |
| 09:30 | ON | `09:30` |
| 13:30 | OFF | `01:30` |

- Шугамын төгсгөл `timenow + 1hr` (Terminus)-д татна → бүх label нэг баганад тэгшилнэ
- Friday non-crypto: шугам Дав 00:00 хүртэл сунгана (амралтын өдрийг алгасна)
- Logic яг reference (`ICT Everything @coldbrewrosh`)-той ижил

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

- Indent rules: үргэлжлэлийн мөр 4-ийн үржвэр зайтай биш байх
- `bool` strict — `na` болохгүй, explicit cast хэрэгтэй
- `time == NYOpenTime` хатуу match `timestamp(TZ, year, month, day, hour, min, sec)` ашигласан
