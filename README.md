# মঙ্গুসে স্ট্যাটিক মেথড বনাম ইন্সট্যান্স মেথড
মঙ্গুসে, আপনি আপনার স্কিমায় এমন মেথড সংজ্ঞায়িত করতে পারেন যা আপনার মডেলের ইন্সট্যান্সের (ইন্সট্যান্স মেথড) বা মডেলের নিজেই (স্ট্যাটিক মেথড) কল করা যেতে পারে। এখানে উভয় ধরণের মেথডের ব্যাখ্যা এবং তাদের ব্যবহার দেখানোর জন্য কিছু কোড উদাহরণ দেওয়া হলো।
## 1. ইন্সট্যান্স মেথড
ইন্সট্যান্স মেথডগুলি মডেলের পৃথক ডকুমেন্ট (ইন্সট্যান্স) এর উপর কাজ করে। এদের অ্যাক্সেস থাকে সেই ডকুমেন্টের সাথে এবং এদের কাজ হলো সেই ডকুমেন্টের ডেটা পরিবর্তন করা বা সেই ডকুমেন্টের সাথে সংশ্লিষ্ট কাজ করা।
### ইন্সট্যান্স মেথড সংজ্ঞায়িত করা
আপনি স্কিমার methods অবজেক্টে ইন্সট্যান্স মেথড সংজ্ঞায়িত করতে পারেন।
<pre>
  const mongoose = require('mongoose');
const { Schema } = mongoose;

const userSchema = new Schema({
  name: String,
  age: Number,
  email: {
    type: String,
    required: true,
    unique: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});

// ইন্সট্যান্স মেথড সংজ্ঞায়িত করা
userSchema.methods.greet = function() {
  return `হ্যালো, আমার নাম ${this.name}`;
};

const User = mongoose.model('User', userSchema);

// উদাহরণ ব্যবহার
const newUser = new User({
  name: 'জন ডো',
  age: 30,
  email: 'john.doe@example.com'
});

console.log(newUser.greet()); // আউটপুট: হ্যালো, আমার নাম জন ডো

</pre>

## 2. স্ট্যাটিক মেথড
স্ট্যাটিক মেথডগুলি মডেলের উপর কাজ করে, আলাদা ডকুমেন্টের উপর নয়। এদের ব্যবহার মডেলের সাথে সংশ্লিষ্ট কাজ করতে যেমন ডাটাবেস থেকে তথ্য খোঁজা বা বহু ডকুমেন্টের উপর অপারেশন চালানো।

### স্ট্যাটিক মেথড সংজ্ঞায়িত করা
আপনি স্কিমার statics অবজেক্টে স্ট্যাটিক মেথড সংজ্ঞায়িত করতে পারেন।
<pre>
  const mongoose = require('mongoose');
const { Schema } = mongoose;

const userSchema = new Schema({
  name: String,
  age: Number,
  email: {
    type: String,
    required: true,
    unique: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});

// স্ট্যাটিক মেথড সংজ্ঞায়িত করা
userSchema.statics.findByEmail = function(email) {
  return this.findOne({ email });
};

const User = mongoose.model('User', userSchema);

// উদাহরণ ব্যবহার
User.findByEmail('john.doe@example.com')
  .then(user => console.log(user))
  .catch(err => console.error('ব্যবহারকারী খুঁজতে ত্রুটি:', err));

</pre>
