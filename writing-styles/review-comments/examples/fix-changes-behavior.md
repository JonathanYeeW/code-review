**Fix** (changes behavior): tapping ✕ during a submit removes the sandwich from the receipt, but the machine still makes it

**What's happening:**

```
1. Tap Submit on an order with two sandwiches     "Sending to the kitchen"
2. Tap ✕ on the first sandwich before it lands    the row goes
3. The submit finishes                            the receipt shows one, the machine makes two
```

**The problem:** the customer pays for one sandwich and the machine uses ingredients for two. It can only happen in the second or so a submit takes, so it's rare.

**Recommended change:** disable ✕ during a submit, the way Submit already is.

```tsx
<RemoveButton disabled={orderStatus === "submitting"} onPress={onRemove} />
```

**How I checked:** reproduced in a test with a delayed submit, which fails today and passes with this change. Then undone.

---

## Two more, from other pull requests

The comment above is the usual case: the defect is in the diff's own lines. These two show where a Fix's claim has to be traced further before it's written.

### When a library's default decides before the code does

**Fix** (changes behavior): the supplier menu page opens other apps without asking the screen

**What's happening:**

```
1. Open a supplier's menu from the order screen; the page opens in the app
2. A script on the page, or an ad in a frame, goes to tel:…, itms-apps://… or pbj://…
3. react-native-webview hands it to Linking.openURL       the phone, the App Store or the app opens
```

`handleShouldStartLoad` is never called for those. react-native-webview checks `originWhitelist` first. Its default is `http://*` and `https://*`, and anything else goes to `Linking.canOpenURL` and then `openURL`, inside the library.

**The problem:** whether the page can open another app depends on the phone, not on this code. The comment at :160 says "anything else goes nowhere", and so does the description.

**Recommended change:** let every address reach the screen's handler, and let a frame load only a web address.

```tsx
<WebView
  // Every address reaches handleShouldStartLoad, which decides
  originWhitelist={["*"]}
```

```ts
if (request.isTopFrame === false) return /^(https?|about|data|blob):/i.test(request.url);
```

**How I checked:** read the library's source. A throwaway test fed the library's own request check the screen's handler. With the default whitelist, `tel:`, `itms-apps:` and `pbj:` called the handler 0 times and `openURL` 3 times. With `["*"]`, `tel:` was refused and an off-site click went to Safari. Then undone. Not run on a device.

### When the defect is on one platform

**Fix** (changes behavior): on Android, the Ready mark on the order banner is a plain white dot

**What's happening:**

```
1. Open an order on an Android phone in light mode      the banner across the top, in the machine's green
2. The order is ready for pickup
3. The Ready mark on the banner                         a white dot with no check
```

**The problem:** on Android, `StatusMark` draws its check in `palette.page`, assuming the mark sits on a page. On a page that's right. On the banner the mark is white, and so is light mode's page colour, so the check disappears. In dark mode the check is black, where an iPhone shows the banner's green.

**Recommended change:** let the banner say what's behind the mark.

```tsx
// StatusMark.android.tsx
checkColor?: string;
{ color: checkColor ?? palette.page }

// OrderBanner.tsx
<StatusMark status={order.status} color={color} checkColor={BANNER_GREEN} />
```

**How I checked:** traced the mark's colour on the banner (`#ffffff`) into `StatusMark.android.tsx` and `palette.ts` (light `page` `#ffffff`, dark `#000000`); rendered the Android `StatusMark` in a scratch test and got both white. Made the change: typecheck, lint and the shared tests pass. Then undone. Not seen on an emulator.
