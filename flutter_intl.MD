1. With the app’s root directory as the current directory, generate l10n/intl_messages.arb from lib/main.dart:
```
flutter pub pub run intl_translation:extract_to_arb --output-dir=lib/l10n lib/localization.dart
```

The intl_messages.arb file is a JSON format map with one entry for each Intl.message() function defined in main.dart. This file serves as a template for the the English and Spanish translations, intl_en.arb and intl_es.arb. These translations are created by you, the developer.

* With the app’s root directory as the current directory, generate intl_messages_<locale>.dart for each intl_<locale>.arb file and intl_messages_all.dart, which imports all of the messages files:


```
flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/l10n   --no-use-deferred-loading lib/main.dart lib/l10n/intl_zh.arb
```


* Add all you words in localization.dart as format : 



* Run command in root : 
```
flutter pub pub run intl_translation:extract_to_arb --output-dir=lib/l10n lib/localization.dart
```

* Copy new added content from file intl_messages.arb to intl_en.arb 



* Generate English version message by run :
```
flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/l10n   --no-use-deferred-loading lib/localization.dart lib/l10n/intl_en.arb
```
* Copy new added content from file intl_messages.arb to intl_zh.arb , change corresponding words to Chinese



* Generate English version message by run :
```
flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/l10n   --no-use-deferred-loading lib/localization.dart lib/l10n/intl_zh.arb
```
