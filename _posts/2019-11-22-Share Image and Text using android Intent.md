---
layout: post
title: Share Image and Text using android Intent
tags: [Android App Development, Android, Kotlin, Java, Mobile App Development]
---

# Share Image and Text using android Intent
![](https://miro.medium.com/max/158/1*6-Or1AYLkeMeCBLE7xDDZA.jpeg)

In this article, we will learn how to share your required data using android intent. This article is part of the “Today I Learnt” category.

## How intent opens share intent?
The intent is a class that helps to perform different actions like open ACTION_VIEW, ACTION_WEB_SEARCH, ACTION_SEARCH, etc. In this article, we’ll see about the action ACTION_SEND.

The ACTION_SEND helps to open the share intent where you will share your required data. Now we’ll learn how we share text and image using android Intent with the help of share intent. So, Let’s started.

## Share text using intent
So, In this section, we’ll share a simple text using intent. So follow this below code.

````Kotlin 
val sendIntent = Intent()
sendIntent.action = Intent.ACTION_SEND
sendIntent.putExtra(
    Intent.EXTRA_TEXT,
    "Send a simple text"
)
sendIntent.type = "text/plain"
startActivity(sendIntent)

````

## Share image using intent

A few days ago I had a task where I wanted to share the image with other apps. I searched many solutions on StackOverflow but those were not cases I wanted. So, I did it by in my own way using Picasso library. Here are the steps to do this.

***Step#01***: Add Picasso dependency in `build.gradle(app)`

````Kotlin
implementation 'com.squareup.picasso:picasso:2.71828'
````


***Step#02***: Add below code in `onCreate()` method. 


````Kotlin
val builder: StrictMode.VmPolicy.Builder = StrictMode.VmPolicy.Builder()
StrictMode.setVmPolicy(builder.build())
````


***Step#03***: After that, call this function on click listener. 


````Kotlin
fun shareImageFromURI(url: String?) {
    Picasso.get().load(url).into(object : Target {
        override fun onBitmapLoaded(bitmap: Bitmap?, from: Picasso.LoadedFrom?) {
            val intent = Intent(Intent.ACTION_SEND)
            intent.type = "image/*"
            intent.putExtra(Intent.EXTRA_STREAM, getBitmapFromView(bitmap))
            startActivity(Intent.createChooser(intent, "Share Image"))
        }
        override fun onPrepareLoad(placeHolderDrawable: Drawable?) { }
        override fun onBitmapFailed(e: java.lang.Exception?, errorDrawable: Drawable?) { }
    })
}

````

So, this method will share the image on different apps. It takes the path/url of image and load into Bitmap. Picasso converts it into the bitmap and then we create a method that returns URI from the bitmap. So, the below method returns the URI from a bitmap.

````Kotlin
fun getBitmapFromView(bmp: Bitmap?): Uri? {
  var bmpUri: Uri? = null
  try {
    val file = File(this.externalCacheDir, System.currentTimeMillis().toString() + ".jpg")
    
    val out = FileOutputStream(file)
    bmp?.compress(Bitmap.CompressFormat.JPEG, 90, out)
    out.close()
    bmpUri = Uri.fromFile(file)

  } catch (e: IOException) {
    e.printStackTrace()
  }
  return bmpUri
}
````

Fewwwwwwww finally done 😃.

Thanks for reading if you want any help then follow me on [Github](https://github.com/AminullahTajMuhammad), [Twitter](https://twitter.com/tajaminullah).

