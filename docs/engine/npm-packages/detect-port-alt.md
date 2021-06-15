## detect-port-alt

Javascript Implementation of Port Detector



### Usage

```
const detect = require('detect-port');
 
/**
 * callback usage
 */
 
detect(port, (err, _port) => {
  if (err) {
    console.log(err);
  }
 
  if (port == _port) {
    console.log(`port: ${port} was not occupied`);
  } else {
    console.log(`port: ${port} was occupied, try port: ${_port}`);
  }
});
```

