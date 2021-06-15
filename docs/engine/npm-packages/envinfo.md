## envinfo

envinfo generates a report of the common detail needed when troubleshooting software issues, such as your operation system, binary versions, browsers, installed languages, and more.



文档地址：https://www.npmjs.com/package/envinfo



## Programmatic Usage

Envinfo takes a configuration object and returns a string(optionally yaml, json or markdown)

```
import envinfo from 'envinfo';

envinfo.run({
    System: ['OS', 'CPU'],
    Browsers: ['Chrome', 'Firefox', 'Safari']
}, {
    json: true, console: true, showNotFound: true
}).then(console.log)
// 通过.then(console.log)来进行打印
```

return value:

```
{
    "System": {
        "OS": "macOS ...",
        "CPU": "x64 ..."
    },
    "Browsers": {
        "Chrome": {"version": "xxx"},
        "Firefox": {"version": "xxx"},
        "Safari": {"version": "xxx"}
    }
}
```