# Advanced TypeScript Types

Session hosted by [Marco](https://www.linkedin.com/in/stnimmerlein/).

We looked at the following problem:
- A translation library uses a json file with a nested object for the translated values.
- Translation keys are strings that define the path inside this json object (like `'TITLE_PAGE.HEADER.GREETING'`).
- Translations can have parameters that need to be provided when using the keys (e. g. `Welcome {{name}}! Nice to have you here!` needs an object `{name: string}` as second parameter).

Problem: In the translation functions, the translation key is just a `string` => No validation whether this key exists, not resilient against typos. Also parameters are not enforced if needed and are also fragile to typos/refactorings.

So in this session we step by step crafted TypeScript types that, in the end, provided the following:
- Only allow translation keys that actually exist in the json file
- If a translation needs parameters, enforce exactly these parameters to be provided
- If a translation does not need parameters, do not allow a second argument (where you would provide the params).

The repository with the examples can be found [here](https://gitlab.com/StNimmerlein/safe-translate-demo).
- `main` branch contains the initial problem
- `01-first-level` handles non-nested translations well.
- `02-deep-levels` adds support for nested translations. To this level, it totally is worth it to think about using something like this in you application.
- `03-params` adds support for translation parameters as well. But here the original translation json file needed to move to a TypeScript file, so some preparation struff needed to be done first (`02.5-param-preparation`). Now we are at the level "quite cool, but maybe not practical for productive stuff".
