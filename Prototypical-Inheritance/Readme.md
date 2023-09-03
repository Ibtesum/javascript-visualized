

কখনও ভেবেছেন, `.length`, `.split()`, `.join()` এরকম বিল্ট-ইন মেথড গুলো আমরা কিভাবে স্ট্রি, অ্যারে বা অবজেক্টের উপর ব্যাবহার করতে পারি? আমরা কখনও আলাদাভাবে এগুলো স্পেসিফাইড করে দেইনি, তাহলে এগুলো আসলো কোথা থেকে? এখন আবার বলিয়েন না, "এটা জাভাস্ক্রিপ্ট, এটা জাদু, 🧚🏻‍♂️ কেউ জানে না, হাহাহা ", এটা আসলে যে কারণে আমরা করতে পারি সেটাকে বলে _prototypal inheritance_ . এটা অস্থির একটা জিনিস এবং আপনি যে এটা কি পরিমাণে ব্যাবহার করেন, আপনি নিজেও জানেন না! 

অনেক সময়ই আমাদের একই রকমের বিভিন্ন জিনিস তৈরী করতে হয়। মনে করেন, আমাদের একটা ওয়েবসাইট আছে যেখানে মানুষ অনেক কুকুর নিয়ে জানতে বা দেখতে পারবে!

প্রতিটা কুকুরের জন্য আমাদের একটা অবজেক্ট দরকার যেটা সেই কুকুরটাকে রিপ্রেজেন্ট করতে পারবে! 🐕 প্রতিবার একটা নতুন অবজেক্ট লিখার বদলে আমি একটা কনস্ট্রাক্টর ফাংশন ব্যাবহার করব(আমি জানি আপনি কি ভাবছেন, ES6 ক্লাস নিয়ে আমি পরে আলাপ করব!) যেটা দিয়ে আমরা Dog *instance* তৈরী করতে পারি `new` keyword দিয়ে(এই পোস্ট আসলে কনস্ট্রাকটর ফাংশন নিয়ে না, তাই এটা নিয়ে তেমন কথা বলব না)। 

প্রত্যেক কুকুরেরই একটা নাম, একটা জাত, একটা রঙ এবং একটা ফাংশন আছে যে ফাংশন দিয়ে সে ঘেউ ঘেউ(bark) করবে! 

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--pDfw39RK--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/caurw7uuk62htpldgtln.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--pDfw39RK--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/caurw7uuk62htpldgtln.png)
যখন আমরা `Dog` কনস্ট্রাক্টর ফাংশনটা ক্রিয়েট করেছি, এটাই কিন্তু আমাদের তৈরি একমাত্র অবজেক্ট ছিল না! অটোমেটিকভাবে আমরা আরেকটা অবজেক্ট তৈরি করেছি যেটাকে বলে *prototype* ! বাই ডিফল্ট, এই অবজেক্টটার একটা *constructor* প্রোপার্টি আছে, যেটা অরিজিনাল কনস্ট্রাক্টর ফাংশনটার একটা রেফারেন্স মাত্র, এই ক্ষেত্রে `Dog` এর রেফারেন্স। 

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--dWGIZ_zz--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/9howj4i3zvlgun3svppp.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--dWGIZ_zz--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/9howj4i3zvlgun3svppp.gif)
`Dog` কনস্ট্রাক্টর ফাংশনের  `prototype` প্রোপার্টিটা non-enumerable, মানে আমরা যখন অবজেক্ট প্রোপার্টি access করতে চাই, তখন এটা দেখা যায় না। কিন্তু এটা ঠিকই আছে!

আচ্ছা, তাহলে.. এই _property_ অবজেক্টটা আমাদের কেন আছে? প্রথমে, চলেন কিছু কুকুর বানাই যেগুলো আমরা দেখাতে চাচ্ছি। বোঝানোর সুবিধার্থে, আমি ওদের নাম দিলাম  `dog1` এবং `dog2`. `dog1` হচ্ছে Daisy, একটা কিউট কালো Labrador! `dog2` হচ্ছে Jack, নির্ভিক সাদা Jack Russell 😎 
[![](https://res.cloudinary.com/practicaldev/image/fetch/s--O_jSVpBB--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/lyajz4lade30ci2koirq.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--O_jSVpBB--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/lyajz4lade30ci2koirq.png)

চলেন `dog1` কে কনসোলে লগ করি, এবং এর প্রোপার্টিগুলো দেখি।

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--cA-2FOVV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/tt4yfoz8ckmxfofv3f9v.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--cA-2FOVV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/tt4yfoz8ckmxfofv3f9v.gif)
আমরা আমাদের অ্যাড করা প্রোপার্টিগুলো দেখতে পাচ্ছি, যেমন  `name`, `breed`, `color`, এবং `bark`..কিন্তু, আরেহ! একি! এই `__proto__` প্রোপার্টিটা কি? এটা non-enumerable, মানে এটা সাধারণত দেখা যায় না যখন আমরা অবজেক্টের প্রোপার্টি দেখতে যাই। চলেন এটা এক্সপ্যান্ড করি! 😃

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--zxO-eMV0--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/dye57pcku5cfaz0er60c.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--zxO-eMV0--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/dye57pcku5cfaz0er60c.gif)
আরেহ! এটা দেখতে একদম `Dog.prototype` অবজেক্টের মতো! কি মনে হচ্ছে? আসলে, `__proto__` হচ্ছে `Dog.prototype` এরই একটা রেফারেন্স। এটাই আসলে  **prototypal inheritance** : কোন কনস্ট্রাক্টর ফাংশনের প্রতিটা ইন্সট্যান্সেরই কনস্ট্রাক্টরের প্রোটোটাইপের access আছে।  🤯

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--FBGV--dx--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/t6kiav029gl2e0hv1xct.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--FBGV--dx--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/t6kiav029gl2e0hv1xct.gif)
তাহলে প্রশ্ন হচ্ছে, এই জিনিসটা এত পিনিকের কেন? মাঝে মাঝে আমাদের কিছু প্রোপার্টি দরকার হয়, যেটা সব ইন্সট্যান্সই শেয়ার করে। উদাহরণ হিসেবে বলা যায়, `bark` ফাংশনের কথা। প্রতিটা ইন্সট্যান্সের জন্যই এই ফাংশনটা একই। তাহলে যতবার আমরা নতুন কুকুর তৈরি করছি ততবারই নতুন ফাংশন তৈরি করার দরকার কি? শুধু শুধু মেমোরি খাচ্ছে! বরং, এটা আমরা এটাকে `Dog.prototype` অবজেক্টের ভেতরে অ্যাড/ঢুকিয়ে দিতে পারি! 🥳

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--2026kdwz--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/59nlnyqioosaowj09xn8.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--2026kdwz--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/59nlnyqioosaowj09xn8.gif)

Whenever we try to access a property on the instance, the engine first searches locally to see if the property is defined on the object itself. However, if it can't find the property we're trying to access, the engine **walks down the prototype chain** through the `__proto__` property!
যখনই আমরা কোন ইন্সট্যান্সের কোন প্রোপার্টি access করতে চাই, জাভাস্ক্রিপ্ট ইঞ্জিন প্রথমে locally সার্চ করে দেখে যে সেই অবজেক্টের *নিজের* ভেতর সেটা ডিফাইন করা আছে কিনা। তারপর সে যদি সেই প্রোপার্টি না পায়, তখন ইঞ্জিন **প্রোটোটাইপ চেইনের নিচে যেতে থাকে(walks down the prototype chain)** `__proto__` প্রোপার্টির সাহায্যে! 

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--gg5KU5nB--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/fabyyjot1s78mttyzzk8.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--gg5KU5nB--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/fabyyjot1s78mttyzzk8.gif)
এক্ষেত্রে এটা শুধু একটা স্টেপে দেখানো হয়েছে, কিন্তু এটার অনেক স্টেপ থাকতে পারে! মনযোগ দিয়ে খেয়াল করলে দেখবেন, আমি যখন `__proto__` অবজেক্টটা এক্সপ্যান্ড করে `Dog.prototype` অবজেক্ট দেখলাম, তখন আসলে একটা প্রোপার্টি ইনক্লুড করিনি। `Dog.prototype` নিজেই একটা অবজেক্ট, অর্থাৎ এটা আসলে `Object` কনস্ট্রাক্টরের একটা ইন্সট্যান্স! এর মানে `Dog.prototype` নিজেও আরেকটা `__proto__` প্রোপার্টি ধারণ করে! এবং সেটা আসলে `Object.prototype` এর একটা রেফারেন্স!

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--vJ7k8Gb3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/8vk5w6loliot818f2lcd.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--vJ7k8Gb3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/8vk5w6loliot818f2lcd.gif)
শেষমেষ, আমরা আমাদের উত্তর পেয়ে গেছি! প্রশ্ন কি ছিল সেটাই কি ভুলে গেলেন নাকি? প্রশ্ন ছিল এতো এতো বিল্ট-ইন মেথড কোথা থেকে আসে? তারা সকলেই আসলে আছে প্রোটোটাইপ চেইনের ভেতরে! 😃


উদাহরণ হিসেবে `.toString()` মেথডটা নেয়া যাক। এই মেথড কি locally `dog1` অবজেক্টের মধ্যে ডিফাইন করা আছে? হুমম নাহ!... এটা কি তাহলে `dog1.__proto__` যেই অবজেক্টকে রেফারেন্স করে(`Dog.prototype`) সেই অবজেক্টের মধ্যে আছে? এবারও উত্তর হচ্ছে, নাহ! এটা কি তাহলে `Dog.prototype.__proto__` এর রেফারেন্স অবজেক্টের(`Object.prototype`) মধ্যে আছে? হ্যাঁ! 🙌🏼

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--16IwaVkk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/fpt5nndkbq5kau0nqeqj.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--16IwaVkk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/fpt5nndkbq5kau0nqeqj.gif)
Now, we've just been using constructor functions (`function Dog() { ... }`), which is still valid JavaScript. However, ES6 actually introduced an easier syntax for constructor functions and working with prototypes: classes!


> Classes are only **syntactical sugar** for constructor functions. Everything still works the same way!

We write classes with the `class` keyword. A class has a `constructor` function, which is basically the constructor function we wrote in the ES5 syntax! The properties that we want to add to the prototype, are defined on the classes body itself.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--3PePIjz5--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/qnbqubcipqjl5pb3i8ds.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--3PePIjz5--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/qnbqubcipqjl5pb3i8ds.gif)

Another great thing about classes, is that we can easily **extend** other classes.

Say that we want to show several dogs of the same breed, namely Chihuahuas! A chihuahua is (somehow... 😐) still a dog. To keep this example simple, I'll only pass the `name` property to the Dog class for now instead of `name`, `breed` and `color`. But these chihuahuas can also do something special, they have a small bark. Instead of saying `Woof!`, a chihuahua can also say `Small woof!` 🐕

In an extended class, we can access the parent class' constructor using the `super` keyword. The arguments the parent class' constructor expects, we have to pass to `super`: `name` in this case.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--Fitn1c9K--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/tx25dar3duqo0z2bpfam.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Fitn1c9K--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/tx25dar3duqo0z2bpfam.png)

`myPet` has access to both the `Chihuahua.prototype` and `Dog.prototype` (and automatically `Object.prototype`, since `Dog.prototype` is an object).

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--WOeqUeM3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/qija16dju8t5j1ksy0ps.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--WOeqUeM3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/qija16dju8t5j1ksy0ps.gif)

Since `Chihuahua.prototype` has the `smallBark` function, and `Dog.prototype` has the `bark` function, we can access both `smallBark` and `bark` on `myPet`!

Now as you can imagine, the prototype chain doesn't go on forever. Eventually there's an object which prototype is equal to `null`: the `Object.prototype` object in this case! If we try to access a property that's nowhere to be found locally or on the prototype chain, `undefined` gets returned.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s---EseK2fk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/1905zxijp45soy0jzle2.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s---EseK2fk--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/1905zxijp45soy0jzle2.gif)

---

Although I explained everything with constructor functions and classes here, another way to add prototypes to objects is with the `Object.create` method. With this method, we create a new object, and can specify exactly what the prototype of that object should be! 💪🏼

We do this, by passing an _existing object_ as argument to the `Object.create` method. That object is the prototype of the object we create!

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--uw9DJFU0--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/kbwwsn1fd4gngd05tm9a.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--uw9DJFU0--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/kbwwsn1fd4gngd05tm9a.png)

Let's log the `me` object we just created.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--9sWtvaRG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/6zzt8zpy85gtitxmpwi9.gif)](https://res.cloudinary.com/practicaldev/image/fetch/s--9sWtvaRG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/6zzt8zpy85gtitxmpwi9.gif)

We didn't add any properties to the `me` object, it simply only contains the non-enumerable `__proto__` property! The `__proto__` property holds a reference to the object we defined as the prototype: the `person` object, which has a `name` and an `age` property. Since the `person` object is an object, the value of the `__proto__` property on the `person` object is `Object.prototype` (but to make it a bit easier to read, I didn't expand that property in the gif!)

---

Hopefully, you now understand why prototypal inheritance is such an important feature in the wonderful world of JavaScript! If you have questions, feel free to reach out to me! 😊