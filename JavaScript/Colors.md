### ColorUtil.ts

```typescript
export class ColorUtil {
  static hexToRGB(hex: string, alpha?: number) {
    // Thanks to AJFarkas: https://stackoverflow.com/questions/21646738/convert-hex-to-rgba

    if (!/^#([0-9A-F]{3}){1,2}$/i.test(hex)) {
      throw new Error(`Invalid HEX color format: ${hex}`);
    }

    if (typeof alpha === "number" && (alpha < 0 || alpha > 1)) {
      throw new Error(`Invalid alpha value ${alpha}. Must be between 0 and 1`);
    }

    const r = parseInt(hex.slice(1, 3), 16);
    const g = parseInt(hex.slice(3, 5), 16);
    const b = parseInt(hex.slice(5, 7), 16);

    if (alpha) {
      return `rgba(${r}, ${g}, ${b}, ${alpha})`;
    }
    return `rgba(${r}, ${g}, ${b})`;
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

    if (!/^#([0-9A-F]{3}){1,2}$/i.test(color)) {
      throw new Error(`Invalid HEX color format: ${color}`);
    }

    if (percent > 100 || percent < -100) {
      throw new Error(
        `Invalid percent value: ${percent}. Must be between -100 and 100`
      );
    }

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

  /**
   * Adjusts a HEX color based on an alpha value, simulating the color when applied on a white background.
   * @param {string} hex The HEX color code
   * @param {number} alpha The alpha value to apply (0 to 1)
   * @returns {string} The adjusted HEX color code
   */
  static applyAlphaToHexColor(hex: string, alpha: number): string {
    if (!/^#([0-9A-F]{3}){1,2}$/i.test(hex)) {
      throw new Error(`Invalid HEX color format: ${hex}`);
    }

    if (typeof alpha !== "number" || alpha < 0 || alpha > 1) {
      throw new Error(`Invalid alpha value ${alpha}. Must be between 0 and 1`);
    }

    let r = parseInt(hex.slice(1, 3), 16);
    let g = parseInt(hex.slice(3, 5), 16);
    let b = parseInt(hex.slice(5, 7), 16);

    // Apply the alpha blending with white
    r = Math.round((1 - alpha) * 255 + alpha * r);
    g = Math.round((1 - alpha) * 255 + alpha * g);
    b = Math.round((1 - alpha) * 255 + alpha * b);

    // Convert back to HEX
    const toHex = (c: number) => {
      const hx = c.toString(16);
      return hx.length === 1 ? "0" + hx : hx;
    };

    return `#${toHex(r)}${toHex(g)}${toHex(b)}`;
  }
}
```

Unit tests:

```typescript
import { ColorUtil } from "../ColorUtil";

describe("ColorUtil", () => {
  describe("hexToRGB", () => {
    it("should convert a hex color to an RGB color", () => {
      const hex = "#000000";
      const alpha = 0.5;
      const rgb = ColorUtil.hexToRGB(hex, alpha);
      expect(rgb).toEqual("rgba(0, 0, 0, 0.5)");
    });

    it("should convert a hex color to an RGB color without alpha", () => {
      const hex = "#000000";
      const rgb = ColorUtil.hexToRGB(hex);
      expect(rgb).toEqual("rgba(0, 0, 0)");
    });
  });

  describe("lighten", () => {
    it("should lighten a color by a given percentage", () => {
      const color = "#808080";
      const percent = 50;
      const newColor = ColorUtil.lighten(color, percent);
      expect(newColor).toEqual("#c0c0c0"); // 50% lighter gray
    });

    it("should not change color when percentage is 0", () => {
      const color = "#123456";
      const percent = 0;
      const newColor = ColorUtil.lighten(color, percent);
      expect(newColor).toEqual(color); // No change in color
    });

    it("should handle edge case when lightening makes the color too bright", () => {
      const color = "#ffffff";
      const percent = 100;
      const newColor = ColorUtil.lighten(color, percent);
      expect(newColor).toEqual("#ffffff"); // White remains white, can't get brighter
    });
  });

  describe("darken", () => {
    it("should darken a color by a given percentage", () => {
      const color = "#ffffff";
      const percent = 10;
      const newColor = ColorUtil.darken(color, percent);
      expect(newColor).toEqual("#e5e5e5"); // 10% darker than white
    });

    it("should not change color when percentage is 0", () => {
      const color = "#123456";
      const percent = 0;
      const newColor = ColorUtil.darken(color, percent);
      expect(newColor).toEqual(color); // No change in color when percent is 0
    });

    it("should handle edge case when darkening makes the color too dark", () => {
      const color = "#000000";
      const percent = 50;
      const newColor = ColorUtil.darken(color, percent);
      expect(newColor).toEqual("#000000"); // Black remains black, can't get darker
    });
  });
});
```
