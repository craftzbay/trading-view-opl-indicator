# Opening Levels — Gaps + OPL

ICT-style TradingView indicator (Pine Script v6).

## Сурвалжууд

- **Pine v6**, timezone сонгомжтой (General → Timezone, default `America/New_York`; `Exchange` = symbol-ийн бирж)
- Зөвхөн **intraday** TF (1m–30m)-д OPL ажиллана
- Бүх timeframe-д HTF Opening Gaps, HTF Candles, Notes/Checklist ажиллана

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
- Session open-ийг 1-минутын session window (`time(tf, "0830-0831:1234567", TZ)`)-оор илрүүлнэ → зөвхөн **жинхэнэ open бар** дээр асна, gap-ийн дараах resume бар дээр **худал асахгүй**
- Шугамыг тэр барын `time`/`open` дээр зангидна (огнооны тооцоо байхгүй); баруун төгсгөл `timenow + 1hr` (Terminus)-д татагдана → label-ууд нэг баганад тэгшилнэ

### NOTES (Top Center)
- 2 мөрт текст: Title (size.huge) + Subtitle (size.normal)
- Тус бүр checkbox + text input + color picker
- Default color: `#000000`

### SETUP CHECKLIST (Top Right)
- 5 ширхэг checklist item, тус бүрд label + checkbox
- ☑ ногоон = OK, ☐ саарал = pending
- Default labels: Bias confirmed / Liquidity taken / FVG aligned / Entry confirmed / Risk OK

### HTF CANDLES
Дээд timeframe-ийн лаануудыг price action-ы баруун талд зурна (timeframe солихгүйгээр HTF структур харах):

- **5 хүртэл HTF set** — тус бүр checkbox + timeframe + candle count (settings-д Weekly→15m дарааллаар)
- Зураг дээр: **жижиг TF price-д ойр → том TF баруун зах руу** (15m … Weekly)
- Default: **Daily** (1 лаа), **4H** (3 лаа), **1H** (5 лаа) асаалттай; Weekly + 15m унтраалттай
- `request.security`-ээр set бүрийн OHLC татаж, сүүлийн N laaг box (body) + line (wick)-ээр зурна
- Хамгийн баруун лаа = одоо **үүсэж буй** (live) HTF лаа, бодит цагт шинэчлэгдэнэ
- Ерөнхий тохиргоо: padding (live бараас зай, default 30), bull/bear/border/wick өнгө (лааны өргөн=2, зай=1 hardcode), label
- **Label**: блок бүрийн дээр TF нэр (`240`→`4H`, `60`→`1H`, `15`→`15m`, D/W/M хэвээр)
- `barstate.islast`-д бүх object устгаад дахин зурна; Pine-ийн +500 барын future хязгаарт багтаахаар лаа/set-ийг автоматаар таслана
- Chart TF-ээс **доош буюу тэнцүү** HTF set автоматаар нуугдана (`timeframe.in_seconds` харьцуулалт): ≤-chart `request.security` нь chart-ийн датаг буцаадаг тул блок давхардахаас сэргийлнэ (жишээ chart 4H → HTF 1H унтарна, chart 1H → HTF 1H унтарна)

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
- Session-ийг `time(tf, "HHMM-HHMM:1234567", TZ)` window-оор илрүүлнэ — crossing (`time >= openTs`) ашиглаж **болохгүй**: gap дээр resume бар дээр худал асаж шугамыг буруу нээлт рүү зангиддаг
- OPL объектуудыг `var array<Opl>`-д хадгалж, `Opl` UDT-ийн line/label field-ийг in-place мутаци хийдэг
