
তুমি আগের Effort-এর Hours-এর সঙ্গে নতুন Story Point-এর Hours যোগ করে হিসাব করতে চাচ্ছ।

তোমার নিয়ম অনুযায়ী:

| Effort | Story Point | Hours Calculation | Total Hours |
| ------ | ----------- | ----------------- | ----------- |
| XS     | 0.5         | 0.5               | 0.5         |
| S      | 1           | 1 + 0.5           | 1.5         |
| M      | 2           | 2 + 1.5           | 3.5         |
| L      | 3           | 3 + 3.5           | 6.5         |
| XL     | 5           | 5 + 6.5           | 11.5        |
| XXL    | 8           | 8 + 11.5          | 19.5        |
| XXXL   | 13          | 13 + 19.5         | 32.5        |

### Formula

```text
Hours=Story Point+Previous Size Hours
```


অর্থাৎ, প্রতিটি নতুন Effort-এর Hours হবে তার Story Point + আগের Effort-এর Total Hours।

মনে রাখবে: এটা তোমার নির্ধারিত custom estimation formula। Agile বা Scrum-এ Hours হিসাব করার কোনো বাধ্যতামূলক universal formula নেই।