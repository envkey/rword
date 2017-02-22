A random generator for real English words. Contains almost 130,000 English words between 3 and 10 characters long.

Used by [Ptorx](https://ptorx.com/) and other projects in the [Xyfir Network](https://xyfir.com/#/network).

# Usage

```js
const rword = require('rword');

rword.generate();
// 'bioplasm'

rword.generate(5);
// ['verniers', 'recognizes', 'shockstall', 'aerofoils', 'sooling']

rword.generate(5, { length: 5 });
// ['crags', 'allyl', 'vower', 'berob', 'divan']

rword.generate(3, { length: '3-4' });
// ['limo', 'fado', 'calf']

rword.generate(5, { contains: '.*ed' });
// ['outvied', 'lappeted', 'roared', 'needfully', 'catechized']

rword.generate(5, { contains: 'af' })
// ['falafels', 'handstaff', 'bushcrafts', 'cafila', 'cybercafe']

rword.generate(5, { contains: /.*ing$/ })
// ['depicting', 'upstanding', 'scending', 'forking', 'numbing']

rword.generate(3, { capitalize: 'all' });
// ['THEORIZED', 'RETIRE', 'HOLDBACK']

rword.generate(3, { capitalize: 'first' });
// ['Grew', 'Waxberries', 'Neesing']

rword.generateFromPool(5);
// [ 'intimae', 'shandygaff', 'likable', 'unrebated', 'broken' ]

rword.shuffle();
```

# API

## rword.generate(count, options)

Generates words from the global words array.

- `count: number` - Optional (default 1) - How many words to return. If 1 or not present, a string is returned. If greater than 1 an array of strings is returned.
- `options: object` - Optional - Allows you to modify the output and words that could be randomly chosen.
- `options.length: number|string` - Optional (default '3-10') - Allows you to set an exact length or range of lengths for words to return.
- `options.contains: string|RegExp` - Optional (default /.*/) - Words that don't match the regexp will not be returned.
- `options.capitalize: string` - Optional (default 'none') - Changes the capitalization of the words returned. Possible values: 'none', 'all', 'first'.

## rword.generateFromPool(count)

Generates words from the global **pool** array. The pool is automatically filled using `rword.generate(500)` and then words are taken out of that array as needed. Much faster than rword.generate().

- `count: number` - Optional (default 1) - How many words to return. If 1 or not present, a string is returned. If greater than 1 an array of strings is returned.

**Limitations:**

- No options object is accepted.
- You cannot request more than 500 words through this method.

## rword.shuffle()

Shuffles both the global words and pool arrays. This method can most likely be ignored as it is automatically called on first run and then every 10 to 30 minutes after.

# How are the words generated?

The words array is shuffled on first load with `Math.random()` and every 10 to 30 minutes after, or whenever `rword.shuffle()` is called. Words are then randomly selected from this array using `Math.random()` again. The shuffled words arrays are shared across all rword instances.

# Notes

- Requesting a single word will always return a string. Requesting multiple words will always return an array.
- When using any of the filter properties for the options object in rword.generate(), there's no guarantee you'll receive a word or as many words as you requested. If you requested one word and none where found, an empty string will be returned. If you requested multiple words and none were found an empty array will be returned.
- It is *much* faster to generate multiple words in one call than it is to make multiple calls. Don't be afraid to generate large arrays of random words. If you know you'll need multiple words, try to keep it all in one call. Don't call rword from within a loop if you can help it. **It's faster to generate 10,000 words in one call than it is one word in 10 calls.**
- Use rword.generateFromPool() over rword.generate() if possible as it is many times faster.