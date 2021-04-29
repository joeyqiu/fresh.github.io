## pkg-up

Find the closet package.json file.



## Usage

```
/
└── Users
    └── sindresorhus
        └── foo
            ├── package.json
            └── bar
                ├── baz
                └── example.js
```

```
// example.js
const pkgUp = require('pkg-up');
 
pkgUp().then(filepath => {
    console.log(filepath);
    //=> '/Users/sindresorhus/foo/package.json'
});
```

