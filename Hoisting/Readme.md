![](https://res.cloudinary.com/practicaldev/image/fetch/s--QzCv1bXR--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://thepracticaldev.s3.amazonaws.com/i/kaf11wh85tkhfv1338b4.png)

# 🔥🕺🏼 JavaScript Visualized: Hoisting


প্রত্যেক জাভাস্ক্রিপ্ট ডেভেলপার কখনও না কখনও একটা বিরক্তিকর এরোর ফেস করেছে। তারপর স্ট্যাক ওভারফ্লোতে যেয়ে দেখে, কোন এক বিজ্ঞ ব্যাক্তি বলছে যে , আপনার এরোরটা আসলে *hoisting* এর কারণে হচ্ছে। 🙃 আচ্ছা, তাহলে Hoisting কি? (আগেই বলে রাখছি, *scope* নিয়ে অন্য পোস্টে আলাপ করব, আমি পোস্ট ছোট আর ফোকাসড রাখতে পছন্দ করি)



আপনি যদি জাভাস্ক্রিপ্টে নতুন হয়ে থাকেন, আপনি হয়ত কোডের কিছু "অদ্ভুতুড়ে" আচরন দেখেছেন। যেমন, হুটহাট কোন ভ্যারিয়েবল `undefined` হয়ে গেল, অথবা কোড `ReferenceError` থ্রো করছে আরও আনেক কিছু। Hoisting কে প্রায়ই এভাবে ব্যাখা করা হয় যে *ভ্যারিয়েবল এবং ফাংশনকে ফাইলের টপে উঠিয়ে আনা* হল হইস্টিং। কিন্তু নাহ! এমনটা ঘটছে না, যদিও আচরণ দেখে ওমনটাই মনে হয়। 😃



জাভাস্ক্রিপ্ট ইঞ্জিন যখন স্ক্রিপ্টটা পায়, প্রথম যে কাজটা সে করে তা হল, আমাদের কোডের ডাটার জন্য **মেমরি সেট করে** । এই সময়টাতে কোন কোডই এক্সিকিউট হয় না। এক্সিকিউট করার জন্য সে শুধু সবকিছুকে প্রিপেয়ার বা রেডি করে। ফাংশন ডিক্লেয়ারেশন আর ভ্যারিয়েবল দুইটা আলাদা উপায়ে স্টোর করে রাখা হয়। 
ফাংশনগুলো স্টোর করার সময় **সম্পূর্ণ ফাংশনের রেফারেন্স** দিয়ে স্টোর করা হয়।

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--lLfiCbTX--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif7.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--lLfiCbTX--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif7.gif)

ভ্যারিয়েবলের ক্ষেত্রে বিষয়টা একটু আলাদা। ES6 ভ্যারিয়েবল ডিক্লেয়ার করার জন্য দুইটা নতুন keyword নিয়ে এসেছেঃ `let` এবং `const` । যেকোন ভ্যারিয়েবলকে যদি `let` অথবা `const` keyword দিয়ে ডিক্লেয়ার করা হয়, তাহলে সেটা *uninitialized* ভাবে স্টোর হয়।

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--vRtKMspn--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif8.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--vRtKMspn--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif8.gif)


`var` দিয়ে যদি ভ্যারিয়েবল ডিক্লেয়ার করা হয়, তাহলে স্টোর করার সময় ডিফল্ট ভ্যালু থাকে `undefined` 

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--zvlaEaAo--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif9.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--zvlaEaAo--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif9.gif)

এখন যেহেতু creation phase হয়ে গেছে, এবার কোডটা এক্সিকিউট করা যায়। চলেন দেখি, ফাইলের শুরুতেই যদি তিনটা `console.log()` স্টেটমেন্ট থাকে(মানে ফাংশন বা ভ্যারিয়েবল ডিক্লেয়ার করার আগেই) তাহলে কি ঘটে। 


যেহেতু ফাংশনগুলো সম্পূর্ণ ফাংশন কোডের রেফারেন্স দিয়ে স্টোর হয়, সেহেতু আমরা যেই লাইনে ফাংশনটা ক্রিয়েট করেছি তার *আগেই*  ইনভোক করতে পারব! 🔥

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--nk1taOke--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif16.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--nk1taOke--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif16.gif)



যখন আমরা `var` keyword দিয়ে ডিক্লেয়ার করা একটা ভ্যারিয়েবলকে তাদের ডিক্লেয়ারেশনের আগেই রেফারেন্স করি, সে তার ডিফল্ট যেই ভ্যালু (`undefined`) দিয়ে স্টোর হয়েছিল, তাই রিটার্ন করে। যাইহোক, এই কারণে মাঝেমধ্যে "অপ্রত্যাশিত" ফলাফল দেখা যায়। বেশিরভাগ ক্ষেত্রেই এর মানে হচ্ছে আপনি অনিচ্ছাকৃত ভাবে এটাকে রেফারেন্স করছেন( আপনি নিশ্চই চান না যে এর ভ্যালু  `undefined` হোক ) 😬

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--2nai6XPr--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif17.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--2nai6XPr--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif17.gif)


`var` keyword দিয়ে আমরা এক্সিডেন্টালি যাতে কোন `undefined` ভ্যারিয়েবলকে রেফারেন্স করে না ফেলি, সেজন্য যখনই কোন *uninitialized* ভ্যারিয়েবলকে access করতে যাই, একটা `ReferenceError` থ্রো হয়। ডিক্লেয়ারেশনের আগের "zone" কে বলা হয় **temporal dead zone** : ভ্যারিয়েবল(এবং ES6 class ও ) আপনি তাদের ইনিশিয়ালাইজেশনের আগে রেফারেন্স করতে পারবেন না। 

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--VVPlWhGC--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif18.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--VVPlWhGC--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif18.gif)


যখন ইঞ্জিন কোডের যেই লাইনে আসলেই ভ্যারিয়েবলটা ডিক্লেয়ার করা হয়েছে, সেই লাইন পড়তে পড়তে আগায়, তখন মেমোরির ভ্যালুগুলো ওভাররাইট হয়ে যায় তার সত্যিকারের ভ্যালু দিয়ে।


(এহ হে! এখানে তো নাম্বার ৭ হওয়ার কথা! দ্রুত আপডেট করে ফেলব! 😬)
[![](https://res.cloudinary.com/practicaldev/image/fetch/s--LGEaCMkS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif12.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--LGEaCMkS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif12.gif)

---



খতম! 🎉 ছোট্ট সামারিঃ 

- একটা এক্সিকিউশন কনটেক্সটের জন্য ফাংশন আর ভ্যারিয়েবল মেমোরিতে স্টোর হয় কোড এক্সিকিউট হওয়ার আগে। এটাই *Hoisting*
- ফাংশনগুলো স্টোর হয় সম্পূর্ণ ফাংশনএর রেফারেন্স সহ। আর ভ্যারিয়েবলের ক্ষেত্রে যদি `var` keyword ব্যাবহার হয় তাহলে ভ্যালু হয় `undefined` এবং `let` অথবা `const` keyword হলে সেক্ষেত্রে ভ্যালু স্টোর হয় *uninitialized* .


আশা করি, এখন যেহেতু আমরা দেখলাম কোড এক্সিকিউট হওয়ার সময় আসলে কি হয়, *hoisting* শব্দটা এখন তুলনামূলক কম অস্পষ্ট। বরাবরের মতোই, এখনও যদি না বুঝেন চিন্তার কিছু নেই। যত কাজ করবেন এটা নিয়ে ততই সহজ হয়ে যাবে বিষয়টা। যেকোন সমস্যায় আমাকে জিজ্ঞেস করতে পারেন, আমি সানন্দে এগিয়ে আসব। 