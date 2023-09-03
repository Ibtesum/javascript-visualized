
![](https://res.cloudinary.com/practicaldev/image/fetch/s--pHrmQNaA--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://thepracticaldev.s3.amazonaws.com/i/q0vxo5pcm6qjo14k0ami.png)



জাভাস্ক্রিপ্ট দুর্দান্ত, কিন্তু একটা মেশিন আসলে কিভাবে আপনার লেখা কোডটা বুঝতে পারে? জাভাস্ক্রিপ্ট ডেভেলপার হিসেবে আমাদের সাধারণত কম্পাইলার নিয়ে সরাসরি কাজ করতে হয়না। তারপরও জাভাক্রিপ্ট ইঞ্জিনের *বেসিক* জেনে রাখা ভাল। কিভাবে এই ইঞ্জিন হিউম্যান ফ্রেন্ডলি জাভাক্রিপ্ট কোডকে হ্যান্ডেল করে এবং কিভাবে এটাকে মেশিনের বোধগম্য করে তোলে? 🥳


নোটঃ এই পোস্টটা মেইনলি V8 ইঞ্জিনের উপর বেস করে করে লিখা। V8 ইঞ্জিন প্রধানত ক্রোমিয়াম বেসড ব্রাউজার এবং Node.js এ ব্যাবহার হয়। 

---

The HTML parser encounters a `script` tag with a source. Code from this source gets loaded from either the **network**, **cache**, or an installed **service worker**. The response is the requested script as a **stream of bytes**, which the byte stream decoder takes care of! The **byte stream decoder** decodes the stream of bytes as it’s being downloaded.


![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--w8scUWXd--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/pv4y4w0doztvmp8ei0ki.gif)

---

The byte stream decoder creates **tokens** from the decoded stream of bytes. For example, `0066` decodes to `f`, `0075` to `u`, `006e` to `n`, `0063` to `c`, `0074` to `t`, `0069` to `i`, `006f` to `o`, and `006e` to `n` followed by a white space. Seems like you wrote `function`! This is a reserved keyword in JavaScript, a token gets created, and sent to the parser (and _pre-parser_, which I didn't cover in the gifs but will explain later). The same happens for the rest of the byte stream.


![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--hve6eM1Y--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/bic727jhzu0i8uep8v0k.gif)

---

The engine uses two parsers: the **pre-parser**, and the **parser**. In order to reduce the time it takes to load up a website, the engine tries to avoid parsing code that's not necessary right away. The preparser handles code that may be used later on, while the parser handles the code that’s needed immediately! If a certain function will only get invoked after a user clicks a button, it's not necessary that this code is compiled immediately just to load up a website. If the user eventually ends up clicking the button and requiring that piece of code, it gets sent to the parser.

The parser creates nodes based on the tokens it receives from the byte stream decoder. With these nodes, it creates an Abstract Syntax Tree, or AST. 🌳


![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--Hw6ffhuc--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/sgr7ih6t7zm2ek28rtg6.gif)

---

Next, it's time for the **interpreter**! The interpreter which walks through the AST, and generates **byte code** based on the information that the AST contains. Once the byte code has been generated fully, the AST is deleted, clearing up memory space. Finally, we have something that a machine can work with! 🎉

GIF

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--0XVq4QSI--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/i5f0vmcjnkhireehicyn.gif)

---

Although byte code is fast, it can be faster. As this bytecode runs, information is being generated. It can detect whether certain behavior happens often, and the types of the data that’s been used. Maybe you've been invoking a function dozens of times: it's time to optimize this so it'll run even faster! 🏃🏽‍♀️

The byte code, together with the generated type feedback, is sent to an **optimizing compiler**. The optimizing compiler takes the byte code and type feedback, and generates highly optimized machine code from these. 🚀

GIF

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--qXNUvgYZ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/ongt4qftovd82sp2vihk.gif)

---

JavaScript is a dynamically typed language, meaning that the types of data can change constantly. It would be extremely slow if the JavaScript engine had to check each time which data type a certain value has.

In order to reduce the time it takes to interpret the code, optimized machine code only handles the cases the engine has seen before while running the bytecode. If we repeatedly used a certain piece of code that returned the _same_ data type over and over, the optimized machine code can simply be re-used in order to speed things up. However, since JavaScript is dynamically typed, it can happen that the same piece of code suddenly returns a different type of data. If that happens, the machine code gets de-optimized, and the engine falls back to interpreting the generated byte code.

Say a certain function is invoked a 100 times and has always returned the same value so far. It will _assume_ that it will also return this value the 101st time you invoke it.

Let’s say that we have the following function sum, that’s (so far) always been called with numerical values as arguments each time:

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--yh8xcZ0V--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/dhiaau4lo3n457yqud4o.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--yh8xcZ0V--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/dhiaau4lo3n457yqud4o.png)

This returns the number `3`! The next time we invoke it, it will assume that we’re invoking it again with two numerical values.

If that’s true, no dynamic lookup is required, and it can just re-use the optimized machine code. Else, if the assumption was incorrect, it will revert back to the original byte code instead of the optimized machine code.

For example, the next time we invoke it, we pass a string instead of a number. Since JavaScript is dynamically typed, we can do this without any errors!

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--g38IU7mW--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/zugnjsg813urbj6vr4iy.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--g38IU7mW--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/zugnjsg813urbj6vr4iy.png)

This means that the number `2` will get coerced into a string, and the function will return the string `"12"` instead. It goes back to executing the interpreted bytecode and updates the type feedback.

---

I hope this post was useful to you! 😊 Of course, there are many parts to the engine that I haven't covered in this post (JS heap, call stack, etc.) which I might cover later! I definitely encourage you to start to doing some research yourself if you're interested in the internals of JavaScript, V8 is open source and has some great documentation on how it works under the hood! 🤖

[V8 Docs](https://v8.dev/) || [V8 Github](https://github.com/v8/v8) || [Chrome University 2018: Life Of A Script](https://www.youtube.com/watch?v=voDhHPNMEzg&t=729s%3Cbr%3E%0A)

---

Feel free to reach out to me! **[Twitter](https://www.twitter.com/lydiahallie) || [Instagram](https://www.instagram.com/theavocoder) || [GitHub](https://www.github.com/lydiahallie) || [LinkedIn](https://www.linkedin.com/in/lydia-hallie)**

FAQ: I use Keynote to make the animations and screen record it lol. Feel free to translate this blog to your language, and thanks so much for doing so! Just keep a reference to the original article and let me know if you've translated it please! 😊
