### ColorUtil.ts

```typescript
/* eslint-disable */

export default class ColorUtil {
  static hexToRGB(hex: string, alpha: string) {
    // Thanks to AJFarkas: https://stackoverflow.com/questions/21646738/convert-hex-to-rgba
    var r = parseInt(hex.slice(1, 3), 16),
      g = parseInt(hex.slice(3, 5), 16),
      b = parseInt(hex.slice(5, 7), 16);
    if (alpha) {
      return "rgba(" + r + ", " + g + ", " + b + ", " + alpha + ")";
    } else {
      return "rgb(" + r + ", " + g + ", " + b + ")";
    }
  }

  /**
   * Lightens or darkens the color (HEX)
   * @param {String} color The HEX color code
   * @param {Number} percent The percentage to adjust. Positive number lightens the color, negative number darkens the color
   */
  static adjustColor(color: string, percent: number) {
    //
    // Thanks to Pablo @ https://stackoverflow.com/a/13532993/1136963
    //

    let R: number = parseInt(color.substring(1, 3), 16);
    let G: number = parseInt(color.substring(3, 5), 16);
    let B: number = parseInt(color.substring(5, 7), 16);

    R = parseInt(((R * (100 + percent)) / 100).toString());
    G = parseInt(((G * (100 + percent)) / 100).toString());
    B = parseInt(((B * (100 + percent)) / 100).toString());

    R = R < 255 ? R : 255;
    G = G < 255 ? G : 255;
    B = B < 255 ? B : 255;

    const RR =
      R.toString(16).length == 1 ? `0${R.toString(16)}` : R.toString(16);
    const GG =
      G.toString(16).length == 1 ? `0${G.toString(16)}` : G.toString(16);
    const BB =
      B.toString(16).length == 1 ? `0${B.toString(16)}` : B.toString(16);

    return `#${RR}${GG}${BB}`;
  }

  /**
   * Darkens the color (HEX)
   * @param {String} color The HEX color code
   * @param {Number} percent The percentage to darken.
   */
  static darken(color: string, percent: number) {
    return ColorUtil.adjustColor(color, -percent);
  }

  /**
   * Lightens the color (HEX)
   * @param {String} color The HEX color code
   * @param {Number} percent The percentage to lighten.
   */
  static lighten(color: string, percent: number) {
    return ColorUtil.adjustColor(color, percent);
  }
}
```
