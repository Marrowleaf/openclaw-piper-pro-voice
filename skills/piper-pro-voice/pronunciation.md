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

### Decimal Fractions & Number-Period concatenation
Piper may interpret periods at end of sentences as decimal points. Add space after period before numbers: "5. 5" instead of "5.5"

**Number-Period concatenation issue:** When a number is followed by a period (end of sentence), the next word can be mispronounced letter-by-letter. E.g., "12345.Let me check" becomes "12345 dot l e t me check".

| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| 12345.Let me check | "one two three four five dot l e t me check" | "12345. Let me check" | Add space: "12345. Let me check" |
| 99.Yes | "ninety nine dot w y e s" | "99. Yes" | Add space: "99. Yes" |

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
| Schedule | "sked-yoo-l" (US) | "shed-yoo-l" (UK) | Use UK voice |
| Privacy | "pry-vuh-see" (US) | "pry-vuh-si" (UK) | Voice-dependent |
| Mobile | "moe-bile" (US) | "moe-byle" (UK) | Already good in UK voices |

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
| npm | "n p m" | "N P M" or "en-pee-emm" | Spell out letters |
| Docker | "d o c k e r" | "docker" | Already generally OK |
| Kubernetes | "k u b e r n e t e s" | "koo-ber-net-ees" | Use phonetic "koo-ber-net-ees" |

## Additional Issues (from GitHub Issues #122, #756)

### Python API Punctuation Fix (piper_phonemize)
Piper's Python API often drops punctuation marks, making speech sound robotic. The `piper_phonemize` tool can fix this:

| Issue | Description | Solution |
|-------|-------------|----------|
| Punctuation not read in Python | Commas, periods not producing pauses | Use `piper_phonemize` from piper_phonemize package |
| System espeak-ng used | Python uses system espeak-ng instead of patched version | Install piper_phonemize: `pip install piper_phonemize` |

*Note:* Even with the patched espeak-ng, the Python code needs `piper_phonemize` to preserve punctuation. See GitHub Issue #122 for details.

### Numbers 10+ Fix (GitHub Issue #756)
Some Piper VITS models pronounce numbers 10+ as separate digits ("10" → "one zero"). This is an eSpeak-NG integration issue:

| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| 10 | "one zero" | "ten" | Pre-process: "ten" |
| 25 | "two five" | "twenty-five" | Pre-process: "twenty-five" |
| 100 | "one zero zero" | "one hundred" | Pre-process: "one hundred" |

*Note:* This issue is model-dependent. Some models handle numbers correctly while others require text pre-processing.

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
| WiFi | "why-fye" or "wee-fee" | "wee-fee" | Use "wee-fee" |
| Router | "row-ter" | "rou-ter" | Use "rou-ter" |
| Ethernet | "ee-ther-net" | "ee-ther-net" | Already good |
| Server | "sar-ver" | "sar-ver" | Already good |
| Database | "day-tuh-base" | "day-tuh-base" | Already generally OK |
| XML | "x m l" | "ecks-em-ell" | Spell out |
| JSON | "j-son" | "jay-son" | Use "jay-son" |
| YAML | "yam-ul" | "yam-ul" | Already good |
| OAuth | "oh-auth" | "oh-awth" | Use "oh-awth" |
| POSIX | "pos-ix" | "poh-zix" | Spell out as "P-O-S-I-X" |

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

### Time Formats (from Community Reports)
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| 2:30 | "two thirty" | "half past two" | Pre-process to "half past two" |
| 10:15 | "ten fifteen" | "quarter past ten" | Pre-process to "quarter past ten" |
| 2:45 | "two forty-five" | "quarter to three" | Pre-process to "quarter to three" |
| 1:30 | "one thirty" | "half past one" | Pre-process to "half past one" |
| 9:00 | "nine o'clock" | "nine o'clock" | Already good in most cases |

*Note:* Piper reads times digit-by-digit or as regular numbers. For natural time announcements, pre-process to spoken phrases.

### Emojis and Special Characters (from Community Reports)
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| 😄 | "blank" or skipped | (silence) | Remove emojis before TTS |
| 🎉 | "blank" or skipped | (silence) | Remove emojis before TTS |
| 👋 | "blank" or skipped | (silence) | Remove emojis before TTS |
| ℹ️ | "information" | (silence) | Remove emoji variation selectors |
| ™️ | "trademark" | (silence) | Remove before TTS |
| ✅ | "checkmark" or skipped | (silence) | Remove before TTS |

*Note:* Piper treats emoji as silence or skips them entirely. Strip all emojis from text before TTS processing.

### Additional Problematic Characters
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| @ | "at" | (silence) | Remove before TTS (except in email addresses) |
| # | "number" or "hash" | (silence) | Remove before TTS (hashtags) |
| % | "percent" | (silence) | Keep for percentages but remove otherwise |
| & | "and" | "and" | Already generally good |
| * | "asterisk" | (silence) | Remove or replace with space |
| ( ) | "left paren" / "right paren" | Pauses | Use spaces around parentheses |

### General Sound Quality Notes
- Piper is a neural TTS, but some users report it sounds "flat" or "jumpy" compared to modern cloud TTS
- This is a model limitation, not fixable via text preprocessing
- Consider higher-quality voices like en_US-libritts-high for better results

### Long Text Issues (GitHub Issue #211)
Piper can become unintelligible after 1-2 minutes of continuous speech:

| Issue | Description | Workaround |
|-------|-------------|------------|
| Mumbling after long text | Piper starts mumbling or becomes unintelligible after ~1-2 minutes of continuous text | Break long text into shorter segments with explicit pauses |
| Flat streaming audio | Streaming output via pipe sounds flatter than file output | Use file output when possible (GitHub Issue #698) |

*Note:* For long notifications, split into multiple shorter segments (e.g., < 30 seconds each) for better clarity.

---

## Additional Issues (from GitHub PR #401 - IPA Phoneme Input)

Piper now supports direct IPA phoneme input using double bracket syntax:

| Input | Description | Usage |
|-------|-------------|-------|
| [[dʒeɪs]] | Direct IPA phonemes | Use for proper nouns that defy phonetic spelling |
| [[ˈhaɪˌkəːləf]] | Multi-syllable IPA | Complex words may need IPA |

*Note:* This feature requires building from the `phoneme-in-text` branch or using piper-plus. Check [PR #401](https://github.com/rhasspy/piper/pull/401) for details.

### IPA Phoneme Tips
- Get IPA for words using: `espeak -q --ipa="word"`
- For names that won't render correctly, IPA is the most reliable fix
- Example: "Geis" → IPA-based workaround or "Ghice" (phonetic spelling)

---

## Additional Issues (from GitHub Issue #252 - Single Word Intonation)

Piper often mispronounces single short words when spoken alone. This is a known training data limitation:

| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| yes | Garbled/robotic | "yes" | Add context: "yes, that's correct" |
| no | Garbled/robotic | "no" | Add context: "no, thank you" |
| what | Garbled/robotic | "what" | Add context: "what is that" |
| dog | Garbled/robotic | "dog" | Add context: "the dog" |
| busy | Garbled/robotic | "busy" | Add context: "I'm busy" |
| me | Garbled/robotic | "me" | Add context: "ask me" |
| it | Garbled/robotic | "it" | Add context: "it works" |
| us | Garbled/robotic | "us" | Add context: "join us" |

*Note:* Single-syllable words often sound garbled because Piper's training data lacked sufficient single-word examples. The workaround is to repeat the word or add surrounding context (e.g., "yes yes" or "yes, I agree").

### Why Single Words Sound Robotic
From GitHub Issue #252: The training samples didn't have enough short lines, so the model wasn't able to learn short word intonation. This is a model training issue, not a text preprocessing fix.

### Best Practice for Single Words
When using Piper for voice notifications:
- Never send single short words alone
- Always wrap in a sentence: "Your order is ready" instead of "ready"
- Repeat important words: "Warning, warning" for critical alerts

---

### Sentence Type Intonation (from GitHub Discussion #477)
| Input | Pronounced As | Should Be | Workaround |
|-------|----------------|-----------|------------|
| "Are you going?" | Flat intonation (sounds like statement) | Rising intonation for questions | Add context or use voice with expressive training |
| "What a great day!" | Flat intonation | Exclamatory emphasis | Use higher-quality expressive voice |
| Questions overall | Statement-like | Proper question rising tone | Some voices lack this training data |

*Note:* Question and exclamation intonation largely depends on whether the voice dataset included those features. Some custom-trained voices handle this better. This is a voice quality issue, not fixable via text preprocessing.

---

## Contributing

Add new problematic words here with:
- The original text
- How it's currently pronounced
- The intended pronunciation
- Suggested phonetic override if known