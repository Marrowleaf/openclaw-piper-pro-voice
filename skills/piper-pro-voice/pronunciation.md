# Piper TTS Pronunciation Guide

 phonetic mappings to fix common mispronunciations in Piper TTS.

## Common Issues (from rhasspy/piper GitHub issues)

### Roman Numerals & Standalone Numbers
| Input | Pronounced As | Should Be |
|-------|----------------|-----------|
| World War II | "World War roman 2" | "World War Two" |
| World War I | "World War roman 1" | "World War One" |
| III | "roman 3" | "Three" |
| IV | "roman 4" | "Four" |
| 10 | "one zero" (in some models) | "ten" |
| 11 | "one one" | "eleven" |
| 12 | "one two" | "twelve" |

*Note:* Some Piper VITS models with eSpeak-NG mispronounce numbers 10+ as separate digits. Pre-process with word forms: "ten", "eleven", "twelve".

### Currency & Numbers
| Input | Pronounced As | Should Be |
|-------|----------------|-----------|
| $500 | "Dollar five hundred" | "$500" or "five hundred dollars" |
| £50 | "Pound fifty" | "fifty pounds" |

### Abbreviations
| Input | Pronounced As | Should Be |
|-------|----------------|-----------|
| Jr. | "Jay arr" | "Junior" |
| Mr | "m r" | "Mister" |
| Mrs | "mrs" | "Missus" |
| Dr | "d r" | "Doctor" |
| St | "s t" | "Street" or "Saint" |
| Ave | "a v e" | "Avenue" |

### Names & Proper Nouns (Workarounds)
| Input | Pronounced As | Workaround |
|-------|----------------|------------|
| Geis | "gice" (wrong) | "Ghice" |
| Surname with -ge | varies | Try adding 'h' after g: "Gheis", "Ghemany" |
| St John | "saint john" or "sinjohn" | UK: "Sinjin" (proper name) |
| St. Mary | "sint mary" | Context-dependent: "Saint Mary" or "Street Mary" |
| St John | "sinjin" (UK) | Context: Saint vs street | "Saint John" or "Sinjin" |
| St. Patrick | "sin patrick" | Irish Saint | "Saint Patrick" |

### More Titles to Expand
| Input | Pronounced As | Should Be |
|-------|----------------|-----------|
| Prof. | "professor" (with pause) | "Professor" |
| Capt. | "captain" (with pause) | "Captain" |
| Gen. | "general" (with pause) | "General" |
| Lt. | "lieutenant" (with pause) | "Lieutenant" |
| Sgt. | "sergeant" (with pause) | "Sergeant" |
| Maj. | "major" (with pause) | "Major" |
| Col. | "colonel" (with pause) | "Colonel" |
| Rep. | "representative" (with pause) | "Representative" |
| Sen. | "senator" (with pause) | "Senator" |

### Years & Dates
| Input | Pronounced As | Should Be |
|-------|----------------|-----------|
| 1920 | "one thousand nine hundred and twenty" | "nineteen twenty" |
| 1908 | "one thousand nine hundred and eight" | "nineteen oh eight" |
| 90210 | "ninety thousand two hundred and ten" | "nine zero two one zero" (zip code) |

### Units of Measurement
| Input | Pronounced As | Should Be |
|-------|----------------|-----------|
| kph | "k p h" | "kilometers per hour" |
| mph | "m p h" | "miles per hour" |
| °C | "degree c" | "degrees Celsius" |
| °F | "degree f" | "degrees Fahrenheit" |
| kg | "k g" | "kilograms" |
| m² | "m 2" | "square meters" |
| km² | "k m 2" | "square kilometers" |

### Homographs (context-dependent)
| Word | Verb (like) | Noun (like) |
|------|-------------|-------------|
| live | /lɪv/ | /laɪv/ |
| lead | /liːd/ | /lɛd/ |
| record | /rɪˈkɔːrd/ | /ˈrekɔːrd/ |
| object | /əbˈdʒɛkt/ | /ˈɒbdʒɛkt/ |

**Workaround for homographs:** Since Piper cannot reliably distinguish context, pre-process text based on known usage:
- For dates/years: use "19 20" format or spelled years
- For website "live": consider "launched" or "active"
- For verb "live": use "reside" or "dwell" as alternative

## Usage

Apply these replacements in pre-processing before sending text to Piper TTS:

```python
import re

def fix_pronunciation(text):
    # Roman numerals
    text = re.sub(r'\bII\b', 'Two', text)
    text = re.sub(r'\bIII\b', 'Three', text)
    text = re.sub(r'\bIV\b', 'Four', text)
    text = re.sub(r'\bI\b', 'One', text)
    
    # Abbreviations
    text = re.sub(r'\bJr\.\s', 'Junior ', text)
    text = re.sub(r'\bMr\b', 'Mister', text)
    text = re.sub(r'\bMrs\b', 'Missus', text)
    text = re.sub(r'\bDr\b', 'Doctor', text)
    
    return text
```

## Voice-Specific Notes

- **en_US_amy_low**: Good for general use, may struggle with complex numbers
- Add model-specific mappings as discovered

## Additional Issues (from Community Reports)

### Ellipsis and Dashes
Piper often skips ellipsis ("…") and em-dashes ("—") entirely instead of treating them as pauses. Workaround: replace with period or explicit pause.

| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| "I just… I just want" | "I just I just want" | Pause between phrases | Replace "…" with "." |
| "thought—actually" | "thought actually" | Pause for dash | Replace "—" with "." |

### Decimal Fractions
Piper may interpret periods at end of sentences as decimal points. Add space after period before numbers: "5. 5" instead of "5.5"

| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| 5.5 | "five point five" | "five point five" or decimal | Add space: "5. 5"
| 3.14 | "three point fourteen" | "three point one four" | Spell out digits: "three one four"

### Years (Two-Digit Format)
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| 1920s | "one thousand nine hundred and twenties" | "nineteen twenties" | Use "19 20s" or "nineteen twenties"
| 1980s | "one thousand nine hundred and eighties" | "nineteen eighties" | Use "19 80s" or "nineteen eighties"

### Numbers in Context
When numbers have special meaning (phone numbers, zip codes, years), spell them out phonetically or use word forms.

## URLs, Acronyms & Technical Terms

| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| www.example.com | "w w w dot example dot com" | "example dot com" | Strip "www." or spell out |
| https:// | "h t t p s colon slash slash" | "https" | Remove protocol |
| FBI | "f b i" (letters) | "F B I" or "the FBI" | Spell out as "F B I" |
| NASA | "n a s a" | "NASA" (nah-sah) | Use "NASA" |
| CEO | "c e o" | "Chief Executive Officer" | Expand full title |
| PDF | "p d f" | "P D F" | Spell letter by letter |
| GIF | "g i f" or "jif" | "JIF" (debated) | Use "JIF" |
| SQL | "s q l" or "sequel" | "S Q L" | Spell "S Q L" |

### Phone Numbers
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| 555-1234 | "five hundred fifty five one two three four" | "five five five one two three four" | Add dashes, use word form |
| +44 20 7946 0958 | "plus forty four twenty seven nine four six zero nine five eight" | UK format | Use "plus four four" and spell |

### Hashtags & Social
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| #hashtag | "hashtag" | "hashtag" | Remove # in TTS |
| @username | "at username" | "username" | Remove @ |

### Technical/Linux Terms
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| sudo | "soo-doo" or "sudo" | "soo-doo" | Already good, but can force |
| ls | "l s" | "list" | Say "list" |
| cd | "c d" | "change directory" | Say "change dir" |
| mkdir | "m k d i r" | "make directory" | Say "make directory" |
| grep | "g r e p" | "grep" | Already okay |
| rm | "r m" | "remove" | Say "remove" |
| chmod | "c h m o d" | "change mode" | Say "change mode" |
| sudo apt-get | "sue-doo apt get" | "sue-doo apt get" | Keep as-is |

## Additional Issues (from Recent Research)

### Em-dashes and Ellipsis Workarounds
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| thought—actually | "thought actually" | Pause for dash | Replace "—" with "." |
| I just… I just want | "I just I just want" | Pause between phrases | Replace "…" with "." |

### Special Characters (GitHub Issue #565)
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| * | "asterisk" | (silence) | Remove or replace with space |
| **bold** | "asterisk asterisk bold asterisk asterisk" | "bold" | Strip markdown formatting |
| markdown | varies | varies | Pre-process to remove markdown symbols |

### More Common Problematic Terms
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| etc | "e t c" | "etcetera" | Expand to "etcetera" |
| eg | "e g" | "for example" | Expand to "for example" |
| ie | "i e" | "that is" | Expand to "that is" |
| vs | "v s" | "versus" | Expand to "versus" |
| approx | "a p p r o x" | "approximately" | Expand to "approximately" |
| info | "i n f o" | "information" | Usually OK, but context matters |
| photo | "ph o t o" | "photo" | Already good, may need emphasis |
| email | "e m a i l" | "email" | Already good in most voices |

### UK-Specific Pronunciations
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| Garage | "garidge" (US) | "garahj" (UK) | Voice-dependent |
| Z | "zee" (US) | "zed" (UK) | Use "zed" for UK |
| Herb | "erb" (US) | "herb" (UK) | Already good in UK voices |
| Aluminium | "aloo-minium" (US) | "al-yoo-minium" (UK) | Use phonetic "al-yoo-min-ee-um" |

### Software & Tech Terms
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| GitHub | "g i t h u b" | "git hub" | Say "git hub" |
| JavaScript | "j a v a s c r i p t" | "javascript" | Already generally OK |
| Python | "p y t h o n" | "python" | OK in most cases |
| HTTP | "h t t p" | "H T T P" or "aitch-tee-tee-pee" | Spell out |
| API | "a p i" | "A P I" or "ay-pee-eye" | Spell out |
| GPU | "g p u" | "G P U" | Spell out |
| CPU | "c p u" | "C P U" | Spell out |

## Additional Issues (from GitHub Issues #711, #363)

### More Problem Words from Community Reports
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| Data | "day-ta" or "da-ta" | "day-ta" (US) or "dah-ta" (UK) | Usually context-dependent |
| Router | "row-ter" | "rou-ter" | Use "rou-ter" spelling |
| Agile | "aj-ul" or "aj-ile" | "aj-ile" | Emphasize second syllable |
| HTTP | "h t t p" | "aitch tee tee pee" | Spell out letters |
| HTTPS | "h t t p s" | "aitch tee tee pee ess" | Spell out letters |
| PDF | "p d f" | "pee dee eff" | Spell out letters |
| GIF | "g i f" or "jif" | "jif" (common) or "g i f" | Use "JIF" for common pronunciation |
| Cache | "cash" | "cash" (correct) or "cah-shay" | Already correct in most cases |
| Azure | "az-yur" | "az-hure" | UK: "az-yoo" |
| Regex | "rej-ex" | "rej-ex" or "reg-ex" | Either works |
| sudo | "soo-doo" | "soo-doo" | Already good |

### Surname Pronunciation Tips
Many surnames get mispronounced. Try these patterns:
- Add 'h' after soft 'g': "Gharrison" instead of "Garrison"
- Use 'ph' for 'f' sound: "Philip" not "Filp"
- Double vowels for long sounds: "Paige" vs "Page"

### Long Text Issues (GitHub Issue #211)
| Issue | Description | Workaround |
|-------|-------------|------------|
| Mumbling after long text | Piper starts mumbling or becomes unintelligible after ~1-2 minutes of continuous text | Break long text into shorter segments with explicit pauses |
| Flat streaming audio | Streaming output via pipe sounds flatter than file output | Use file output when possible (GitHub Issue #698) |

### Punctuation in Python (GitHub Issue #122)
| Issue | Description | Workaround |
|-------|-------------|------------|
| Punctuation not read | Commas, periods not producing pauses in Python API | Use `piper_phonemize` or install patched espeak-ng fork |
| Voice-dependent punctuation | Some voices were trained without punctuation data | Use expressive voices (e.g., en_US-amy-medium, en_US-libritts-high) |

### General Sound Quality Notes
- Piper is a neural TTS, but some users report it sounds "flat" or "jumpy" compared to modern cloud TTS
- This is a model limitation, not fixable via text preprocessing
- Consider higher-quality voices like en_US-libritts-high for better results

---

## Contributing

Add new problematic words here with:
- The original text
- How it's currently pronounced
- The intended pronunciation
- Suggested phonetic override if known