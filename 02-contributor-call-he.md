# GitHub — פוסט 2 מתוך 3 · קריאה לתורמים / "good first issues"

**שימוש:** דיון נעוץ ("תרומה ו-good first issues") או כמבוא לקובץ CONTRIBUTING.md.
**מילות מפתח:** קוד פתוח, תרומה, good first issue, i18n, לוקליזציה, OCR, Python, TypeScript, Vitest, pytest, נגישות, UAP, נתונים פתוחים
**היפר-קישורים:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## תרומה ל-ufolens.com

[ufolens.com](https://www.ufolens.com) הופך את [ארכיון ה-PURSUE UAP](https://www.war.gov/ufo) של מחלקת המלחמה של ארה"ב לפלטפורמה רב-לשונית וניתנת לחיפוש עם [API ציבורי](https://www.ufolens.com/api/v1). הוא מורכב משני חצאים — pipeline קליטת נתונים (ingest) מקומי ב-Python (`pipeline/`) ואפליקציית קצה (edge) ב-TypeScript/Hono (`worker/`) — שנפגשים בממשק אחד: חבילת SQL + נכסים שפורסמה.

אין צורך באישורי גישה לענן כדי לתרום. מודולי הליבה של ה-pipeline משתמשים בספרייה הסטנדרטית בלבד (stdlib-only) והבדיקות של ה-Worker רצות מול אחסון בזיכרון (in-memory).

### הגדרה

```bash
# pipeline
python3 -m pytest pipeline/tests/          # אמור להציג הכל ירוק, ללא צורך ב-pip install

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### היכן עזרתכם תהיה מועילה ביותר

**i18n / לוקליזציה** — `worker/src/i18n/ui-strings.json` הוא קובץ המקור למחרוזות (strings) של ממשק המשתמש. סקירה של דובר שפת אם עבור כל שפה שאינה אנגלית היא בעלת ערך גבוה: איתור פלט מכונה מסורבל, תיקון בעיות RTL/פריסה, ושיפור מקרי קצה במשא ומתן על שפה (language-negotiation).

**איכות OCR** — עיבוד-קדם (pre-processing) טוב יותר של סריקות ישנות ממכונת כתיבה לפני OCR; רתמת הערכה (evaluation harness) המשווה את מנוע הקוד הפתוח לחלופת ה-Tesseract על דפים לדוגמה.

**נגישות** — ביצוע ביקורת (audit) לדפים המעובדים (rendered pages) (`worker/src/render/`) בהתאם לתקן WCAG; ה-CSP הוא מחמיר (ללא `unsafe-inline`), ולכן פתרונות חייבים לעבוד במסגרת זו.

**ארגונומיית API** — `worker/src/routes/` — עימוד (pagination), סינון, תיאור OpenAPI, ולקוחות לדוגמה.

**עמידות ה-Pipeline** — נתיבי כשל-חינני (graceful-degradation) נוספים, דיווח התקדמות משופר, ומקרי קצה בזיהוי דלתא (`pipeline/lib/delta.py`).

**תיעוד** — `docs/20260511/` (繁體中文; `00-*` הוא האינדקס). תרגומים של מסמכי התכנון לאנגלית יתקבלו בברכה.

### כללי יסוד

- כל הנתיבים יחסיים — הפרויקט חייב להיות נייד בין מכונות. אין להשתמש בנתיבים מוחלטים המקודדים באופן קשיח.
- אין להוסיף תלות pip למודול *ליבה* ב-pipeline. שלבים אופציונליים עשויים להשתמש בחבילות אופציונליות, וחייבים לספק כשל-חינני בלעדיהן.
- אין להחליש את מכונת המצבים המתקדמת-קדימה-בלבד — זוהי תקרת העלות.
- אין להוסיף סמלים רשמיים של ממשלת ארה"b, ואין להוסיף דבר המבטל צנזורה (redactions) מהמקור.
- שינויי סכמה ב-D1 נוגעים ב**שני** קבצים: `pipeline/lib/manifest_schema.sql` ו-`db/schema.sql`.
- בדיקות עם קוד חדש. הודעות commit בסגנון Conventional Commits.

קראו תחילה את `CLAUDE.md` ואת `docs/20260511/00-*`, ואז פתחו issue כדי לדון בכל שינוי מבני לפני שליחת PR.

