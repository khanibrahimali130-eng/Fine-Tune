# clip_request_to_json

## Task
Turn a casual message about making video clips (English, Roman Urdu or mixed, with typos) into one JSON object. Two people would write the same correct answer, so it can be scored by exact match.

## Output format
Exactly one line of JSON, same keys in the same order every time:

{"intent":"make_clips","source":"youtube","duration_sec":45,"num_clips":3,"topic":"budgeting"}

- intent: "make_clips" or "other"
- source: "youtube", "upload" or null
- duration_sec: integer seconds or null (1 minute = 60, half a minute = 30)
- num_clips: integer or null ("a clip" = 1)
- topic: short lowercase phrase or null
- Missing info = null. Not a clip request (off-topic or unclear) = intent "other" and all other keys null.

## How the data was made
- 192 examples total: 132 make_clips, 60 other (ratio 2.2 to 1, under the 3x limit).
- Variety: English and Roman Urdu, short and long, typos, lowercase and shouting, polite and casual.
- Edge cases (about 45%): missing source/duration/count/topic, number words ("two", "teen"), a 5-minute and a 90-second request, 60 off-topic or unclear messages ("ok", "same as before", weather, jokes).
- The 120 main make_clips examples were AI-drafted from templates (a script combining openers, topics, durations, counts and sources), and the 72 edge cases were AI-written by hand. I checked and fixed every example myself.
- All data is fake. There are no real names, phones or addresses.
- Split: 154 train, 19 val, 19 test (random, seed 7). test.jsonl is not edited after creation.

## Files
system_prompt.txt, examples.csv, train.jsonl, val.jsonl, test.jsonl

## 3 sample examples
1. "pls cut 3 clpis abt budgeting 45 secnds each from yt vid" -> {"intent":"make_clips","source":"youtube","duration_sec":45,"num_clips":3,"topic":"budgeting"}
2. "ek minute ka clip bana do study tips par" -> {"intent":"make_clips","source":null,"duration_sec":60,"num_clips":1,"topic":"study tips"}
3. "haan wahi wala" -> {"intent":"other","source":null,"duration_sec":null,"num_clips":null,"topic":null}
