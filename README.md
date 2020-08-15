# AXEmojiView
it is an advanced library which adds emoji,sticker,... support to your Android application.

<img src="./images/main.png" width=300 title="Screen">

## Installation

AXEmojiView is available in the JCenter, so you just need to add it as a dependency (Module gradle)

Gradle
```gradle
implementation 'com.aghajari.emojiview:AXEmojiView:1.2.0'
```

Maven
```xml
<dependency>
  <groupId>com.aghajari.emojiview</groupId>
  <artifactId>AXEmojiView</artifactId>
  <version>1.2.0</version>
  <type>pom</type>
</dependency>
```

# Usage
Let's START! :smiley:


## Install Emoji Provider 
First step, you should install EmojiView with your EmojiProvider!

```java
AXEmojiManager.install(this,new AXIOSEmojiProvider());
```

### Custom Emoji Provider
If you wanna display your own Emojis you can create your own implementation of [`EmojiProvider`](AXEmojiView/AXEmojiView/src/main/java/com/aghajari/axemojiview/emoji/EmojiProvider.java) and pass it to `AXEmojiManager.install`.

 <img src="./images/google.jpg" width=200 title="Screen"> <img src="./images/Twitter.jpg" width=200 title="Screen"> <img src="./images/one.jpg" width=200 title="Screen">

## Basic Usage

Create an [`AXEmojiEditText`](AXEmojiView/AXEmojiView/src/main/java/com/aghajari/axemojiview/view/AXEmojiEditText.java) in your layout.
```xml
<com.aghajari.axemojiview.view.AXEmojiEditText
            android:id="@+id/edt"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:hint="enter your message ..." />
```

Now, you should create a Page. 
Current pages are :
- EmojiView
- SingleEmojiView
- StickerView

Let's try EmojiView :
```java
AXEmojiEditText edt = findViewById(R.id.edt);

AXEmojiView emojiView = new AXEmojiView(this);
emojiView.setEditText(edt);
```

And add this page to AXEmojiPopup :
```java
AXEmojiPopup emojiPopup = new AXEmojiPopup(emojiView);

emojiPopup.toggle(); // Toggles visibility of the Popup.
emojiPopup.show(); // Shows the Popup.
emojiPopup.dismiss(); // Dismisses the Popup.
emojiPopup.isShowing(); // Returns true when Popup is showing.
```

And we are done! :smiley:
Result :

<img src="./images/ios.jpg" width=200 title="Screen">

### AXEmojiPopupLayout

you can also create an AXEmojiPopupLayout instead of AXEmojiPopup!
i believe that AXEmojiPopupLayout has a better performance.

1. create an AXEmojiPopupLayout in your layout.
```xml
    <com.aghajari.emojiview.view.AXEmojiPopupLayout
        android:id="@+id/layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"/>
```

2. add the created page to AXEmojiPopupLayout :
```java
AXEmojiPopupLayout layout = findViewById(R.id.layout);
layout.initPopupView(emojiView);

layout.toggle(); // Toggles visibility of the Popup.
layout.show(); // Shows the Popup.
layout.dismiss(); // Dismisses the Popup.
layout.hideAndOpenKeyboard(); // Hides the popup
layout.isShowing(); // Returns true when Popup is showing.
```

Result is just same as AXEmojiPopup result!

### Single Emoji View
SingleEmojiView is a RecyclerView and all emojis will load in one page (Same As Telegram Inc)

```java
AXSingleEmojiView emojiView = new AXSingleEmojiView(this);
emojiView.setEditText(edt);
```

Result :

<img src="./images/SingleEmojiView.png" width=200 title="Screen">

### StickerView
StickerView :
you have to create your StickerProvider and load all your Stickers (from Url,Res,Bitmap or anything you want!)
see example : [`WhatsAppProvider`](./AXEmojiView/app/src/main/java/com/aghajari/axemoji/sticker/WhatsAppProvider.java)

```java
AXStickerView stickerView = new AXStickerView(this , "stickers" , new MyStickerProvider());
```

Result :

<img src="./images/sticker.png" width=200 title="Screen">


Also you can create your custom pages in StickerProvider . see example : [`ShopStickers`](./AXEmojiView/app/src/main/java/com/aghajari/axemoji/sticker/ShopStickers.java)

Result :
<img src="./images/shop_sticker.png" width=200 title="Screen">

## AXEmojiPager - Use Multiple Pages Together!
you can create an AXEmojiPager and add all your pages (EmojiView,StickerView,...) to the EmojiPager

Enable Footer view in theme settings (if you want) :
```java
AXEmojiManager.getEmojiViewTheme().setFooterEnabled(true);
```

And Create your EmojiPager :
```java
AXEmojiPager emojiPager = new AXEmojiPager(this);

AXSingleEmojiView singleEmojiView = new AXSingleEmojiView(this);
emojiPager.addPage(singleEmojiView, R.drawable.ic_msg_panel_smiles);

AXStickerView stickerView = new AXStickerView(this, "stickers", new WhatsAppProvider());
emojiPager.addPage(stickerView, R.drawable.ic_msg_panel_stickers);

emojiPager.setSwipeWithFingerEnabled(true);
emojiPager.setEditText(edt);

AXEmojiPopup emojiPopup = new AXEmojiPopup(emojiPager);
//layout.initPopupView(emojiPager);
```

Add search button to the footer:
```java
emojiPager.setLeftIcon(R.drawable.ic_ab_search);
        
        //Click Listener
        emojiPager.setOnFooterItemClicked(new AXEmojiPager.onFooterItemClicked() {
            @Override
            public void onClick(boolean leftIcon) {
                if (leftIcon) Toast.makeText(EmojiActivity.this,"Search Clicked",Toast.LENGTH_SHORT).show();
            }
        });
```

Result :

<img src="./images/emojipager.png" width=200 title="Screen">

### Create Your Custom Pages
Create an AXEmojiBase (ViewGroup) and load your page layout
And add your CustomPage to emojiPager

Example: [`LoadingPage`](./AXEmojiView/app/src/main/java/com/aghajari/axemoji/customs/LoadingView.java)

```java
emojiPager.addPage(new LoadingView(this), R.drawable.msg_round_load_m);
```

Result :

<img src="./images/loading.png" width=200 title="Screen">

## Customization
Customize theme with AXEmojiTheme.
```java
AXEmojiManager.getEmojiViewTheme().setSelectionColor(0xffFF4081);
AXEmojiManager.getEmojiViewTheme().setFooterSelectedItemColor(0xffFF4081);
AXEmojiManager.getEmojiViewTheme().setFooterBackgroundColor(Color.WHITE);
AXEmojiManager.getEmojiViewTheme().setSelectionColor(Color.TRANSPARENT);
AXEmojiManager.getEmojiViewTheme().setSelectedColor(0xffFF4081);
AXEmojiManager.getEmojiViewTheme().setCategoryColor(Color.WHITE);
AXEmojiManager.getEmojiViewTheme().setAlwaysShowDivider(true);
AXEmojiManager.getEmojiViewTheme().setBackgroundColor(Color.LTGRAY);

AXEmojiManager.getStickerViewTheme().setSelectedColor(0xffFF4081);
AXEmojiManager.getStickerViewTheme().setCategoryColor(Color.WHITE);
AXEmojiManager.getStickerViewTheme().setAlwaysShowDivider(true);
AXEmojiManager.getStickerViewTheme().setBackgroundColor(Color.LTGRAY);
```

Result :

<img src="./images/theme.png" width=200 title="Screen">

### Custom Footer

```java
// disable default footer
AXEmojiManager.getEmojiViewTheme().setFooterEnabled(false);
AXEmojiManager.getInstance().setBackspaceCategoryEnabled(!mCustomFooter);

// add your own footer to the AXEmojiPager
EmojiPager.setCustomFooter(footerView,true);
```

Result :

<img src="./images/custom_footer_1.png" width=200 title="Screen">     <img src="./images/custom_footer_2.png" width=200 title="Screen">

## Views
- AXEmojiPopupLayout
- AXEmojiBase / AXEmojiLayout
- AXEmojiView
- AXSingleEmojiView
- AXStickerView
- AXEmojiEditText
- AXEmojiMultiAutoCompleteTextView
- AXEmojiButton
- AXEmojiImageView
- AXEmojiTextView
- AXEmojiCheckBox

## Listeners
onEmojiActions :
```java
    void onClick (View view, Emoji emoji, boolean fromRecent, boolean fromVariant);
    void onLongClick (View view, Emoji emoji, boolean fromRecent, boolean fromVariant);
```

onStickerActions :
```java
    void onClick(View view, Sticker sticker, boolean fromRecent);
    void onLongClick(View view, Sticker sticker, boolean fromRecent);
```

onEmojiPagerPageChanged :
```java
    void onPageChanged (AXEmojiPager emojiPager, AXEmojiBase base, int position);
```

PopupListener :
```java
    void onDismiss();
    void onShow();
    void onKeyboardOpened(int height);
    void onKeyboardClosed();
```

## Set TextViews Text with emojis
first you need to get Unicode of emoji :
```java
String unicode = AXEmojiUtils.getEmojiUnicode(0x1f60d); // or Emoji.getUnicode();
```
now set it to your view with AXEmojiUtils.replaceWithEmojis.

Example: Set ActionBar Title :
```java
String title = "AXEmojiView " + unicode;
getSupportActionBar().setTitle(AXEmojiUtils.replaceWithEmojis(this, title, 20));
```

Result :

<img src="./images/actionbar.png" width=500 title="Screen">

## RecentManagers And VariantManager
you can add your custom recentManager for emojis and stickers . implements to RecentEmoji/RecentSticker
```java
AXEmojiManager.setRecentEmoji(emojiRecentManager);
AXEmojiManager.setRecentSticker(stickerRecentManager);
```

Disable RecentManagers :
```java
AXEmojiManager.getInstance().disableRecentManagers();
```

### Variant View
you can also create your own VariantPopupView ! but you don't need to, the default one is also nice :)

The Default Variant:
<img src="./images/variants.png" width=200 title="Screen">


## Emoji Loader
you can add an custom EmojiLoader with AXEmojiLoader :
```java
AXEmojiManager.setEmojiLoader(new EmojiLoader(){
  @Override
  public void loadEmoji (AXEmojiImageView imageView,Emoji emoji){
   imageView.setImageDrawable(emoji.getDrawable(imageView.getContext());
  }
});
```

## Download Apk
<img src="./images/apk.png" width=200 title="Screen">

[`Download Sample Apk`](./AXEmojiView1.2.0.apk)

## Author 
Amir Hossein Aghajari

Telegram : @KingAmir272
