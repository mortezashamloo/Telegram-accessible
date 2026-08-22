# ۲) حذف دکمه Share بین پیام‌ها

**فایل:** `TMessagesProj/src/main/java/org/telegram/ui/Cells/ChatMessageCell.java`  
**متد:** `checkNeedDrawShareButton`

با برگرداندن همیشگی `false`، دکمه کنار پیام‌ها رسم نمی‌شود و از درخت دسترس‌پذیری هم حذف می‌شود → پیمایش TalkBack فقط روی خود پیام می‌ماند.

### کل متد را این‌طور کن:

```java
protected boolean checkNeedDrawShareButton(MessageObject messageObject) {
    // A11Y: hide side Share button between messages for faster TalkBack navigation.
    // Share remains available via message long-press menu.
    return false;
}
```

اشتراک‌گذاری از منوی لمس‌طولانی پیام همچنان در دسترس است.
