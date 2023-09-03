![](https://res.cloudinary.com/practicaldev/image/fetch/s--78xAYQdy--/c_imagga_scale,f_auto,fl_progressive,h_420,q_66,w_1000/https://thepracticaldev.s3.amazonaws.com/i/i4jymvdb2vqc4m2wg5jm.gif)

এখন সময় স্কোপ চেইনের। 🕺🏼 এই পোস্টে আমি ধরে নিব যে আপনি এক্সিকিউশন কনটেক্সট এর বেসিক জানেনঃ যদিও সেটা নিয়েও আমি খুব দ্রুত পোস্ট করব 😃

---


নিচের কোডটা দেখেনঃ

```
const name = "Lydia"
const age = 21
const city = "San Francisco"


function getPersonInfo() {
  const name = "Sarah"
  const age = 22

  return `${name} is ${age} and lives in ${city}`
}

console.log(getPersonInfo())
```


আমরা `getPersonInfo` ফাংশনটা ইনভোক করছি, যেটা একটা স্ট্রিং রিটার্ন করে যার মধ্যে `name`, `age` এবং `city` ভ্যারিয়েবলগুলো থাকেঃ `Sarah is 22 and lives in San Francisco` 
কিন্তু `getPersonInfo` ফাংশনের মধ্যে  `city` ভ্যারিয়েবলটা তো নেই🤨! তাহলে এটা কিভাবে  `city` র ভ্যালু জানতে পারল? 


প্রথমত, বিভিন্ন কনটেক্সটের জন্য মেমোরি স্পেস সেট হয়। যেই `getPersonInfo` ফাংশনটা ইনভোক হয়েছে, তার দুই ধরণের কনটেক্সট আছে, গ্লোবাল কনটেক্সট(`window` in a browser, `global` in Node) এবং লোকাল কনটেক্সট । প্রত্যেক কনটেক্সটের আবার স্কোপ চেইন আছে। 


 `getPersonInfo` ফাংশনের স্কোপ চেইনটা কিছুটা এরকম দেখতে( বুঝতে না পারলে, চিন্তার কিছু নাই)ঃ

[![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--Ya80sAQN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/89b9buizhevs0jf6djyn.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Ya80sAQN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/89b9buizhevs0jf6djyn.png)
স্কোপ চেইন মূলত এক্সিকিউশন কনটেক্সেটে যেসব ভ্যালু(এবং অন্য স্কোপ) আছে , তারা যেসব অবজেক্টের ভিতর থাকে তাদেরকে নির্দেশ করার একটা শিকল বা "Chain of references" . (⛓: "ও ভাই! এই যে! এই সব ভ্যালুকে আপনি এই কনটেক্সটের ভেতর থেকে রেফারেন্স করতে পারবেন".) যখন এক্সিকিউশন কনটেক্সট তৈরী হয় তখনই স্কোপ চেইন তৈরী হয়। মানে এটা রানটাইমে সৃষ্টি হয়!  

যাইহোক, এই পোস্টে আমি *activation object* বা এক্সিকিউশন কনটেক্সট নিয়ে আলাপ করব না। স্কোপের মধ্যেই আমাদের ফোকাস রাখব। নিচের উদাহরণগুলোতে, এক্সিকিউশন কনটেক্সটের  key/value pair গুলো দিয়ে ভ্যারিয়েবলের রেফারেন্সগুলো(যেগুলো স্কোপ চেইনের কাছে আছে) বুঝায়। 

[![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--U_63-TRk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/iala2et7bg9bgdj4c2lg.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--U_63-TRk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/iala2et7bg9bgdj4c2lg.png)

গ্লোবাল এক্সিকিউশন কনটেক্সটের স্কোপ চেইননে ৩ টা ভ্যারিয়েবলের রেফারেন্স আছেঃ  `name` যার ভ্যালু `Lydia`, `age` যার ভ্যালু `21`, এবং `city` যার ভ্যলু `San Francisco`. লোকাল কনটেক্সটে, ২ টা ভ্যারিয়েবলের রেফারেন্স আছেঃ `name` যার ভ্যালু `Sarah`, and `age` যার ভ্যালু `22`.

যখন আমরা `getPersonInfo` ফাংশনের ভেতরে ভ্যারিয়েবল access করতে চেষ্টা করি, ইঞ্জিন প্রথমে লোকাল স্কোপ চেইনে চেক করে। 

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--9A6d4z3e--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/xn17f0t54acz8tiq7122.gif)

লোকাল স্কোপ চেইনের কাছে `name` and `age` এর রেফারেন্স আছে।  `name` এর ভ্যালু আছে `Sarah` and `age` এর ভ্যালু আছে `22` । কিন্তু এখন, কি ঘটবে যদি সে `city` access করতে চায়?? 

`city` এর ভ্যালু খুজে পাওয়ার জন্য ইঞ্জিন  "goes down the scope chain" । সহজ ভাষায় বলতে গেলে, ইঞ্জিন এত সহজে হাল ছেড়ে দেয় না। ইঞ্জিন আপনার জন্য অনেক খাটুনি খাটে। `city` র ভ্যালু পাওয়ার জন্য সে বাইরের স্কোপে চলে যায় খুজতে। যেই বাইরের স্কোপের রেফারেন্স লোকাল স্কোপের আছে। এই ক্ষেত্রে সেটার নাম **গ্লোবাল অবজেক্ট** ।


![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--mjJbIqM6--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/z9iclg23rmbpts7meoq6.gif)

গ্লোবাল কনটেক্সটে আমরা `city` ভ্যারিয়েবল ডিক্লেয়ার করেছি এবং ভ্যালু দিয়েছি `San Francisco`, অর্থাৎ এখানে `city` ভ্যারিয়েবলের রেফারেন্স আছে। এখন যেহেতু আমাদের কাছে ভ্যারিয়েবলের ভ্যালু আছে, তাহলে  `getPersonInfo` ফংশনটি `Sarah is 22 and lives in San Francisco`  এই স্ট্রিংটি রিটার্ণ করতে পারবে। 🎉

---

আমরা স্কোপ চেইনের *নিচে* যেতে পারব, কিন্তু *উপরে* যেতে পারব না।(আচ্ছা আচ্ছা বুঝছি, এটা কনফিউজিং মনে হতে পারে পারে, কারণ কিছু মানুষ *নিচের*  বদলে *উপরে* বলে। তাই আমি আবার নতুন ভাবে বলছি, আপনি *বাইরের(outer)* স্কোপে যেতে পারবেন, কিন্তু *ভেতরের(inner)* স্কোপে যেতে পারবেন না।) আমি এটাকে জলপ্রপাতের সাথে তুলনা করতে পছন্দ করিঃ 

[![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--5xdIAMGr--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/doq46yc6nuiam51evy44.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--5xdIAMGr--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/doq46yc6nuiam51evy44.png)
অথবা আরও গভীরেঃ

[![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--vbJ2Eh2w--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/rece2zj4pb4w1fn56q5k.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--vbJ2Eh2w--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/rece2zj4pb4w1fn56q5k.png)

---

উদাহরণ হিসেবে এই কোডটাকে ধরা যাকঃ

[![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--xb-kf0ML--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/0z6342b72f3n6v6ufafk.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--xb-kf0ML--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/0z6342b72f3n6v6ufafk.png)
এটা প্রায় একই রকম, তা সত্ত্বেও একটা বড় পার্থক্য আছেঃ আমরা `city` ভ্যারিয়েবলটা ডিক্লেয়ার করেছি, *শুধুমাত্র*  `getPersonInfo` ফাংশনের ভেতরে, গ্লোবাল স্কোপে *নয়*। আমরা  `getPersonInfo` ফাংশনটা কলও করিনি, অর্থাৎ লোকাল কনটেক্সটও তৈরী হয়নি। তারপরও আমরা গ্লোবাল কনটেক্সটে `name`, `age` আর `city`এর ভ্যালুকে কে access করার চেষ্টা করছিলাম।  


![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--PQt7f7YR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/f3wvlo4c3gqf3mve1g0n.gif)
এটা একটা `ReferenceError` থ্রো করে ! গ্লোবাল স্কোপে  সে `city` ভ্যারিয়েবলের রেফারেন্স পাচ্ছে না, এবং এর কোন বাইরের(outer) স্কোপও নেই, এবং এটা স্কোপ চেইনের *উপরে* যেতে **পারেনা** । 


এই উপায়ে, আপনি স্কোপকে ব্যাবহার করে আপনার ভ্যারিয়েবলকে "protect" করতে পারেন, এবং পুনরায় ভ্যারিয়েবলের নামটা ব্যাবহারও করতে পারেন।

---

গ্লোবাল আর লোকাল স্কোপ ছাড়াও আরেকটা স্কোপ আছে, **ব্লক স্কোপ** ।  যেসব ভ্যারিয়েবল `let` অথবা `const` keyword দিয়ে ডিক্লেয়ার করা হয় তারা তাদের নিকটতম দ্বিত্বীয় বন্ধনীর (`{``}`) ভেতরে স্কোপড হয়ে যায়। 

```
const age = 21

function checkAge() {
  if (age < 21) {
    const message = "You cannot drink!"
    return message
  } else {
    const message = "You can drink!"
    return message
  }
} 
```


স্কোপটা আপনি নিচের মত করে ভিজুয়ালাইজ করতে পারেনঃ

[![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--glZ2RaGi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/75n1vpm7z4d8924cnvje.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--glZ2RaGi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/75n1vpm7z4d8924cnvje.png)

এখানে আমাদের একটা গ্লোবাল স্কোপ, একটা ফাংশন স্কোপ, এবং দুইটা ব্লক স্কোপ আছে। আমরা  `message` ভ্যারিয়েবলটা দুইবার ডিক্লেয়ার করতে পেরেছি, যেহেতু ভ্যারিয়েবল গুলো তাদের দ্বিতীয় বন্ধনীর ভেতর স্কোপড হয়ে ছিল। 

---

খতম! ছোট্ট সামারিঃ

- "স্কোপ চেইন" বিষয়টাকে আপনি এভাবে কল্পনা করতে পারেন যে, এটা current context এ ভ্যালু access করার রেফান্সের শিকল বা চেইন। 
- স্কোপের কারণে আমরা ভ্যারিয়েবলের নাম রি-ইউজ করতে পারি। যেই নাম স্কোপ চেইনের **নিচে** ডিফাইন করা ছিল, যেহেতু এটা স্কোপ চেইনের শুধু **নিচে** যেতে পারে, **উপরে**  না। 

আপাতত স্কোপ(চেইন) নিয়ে এটুকুই! এটা নিয়ে আরও অনেক অনেক কথা বলা যাবে তাই আমি পরে এক্সট্রা আরও অনেক কিছু যোগ করতে পারি। যেকোন প্রশ্ন নিয়ে আমাকে জিজ্ঞেস করতে পারেন, কারও পাশে দাড়াতে বেশ লাগে আমার। 💕