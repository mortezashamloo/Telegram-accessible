# ۴) برچسب‌گذاری دکمه‌های بدون برچسب

TalkBack روی دکمه‌های بدون `contentDescription` فقط «دکمه» می‌گوید.
هدف: هر کنترل قابل‌کلیک یک برچسب داشته باشد.

## الف) کمک‌کنندهٔ سراسری (پیشنهادی)

فایل جدید:

`TMessagesProj/src/main/java/org/telegram/ui/Components/A11y.java`

```java
package org.telegram.ui.Components;

import android.view.View;
import android.widget.ImageView;
import org.telegram.messenger.LocaleController;

public final class A11y {
    private A11y() {}

    public static void label(View v, int stringRes) {
        if (v == null) return;
        v.setContentDescription(LocaleController.getString(stringRes));
        v.setImportantForAccessibility(View.IMPORTANT_FOR_ACCESSIBILITY_YES);
    }

    public static void label(View v, String text) {
        if (v == null) return;
        v.setContentDescription(text);
        v.setImportantForAccessibility(View.IMPORTANT_FOR_ACCESSIBILITY_YES);
    }

    /** If description empty, set fallback so TalkBack never says only "Button". */
    public static void ensure(View v, String fallback) {
        if (v == null) return;
        CharSequence d = v.getContentDescription();
        if (d == null || d.length() == 0) {
            v.setContentDescription(fallback);
        }
        v.setImportantForAccessibility(View.IMPORTANT_FOR_ACCESSIBILITY_YES);
    }
}
```

## ب) جاهای پرتکرار بدون برچسب

بعد از ساخت View، `A11y.label(...)` بگذار:

| محل | فایل تقریبی | برچسب نمونه |
|-----|-------------|-------------|
| دکمه ارسال | `ChatActivityEnterView` | Send / ارسال |
| ضمیمه / گیره | همان | Attach / پیوست |
| میکروفون | همان | Voice message |
| اموجی | همان | Emoji |
| منوی سه‌نقطه اکشن‌بار | `ActionBar` / `ActionBarMenuItem` | More options |
| بازگشت | `ActionBar` | Back |
| جستجو در لیست چت | `DialogsActivity` | Search |
| تماس صوتی/تصویری | `ChatActivity` | Voice call / Video call |
| اسکرول به پایین | `ChatActivity` | Scroll to bottom |
| پلی/توقف ویس | `ChatMessageCell` / player | Play / Pause |
| بستن پلیر | | Close player |

مثال:

```java
A11y.label(attachButton, R.string.AccDescrAttachButton);
A11y.label(emojiButton, R.string.AccDescrEmojiButton);
A11y.label(sendButton, R.string.Send);
```

خیلی از این رشته‌ها از قبل در `strings.xml` تلگرام هست (`AccDescr...`).

## ج) ActionBarMenuItem

در `ActionBarMenuItem.java` بعد از `setIcon` / ساخت آیتم:

```java
if (getContentDescription() == null || getContentDescription().length() == 0) {
    setContentDescription(LocaleController.getString(R.string.AccDescrMoreOptions));
}
```

## د) ImageViewهای کلیک‌پذیر بدون توضیح

هر جا `setOnClickListener` روی `ImageView` است و `setContentDescription` نیست:

```java
A11y.ensure(imageView, "Button"); // موقت
// بهتر: رشتهٔ معنی‌دار فارسی/انگلیسی
```

## هـ) تست با TalkBack

1. TalkBack را روشن کن.
2. در چت، لیست چت، تنظیمات، پلیر، منوی پیام بگرد.
3. هر جا فقط «Button» شنیدی، همان View را در سورس پیدا کن و برچسب بده.
4. لیست موارد را در `a11y-todo.txt` نگه دار تا یکی‌یکی تمام شود.

برچسب‌گذاری «همه» دکمه‌ها یک‌باره ممکن نیست (هزاران View)؛ با `A11y` + تست TalkBack به‌تدریج کامل می‌شود.
