---
layout: post
title: "ios localization"
date: "2018-02-26 10:53:34 +0100"
---

### Internationalization vs localization

- Internationalization: process of making your app able to adapt to different languages, regions, and cultures.
- Localization: process of translating your app into multiple languages.

## Internationalization of UI resources vs code resources

### [UI Internationalization](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
1. enable Base Internationalization in the project info settings.
2. add localizations languages. this creates ```xxx.lproj``` folders containing the strings that need translation in the storyboard and xib files.

### [Internationalizing Your Code](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourCode/InternationalizingYourCode.html)

- Make sure all strings needing localization are declared with ```NSLocalizedString()```.
Example for an alert message:
```Swift
let alert = UIAlertController(title: NSLocalizedString("Attention", comment: "titre popup erreur compte utilisateur"), message: NSLocalizedString("Vous êtes sur le point de supprimer une classe", comment: "popup avertissement suppression classe"), preferredStyle: UIAlertControllerStyle.alert)
alert.addAction(UIAlertAction(title: NSLocalizedString("Annuler", comment: "suppression classe"), style: UIAlertActionStyle.default, handler: { (alert: UIAlertAction!) in
    self.showSuppressButtons = false
```

- Using the ```genstrings``` command line tool, extract all localized strings from your code:
```bash
find . -name \*.swift | xargs genstrings -o .
```
This creates a ```Localizable.strings``` file containing the strings to translate. The above code example will generate the following file:

```Swift
/* titre popup erreur compte utilisateur */
"Attention" = "Attention";

/* popup avertissement suppression classe */
"Vous êtes sur le point de supprimer une classe" = "Vous êtes sur le point de supprimer une classe";

/* suppression classe */
"Annuler" = "Annuler";
```
- Do the actual translation in the ```Localizable.strings``` file :

```Swift
/* titre popup erreur compte utilisateur */
"Attention" = "Warning";

/* popup avertissement suppression classe */
"Vous êtes sur le point de supprimer une classe" = "You are about to delete a class account";

/* suppression classe */
"Annuler" = "Cancel";
```
- Save the file in your ```xxx.lproj``` folder for the appropriate language. (```en.proj``` in this case)

# Resources
- [quick tutorial](https://medium.com/lean-localization/ios-localization-tutorial-938231f9f881)
- [ios localization - The Ultimate Guide to the Right Developer Mindset](https://medium.com/best-of-i18n/ios-localization-the-ultimate-guide-to-the-right-developer-mindset-d4749b04b990)
- [localizing text from API](http://alejandromp.com/blog/2017/6/24/loading-translations-dynamically-generating-localized-string-runtime/)
- [Apple docs](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i-CH1-SW1)
