## tar-pack

文档地址： https://www.npmjs.com/package/tar-pack



Package and un-oackage modules of some sort(in tar/gz bundles). This is mostly useful for package managers. Note that it doesn't check for or touch `package.json` so it can be used even if that's not the way you store your package info.





## API

### unpack(folder, [options,] cb)

Return a stream that unpacks a tarball into a folder at `folder`, the output folder will be removed first if it already exists.

