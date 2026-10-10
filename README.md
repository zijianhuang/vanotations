# Overview

This project provides [visual acuity notations side by side](https://zijianhuang.github.io/vanotations/) as an alternative to traditional printed conversion tables, such as:

* [Visual Acuity Ranges and Visual Acuity Notations (Colenbrander, ResearchGate)](https://www.researchgate.net/profile/August-Colenbrander/publication/255022724/figure/tbl4/AS:667833218392071@1536235255216/sual-Acuity-Ranges-and-Visual-Acuity-Notations.png)

The [Visual Acuity Conversion Chart](https://zijianhuang.github.io/vanotations/) delivered by this project includes a dynamic converter for arbitrary VA values. 

**Hints:**
* You may save [Visual Acuity Conversion Chart](https://zijianhuang.github.io/vanotations/) to a local HTML file which can be working well locally as long as the browser supports JavaScript.
* Visual Acuity testing for distance vision can be done through smartphone app Eye Charts:
<p><a href="https://apps.apple.com/au/app/eye-charts/id1442509969?ign-itscg=30200&amp;ign-itsct=apps_box_badge" target="_blank" style="color: rgb(13, 110, 253); display: inline-block; overflow: hidden; border-radius: 13px; width: 125px; height: 41.5px;"><img src="https://tools.applemediaservices.com/api/badges/download-on-the-app-store/black/en-us?size=125x41.5" alt="Download on the App Store" style="height: 41.5px; border-radius: 13px; width: 125px;"></a></p>

<p><a href="https://play.google.com/store/apps/details?id=com.fonlow.VAC" target="_blank" style="color: rgb(13, 110, 253);"><img alt="Get it on Google Play" src="https://play.google.com/intl/en_us/badges/images/badge_new.png"></a></p>


**Remarks:**
* The conversation table was generated with the AI coding assistant Claude.


# Prompt to Claude

Based on LogMar Scaling, I have the following base values for different rows of visual acuity chart presented in JavaScript:
```
const logMars = [1, 0.9, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3, 0.2, 0.1, 0, -0.1, -0.2m, -0.3];
```

And these functions convert VA value between 2 different VA notations of visual acuity tests:
```
export class VASConverter {
  static logMar2FivePoint(logMar: number): number {
    return 5 - logMar;
  }

  static logMAR2VAS(logMar: number): number {
    return 100 - logMar * 50;
  }

  static decimal2MAR(decimalValue: number): number {
    return 1 / decimalValue;
  }

  static logMAR2MAR(logMar: number): number {
    return Math.floor(Math.pow(10, logMar) * 100) / 100;
  }

  static vas2LogMAR(vas: number): number {
    return (100 - vas) / 50;
  }

  static vas2Decimal(vas: number): number {
    const logMar = VASConverter.vas2LogMAR(vas);
    return Math.pow(10, -logMar);
  }

  static vas2MAR(vas: number): number {
    const logMar = VASConverter.vas2LogMAR(vas);
    return VASConverter.logMAR2MAR(logMar);
  }

  static vas2FivePoint(vas: number): number {
    const logMar = VASConverter.vas2LogMAR(vas);
    return VASConverter.logMar2FivePoint(logMar);
  }

  static calculateLogMAR(logMarBase: number, errorCount: number): number {
    return logMarBase + 0.02 * errorCount;
  }
}
```

Please craft a comparison table using HTML and JavaScript between different VA notations:
* LogMAR
* MAR
* VAS
* Snellen faction 1 meter like 1/0.63, 1/1 and 1/10
* Snellen faction 4 meters like 4/2.5, 4/4 and 4/40
* Snellen faction 5 meter like 5/3.2, 5/5 and 5/50
* Snellen faction 6 meter like 6/3.8, 6/6 and 6/60
* Snellen faction 20 feet like 20/12.5, 20/20 and 20/200
* Decimal like 1.6, 1.0 and 0.1

The notation names including optional distance stay in the headers. 
The VA values of each VA notation are listed in rows sorted by the numeric order of LogMar values as listed in the array.
Values LogMAR 0.3 to 0.1 are of range of normal vision; values between 0.2 and 0.5 are of mild vision loss, values between 0.6 and 0.9 are of moderate vision loss,  and values between 1.0 and 1.0 are of severe vision loss. For rows of normal vision, use white background, for mild vision loss, use light yellow, for moderate vision loss, use yellow, and for severe vision loss, use light blue.

Though technically you may generate a static HTML page  conforming to the functional requirements without using JavaScript, I would add some dynamic conversion features:
1. In the last row of the table, provide a numeric input box for column Decimal, and the incremental value is 0.01;  an input box for column MAR, and the incremental value is 0.1; an input box for LogMAR, and the incremental value is 0.1; an input box of VAS, and the incremental value is 1.
2. Whenever the user manually input a decimal value to any of the input box and press enter, the values in the other table cells of the same row will change according to the conversion functions that you have previously converted from C# code to TypeScript.
