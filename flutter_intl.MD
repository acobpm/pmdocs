1. With the app’s root directory as the current directory, generate l10n/intl_messages.arb from lib/main.dart:
```
flutter pub pub run intl_translation:extract_to_arb --output-dir=lib/l10n lib/localization.dart
```

The intl_messages.arb file is a JSON format map with one entry for each Intl.message() function defined in main.dart. This file serves as a template for the the English and Spanish translations, intl_en.arb and intl_es.arb. These translations are created by you, the developer.
2. With the app’s root directory as the current directory, generate intl_messages_<locale>.dart for each intl_<locale>.arb file and intl_messages_all.dart, which imports all of the messages files:


```
flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/l10n   --no-use-deferred-loading lib/main.dart lib/l10n/intl_zh.arb
```


3. Add all you words in localization.dart as format : 



4. Run command in root : 
```
flutter pub pub run intl_translation:extract_to_arb --output-dir=lib/l10n lib/localization.dart
```

5. Copy new added content from file intl_messages.arb to intl_en.arb 



6. Generate English version message by run :
```
flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/l10n   --no-use-deferred-loading lib/localization.dart lib/l10n/intl_en.arb
```
7. Copy new added content from file intl_messages.arb to intl_zh.arb , change corresponding words to Chinese



8. Generate English version message by run :
```
flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/l10n   --no-use-deferred-loading lib/localization.dart lib/l10n/intl_zh.arb
```
